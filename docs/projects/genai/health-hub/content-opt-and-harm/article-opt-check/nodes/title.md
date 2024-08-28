---
updated: 26 August 2024
authors:
  - Issac J
---

# Title Evaluation

## Title Flags (Rule-based)

### Title Length

The length of the article title must not exceed 70 characters. This is a requirement for storing purposes.

## Title Judge (LLM-based)

### Title Relevance

The title evaluation assesses the relevance of the title with respect to its content. It performs the following steps -

```text
1. Identify the Title

   - What is the title of the article?

2. Analyse the Title

   - What is the main topic conveyed?
   - Is the title clear and specific?

3. Review the Content

   - Summarise and Review the article

4. Compare the Title to the Content

   - Does the content directly address the main topic of the title?
   - Is the message and theme consistent between the content and title?
   - Any missing information not reflected in the title?

5. Evaluate Relevance

   - Explain the relevance of the title and content
   - Any discrepancies
```

As the Title Evaluation is an LLM-based check, we follow the same structure (`Evaluation -> Decision -> Summarisation`) as mentioned [here](../index.md#llm-based-checks). 

Refer to the `title_evaluation_prompt` function in [`agents/prompts.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/agents/prompts.py) to understand the exact criteria used to evaluate the article title.
