---
updated: 29 August 2024
authors:
  - Issac J
---

# @label(func) `extract_data`

## Overview

The `extract_data` node helps to extract data from the updated dataframes and stores it in parquet files and text files.

This function takes in a dictionary of dataframes, where each dataframe is associated with a filename. It then extracted various data by performing the following steps:

1. Check if the article is flagged for removal via the `to_remove` column
2. Check if the HTML content contains a table and/or an image
3. Extracts the related sections, tables, URLs, headers, image links and processed text via BeautifulSoup
4. Flags articles that should be removed after extraction.

The function returns a tuple containing a dictionary mapping content categories to the updated dataframes and a dictionary mapping the filenames to the extracted texts as a `.txt` file.

```python
def extract_data(
    all_contents_added: dict[str, Callable[[], Any]],
    word_count_cutoff: int,
    whitelist: list[int],
    blacklist: dict[int, str],
) -> tuple[dict[str, pd.DataFrame], dict[str, str]]:
```

## Parameters

: **`all_contents_added`** (`dict[str, Callable[[], Any]]`):
A dictionary where keys are content categories and values are functions that return dataframes of the updated content.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

: **`word_count_cutoff`** (`int`):
The minimum number of words in an article to be considered before flagging for removal.
This uses the `word_count_cutoff` parameter from [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml).

: **`whitelist`** (`list[int]`):
The list of article IDs to keep.
This uses the `whitelist` parameter from [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml).

: **`blacklist`** (`dict[int, str]`):
A dictionary containing the article IDs and the reason to remove it.
This uses the `blacklist` parameter from [`parameters_data_processing.yml`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_data_processing.yml).

## Returns

: **`all_contents_extracted`**:
Returns a set of parquet files corresponding to the content categories. These files are saved at `data/02_intermediate/all_contents_extracted`. Refer to the `all_contents_extracted` key in the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)

: **`all_extracted_text`**:
Returns a set of `.txt` files corresponding to the content categories. These files are saved at `data/02_intermediate/all_extracted_text`. Refer to the `all_extracted_text` key in the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)

## Note

: Some articles will be flagged for removal after extraction. Refer to the `flag_articles_to_remove_after_extraction` function in [`utils.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/src/content_optimization/pipelines/data_processing/utils.py) for more information.
