---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p>Content Guidelines Optimisation Node (Article Optimisation)</p></h2></center>

## 1. Introduction

The content guidelines optimisation node is used to rewrite and restructure the given article keypoints to fit the criterias of the guidelines given in the Health Hub (HH) content playbook. These guidelines include a certain structure that the article should follow, as well as certain personality writing guides.

!!! Note
  The details here only apply to Article Optimisation. Kindly refer to [Content Guidelines Optimisation Node (Article Harmonisation)](/content_guidelines_harmonisation.md) for details on Content Guidelines Optimisation in Article Harmonisation

## 2. Rationale for Content Guidelines Optimisation Node

From the Health Hub content playbook, articles are expected to carry a certain voice and personality that emulates the writing style of Health Hub. Some voice guidelines include carrying a positive tone and be offer reassurance to the readers. Apart from rewriting the content in HealthHub's style, the content guidelines also restructures the given keypoints such that it follows a stipulated article structure. This article structure can be found in the Figma board "HealtHub Main UI prototype".

Do note that in the context of Article Optimisation, only articles under the `diseases-and-conditions` category can be flagged for content guidelines optimisation. `live-healthy` articles do not undergo content guidelines optimisation due to the following reasons:

1. There are many sub categories under `live-healthy` articles, hence each category will require a separate prompt which will require extensive testing. Apart from that, some categories do not have a sample structure on the HealthHub Figma board.

2. `live-healthy` articles are encouraged to carry a more individualistic structure to avoid identical structure between different articles. `live-healthy` articles are meant to pique interest the readers and should avoid a set structure.

## 3. Flow of Content Guidelines Optimisation Node

The content guidelines optimisation node for Article Optimisation is a single step process.

The LLM agent is tasked with two objectives: To use the given keypoints to fill in the sections and rewrite the content based on the given guidelines.

The final output should consist of an article following this structure.

1. Overview of the condition
2. Causes and Risk Factors
3. Symptoms and Signs
4. Complications
5. Treatment and Prevention
6. When to see a doctor

After the article content undergoes content optimisation, the graph state will be updated with the optimised content.

!!! Note

    This task will be broken up into multiple steps in future developments to better capture key information in the researcher keypoints.

## 4. Additional Resources for the Content Guidelines Optimisation Node

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `return_content_prompt()` : This function returns the appropriate prompt for the content guidelines optimisation node. This function can be found at `article-harmonisation/agents/prompts.py`.

- @label(func) `optimise_content()` : This function defines the LangChain for optimising the article content and invokes it to obtain the final article content. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `content_guidelines_optimisation_node()` : This function runs `optimise_content()` and updates the graph state with the optimised article content output. This function can be found at `article-harmonisation/harmonisation.py`.

- [HealthHub Content Playbook](https://drive.google.com/drive/folders/1uF68nWXyc5wRwyDn0OahMYYiqTWTYM2d): This link will direct you to the Google Drive folder containing the Health Hub Content Playbook. The content playbook contains the guidelines used in the content optimisation prompt and they can be found under Chapter 3, Step 1, (iii) writing guide.
