---
date created: Monday, January 8th 2024, 21:56:18
date modified: Monday, March 4th 2024, 23:46:41
tags:
  - deep-learning
  - pytorch
---
# functions


## einsum

```python
"""
https://pytorch.org/docs/stable/generated/torch.einsum.html
"""
>>> # trace
>>> torch.einsum('ii', torch.randn(4, 4))
tensor(-1.2104)

>>> # diagonal
>>> torch.einsum('ii->i', torch.randn(4, 4))
tensor([-0.1034,  0.7952, -0.2433,  0.4545])

>>> # outer product
>>> x = torch.randn(5)
>>> y = torch.randn(4)
>>> torch.einsum('i,j->ij', x, y)
tensor([[ 0.1156, -0.2897, -0.3918,  0.4963],
        [-0.3744,  0.9381,  1.2685, -1.6070],
        [ 0.7208, -1.8058, -2.4419,  3.0936],
        [ 0.1713, -0.4291, -0.5802,  0.7350],
        [ 0.5704, -1.4290, -1.9323,  2.4480]])

>>> # batch matrix multiplication
>>> As = torch.randn(3, 2, 5)
>>> Bs = torch.randn(3, 5, 4)
>>> torch.einsum('bij,bjk->bik', As, Bs)
tensor([[[-1.0564, -1.5904,  3.2023,  3.1271],
        [-1.6706, -0.8097, -0.8025, -2.1183]],

        [[ 4.2239,  0.3107, -0.5756, -0.2354],
        [-1.4558, -0.3460,  1.5087, -0.8530]],

        [[ 2.8153,  1.8787, -4.3839, -1.2112],
        [ 0.3728, -2.1131,  0.0921,  0.8305]]])

>>> # with sublist format and ellipsis
>>> torch.einsum(As, [..., 0, 1], Bs, [..., 1, 2], [..., 0, 2])
tensor([[[-1.0564, -1.5904,  3.2023,  3.1271],
        [-1.6706, -0.8097, -0.8025, -2.1183]],

        [[ 4.2239,  0.3107, -0.5756, -0.2354],
        [-1.4558, -0.3460,  1.5087, -0.8530]],

        [[ 2.8153,  1.8787, -4.3839, -1.2112],
        [ 0.3728, -2.1131,  0.0921,  0.8305]]])

>>> # batch permute
>>> A = torch.randn(2, 3, 4, 5)
>>> torch.einsum('...ij->...ji', A).shape
torch.Size([2, 3, 5, 4])

>>> # equivalent to torch.nn.functional.bilinear
>>> A = torch.randn(3, 5, 4)
>>> l = torch.randn(2, 5)
>>> r = torch.randn(2, 4)
>>> torch.einsum('bn,anm,bm->ba', l, A, r)
tensor([[-0.3430, -5.2405,  0.4494],
        [ 0.3311,  5.5201, -3.0356]])
```

## register_buffer

https://discuss.pytorch.org/t/what-is-the-difference-between-register-buffer-and-register-parameter-of-nn-module/32723