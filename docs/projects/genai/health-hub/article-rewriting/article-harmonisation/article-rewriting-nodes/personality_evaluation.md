---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p> Personality Evaluation Node</p></h2></center>

## 1. Introduction
The personality evaluation node is used to determine if an article's content meets the HealthHub's writing guidelines. This node is used to evaluate if a readability optimised writing still carries HealthHub's voice and personality after a series of rewriting.

## 2. Rationale for Personality Evaluation Node
To avoid overcomplicating the prompts, the HealthHub writing guidelines were excluded from the readability optimisation nodes. Hence, the readability optimisation prompts only includes instructions for improving the readability of the article writing. 

As a result, the article writing may no longer effectively capture HealthHub's voice after readability optimisation. An additional step was required to evaluate the writing and determine if the writing needs to undergo a subsequent round of writing guidelines optimisation to better capture HealthHub's writing guidelines. 

## 3. Flow of Personality Evaluation Node
The personality evaluation node is a single step process where the LLM agent is instructed to determine if the article writing still adheres to HealthHub's guidelines and return a corresponding boolean value. If HealthHub's writing guidelines is still captured, the LLM agent will return True. Otherwise the LLM agent will return False.

The answer from the LLM agent will be stored in the graph's state and will be subsequently used to determine if the article writing needs to undergo another round of writing guidelines optimisation.

## 4. Additional Resources for the Personality Evaluation Node
Kindly refer to these sections for further clarification and details on the code:

- @label(func) ```return_personality_evaluation_prompt()``` : This function returns the personality evaluation prompt for the personality evaluation node. This function can found at `article-harmonisation/agents/prompts.py`.

-  @label(func) ```evaluate_personality()``` : This function defines the LangChain for evaluating the article's personality and invokes it to obtain the outcome of the evaluation. This function can found at `article-harmonisation/agents/models.py`.

-  @label(func) ```personality_guidelines_evaluation_node()``` : This function runs `evaluate_personality()` and updates the graph state with the evaluation outcome. This function can found at `article-harmonisation/harmonisation.py`.

-  @label(func) ```check_personality_after_personality_evaluation()```: This function is used to check if the readability optimised article still meets the HealthHub's writing guidelines. It will then determine which node to direct the state to based on the evaluation outcome. This function can found at `article-harmonisation/harmonisation.py`.

