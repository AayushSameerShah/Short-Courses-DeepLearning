# ✌🏻 Scaling of the features

## `1.` Just division by MAX

## $\frac{x_i}{max(\vec{x})}$

## `2.` Mean Normalization

## $\frac{x_i - \mu_x}{max(\vec{x}) - min(\vec{x})}$

## `3.` Z-Scaling (Standardization)

## $\frac{x_i - \mu_x}{\sigma_x}$

# 🍾 Checking Convergence

1. Just plot the X → # Iterations and Y → Loss. It should create an elbow 😊😊😊

2. Use the **<u>epsilon</u>** like 0.0001 which is the difference between previous and current. And stop if the difference is small.

## Choose the alpha

- Need to try several and then choose.

- Plot them (the elbow)

- Choose one which is makes steady decrease in loss

- [0.001, 0.01, 0.1, 1]... also try increasing by `x3`.

> ## 💡
> 
> In the polynomial regression feature generation, you can ***also use the square roots!***



✌🏻 Not much in this week, so let's go to the week 3: classification
