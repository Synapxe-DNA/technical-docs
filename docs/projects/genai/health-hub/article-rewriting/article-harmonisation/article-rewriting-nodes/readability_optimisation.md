---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p>Readability Optimisation Node</p></h2></center>

## 1. Introduction

The readability optimisation node utilizes multiple prompts to improve the readability of the writing guidelines optimised article. Each prompt instructs the LLM agent to identify and rectify specifc areas for improvement.

The article's readability is measured using the Hemingway Readability scoring system and the purpose of the readability optimisation node is to produce a final article writing that has a Hemingway score of less than 10.

## 2. Rationale for Readability Optimisation Node
The quality of an article can be subjective between different readers. Hence, this project utilizes the Hemingway scoring system to provide an objective evaluation for the rewritten article. 

## 3. Flow of Readability Optimisation Node
There are 4 steps in the readability optimisation node, each targetting a specifc area of the article to improve. Each step is represented by a unique prompt.

### Step 1: Cutting out redundant writing 
The first prompt instructs the LLM to identify and cut out any redundant writing in the article body. This step helps to reduce the word count of the article, which can improve the Hemingway readability score as it is dependent on the number of words and characters in the content.

### Step 2: Shortening lengthy sentences 
The next prompt instructs the LLM to identify and shorten any run-on sentences. The LLM agent will aim to replace these long sentences with multiple, shorter sentences or to rephrase the sentence using similar synonyms. 

Since the Hemingway readability score penalizes long sentences, run-on sentences contribute negatively to the Hemingway readability score. Shortening these sentences directly improves the Hemingway readability score.

### Step 3: Breaking list-like sentences into bullet points 
This prompt instructs the LLM to identify lengthy, list-like sentences and break them up into bullet points. List-like sentences refer to sentences listing out multiple items. For example, *"The best ways to relax are to watch shows, play games, enjoy good food, go for a hike and hang out with friends"* is a list-like sentence. 

List-like sentences are usually very long, which leads to a poorer Hemingway Readability score. Breaking these sentences up into bullet points where each item is an individual bullet point overcomes this issue and greatly improves the Hemingway score.

### Step 4: Simplifying complex words
Apart from penalizing long sentences, the Hemingway Scoring system also penalizes the use of complex words. Complex words includes medical jargon or any uncommon terms. This prompt instructs the LLM to identify such terms and replace them with simpler phrases or common words to convey the same meaning.

After completing all the readability optimisation steps, a new readability score will be calculated for the new readability optimised article and updated in the state.

!!!Note
    Do note that the number of rewriting tries increases by 1 whenever the article content undergoes readability optimisation. The maximum number of tries for optimising the readability of the article is set to be 3. If the number of rewriting tries exceeds the limit, the graph state will not undergo readability optimisation and instead move on to the next optimisation node.

## 4. Additional Resources for the Readability Optimisation Node
Kindly refer to these sections for further clarification and details on the code:

- @label(func) ```return_hemingway_readability_optimisation_prompt()``` : This function returns the appropriate readability optimisation prompt for the readability optimisation node. This function can found at `article-harmonisation/agents/prompts.py`.

-  @label(func) ```optimise_readability()``` : This function defines the LangChain for optimising the article's readability and invokes it to obtain the final readability optimised article. This function can found at `article-harmonisation/agents/models.py`.

-  @label(func) ```readability_optimisation_node()``` : This function runs `optimise_readability()` and updates the graph state with the readability optimised article. This function can found at `article-harmonisation/harmonisation.py`.

-  @label(func) ```check_personality_after_readability_optimisation()```: This function is used to check if the number of rewriting tries made for article optimisation has surpassed the limit and if the new readability optimised article is of acceptable readability. If either conditions returns True, this function will direct the graph state to the personality evaluation node. Otherwise, if the article has a readability score more than or equals to 10 and the number of rewriting tries has not exceeds the limit, this function will direct the graph state back to the readability optimisation node for another round of readabilty optimisation.

-  @label(func) ```calculate_readability()```: This is a rule-based function calculates the Hemingway readability score of a given text and returns the score as an integer. This function can be found at `article-harmonisation/utils/evaluations.py`