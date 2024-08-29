---
updated: 28 August 2024
authors:
  - Jin Chou
---

<center><h2><p> Meta Description Optimisation Node</p></h2></center>

## 1. Introduction

The meta description optimisation node is used to create optimised meta descriptions. Users can choose to flag an article for meta description optimisation, which will produce 3 optimised meta description. Based on HealthHub's meta description length requirements, each meta description must be more than 70 characters and less than 160 characters (including spaces between words). Since there are no specified guidelines for writing meta descriptions in HealthHub's content playbook, the instructions for writing optimised meta descriptions are mainly sourced online.

Additionally, the User Annotation Excel sheet may contain some feedback for optimising the meta description that will be addressed by the meta description optimisation node.

## 2. Rationale for Meta Description Optimisation Node

Similar to title optimisation, meta description optimisation was required to improve the original article's meta description as it's written poorly. Some meta descriptions are not sufficiently descriptive about the article's content, which will not incentivize readers to click on the link. Some key areas that needed to be addressed includes adding a call to action to the article as well as writing in an active voice.

A feedback loop was added later in the project to ensure that all optimised meta description meet the character length requirements. At the time of writing this documentation, the project uses ChatGPT 4.o mini as the primary LLM model. While this model is able to rewrite articles effectively, it yields poor accuracy when counting the number of characters in a given text.

Hence, it is challenging to instruct the LLM model to keep the optimised meta descriptions character length within a certain character limit while following other instructions. As such, a second prompt was required to provide clear instructions for the LLM to ensure that each optimised meta description meets the length requirements.

## 3. Flow of Meta Description Optimisation Node

Meta description optimisation is a two step process, involving two unique prompts. The node starts by optimising the meta description based on a set of custom guidelines and addressing any feedback exracted from the User Annotation Excel file. Following that, the node will ensure that each optimised meta description meets the character length requirements and rewrite meta descriptions exceeding the character length limit.

### Step 1: Optimising meta description

In the first step, the meta description optimisation node takes in two inputs: the article content and any feedback for improving the original article meta description. The meta description optimisation node will then rewrite the article meta descriptions while following the custom guidelines and addressing the feedback from the evaluation node. The LLM agent's answer will include 3 optimised meta descriptions. An example is shown below:

```python
"""1. Discover how Singapore's Infectious Diseases Act safeguards public health by enforcing vaccinations and controlling outbreaks.
2. Learn how the Infectious Diseases Act helps Singapore combat infectious diseases through proactive measures and mandatory vaccinations.
3. Explore the role of the Infectious Diseases Act in Singapore, ensuring safety through strict reporting, vaccinations, and outbreak management."""
```

Each line starts with it's corresponding number and an article meta description.

### Step 2: Shortening optimised meta descriptions

The second step instructs the LLM agent to check that the optimised meta descriptions have a character length between 100 and 130 characters, instead of the actual 70 - 160 characters range. This adds an additional 60 characters as a buffer as ChatGPT models could still exceed the character limit even with clear instructions. Any optimised meta description exceeding the meta description length limit will be shortened at this step.

Finally, the actual length of each meta description is calculated after meta description optimisation through a rule-based function. If any meta description exceeds the character limit, the state graph will be redirected back for another round of meta description optimisation to produce shorter meta descriptions. In subsequent rewrites, all meta descriptions will be rewritten. If all optimised meta descriptions meet the length requirements, the graph state will be directed to other optimisation nodes.

!!! Note

    Further development can be done to improve the effeciveness of this feedback loop by adding additional fields for feedback on shortening the meta descriptions, as well as reducing the meta description length limit in the prompts with each subsequent rewrite. The rule-based function could also be configured to only rewrite lengthy meta descriptions while retaining shorter ones.

## 4. Sample of the Meta Description Optimisation Node output

An example of the output from each step of meta description optimisation is shown below:

### Optimised meta descriptions after step 1

```python
"""1. Boost your mental well-being by developing emotional intelligence. Discover actionable tips to enhance your emotional awareness today! # 134 characters
2. Unlock the power of emotional intelligence! Learn effective strategies to manage your emotions and improve your daily life. #123 characters
3. Enhance your emotional intelligence with five practical steps. Transform your relationships and boost your mental health now! # 125 characters"""
```

### Optimised meta descriptions after step 2

```python
"""1. Boost your mental well-being by developing emotional intelligence. Discover tips to enhance your emotional awareness! # 117 characters
2. Unlock the power of emotional intelligence! Learn strategies to manage your emotions and improve your daily life. # 113 characters
3. Enhance your emotional intelligence with five practical steps. Transform relationships and boost your mental health! # 116 characters"""
```

In the optimised meta description output from step 1, the first meta description exceeded the character limit of 130 characters stated in the prompt. In the subsequent step of meta description optimisation, the first meta description was shortened to just 117 characters. The other 2 titles were shortened as well, even though they do not exceed the character limit.

!!! Note

    Further testing could be done with varying limits for number of characters to achieve a better balance between meta description quality and meta description lengths.

## 5. Additional Resources for the Meta Description Optimisation Node

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `return_meta_desc_prompt()` : This function returns the appropriate prompt for the meta description optimisation node. This function can be found at `article-harmonisation/agents/prompts.py`.

- @label(func) `optimise_meta_desc()` : This function defines the LangChain for both meta description optimisation steps and combines them into a single chain. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `meta_description_optimisation_node()` : This function runs `optimise_meta_desc()` and updates the graph state with the final optimised meta description output. This function can be found at `article-harmonisation/harmonisation.py`.

- @label(func) `check_optimised_meta_descs_length()` : This function checks the character length of each optimised meta description and determines which node to send the graph state to. This function can be found at `article-harmonisation/harmonisation.py`.

- @label(func) `split_into_list()` : This is a rule-based function that takes the String output from the meta description optimisation as it's input. It will then return a list where each item is a meta description. This function can be found at `article-harmonisation/utils/formatters.py`.
