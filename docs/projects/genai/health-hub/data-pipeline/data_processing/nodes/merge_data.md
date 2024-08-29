---
updated: 29 August 2024
authors:
  - Issac J
---

# @label(func) `merge_data`

The `merge_data` node helps to merge the data from multiple partitioned dataframes.

This function takes in a dictionary of dataframes, where each dataframe is associated with a filename. It then concatenates all the dataframes into a single one,
returning a single pandas dataframe where all articles (from various content categories) can be retrieved.

```python
def merge_data(
        all_contents_mapped: dict[str, Callable[[], Any]]
) -> pd.DataFrame:
```

Parameters
: **`all_contents_mapped`** (`dict[str, Callable[[], Any]]`):
A dictionary where keys are content categories and values are functions that return dataframes with the mappings.
Refer to the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml) for more information.

Returns
: **`merged_data`**:
Returns a parquet file (versioned based on the time of execution). The file is saved at `data/03_primary/merged_data.parquet`. A new file is generated each time the `data_processing` pipeline is executed.
Refer to the `merged_data` key in the [Data Catalog](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)
