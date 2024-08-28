---
updated: 26 August 2024
authors:
  - Issac J
---

# Content Evaluation

## Content Flags (Rule-based)

### Readability

To assess the readability score of the article, we utilised a similar scoring mechanism as the [Hemmingway Editor](https://hemingwayapp.com/). We reverse-engineered the scoring function using this [article](https://medium.com/free-code-camp/https-medium-com-samwcoding-deconstructing-the-hemingway-app-8098e22d878d) from medium.

???+ Note "Hemmingway Score (H)"

    H = Math.round(4.71 * **average word length** + 0.5 * **average sentence length** - 21.43)

However, we had to amend the function slightly due to the presence of URL links and the breaking up of sentence during parsing. We used `Math.ceil()` instead to resolve these issues.
If the Hemmingway score is at least 10, it is considered `hard` to read. These articles are then passed to an LLM to explain why the article is `hard` to read.

For a better understanding of the scoring function, refer to `utils/evaluations.py`.

### Insufficient Content

To assess whether the article has sufficient content, we used word as a proxy indicator. Our focus is on these five content categories (`"cost-and-financing"`,`"live-healthy-articles"`, `"diseases-and-conditions"`, `"medical-care-and-facilities"`, `"support-group-and-others"`).

We first plotted a box plot to obtain the distribution of the article word counts across these content categories from all content providers. From there, we decided that the lower quartile (i.e. 25th percentile) can serve a good threshold to flag insufficient content.

???+ Note "Word Count Thresholds"

    - "cost-and-financing": 267
    - "live-healthy-articles": 413
    - "diseases-and-conditions": 368
    - "medical-care-and-facilities": 202
    - "support-group-and-others": 213

## Content Judge (LLM-based)

### Writing Style

!!! Warning

    The Writing Style Evaluation has been deprecated as its current implementation results in all articles being flagged for optimisation. While removing it is a stopgap measure, the long-term goal is to refine the prompts (i.e. using Decomposed Prompting) so that the content evaluations are more targeted.

The Writing Style (structure) evaluation assesses the content structure across 5 key dimensions - Opening, Content Structure, Writing Style, Closing and Overall Effectiveness.

```text
1. Opening

   - Evaluates the clarity and effectiveness of the introduction

2. Content Structure

   - Evaluates the organisation of the content
   - Evaluates whether the paragraphs are succinct and easy to read

3. Writing Style

   - Evaluates whether the tone is conversational
   - Evaluates whether complicated terms were used for its intended audience
   - Evaluates whether the content is relatable

4. Closing

   - Evaluates whether the next steps are clearly outlined
   - Evaluates whether the message is clearly reinforced

5. Overall Effectiveness

   - Evaluates whether the article insights are actionable
   - Evaluates whether the main content fulfills the promise made in the introduction
```

As the Writing Style Evaluation is an LLM-based check, we follow the same structure (`Evaluation -> Decision -> Summarisation`) as mentioned [here](../index.md#llm-based-checks). 

Refer to the `structure_evaluation_prompt` function in [`agents/prompts.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/agents/prompts.py) to understand the exact criteria used to evaluate the article content.
