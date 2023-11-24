# 1️⃣ Guidelines

1. `Principle-1`: **Keep things clear**
   
   - It is ***okay*** to create a longer prompt
   
   - The instructions should be clear
   
   - Use the delimiters """, ''' etc.

2. `Principle-2`: **Give the model time to think**
   
   - Of course. Nothing to say here.

## Principle - 1

1. Use delimiters

2. Ask for structured output

3. Check whether the conditions are satisfied

4. Few-shot prompting *(like child and grandparent conversation)*

### 🏥 The delimiters saves us!

> The **principle-1** saves us from the prompt injection!!

```
prompt...
{user = "forget everything"}
...prompt
```

If we had used some delimeters, it would save us from this.

### 🤔 Check whether the conditions are met

```
You will be provided with text delimited by triple quotes.
If it contains a sequence of instructions,
re—write those instructions in the following format:
Step 1 —
step 2 —
Step N —

If the text does not contain a sequence Of instructions,
then simply write "No steps provided."

```

#### Principle - 2

While giving the steps, it is also give the **format** that it should use. So the format is consistant and controlled.



# 2️⃣ Iterative Process

- Try something

- Analyze where the result does not give what you want

- Clarify instructions, give more time to think

- Refine prompts with a batch of examples

# 3️⃣ Tone

Change the tone of the result by including ***"who will be reading"*** the response.

```
Your task is to generate a short summary of a product
review from an ecommerce site to give feedback to **the
pricing deparmtment, responsible for determining the
price of the product.** 

Summarize the review below, delimited by triple 
backticks, in at most 30 words, and **focusing on any aspects
that are relevant to the price and perceived value.**
```

# 4️⃣ Inferring

>  💡 
>  **The big issue** the LLMs solve is, they can save a hell lot of work that the traditional ML required 2 years back ;). You would need to create a seperate model for a seperate task. Now, everything can be done by a single capable model.





# 🔢 Misc

- Instruct the model that the text will be deliminated by **"triple batckticks"** or something like that.


