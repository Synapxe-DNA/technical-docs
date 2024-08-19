---
updated: 16 August 2024
authors:
  - Ho Si Xian
  - Joycelyn Woo
---

!!! note
WIP

### Generate Cluster Keywords

The `generate_cluster_keywords` function extracts key terms for each cluster from a set of documents using Class-based Term Frequency-Inverse Document Frequency (Class-based TF-IDF). It processes the body content of articles assigned to each cluster, performs lemmatization, and applies Class-based TF-IDF to identify the most significant words for each cluster. The function returns a dictionary where each key is a cluster identifier and the associated value is a list of the top five keywords for that cluster.

Parameters
: **`pred_cluster`** (`pd.DataFrame`): A DataFrame output from with the following columns:

    - `body_content` (`str`): The text data from which keywords will be extracted.
    - `new_cluster` (`int`): The second level predicted cluster assignment for each `body_content`.

Returns
: **`cluster_keywords_dict`** (`Dict[int,list]`):
: **Keys**: Cluster ID as indicated in `new_cluster` column
: **Values**: List of top 5 keywords that represents the cluster
