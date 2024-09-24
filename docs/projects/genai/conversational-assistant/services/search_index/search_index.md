---
updated: 24 September 2024
authors:
  - Richmond Sin
---

# Search Index

Azure Cognitive Search is a powerful search service for building fast, scalable, and full-text search solutions. In this project, we use Azure Cognitive Search to index and retrieve articles from HealthHub, enabling semantic search based on vector embeddings.

## Azure Search Components

### SearchClient

- **Purpose**: The `SearchClient` is responsible for handling individual document operations within the search index.
- **Functions**:
  - **Add Documents**: Upload documents one by one or in batches to the search index.
  - **Delete Documents**: Remove documents from the search index by their unique identifier.
  - **Search Documents**: Execute full-text searches and vector-based searches across the indexed data.

### SearchIndexClient

- **Purpose**: The `SearchIndexClient` is used to manage the search index itself, handling tasks related to the creation, updating, or deletion of indexes.
- **Functions**:
  - **Create Index**: Define the schema (fields, types, and attributes) of a new index.
  - **Update Index**: Modify the schema or settings of an existing index.
  - **Delete Index**: Remove the entire index from the service.

### SearchIndexerClient

- **Purpose**: The `SearchIndexerClient` is responsible for managing automated indexing pipelines that can ingest data from external sources like Blob Storage or databases.
- **Functions**:
  - **Data Source Integration**: Configure data sources (e.g., Azure Blob Storage) for automatic indexing.
  - **Run Indexer**: Schedule or trigger the indexing process that pulls data from the source and updates the search index.
  - **Monitor Indexing Jobs**: Check the status of indexing jobs and review logs for potential errors.

## Search Fields

Azure Cognitive Search fields are defined with attributes that determine how the data is used in searches, filtering, sorting, and faceting. Below is an overview of the key fields used in our search index.

## Key Fields in the Search Index

- **id**: The primary key for each document.
  - `searchable: true`
  - `filterable: true`
  - `sortable: true`
  - `facetable: true`
  - `retrievable: true`

- **title**: The title of the article.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`
  - `analyzer_name: en.microsoft`

- **cover_image_url**: URL pointing to the article’s cover image.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`

- **full_url**: URL to the full article on HealthHub.
  - `searchable: false`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`

- **content_category**: The category of the article (e.g., 'programs').
  - `searchable: true`
  - `filterable: true`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`
  - `analyzer_name: en.microsoft`

- **category_description**: Description of the category.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`
  - `analyzer_name: en.microsoft`

- **pr_name**: The name of the article provider.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: false`

- **date_modified**: The last edited date of the article.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: false`

- **chunks**: The individual chunks of the article content created by the `SplitSkill`.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`
  - `analyzer_name: en.microsoft`

- **embedding**: The vector representation of the article content, generated using Azure OpenAI.
  - `searchable: true`
  - `filterable: false`
  - `sortable: false`
  - `facetable: false`
  - `retrievable: true`
  - `vector_search_dimensions: 1536`

## Search Field Attributes

Each field in the index can be configured with specific attributes that determine how it is used in searches, filters, sorting, or faceting:

- **searchable**: Allows full-text search on the field.
- **filterable**: Enables filtering on exact matches or ranges (e.g., by date or category).
- **sortable**: Allows sorting of results based on the values in this field.
- **facetable**: Supports faceted navigation by creating categories or filters from this field.
- **retrievable**: Specifies whether the field’s content can be retrieved and returned in search results.
