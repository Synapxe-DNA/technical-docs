---
updated: 23 September 2024
authors:
  - Richmond Sin
---

# Azure RAG Data Processing

## Introduction

Our primary goal is to prepare the data (in JSON format) for the article ingestion in Microsoft Azure Search Index. The pipeline is branched out from the merged node in `data_processing`, and it converts the post processed parquet file into individual JSON file for each article. This enables the article content to be ingested into the search index for context retrieval in the Retrieval Augmented Generation (RAG) process for the app.

In this pipeline, we focus on the following:

1. Filtering the articles according to their `remove_type`.
2. Process the `content_body` column into the new `processed_table_content` for articles with tables.
3. Extracting the content and their table content from the parquet file into individual JSON files.

## Setting up the Project

1. No additional files required for this pipeline.
2. Refer to the [`README.md`](https://github.com/Synapxe-DNA/healthhub-content-optimization/tree/main/content-optimization) to setup and run the Kedro Pipeline.

!!! Note

    We are interested in running the `azure_rag` pipeline. To do so, run the following command after setting up the virtual environment and its dependencies -
    ```python
    cd content-optimization
    # Run the data processing pipeline to get the merged data
    kedro run --pipeline="data_processing"
    # Run the azure_rag pipeline to process the merged data
    kedro run --pipeline="azure_rag"
    ```

## Adding new Kedro nodes

When adding new kedro nodes, it is important to understand the key processes that accompany it. You will usually add the filepath(s) to the Data Catalog, supplement the required parameters and write your functions.

### [`Data Catalog`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/catalog.yml)

The Data Catalog is one of the most important files in the Kedro Pipeline. It indicates where your files are located in the project. Kedro handles the loading and saving of data on your behalf. You do not need to specify it in your nodes.

Here is an example of how to define it:

```yaml
# Variable Name/Dictionary Key
filtered_data_rag:
  # File Type - Refer to kedro-datasets (https://docs.kedro.org/projects/kedro-datasets/en/kedro-datasets-4.1.0/api/kedro_datasets.html) for the appropriate data connector
  type: pandas.ParquetDataset
  # File Path - Indicate where to save/load the desired file
  filepath: data/03_primary/filtered_data.parquet
  # Data Versioning - Indicate whether you want to only obtain the latest file or track the changes (via timestamp)
  versioned: true
```

Once you have defined your variables, you can use them in your Kedro pipeline.

### [`Parameters`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/conf/base/parameters_azure_rag.yml)

The Parameters are used to store any static definition of variables in the Kedro Pipeline. Each Pipeline has its own set of parameters. We do not hardcode any values in the code. We retrieve it from this file.

Here is an example of the defining the duplicated article ids to filter out -

```yaml
# Parameter for the function in nodes.py and pipeline.py
duplicated_articles:
  # List defining the article ids that we are interested in
  - 1445629
  - 1443608
  - 1435183
  - 1435335
  - 1434652
```

After defining your data variables and the parameters required for your function, we can proceed to the writing our function.

### [`Nodes`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/src/content_optimization/pipelines/azure_rag/nodes.py)

Here is where we define our functions to transform our data. A node is merely a step within the Kedro Pipeline that performs a set of transformations. When writing your node functions, you should do the following -

1. Name your node as an `action` (i.e. it should start with a verb).
2. Keep your implementation succinct. If your function requires a multiple functions or has a long implementation, you should consider creating a new Python file and import the main function over to `nodes.py`.
3. Ensure that your input and output variables can be found either in your Data Catalog `catalog.yml` or Parameters `parameters_data_processing.yml`. We prefer to use the same variable names as defined in these configuration files for better clarity.

Once we have created the node(s), we can now integrate them into the Kedro Pipeline.

### [`Pipeline`](https://github.com/Synapxe-DNA/healthhub-content-optimization/blob/main/content-optimization/src/content_optimization/pipelines/azure_rag/pipeline.py)

This is the easiest part of the Kedro pipeline. To integrate the new node, just add a new node function to the pipeline.

!!! WARNING

    The nodes in the Kedro pipeline are ordered. Do not mess the order of these nodes as we are generating the files dynamically from the input files. Each node function has dependencies that are generated by preceding nodes.

Here is an example -

```python
def create_pipeline(**kwargs) -> Pipeline:
    return pipeline(
        [
            node(
                func=filter_articles,
                inputs=[
                    "merged_data",
                    "params:duplicated_articles",
                    "params:duplicated_content",
                    "params:lengthy_articles",
                ],
                outputs="filtered_data_rag",
                name="filter_articles_node",
            ),
            node(
                func=process_html_tables,
                inputs=[
                    "filtered_data_rag",
                    "params:temperature",
                    "params:max_tokens",
                    "params:n_completions",
                    "params:seed",
                ],
                outputs="processed_data_rag",
                name="process_html_tables_node",
            ),
            node(
                func=extract_content,
                inputs=[
                    "processed_data_rag",
                    "params:article_content_columns",
                    "params:table_content_columns",
                ],
                outputs="json_data_rag",
                name="extract_content_node",
            ),
        ]
    )
```
