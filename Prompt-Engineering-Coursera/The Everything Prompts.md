# 👋 Thinking out loud
This is going to be **very short** course on the prompt engineering. This is just the ***single*** file that contains everything.

This coursera course is just so "professional" that they have made it like 8 hours which would just take 30 mins to learn.

But as I have started this course, I will complete it. So let's get started and finished fast 🔥

# 🚓 The misc patterns

> [!NOTE]
> The number of fire (🔥) will determine the "wowness" of this new pattern that I am going to introduce here. 

### `1.` The persona pattern (🔥)
This is just like saying the model **how to act**:
```
Act as a python teacher...
```
This is easy. Just skip it.


### `2.` Question refinement pattern (🔥🔥)
> *If we present a more general question and then ask the AI to refine it, it can leverage its knowledge of related topics to improve the question. By considering associated words and context, it can make the question more specific or provide additional relevant information.*

```
🔴 PROMPT

"Whenever I ask a question, suggest a better question and ask me if I would like to use it instead."
```
That seems to be cool! 

### `3.` Cognative verifier pattern (🔥)
This is where the **model has to reason** through his approach. Or call it "think step by step".

Here we also tel the model to "follow the given rules". This is also **considered as the CoT**.

> [!INFO]
> *In simple terms, cognitive verification refers to the process of confirming or validating our thoughts and beliefs by seeking consistency with what others around us think or believe.*

💡This is **without specifiying the exact questions or steps** making the model to perform them!

```
🔴 PROMPT

When you are asked a question, follow these rules:
- Generate a number of additional questions that would help more
accurately answer the question.

- Combine the answers to the individual questions to produce the final
answer to the overall question.
```

The **last step is important**. There we are telling the model **to combine the answers** and then give it final answer. So, that needed to be done there.


### `4.` Ask until it gets enough information

```
 🔴 PROMPT

Ask me questions about fitness goals **until you have enough information** to suggest a
strength training regime for me. 

When you have enough information, show me the strength training regime.

Ask me the first question.
```

### `5.` The intermediate steps (🔥🔥🔥)
This is the a form of CoT but will work amazingly. It is the **thought, action** thing 😉

```
 🔴PROMPT

### Example - 1
Situation: I am traveling 60 miles per hour and I see the brake lights on the car in front of me 
Think: I need to slow the car down before I hit the car in front of me
Action: Press foot on brake
Think: The car isn't going to stop in time
Action: Check if the shoulder is wide enough to swerve into
Think: The shoulder is wide enough
Action: Swerve into shoulder

### Example - 2
Situation: I have just entered the highway from an on-ramp and am traveling 30mph
Think: I need to speed up to the speed limit so that I don't get hit from behind
Action: Press foot on accelerator
Think: I have reached the speed limit
Action: Let up on accelerator

### Work on it.
Situation: I am backing out of a parking spot and I see the reverse lights illuminate on the
car behind me
Action:
```

# **Main Ideas:** Chain Of Thought (from paper)
> *"Few shots isn't enough, with few shots you need to provide some steps to learn from"*

```
Q: Roger has 5 tennis balls. He buys 2 more cans of 
tennis balls. Each can has 3 tennis balls. How many 
tennis balls does he have now? 
A: The answer is 11. 

Q: The cafeteria had 23 apples. If they used 20 to 
make lunch and bought 6 more, how many apples 
do they have?

Model Output
A: The answer is 27 ❌
```

```
Q: Roger has 5 tennis balls. He buys 2 more cans of 
tennis balls. Each can has 3 tennis balls. How many 
tennis balls does he have now? 
A: Roger started with 5 balls. 2 cans of 3 tennis balls 
each is 6 tennis balls. 5 + 6 = 11. The answer is 11. 
Q: The cafeteria had 23 apples. If they used 20 to 
make lunch and bought 6 more, how many apples 
do they have?

Model Output
A: The cafeteria had 23 apples originally. They used 
20 to make lunch. So they had 23 - 20 = 3. They 
bought 6 more apples, so they have 3 + 6 = 9. The 
answer is 9 ✅
```

**It is also possible to give the "options" with the reasoning**:
```
Q: How many keystrokes are needed 
to type the numbers from 1 to 500?

Answer Choices: (a) 1156 (b) 1392 (c) 1480 
(d) 1562 (e) 1788 

A: There are 9 one-digit numbers 
from 1 to 9. There are 90 two-digit 
numbers from 10 to 99. There are 
401 three-digit numbers from 100 to 
500. 9 + 90(2) + 401(3) = 1392. The 
answer is (b).
```

**A good way to inject anything in prompt** (🔥🔥🔥🔥🔥)
```
Q: Take the last letters of the words 
in “Lady Gaga” and concatenate 
them. 

A: The last letter of “Lady” is “y”. The 
last letter of “Gaga” is “a”. 
Concatenating them is “ya”. So the 
answer is ya.
```

# ReAct (🔥🔥🔥🔥🔥)

This is the `Think, Search, Action` methodology!

- This is used incrementally.
- First we provide the example so that the model knows which steps it has to perform.
- Then giving the real life example.
- In the intermediate steps **it uses tools**.

Example:
```
## Example 🟡
Task: Calculate when I need to arrive at the Music City BMX National Race for my son to be
on time for his 9-10 Open race.

Think: I need to find out what time the first race begins. I can use a web search of the Music
City BMX site to get the information.

Action: SEARCH htps://www.musiccitybmx.com

Result: All races start at gam.

Think: I need to find out how many motos run before the 9-10 Open race. I can use a web search of the Music City BMX site to get the information.

Action: SEARCH

Result: There are 30 rnotosthat run re the 9-10 Open race.

Think: I need to find out how long each moto took last year. I can use the video live stream from the USA BMX website to and record how long the first ten motos took.

Action: VI DEO https://www.usabmx.com/live/2022/musiccitybmx/live.mp4

Result:  The first 10 motos took 5mins

Think: If 10 motos took 5mins, 30 motos will take 15mins. The race starts at gam, so I need to arrive before 9:15am when the race starts- I will arrive 30mins early at 8:45am to be safe.


## New Task 🟢
Task: Calculate what time my 36-40 novice race will end at the USA BMX Grand Nationals.

Think: 
```

**Hot line**:

> ### 🧨
> **We can have the system** whenever **it sees a result**, **stop pulling the actual result**, **put it in there**, and then basically **feed this first, sec chunk back to the language model** to then **continue the process**. We don't actually have to worry about if it's going to fake the rest of this, we could just have it go and we can **incrementally get to every, each time we get a new result out that we actually need to fill in**, we could go and **run the web search and fill in that result**.

# Check the model output consistency!

After times, the model keeps updating. So, to keep checking it's output consistency, one of the ways is "to grade the passage".

1. Give say, 5 to 6 different passages and make the model to grade them out of 10.
2. Store that info in the PC.
3. After 3 months, is the model is updated, then again check these examples and if the grading is dropped **below** some threshold, then that means that the model is changed.