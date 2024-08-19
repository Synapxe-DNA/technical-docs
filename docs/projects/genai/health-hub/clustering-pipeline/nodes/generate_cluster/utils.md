---
updated: 19 August 2024
authors:
    - Ho Si Xian
    - Joycelyn Woo
---

### Create Graph Nodes

The create_graph_nodes function is designed to create nodes in a Neo4j graph database. Specifically, it inserts nodes representing articles with various attributes such as title, URL, content, meta description, and multiple vector representations.

```python
def create_graph_nodes(tx, doc)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.
: **`doc`** (`Dict[str, Any]`):  A dictionary containing the article's data and vector representations. The dictionary must include the following keys:

    - `id`: The unique identifier for the article.
    - `title`: The title of the article.
    - `full_url`: The URL of the article.
    - `content`: The body content of the article.
    - `meta_description`: The meta description of the article.
    - `vector_title`: The vector representation of the article title.
    - `vector_article_category_names`: The vector representation of the article category names.
    - `vector_category_description`: The vector representation of the article category description.
    - `vector_extracted_content_body`: The vector representation of the article body content.
    - `vector_combined`: The combined vector representation of the article.
    - `vector_keywords`: The vector representation of the article keywords.
    - `ground_truth_cluster`: The ground truth cluster label of the article.

#### Returns

: The function does not return any values. It performs an operation on the Neo4j database, creating a node with the provided attributes for each article.

### Calculate Similarities

The calculate_similarity function uses Neo4j GDS to compute cosine similarity between 2 articles.

```python
def calculate_similarity(tx, vector_name)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.
: **`vector_name`** (`str`): Column name of vector to calculate cosine similarity.

#### Returns

The function returns a dictionary in the following structure:

```python
{(node_1_id, node_2_id): similarity_score}
```

: **Keys**: A tuple containing article ID for the first node (`node_1_id (int)`) and article ID for the second node (`node_2_id (int)`).
: **Values**: Cosine Similarity score between the `vector_name` of `node_1_id` and `node_2_id`.

### Fetch Ground Truth

This function fetches the ground truth for each article and return a dictionary of article id and the ground truth label. To be used within `combine_similarities` function.

#### Parameters

: **`session`** (`neo4j.Session`): A Neo4j session object used to execute queries within a transaction.

#### Returns

: **`ground_truth`** (`Dict[int, int]`): A dictionary containing the article's id as the key and ground truth as the value.

### Combine Similarities

The `combine_similarities` function combines similarity scores from various feature vectors (such as title, category, description, body content, combined vectors, and keywords) into a single weighted similarity score for each pair of nodes (articles) in a Neo4j database. This combined similarity score is then used for further analysis or clustering tasks.

```python

def combine_similarities(
    session,
    weight_title,
    weight_cat,
    weight_desc,
    weight_body,
    weight_combined,
    weight_kws,
):
```

#### Parameters

: **`session`** (`neo4j.Session`): A Neo4j session object used to execute queries within a transaction.
: **`weight_title`** (`int`): The weight assigned to title similarity for weighted similarity. To be retrieved from `parameters_clustering.yml`
: **`weight_cat`** (`int`): The weight assigned to the category similarity score for weighted similarity. To be retrieved from `parameters_clustering.yml`
: **`weight_desc`** (`int`): The weight assigned to the description similarity score for weighted similarity. To be retrieved from `parameters_clustering.yml`
: **`weight_body`** (`int`): The weight assigned to the body content similarity score for weighted similarity. To be retrieved from `parameters_clustering.yml`
: **`weight_combined`** (`Literal[0,1]`): A flag indicating whether weighted embeddings will be used. Set to `1` to use weighted embedding, and `0` to disable their use. When set to `1` ensure all other related weights are set to `0`. To be retrieved from `parameters_clustering.yml`
: **`weight_kws`** (`int`): The weight assigned to the keywords similarity score. To be retrieved from `parameters_clustering.yml`.

#### Returns

: **`combined_similarities_df`** (`pd.DataFrame`):
A DataFrame containing combined similarity scores for each pair of articles. Each row represents a pair of articles with the following columns:

    - **`node_1_id`** (`int`): The ID of the first article in the pair.
    - **`node_2_id`**  (`int`): The ID of the second article in the pair.
    - **`similarities_title`** (`float`): The similarity score based on the title vectors.
    - **`similarities_cat`** (`float`): The similarity score based on the category vectors.
    - **`similarities_desc`** (`float`): The similarity score based on the description vectors.
    - **`similarities_body`** (`float`): The similarity score based on the body content vectors.
    - **`similarities_combined`** (`float`): The similarity score based on the combined vectors.
    - **`similarities_kws`** (`float`): The similarity score based on the keywords vectors.
    - **`weighted_similarity`** (`float`): The combined weighted similarity score calculated from the individual feature similarities using the provided weights.

### Get Median Threshold

The `median_threshold` function calculates the median of the weighted similarity scores for pairs of articles that share the same ground truth cluster. This median value may be used as a threshold for clustering.

Note: ground truth cluster information is gathered from the reference excel file provided by HH team.

```python
def median_threshold(combined_similarities_df)
```

#### Parameters

: **`combined_similarities_df`** (`pd.DataFrame`): A DataFrame output from combine_similarities function, containing similarity scores between pairs of articles

#### Returns

: **`threshold`** (`float`): Median of the weighted similarity scores for pairs of articles that share the same ground truth cluster.

### Create Similarity Edges

The `create_sim_edges` function creates edges between nodes in a Neo4j graph database based on their similarity scores. It iterates through a DataFrame of similarity scores and creates an edge between nodes if their weighted similarity exceeds a given threshold. This helps in forming connections between articles that are considered similar according to the specified criteria.

```python
def create_sim_edges(tx, similarities, threshold)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.
: **`similarities`** (`pd.DataFrame`): A dataframe output from `combine_similarities` function, containing similarity scores between pairs of articles.
: **`threshold`** (`float`): A value used as a cut-off point. Only pairs of nodes with a weighted similarity above this threshold will have edges created between them. This value can be pre-defined in `parameters_clustering.yml` or using the output threshold value from `median_threshold` function.

#### Returns

: The function does not return any values. It performs operations within the Neo4j database to create edges based on the provided similarity scores and threshold.

### Drop Graph Projection

This function checks if a graph projection named `articleGraph` exists within the Neo4j database. If the projection exists, the function proceeds to drop (delete) it.

```python
def drop_graph_projection(tx)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: The function does not return any values. It performs operations within the Neo4j database to drop the `articleGraph` graph projection if exists

### Create Graph Projection

The `create_graph_proj` function creates a graph projection named `articleGraph` in Neo4j using the Graph Data Science (GDS) library. This projection is used to facilitate graph algorithms and analysis by defining the nodes and relationships that should be included in the projected graph. In this case, it projects all nodes of type `Article` and their `SIMILAR` relationship defined as `similarity`.

```python
def create_graph_proj(tx)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: The function does not return any values. It performs an operation within the Neo4j database to create a graph project for subsequent graph data science operations.

### Detect Community

The `detect_community` function performs community detection on a projected graph in Neo4j using the Louvain algorithm from the Graph Data Science (GDS) library. This algorithm is used to identify clusters or communities within the graph. The results of the community detection are written back to the nodes with a property called community.

```python
def detect_community(tx)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: The function does not return any values. It performs an operation within the Neo4j database to execute the Louvain community detection algorithm and write the detected community information back to the nodes in the graph.

### Retrieve Predicted Cluster

The `return_pred_cluster` function retrieves the predicted cluster assignments for articles from a Neo4j graph database. It queries the graph to get details of each article, including its ID, title, URL, body content, and the detected community (i.e. cluster label) to which it belongs. The results are ordered by the community and returned as a Pandas DataFrame.

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: **`df`** (`pd.DataFrame`): A DataFrame consisting of the following columns:

    - `id` (`int`): The unique identifier for the article.
    - `title` (`str`): The title of the article.
    - `url` (`str`): The URL of the article.
    - `body_content` (`str`):  The body content of the article.
    - `cluster` (`int`): the community/cluster label to which each article has been assigned.

### Get Cluster Size

The `get_cluster_size` function analyzes the sizes of clusters predicted by the clustering algorithm. It groups the articles by their cluster assignment, calculates the size of each cluster, and then categorizes these sizes into bins of size 5 for easier interpretation. The function returns a DataFrame summarizing the number of clusters within each size range, including a count of single unclustered articles.

```python
def get_cluster_size(pred_cluster, column_name="cluster")
```

#### Parameters

: **`pred_cluster`** (`pd.DataFrame`): A DataFrame of predicted cluster with a cluster column with column name defined in `column_name` parameter.
: **`column_name`** (`str`): Column name of the cluster label.

#### Returns

: **`cluster_size`** (`pd.DataFrame`): DataFrame containing the size of each cluster in bins of size 5.

### Get Clustered Nodes

This `get_clustered_nodes` function retrieves information about each pair of nodes and their connections within a Neo4j graph database. It executes a Cypher query to match nodes and their relationships, returning details about the nodes, their similarities and cluster information.

```python
def get_clustered_nodes(tx)
```

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: **`df`** (`pd.DataFrame`): A DataFrame consisting of the following columns:

    - `node_1_id`: Identifier of the first node.
    - `node_2_id`: Identifier of the second node.
    - `node_1_title`: Title of the first node.
    - `node_2_title`: Title of the second node.
    - `edge_weight`: Similarity score between the two nodes.
    - `node_1_ground_truth`: Ground truth cluster of the first node.
    - `node_2_ground_truth`: Ground truth cluster of the second node.
    - `node_1_pred_cluster`: Predicted community cluster of the first node.
    - `node_2_pred_cluster`: Predicted community cluster of the second node.

### Get Unclustered Nodes

The `get_unclustered_nodes` function retrieves details about nodes in a Neo4j graph database that do not have any relationships with other nodes. It executes a Cypher query to match nodes that are isolated.

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: **`df`** (`pd.DataFrame`): A DataFrame consisting of the following columns:

    - `node_title`: Title of the first node.
    - `node_meta_desc`: Meta Description of the node
    - `node_community`: Predicted community cluster of the node

### Count Articles

The `count_articles` function retrieves the count of articles grouped by their community cluster assignment from a Neo4j graph database. It executes a Cypher query to match nodes with the Article label, aggregate them by their cluster, and count the number of articles in each cluster.

#### Parameters

: **`tx`** (`neo4j.Transaction`):  A Neo4j transaction object used to execute the Cypher query.

#### Returns

: **`df`** (`pd.DataFrame`):  A DataFrame consisting of the following columns:

    - `cluster`: The community cluster
    - `article_count`: The number of articles in each community cluster
