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

**If the binary classification**: Sigmoid

**If Regression (in which the values can be `-/+`):** Linear

**If Regression (in which the values can be only `+`):** ReLU

#### For hidden layer:

The most common choice is **ReLU**.

> #### Why ReLU instead of Softmax?
> 
> 1. ReLU is ***faster*** and ***easier*** to compute.
> 
> 2. ReLU only has a single side which is **flat** while sigmoid has both sides flat which makes the network slow to learn.
> 
> 3. ...continuing 2nd point; When the values are flat, the change is so small (like 0.98 and 0.97) which can have similar and small slope and changes in each pass. And the same for the other side (0.0001 and 0.0002) so, it makes the learning slow.

## 🤔🤔 Yo! But why to choose the activation functions?

> ### 👇🏻
> 
> Using only linear activation *(or no activation)* will **defeat the purpose** of making the neural network. Then this network **will become the linear regression** model.

Please head over to the title: **🤔 So, why to include activations!?** in this notebook [here](https://github.com/AayushSameerShah/Neural-Net-Zero-to-Hero-with-Andrej/blob/main/04%20-%20Diagnosing%20the%20MLP/2.%20Pytorchifying%20the%20code.ipynb) — for more details.

### Some Things about ReLU...

- The strength of the ReLU function lies not in itself, but in an entire army of ReLUs.

- The ReLU is brother of linear-function... but it is non-linear and thus can be used for any non-linear problems.

- > *When there are enough of them, they can approximate any function just as well as other activation functions like sigmoid or tanh, much like stacking hundreds of Legos, without the downsides.*

- There are **several issues with smooth-curve functions** that do not occur with ReLU — one being that computing the **derivative**, or the rate of change, the driving force behind gradient descent, is much cheaper with ReLU than with any other smooth-curve function.

- Another is that sigmoid and other **curves** have an issue with the **vanishing gradient** problem; because the derivative of the sigmoid function gradually slopes off for larger absolute values of *x*.

- ReLU is designed to **work in abundance** *(a very large quantity of something)*; with heavy volume it approximates well, and with good approximation it performs just as well as any other activation function, without the downsides.

### Prove why ReLU is non-linear (so easy):

- If the function is linear it should satisfy: $f(x)+f(y)=f(x+y)$

- By definition the ReLU is $max(0,x)$. Therefore, if we split the domain from $(−∞,0] or [0,∞)$ then the function is **linear**. 

- However, it's easy to see that $f(−1)+f(1)≠f(0)$. Hence by definition ReLU **is not linear**.

- Thus, it can be said as: "ReLU is comprised on linear functions but it is non-linear function."
  
  Reference: [machine learning - Why is ReLU used as an activation function? - Data Science Stack Exchange](https://datascience.stackexchange.com/questions/26475/why-is-relu-used-as-an-activation-function).

<img src="file:///E:/GitHub/Short-Courses-DeepLearning/Coursera%20Machine%20Learning%20Andrew%20Ng/images/why-ReLU-works.png" title="" alt="" data-align="center">

Yo! So, this is why ReLU works and this is something need to keep in mind: It needs **an army of nodes** to make a form of any non-linear shape.

# 🔢 Multi-class classification

## Softmax.

**Before:**

- We only had two classes, so just: $g(z) = \frac{1}{1 + e^{-z}} = P(y=1|\vec x) = 0.9$ would be enough to know the probability of the other class.

**Now:**

- We have many classes, so the function will be... wait let me create the NN for ya.

<img src="file:///E:/GitHub/Short-Courses-DeepLearning/Coursera%20Machine%20Learning%20Andrew%20Ng/images/softmax-nn.png" title="" alt="" data-align="center">

So, now **for each out class, we will calculate the following:**

- $a_1 = \frac{e^{z_1}}{e^{z_1} + e^{z_2} + e^{z_3}}$

- $a_2 = \frac{e^{z_2}}{e^{z_1} + e^{z_2} + e^{z_3}}$

- $a_3 = \frac{e^{z_3}}{e^{z_1} + e^{z_2} + e^{z_3}}$

Thus, `g(z)` is simply as given above for the final output layer!

**And just to generalize... the formulae will become:**

> ## $a_j = \frac{e^{z_j}}{\sum_{k=1}^N e^{z_k}}$

Just don't worry, it is the generalized formulae. 

# 💵 Cost function for Softmax (multi-label)

This has to be looked into, because in logistic we used to $(1-y)$ which did the magic for us... here...

> It turns out that the loss function of a single example is:
> 
> ## $-log(a_j)$

That's it. Means, if you are looking for `jth` class' probability, then it will be simply the log... of that probability. Here `a` will always be between 0-1 since it will be a value after the softmax is applied.

### Why didn't you take the (1 - y) and so on?

See, before this in logistic regression, we were getting the value either for zero or for one. And if the "true" value were one, we simply took `-log()` but if it were zero, we would convert that into zero's probability and then took `-log()`.

Here, the conversion **is not needed** because each class will  be getting their own probability.

Thus, no need to perform `(1 - y)`.

> **Attention:** We will only calculate the loss for the current ***true y.*** Meaning, out of 5 possible classes, if the current record has the 3rd class, then we would only take `-log(a3)`. ***We won't bother for other!***

## 🤏🏻Softmax is a little unusual

In other activation functions, take sigmoid, ReLU, linear etc, they **were only applied** on the `z1` or `z2` or whichever unit we are talking about.

The softmax is *unusual* in that sense that to compute it, we **need all z** from, for the denominator $\frac{e^{z_1}}{e^{z_1} + ... + e^{z_n}}$

Hope its clear!

## In tensorflow...

It is called `SparseCategoricalCrossEntropy`.

- **Sparse**: Each example will only point to the *single* class. Meaning the example won't have multiple true labels.

- **Categorical**: You know!

- **Crossentropy**: Don't wanna know!

# Just use "linear" as the activation in the final layer!

Holy moly! When you set `finalLayer = dense(units=..., activation="sigmoid / softmax")` things will work!

But **it may introduce the rounding off errors!** So, instead doing that do this:

```python
# ❌ Don't use this!

layer = Dense(units=10, activation="softmax") # last layer
model.compile(loss=SparseCategoricalCrossEntropy())

# ✅ Use this!

layer = Dense(units=10, activation="linear") # last layer
model.compile(loss=SparseCategoricalCrossEntropy(from_logits=True))
```

This will make sure that the things are not getting lost in the rounding off error.

**Small note from the lecture:**

> 📝
> 
> In the preferred organization the final layer has a linear activation. For historical reasons, the outputs in this form are referred to as **logits**. The loss function has an additional argument: `from_logits = True`. This informs the loss function that the softmax operation should be included in the loss calculation. This allows for an optimized implementation.
> 
> Notice that in the preferred model, the outputs are not probabilities, but can range from large negative numbers to large positive numbers. The output must be sent through a softmax when performing a prediction that expects a probability.
> 
> ***To select the most likely category, the softmax is not required.*** One can find the index of the largest output using [np.argmax()](https://numpy.org/doc/stable/reference/generated/numpy.argmax.html).

## SparseCategorialCrossentropy or CategoricalCrossEntropy

Tensorflow has **two potential formats** for target values and the selection of the loss defines which is expected.

- **SparseCategorialCrossentropy**: expects the target to be an integer corresponding to the index. For example, if there are 10 potential target values, y would be between 0 and 9.
- **CategoricalCrossEntropy**: Expects the target value of an example to be one-hot encoded where the value at the target index is 1 while the other N-1 entries are zero. An example with 10 potential target values, where the target is 2 would be `[0,0,1,0,0,0,0,0,0,0]`.

# 🎢 Advanced Optimizations! (not GD)

There is ***an intelligent*** optimization algorithm which automatically knows "how much learning rate to set" to get the network trained faster: `ADAM`.

> Based on *how the gradient descent* is going (the direction), ADAM automatically adjusts the learning rate — increases or decreases to make the network train faster and optimal.

**ADAM: Adaptive Moment Estimation**.

### 🤯 ADAM uses different learning rate for *each* unit of the model

Yeah, so unlike *regular GD*, ADAM uses different learning rate for each unit or node in the neural network to set each node at different rate of learning based on their preformance; **how cool is that!!** 

— instead of single learning rate for all nodes!

> That's why it 

A little code might help:

```python
model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3),
              loss=tf.keras.SparseCategoricalCrossEntropy(from_logits=True))
```

# 🎓 Backpropogation Masterclass

### Preface

The goal is to learn **how the given parameter `w` affects the target function** and that ***how*** is defined by the *slope* or the *gradient*. That is known, updated on repeat; and called "learning".

### So, what is the background?

> *I would **highly recommend** you to check out my course with Andrej — in which I explore how the slope and gradients work in detail with many interactive examples here → [First Lecture Micrograd](https://github.com/AayushSameerShah/Neural-Net-Zero-to-Hero-with-Andrej/blob/main/01%20-%20Micrograd/Micrograd%20Foundations.ipynb)* 😲

- We have one variable (weight) say `w`

- We want to know the slope of this variable.

- The slope means, how much change in this variable will impact the loss

- This is done by taking ***infitesimmally*** small amount like `0.000000000000001` and know its change in the function.

Let's do that a little mathematically.

### Simulation - 1:

Say the function is: $f(w) = w^2$. Means, whatever the value of `w` is, we will get the result `w**2` back; as a result.

- Say when `w=3` we get `f(3)=9`.

- Now, if we add *very (infitesimmally) small amount* to `w`, how much change happens in the function? (that shows how much w impacts!)

- So, now the updated `w = w + 0.0001` and thus `f(3.0001)=9.0006`.

- That means when `w=3` then a small change in its value will lead to `0.0006` increase in the function.

- That means the slope of the function for `w=3` is `6`.
  
  - That is simply calculated by $(new - old) / change$
  
  - ie, $(f(3+0.0001) - f(3))/0.0001$

### Simulation - 2:

- Consider this `f` as the loss function `j`.

- When `w` was `3`, we had the loss of `9`.

- So what change should I make in `w` to make loss `0`? Or close to zero?

- This is done with the `slope` that we get. The slope **will be different** for each `w`.

- Now, lets do `w = w - learning_rate * slope`, so `w = 3 - 0.01 * 9` so now `w` is `2.91`.

- And now `f(2.91)=8.46`

- Cool! 😎

### Thus...

👉🏻 When we have the **large slope** the update step gets **big update!** (0.001 * 123 gives 1.23 update)

👉🏻 When the slope **is small** the update step gets **small update!** (0.001 * 1.23 gives only 0.0123 update)

>  That's all you need to know about the derivation! *(for now)*
