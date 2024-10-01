---
updated: 1 October 2024
authors: Lyndon Lim
---

# @label(type) Source

Source pydantic model contains parameters of an article retrieved from Azure AI Search
It does not contain the article text chunk.

=== "Python"

    ```python
    class Source(BaseModel):
        ids: List[str]
        title: str
        cover_image_url: str
        full_url: str
        content_category: str
        category_description: str
        pr_name: str
        date_modified: str
    ```

## Attributes

### @label(attr) ids

`List[string]` List of ids of the article. (Note: This is not the parent id, it is the 'sub' id after indexing)

### @label(attr) title

`string` Title of the article.

### @label(attr) cover_image_url

`string` URL of the cover image of the article.

### @label(attr) full_url

`string` URL of the full article.

### @label(attr) content_category

`string` Content category of the article.

### @label(attr) category_description

`string` Category description of the article.

### @label(attr) pr_name

`string` Publisher name of the article.

### @label(attr) date_modified

`string` Date the article was last modified.
