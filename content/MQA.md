---
date created: Sunday, March 3rd 2024, 16:17:16
date modified: Monday, March 25th 2024, 22:18:33
---

In vanilla multi-head attention setting, there is a separate set of query, key and value vectors for each token. In this case, the memory usage goes up quickly for long sequence and incremental inference is often slow. Multi-query attention(MQA) instead shares keys and values across all different attention heads.

```python
import torch

B = 4  # batch
T = 128  # sequence length
D = 512  # embeddings dimension
H = 8  # number of heads

D_single = D // H  # single head dimension

torch.manual_seed(47)
X = torch.randn(B, T, D)

Wq = torch.nn.Linear(D, D)
Wk = torch.nn.Linear(D, D_single)
Wv = torch.nn.Linear(D, D_single)

Q, K, V = Wq(X), Wk(X), Wv(X)
print("Q: ", Q.shape)
print("K: ", K.shape)
print("V: ", V.shape)
```

```bash
>>> torch.Size([4, 128, 512])
>>> torch.Size([4, 128, 64])
>>> torch.Size([4, 128, 64])
```

During the calculation for block $i$, the $K$ and $V$ vectors are broadcast to multiple heads by [`Tensor.expand`](https://pytorch.org/docs/stable/generated/torch.Tensor.expand.html) before doing multi-head attention. Once the output is computed, memory for the KV-cache is free before proceeding to the next block.

```python
Q_ = Q.view(B, T, H, D_single).transpose(1, 2)
K_ = K.unsqueeze(1).expand(B, H, T, D_single).transpose(2, 3)
V_ = V.unsqueeze(1).expand(B, H, T, D_single)

print("Q: ", Q_.shape)
print("K: ", K_.shape)
print("V: ", V_.shape)
```

The output is

```bash
>>> Q: torch.Size([4, 8, 128, 64])
>>> K: torch.Size([4, 8, 64, 128])
>>> V: torch.Size([4, 8, 128, 64])
```

> [!note] In terms of memory usage comparison between MQA and MHA, there may not be a noticeable decrease in memory usage for generating relatively short sequences, one can refer to [this discussion](https://discuss.huggingface.co/t/generate-using-k-v-cache-is-faster-but-no-difference-to-memory-usage/31272) or conduct more experiments themselves.
