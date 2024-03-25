---
date created: Monday, March 4th 2024, 23:46:44
date modified: Monday, March 4th 2024, 23:54:29
tags:
  - huggingface
  - transformers
---
> [!tldr] This note effectively serves as a cheatsheet for huggingface and/or transformers.

# Generation

## sample

```python
# https://github.com/huggingface/transformers/blob/831bc25d8fdb85768402f772cf65cc3d7872b211/src/transformers/generation/utils.py#L2724-L2725
next_token_scores = logits_processor(input_ids, next_token_logits)
next_token_scores = logits_warper(input_ids, next_token_scores)
```

> [!tip] `logits_processors` are always applied before `logits_warpers`.

