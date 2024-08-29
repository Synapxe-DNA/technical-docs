---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p>Compiler Node</p></h2></center>

## 1. Introduction

The main task of the compiler node is to combine the processed article keypoints from the researcher node. This node is part of the article harmonisation process and compiles the processed keypoints of multiple articles.

## 2. Rationale for Compiler Node

Since article harmonisation involves combining several largely similar articles to form a single optimised article, there will be cases where content from different articles will overlap. As such, an additional step has to be included to remove duplicate information. Having an additional LLM agent also overcomes the short context lengths of open source models used earlier in the project. Hence, the compiler node was designed to serve as a subsequent step that processes these article keypoints and ensures that there is not duplicate information/keypoints when harmonising the article content.

While the current ChatGPT 4.o mini has a sufficiently long context length, the compiler node still serves as an additional step to combine duplicate information. This helps to split up article content processing between multiple LLM agents, which reduces the number of key instructions in the prompts which directly improve their effectiveness.

## 3. Flow of Compiler Node

The compiler node is a single step progress, where it takes processed keypoints from multiple articles as an input. it starts by extracting all researcher keypoints from the graph state and concatenating them into a single String. Clear headers and endings are indicated at the start and end of an article's keypoints. This clearly indicates the start and end of each article's keypoints for the LLM agent.

Once the keypoints of all articles have been concatenated into a single String, the LLM agent will take the String of compiled keypoints as it's input and starts the compilation process. If any 2 keypoints contained duplicate information, the LLM agent will ensure that no content will be duplicated. After processing through these keypoints, the compiler node will return a single String containing the processed output and update it in the graph state.

## 4. Additional Resources for the Compiler Node

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `return_compiler_prompt()` : This function returns the appropriate prompt for the compiler node. This function can be found at `article-harmonisation/agents/prompts.py`.

- @label(func) `compile_points()` : This function defines the LangChain for processing the article keypoints and invokes it to obtain a processed compilation of article keypoints. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `compiler_node()` : This function runs `compile_points()` and updates the graph state with the final compiled output. This function can be found at `article-harmonisation/harmonisation.py`.

- @label(func) `decide_next_optimisation_node()` : This function checks for the user flags after content guidelines optimisation and determines which optimisation node to direct the state to next. This function can be found at `article-harmonisation/harmonisation.py`.
