---
updated: 23 September 2024
authors:
  - Richmond Sin
---

# Filtered Articles for Removal

## Overview

Based on our Exploratory Data Analysis and requests from the HealthHub Team, we had to filter some articles to be removed from downstream applications.

## Filter Before Extraction

### NaN Content in `content_body`

There is no HTML content present for extraction is the content is null.

### No HTML Tags

This typically occurs when test articles that are submitted into the VML system for testing purposes.

### No Extracted Content

This typically occurs when there are only HTML tags present in the content body. As such, the extracted content is null. This is usually because the article is an infographic.

### Duplicated Content

We remove duplicated content based on the `extracted_content_body`. There is a high chance that it refers to the same article or that the article link has been updated.

### Duplicated URLs

We remove duplicated content based on the `full_url`. There is a high chance that it refers to the same article or the article content has been updated.

### Multilingual Content

We remove Chinese, Malay or Tamil articles as we do not want it to interfere with the clustering algorithm. Moreover, there are future plans to implement a translation process to convert all English articles into Chinese, Malay and Tamil.
