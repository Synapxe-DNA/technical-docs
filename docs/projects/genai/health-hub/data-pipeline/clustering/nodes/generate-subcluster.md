---
updated: 20 August 2024
authors:
  - Ho Si Xian
  - Joycelyn Woo
---

# @label(func) `generate_subclusters`

This function processes the first level clusters (`first_level_pred_cluster`) and embeddings (`weighted_embeddings`) to perform a second level of clustering on clusters that have more than the specified `size_threshold`.The function performs the following tasks:

1. **Cluster Filtering:** Identifies clusters that exceed the size threshold for subclustering.
2. **Subclustering:** Applies BERTopic to perform subclustering on identified clusters.
3. **Topic Assignment:** Assign unique topic number to the newly generated subclusters.
4. **Keyword Generation:** Generates keywords for clusters that did not undergo subclustering but contain more than one article.
5. **Metrics Calculation:** Calculates and returns metrics including the number of clusters, the minimum and maximum cluster sizes, and the number of unclustered articles.

```python
def generate_subclusters(
    weighted_embeddings: pd.DataFrame,
    first_level_pred_cluster: pd.DataFrame,
    umap_parameters: Dict,
    size_threshold: float,
) -> Tuple[pd.DataFrame, pd.DataFrame, pd.DataFrame]:
```

Parameters
: **`weighted_embeddings`**`(pd.DataFrame)`: DataFrame output from the feature_engineering pipeline, containing the weighted embeddings (loaded from a .pkl file) of each article and other features such as `id` and `extracted_content_body_embeddings` columns.
: **`first_level_pred_cluster`** `(pd.DataFrame)`: A DataFrame output from `generate_clusters` node, with the predicted cluster assignments for each article.
: **`umap_parameters`** `(Dict)`: A dictionary of UMAP parameters (n_neighbors, n_components) to be used for the clustering process. Derived from values specified in `parameters_clustering.yml`.
: **`size_threshold`** `(float)`: The minimum number of articles a cluster must have to proceed with subclustering.

Returns
: **`final_predicted_cluster`** `(pd.DataFrame)`: A DataFrame with the updated clusters, including the new subclusters and cluster keywords.
: **`final_cluster_size`** `(pd.DataFrame)`: DataFrame containing the size of each cluster in bins of size 5.
: **`final_metrics`** `(pd.DataFrame)`: DataFrame containing clustering metrics which include the number of clusters, the minimum and maximum cluster sizes, and the number of unclustered articles.
