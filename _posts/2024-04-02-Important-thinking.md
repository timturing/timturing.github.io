This is some interesting thinking (also important for myself.)

### View the network as a probability function

This helps me to understand the relationship between mathematics and neural network's abstract function.

For example, we can view the classification of 3 labels (annotated by $y$) as $P(y|x)$. Then we can use the logits to calculate this formula. Because originally we believe the model performs just like a function $y = f(x)$. In this thinking way, we can connect calculus, probability theory, and networks. Actually there is an important function in connecting calculus and probability theory from mathematical view: $E_{x\sim p(x)}f(x) = \int f(x)p(x)dx$. So in my view, calculus is more closed to math, while probability is more likely to be an application of calculus, which works as a bridge for math and implementation. 

Besides, the $p(x)$ means the probability that some specific inputs' appearance times, which is always one (there is no repeat input), so it's often a term that we should eliminate or ignore.

