# 🤔 What to do next if the model isn't working?

- Get more training examples

- Remove some irrelevant features

- Add relevant features

- Add polynomial features

- Decrease lambda ($\lambda$) parameter used in regularization

- Increase lambda ($\lambda$) parameter used in regularization

# 🩺 Machine Learning Diagnostic

> *Diagnostics can take time to implement, but implementing them will save a lot of time.*

1. Split the dataset (obviously?)

2. See the testing error.

## 💃🏻 Model selection (overall - interesting!)

Say, we have 5 different models **with varying degrees of polynomials**:

1. Model-1: Degree1

2. Model-2: Degree2

3. Model-3: Degree3

4. Model-4: Degree4

5. Model-5: Degree5

👉🏻 If say, we get the best **test error** (the lowest) with "Model-4" then ***still we should not select the model 4 as the final model*** because that would be **overly optimistic** on the test error.

👉🏻 Meaning, the "actual real world" error would still be higher than the test **because we have set and selected the `d` based on only the test error** during training.

### Just do this 👇🏻

> ⭕ Instead of "training" and "testing"... 
> 
> 🟢 We go for **"training set"**, **"*(cross)* validation set"** and **"test set"**!

## 🧻 Cross validation set

>  Development set / Dev set / Validation set / Cross validation set

**Note:** While testing the error, we don't include the **regularization term**. We just use that during training loss.

- After we get 3 different set, get the error for all of these three.

- Compare (out of 5 models in our example) validation set errors.

- Pick the model with **lowest validation error**.

- <u>Then</u> note the error for that model on the test set!

This is why the test set would be **a fair estimate** for the generalized model error!

> 👉🏻 Same goes for the neural networks. **You train different** neural nets, choose the one which has lowest validation error and then finally report the test error for that lowest dev error model.

# 💊 Diagnostic-1: Bias and Variance

![](..\images\bias-variance-tradeoff.png)

 Just choose the one which is in the middle!

## 🧔🏻 What is "high" and "low"?

How high is too high... you've got the point.

→ **Have a baseline by these methods:**

- Human level performance

- Other algorithm error (previously)

- Guess based on experience

Once you have the base line, then compare the model with these:

1. Baseline error 

2. Training error

3. Validation error

Example (let's say on a dataset of speech recognition):

1. Baseline error: 10.6% (even human can only reach at this error!)

2. Training error: 10.8% 

3. Validation error: 14.8% 

Now we conclude high bias or high variance by difference of:

1. `Training error - base line` = high bias or low determination

2. `validation error - training error` = high variance or low determination

Cool!!!

> Sometimes the **baseline error** can be 0. But sometimes it could be some number as in speech recognition where there is a lot of noise and so on.

## ⤴ Learning curve

![](..\images\learning-curve.png)

> As the training data gets larger, it is likely that the training error gets larger...

## Learning curve when high bias

![](..\images\learning-curve-high-bias.png)

- When there is a high bias, then the curve will **flat out**.

- The model may be too simple.

- No matter how many more data you throw, it will not bring the error down.

## Learning curve when high variance

![](..\images\learning-curve-high-variance.png)

- Here the performance of the validation is much higher than the training.

- **Adding more data would help** here 👇🏻

![](..\images\learning-curve-high-variance-2.png)

> ### The main part is:
> 
> ### You train the model on subset of data, and not as a whole to see this type of learning curve.

That is important, to plot such curve, you need to keep adding more data. Which is in contrast to other **plots** where we were looking at the curves on whole data with some other things changing like `b` and `d` and so on.

### Knowing bias or variance is important!

- Say your model doesn't perform well on the given problem, then **instead of collecting** more data, you must modify the model or features or do some engineering in the dataside or model side.

- This is the high bias problem, collecting more data won't help. 

- So knowing where you are right now, is important!

### Do ✌🏻 things.

1. Plot the model's performance for different degrees of hyperparameter (like degrees) for training and test error to know the sweet spot.

2. Gradually add the data, to get the learning curve.

# 💊 Diagnostic-2: Error analysis

#### **Plot (overall thing)**:

- Let's say out of 500 examples, your model gets 100 wrong.

- Then **manually checking** these 100 examples and **their properties** may tell you more about where and on what kind of examples your model suck.

- This involved **creating groups of examples** with **common themes or properties** so that you can get a general sense.

> So we can see here that, the term **error analysis** is the analysis of the records which have caused this issue with the model. It is that simple 😊

#### If the examples to analyze is not `100`, then?

Make it 100! You don't want to analyze all examples by your own, you simply don't have time. Take the samples and that will generally give a good sense of overall population of errors!!!

# 💿 How to collect more data? (Yeah it has some ways!)

## `1.` Collect more data (just collect)

1. Trying to get **all types of data** can be slow and expensive. (Like you need more data, I agree but of what aspect?)

2. So, it is **a better choice** to collect data for only some aspect. Like you get a sense **from error analysis** that "the examples in this group" are causing more errors, then collecting more relevant data of such group can be more fruitful than getting all kinds of data! 😉

## `2.` **Data augmentation**!!!

1. In the case of "letter recognition" (in which you recognize `A`, `B` etc from an image) you can create more images...

2. Like flipping, rotating, shrinking, enlarging, mirroring, **warping** etc.

3. Now you have more images to train the model on!

---

4. In case of audio recognition, for the same example you can **add some background noise**.

### 🤚🏻 But, but, but...

Don't add noise or distortions **which are irrelevant!**. Just don't add random noise!

> *So think it like, when you want to distort the data, distort it in a way that you get the t=new data in the real world. In different settings.*

## `3.` Data synthesis

This **is not manipulating the existing dataset** but **creating a new data.** Yes, **<u>creating</u>**.

How? Depends. Just create! Take screenshots, do it yourself, record your voice, use GPT... just create!

## `4.` Transfer learning!

>  This is a GOOD ONE! Don't miss reading.

The issue is **we don't have enough data.** So we need to do something to make the model better to perform well.

Now, in this **transfer learning**, we ***don't touch the data at all!*** — instead we take advantage of `2` models.

**Overall *(with example)*:**

- Let's say you want to train a model which can distinguish between 10 digits (just classify 10 digits)

- So you train the model and still don't get the proper result.

- Here, you get an idea of applying transfer learning.

- So, what you do is **train another model from scratch** or **take an existing pre trained model** which is trained on similar task or general task.

- Say you train the model which is trained on the dataset of all objects in the world, cats, dogs, horses etc.

- 👉🏻 Here the model will have layers [L1, L2, L3 and Output].

- The output layer with 1000 categories (say)

- Now you have **2 options for transfer learning**:

#### `1.` Freeze all weights of general model

Here you freeze all weights of the hidden layers **and train the model's output layer weights for your new task (of digit classification).**

So now, all [L1, L2, L3] will stay the same just the Output layer will change as it will have 10 nodes only. And these weights will be trained for the digits classification.

#### `2.` Freeze nothing, train as usual.

Here, you don't freeze any weights but **train the model as usual.** The advantage you get here is **the model won't learn from scratch** as it will have **some initial weights which make sense** from the general learning.

This will help the model to learn faster and hopefully it will work out.

> ### 💡
> 
> **Motive:** With doing all this, you get the advantage of the model's previous knowledge (general knowledge) as it already has seen digits with other things and now it will hopefully learn things well.
> 
> **Choose - Option - 1** when you have ***really small training set for this task.***
> 
> **Choose - Option - 2** when you have **large training set**.

___

📝 The first model training is called **Supervised pretraining!**; Yes **pretraining.** And the second model training is called **fine tuning!**.

___

# 🗺 Building an ML system — think through

> **Starter:** Training a model, is just a part of a puzzle. There are other things as well! 
> 
> — Andrew

1. **Project scoping:** Decide what you want to solve. (Ex. I want to solve typing, don't want to type, but text should be typed by voice)

2. **Collect data:** Carry out the initial data collection.

3. **Train the model:** Train, get loss, error analysis and iteratively improve the model (repeat step 2 and 3 🔄)

4. **Deploy in production:** Make sure to monitor the performance of the model. Here also you can go back if the model is not working.

# ⛸ Skewed Distributed Dataset

- If you make a function which always predicts `0` class then you will get only 1% error! Because the training dataset had 99 for `0` class and 1 for `1` class.

- There could be another "model" which may have 2% error but could be better than this static even it has worse error.

- There, need to look for confusion matrix.

## So easy, the easiest explanation of Precision and Recall:

✨ **Precision:** Out of all records we predicted as TRUE how many of them were actually TRUE?

- Means if the algorithm says "this is TRUE" then what is the chance it actually is TRUE?

🌟 **Recall:** Out of all actual TRUE records, how many of them were identified as TRUE?

- Mean the algorithm is able to identify 'this many' percentage of records correctly as TRUE.

![](..\images\precision-recall.png)

This single is the BEST slide ever for the precision and recall. Ever.

# Precision and Recall Tradeoff

- See choosing the precision and recall is **not upto the cross validation** it is something that you set ie. setting a threshold.

## F-1 Score

If you have trained 3 algorithms and their precision and recalls will be different, then how would we choose which is the good algo?

`F1 Score` combines both. 

> Average doesn't work as it treats both precision and recall the same. There even if you had precision `0.01` and recall `1` then the average would come `0.51`. We can see that precision looks suspicious (maybe printing 0 all times.)
> 
> 
> 
> So F1-Score gives much more emphasis to the smaller value by multiplying that and dividing them with their sum. This is **harmonic mean**.



Amazing stuff! Let's meet in the next week for learning Gini 🧙🏻‍♂️