---
date created: 2024-01-05T00:14:18.1818+08:00
date modified: 2024-03-25T22:31:00.000+08:00
tags:
  - llm/tokenizer
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
