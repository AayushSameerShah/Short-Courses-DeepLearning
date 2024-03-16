# 👱🏻‍♂️ Recommender System

**The basic idea:**

- Let's say we have 10 users and 6 movies in the database.

- In that case, 10 **might have** seen some movies and rated them and some of may not have been seen and rated.

- So, the task is to **predict** how much rating the user `ni` would have given to the unwatched movie `mi`.

- So say user 4, Aayush has not seen the movie "Black Swan" and based on his other movie preferrences if we predict he would rate Black Swan a rating of 3.5 out of 5, then we would recommend the movie to him!

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\recommender-intro.png)

> ### 👉🏻 The recommender system can have 2 settings:
> 
> 1. When you have the features of the movie **available**.
> 
> 2. When you **don't have the features** available.

Let's see both cases one by one.

# `1.` When the features of the movies are available.

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\recommender-features.png)

- Here, two features are available for each movie. The degree of romance and degree of action.

- There can be other features too, but generalize it as is.

## 👸🏻 The model

The **linear regression** to rescue. It is very simple.

> ***Predict the rating, for the user `i` for the movie `j` based on the learned weights.***

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\recommender-linear-regression.png)

- Here, we have the features x1 and x2.

- We are trying to find the movie rating "Cute puppies of love"

- And we have the learned the weights $w^{(1)}$ and $b^{(1)}$.

- The $w^{(1)}$ is for user `1` Alice.

- There can be $w^{(3)}$ which would be for user `3` Carol.

> 👉🏻 *This means, there will be `n` different linear regression models if there are `n` users.*

## 📖 How the model learns?

1. The model learning is **exactly the same** as the usual linear regression model

2. Just here we will feed the subset of data, meaning we **will only train the model** with the records which have the data populated.

**The example will make it more clear 🌟**

| Movie      | Aayush | Sameer | Shah | x1  | x2   |
| ---------- | ------ | ------ | ---- | --- | ---- |
| Black Swan | ?      | 4      | 5    | 0.3 | 0.01 |
| Snow White | 2      | ?      | 1    | 0.1 | 0.9  |
| Life of Pi | 5      | 5      | 1    | 0.9 | 0.9  |

> BTW, the given movies are not my first choices. I just picked them randomly. All of these movie suck.

### Model - 1 *(Aayush)*:

| Movie      | x1  | x2  | Target (rating) |
| ---------- | --- | --- | --------------- |
| Snow white | 0.1 | 0.9 | 2               |
| Life of Pi | 0.9 | 0.9 | 5               |

### Model - 2 *(Sameer)*:

| Movie      | x1  | x2   | Target (rating) |
| ---------- | --- | ---- | --------------- |
| Black Swan | 0.3 | 0.01 | 4               |
| Life of Pi | 0.9 | 0.9  | 5               |

### Model - 3 *(Shah)*:

| Movie      | x1  | x2   | Target (rating) |
| ---------- | --- | ---- | --------------- |
| Black Swan | 0.3 | 0.01 | 5               |
| Snow white | 0.1 | 0.9  | 1               |
| Life of Pi | 0.9 | 0.9  | 5               |

Neat! Isn't it?

## 💸 The Cost Function

The cost function is the same as well.

## $\frac{1}{2m^{(j)}} \sum_{i:r(i,j)=1} \left( (w^{(j)} \cdot x^{(i)} + b^{(j)}) - y^{(i,j)} \right)^2$

That's it. Just the **same** as regression. But here:

- The division is done with `m(j)` that means with only those records which are used for a particular user. For example for Aayush, there will be `m(j)=2` and so on — but that is generally omitted as it does very little effect. Thus, we just write $\frac{1}{2}$.

- The $i:r(i,j)==1$ indicates the records where the user have rated. So in the matrix it will be `0` and `1` to indicate have rated and not rated. 

#### 👮🏻‍♂️ This cost is not the actual cost.

We find the ***real cost*** across all users.

## $\frac{1}{2} \sum_{j=1}^{n_u}\sum_{i:r(i,j)=1} \left( (w^{(j)} \cdot x^{(i)} + b^{(j)}) - y^{(i,j)} \right)^2$

That's it, that's it buddy:

- So we will find the cost per user model, and say we get `[0.5, 0.9, 0.2]` for all three users.

- Instead of optimizing them with their individual costs, we will find the **average cost**.

- So the average cost would be `0.8` and that will be optimized.

# `2.` When the features are not available.

| Movie      | Aayush | Sameer | Shah | x1  | x2  |
| ---------- | ------ | ------ | ---- | --- | --- |
| Black Swan | ?      | 4      | 5    | ?   | ?   |
| Snow White | 2      | ?      | 1    | ?   | ?   |
| Life of Pi | 5      | 5      | 1    | ?   | ?   |

***This*** will be the case most of the time mate! When you don't have the features available, just you have the data about the user's rating and the product or the movie — here you need to ***"generate"*** the features.

> ## 💡
> 
> This. Is the actual **collaborative filtering algorithm.**

## 🕵🏻‍♀️ This is the first time when we will estimate `x`

Till now we have been focusing on finding the values of `w` and `b` for each features `x`. There, we had the `x` available per movie and for each user we did a good job.

Now, the situation is a bit different.

We need to estimate the `x` as well. Let me summarize things so no things get missed out while keeping things simple.

1. Here we will have the dataset simply like given below:
   
   | Movie       | Aayush | Sameer | Shah |
   | ----------- | ------ | ------ | ---- |
   | The FF 6    | 4      | ?      | 2    |
   | John Wick   | 5      | 2      | 2    |
   | Minimalists | ?      | 2      | 5    |

2. So, we will learn the values of `x` as well.

3. The number of the models to create will be just the same. We will still create the model for each user, but here we will also optimize `x` value.

4. To simplify, we will start with random values of: `w`, `b` and `x`

5. Here **our target will be the rating**.

6. So all of these parameters will be updated to find the target.

This is how it works in a nutshell. There are some changes in the loss function, but I am sure we won't need to go into that details now. The implementation will be simple as well. Just know that we will also learn the  `x` as a feature vector now. That's it.

And that is the collaborative filtering! Because this algorithm helps find the features "collaboratively".

# ❤ When user doesn't rate, but likes!

It becomes a binary variable when user "likes", "purchases", "interacts", "clicks" etc. That means, we need to show more of these, and also recommend a particular item.

**The situation is similar this time too.** We can solve this with 2 options:

1. When we have the features.

2. When we don't have the features.

> ***Hitline***:
> 
> This is **<u>just the same</u>** as the rating system **except** we will use the logistic regression here instead of linear regression!

### An example table

| Movie        | Aayush | Sameer | Shah |
| ------------ | ------ | ------ | ---- |
| Transformers | 1      | ?      | 0    |
| Avengers     | 1      | 1      | 1    |
| Avtar        | ?      | ?      | 1    |

Here:

- `1` Means the user has liked

- `0` Means user has not liked the movie **but was shown to him.**

- `?` Means the movie has not been shown to the user. **Which can be a candidate for recommendation.**

The procedure is the same:

- We will make the logistic regression models for each user.

- We will find the optimal values for `w`, `b` and `x`. And done. 

- Of course it will have its loss function, but that will be just a little modification of our original logistic loss function.

# 🌾 Mean Normalization

>  *Ah — don't miss this, it is so important!*

### 🦝 Current Case

| Movie         | Aayush | Sameer | Shah |
| ------------- | ------ | ------ | ---- |
| Sherlock      | 5      | 5      | ?    |
| Master's Plan | ?      | 3      | ?    |
| AI            | 4      | 5      | ?    |

Let's just take that this is the case, where we have one user *(here **shah**)* who haven't seen any movies.

In this case, if we try to learn the recommendation model, then we will get the weights like the below (assume we have only 2 features to learn — means `x` length is 2.)

- **Aayush**: x = `[2, 4]`

In this case, the weights for **Shah** will become `0` and recommendation for any movie will be??? `0`!

So that means, *We assume that Shah will give this movie `0` rating.*

### 👱🏻‍♂️ Now, the Mean Normalization

- Here we will **subtract means** of each movies from their ratings.

- Then based on that we train the model, with the scaled values.

For this example...

| Movie         | Aayush | Sameer | Shah |
| ------------- | ------ | ------ | ---- |
| Sherlock      | 5      | 5      | ?    |
| Master's Plan | ?      | 3      | ?    |
| AI            | 4      | 5      | ?    |

We will have the means:

- **Sherlock**: 2.5

- **Master's Plan**: 3

- **AI**: 4.5

So we will subtract:

| Movie         | Aayush | Sameer | Shah |
| ------------- | ------ | ------ | ---- |
| Sherlock      | 2.5    | 2.5    | ?    |
| Master's Plan | ?      | 0      | ?    |
| AI            | -0.5   | 0.5    | ?    |

This 👆🏻 will be the data on which the recommender model will get trained.

### The Main Part Is...

Now, we will have a different set of weights (probably) but still:

- **Aayush**: x = `[1.22, 2.3]`

- **Sameer**: x = `[2.33, 1.123]`

- **Shah**: x = `[0, 0]`

Still, the Shah has weights `[0, 0]`. **But**, when we make the prediction with: $x \cdot w + b$ we will get these scaled (normalized) rating. Thus **we will have to transform the numbers back to the original scale.**

The updated inference will be: $x \cdot w + b + \mu$. Where $\mu$ is the mean of a particualr movie.

With this instead of predicting `0` for all movies for **Shah** we will get the **mean** for Shah. Thus for **Shah** each movie:

- **Sherlock**: 2.5

- **Master's Plan**: 3

- **AI**: 4.5

Will be the predicted ratings! Which is more intuitive than predicting `0`.

**Other benefits:**

- The model will learn more efficiently

- The predictions will be better

- The algorithm gives more reasonable responses **even when** the user has very low ratings.

> ### 🙄
> 
> **Note:** Normalizing by movie can be beneficial in this case. But in other case, <u>normalizing by column</u> (or user) can be useful too.

# 💕 If you like this, you may also like that!

Let's find how "related items" are found!

> 👊🏻 ***Hitline***
> 
> In the earlier example, it was mentioned that "we have features of each movies" like the degree of romance, degree of action etc. But when we learn that, they don't be interpretable (like degree or romance) **but** they convey meaningful information though!
> 
> And based on them, yes guys, based on them we can get similar movies!

Just find the distance! And there you go!

## 👎🏻  Some limitations of Collaborative Filtering

### `1.` **Cold Start Problem**

- **Difficulty with new items:** Collaborative filtering struggles when there's a new item in the catalog with very few user ratings.
- **Limited data for new items:** Ranking a new item becomes challenging if only a handful of users have rated it.
- **Challenge with new users:** For users who have rated only a few items, providing accurate recommendations is a problem.
- **Mean normalization:** Although mean normalization helps, it may not be sufficient to address the cold start problem effectively.
- **Need for better methods:** Exploring better ways to recommend items to users with limited ratings, addressing the cold start problem.

### `2.` **Inability to Utilize Side Information**

- **Lack of incorporation of additional item/user information:** Collaborative filtering <u>does not naturally integrate side information about items or users</u>.
- **Examples of side information:** Features about a movie *(genre, cast, budget)*, user demographics *(age, gender, location)*, and preferences *(genre preferences)*.
- **Unutilized cues:** Information like user IP address, access device (mobile/desktop), web browser used, which could be useful, remains unexploited.
- **Potential correlations:** Details like the user's web browser can surprisingly correlate with user preferences, but collaborative filtering does not leverage such cues.

### `3.`  Opportunity for Content-Based Filtering

- **Introduction to content-based filtering:** Now, Andrew suggests exploring content-based filtering algorithms as a solution to collaborative filtering limitations.
- **State-of-the-art technique:** Content-based filtering is mentioned as a modern and effective approach used in many commercial applications.
- **Addressing collaborative filtering limitations:** Content-based filtering is expected to overcome the challenges posed by the cold start problem and the inability to use side information.

# 🤑 The idea of content based filtering

This algorithm takes advantage of **the properties of the parties involved** in the play — users and items.

#### **The differences:**

- **Collaborative Filtering**: Recommends the items based on the ratings of the user who have similar ratings as you. *(I have a doubt here, as we are not taking similar rating wale users!)*

- **Content Based Filtering**: Recommends items <u>based on the features of the item and the user</u>.

> **Note!** The content based filtering **can also have the ratings of the users**. But that will be considered as a "feature". Like "average rating of the movie". So, yeah it can have the rating too.

## 😋 Training

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\usr-net-movie-net.png)

- So, we will have **some set of features for users and for movies**.

- We will train multiple networks for both.

- Both networks will output a vector of same size.

- Both vectors will be multiplied.

- Then applying the sigmoid function, we can say whether or not to recommend this movie to the user.

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\nn-architechture-content-based.png)

- The main benefit of the neural network is **we can combine them the way we want.**

- Here both of them can learn simultaneously.

- They have the same cost function.

# 🧬 PCA

> *Commonly used by data scientist tp visualize the high dimensional data.*

**Main ideas:**

- Let's say you have 2 features and when you plot them (X1 and X2) you find that X2 only goes from 1 to 2 while X1 varies from 1 to 100.

- In this case, it will be more useful to just use the X1 for visualization because that solely provides more information about an overall object without need of X2.

- So we will take just that. But PCA does more things rather than taking the X1.

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\just-x1.png)

## The mutual importance.

As said above, ***we can't just drop the axes*** they both give some information. The below example shows just that, when we have the X1 and X2 from which we can't just drop because without another one can't give enough information 👇🏻

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\both-important-axes.png)

- The example above *suggests to find a new feature called "size" which may be a multiplication of x1 and x2*.

- So the PCA algorithm create a new feature which encapsulates the information from all of the axes.

> PCA reduces the features by removing the "less relevant" features which are not varying too much and by keeping those which vary more and can be viable for the visualization.

### PCA and Linear Regression difference

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\pca-vs-lr.png)

Summary of the difference between PCA and linear regression:

1. **Objective:**
   
   - Linear Regression: Predicts a target output (Y) based on input features (X).
   - PCA: Aims to reduce the number of features to represent the data efficiently, treating all features equally.

2. **Supervision:**
   
   - Linear Regression: Supervised learning algorithm requiring labeled data with a target output.
   - PCA: Unsupervised learning algorithm, works with unlabeled data, focusing on feature reduction.

3. **Treatment of Features:**
   
   - Linear Regression: Emphasizes a specific output (Y) for prediction, treating it differently.
   - PCA: Treats all features equally, finding an axis to retain maximum variance when projected.

4. **Algorithmic Purpose:**
   
   - Linear Regression: Predicts and minimizes the **vertical distance** between predictions and actual labels.
   - PCA: Reduces dimensionality while preserving as much variance as possible, irrespective of predicting a specific output.

5. **Visualization:**
   
   - Linear Regression: Used for predicting and visualizing relationships between input features and output.
   - PCA: Utilized for dimensionality reduction, aiding visualization of data with fewer features.

6. **Application:**
   
   - Linear Regression: Suitable for prediction tasks where a target output is known.
   - PCA: Applied when the goal is to reduce the number of features for improved efficiency in representation and visualization.

In essence, while linear regression focuses on predicting a specific output, PCA is designed for unsupervised feature reduction, treating all features equally to capture maximum variance in the data.

## 🏹 Getting the value of PCA

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\getting-pca-value.png)

- Let's say we have the 2 features in the data.

- Then we have a principal component (with some way...)

- Then to get the PCA value for ***that value*** we will have the values of principal vector `[0.71, 0.71]` *(this vector will have the same length as the number of features that we have).*

- Then, just **perform the dot product** and then we will get the value for that data point projected on the principal component.

## 🏗 Reconstruction

The data we have after applying the PCA is "compressed". But we can also get back the original data (not exact, but approximated).

![](E:\GitHub\Short-Courses-DeepLearning\Coursera%20Machine%20Learning%20Andrew%20Ng\images\reconstruction-pca.png)

- That's simple!

> ### Hint
> 
> 
> After running (fitting) the PCA, you must checkout the `explained_variance_ratio` variable to see how much variance is still retained in the PCA components! 

**Some ideas**:

- Let's say if you have 2D data and you also run the PCA to get `2` components only. Then what will happen?

- Then the `explained_variance_ratio` will add up to `1` because both PCA can together explain whole data.

- But the scale will change than the original.

## PCA Applications

1. Visualization

2. Data compression

3. To make supervised training faster



Let's meet in the last week!
