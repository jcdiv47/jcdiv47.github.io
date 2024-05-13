---
tags:
  - llm/models
date: 2024-03-24T15:42:15.1515+08:00
last-modified: 2024-05-01T16:10:53.5353+08:00
---
# Mistral

> [!info] Mistral models adopt Llama2 architecture and various settings. Below are some noticeable add-ons.

## sliding window attention

Sliding window attention(SWA) reduces the number of dot-product calculation performed, hence speeds up both training and inference.

On top of causal mask, in which every token only attends to all previous tokens(including itself), now it pulls back its attention even more, i.e., it only attends to $k$ most recent tokens(including itself).

> [!tip] Sliding window attention still allows a token to attend to previous tokens that are outside the current window. For inspiration refer to the concept [receptive field](https://theaisummer.com/receptive-field/) in CNN.

## KV-Cache with rolling buffer cache

The size of KV-cache is fixed, set to be the sliding window width for convenience.

## pre-filling and chunking

Instead of feeding one token at a time, the prompt tokens are pre-filled altogether, then chunked to size of sliding-window width.

> [!todo] maybe add a figure for better and easier understanding

# Mixtral


> [!info] [Mixtral-8x7b](https://arxiv.org/pdf/2401.04088.pdf) is regarded as SOTA [[MoE]] model which outperforms LLaMA-70b in various tasks while being roughly equivalent to a 40b model in terms of parameters.

# Model Structure


> [!warning] [[mistral#sliding window attention|Sliding window attention]] was _not_ supported for Mixtral and its variants. See [official model config](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1/blob/1e637f2d7cb0a9d6fb1922f305cb784995190a83/config.json#L23).


## Sparse MoE


