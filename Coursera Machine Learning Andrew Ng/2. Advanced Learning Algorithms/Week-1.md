# 🧠 The Network Internals (quickly)

- When a node has a value, it just is a variable; when it applies some activation function then it becomes a **model**.

- Yes, with the activation function applied *(say sigmoid)*, that node has now become a small logistic regression model which only thinks and works for selected **local signals**.

- Agreed, the learning is done with the different loss function, and that won't be the log-loss function, but still, you got the point.

## At the initialization

- Input layer *(just the list of input-features)* has some connections with the next later.

- Like X1, X2 is connected with only Node-6.

- But in practice, we don't know that, so all Nodes in the middle layer are connected with every nodes in the input layer.

> ### 🆒
> 
> **A cool perspective of ANN's working:** When we were doing the *manual feature engineering*, we were trying to combine the multiple set of features (x1,x2) together and try making the better feature there; in the ANN, it does **this all automatically; by its foundation!**. So only those features will have the active weights which are important predicting the target.

# 👩🏻‍💻 A bit of a code

The initial way looks like:

```python
x = # 2D data
layer_1 = Dense(units=3, activation="sigmoid")
a_1 = layer_1(x) # the matmul (gives result after applying activation)

output = Dense(units=1, activation="sigmoid")
result = layer_1(a_1)
```

But, instead of passing the data **manually**, we can do that automatically:

```python
layer_1 = Dense(units=3, activation="sigmoid")
layer_2 = Dense(units=3, activation="sigmoid")
model = Sequential([layer_1, layer_2])
model(x, y)
```

 And by **convention** we write...

```python
model = Sequential([
    Dense(units=3, activation="sigmoid"),
    Dense(units=1, activation="sigmoid")
])
### ;)
```

# 👑❓ Are Neural Networks a major part of achieving AGI?

Here, we will see a **framework** to think through the answer of this question.

1. AI is made-up of 2 sup-types: 
   
   1. **ANI - Artificial Narrow Intelligence:** It has shown a tremendous progress in last years, detecting something, doing some specific task
   
   2. **AGI - Artificial General Intelligence:** Do anything that a human can do.

2. In Neural net we have **tried to simulate the human brain**. But we can see that doing this simple linear algebra in the ANN, is nothing like what the biological neuron does. And that can be the reason the AGI can not be as simple as it seems *(even though ChatGPT is here)*.

3. We have *(almost)* no idea of how **exactly** the brain works.
