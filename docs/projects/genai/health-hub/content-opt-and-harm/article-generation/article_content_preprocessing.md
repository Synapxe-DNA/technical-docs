---
updated: 27 August 2024
authors:
  - Jin Chou
---

## 1. Introduction

This section will be covering the preprocessing step for an article content before it is processed by the researcher node. The preprocessing is conducted by a rule-based function. All article content will need to undergo this step before being processed by the researcher node.

## 2. Rationale for Article Content Preprocessing

Some articles contain a list-like structure, where there is a clear hierachy between headers. For example, a h2 header can be used to introduce the readers to the list, followed by h3 headers representing an item each. This example can be be seen below:

```html
<h2> 2 bodyweight exercises to help you achieve your weight goal </h2>

<h3> 1. Pushups </h3>
<p> Details about pushups </p>

<h3> 2. Situps </h3>
<p> Details about situps </p>
```

For this explanation and for the rest of this page, we will be referring to the h2 headers as a "parent" to the h3 headers and the h3 headers as a "child" to the h2 headers.

This relationship between a parent and child header is not captured effectively in the `extracted_content_body` when extracting it from the  `merged_data.parquet` file. Hence, a pre-processing step is added to better accentuate these relations between headers.
