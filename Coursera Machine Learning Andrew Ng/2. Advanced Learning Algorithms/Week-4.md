# 🌳 Trees!

When creating a tree, there are certain decisions to take:

1. **Decision - 1:** Which feature to choose?

Here the answer is: *Choose the feature which has highest purity.* We call this **entropy.**

2. **Decision - 2:** When to stop creating the tree?
- When the maximum **depth** is reached.

- When the leaf node has **all same** class.

- When the leaf node has **impurity** less than certain threshold.

- When the node has less than threshold **number** of records.

# Purity 💜🌟

Some words:

- **Entropy = Impurity.**

- Purity is purity 🤗

>  👉🏻 So high entropy (hence high impurity) is bad.

## ⤴⤵ The entropy curve

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\entropy.png)

- The entropy curve is given above.

- The `x - axis` represents the probability (of just the fraction) of your TRUE class.

- The impurity (entropy) is highest when there are **50-50** classes. This will be a bad split.

- For an example if there are 40 TRUE and 40 FALSE examples then this would be a freaking bad split.
  
  - We will calculate the entropy via: taking the P(TRUE) = 40/80 = 0.5
  
  - Hence 0.5 → 1.
  
  - We can see that the entropy is highest there.
  
  - If there is ***some imbalance*** like 70 TRUE and 10 FALSE then 70/80 = 0.87 = ~0.6 will be the entropy - which is better.

- And that's it! Choose the split which decreases the impurity.

### Entropy equation:

- The TRUE class is $p_1$

- The FALSE class is $p_0$

- Hence, $p_0 = 1 - p_1$.

**The equation is:**

#### $H(S) = -p_1\text{log}_2(p_1) - p_0\text{log}_2(p_0)$

Here the $S$ is the set — the mixture of different classes. So we will calculate the entropy of ***that*** set $S$.

> 💡 
> 
> You may think that **the formula of this impurity is very similar** to what we saw in logistic loss! And yes indeed! That's why logistic loss is also called "Binary cross entropy!".
> 
> The logistic loss looked like: $L(f(\vec x), y) = -y(log(f(\vec x)) -(1 - y)(log(1 - f(\vec x)))$

## 🧔🏻 Gini?

If you don't want to use the `entropy` then Gini impurity can also be used.

# 🖐🏻 Choosing which feature to split (information gain)

> In decision tree, the way we decide which feature to split on; will be based on the choice of feature reduces entropy the most.
> 
> Here, the **reduction of entropy** is called **information gain**.
> — Andrew

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\choosing-split.png)

So, when we choose any feature we will get the entropy for each split.

Say...

- I split with `feature - 1` and on the left node I get entropy 0.8 and right we get 0.6.

- with `feature - 2` we get left 0.7 and right 0.6.

**Now which one to choose?**

So...

> *Rather than comparing the entropy of each split, we **need a way to average them**. Once again <u>a way to average them</u>*.

This **isn't the simple average**. Because we shouldn't be using the simple average. The impurity that we have **has come from the number of records in each branch.** So, **the number of records matter.** — and thus it will be a <u>weighted average.</u>

Now, say for each feature: (don't calculate, just feel.)

- `feature - 1` we have weighted average of 0.6

- `feature - 2` we have weighted average of 0.3

Now, we should choose the `feature - 2`, right?

But no, just a single step.

## ℹ Information Gain

We **compare the current entropy with the previous node entropy** and we see how much **reduction in entropy we get from this split**.

So, say from root node, or previous node we had the entropy of **0.8** and now:

- 0.8 - entopry(feature 1) = 0.8 - 0.6 = `0.2`

- 0.8 - entropy(feature 2) = 0.8 - 0.3 = `0.5`

And that, my friends is **information gain.** I love the naming here 💓

**→ THE HIGHER INFORMATION GAIN IS BETTER ←**

So, in this case we would choose the `feature-2` for the split 🤗

> 📝 The reduction in entropy / information gain **is also one of the criteria** to top the learning.

# 🐞 Which value to ask if feature has more than 2 values?

Till now we ask the question like: **Is it male?** then just the answer is YES and NO because that simply means other thing is **female**. 

But what if the feature has `4` values like: North, East, West and South, then what to ask? What is **the best ask value?**

- Should I ask: Is it North?

- Should I ask: Is it South?

- Should I ask: Is it East?

- Should I ask: Is it West?

These are all values of that same feature. 

___

💡 Here, Andrew suggests: ***we should one-hot encode the categorical features.***

___

This way, all values will get their **different features.** And the algorithm will work just fine.

# 🔢 Continuous feature selection and processing

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\continuous-splitting.png)

- For that feature, we will take multiple thresholds (numbers)

- For each threshold, we will get the information gain

- The number which gives the highest information gain, wins.

> ### 🤔
> 
> **How to offset thresholds?**: The common practice is, we will sort the data with that feature first, then will take the next threshold as the next record number average. Means if the values are like `[2, 5, 7, 8, 12]` then we will take the thresholds like `[2.5, 6, 7.5, 10]`. So if there are 10 records, there will be 9 thresholds to test.

### 📝 Ending notes for classification trees:

1. We will calculate the entropy (impurity) for the SET, and not for some class.

2. The set can have mixture of classes (4 dogs, 5 cats) etc.

3. The formula of entropy is: $H(S) = -p_1\text{log}_2(p_1) - p_0\text{log}_2(p_0)$

4. If the problem is **multiclass,** the same entropy formulae will be applied but it will go like: $H(S) = -p_0\text{log}_2(p_0) - p_1\text{log}_2(p_1) - p_2\text{log}_2(p_2) ...$

5. The generalization would be: $Entropy(S) = - \sum_{i=1}^{K} p_i \cdot \log_2(p_i)$

# Regression Trees 🧙🏻‍♂️

- We take the average in the last (leaf node)

### 🧻 How does the entropy look like in the regression setting?

> #### 🔑 Key sentence!
> 
> ***Rather than trying to <u>reduce entropy</u> as we did in classification, here we will <u>reduce variance</u>.***

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\variance-decision-trees.png)

- This is **very similar** to what we did in the classification trees.

- Here we **take the variance** instead of **entropy** and then the same way we take the weighted average.

- Then we will also measure the **reduction in variance!** 👇🏻

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\variance-decision-trees-2.png)

# 🌳🌲🎄 Random Forest 🎋🌲🌴

- Create sub-sets of the original dataset with the same number of rows **with replacement**.

- If you just create `B` samples (say 50) and train 50 models on these, then these will be called "Bagged decision trees". Because the it is just like putting the records in the bag and simply picking up randomly.

- **To make it truly `Random`**, we will introduce another randomness. With these `50` subsets, **at each step of splitting** the algorithm will **NOT take all features into account** but it will only consider `k` features out of `n` features. 

- These `k` features will be selected on random bases. This is because, if we were to create different decision trees, even with the sub-sets, many times the **same root** node will be chosen.

- The typical choice for the `k` is `k = sqrt(n)`.

# 🔥 XGBoost

> #### Hitline:
> 
> ***Instead of getting trained on those examples which you are already good at, get trained on those which you perform poorly on.***

So **while creating the subsets,** instead of picking the records with equal probability `1/m` for each records, here we will pick the records which were misclassified before and make them highly probable of getting picked.

### "Deliberate Practice.""

**XGBoost** is most commonly and widely used technique for the boosting algorithm (Extreme Gradient Boosting).

> *Rather than doing sampling with replacement on each new tree, XGBoost assigns different weights to the each record in the original dataset. So, this way it is more sufficient.*

# Decision Trees vs Neural Networks

### Decision Trees and Tree ensembles

- Works well on tabular (structured) data

- Not recommended for unstructured data (images, audio, text)

- Fast

- Small decision trees may be human interpretable

## Neural Networks

- Works well on all types of data, including tabular (structured) and unstructured data

- May be slower than a decision tree

- Works with transfer learning

- When building a system of multiple models working together, it might be easier to string together multiple neural networks



Let's learn about unsupervised learning next!