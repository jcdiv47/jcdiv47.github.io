---
date created: Sunday, March 3rd 2024, 0:22:53
date modified: Monday, March 4th 2024, 23:09:35
tags:
  - llm/generation
date: 2024-03-24T15:42:15.1515+08:00
last-modified: 2024-05-01T16:12:43.4343+08:00
---
> [!tip] Motivation
> One of the major downsides of transformer architecture is that memory footprint and calculation grow quadratically in terms of sequence length.

During inference time, the model is essentially performing next token prediction task, i.e., it is trying to generate a next token conditioned on all previously generated tokens. Take a closer look at [[attention#Self attention|self attention mechanism]], one could notice that it only needs $Q_{T}$, $K$ and $V$ in order to perform necessary calculation for the next token. 

Without KV-cache, each time before the model generates a new token, it goes through calculating $Q, K, V$ matrices for all previous tokens. However, there exist a huge amount of repeated calculations during the process. With KV-cache, we only use query for the last token, and append the corresponding key and value to KV-cache.

> [!info] Memory usage for KV-cache is $2*\text{num\_layers}*\text{sequence\_length}*d_{k}$.

# Reference


- [More detailed calculation analysis for KV-Cache](https://kipp.ly/transformer-inference-arithmetic/)
- https://medium.com/@plienhar/llm-inference-series-3-kv-caching-unveiled-048152e461c8
