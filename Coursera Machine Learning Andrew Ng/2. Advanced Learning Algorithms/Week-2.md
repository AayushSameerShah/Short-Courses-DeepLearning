# ⭕ The loss function

I hope you wouldn't scream... 

> The **loss function used in ANN** is called "<u>Binary Cross Entropy</u>" and ***is just the same*** - exactly the same as the loss function used in **logistic regression;** where it was called "<u>log loss.</u>"

### The "cross" and "entropy"

1. **Cross-Entropy:** Cross-entropy is a concept from information theory that measures the difference between two probability distributions. In the context of machine learning and classification tasks, it quantifies the dissimilarity between the predicted probability distribution (output of the model) and the true probability distribution (actual labels).

2. **Entropy:** Entropy is a measure of uncertainty or disorder. In information theory, it is used to quantify the amount of information contained in a message or probability distribution. High entropy means high uncertainty, and low entropy means low uncertainty.

Now, when you combine these terms in "binary cross entropy," it essentially means a measure of the difference in uncertainty (or information content) between the predicted probability distribution and the true distribution for a binary classification problem. The goal during training is to minimize this difference, which leads to more accurate predictions. In practical terms, this is achieved by adjusting the model's parameters using optimization algorithms during the training process.

### Re-iterating...

### $L(f(\vec x), y) = -y(log(f(\vec x)) -(1 - y)(log(1 - f(\vec x)))$

So, yeah! That's the loss function!

```python
# For classification
model.compile(loss=BinaryCrossEntropy())

# For regression
model.compile(loss=MeanSquaredError())
```

# 👨🏻‍🔬 Other Activation Functions

See, everything which goes into the activation function is just $g(z)  \text{ or } g(\vec w \cdot \vec x + b)$.

**Most commonly used**:

1. Linear: $\text{no activation}$

2. Sigmoid: $sigmoid(z)$

3. ReLU: $max(0, z)$

### 🤔 When to choose which?

#### For output layer:

**If the binary classification**: 
