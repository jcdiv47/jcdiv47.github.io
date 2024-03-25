---
date created: Wednesday, January 10th 2024, 20:59:11
date modified: Tuesday, March 5th 2024, 23:31:52
tags:
  - llm/generation
---
# Motivation

> [!abstract] Transformers are slow and memory-hungry on long sequences due to quadratic time and memory complexity of self-attention in sequence length. The [flash attention paper](https://arxiv.org/pdf/2205.14135.pdf) propose an IO-aware and memory efficient algorithm that implements exact attention.

## Hardware prerequisites

- GPU SRAM(Static Random-Access Memory): 19TB/s(20MB)
- GPU HBM(High Bandwidth Memory): 1.5TB/s(40GB)
- CPU DRAM: 12.8GB/s(>1TB)

## Compute/memory bandwidths in attention calculation

> [!todo] finish the note

Consider A100-40GB SXM, which has a compute bandwidth of 312TFLOPS and memory bandwidth of 1555GB/s and mixed precision training, the operational intensity for the particular hardware is

$$
\begin{align*}
\frac{\pi}{\beta}=\frac{312*1e12}{1555*1e9}=201\text{FLOPS/Bytes}
\end{align*}
$$

Suppose we are to calculate attention for $S=QK^{T}$,

$$
\begin{align*}
\frac{\pi_{t}}{\beta_{t}}=\frac{2N^{2}d}{2Nd+2Nd+2N^{2}}=\frac{N^{2}d}{2Nd+N^{2}}
\end{align*}
$$

For more details refer to this [wonderful blog](https://zhuanlan.zhihu.com/p/639228219). The bottleneck may vary for different values of $N$ and $d$.

| $N$  | $d$ | operations/bytes | bottleneck      |
| ---- | --- | ---------------- | --------------- |
| 256  | 128 | 64               | memory-bound    |
| 1024 | 128 | 102              | memory-bound    |
| 4096 | 128 | 120              | memory-bound    |
| 256  | 256 | 85               | memory-bound    |
| 1024 | 256 | 171              | memory-bound    |
| 4096 | 256 | _228_            | _compute-bound_ |
| 256  | 512 | 102              | memory-bound    |
| 1024 | 512 | _256_            | _compute-bound_ |
| 4096 | 512 | _410_            | _compute-bound_ |

# Online softmax

## Safe softmax

Vanilla softmax could encounter numerical instability, hence a safe version of softmax is used in flash attention where the maximum of the vector is subtracted before exponentiation.

$$
\begin{align*}
\text{softmax}(x_{i})=\frac{\exp\{x_{i}-max(x)\}}{\sum_{j=1}^{n} \exp\{x_{j}-max(x)\}}
\end{align*}
$$

## 3-pass softmax

For $i=1, \cdots, N$,

$$
\begin{align*}
m_{i}:=\max(m_{i-1}, x_{i})
\end{align*}
$$

For $i=1, \cdots, N$,

$$
\begin{align*}
d_{i}:=d_{i-1}+e^{x_{i}-m_{N}}
\end{align*}
$$

For $i=1, \cdots, N$,

$$
\begin{align*}
a_{i}:=e^{x_{i}-m_{N}}/d_{N}
\end{align*}
$$

## 2-pass softmax
# Reference

- [图解大模型计算加速系列：Flash Attention V1，从硬件到计算逻辑](http://mp.weixin.qq.com/s?__biz=MzU3Mzg5ODgxMg==&mid=2247486824&idx=1&sn=3016c0506123c23b26c582ccf81d02d2&chksm=fd3be43bca4c6d2de1904c1941da3aba62fe45909146ad51f31f2b0949fabdfd1357fcb8dc90&mpshare=1&scene=1&srcid=01095d0lx3E0YSbxTk2ZCmAY&sharer_shareinfo=6b9003e541fc0688dd66efa27a758ef2&sharer_shareinfo_first=6b9003e541fc0688dd66efa27a758ef2#rd)
