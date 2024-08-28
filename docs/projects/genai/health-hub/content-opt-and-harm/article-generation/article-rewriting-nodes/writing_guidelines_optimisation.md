---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p>Writing Guidelines Optimisation Node</p></h2></center>

## 1. Introduction

The writing guidelines optimisation node is used to rewrite the given article content in Health Hub's voice and personality guidelines. The voice and personality guidelines can be found in Health Hub's content playbook.

The quality of the optimised writing is measured using the Hemingway readability score, wherein the output article should have a readability score less than 10.

## 2. Rationale for Writing Guidelines Optimisation Node

Initially, article rewriting was planned to be a process handled by a single LLM agent. However, this approach did not accomodate for use cases where users may wish to only improve on the writing of an article while maintaining it's structure. Hence, the article rewriting process was split into two individual parts: **content guidelines optimisation** and **writing guidelines optimisation**.

While the content guidelines optimisation node is primarily tasked with restructuring of the article content, the writing guidelines optimisation node is tasked with doing a complete rewrite of the article content to match HealthHub's voice and personality guide.

## 3. Flow of Writing Guidelines Optimisation Node

The writing guidelines optimisation node is a single step process where it will rewrite the article content based on the HealthHub's voice and personality guidelines.

Below is a abbreviated sample of the guidelines in the prompt:

```python
"""1. Welcome your readers warmly, understand their needs, and accommodate them. You should also account for diverse needs and health conditions if applicable.
Example: “Living with diabetes doesn't mean you can’t travel. With proper planning, you can still make travel plans safely.”

2. Ensure your writing is relevant to the visitor's needs and expectations.
Example: “Worried about new COVID-19 variants? Hear from our experts on infectious diseases and learn how you can stay safe!”

3. Personalize the experience for visitors with relevant content.
Example: “Are you a new mum returning to work soon? Here are some tips to help you maintain your milk supply while you work from the office.”"""
```

A clear instruction along with an example is given for each guideline for the LLM agent to emulate.

After rewriting the article content in HealthHub's voice and personality guidelines, the Hemingway readability score of the new rewritten article will be calculated and added to the graph state. If the readability score is less than 10, the graph state will move on to subsequent optimisation steps or conclude the article optimisation flow if no other optimisation steps are required. Otherwise, the graph state will be directed to the readability optimisation node.

**Do note that the limit on the maximum number of article readability rewrites is set to 3.**

## 4. Additional Resources for the Writing Guidelines Optimisation Node

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `return_writing_prompt()` : This function returns the writing prompt for the writing guidelines optimisation node. This function can found at `article-harmonisation/agents/prompts.py`.

- @label(func) `optimise_writing()` : This function defines the LangChain for optimising the article writing and invokes it to obtain the final optimised article writing. This function can found at `article-harmonisation/agents/models.py`.

- @label(func) `writing_guidelines_optimisation_node()` : This function runs `optimise_writing()` and updates the graph state with the optimised article content output. This function can found at `article-harmonisation/harmonisation.py`.

- @label(func) `calculate_readability()`: This is a rule-based function calculates the Hemingway readability score of a given text and returns the score as an integer. This function can be found at `article-harmonisation/utils/evaluations.py`

- @label(func) `check_readability_after_writing_optimisation`: This function is used to check the Hemingway readability score of the rewritten text. It will then decide which node to send the graph state to through some rule-based processes. This function can be found at `article-harmonisation/harmonisation.py`.

- [HealthHub Content Playbook](https://drive.google.com/drive/folders/1uF68nWXyc5wRwyDn0OahMYYiqTWTYM2d): This link will direct you to the Google Drive folder containing the Health Hub Content Playbook. The content playbook contains the guidelines used in the writing optimisation prompt and the writing style guides can be found in Section A from pages 5 - 11.
