---
tags:
  - llm/tokenizer
date: 2024-03-24T15:42:15.1515+08:00
last-modified: 2024-05-01T16:09:28.2828+08:00
---
# Byte Pair Encoding


### GPT Tokenizer

> [!info] Byte Level byte pair encoding(BPE) and `tiktoken` were used for GPT2/3 tokenizers. In comparison, `sentencepiece` was used in Llama and mistral, among many other models.

> [!tip] GPT3 improves its capability in understanding Python code by encoding consecutive blank spaces into single tokens, which compresses the Python code string into shorter sequence fitting into the context length.



# WordPiece tokenizer


> [!info] used in BERT



# Reference

- [Karpathy's YouTube tutorial video](https://www.youtube.com/watch?v=zduSFxRajkE)
- [A website to check out different tokenizers](https://tiktokenizer.vercel.app/)
