---
updated: 20 August 2024
authors:
  - Joycelyn Woo
---

# Embedding Techniques

## Introduction

Ablation studies were conducted to choose the best embedding method to generate document embeddings to best represent each article. First, we evaluate the effectiveness of different pooling strategies and embedding models for generating document representations. We focused on comparing mean pooling and max pooling, which are two prevalent methods for aggregating a sequence of embeddings into a fixed-length vector. Mean pooling averages the values across embeddings to capture a balanced representation, while max pooling selects the highest values to highlight prominent features. This analysis was performed using three Sentence-BERT models on a subset of data, and the results revealed that mean pooling generally produced superior document representations.

Following this, we expanded our investigation to include various embedding methods, encompassing both neural network-based and statistical vector-based approaches. The performance of these methods was assessed through quantitative metrics such as V-measure and median difference, as well as qualitative evaluations of clustering outputs, to assess their effectiveness in capturing semantic meaning and the quality of the clustering results.

The HH team and its agency had a prior content optimisation exercise to annotate articles for combine and rewrite. The exercise was conducted manually, and was documented in an excel file which is used as a point of reference for the study below.

## Ablation Study on Pooling Strategy

First, an ablation study on pooling strategy was conducted to identify the best method to convert a sequence of embeddings into a single fixed-length representation of each article. In the context of embeddings, mean pooling and max pooling are two common strategies used to aggregate a set of embeddings (typically vectors) into a single embedding that represents a larger text segment or document.

Mean pooling computes the average value for each dimension of the embeddings across all the input vectors. Max pooling selects the maximum value for each dimension of the embeddings across all the input vectors. When applying these pooling strategies to embeddings, the choice between mean and max pooling depends on the specific use case and the nature of the text data:

- **Mean Pooling:** Suitable for obtaining a balanced and overall representation of the text, capturing the general meaning by averaging out variations. It’s useful when the goal is to represent the entire text uniformly without emphasizing any particular part.
- **Max Pooling:** Suitable for scenarios where capturing the most salient or important features is crucial. It’s useful when key information is expected to be reflected in the highest values of the embeddings, making it effective for tasks where the most significant aspects need to be highlighted.

### Pooling Strategy: Evaluation Methodology

In the ablation study, mean and max pooling strategies were evaluated using 3 Sentence Bert models (`all-MiniLM-L6-v2`, `all-mpnet-base-v2`, `msmarco-bert-base-dot-v5`) on a subset of the reference excel file provided by HH team (20 articles). 20 x 20 confusion matrices (heatmaps) were generated and evaluated. The figure below shows an example output using `msmarco-bert-base-dot-v5` embeddings with mean pooling. The colour-coded text on each axis signifies that article titles of the same colour belong to a single group of similar articles. Ideally, confusion matrixes should form along the diagonal axis in lighter colours (signifying high correlation), while the areas away from the diagonal line should be as dark as possible (signifying low correlation).

![Figure 1. Confusion matrix / heat map generated from Dot Product Similarity between documents on `msmarco-bert-base-dot-v5` embeddings with mean pooling.](./images/heatmap.png)

Confusion Matrix / Heat Map generated from Dot Product Similarity between documents on `msmarco-bert-base-dot-v5` embeddings with mean pooling.

### Pooling Strategy: Results

A summary of the results are documented as follows:

| SBERT / Pooling Strategy   | Max Pooling                                                                                                                                                                                                                                                                                                      | Mean Pooling                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `all-MiniLM-L6-v2`         | • From the confusion matrix heatmap, the similarity distribution seems to be mostly even (a lot of values are in the 0.4 - 0.6 range). <br> •This makes distinct clusters (along the diagonal) harder to discern (only clusters at the bottom right-hand corner of the confusion matrix can be spotted clearly). | • Similarity distribution seems bimodal with the higher similarities lying on the diagonal axis of the confusion matrix heat map and lower similarities in other areas. <br> • This would suggest a more effective clustering of similar documents together.                                                                                                                                |
| `all-mpnet-base-v2`        | •Same as `all-MiniLM-L6-v2` using max pooling however, clusters along the diagonal are slightly more obvious. <br> • This would suggest that the `all-mpnet-base-v2` is a better sentence embedding model should max pooling be the aggregation strategy used.                                                   | • Results are very similar to `all-MiniLM-L6-v2` using mean pooling. In that case, it would be wise to select the model with the smaller memory footprint (i.e. `all-MiniLM-L6-v2` which is [5 times smaller](https://sbert.net/docs/sentence_transformer/pretrained_models.html#:~:text=while%20all%2DMiniLM%2DL6%2Dv2%20is%205%20times%20faster%20and%20still%20offers%20good%20quality)) |
| `msmarco-bert-base-dot-v5` | • Comparison results between max and mean pooling for this model seem to hold as well — mean pooling proved to be the better aggregation operation.                                                                                                                                                              | • Comparison results between max and mean pooling for this model seem to hold as well — mean pooling proved to be the better aggregation operation.                                                                                                                                                                                                                                         |

In this ablation study, we have compared two pooling strategies and results have shown that **mean pooling generates better document representations than max pooling**.

This means that after mean pooling aggregated across all the chunk embeddings into a single document embedding, this document embedding better encapsulates the meaning and semantic of the document/article than if we had used max pooling.

## Ablation Study on Embedding Models

Neural network-based and statistical vector-based embedding methods are two different approaches for representing words or other textual data as vectors in a continuous vector space. These embeddings are used in various natural language processing (NLP) tasks to capture the semantic meaning of words and phrases.

- Neural network-based embedding methods use neural networks to learn dense, low-dimensional vector representations of words or phrases from large corpora. These methods typically involve training on large amounts of text data to capture contextual information and semantic relationships between words.
- Statistical vector-based embedding methods rely on statistical techniques to represent words as vectors based on their distributional properties in a corpus. These methods typically involve counting word occurrences and co-occurrences and using various mathematical techniques to derive the embeddings.

The following embedding methods were explored:

| Neural Network-based                                                                                                                                                                                                                                                                                                                                                                                                       | Statistical Vector-based                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| Doc2Vec                                                                                                                                                                                                                                                                                                                                                                                                                    | Latent Semantic Analysis (LSA)                     |
| GloVe (Global Vectors for Word Representation)                                                                                                                                                                                                                                                                                                                                                                             | Latent Dirichlet Allocation (LDA)                  |
| Contextual Embeddings <br> • all-MiniLM-L6-v2 <br> • all-mpnet-base-v1 <br> • all-mpnet-base-v2 <br> • bge-large-en-v1.5 <br> • bge-large-en-v1.5-quant <br> • bge-m3 <br>• gte-base-en-v1.5 <br> • jina-embeddings-v2-base-en <br> • multi-qa-mpnet-base-cos-v1 <br> • multi-qa-mpnet-base-dot-v1 <br> • mxbai-embed-large-v1 <br> • nomic-embed-text-v1.5 <br> • snowflake-arctic-embed-m-long <br> • stsb-mpnet-base-v2 | Term Frequency-Inverse Document Frequency (TF-IDF) |
| -                                                                                                                                                                                                                                                                                                                                                                                                                          | Bag-of-words (BoW)                                 |

### Embedding Models: Evaluation Methodology

#### Embedding Models: Quantitative Analysis

Quantitative analysis of clustering performance involves evaluating the results using specific metrics that provide insights into the effectiveness of the clustering solution. Each embedding method is evaluated using the reference excel file provided by HH team as guidance with the following metrics:

1. **V-measure:** The harmonic mean of homogeneity and completeness. Higher score, the better.
   1. Homogeneity: Each cluster contains only members of a single class.
   2. Completeness: All members of a given class are assigned to the same cluster.
2. **Median difference:** Difference in correlation scores between the median of same-group articles and that of different-group articles. This is a quantitative measure of intra-cluster versus inter-cluster similarity. Higher difference, the better.

#### Embedding Models: Qualitative Analysis

Qualitative analysis of clustering output involves visual inspection of the clusters, where data points within each cluster are reviewed to determine if they align with expected groupings and whether the clusters make logical sense and are meaningful in the context of the data. Key aspects of this analysis include:

1. **Similarity of articles within clusters:** Examining whether the predicted clusters contain articles that are similar to each other. This involves checking if the articles within a cluster share common themes or characteristics, ensuring that the clustering results reflect meaningful groupings.
2. **Cluster topic specificity:** Assessing whether the topics represented by the clusters are too broad or if they can be refined into more specific sub-topics. This step helps in determining if the clusters are sufficiently detailed or if they require further segmentation to enhance their relevance.
3. **Unclustered nodes:** Evaluating if the unclustered nodes should be assigned to an existing cluster.

### Embedding Models: Results

#### Results of Quantitative Analysis

The top 5 models that had the highest V-measure score and median difference are as follow:

| Ranking | V-measure                  | Median difference          |
| ------- | -------------------------- | -------------------------- |
| #1      | nomic-embed-text-v1.5      | TF-IDF                     |
| #2      | multi-qa-mpnet-base-cos-v1 | all-mpnet-base-v2          |
| #3      | all-mpnet-base-v2          | all-MiniLM-L6-v2           |
| #4      | bge-m3                     | multi-qa-mpnet-base-cos-v1 |
| #5      | TF-IDF                     | all-mpnet-base-v1          |

Results showed that these 6 identified models outperformed the rest. Next, we conducted a qualitative analysis on the clustering output of these 6 models to evaluate the quality of the clusters they produced.

#### Results of Qualitative Analysis

The results of the qualitative analysis of the clustering output are summarized and ranked based on model performance as follows:

| Ranking | Model                      | Remarks                                                                                                                                                                                                                 |
| ------- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| #1      | nomic-embed-text-v1.5      | • Able to breakdown more broad clusters compared to #2. <br> • Able to generate specific themed clusters not seen in other models. <br> • Time taken to embed 623 articles: 45min                                       |
| #2      | all-MiniLM-L6-v2           | • Able to breakdown broad clusters in other models into specific-themed clusters, compared to #3 & #4. <br> • Unclustered nodes seen in #3 & #4 managed to form clusters <br> • Time taken to embed 623 articles: 6min  |
| #3      | multi-qa-mpnet-base-cos-v1 | • Able to produce specific-themed clusters compared to #4. Example: healthy cooking, healthy eating at hawker                                                                                                           |
| #4      | all-mpnet-base-v2          | • Produces decent clusters with similar articles clustered together. <br> • However, 2 of the clusters were noted to be too broad, and can be further broken down.                                                      |
| #5      | bge-m3                     | • A few clusters contained dissimilar topics grouped together. This is not ideal.                                                                                                                                       |
| #6      | tfidf                      | • A few clusters contained dissimilar topics grouped together. This is not ideal. <br> • Model also produced the highest number of unclustered nodes with most single nodes being similar to existing predicted topics. |

`nomic-embed-text-v1.5` and `all-MiniLM-L6-v2` embedding models were able to generate specific-themed clusters not seen in other models.

## Conclusion

In conclusion, this study evaluated pooling strategies and various embedding methods to determine the most effective approach for generating document embeddings. Key findings:

1. Mean pooling outperforms max pooling in producing more coherent document representations, capturing the general meaning of the text better.
2. Embedding models `nomic-embed-text-v1.5` and `all-MiniLM-L6-v2` generated more meaningful, specific-themed clusters, outperforming others in both quantitative and qualitative aspects.

In the downstream studies, experiments were conducted using **mean pooling strategy** and **embedding models `nomic-embed-text-v1.5` and `all-MiniLM-L6-v2`** .
