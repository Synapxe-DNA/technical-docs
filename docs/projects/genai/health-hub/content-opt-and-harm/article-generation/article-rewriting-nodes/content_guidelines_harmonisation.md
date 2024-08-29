---
updated: 28 August 2024
authors:
  - Jin Chou
---

<center><h2><p>Content Guidelines Optimisation Node (Article Harmonisation)</p></h2></center>

!!! Warning

    This document is still under development and only consists of the documentation up to this point. It will require further updates in the future.

## 1. Introduction

The content guidelines optimisation node is used to rewrite the article keypoints to meet HealthHub's content guidelines and improve on the structure of the new article. Under article harmonisation, the content guidelines optimisation process varies between `live-healthy` and `diseases-and-conditions` articles.

The process of optimising the content of `diseases-and-conditions` articles is identical to that of content optimisation for article optimisation. On the other hand, `live-healthy` articles will be restructured based on the structure of the user annoted "main" article.

!!! Note

    The details here only apply to Article Harmonisation. Kindly refer to [Content Guidelines Optimisation Node (Article Optimisation)](/content_guidelines_optimisation.md#3-flow-of-content-guidelines-optimisation-node) for details on Content Guidelines Optimisation in Article Optimisation

## 2. Rationale for Content Guidelines Optimisation Node

This section will be adding on to the rationale for [content guidelines optimisation node](./content_guidelines_optimisation.md#3-flow-of-content-guidelines-optimisation-node).

While `live-healthy` articles under article optimisation does not require content optimisation, `live-healthy` articles under article harmonisation requires a structure template for the output article.

## 3. Flow of Content Guidelines Optimisation Node

Kindly refer to [here](./content_guidelines_optimisation.md#2-rationale-for-content-guidelines-optimisation-node) for the flow of Content Guidelines Optimisation Node in article optimisation

In the User Annotation Excel file, files in the same group can be marked for harmonisation. Users can further split these articles into different sub groups, wherein articles within the same subgroup will be merged to produce a single succinct article.

Within each subgroup, there will be one article designated by the user as the "main article". The main article will be labelled by the user in the User Annotation Excel file with a "Y" under the column `Main Article? (Y)`. Only `live-healthy` articles should require a main article structure.

Content guidelines optimisation starts by extracting the content of the main article. It will then begin a three step process:

!!! Warning

    This section is still under development and only consists of the documentation of completed parts up to this point. It will require further updates in the future.

## 4. Additional Resources for the Content Guidelines Optimisation Node

- @label(func) `return_content_prompt()` : This function returns the appropriate prompt for the content guidelines optimisation node. This function can be found at `article-harmonisation/agents/prompts.py`.

- @label(func) `optimise_content()` : This function defines the LangChain for optimising the article content and invokes it to obtain the final article content. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `content_guidelines_optimisation_node()` : This function runs `optimise_content()` and updates the graph state with the optimised article content output. This function can be found at `article-harmonisation/harmonisation.py`.

- [HealthHub Content Playbook](https://drive.google.com/drive/folders/1uF68nWXyc5wRwyDn0OahMYYiqTWTYM2d): This link will direct you to the Google Drive folder containing the Health Hub Content Playbook. The content playbook contains the guidelines used in the content optimisation prompt and they can be found under Chapter 3, Step 1, (iii) writing guide.
