---
updated: 23 September 2024
authors:
  - Richmond Sin
---

# @label(func) `extract_content`

## Overview

The `Extract Content` node extracts and organizes content and table data from `processed_data_rag` dataframe into a dictionary format that will be partitioned into individual JSON files that represents each article.

This function takes in the `processed_data_rag` dataframe, the `article_content_columns`, and `table_content_columns` parameters. It extracts and organized the article data by performing the following steps:

1. Initializes an empty dictionary `json_data_rag` to store extracted data.
2. Iterates over each row in the DataFrame using iterrows(), identifying the row by its “id” column.
3. Extracts content columns specified in `article_content_columns`, checks for the presence of tables (has_table column), and processes the article’s content by removing the `has_table` column and converting the `extracted_content_body` to a string format.
4. Appends the processed content to the dictionary using a unique key generated from the row’s `id` with the suffix `_content`.
5. If a table is present (has_table is true), the function extracts and processes the table columns specified in `table_content_columns`, converting the `processed_table_content` to a string format.
6. Appends the processed table data to the dictionary using a unique key generated from the row’s `id` with the suffix `_table`.
7. Returns the json_data_rag dictionary containing both content and table data for each row.

The function returns a dictionary that will be split into individual JSON articles by Kedro PartitionDataset.

```python
def extract_content(
    processed_data_rag: pd.DataFrame,
    article_content_columns: List[str],
    table_content_columns: List[str],
) -> Dict[str, List[Dict[str, Any]]]:
```

## Parameters

: **`article_content_columns`** (`List[str]`):
A list of strings of the columns to be extracted to prepare the articles content JSON.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

: **`table_content_columns`** (`List[str]`):
A list of strings of the columns to be extracted to prepare the articles table content JSON.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

## Returns

: **`json_data_rag`**:
Returns a dictionary that will be split into individual JSON articles by Kedro PartitionDataset. The JSON files are saved at `data/03_primary/processed_articles`. Refer to the `json_data_rag` key in the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)

!!! Note

    The data generated from this pipeline is not final, you will have to combine the JSON files generated from this pipeline and the [`two Javascript articles`](https://synapxe-dna.github.io/technical-docs/projects/genai/health-hub/data-pipeline/azure_rag/js_articles/) to ingest into the search index.
