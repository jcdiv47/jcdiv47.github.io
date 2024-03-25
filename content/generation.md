---
date created: Monday, January 8th 2024, 21:16:29
date modified: Monday, March 25th 2024, 22:18:30
tags:
  - llm/generation
aliases:
  - inference
---

> [!summary] Start from an [overview](https://huggingface.co/blog/how-to-generate) on huggingface about how to generate tokens with llm.


# Different decoding strategies

## greedy search

The model selects the token with the highest conditional probability in each step during generation.

![huggingface blog](https://huggingface.co/blog/assets/02_how-to-generate/greedy_search.png)

## beam search

In each time step, $k$ most probable candidates are kept to the next time step where the model generates the next token for $k$ different sequences, after which the most probable sequence overall is chosen.

![huggingface blog](https://huggingface.co/blog/assets/02_how-to-generate/beam_search.png)

As illustrated from the graph above, in time step 1, the sequence ("The", "dog") and ("The", "nice") are reserved. In time step 2, the model generates next token for both sequences and the new sequence ("The", "dog", "has") is selected because its overall probability is higher: $0.4*0.9 > 0.5 * 0.4$.

## constrained beam search

[read more](https://huggingface.co/blog/constrained-beam-search)

> [!tip] 
> In open-ended generation, a couple of reasons have been brought forward why beam search might not be the best possible option:
> >- Beam search can work very well in tasks where the length of the desired generation is more or less predictable as in machine translation or summarization - see [Murray et al. (2018)](https://arxiv.org/abs/1808.10006) and [Yang et al. (2018)](https://arxiv.org/abs/1808.09582). But this is not the case for open-ended generation where the desired output length can vary greatly, e.g. dialog and story generation.
>> - We have seen that beam search heavily suffers from repetitive generation. This is especially hard to control with _n-gram_- or other penalties in story generation since finding a good trade-off between inhibiting repetition and repeating cycles of identical _n-grams_ requires a lot of finetuning. 
>> - As argued in [Ari Holtzman et al. (2019)](https://arxiv.org/abs/1904.09751), high quality human language does not follow a distribution of high probability next words. In other words, as humans, we want generated text to surprise us and not to be boring/predictable. The authors show this nicely by plotting the probability, a model would give to human text vs. what beam search does.

## sampling


## stopping criteria

```python
from transformers import AutoTokenizer, AutoModelForCausalLM, \
    GenerationConfig, BitsAndBytesConfig, StoppingCriteria, \
    TextStreamer, pipeline
import torch


class GenerateSqlStoppingCriteria(StoppingCriteria):

    def __call__(self, input_ids, scores, **kwargs):
        # stops when sequence "```\n" is generated
        # Baichuan2 tokenizer
        # ``` -> 84
        # \n -> 5
        return (
            len(input_ids[0]) > 1
            and input_ids[0][-1] == 5
            and input_ids[0][-2] == 84
        )
    def __len__(self):
        return 1
    
    def __iter__(self):
        yield self

        
model_id = "baichuan-inc/Baichuan2-13B-chat"
tokenizer = AutoTokenizer.from_pretrained(
    model_id,
    use_fast=False,
    trust_remote_code=True,
    revision="v2.0"
)
quantization_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.bfloat16,
)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    device_map="auto",
    quantization_config=quantization_config,
    trust_remote_code=True,
)
model.generation_config = GenerationConfig.from_pretrained(model_id, revision="v2.0")
streamer = TextStreamer(tokenizer, skip_prompt=True,)
pipeline = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    revision="v2.0",
    do_sample=False,
    num_return_sequences=1,
    eos_token_id=tokenizer.eos_token_id,
    stopping_criteria=GenerateSqlStoppingCriteria(),
    streamer=streamer,
)
```

# Speed up generation

## speculative decoding

In [speculative decoding](https://huggingface.co/blog/whisper-speculative-decoding), a small but competent draft model is used to start generating tokens. The base model(big one) is used to examine the output tokens. Tokens are accepted according to the log probabilities. The base model is then used again to actually generate tokens starting from the rejected token. It can be mathematically proved that the probability distribution is the same as if it was just the base model the whole time. Hence no performance loss while a significant speed up in generation is obtained.

Recent approaches include [LLMLingua](https://github.com/microsoft/LLMLingua) which speeds up inference by compressing prompt and KV cache with minimal performance loss, and [Medusa](https://sites.google.com/view/medusa-llm) which achieves the same performance as speculative sample by attaching and training multiple medusa heads, hence eliminating the need for small yet competent draft models. Note that Medusa indeed requires extra training.
