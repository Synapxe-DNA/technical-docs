---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p> Title Optimisation Node</p></h2></center>

## 1. Introduction

The title optimisation node is used to create optimised article titles. Users can choose to flag an article for title optimisation, which will produce 3 optimised article titles. Based on HealthHub's title length requirements, each article title must be less than 71 characters (including spaces between words). The title will also be rewritten based on the HealthHub title guidelines.

Additionally, the User Annotation Excel sheet may contain some feedback for optimising the article title that will be addressed by the title optimisation node.

## 2. Rationale for Title Optimisation Node

Title optimisation was required to improve on original article titles as they do not meet HealthHub's title guidelines. Some article titles are not sufficiently descriptive about the article's content, which will not incentivize readers to click on the link. Some key areas that needed to be addressed includes adding a call to action to the titles and writing in an active voice.

This task was identified as a key task in the early process of planning out the structure of the LangGraph and was decoupled from the article rewriting process as it is a largely isolated task.

A feedback loop was added later in the project to ensure that all optimised titles meet the character length requirements. At the time of writing this documentation, the project uses ChatGPT 4.o mini as the primary LLM model. While this model is able to rewrite articles effectively, it yields poor accuracy when counting the number of characters in a given text.

Hence, it is challenging to instruct the LLM model to keep the optimised article title character length within a certain character limit while following other instructions. As such, a second prompt was required to provide clear instructions for the LLM to ensure that each optimised title meets the length requirements.

## 3. Flow of Title Optimisation Node

Title optimisation is a two step process, involving two unique prompts. The node starts by optimising the title based on the HealthHub's title guidelines and addressing any feedback exracted from the User Annotation Excel file. Following that, the node will ensure that each optimised title meets the character length requirements and rewrite titles exceeding the character length limit.

### Step 1: Optimising article title

In the first step, the title optimisation node takes in two inputs: the article content and any feedback for improving the original article title. The title optimisation node will then rewrite the article while following the HealthHub title guidelines and addressing the feedback from the evaluation node. The LLM agent's answer will include 3 optimised titles. An example is shown below:

```python
"""1. Safeguarding Singapore: Laws Against Infectious Diseases
2. How Singapore's Laws Combat Infectious Disease Threats
3. Empowering Public Health: Singapore's Infectious Disease Laws"""
```

Each line starts with it's corresponding number and an article title.

### Step 2: Shortening optimised titles

The second step instructs the LLM agent to check that the optimised titles have a character length of less than 65 characters, instead of the actual 70 characters limit. This adds an additional 6 characters as a buffer as ChatGPT models could still exceed the character limit even with clear instructions. Any optimised title exceeding the title length limit will be shortened at this step.

Finally, the actual length of each title is calculated after title optimisation through a rule-based function. If any title exceeds the character limit, the state graph will be redirected back for another round of title optimisation to produce shorter titles. In subsequent rewrites, all titles will be rewritten. If all optimised titles meet the length requirements, the graph state will be directed to other optimisation nodes.

!!! Note
  Further development can be done to improve the effeciveness of this feedback loop by adding additional fields for feedback on shortening the titles, as well as reducing the title length limit in the prompts with each subsequent rewrite. The rule-based function could also be configured to only rewrite lengthy titles while retaining shorter ones.

## 4. Sample of the Title Optimisation Node output

An example of the output from each step of title optimisation is shown below:

### Optimised titles after step 1

```python
"""1. Understanding Pneumonia: Causes, Symptoms, and Treatment Options  # 64 characters
2. Protect Yourself: Recognizing Pneumonia Symptoms and Prevention  # 63 characters
3. Navigate Pneumonia: Essential Insights on Causes and Care # 57 characters"""
```

### Optimised titles after step 2

```python
"""1. Understanding Pneumonia: Causes, Symptoms, and Treatments # 57 characters
2. Protect Yourself: Recognizing Pneumonia Symptoms and Prevention  # 63 characters
3. Navigate Pneumonia: Key Insights on Causes and Care # 51 characters"""
```

While none of the article titles from the first step exceeded the limit of 65 characters stated in the prompt, the first and third titles were still shortened in the second step.

In the first title, the word "Options" was removed in the revised title, bringing down the number of characters to 57. In the third title, the word "Essential" was replaced with "Key", hence shortening the title to just 51 characters.

In this example, it can be seen that ChatGPT models are not very accurate with counting the number of characters in a given text. Nonetheless, the feedback loop works as intended.

## 5. Additional Resources for the Title Optimisation Node

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `return_title_prompt()` : This function returns the appropriate prompt for the title optimisation node. This function can be found at `article-harmonisation/agents/prompts.py`.

- @label(func) `optimise_title()` : This function defines the LangChain for both title optimisation steps and combines them into a single chain. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `title_optimisation_node()` : This function runs `optimise_title()` and updates the graph state with the final optimised title output. This function can be found at `article-harmonisation/harmonisation.py`.

- @label(func) `check_optimised_titles_length()` : This function checks the character length of each optimised article and determines which node to send the graph state to. This function can be found at `article-harmonisation/harmonisation.py`.

- @label(func) `split_into_list()` : This is a rule-based function that takes the String output from the title optimisation as it's input. It will then return a list where each item is a title. This function can be found at `article-harmonisation/utils/formatters.py`.
