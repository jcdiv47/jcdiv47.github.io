---
date created: Saturday, January 6th 2024, 23:45:50
date modified: Monday, March 25th 2024, 20:50:09
---

# MoE in language models


> [!question] Why not one expert but two?
> Experiments indicate that at least two experts are required for the model to adapt to the routing process, i.e., the model may not be able to learn routing experts if trained with only one expert the whole time.

## Inefficiency during training

For example, although large batch sizes are usually better for performance, batch sizes in MOEs are effectively reduced as data flows through the active experts. For example, if our batched input consists of 10 tokens, five tokens might end in one expert, and the other five tokens might end in five different experts, leading to uneven batch sizes and underutilization.[^1]

Fine-tune is more difficult because it is easier to hit overfitting. Active experts are exposed to more training data, hence trained more thoroughly.

## Load balancing tokens for MoEs

As discussed before, if all our tokens are sent to just a few popular experts, that will make training inefficient. In a normal MoE training, the gating network converges to mostly activate the same few experts. This self-reinforces as favored experts are trained quicker and hence selected more.

To mitigate this, an *auxiliary loss* is added to encourage giving all experts equal importance. This loss ensures that all experts receive a roughly equal number of training examples. In `transformers`, the auxiliary loss is exposed via the `aux_loss` parameter.

[^1]: https://huggingface.co/blog/moe?continueFlag=a09556ebd7121bce97f7bbb8eb2598c8


# Reference

- [Mixture of Experts Explained](https://huggingface.co/blog/moe)
-  [全网最细致大模型MoE原理+代码手撕版](https://mp.weixin.qq.com/s?__biz=MzIwNDY1NTU5Mg%3D%3D&mid=2247487837&idx=1&sn=90dc098e8c2606704263de859c48e106&chksm=973d8fdaa04a06cc8dd098ab3310ade818adf93a6a1d632108f7b3e5c5749a809344656ba9e5&mpshare=1&scene=1&sharer_shareinfo=c3cd7128198bb4fda6568a5b25718855&sharer_shareinfo_first=c3cd7128198bb4fda6568a5b25718855#rd)
- [makeMoE: Inplement a Sparse Mixture of Experts Language Model from Scratch](https://huggingface.co/blog/AviSoori1x/makemoe-from-scratch)
