# `1.` Intro

- Catastrophic forgetting

- PEFT

# `2.` Fine Tuning

- Pretraining was "**self-supervised learning**" where you were given the labels, yes but still the objective was to understand the *underlying structure* of the language and predict the next token, instead of performing some specific task.

- That used billions of tokens and samples to train with.

**In contrast:**

- The fine-tuning is **"supervised"**, where the labels are given and are different from the prompt.

- The dataset is built with `PROMPT - COMPLETION` pairs.

- The dataset is much less, often in thousands of examples only.

# 🤔 Catastrophic Forgetting

> *This is a phenomenon in which when the LLM is fine-tuned on the specific new task, it looses the ability to perform well in the previous task it was trained on.*

### ⚕ Remedies to avoid this CF

1. Important to decide **whether** the CF is impacting the usecase!? That's okay if the model only knows how to classify text into positive and negative and does not summarize the text well! Hah!

2. If you need the model to **perform well on multiple tasks**, then you will need to construct the dataset which involves the examples of multiple tasks and these examples should vary between `50-1,00,000` **examples each task**.

3. Use PEFT! ***Each adapter for each task!***

> 📝 
> 
> Since this [multitask tuning] is the last step if the model (after pretraining), the authors of FLAN-T5 called this step: ***"The metaphorical dessert to the main course of pretraining"***.

# 📏 Evaluation Metrics

- **Rouge:** For ***summarization*** — compares one summary to one or more reference summaries.
  
  - Rouge-**1**: Compares single-single word pairs between reference *(human generated sentence)* and the model's response. (poor)
  
  - Rouge-**2**: Compares bigram (slightly better)
  
  - Rouge-**L**: Longest common subsequence — LCS (first see what is the maximum length of common matches between 2 sentences).
  
  - ***Precision, Recall and F1 scores*** are calculated for Rouge score.

- **BLEU:** Bilingual Language Understanding — For ***translation***.

## 📊 Rouge

> 🔴 It is also possible for the Rogue to be higher **for the bad completion**!!

**Bad because**:

- A single word **even repeated n times in the completion** can lead a higher score *(which can be solved by a "clipping function" i.e. keeping clip to `1` will only consider 1 repetition of the word to be considered in the calculation.* 

- **The order:** all words can be arranged in any order *(in the case of rouge 1)* and can lead to high score! *(clipping won't be affective)*.

## 📊 BLEU Score

>  It is calculated **based on the average <u>precision</u> over multiple n-gram sizes**.
> 
>  — *OR it also means* — 
> 
>  How many n-grams in the machine generation translation (completion) match those in the reference text!?

___



> ## ⚠
> 
> 
> **Rouge score** or **BLEU score** for different tasks are NOT comparable to one-another. *(Obviously)* And also they **should not** be used as a "final evaluation metric" to report the model performance — these are just the diagnostic scores.

# 🏖 The benchmarks

👉🏻 *Select the dataset(s) which can isolate the model's skills like reasoning or common sense knowledge.*

- **GLUE:** General Language Understanding Evaluation
  
  - ***Tasks:*** Question answering, Sentiment analysis etc.
  
  - In 2018.

- **Super-GLUE:**
  
  - ***Tasks:*** Multi sentence reasoning and reading comprehension.
  
  - More than GLUE and more challenging.
  
  - In 2019.

- **HELM:** Holistic Evaluation of Language Models
  
  - Performs multi metric approach
  
  - Total 7 metrics in wide tasks
  
  - ***Fairness, Bias and Toxicity is also included.***

- **Big-Bench:** Comes in 3 flavors.
  
  - 204 tasks...
  
  - In 2022.

- **MMLU:** Massive Multitask Language Understanding
  
  - Complex challenges
  
  - Maths, History, Law etc.
  
  - In 2021.

# ⚡🚈 PEFT

→ **Three** high level methods exist

1. **Selective**

2. **Reparameterization**

3. **Additive**

### 🧮 Selective

Tunes **only certain weights of the model**. 

- You can choose between certain weights

- or, Specific layer

- or, Specific type

### 👸🏻 Reparameterization (LoRA)

Instead to touching and changing the parameter of the model, here we **train the new set of weights** and then **add these weights with the model** at inference time.

- These additional weights are added in the self-attention block

- It is found that adding these new weights in the self-attention block is highly affective *(since most of the computation is done there)*

### 🎀 Additive

Overlaps with the Reparameterization but here several methods and more flexibility can be achieved.

- Soft prompts 

- Adapters *(overlap)*

Still the model weights are unchanged.

#### Prompt-tuning (soft prompt) 🧸

- Changes done in the weight of the embedding layer.

- Added some new **extra tokens** in he embedding layer.

- Generally `20-100` new tokens are added to achieve good performance, and these new tokens also have the same dimensions as the original vocabulary.

- These tokens **don't represent any real word from the corpus** but after training, we can take a particular token's embeddings and see the neighbor tokens which can indicate the similar meaning!

- The weights of the model are frozen ❄

- Like LoRA, we can change the soft-prompts for different tasks!!!
