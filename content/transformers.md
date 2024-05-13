---
tags:
  - transformers
date: 2024-03-24T15:42:15.1515+08:00
last-modified: 2024-05-01T16:09:03.033+08:00
---
# Before transformers


## Problems with RNN

- computation becomes really slow in long sequences
- vanishing/exploding gradients
- difficult to attend to information from further position in sequence


# Attention mechanism


> [!question] can Q, K, V have different shape?
> Q and K should have the same shape in corresponding dimensions, while V can theoretically have different shape. In reality people set all three to have the same vector shape most of the time.

Attention mechanism allows the model to attend every token in the sequence with different amount of focus for each token.

## scaled dot-product attention

Before applying softmax to the dot product attention, it should be scaled by a factor of $\sqrt{d_{k}}$ to avoid gradient vanishing and slow training.

## self-attention

> [!note] self-attention is permutation invariant

mask interactions between two tokens by setting the attention values to $-\infty$ before `softmax` layer.

## cross-attention

In self-attention, we are working with the same input sequence. While in cross-attention, we are mixing or combining two different input sequences. In the case of the vanilla transformer architecture, that's the sequence returned by the last/top encoder layer on the left and the input sequence being processed by the decoder part on the right.

> [!tip] that the queries usually come from the decoder, and the keys and values typically come from the encoder.

## causal/masked attention



# Calculating Transformers Parameters


> [!info] Reference
> - [分析transformer模型的参数量、计算量、中间激活、KV cache](https://zhuanlan.zhihu.com/p/624740065?from=10DC093010&launchid=default&WBAPIAnalysisOriUICodes=10000001_10000002&v_p=90&aid=01A_Yt-2aPt_nM1I_IVnwXcvGeZp7B9yXixy4o_xVXtb8tiQQ.&wm=3333_2001&theme=dark&utm_id=0&s_trans=7877617177_&s_channel=4])
> - [大模型推理需要多大的显存？](https://www.bilibili.com/video/BV1Rc411S7jj/?spm_id_from=333.1007.tianma.1-1-1.click&vd_source=6060bdf75fd1af2ad46dacc1e2e50bf1)




