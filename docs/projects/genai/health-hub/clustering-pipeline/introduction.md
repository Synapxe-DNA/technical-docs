---
updated: 12 August 2024
authors:
  - Ho Si Xian
  - Joycelyn Woo
---

# Introduction

The clustering pipeline contains 5 nodes:

1. `merge_ground_truth_to_data`: To merge reference excel from HH team into the processed dataset
2. `generate_clusters`: To generate first-level clusters using Louvain algorithm
3. `generate_subclusters`: To generate subclusters for those with cluster size > 10 using BERTopic
4. `update_edges_dataframe`: To update first-level edge dataframe output from `generate_clusters` node
5. `cluster_viz`: To visualise the clusters using pyvis
