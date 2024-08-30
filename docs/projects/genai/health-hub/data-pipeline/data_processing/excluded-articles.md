---
updated: 27 August 2024
authors:
  - Ho Si Xian
---

# Excluded Articles

## Overview

Certain articles were excluded from clustering due to various data issues flagged by the `data_processing` pipeline, such as duplicated content or being categorized as recipes. Additionally, some were excluded based on user annotations (see section: User Annotation (HH Team))

## Remove Type Data Count

The table below reflects the data count for each remove types for HPB articles:

|                                                     to_remove categories                                                      | HPB only to_remove articles Counts (final) |                              Flagged/Annotated by                               |
| :---------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------: | :-----------------------------------------------------------------------------: |
|                                                      Duplicated Content                                                       |                     1                      |                       Flagged by data_processing pipeline                       |
|                                                        Duplicated URL                                                         |                     2                      |                       Flagged by data_processing pipeline                       |
|                                                            Recipe                                                             |                     45                     | Flagged by data_processing pipeline + Annotated by HH and included in blacklist |
|                                                       Table of Contents                                                       |                     4                      |                    Annotated by HH and included in blacklist                    |
|                                             No relevant content and mainly links                                              |                     6                      |                    Annotated by HH and included in blacklist                    |
|                                                       Service Directory                                                       |                     3                      |                    Annotated by HH and included in blacklist                    |
|                                                          Infographic                                                          |                     14                     |                    Annotated by HH and included in blacklist                    |
| LLM is unable to process content due to Azure's content filtering on hate, sexual, violence, and self-harm related categories |                     1                      |                             Annotated in blacklist                              |
|                                                             TOTAL                                                             |                     76                     |                                                                                 |

### Excluded content

The `excluded_content_tab`(in Google Drive under `LLM Exploration/Health Hub/User Annotation/Step 1 Harmonisation and Optimisation Checks`) file contains articles that should be placed in the 'excluded articles' tab of user annotation Excel sheet. This file is generated using the `content-optimization/notebooks/exclude_articles.ipynb` notebook.

Articles flagged for duplicated URLs or content will be excluded from this tab, as they are considered backend data issues. Specifically, this includes one article (Article ID: 1445972) marked for removal due to "duplicated content," two articles (Article IDs: 1444417, 1445629) flagged for "duplicated URLs," and one article (Article ID: 1445829) identified as a "recipe." The last article was flagged as a recipe first due to the ordering of flags in the data processing pipeline, even though it also had duplicated content.

### User Annotation (HH Team)

The HH team conducted two rounds of annotation to assess the quality of the clusters generated during the content optimisation process. The annotations were performed using outputs from `content-optimization/notebooks/create_annotation_excel_final.ipynb` notebook, which generates `user_annotation` files to be sent to HH team for evaluation.

!!! Note

    The `final_pred_cluster` file is generated from the first iteration of clustering. Since clustering is dynamic, the clustering results changes with every iteration.

### First Round of Annotation

Annotation Process

- The HH Team annotated the "User Annotation (To Combine)" tab within the `user_annotation` file.
- Articles labelled as "Individual" under the "Action" tab, and those with remarks in "algorithm remarks" column, were shifted to "User Annotation (To Optimise)" tab. This includes:

  - 66 articles labelled as "Individual" by HH team
  - 212 articles under `final_pred_cluster`(in Google Drive under `LLM Exploration/Health Hub/Clustering/Final Clustering Result` folder) from the first iteration.

Output

- The result of this annotation round was saved as `user_annotation_25jul.xlsx`(in Google Drive under `LLM Exploration/Health Hub/User Annotation` folder)

### Second Round of Annotation

Annotation Process

- The HH team annotated the "User Annotation (To Optimise)" tab based on the first round's output
- Articles marked with comments containing "exclude" under the "user_annotation [HH's team's comments]" were removed from optimisation

Output

- The result of this annotation was saved as `Stage 1 user annotation for HPB_HHcomments_20Aug24.xlsx`(in Google Drive under `LLM Exploration/Health Hub/User Annotation/Step 1 Harmonisation and Optimisation Checks` folder)
