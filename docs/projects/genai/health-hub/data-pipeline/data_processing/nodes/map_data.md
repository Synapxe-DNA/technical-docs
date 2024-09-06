---
updated: 29 August 2024
authors:
  - Issac J
---

# @label(func) `map_data`

## Overview

The `map_data` node helps to map the article to L1 and L2 Information Architecture (IA) categories. Refer to this [IA Mappings](https://docs.google.com/spreadsheets/d/1bcmD7GXILn4LqF0Bo9dcdCnRomwCVFQv) document for more information.

This function takes in a dictionary of dataframes, where each dataframe is associated with a filename. It then applies the mappings by performing the following steps:

1. Invert the provided L1 and L2 mappings for each content category
2. Applies the new mappings as new columns - `l1_mappings` and `l2_mappings` using the `article_category_names` column value

The function returns a dictionary mapping content categories to the updated dataframes - new columns are inserted into the dataframe.

```python
def map_data(
    all_contents_extracted: dict[str, Callable[[], Any]],
    l1_mappings: dict[str, dict[str, list[str]]],
    l2_mappings: dict[str, dict[str, list[str]]],
) -> dict[str, Callable[[], Any]]:
```

## Parameters

: **`all_contents_extracted`** (`dict[str, Callable[[], Any]]`):
A dictionary where keys are content categories and values are functions that return dataframes with the extracted content.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

: **`l1_mappings`** (`dict[str, dict[str, list[str]]]`):
A dictionary of L1 category mappings. The outer key is the content category, inner key is the target (new) category, and the value is a list of source (old) categories.
This uses the `l1_mappings` parameter from [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml).

: **`l2_mappings`** (`dict[str, dict[str, list[str]]]`):
A dictionary of L2 category mappings, structured similarly to `l1_mappings`.
This uses the `l2_mappings` parameter from [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml).

## Returns

: **`all_contents_mapped`**:
Returns a set of parquet files corresponding to the content categories. These files are saved at `data/02_intermediate/all_contents_mapped`. Refer to the `all_contents_mapped` key in the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)
