---
date created: 2024-01-13T23:16:30.3030+08:00
date modified: 2024-03-25T22:40:07.077+08:00
tags:
  - deep-learning
  - machine-learning
draft: "true"
---

# Hinge Loss


# Cross-entropy Loss

The cross-entropy between a "true" distribution **p** and an estimated distribution **q** is defined as:

$$H(p, q)=-\sum_{x}p(x)\log q(x)$$

> [!quote] [Possibly confusing naming conventions](https://cs231n.github.io/linear-classify/)
> To be precise, the _SVM classifier_ uses the _hinge loss_, or also sometimes called the _max-margin loss_. The _Softmax classifier_ uses the _cross-entropy loss_. The Softmax classifier gets its name from the _softmax function_, which is used to squash the raw class scores into normalized positive values that sum to one, so that the cross-entropy loss can be applied. In particular, note that technically it doesn’t make sense to talk about the "softmax loss", since softmax is just the squashing function, but it is a relatively commonly used shorthand.


# Focal Loss



# Reference


- [CS231n - Linear Classification](https://cs231n.github.io/linear-classify/)
