---
date created: Sunday, January 14th 2024, 0:10:39
date modified: Monday, March 4th 2024, 23:39:25
tags:
  - llm/models
---

# GPT Tokenizer

## Some issues with tokenizers

The corpus used to train tokenizer and that of pre-training stage may very well be different. Hence it could be the case that some tokens were really present in pre-training data, which means the corresponding embedding vectors of these tokens were seldom/never hit by back propagation so they never got updated or were not trained enough. Then when these tokens appear in the input, transformer essentially gets confused and doesn't know what the most probable next-token is. Hence the all kinds of weird outputs.

# Reproduce GPT

To reproduce GPT, one could start from Karpathy's tutorial and his project [nanoGPT](https://github.com/karpathy/nanoGPT). Besides reasonable training/inference code snippets, a substantial amount of data and corresponding compute units are required.

<iframe width="560" height="315" src="https://www.youtube.com/embed/kCc8FmEb1nY?si=7aOniUH33Dfv9m6A" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
