---
date created: Tuesday, March 5th 2024, 20:44:41
date modified: Tuesday, March 5th 2024, 21:03:51
---

> [!abstract] Multi-query attention([[MQA]]), which only uses a single key-value head, drastically speeds up decoder inference. However, MQA can lead to quality degradation, and moreover it may not be desirable to train a separate model just for faster inference. In the [original paper](https://arxiv.org/abs/2305.13245), the authors introduce grouped-query attention(GQA), a generalization of MQA which uses an intermediate (more than one, less than number of query heads) number of key-value heads.

![](https://klu.ai/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fklu-gqa-grouped-query-attention.083adbc7.png&w=1920&q=100)

# Reference

- https://klu.ai/glossary/grouped-query-attention
