---
date created: Thursday, January 4th 2024, 23:49:30
date modified: Monday, March 25th 2024, 20:49:55
tags:
  - llm/models
---
# GeLU

For more detail, check [[activation#GeLU]].

# RMSNorm

In recent settings, RMSNorm is typically placed before a layer, a.k.a pre-norm. One possible explanation is the increase of stability for the training process.

In the [original RMSNorm paper](https://dl.acm.org/doi/pdf/10.5555/3454287.3455397#:~:text=The%20main%20difference%20to%20LayerNorm,to%20all%20re%2Dcentering%20operations.&text=where%20v%20is%20short%20for,f(%C2%B7)%20in%20Eq.), the authors claimed that recentering step in layer norm yields insignificant boost performance, while removing it speeds up the computation noticeably.

# GQA

[[GQA]](Grouped Query Attention), is a balance between multi-head attention([[attention#Multi head attention|MHA]]) from vanilla transformers and [[MQA]](Multi Query Attention), leveraging the advantages from both sides.

Grouped query attention is somewhat an interpolation of MHA and MQA with the goal of balancing the pros and cons. Tokens are first grouped together according to positions and share key and value vectors within groups.

The following table is a direct comparison between dimensions of query, key, value and output for MHA and MQA. $B$ is batch size, $T$ is sequence length, $d$ is embedding dimension, $\text{n\_heads}$ is number of heads, $d_{k}=d/\text{n\_heads}$ is query/key/value vector dimension, $\text{n\_kv\_heads}$ is the number of key/value heads and $G=\text{n\_heads/n\_kv\_heads}$ is the number of groups.

|         | MHA                              | MQA                              | GQA                                     |
| ------- | -------------------------------- | -------------------------------- | --------------------------------------- |
| $X$     | $(B, T, d)$                      | $(B, T, d)$                      | $(B, T, d)$                             |
| $Q$     | $(B, \text{n\_heads}, T, d_{k})$ | $(B, \text{n\_heads}, T, d_{k})$ | $(B, G, \text{n\_kv\_heads}, T, d_{k})$ |
| $K$     | $(B, \text{n\_heads}, T, d_{k})$ | $(B, \textbf{1}, T, d_{k})$      | $(B, \text{n\_kv\_heads}, T, d_{k})$    |
| $V$     | $(B, \text{n\_heads}, T, d_{k})$ | $(B, \textbf{1}, T, d_{k})$      | $(B, \text{n\_kv\_heads}, T, d_{k})$    |
| $O$     | $(B, T, d)$                      | $(B, T, d)$                      | $(B, \text{n\_kv\_heads}, T, d_{k})$    |
| $W_{O}$ | $(d, d)$                         | $(d, d)$                         | $\text{n\_kv\_heads}, d$                |
| Y       | $(B, T, d)$                      | $(B, T, d)$                      | $(B, T, d)$                             |
