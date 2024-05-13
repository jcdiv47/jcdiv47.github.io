---
title: backpropagation
date created: Saturday, January 6th 2024, 23:40:15
date modified: Sunday, March 3rd 2024, 12:14:47
tags:
  - deep-learning
date: 2024-03-24T15:42:15.1515+08:00
last-modified: 2024-05-01T16:12:54.5454+08:00
---
> [!tip] Dive deep(pun intended) into neural nets with some exciting learnings! Try to become a back prop ninja with [Karpathy’s courses](https://github.com/karpathy/nn-zero-to-hero/blob/master/README.md).

  
# Derive Backprop Gradients step by step

```python

dprobs = dlogprobs / probs

dcounts_sum_inv = (dprobs * counts).sum(1, keepdim=True)

dcounts = dprobs * counts_sum_inv

dcounts_sum = -dcounts_sum_inv * counts_sum ** -2

dcounts += dcounts_sum.broadcast_to(counts.shape)

dnorm_logits = dcounts * norm_logits.exp() # norm_logits.exp() is actually counts

dlogit_maxes = (-dnorm_logits).sum(1, keepdim=True)

dlogits = dnorm_logits.clone()

tmp = torch.zeros_like(logits)

tmp[range(n), logits.max(1, keepdim=True).indices.view(-1)] = 1 # try F.one_hot

dlogits += dlogit_maxes * tmp

dh = dlogits @ W2.T

dW2 = h.T @ dlogits

db2 = dlogits.sum(0, keepdim=False)

# dhpreact = dh * (1 - torch.tanh(hpreact) ** 2)

# dhpreact = (1.0 - h ** 2) * dh # figure out later

dhpreact = hpreact.grad.clone()

# dbngain = (dhpreact * bnraw).sum(0, keepdim=True)

dbngain = (dhpreact * bnraw).sum(0, keepdim=True)

dbnbias = dhpreact.sum(0, keepdim=True)

dbnraw = dhpreact * bngain

dbnvar_inv = (dbnraw * bndiff).sum(0, keepdim=True)

dbndiff = dbnraw * bnvar_inv

# dbnvar = dbnvar_inv * (-0.5) * bnvar_inv ** 3

dbnvar = dbnvar_inv * (-0.5) * (bnvar + 1e-5) ** -1.5

dbndiff2 = 1.0 / (n-1) * torch.ones_like(bndiff2) * dbnvar

dbndiff += 2 * bndiff * dbndiff2

dbnmeani = -dbndiff.sum(0, keepdim=True)

dhprebn = dbndiff.clone()

dhprebn += dbnmeani * 1.0 / n * torch.ones_like(hprebn)

dembcat = dhprebn @ W1.T

dW1 = embcat.T @ dhprebn

db1 = dhprebn.sum(0, keepdim=False)

demb = dembcat.view(emb.shape)

dC = torch.zeros_like(C)

for k in range(Xb.shape[0]):

for j in range(Xb.shape[1]):

ix = Xb[k,j]

dC[ix] += demb[k,j]

```

## `dlogprobs`

  

Notation-wise, $dlogprobs$ stands for the gradient of loss through $logprobs$. To start we have the following

$$

loss = -\frac{1}{n}\sum_{i=1}\sum_{j\in Y_{b}}logprobs_{i, j}

$$

which easily yields the gradient as follows.

$$

\left(\frac{dloss}{dlogprobs}\right)_{i,j}=\left\{

\begin{aligned}

-\frac{1}{n},\quad&j\in Y_{b} \\

0,\quad&\text{otherwise}

\end{aligned}

\right.

$$

```python

dlogprobs = torch.zeros_like(logprobs)

dlogprobs[range(n), Yb] = -1. / n

```

## `dprobs`

  

Continue the process, we have

$$

logprobs_{i,j}=\log(probs_{i,j})

$$

Hence

$$

\left(\frac{dloss}{dprobs}\right)_{i,j}=\left(\frac{dloss}{dlogprobs}\right)_{i,j}\cdot\left(\frac{dlogprobs}{dprobs}\right)_{i,j}=\left(\frac{dloss}{dlogprobs}\right)_{i,j}\cdot\frac{1}{probs_{i,j}}

$$

```python

dprobs = dlogprobs / probs

```

## `dcounts_sum_inv`

```python

dcounts_sum_inv = (dprobs * counts).sum(1, keepdim=True)

```

Note that `probs = counts * counts_sum_inv` is in fact

$$

probs_{i,j}=counts_{i,j}\cdot csi_{i}

$$

For simplicity of notation, denote `counts_sum_inv` by `csi`.

  
  

> [!info] Beware of the broadcasting by checking the shapes.

```python

>> counts.shape, counts_sum_inv.shape

(torch.Size([32, 27]), torch.Size([32, 1]))

```

Hence

$$

\left(\frac{dprobs}{dcsi}\right)_{i}=\sum_{j}counts_{i,j}

$$

and

$$

\left(\frac{dloss}{dcsi}\right)_{i}=\sum_{j}\left(\frac{dloss}{dprobs}\right)_{i,j}\cdot\left(\frac{dprobs}{dcsi}\right)_{i}

$$

```python

dcounts_sum_inv = (dprobs * counts).sum(1, keepdim=True)

```

## `dcounts`

  

Note that from one also has

```python

probs = counts * counts_sum_inv

```

which leads to

$$

\left(\frac{dprobs}{dcounts}\right)_{i,j}=csi_{i}

$$

However, other than contributing to loss through `probs`, `counts` also does that through `counts_sum` and then `counts_sum_inv`. The complete gradient of `dcounts` remains to be determined.

```python

counts_sum_inv = counts_sum**-1

```

## `dcounts_sum`

  

leads to

$$

\left(\frac{dcsi}{dcs}\right)_{i}=-cs_{i}^{-2}

$$

and then

$$

\left(\frac{dloss}{dcounts\_sum}\right)_{i}=-\left(\frac{dloss}{dcsi}\right)_{i}\cdot counts\_sum_{i}^{-2}

$$

## `dnorm_logits`

```python

dnorm_logits = dcounts * norm_logits.exp() # norm_logits.exp() is actually counts

```

## `dlogit_maxes`

```python

dlogit_maxes = (-dnorm_logits).sum(1, keepdim=True)

```

## `dlogits`

```python

dlogit_maxes = (-dnorm_logits).sum(1, keepdim=True)

```

# Backprop through `cross_entropy` but All in One Go
  

Basically what happens in the forward pass can be described by the following pseudocode

```python

logprobs = log(norm(softmax(logits, 1), 1))

loss = -mean(logprobs[range(n), Yb])

```

To discuss derivative for each single element, for every $i$, denote $(Y_{b})_{i}$ by $y$. For simplicity of notation, denote $logits$ by $lg$.

$$

\begin{aligned}

loss&=-\frac{1}{n}\sum_{i=1}^{n}logprobs_{i,y}\\

&=-\frac{1}{n}\sum_{i=1}^{n}\log(probs_{i,y})\\

&=-\frac{1}{n}\sum_{i=1}^{n}\log\left(\frac{\exp\{lg_{i,y}\}}{\sum_{k}\exp\{lg_{i,k}\}}\right)

\end{aligned}

$$

> [!info] Keep in mind that there is supposed to be a subtracting the maximum of logits (in each row) in the numerator. Here it is omitted because it does not affect the gradient towards loss.


  

Now conduct chain rules to derive the derivatives. If $j\neq y$,

$$

\left(\frac{dloss}{dlg}\right)_{i,j}=-\frac{1}{n}\frac{\sum_{k}\exp\{lg_{i,k}\}}{\exp\{lg_{i,y}\}}\cdot(-1)\cdot\frac{\exp\{lg_{i,y}\}}{(\sum_{k}\exp\{lg_{i,k}\})^{2}}\cdot\exp\{lg_{i,j}\}

$$

which yields

$$

\left(\frac{dloss}{dlg}\right)_{i,j}=\frac{1}{n}\cdot\frac{\exp\{lg_{i,j}\}}{\sum{k}\exp\{lg_{i,k}\}}=\frac{1}{n}\cdot\text{softmax}(lg_{i,\cdot})_{j}

$$

If $j=y$,

$$

\left(\frac{dloss}{dlg}\right)_{i,j}=-\frac{1}{n}\frac{\sum_{k}\exp\{lg_{i,k}\}}{\exp\{lg_{i,y}\}}\cdot\frac{\exp\{lg_{i,y}\}\cdot\sum_{k}\exp\{lg_{i,k}\}-\exp\{lg_{i,y}\}^{2}}{(\sum_{k}\exp\{lg_{i,k}\})^{2}}

$$

which yields

$$

\left(\frac{dloss}{dlg}\right)_{i,j}=-\frac{1}{n}+\frac{1}{n}\cdot\frac{\exp\{lg_{i,y}\}}{\sum_{k}\exp\{lg_{i,k}\}}=\frac{1}{n}\cdot(\text{softmax}(lg_{i,\cdot})_{y})-1

$$

While the two cases are discussed separately, they share a common part in `softmax`.

```python

dlogits = F.softmax(logits, 1)

dlogits[range(n), Yb] -= 1

dlogits /= n

```

> [!todo] WIP
> finish calculating the rest derivatives

# Reference

- [https://youtu.be/q8SA3rM6ckI](https://youtu.be/q8SA3rM6ckI)
- https://github.com/karpathy/nn-zero-to-hero/blob/master/lectures/makemore/makemore_part4_backprop.ipynb