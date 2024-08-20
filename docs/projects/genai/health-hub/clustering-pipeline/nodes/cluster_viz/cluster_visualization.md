---
updated: 20 August 2024
authors:
  - Ho Si Xian
  - Joycelyn Woo
---

The `cluster_viz` node generates an interactive visualization of clustered and unclustered nodes in a network graph using the Pyvis library. The function creates a visual representation where nodes represent articles, and edges represent similarity score between the nodes. Details such as predicted cluster label and cluster keywords are included in the graph.

```python
def cluster_viz(
    final_clustered_nodes: pd.DataFrame,
    final_unclustered_nodes: pd.DataFrame
):
```

Parameters
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

Returns
: The function saves the interactive network graph as an HTML file at `data/07_model_output/neo4j_cluster_viz.html` and does not return any value. This file visualizes:

    - Nodes and edges from the clustered nodes with attributes like predicted clusters, keywords, and edge weights.
    - Single nodes from the unclustered node.
