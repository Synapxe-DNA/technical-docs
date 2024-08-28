---
updated: 17 July 2024
authors: Ong Tsien Jin
---

# `Job Optimise`

This datatype represents a job for optimising various aspects of an article within the system.

=== "Python"

    ```python
    class JobOptimise(BaseModel):
        id: str
        optimise_title: bool
        title_remarks: str
        optimise_meta: bool
        meta_remarks: str
        optimise_content: bool
        content_remarks: str

        original_article: Article
        generated_article: Optional[GeneratedArticle]
    ```

## Attributes

### `attr` id

`string` Unique identifier for the job.

### `attr` optimise_title

`bool` Indicates whether the title of the article should be optimised.

### `attr` title_remarks

`string` Remarks about the title optimisation.

### `attr` optimise_meta

`bool` Indicates whether the metadata of the article should be optimised.

### `attr` meta_remarks

`string` Remarks about the metadata optimisation.

### `attr` optimise_content

`bool` Indicates whether the content of the article should be optimised.

### `attr` content_remarks

`string` Remarks about the content optimisation.

### `attr` original_article

`Article` The original article before optimisation.

### `attr` generated_article

`Optional[GeneratedArticle]` The article generated after optimisation, if available.
