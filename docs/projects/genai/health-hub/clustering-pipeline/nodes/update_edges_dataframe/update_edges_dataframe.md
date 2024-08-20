---
updated: 20 August 2024
authors:
  - Ho Si Xian
---

This function updates the edges DataFrame (`first_level_clustered_nodes`) with new cluster assignments (`final_predicted_cluster`) and keywords from the updated prediction clusters after subclustering. It also identifies nodes that remain unclustered (single article clusters) and prepares a DataFrame for these unclustered nodes with relevant columns.

```python
def update_edges_dataframe(
    first_level_clustered_nodes: pd.DataFrame,
    final_predicted_cluster: pd.DataFrame
) -> Tuple[pd.DataFrame, pd.DataFrame]:
```

Parameters
: **`first_level_clustered_nodes`** (`pd.DataFrame`): A DataFrame with information about each pair of nodes and their edge connections. Output from `generate_cluster` function.
: **`final_predicted_cluster`** `(pd.DataFrame)`: A DataFrame with the updated clusters, including the new subclusters and cluster keywords. Output from `generate_subcluster` function.

Returns
: **`final_unclustered_node`** (`pd.DataFrame`): A DataFrame containing the nodes that remained unclustered, with columns:

    - `node_id` (`int`): Article ID of the unclustered node
    - `node_title` (`str`): Article title of the unclustered node
    - `node_community` (`int`): Topic number assigned to the unclustered node

: **`final_clustered_node`** (`pd.DataFrame`): A DataFrame containing the updated clustered nodes with new cluster assignments and keywords, filtered to remove rows with missing keywords. Columns include:

    - `node_1_id` (`int`): ID of the first node
    - `node_2_id` (`int`): ID of the second node
    - `node_1_title` (`str`): Title of the first node
    - `node_2_title` (`str`): Title of the second node
    - `edge_weight` (`float`): Similarity between the two nodes
    - `node_1_pred_cluster_new` (`int`): New predicted cluster for the first node
    - `node_1_cluster_kws` (`List`): The top 5 keywords representing `node_1_pred_cluster_new` cluster
    - `node_2_pred_cluster_new` (`int`): New predicted cluster for the second node
    - `node_2_cluster_kws` (`List`): The top 5 keywords representing `node_2_pred_cluster_new` cluster
