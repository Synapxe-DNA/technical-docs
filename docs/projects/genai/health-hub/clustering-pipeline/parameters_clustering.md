---
updated: 12 August 2024
authors: Ho Si Xian
---

## parameters_clustering.yml

This document describes the configuration parameters used in the `parameters_clustering.yml` file for the `clustering` Kedro pipeline.

## Neo4j Configuration
```yaml
neo4j_config:
  uri: neo4j://localhost:7687
  database: hh-articles
```
: **`uri`**: The URI for the Neo4j Instance. In this case, it points to a locally hosted instance on port 7686
: **`database`**: The name of databse to connect to, here it is set to `hh-articles`

## Content Contributor
This parameter specifies the organization or entity contributing to the content. 
```yaml
content_contributor: Health Promotion Board
```
: **`content_contributor`**: The name of the content contributor, set to "Health Promotion Board". This filters the data to only include rows where the `pr_name` column matches this value

## Similarity Weightage
This section defines the weightage for different aspects of similarity. To ensure proper weighted similarity, the total sum of all weights must equal `1`.  

!!! NOTE 
    The current clustering pipeline uses weighted embeddings instead of weighted similarity. This configuration is still kept, but `weight_combined` can be set to `1`, with all other set to `0`, to use the weighted embedding instead of weighted embedding.

```yaml
sim_weightage:
  weight_title: 0
  weight_cat: 0
  weight_desc: 0
  weight_body: 0
  weight_combined: 1
  weight_kws: 0
```
: **`weight_title`**: The weight assigned to title similarity. Here it is set to `0`, indicating no weight
: **`weight_cat`**: The weight assigned to category similarity, also set to `0`
: **`weight_desc`**: The weight assigned to meta description similarity, set to `0`
: **`weight_body`**: The weight assigned to body content similarity, set to `0`
: **`weight_combined`**: The weight assigned to the weighted embeddings, set to `1`. This indicate that weighted embeddings will be used instead of weight similarity. Ensure that all other values are set to `0`.
: **`weight_kws`**: The weight assigned to the keyword similarity, set to `0`.

## Threshold Setting
This parameter controls whether a predefined threshold is set. 
```yaml
set_threshold: False
```
: **`set_threshold`**: Threshold value used to determine if edges should be formed between nodes based on similarity. This should be a `float`. If set to `False`, the threshold will default to the median similarity threshold derived from ground truth

## UMAP Parameters
This section defines the UMAP (Uniform Manifold Approximation and Projection) parameters for dimensionality reduction. To be used for BERTopic.

```yaml 
umap_parameters:
  n_neighbors: 15
  n_components: 8
```
: **`n_neighbors`**: The number of neighbours to consider for each point in UMAP. Balances local versus global structure in the data. Low values will force UMAP to concentrate on very local strucuture while large values will push to look at larger neighborhoods of each point.

: **`n_components`**: The number of dimensions to reduce the data to. 