# Additive Attention

**Additive Attention**, also known as **Bahdanau Attention**, introduced in [Neural Machine Translation by Jointly Learning to Align and Translate](https://paperswithcode.com/paper/neural-machine-translation-by-jointly), uses a one-hidden layer feed-forward network to calculate the attention alignment score: (**A score means a single value**)

$f_{att}(h_i,s_j)=v_a^T\tanh(W_a[h_i;s_j])$

where $v_a^T$ and $W_a$ are learned attention parameters. Here h refers to the hidden states for the encoder, and s is the hidden states for the decoder. The function above is thus a type of alignment score function. We can use a matrix of alignment scores to show the correlation between source and target words, as the Figure below.

![additive-attention](/images/additive-attention.png)

Within a neural network, once we have the alignment scores, we calculate the final scores using a [softmax](https://paperswithcode.com/method/softmax) function of these alignment scores (ensuring it sums to 1).

## Dot-Product Attention

If you have seen the formula in the paper [Attention is all you need](https://arxiv.org/abs/1706.03762) about **Scaled Dot-Product Attention**, the only difference is just about the constant $\frac{1}{\sqrt{d_k}}$.

$Attention(Q,K,V)=softmax(\frac{QK^T}{\sqrt{d_k}})V$

Actually, without parallelization, this will calculate the attention score one by one too, however here it could be written into a matrix form, thus enhancing the ability to parallelization.