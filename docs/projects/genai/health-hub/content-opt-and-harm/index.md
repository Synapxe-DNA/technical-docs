---
updated: 26 August 2024
authors:
  - Issac J
  - Jin Chou
---

## Introduction

Our primary objective is to reduce the workload in the manual review process. It aims to identify and optimise the articles across three different areas -

1. Article Content
2. Article Title
3. Article Meta Description

## Workflow

### Optimisation Workflow

![Optimisation Checks & Rewriting Workflow](./images/article_optimisation_flow.jpg)
For individual articles, the initial steps of the Article Rewriting Workflow is to evaluate the quality of the article based on its content, title and meta description. We utilise both Rules, Statistics and Large Language Models (LLMs) to perform these evaluations.
Then, these evaluations are later shared to the users for approval/refinement. Finally, the selected articles (together with their evaluations) are passed down to the `Article Optimisation Flow` to refine and optimise the articles.

### Harmonisation Workflow

![Article Harmonisation Workflow](./images/article_harmonisation_flow.jpg)
For grouped articles, the user will annotate articles to harmonise in the User Annotation Excel file. Following that, these artilces will undergo Article Harmonisation.

## Project Setup

### Setting up for Optimisation Checks
1. Run the Data Processing Pipeline via Kedro. Refer to the [`README.md`](https://github.com/Synapxe-DNA/healthhub-content-optimization/tree/main/content-optimization) in `content-optimization` for more information.
2. Fetch the `merged_data.parquet` file from the [`content-optimization/data/03_primary`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/data/03_primary) directory
3. Generate the `ids_for_optimisaton.csv` files by referring to the [`exclude_articles.ipynb`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/notebooks/exclude_articles.ipynb) notebook.
4. Add these 2 files into the [`data`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/data) subdirectory in the `article-harmonisation` project.
5. Run the [`checks.py`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/checks.py) Python file to execute the Optimisation Checks workflow.

Refer to the [`README.md`](https://github.com/Synapxe-DNA/healthhub-content-optimization/tree/main/article-harmonisation#instruction-to-run-the-project) in `article-harmonisation` for more information.

### Setting up for Article Rewriting

1. Run the Data Processing Pipeline via Kedro. Refer to the [`README.md`](https://github.com/Synapxe-DNA/healthhub-content-optimization/tree/main/content-optimization) in `content-optimization` for more information.
2. Fetch the `merged_data.parquet` file from the [`content-optimization/data/03_primary`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/data/03_primary) directory
3. Add the `merged_data.parquet` file into the [`data`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/article-harmonisation/data) subdirectory in the `article-harmonisation` project.
4. Go to the [Step 1 Harmonisation and Optimisation Checks](https://drive.google.com/drive/folders/1ywUNxLDkNLVaYlawjZrz8CX_fKbf5joz) subdirectory within [User Annotation](https://drive.google.com/drive/folders/1kuTAWvOvBRAVP0wmJIjIXdTP9X77j3lI) directory.
5. Download [Stage 1 user annotation for HPB_sample.xlsx](https://docs.google.com/spreadsheets/d/1-cSXf1ZWmi_p6nr_lCfGl0_F6XSnWpvn/edit?usp=drive_link) and store it in `article-harmonisation/data/article_rewriting/` in the `article-harmonisation` project.

Refer to the [`README.md`](https://github.com/Synapxe-DNA/healthhub-content-optimization/tree/main/article-harmonisation#instruction-to-run-the-project) in `article-harmonisation` for more information.