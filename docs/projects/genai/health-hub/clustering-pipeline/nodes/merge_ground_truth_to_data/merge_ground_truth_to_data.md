---
updated: 12 August 2024
authors: Ho Si Xian
---

```python
def merge_ground_truth_to_data(
    ground_truth_data: pd.DataFrame,
    content_contributor: str,
    weighted_embeddings: pd.DataFrame,
) -> pd.DataFrame:
```

This function filters the ground truth data to include only entries that match the specified content contributor. It then merges this filtered ground truth data with the weighted embeddings dataframe based on the URL.

!!! NOTE "Ground Truth Data"

    The ground truth cluster information is derived from the reference Excel file provided by the HH team. Please note that this data serves as a reference from past manual audits and should not be considered as the definitive ground truth.

Parameters

: **`ground_truth_data`** `(pd.DataFrame)`: DataFrame containing reference ground truth data, with columns including "Owner", "Page Title", "Combine Group ID", and "URL".
: **`content_contributor`** `(str)`: The name of the content contributor to filter the ground truth data. To be specified in `parameters_clustering.yml`.
: **`weighted_embeddings`** `(pd.DataFrame)`: DataFrame output from the `feature_engineering` pipeline, containing the weighted embeddings (loaded from a .pkl file) of each article and other features such as ID and URL

Returns

: **`articles_df`** `(pd.DataFrame)`: A merged DataFrame containing the weighted embeddings and ground truth data.
