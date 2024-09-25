---
updated: 24 September 2024
authors:
  - Richmond Sin
---

# Skillset

The skillset in Azure Cognitive Search handles the transformation of article content, including chunking and embedding. Below are the key skills used in the pipeline:

## 1. SplitSkill (Chunking)

- **Purpose**: Splits the content of each article into smaller chunks, making it easier to process for embedding and search.

- **Mode**: Set to `pages` for dividing the content into pages.

- **Configuration**:

  ```python
  split_skill = SplitSkill(
    text_split_mode="pages",  # Can also split by sentences
    context="/document/content",
    maximum_page_length=5000,  # Maximum characters per chunk
    page_overlap_length=200,   # Overlap between chunks
    inputs=[InputFieldMappingEntry(name="text", source="/document/content")],
    outputs=[OutputFieldMappingEntry(name="textItems", target_name="pages")])
  ```

## 2. Azure OpenAI EmbeddingSkill

- **Purpose**: Uses Azure OpenAI to generate vector embeddings for each chunk of content.

- **Model**: `text-embedding-3-small`

- **Configuration**:

  ```python
  embedding_skill = AzureOpenAIEmbeddingSkill(
  context="/document/content/pages/*",
  inputs=[InputFieldMappingEntry(name="text", source="/document/content/pages/*")],
  outputs=[OutputFieldMappingEntry(name="embedding", target_name="vector")])
  ```

## 3. Final Document Structure in the Index

- After chunking and embedding, each article document in the index will look like this:

  ```json
  {
    "id": "1434570_content",
    "title": "National Steps Challenge™ Community Challenge",
    "content": {
        "pages_0": {
            "vector": [0.123, 0.456, 0.789, ...]
        },
        ...
    }
    }
  ```

## Index Projections

The chunks and embeddings are projected into the Azure Search Index with the following structure:

| **Field**                | **Description**                                                               |
| ------------------------ | ----------------------------------------------------------------------------- |
| **parent_id**            | Identifier for the parent document or related article, if applicable.         |
| **id**                   | Primary key of the document, uniquely identifies each article.                |
| **title**                | Title of the article.                                                         |
| **cover_image_url**      | A URL pointing to an image associated with the article.                       |
| **full_url**             | A URL to the article’s full content on HealthHub.                             |
| **content_category**     | The category under which the article falls (e.g., 'programs').                |
| **category_description** | A brief description of the article category, usually found at the top.        |
| **pr_name**              | The name of the article provider or publisher (e.g., Health Promotion Board). |
| **date_modified**        | The date when the article was last modified (if any).                         |
| **chunks**               | The individual chunks of the content split by the **SplitSkill**.             |
| **embedding**            | Vector representation of each chunk, used for vector-based semantic search.   |
