---
updated: 26 August 2024
authors:
  - Issac J
---

# Meta Description Evaluation

## Meta Description Flags (Rule-based)

### Meta Description Length

The length of the article meta description must be within 70 and 160 characters. This is a requirement for storing purposes.

## Meta Description Judge (LLM-based)

### Meta Description Relevance

The meta description evaluation assesses the relevance of the meta description with respect to its content. It performs the following steps -

```text
1. Identify the Meta Description

    - What is the meta description of the article?

2. Analyse the Meta Description

    - What is the main topic conveyed?
    - Is the meta description clear, concise and engaging?

3. Review the Content

    - Summarise and Review the article

4. Compare the Meta Description to the Content

    - Does the content directly address the main topic of the meta description?
    - Is the message and theme consistent between the content and meta description?
    - Any missing information not reflected in the meta description?

5. Evaluate Relevance

    - Explain the relevance of the meta description and content
    - Any discrepancies
```

As the Meta Description Evaluation is an LLM-based check, we follow the same structure (`Evaluation -> Decision -> Summarisation`) as mentioned [here](../index.md#llm-based-checks). 

Refer to the `meta_desc_evaluation_prompt` function in [`agents/prompts.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/agents/prompts.py) to understand the exact criteria used to evaluate the article meta description.
