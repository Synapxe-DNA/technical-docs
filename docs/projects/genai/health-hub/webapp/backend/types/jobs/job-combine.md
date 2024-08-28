---
updated: 17 July 2024
authors: Ong Tsien Jin
---

# `Job Combine`

=== "Python"

    ```python
    class JobCombine(BaseModel):
        group_id: str
        group_name: str
        sub_group_name: str
        remarks: str
        context: str
        original_articles: List[Article]
        generated_article: Optional[GeneratedArticle]
    ```

## Attributes

### `attr` group_id

`str` The unique identifier for the group associated with the job combine.

### `attr` group_name

`str` The name of the group associated with the job combine.

### `attr` sub_group_name

`str` The name of the sub-group within the group.

### `attr` remarks

`str` Remarks or notes associated with the job combine.

### `attr` context

`str` The context or background information relevant to the job combine.

### `attr` original_articles

`List[Article]` A list of original articles involved in the job combine.

### `attr` generated_article

`Optional[GeneratedArticle]` The article generated as a result of combining the original articles.
