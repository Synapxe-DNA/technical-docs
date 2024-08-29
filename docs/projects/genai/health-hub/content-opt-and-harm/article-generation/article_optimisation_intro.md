<h2><p style="text-align: center;">Article Optimisation Workflow</p></h2>

![Optimisation Checks & Rewriting Workflow](../images/article_optimisation_flow.jpg)

<p style="text-align: center; font-size: 10px;"><i>Article Optimisation Workflow diagram</i></p>

**Article optimisation** refers to the process of optimising individual articles.

The first step of article optimisation would be to evaluate the quality of the article based on its content, title and meta description. The article will be evaluated using Rules, Statistics and Large Language Models (LLMs). The output will then be stored on the User Annotation Excel File.

Users will then analyze the evaluation outputs on the User Annotation Excel File and flag out areas to be optimised. Users can choose to flag these areas for optimisation: title, meta description and content.

After user annotation, the article will undergo article optimisation. The flow of article optimisation is as follows:

1. [**Researcher node**](./article-rewriting-nodes/researcher_node.md): This node is used to process the article content. It checks for irrelevant sentences in the article content and categorises them under a separate section. <br/>

2. [**Content guidelines optimisation**](./article-rewriting-nodes/content_guidelines_optimisation.md): Content guidelines optimisation is carried out when the user flags the article for content optimisation. Under article optimisation, only diseases-and-conditions articles will undergo content optimisation.
   <br/><br/>
   This process will strucure the article content into a specified structure and rewrite the content to fit the guidelines from the HealthHub content playbook.

3. **Writing optimisation**: Writing optimisation is carried out when the user flags the article for writing optimisation. The writing optimisation process aims to rewrite the article content to fit HealthHub's personality guidelines as well as improving the article's Hemingway readability score until it is less than 10.
   <br/><br/>
   Writing guidelines is a multi-node process, consisting of a feedback loop with the following nodes:

   1. [Writing guidelines optimisation](./article-rewriting-nodes/writing_guidelines_optimisation.md): This node is used to rewrite the article content to meet the voice and personality guidelines of HealthHub's voice and personality guidelines.
   2. [Readability optimisaion node](./article-rewriting-nodes/readability_optimisation.md): This node is used to rewrite the article content to improve the article's readability score. The article's readability is scored using the Hemingway scoring system.
   3. [Personality evaluation node](./article-rewriting-nodes/personality_evaluation.md): This node is used to evaluate if a readability optimised article still adheres to the HealthHub voice and personality guidelines. This evaluation will be used to determine if the article content requires another round of rewriting or to move on to subsequent optimisation nodes.

4. [**Title optimisation**](./article-rewriting-nodes/title_optimisation.md): The title optimisation process involves producing three optimised article titles from the article content. The titles will be optimised based on the HealthHub content playbook guidelines.

5. [**Meta description optimisation**](./article-rewriting-nodes/meta_desc_optimisation.md): The meta description optimisation process involves producing three optimised meta description from the article content.

After the article optimisation process has been completed, the optimised output will be stored in the User Annotation Excel file for users to review.
