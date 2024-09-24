---
updated: 23 September 2024
authors:
  - Richmond Sin
---

# @label(func) `filter_articles`

## Overview

The `Filter Articles` node filter out the articles that have `No HTML Tags`, `No Extracted Content`, `NaN`, `Multilingual`, `Duplicated Content` from `remove_type` column. It also removes duplicated articles with specific 'id' values and articles that are too lengthy to be processed by the LLM.

The link to the article `id` that are filtered for removal are in this file [`To remove excel sheet`](https://docs.google.com/spreadsheets/d/1PjRx_GkdlNZpV--Ui6sLd0Hvk-3LJ6qk/edit?gid=1214237528#gid=1214237528).

This function takes in the `merged_data` dataframe, the `duplicated_articles`, `duplicated_content`, and `lengthy_articles` parameters. It filters out the articles by performing the following steps:

1. Remove articles with 'No HTML Tags' from the 'remove_type' column
2. Remove the rows with 'No Extracted Content' from 'remove_type' column
3. Remove the rows with 'NaN' from 'remove_type' column
4. Remove 'Multilingual' from 'remove_type' column
5. Remove the duplicated articles with specific 'id' values
6. Remove 'Duplicated Content' from 'remove_type' column, except for specific 'id' values
7. Remove the articles that are too lengthy

The function returns a dataframe for further processing.

```python
def filter_articles(
    merged_data: pd.DataFrame,
    duplicated_articles: List[int],
    duplicated_content: List[int],
    lengthy_articles: List[int],
) -> pd.DataFrame:
```

## Parameters

: **`duplicated_articles`** (`List[int]`):
A list of integers where it contains the `id` of the articles that have duplicated URLs.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

: **`duplicated_content`** (`List[int]`):
A list of integers where it contains the `id` of the articles that have duplicated content.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

: **`lengthy_articles`** (`List[int]`):
A list of integers where it contains the `id` of the articles that are too lengthy to be processed by the LLM.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

## Returns

: **`filtered_data_rag`**:
Returns a parquet files that have filtered out the articles. The file is saved at `data/03_primary/filtered_data.parquet`. Refer to the `filtered_data_rag` key in the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)
