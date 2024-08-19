---
updated: 19 August 2024
authors:
  - Ho Si Xian
  - Joycelyn Woo
---

```python
def generate_clusters(
    merged_df_with_groundtruth: pd.DataFrame,
    neo4j_config: Dict[str, str],
    weight_title: float,
    weight_cat: float,
    weight_desc: float,
    weight_body: float,
    weight_combined: float,
    weight_kws: float,
    set_threshold: Union[Literal[False], float],
) -> Tuple[pd.DataFrame, pd.DataFrame, pd.DataFrame, pd.DataFrame, pd.DataFrame]:
```

The `generate_cluster` node orchestrates the process of clustering articles using multiple features by weightage and community detection in a Neo4j graph database. It takes in a DataFrame with article data, configuration for Neo4j, and various weights for different features. The function performs the following tasks:

1. **Similarity Calculation:** Computes similarity scores between articles based on weighted embeddings of their features.
2. **Threshold Setting:** Determines a threshold value for edge creation based on similarity scores. A pre-defined threshold value may also be used.
3. **Graph Projection and Community Detection:** Projects the graph and detects communities within the graph.
4. **Cluster Results:** Compiles clustering results, including generated cluster keywords and counting articles in each cluster.
5. **Output Files for Visualisation:** Returns list of clustered and unclustered nodes for visualisation in `cluster_viz` node.

**Parameters**

: **`neo4j_config`** (`Dict[str, str]`): Configuration to connect to Neo4j database. Derived from values specified in `parameters_clustering.yml`.
: **`merged_df_with_groundtruth`** (`pd.DataFrame`): The DataFrame output from `merge_ground_truth_data` node.
: **`weight_title`** (`float`): Weightage of title similarity score to use to compute weighted similarity. Derived from values specified in `parameters_clustering.yml`.
: **`weight_cat`** (`float`): Weightage of article category similarity score to use to compute weighted similarity. Derived from values specified in `parameters_clustering.yml`.
: **`weight_desc`** (`float`): Weightage of meta description similarity score to use to compute weighted similarity. Derived from values specified in `parameters_clustering.yml`.
: **`weight_body`** (`float`): Weightage of body content similarity score to use to compute weighted similarity. Derived from values specified in `parameters_clustering.yml`.
: **`weight_kws`** (`float`): Weightage of body content similarity score to use to compute weighted similarity. Derived from values specified in `parameters_clustering.yml`.
: **`weight_combined`** (`float`): Weightage of article keywords similarity score to use to compute weighted similarity. Derived from values specified in `parameters_clustering.yml`.
: **`set_threshold`** (`Union[None, float]`): Threshold for clustering. Set as "" if not pre-defining threshold.

**Returns**

##### For analysis

: **`first_level_pred_cluster`** (`pd.DataFrame`): A DataFrame with the predicted cluster assignments for each article and the keywords description of each cluster.
: **`first_level_metrics`** (`pd.DataFrame`):A DataFrame containing the threshold, parameters and cluster output metrics which include number of clusters, minimum cluster size, maximum cluster size, number of single unclustered articles.
: **`first_level_cluster_size`** (`pd.Series`): A DataFrame summarising the number of clusters within each size range, including a count of single unclustered articles.

##### For visualisation (in `cluster_viz` node)

: **`first_level_clustered_nodes`** (`pd.DataFrame`): A DataFrame with information about each pair of nodes and their edge connections.
: **`first_level_unclustered_nodes`** (`pd.DataFrame`): A DataFrame with information about each single node.

**Raises**

: **`Neo4jError`**: If an error occurs with Neo4j database operations.
