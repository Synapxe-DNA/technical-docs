---
updated: 27 August 2024
authors:
  - Jin Chou
---

<center><h2><p>Researcher Node</p></h2></center>

## 1. Introduction

The main task of the researcher node is to process article content and sieve out unnecessary content in the article. The researcher node also adds any additional content from the User Annotation Excel file to the article.

## 2. Rationale for Researcher Node

The rationale for developing a researcher node was mainly to overcome the token limits. It also improves the effectiveness of the Large Language Model (LLM) by splitting up the two tasks at the "research" step: processing article content and adding additional content.

## 3. Flow of Researcher Node

### Step 1: Processing the article content

A series of preprocessing steps are carried out before processing the article content through the researcher node. An abbreviated sample of an article input is shown below:
<br/>

```python
"""h3 Sub Header: Tremor in hands, arms, legs, jaw, or head
Sub Section to h2 Sub Header: Symptoms of Parkinson's disease
Content: Patients of PD may suffer from tremors in their limbs that may worsen over time. For example, people may feel mild tremors or have difficulty getting out of a chair. They may also notice that they speak too softly, or that their handwriting is slow and looks cramped or small. """
```

Kindly refer to [article content processing](../article_content_preprocessing.md) for further explanation on the preprocessing steps for the article content.

Utilizing the details of the headers and their relations to other headers, the researcher node will categorise them as a `main keypoint` or `sub keypoint`. If a header has been indicated to be the sub section/sub header of another header, it will be referred to as a `sub keypoint`. Otherwise, it will be referred to as a `main keypoint`. This process helps to retain the flow of a list-like structure in an article.

The researcher node will also sieve out any unnecessary sentences and add it in a separate section at the bottom of it's answer. Unnecessary sentences includes citations, references, links to other pages and irrelevant sentences to the current context. These sentences will be added under an additional section at the bottom of the answer titled `omitted sentences`.

### Step 2: Adding additional content, if any

Following the processing step above, the researcher node will add any additional content to the article content. The additional content refers to additional content added by the user in the User Annotation Excel file. The additional content can be found under the `User: additional content to add for harmonisation` column in the Excel file.

The additional content will be added as an additional input, along with the processed article content. The additional content will be added as a `main keypoint` to the processed article content. The researcher node is also tasked with writing an appropriate header for the additional content.

No additional content will be added if the corresponding cell in the `User: additional content to add for harmonisation` column is empty.

## 4. Sample of the Researcher Node output

A sample of the final output from the researcher node can be seen below. In this example, there is additional content to be added, that can be seen in the final output.

**Additional content to be added:**

```python
"""Joining a Parkinson's support group can provide emotional support and valuable insights from others who are experiencing similar challenges. These groups offer a sense of community and shared understanding."""
```

**Final output from researcher node:**

```python
"""Main keypoint: Introduction to Parkinson's disease
Content: Parkinson's is a neurodegenerative disease. It is a progressive disorder that affects the nervous system and other parts of the body. There are approximately 90,000 new patients diagnosed with PD annually in the US.

Main keypoint: Symptoms of Parkinson's disease

Sub keypoint: Tremor in hands, arms, legs, jaw, or head
Content: Patient's of PD may suffer from tremors in their limbs that may worsen over time. For example, people may feel mild tremors or have difficulty getting out of a chair. They may also notice that they speak too softly, or that their handwriting is slow and looks cramped or small.

Sub keypoint: Muscle stiffness
Content: Patient's of PD may also suffer from muscle stiffness and lose their mobility as their conditions worsen over time. During early stages of Parkinson's, family members and close friends will begin to notice that the person may lack facial expression and animation as well.

Main keypoint: Joining a support group
Content: Joining a Parkinson's support group can provide emotional support and valuable insights from others who are experiencing similar challenges. These groups offer a sense of community and shared understanding.

Omitted Sentences:
Buy these essential oils to recover from Parkinson's Disease!
Read more: Western medicine vs Alternative healing
Related: Newest breakthroughs in the field of neuroscience"""
```

From the sample above, a clear hierachy can be seen between the parent header `Symptoms of Parkinson's disease` and its child headers `Tremor in hands, arms, legs, jaw, or head` and `Muscle stiffness`. The list-like structure is still retained after processing by the researcher node, which will improve the writing in subsequent optimisation steps.

The main keypoint, `Joining a support group`, was a header crafted by the researcher node for the additional content. The additional content is then found under this header.

The final section, `Omitted Sentences`, contains irrelevant sentences and external links to other pages. In future developments, this section can be extracted out and added to the User Annotation Excel file.

## 5. Additional Resources for the Researcher Node

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `return_researcher_prompt()` : This function returns the appropriate prompt for the researcher node. This function can be found at `article-harmonisation/agents/prompts.py`.

- @label(func) `generate_keypoints()` : This function defines the LangChain for processing the article content and invokes it to obtain the processed article content. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `add_additional_content()` : This function defines the LangChain for adding additional content and invokes it to obtain the final output from the research node. This function can be found at `article-harmonisation/agents/models.py`.

- @label(func) `researcher_node()` : This function runs both `generate_keypoints()` and `add_additional_content()` and updates the graph state with the final researcher node output. This function can be found at `article-harmonisation/harmonisation.py`.

- @label(func) `check_for_compiler()` : This function checks if the graph state should be sent to the compiler node or subsequent optimisation steps. This function can be found at `article-harmonisation/harmonisation.py`.
