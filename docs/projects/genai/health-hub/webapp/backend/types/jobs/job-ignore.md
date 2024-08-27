---
updated: 17 July 2024
authors: Ong Tsien Jin
---

# `Job Ignore`

This datatype represents a job to ignore specific articles within the system.

=== "Python"

    ```python
    class JobIgnore(BaseModel):
        article: Article
        remarks: str
    ```

## Attributes

### `attr` article

`Article` The article associated with the ignore job.

### `attr` remarks

`string` Remarks explaining why the article is ignored.
