---
updated: 29 August 2024
authors:
  - Issac J
---

# Flagging Articles for Removal

## Overview

Based on our Exploratory Data Analysis and requests from either the Clustering or HealthHub Team, we had to flag some articles to be removed from downstream applications. We perform the flagging before and after extracted the processed text from the HTML content.

## Flagging Before Extraction

### NaN Content in `content_body`

There is no HTML content present for extraction is the content is null.

### Excel Error

This typically occurs when the number of characters exceeds the cell capacity in Excel. As such, we utilised the `parquet` file format to combat this limitation.

### No HTML Tags

This typically occurs when test articles that are submitted into the VML system for testing purposes.

## Flagging After Extraction

Please note that the flagging of articles in this case is ordered due to overlapping flags. We will typically ignore the articles that are already flagged as `to_remove` before flagging the remaining articles.

Additionally, some articles are whitelisted as HealthHub Team wishes to perform Content Optimisation and Harmonisation using these articles. Refer to the `whitelist` key in [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml).
Refer to the `flag_articles_to_remove_after_extraction` function in [`utils.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/src/content_optimization/pipelines/data_processing/utils.py) for more information.

### No Extracted Content

This typically occurs when there are only HTML tags present in the content body. As such, the extracted content is null. This is usually because the article is an infographic.

### Recipe Articles

HealthHub Team has informed us to exclude recipe articles from Content Optimisation and Harmonisation as these recipes are usually different from one another. Moreover, the current structure is deemed suitable for reading.

We perform a keyword search to remove these articles. However, please note that it does not flag all recipe articles as we are using a heuristic.

### Duplicated Content

We remove duplicated content based on the `extracted_content_body`. There is a high chance that it refers to the same article or that the article link has been updated.

### Duplicated URLs

We remove duplicated content based on the `full_url`. There is a high chance that it refers to the same article or the article content has been updated.

### Multilingual Content

We remove Chinese, Malay or Tamil articles as we do not want it to interfere with the clustering algorithm. Moreover, there are future plans to implement a translation process to convert all English articles into Chinese, Malay and Tamil.

### Below Word Count

Some articles have extraordinarily low word counts. We believe that these articles are either short recipes or a brief summary of the infographic embedded in the article. By plotting the Word Count Distribution of all articles, we deemed `90 words` as the threshold to flag such articles.

### Blacklisted Articles

These articles are either flagged during clustering or upon the HealthHub Team's request. Refer to this [link](https://docs.google.com/spreadsheets/d/1W5GuJ2beB7rICJyIOuEyCNmCTaVhC8I7/edit?gid=575939891#gid=575939891) for more information.
The blacklisted articles are stated as parameters under the `blacklist` key in the [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml) file.
