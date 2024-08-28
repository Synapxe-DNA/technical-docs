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

## 3. Explanation of Article Content Preprocessing Output

This step starts by extracting the article headers and its content body from `merged_data.parquet`.

It will extract the html tag of the header from `extracted_headers` and determine which header category the header to. The header categories are listed in the table below.
<center>

| html tag | header category |
| ----------- | ----------- |
| h1 | Main Header |
| h2 | Sub Header|
| h3 - h6 | Sub Section|

</center>

The article preprocessing will concatenate the html header tag with the header category like so:

`< html tag > < header category >: < article header >`

The function will also check for any potential parent for each header. Any preceding header of a greater hierachy than the header currently processed will be considered it's "parent". In other words, a h2 header will be considered to be the parent of any subsequent h3 headers, until the next h2 header or h1 header. Likewise, if the article flows consist of a h2, h3, then a h4 header, the h3 header will be considered the parent of the h4 header.

If any parent header is found, the details will be captured in a String like so:

`<header category of current header> to <html tag of parent header> <header category of parent header>: <parent header>`

If no parent header is found, there will be no additional String below.

After processing the header type and checking for any parents, the rest of the content pertaining to the specific header will follow.

## 4. Example of Article Content Preprocessing Output

Continuing from the example above, this is will be the output after the preprocessing step:

```python
"""h2 Sub Header: 2 bodyweight exercises to help you achieve your weight goal

h3 Sub Section: 1. Pushups
Sub Section to h2 Sub Header: 2 bodyweight exercises to help you achieve your weight goal
Content: # All content under this header

h3 Sub Section: 2. Situps
Sub Section to h2 Sub Header: 2 bodyweight exercises to help you achieve your weight goal
Content: # All content under this header"""
```

The h2 header in the example is referred to as a h2 Sub Header, corresponding to it's header category from the table. The h2 header is not a sub header to any parent header, and neither does it have any content below the header, hence there is only a single line.

The h3 headers also has their html tags and corresponding header category captured in the first line, followed by a second sentence stating their relation to the h2 Sub Header. Finally, the content for the h3 Sub Sections are concatenated below.

## 5. Additional Resources for the article content preprocessing process

Kindly refer to these sections for further clarification and details on the code:

- @label(func) `concat_headers_to_content()` : This function runs the rule-based preprocessing step. This function can found be at `article-harmonisation/utils/formatters.py`.
