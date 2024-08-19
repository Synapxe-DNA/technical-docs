---
updated: 19 August 2024
authors:
  - Ho Si Xian
---

### Get Embeddings

The `get_embeddings` processes a DataFrame containing clustered data to extract content embeddings and associated metadata. It then applies UMAP to reduce the dimensionality of the extracted embeddings for further analysis.

```python
def get_embeddings(cluster_df, umap_parameters):
```

Parameters
: **`cluster_df`** `(pd.DataFrame)`: A DataFrame containing the clustered data with the following relevant columns:

    - `extracted_content_body_embeddings` (List[np.ndarray]): Precomputed embeddings of the body content.
    - `title` (str): Titles of the documents or articles.
    - `body_content` (str): The text content of the documents or articles.
    - `id` (int): Unique identifier for each document or article.

: **`umap_parameters`** `(Dict[str, int])`: A dictionary of parameters for configuring the UMAP model. Expected keys:

    - `n_neighbors`: The size of the local neighborhood used for manifold approximation.
    - `n_components`: The dimension of the space to which the embeddings will be reduced.

Returns
: **`embeddings`** `(np.ndarray)`: The original embeddings extracted from `extracted_content_body_embeddings`.
: **`doc_titles`** `(List[str])`: A list of titles corresponding to the documents. Extracted from `title` column of `cluster_df`.
: **`docs`** `(List[str])`: A list of document body content. Extracted from `body_content` column of `cluster_df`.
: **`ids`** `(List[int])`: A list of document or article IDs. Extracted from `id` column of `cluster_df`.
: **`umap_embeddings`** `(np.ndarray)`: The embeddings after dimensionality reduction using UMAP.

### Generate Cluster Keywords

The `generate_cluster_keywords` function extracts key terms for each cluster from a set of documents using Class-based Term Frequency-Inverse Document Frequency (Class-based TF-IDF). It processes the body content of articles assigned to each cluster, performs lemmatization, and applies Class-based TF-IDF to identify the most significant words for each cluster. The function returns a dictionary where each key is a cluster identifier and the associated value is a list of the top five keywords for that cluster.

```python
def generate_cluster_keywords(pred_cluster):
```

Parameters
: **`pred_cluster`** (`pd.DataFrame`): A DataFrame output from with the following columns:

    - `body_content` (`str`): The text data from which keywords will be extracted.
    - `new_cluster` (`int`): The second level predicted cluster assignment for each `body_content`.

Returns
: **`cluster_keywords_dict`** (`Dict[int,list]`):

    : **Keys**: Cluster ID as indicated in `new_cluster` column
    : **Values**: List of top 5 keywords that represents the cluster

### Hyperparameter Tuning

The `hyperparameter_tuning` function iterates over various combination of HDBSCAN hyperparameters to identify the optimal set that yields the highest clustering quality, as measured by DBCV scores. The parameters being tuned include minimum cluster size, minimum samples, cluster selection method and the distance metric.

```python
def hyperparameter_tuning(embeddings):
```

Parameters

: **`embeddings`** `(np.ndarray)`: A numpy array of embeddings representing the data points to be used. This can be original `embeddings` or `umap_embeddings` obtained from `get_embeddings` function.

Returns

: **`best_parameters`** `(Dict[str, Union[int,str]])`: A dictionary containing the optimal set of HDBSCAN hyperparameters. Expected keys:

    - `min_cluster_size` (`int`): The optimal minimum cluster size, selected from the range `[2,3,4,5,6]`
    - `min_samples` (`int`): The optimal minimum number of samples, selected from the range `[1,2,3,4,5,6,7]`
    - `cluster_selection_method` (`str`): The optimal method for selecting clusters, with `"leaf"` being used in this tuning process (though `"eom"` can also be included as an option).
    - `metric` (`str`): Th`e optimal distance metric, selected from `["euclidean", "manhattan"]`.

### Topic Modelling

The `topic_modelling` function constructs and configures a BERTopic model to perform topic modeling on text data. It utilizes HDBSCAN for clustering reduced embeddings and allows for optional fine-tuning of topic representations. The function performs the following steps:

1. **Cluster Reduced Embeddings:** Uses HDBSCAN to cluster the reduced embeddings based on the provided hyperparameters.
2. **Tokenize Topics:** Uses `CountVectorizer` to tokenize the text data, excluding common English stop words.
3. **Create Topic Representation:** Applies the `ClassTfidfTransformer` to extract topic words from the tokenized data.
4. **Optional Fine-Tuning:** Optionally fine-tunes the topic representations using `MaximalMarginalRelevance` to enhance diversity.
5. **Return BERTopic Model:** Returns the fully configured BERTopic model ready for use in topic modeling.

```python
def topic_modelling(hyperparameters)
```

Parameters
: **`hyperparameters`** (`Dict[str, Union[int, str]`):
A dictionary containing the hyperparameters for HDBSCAN clustering, including:

- **`min_cluster_size`** (`int`): The minimum size of clusters.
- **`min_samples`** (`int`): The minimum number of samples in a neighborhood for a point to be considered a core point.
- **`cluster_selection_method`** (`str`): The method for selecting clusters.
- **`metric`** (`str`): The distance metric to be used for clustering.

Returns

: **`topic_model`** (`BERTopic`):
A BERTopic model object configured with the specified HDBSCAN parameters, a vectorizer for tokenizing topics, a c-TFIDF transformer for creating topic representations, and an optional representation model for fine-tuning topics.

### Create Topic Assigner

The `create_topic_assigner` function generates a closure that assigns new topic numbers to elements based on a specified starting counter. This is particularly useful for assigning unique topic numbers to unclustered data points (labelled as `-1` in HBDSCAN) so that each unclustered point will be assigned a unique value.

```python
def create_topic_assigner(start_counter)
```

Parameters

: **`start_counter`** (`int`):
The initial value for the topic counter, typically set as the max value of the clustered data points. This counter will be incremented each time a new topic is assigned.

Returns

: **`assign_new_topic`** (`function`):
A function that takes an input value `x` and assigns a new topic number if `x` equals `-1`. If `x` is not `-1`, the original value is returned unchanged.

### Process Cluster

The `process_cluster` function processes a DataFrame of clustered data to extract embeddings, perform hyperparameter tuning, and apply topic modeling. It assigns topics to each document, extracts relevant keywords, and ensures that unclustered data points are assigned unique topic numbers. The function involves the following steps:

1. **Extract embeddings**: Extracts both the original and UMAP-reduced embeddings from the input DataFrame, using `get_embeddings` function.
2. **Hyperparameters Tuning**: Tunes HDBSCAN hyperparameters using the UMAP-reduced embeddings, using `hyperparameter_tuning` function.
3. **Fit topic model**: Creates and fits a BERTopic model to the document data, using `topic_modelling` function.
4. **Construct DataFrame**: Constructs a DataFrame with the assigned topics, document titles, and IDs.
5. **Merge keywords**: Extracts and merges the top 5 keywords for each topic into the DataFrame.
6. **Assign new topics**: Assigns new topic numbers to unclustered data points (those with a topic value of -1), using `create_topic_assigner` function.
7. **Update topic information**: Updates the Assigned Topic column to include cluster information, ensuring there are no repeated topic numbers across clusters.

```python
def process_cluster(cluster_df, umap_parameters):
```

Parameters
: **`cluster_df`** `(pd.DataFrame)`: A DataFrame containing the clustered data with the following relevant columns:

    - `extracted_content_body_embeddings` (List[np.ndarray]): Precomputed embeddings of the body content.
    - `title` (str): Titles of the documents or articles.
    - `body_content` (str): The text content of the documents or articles.
    - `id` (int): Unique identifier for each document or article.

: **`umap_parameters`** `(Dict[str, int])`: A dictionary of parameters for configuring the UMAP model. Expected keys:

    - `n_neighbors`: The size of the local neighborhood used for manifold approximation.
    - `n_components`: The dimension of the space to which the embeddings will be reduced.

Returns
: **`result_df_kws`** (`pd.DataFrame`): A DataFrame containing the following columns:

    - `id` (`int`): Unique identifier for each document.
    - `Title` (`str`): Titles of the documents.
    - `Assigned Topic` (`str`): The final assigned topic.
    - `top_5_kws` (`List[str]`): The top 5 keywords representing each topic.

### Process All Clusters

The `process_all_clusters` function processes multiple clusters of data that needs to go through subclustering and apply the `process_cluster` function to each cluster individually. It consolidates the results into a single DataFrame.

```python
def process_all_clusters(cluster_morethan_threshold, umap_parameters):
```

Parameters
: **`cluster_morethan_threshold`** (`pd.DataFrame`):
A DataFrame containing the data for clusters that exceed a specified threshold size. This DataFrame should include a cluster column that indicates the cluster assignment for each data point.

: **`umap_parameters`** (`Dict[str, int]`): A dictionary of parameters for configuring the UMAP model. Expected keys:

    - `n_neighbors`: The size of the local neighborhood used for manifold approximation.
    - `n_components`: The dimension of the space to which the embeddings will be reduced.

Returns
: **`combined_df`** (`pd.DataFrame`): A DataFrame that consolidates the results from processing each cluster. This DataFrame contains the combined `result_df_kws` output of the `process_cluster` function applied to each individual cluster.
