---
updated: 17 July 2024
authors: Ong Tsien Jin
---

# `Job Remove`

This datatype represents a job removal request and associated metadata within the system.

=== "Python"

    ```python
    class JobRemove(BaseModel):
        id: str
        article: Article
        remarks: str
    ```

## Attributes

### `attr` id

`string` Unique identifier for the job removal request.

### `attr` article

`Article` The article associated with the job removal.

### `attr` remarks

`string` Remarks or notes associated with the job removal request.
