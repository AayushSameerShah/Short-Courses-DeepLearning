# 🌌 Misc Talk

- In **unsupervised learning** we are not seeking the machine to give the ***right*** answers, but we are looking to get some hidden structure in the data.

- **Unsupervised Learning Types**:
  
  - Clustering
  
  - Anomaly detection
  
  - Dimensionality Reduction

# 👨🏻‍🏫 Supervised learning (cool things ahead)

> ⌚ Think in the terms of the `functions`!

[TRAINING SET] → [LEARNING ALGORITHM] → model ***f***.

#### Call: Model == function `f`

## 📖 How the model learns?

- The model learns by "**learning the optimal values**" of its parameters or weights. In the standard linear regression it is `w` and `b`.

- For the linear regression the cost function is:

$J(w, b) = \frac{1}{2m}\sum_{i=1}^m (\hat y^{(i)} - y^{(i)})$

> *Yeah, it is divided by `2m` instead of `m` or total number of records. But either way is fine.*

- Our **main aim is to find the optimal combination of weights** which reduces the cost function `J`.

- The ***contour plots*** can help visualizing the cost function and so on 😊

# 🎢 Gradient Descent

Of course we are not going to **try different combinations**. So this gradient descent provides a neat way to ***either reach at or near the minimum value of the cost function***.

- <u>Depending on the initial values</u> of `b` and `w`, we reach at different **local minimum** of the cost function `j`.

- The default values for simple model like linear regression are `f(w,b) = f(0, 0)`.

#### 🖖🏻 The equations for gradient updates

$w = w - \alpha \times \frac{\partial}{\partial w}J(w,b)$

$b = b - \alpha \times \frac{\partial}{\partial b} J(w,b)$

**Note the `—` sign?** It uses the negation sign because of the following:

- If the gradient is positive for a particular parameter, it means that increasing the value of that parameter will increase the value of the objective function. Subtracting the positive gradient from the parameter value will decrease the parameter value, moving in the direction that reduces the objective function.

- If the gradient is negative, it means that decreasing the value of that parameter will increase the value of the objective function. Subtracting the negative gradient from the parameter value will increase the parameter value, again moving in the direction that reduces the objective function.

In summary, the negative sign ensures that the optimization algorithm seeks to minimize the objective function by iteratively adjusting the parameters in the direction that reduces the function's value.

> 🔴 **Important**: While developing the code for this gradient descent, the update of all the weights should be done **simultaneously**. Meaning:
> 
> ```python
> # Correct ✅
> temp_w = w - derivative_w
> temp_b = b - detivative_b
> 
> w = temp_w
> b = temp_b
> 
> # Incorrect ❎
> temp_w = w - derivative_w
> w = temp_w
> 
> temp_b = b - detivative_b
> b = temp_b
> ```
> 
> Because the *incorrect way* first changes the one of the weights and that change is affected in the second weight change. So keep in mind.

## Derivative == Slope!

# 🦙 Working of Alpha (Learning rate)

- **Even if the alpha** is fixed. It will take smaller steps as it reaches to the local minimum.

- Like say first the derivative was 5 and alpha was 0.01, then the step to take become `0.05`.

- Now the slope is 2 and with the same alpha the it will be `0.02`. 

- As we can see that the step will become shorter as we reach to the local minimum.



# NEW 🤯

> # 👇🏻
> 
> 
> When we use the **squared error cost** function, <u>with linear regression</u> then **that function will never have local minimum** it will ***always have one single global minimum*** and thus the function will be a <u>convex function.</u>

(That's why probably we can use the ***emparical formula for the linear regression and still get these weights.***)

- This **normal** gradient descent is called "Batch" gradient descent.
