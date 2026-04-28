## Understanding the Data

How many conversations are in the dataset? : 3300
How many columns? 13
What do the columns mean?

1. question ID : Unique id of the question either asked to the user or what the user has asked the LLM, this could also be the question they are using to judge the LLMs answers
2. model_a : The LLM used by the user, they are comparing it with model b
3. model_b : Second model they are comparing the answers to
4. winner : Who was awarded winner by the user
5. judge : The username of the person who judged the model
6. conversation_a : How the 1st conversation a was going
7. conversation_b : How the 2nd conversation a was going
8. turn : What is the question number for that user?
9. anony : was it anonymous?
10. language : language used
11. tstamp : Timestamp
12. openai_moderation : Is there a moderation layer applied by the LLM?
13. toxic_chat_tag : is the chat tagged/flagged as toxic?

## Research Question

What linguistic patterns characterize human-preferred AI responses across different subject domains and what do these patterns imply about the type of training data and precision needed per domain?

## Smaller Questions

Data Representation we need :
Get the data on the majority and then do analysis.

# Set 1 — Competition overview : Who won and by how much?

# Set 2 — Winning responses : Models with win rate >50%, their actual response text and prompts

# Set 3 — Losing responses : Models with win rate <50%, their response text and prompts — contrast set

# Set 4 — Tie responses : Rows where winner is tie or tie (both bad) — these are actually fascinating because they tell you when humans genuinely couldn't differentiate, or when both were bad

# Set 1 : Which model has been chosen as the winner?

winner
model_a 11744
model_b 11550
tie (bothbad) 6263
tie 3443

- So the model_a and model_b could be any of these
  model_a :['chatglm-6b' 'oasst-pythia-12b' 'koala-13b' 'vicuna-13b'
  'stablelm-tuned-alpha-7b' 'alpaca-13b' 'llama-13b' 'dolly-v2-12b'
  'fastchat-t5-3b' 'gpt-3.5-turbo' 'gpt-4' 'claude-v1' 'RWKV-4-Raven-14B'
  'mpt-7b-chat' 'palm-2' 'claude-instant-v1' 'vicuna-7b' 'wizardlm-13b'
  'gpt4all-13b-snoozy' 'guanaco-33b']

model_b: ['koala-13b' 'alpaca-13b' 'oasst-pythia-12b' 'dolly-v2-12b' 'vicuna-13b'
'llama-13b' 'stablelm-tuned-alpha-7b' 'chatglm-6b' 'fastchat-t5-3b'
'gpt-3.5-turbo' 'gpt-4' 'RWKV-4-Raven-14B' 'claude-v1' 'mpt-7b-chat'
'palm-2' 'claude-instant-v1' 'vicuna-7b' 'wizardlm-13b'
'gpt4all-13b-snoozy' 'guanaco-33b']

Majority of winners :
gpt-4 2888
vicuna-13b 2667
gpt-3.5-turbo 2524
claude-v1 2390
koala-13b 1901
claude-instant-v1 1493
oasst-pythia-12b 1150
palm-2 1125
alpaca-13b 1118
vicuna-7b 883
RWKV-4-Raven-14B 878
mpt-7b-chat 671
chatglm-6b 649
fastchat-t5-3b 611
stablelm-tuned-alpha-7b 485
dolly-v2-12b 443
guanaco-33b 436
wizardlm-13b 426
llama-13b 304
gpt4all-13b-snoozy 252

Different versions of the same product have different choices.
What does that say about someone choosing it? Because, even if they choose it the model version will always change and how can we know what to keep even if it's chosen?

The frequencies differ by a range of 4897.
The standard deviation of frequencies is 1396.80.

## Key Observation 1 : Exposure Bias

- Models are not equally presented in the dataset
- Raw win counts might not tell us about which model fairs better
  All win analysis must use win rate (wins/total appearance)
- unbalanced evaluation exposure can misrepresent model quality.

# Calculate win rate and get the list of all models

gpt-4 0.684847
claude-v1 0.608607
claude-instant-v1 0.568545
gpt-3.5-turbo 0.542329
vicuna-13b 0.449671
guanaco-33b 0.421663
wizardlm-13b 0.381720
palm-2 0.380711
koala-13b 0.341109
vicuna-7b 0.307773
alpaca-13b 0.251067
RWKV-4-Raven-14B 0.238457
oasst-pythia-12b 0.235174
mpt-7b-chat 0.235109
gpt4all-13b-snoozy 0.229717
chatglm-6b 0.195364
fastchat-t5-3b 0.190343
stablelm-tuned-alpha-7b 0.173524
dolly-v2-12b 0.159009
llama-13b 0.151319
Name: count, dtype: float64

## Key Finding — Win Rates by Model
After normalizing for exposure, GPT-4 leads with a 68% win rate, followed
by Claude-v1 (61%) and Claude-instant (57%). Smaller open source models
(LLaMA-13b, Dolly, StableLM) cluster below 20%. This suggests model size
and training quality significantly impact human preference.

# Visualisation
1. Model names + frequency
2. How often they are shown
3. How often they win?
4. Win rate
5. Which models fall under and over 50%? What are the names of those models?

Done, saved ; Set 1 done.

# Set 2 : Get all the responses from models with a win rate of 50%<

- From the models with, 50% win rate, get all the conversation in 2 separate columns : prompt and response.
- What was the topic about?

## Set 2  — Winning Model Responses

- SET 2 : Win Rate > 50%
- SET 2.1 : 50% > Win Rate > 25%
- SET 2.2 : 25% > Win Rate > 0%
- 8101 English conversations extracted
- Each row contains the prompt, winning response, model name and win rate
- Filtered to English only to ensure linguistic analysis is valid

Logic : 
- Mask english 

get rows : 
- for rows in top_models 
rows = []
mask = english_mask & (
  ((df["tie"] == "model_a") & (df["model_a"] == model))
  ((df["tie"] == "model_b") & (df["model_b"] == model))
)

for


## Set 3  — Tie Responses 

One file for all models. Model a & Model b 


## Set 2.a : Domain Distribution Insight
The dataset is heavily skewed toward General Knowledge and Programming/Coding prompts. This likely reflects the demographics of Chatbot Arena users — developers and tech-savvy individuals. This skew has implications for what "human preference" actually means — it may reflect developer preference more than general population preference.




