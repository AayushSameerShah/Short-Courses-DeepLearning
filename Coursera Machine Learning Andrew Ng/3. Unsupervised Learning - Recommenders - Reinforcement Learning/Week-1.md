# 🔯 Clustering

It is where we don't have the labels given, but we group the records together based on their similar characteristics. 

## K-Means

This is simple, how it works -- so I am not providing the description here. But there is one area that may require some attention; which is **there is the <u>objective function</u>** for the K-Means too. 

**The cost function of K-means**:

$\frac{1}{m} \sum_{i=1}^m ||x^i - \mu^i||^2$

Which is simply the distance between the cluster centroid and the example that the cluster centroid is assigned. And we will be trying to minimize that.

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\clustering-objective-function.png)

- And... that 👆🏻 is how the objective function looks like.

- That is very simple, just compute the distances between each records and their cluster centroids.

> On every single iteration the cost function should **and must go down or stay the same**. It can't go up. If it does, then there is the bug in the code.
> 
> — Andrew 

## ⭕ Initialization of the centroids

- It all depends on the initialization of the centroids.

- It **may happen** that the algorithm converges to the local minima where we have not placed the cluster centers in the optimal places.

- In such scenario, ***we should create multiple models*** and then see which model is giving the **<u>lowest cost</u>**.

> *Because clustering is unsupervised learning algorithm **you're not given the "right answers"** in the form of specific labels to try to replicate.  There are lots of applications where the data itself does not give a clear indicator for how many clusters there are in it.*
> 
> — Andrew

# 🖐🏻 Choosing the value of `k`

### `1.` Elbow method

- It doesn't work when you have the plot which doesn't form the elbow proper.

- Some times, the plot may have the smooth curve.

- Increasing `k` would always result in the lower cost function.

- ***Isn't recommended by Andrew.***

### `2.` Evaluate K-means for downstream purposes

- This is recommended approach.

- The model that you are building will **often have the downstream purpose.**

- Evaluate K-means based on how well it performs on that later purpose.

>  👉🏻 **k-mean can be used for image compression!**



# 🏸 Anomaly detection

**Some opening thoughts and discussions which are <u>really required.</u>**:

- Let's take an example of "plane crash"

- If we had an algorithm which could tell in advance that ***there is some shady thing*** in the engine which you might want to consider, then the plane crash could be avoided.

- Now <u>the irony is that</u> in the real world **we don't have so many plan crash examples** so the model will only get to learn more about the *non crash* examples.

- **This might sound like a classification** thing, but the traditional ML algorithm for the classification would not be a good fit because they would not learn the anomalous data-points.
  
  ___

- Here, we need to employ the anomaly detection algorithm which can tell, ***this data-point is significantly different from the normal instances***.

### 🤖 Chat-GPT Discussion

1. **Imbalanced Data:**
   
   - The statement "there are not so many bad engines" suggests that the instances of faulty or problematic aircraft engines are relatively rare in the dataset. This is a common scenario in anomaly detection where the majority of data points represent normal behavior, and anomalies are the exceptions.
   - In such cases, traditional machine learning models may struggle with imbalanced data. Anomalies can be underrepresented, making it challenging for the model to learn their patterns effectively.

2. **Anomaly Detection vs. Classification:**
   
   - Anomaly detection and classification serve different purposes, although they share similarities.
   - In classification, you train a model to predict the category or class of a given input based on labeled training data. For example, you might train a model to predict whether an aircraft engine will fail or not fail (binary classification).
   - Anomaly detection, on the other hand, deals with unlabeled data and focuses on identifying instances that deviate significantly from the norm. It's not about predicting predefined classes but detecting unusual patterns.
   - In the context of the lecture, the goal is not to predict whether a plane will crash or not. Instead, it's about identifying anomalous behavior in aircraft engines, which might indicate potential issues.

3. **Density Estimation and Anomaly Detection:**
   
   - Anomaly detection often involves density estimation. In simple terms, the algorithm learns the normal patterns of the data and considers regions with low data density as potential anomalies.
   - The lecture describes plotting the features of normal engines on a graph and then identifying anomalies based on where a new data point falls in this feature space.
   - Density estimation helps the algorithm distinguish between typical and atypical behavior. If a new aircraft engine has feature values that deviate significantly from the norm, it may be flagged as an anomaly.

In summary, anomaly detection is more about identifying unusual patterns in data, and it doesn't necessarily predict predefined classes like a classification algorithm. It's particularly useful when anomalies are rare and may have serious consequences, as in the case of aircraft engine failures. The focus is on understanding the normal behavior of the system and flagging deviations from that norm.

___

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\anomaly-intuition.png)

# 〰 Gaussian Distribution

The formulae of the gaussian distribution for the number $x$ is:

### $f(x) = \frac{1}{\sqrt{2 \pi \sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2 \sigma^2}\right)$

- Which means, for the given `mean` and `std`, you can get the probability of the `x` (number) for that distribution.

**A little experiment:**

```python
def normal_distribution_pdf(x, mu, sigma):
    exponent = -((x - mu) ** 2) / (2 * sigma ** 2)
    pdf = (1 / (np.sqrt(2 * math.pi) * sigma)) * np.exp(exponent)
    return pdf
```

This is the function that we will use.

**Test - 1:**

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\normal-1.png)

**Test - 2:**

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\normal-2.png)

**Test - 3:**

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\notmal-3.png)

## 👼🏻 How is it applied in anomaly detection?

1. We have the data.

2. Now, the goal is **to fit the distribution** on this data. 

3. **Why?** Because if we know ***from which distribution these data points have come from,*** we can know **what is the anomaly, and how far is too far.**

4. Example: If the dataset **doesn't** follow the normal distribution (but some other) and if we think that this dataset follows normal distribution, then a new point which may have high p(x) but would be anomalous *(which would be wrong)*.

5. So the goal is to fit the distribution or find a distribution which fits well on the data **and we can say, probably *this* dataset is drawn from *this* distribution**.

____

# 🎯 In Practice *(amazing!)*

- We have the dataset (let's say have 3 features `x1`, `x2`, and `x3`)

- To "model" the distribution on these features we will apply the steps below. And BTW, "modeling" means **finding the set of parameters for the distribution which then can tell us $p(\vec x)$** for the new instance $\vec x$.

- The *"set of parameters"* differ from distribution to distribution, so if we are fitting the "gaussian" then it will only require to find the $\vec \mu$ and $\vec \sigma$.

☝🏻 **Process:**

1. Choose the features to include in the model *(obvious, not all features are relevant)*.

2. Fit the parameters $\vec \mu$ and $\vec \sigma$.
   
   1. This is just applying mean and std per feature
   
   2. Like `df.mean(axis=0)` and `df.std(axis=0)`

3. Given **a new example** compute $p(\vec x)$.

✌🏻 **Here to compute the $p(\vec x)$, we will use the PDF function of the chosen distribution:**

- $p(\vec x) = p(x_1; \mu_1, \sigma^2_1) \times p(x_2; \mu_2, \sigma^2_2) \times p(x_3; \mu_3, \sigma^2_3)$

🤔 **Why multiplication?**

- Let's say the $x_1$ corresponds to "engine heat" and $x_2$ corresponds to the "engine vibration".

- If $p(x_1; \mu_1, \sigma^2_1)$ comes out to be $\frac{1}{20}$

- And $p(x_2; \mu_2, \sigma^2_2)$ comes to be $\frac{1}{10}$

- Then **multiplying them will give us the conditional probability** ie. $\frac{1}{20} \times \frac{1}{10} = \frac{1}{200}$.

- Which may tell how likely is the event **based on the threshold** that you've set.

# 👨🏻‍🦰 Choosing the "threshold" to declare anomaly.

Here we will "need some labels". That we *obviously will have*.

**The process looks like this:**

- Let's say we have 10000 total examples of "GOOD" and only 20 of "BAD" (anomaly) records.

- Here, you would create the dataset as the following:

| **Split** | # GOOD records | # BAD records |
| --------- | -------------- | ------------- |
| Training  | 6000           | 0             |
| CV        | 2000           | 10            |
| Test      | 2000           | 10            |

- There, while training **we are still not using any labels** we are just throwing in all the data ***assuming they all are the good examples***.

- And it is also ***okay*** if a couple of BAD records (anomaly records) are slipped in that training set.

- Then we will <u>tune the epsilon</u> parameter $\epsilon$ as the threshold on the CV split.

- Done!

# ✨ When I have balanced data, can I make this case "Supervised instead"?

| Criteria                          | Anomaly Detection                                                                                                                       | Supervised Learning                                                                                                  |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Number of Positive Examples**   | Suitable for very few positive examples `(0-20 examples)`.                                                                              | More applicable when you have a **larger number** of positive examples.                                              |
| **Number of Negative Examples**   | Works well when there is a relatively large number of negative examples.                                                                | Effective even with a **balanced** or larger number of negative examples.                                            |
| **Types of Anomalies**            | **Ideal** when there are many **different** types of anomalies or positive examples.                                                    | Assumes future positive examples are **likely to be similar** to ones in the training set.                           |
| **Adaptability to New Anomalies** | Capable of detecting **brand new anomalies** or positive examples that have **never been seen before**.                                 | Assumes future positive examples **will be similar** to those in the training set.                                   |
| **Applications**                  | Useful for **fraud detection** with **constantly evolving methods**, email spam detection, and manufacturing for detecting new defects. | Suitable for tasks like weather prediction, where the **output labels are consistent** and don't change drastically. |
| **Security-Related Applications** | Commonly used in security-related applications due to the potential for hackers to find brand new ways to exploit systems.              | May be less suitable for security-related tasks unless the positive examples remain consistent over time.            |

# 💻 Choice of features

> # ""
> 
> When building an anomaly detection algorithm, I found that choosing a **good choice of features turns out to be really important**. 
> 
> In supervised learning, if you don't have the features quite right, or if you have a few extra features that are not relevant to the problem, **that often turns out to be okay.** Because the algorithm **has the supervised signal that is enough** labels why for the algorithm to figure out what features ignore, or how to re scale feature and to take the best advantage of the features you do give it. 
> 
> ***But for anomaly detection*** which runs, or learns just from unlabeled data, <u>is harder for the algorithm to figure out what features to ignore</u>. 
> 
> 
> — Andrew

### `1.` Choose the Gaussian features; if not make them!

- Gaussian features help the algorithm for the anomaly detection.

- Say your variable $x_n$ is not gaussian, then you can **transform it** by employing several methods.

**How to transform?**:

1. `np.log(x)`

2. `np.log(x + 1)`

3. `np.log(x + n)` (try different values)

4. `np.sqrt(x)` ($\sqrt{x}$ or $x^{\frac{1}{2}}$)

5. $x^{\frac{1}{3}}$

6. etc...

### `2.` Error analysis

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\ambiguous-anomaly.png)

- It may happen that the given anomaly seems under **the normal range** but it actually is not.

- In this case **you would see that particular record** and see "why" it failed and "what" made them stand out.

- This way, you can think that *if it were you to decide whether this is an anomaly or not, what would you spot?*

- By doing that, **you will come up with entirely new feature** that may help detect anomalies better! 👇🏻

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\added-feature-anomaly.png)



# 🔥 Extra content:

⁉ **Can we model the anomaly detection as classification problem?**

🅰 Yes, it is possible to frame the anomaly detection problem as a classification problem, but there are some considerations and challenges to be aware of:

1. **Imbalanced Data:**
   
   - As mentioned earlier, the data in anomaly detection scenarios is often imbalanced, with normal instances far outnumbering anomalies. This can lead to challenges in training a classification model, as the model may become biased towards predicting the majority class.

2. **Labeling Anomalies:**
   
   - In a classification approach, you would need labeled data where instances are categorized as either normal or anomalous. Labeling anomalies might be challenging, as they are usually rare events and may not be well-represented in the dataset.

3. **Threshold Selection:**
   
   - Anomaly detection often involves setting a threshold to distinguish between normal and anomalous instances. In a classification approach, you would need to decide on a threshold for classifying instances. This can be non-trivial, and the choice of the threshold may impact the model's performance.

4. **Feature Engineering:**
   
   - Feature engineering becomes crucial in classification. You need to choose or engineer features that effectively capture the differences between normal and anomalous instances.

5. **Model Selection:**
   
   - Traditional classification algorithms (e.g., logistic regression, decision trees, support vector machines) can be applied, but they may not perform as well when dealing with imbalanced data and rare events. Specialized techniques, such as ensemble methods or anomaly-specific models, might be more suitable.

In summary, while it's possible to approach anomaly detection as a classification problem, it comes with its own set of challenges. Anomaly detection methods, particularly unsupervised ones like density-based or clustering approaches, are often preferred in scenarios where anomalies are rare and difficult to label. These methods can learn the normal behavior of the system without explicitly requiring labeled anomaly data.

Certainly! Let's address each of your questions:

⁉ **What does it mean to "fit" a distribution?**

🅰 When the professor talks about "fitting" a distribution, he means finding the parameters of a probability distribution that best describe the given dataset. While the data may have some inherent distribution, the goal is to find a theoretical distribution (in this case, a Gaussian distribution) that closely matches the observed data. This theoretical distribution is then used as a model to make predictions or analyze the data further.

In the context of anomaly detection, fitting a distribution helps in understanding the normal behavior of the dataset. Once you have a good fit, you can use the distribution to identify anomalies by looking for instances that deviate significantly from the expected pattern.

⁉ **What does he mean by "estimate a good choice for the mean parameter Mu and the variance parameter Sigma squared"?**

🅰 Even though the dataset has its own mean and standard deviation, the idea is to find the best-fitting Gaussian distribution that represents the data. This involves estimating the parameters of the Gaussian distribution, namely the mean (Mu) and the variance (Sigma squared).

While the dataset may have its own mean and standard deviation, these are considered as sample statistics. The professor is introducing the concept of estimating population parameters (mean and variance) based on the given dataset. The mean (Mu) estimated from the dataset becomes the center of the Gaussian distribution, and the variance (Sigma squared) becomes a measure of how spread out the data is.

So, when he talks about "a good choice," he means finding values for Mu and Sigma squared that make the Gaussian distribution a good fit to the observed data, allowing it to capture the central tendency and variability of the dataset.

⁉ **What is maximum likelihood estimation (MLE)?**

🅰 Maximum Likelihood Estimation (MLE) is a statistical method used to estimate the parameters of a probability distribution. In the context of the lecture, MLE is applied to estimate the mean (Mu) and variance (Sigma squared) of the Gaussian distribution.

In simple terms, MLE seeks to find the values of the parameters that maximize the likelihood of observing the given dataset under the assumed distribution. The likelihood is a measure of how well the distribution explains the observed data. By maximizing this likelihood, you find the parameter values that make the observed data most probable.

In the lecture, the formulas for Mu and Sigma squared that the professor provides (average for Mu and average of squared differences for Sigma squared) are, in fact, the maximum likelihood estimates for the parameters of a Gaussian distribution based on the given dataset.

⁉ **What if the Gaussian distribution doesn't explain the data well?**

🅰 If the Gaussian distribution doesn't provide a good fit to the data, it may be necessary to consider alternative distributions. Not all datasets follow a Gaussian distribution, and different types of data may be better described by other probability distributions. Some common alternatives include:

- **Log-Normal Distribution:** Useful for data that is positive and right-skewed.

- **Exponential Distribution:** Suitable for modeling the time between events in a process that occurs at a constant rate.

- **Poisson Distribution:** Appropriate for count data, representing the number of events in fixed intervals of time or space.

- **Uniform Distribution:** Useful when all values in a range are equally likely.

The choice of distribution depends on the characteristics of the data. There isn't a one-size-fits-all solution, and domain knowledge is often crucial in selecting an appropriate distribution.

⁉ **Is the procedure to test multiple distributions and pick the best one?**

🅰 Yes, in practice, you might explore different distributions and compare their goodness of fit to the data. However, it's important to note that this process is often more nuanced. Instead of manually testing each distribution, statistical methods and techniques are employed to assess the goodness of fit.

Common techniques include:

- **Visual Inspection:** Plot the data against the assumed distribution and visually assess the fit. Deviations from the theoretical distribution might suggest a poor fit.

- **Quantile-Quantile (Q-Q) Plots:** Compare the quantiles of the observed data with the quantiles of the theoretical distribution. A straight line in a Q-Q plot suggests a good fit.

- **Statistical Tests:** There are formal statistical tests, such as the Kolmogorov-Smirnov test or Anderson-Darling test, that quantify how well the observed data aligns with a given distribution. These tests provide p-values that indicate the likelihood of the observed data coming from the assumed distribution.

⁉ **How to know whether the assumed distribution is a good fit or not?**

🅰 The goodness of fit is generally assessed through a combination of visual inspection and statistical tests. If the visual examination of plots and Q-Q plots suggests a close match between the theoretical distribution and the data, that's a positive sign. Additionally, statistical tests can provide quantitative measures of fit.

It's important to remember that in real-world scenarios, finding the "perfect" distribution is rare. Instead, the goal is often to find a distribution that is a reasonable approximation and meets the requirements of the analysis or modeling task at hand.

Domain knowledge plays a crucial role in guiding the selection of an appropriate distribution, and sometimes, empirical considerations may also influence the choice. The ultimate goal is to have a model that captures the essential characteristics of the data for the specific application.

____

💬 Continue the chat:

- [Anomaly Detection Clarification](https://chat.openai.com/share/c355c783-b8db-4196-956c-cf8ab92b1b2c)

- [Normal Distribution Doubts](https://chat.openai.com/share/e17ba8e1-8af9-4bae-a42b-21aae9ec3255)


