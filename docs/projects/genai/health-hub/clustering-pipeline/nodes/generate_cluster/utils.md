---
updated: 12 August 2024
authors:
    - Ho Si Xian
    - Joycelyn Woo
---

!!! note
    WIP

### Create Graph Nodes
The create_graph_nodes function is designed to create nodes in a Neo4j graph database. Specifically, it inserts nodes representing articles with various attributes such as title, URL, content, meta description, and multiple vector representations.
```python 
def create_graph_nodes(tx, doc) -> None:
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
The function does not return any values. It performs an operation on the Neo4j database, creating a node with the provided attributes for each article. 

### Calculate Similarities 

```python
def calculate_similarity(tx, vector_name)
```