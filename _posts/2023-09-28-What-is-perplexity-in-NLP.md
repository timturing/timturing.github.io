Oirginal link: [here](https://lukesalamone.github.io/posts/perplexity/)

Perplexity is a NLP metric ranging from 1 to infinity. Low perplexity means better results.

In natural language processing, perplexity is the most common metric used to measure the performance of a language model. To calculate perplexity, we use the following formula:

$$ perplexity = e^z$$

where

$$ z = -\frac{1}{N}\Sigma_{i=0}^{N}ln(P_n)$$

## Example
Imagine that we have a language model which generates the following sequence of tokens:

`<start> jack and jill went up the hill`

And suppose that the conditional probabilities for each of the tokens are as follows:

| token | probability |
|-------|-------------|
| <start> | 15% |
| jack | 5% |
| and | 12% |
| jill | 18% |
| went | 25% |
| up | 40% |
| the | 33% |
| hill | 50% |

For the purposes of calculating perplexity it doesn’t matter how the sequence was generated. It may be using an n-gram model or an LSTM or a transformer. All that matters is the probabilities the model assigns to each of the tokens. To calculate perplexity, we calculate the logarithm of each of the values above:

| token | P | ln(P) |
|-------|---|-------|
| <start> | 15% | -1.897 |
| jack | 5% | -2.996 |
| and | 12% | -2.120 |
| jill | 18% | -1.715 |
| went | 25% | -1.386 |
| up | 40% | -0.916 |
| the | 33% | -1.109 |
| hill | 50% | -0.693 |

Summing the logs, we get -12.832. Since there are 8 tokens, we divide -12.832 by 8 to get -1.604. Negating that allows us to calculate the final perplexity:

$$ perplexity = e^{1.604} = 4.973 $$

Therefore the perplexity of this sequence is about 4.973.
