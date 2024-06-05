## **Batch Normalization (BN)**:

Batch Normalization is a technique used to normalize the activations of a neural network layer by computing the mean and standard deviation of the activations within a mini-batch during training. This helps to stabilize and speed up the training process by reducing the internal covariate shift, which can lead to faster convergence and better generalization. Batch Normalization is usually applied to the output of a fully connected or convolutional layer before the activation function.

**Simply speaking, BN works in the dimension of Batch. However, in NLP area, this is not often used as different sentence has different length, that's why we need LN.**

```python
import numpy as np

def batch_normalize(x, gamma, beta, epsilon=1e-5):
    mean = np.mean(x, axis=0, keepdims=True)
    variance = np.var(x, axis=0, keepdims=True)
    x_normalized = (x - mean) / np.sqrt(variance + epsilon)
    normalized_output = gamma * x_normalized + beta
    return normalized_output

# Example input data
batch_size = 4
hidden_size = 3

input_data = np.random.randn(batch_size, hidden_size)
gamma = np.random.randn(hidden_size)
beta = np.random.randn(hidden_size)

# Apply Batch Normalization
normalized_output = batch_normalize(input_data, gamma, beta)

print("Input shape:", input_data.shape)
print("Normalized output shape:", normalized_output.shape)
```



## **Layer Normalization (LN)**: 

Layer Normalization is a similar technique to Batch Normalization, but instead of normalizing over a mini-batch, it normalizes over the features within a single training example. This makes it well-suited for recurrent neural networks (RNNs) and other models where the concept of a mini-batch might not be as clear. Layer Normalization is applied along the feature dimension, helping to stabilize training and improve convergence.

**Simply speaking, LN works in the dimension of features. In NLP it's hidden_size direction of the word vector.**

```python
import numpy as np

def layer_normalize(x, gamma, beta, epsilon=1e-5):
    mean = np.mean(x, axis=-1, keepdims=True)
    variance = np.var(x, axis=-1, keepdims=True)
    x_normalized = (x - mean) / np.sqrt(variance + epsilon)
    normalized_output = gamma * x_normalized + beta
    return normalized_output

# Example input data
batch_size = 4
hidden_size = 3
sequence_length = 5

input_data = np.random.randn(batch_size, sequence_length, hidden_size)
gamma = np.random.randn(hidden_size)
beta = np.random.randn(hidden_size)

# Apply Layer Normalization
normalized_output = layer_normalize(input_data, gamma, beta)

print("Input shape:", input_data.shape)
print("Normalized output shape:", normalized_output.shape)
```

