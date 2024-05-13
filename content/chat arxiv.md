---
date created: Wednesday, February 7th 2024, 16:22:40
date modified: Saturday, February 24th 2024, 21:11:02
draft: "true"
date: 2024-03-24T15:42:15.1515+08:00
last-modified: 2024-05-01T16:12:53.5353+08:00
---


- bi-encoders for retrieval
- cross-encoders for rerank


> [!note] notable datasets
>
>  ```python
>  # preprocessed chunks of 400 arxiv papers
>  from datasets import load_dataset
>  dataset = load_dataset("jamescalam/ai-arxiv-chunked")
>  ```

> [!tip] notable reranking models
>
> ```python
> from sentence_transformers import CrossEncoder
> cross_encoder = CrossEncoder("BAAI/bge-reranker-base")

> [!note] 
> take a look at [this model](https://huggingface.co/allenai/specter2) that is trained specifically for arxiv papers
>

f
