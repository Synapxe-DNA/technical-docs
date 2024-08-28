---
updated: 12 August 2024
authors:
  - Ho Si Xian
  - Joycelyn Woo
---

# Overview

The clustering pipeline contains 5 nodes:

1. @label(func) `merge_ground_truth_to_data`: To merge reference excel from HH team into the processed dataset
2. @label(func) `generate_clusters`: To generate first-level clusters using Louvain algorithm
3. @label(func) `generate_subclusters`: To generate subclusters for those with cluster size > 10 using BERTopic
4. @label(func) `update_edges_dataframe`: To update first-level edge dataframe output from `generate_clusters` node
5. @label(func) `cluster_viz`: To visualise the clusters using pyvis
