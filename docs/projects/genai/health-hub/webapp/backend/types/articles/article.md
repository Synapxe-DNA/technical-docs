---
updated: 17 July 2024
author: Ong Tsien Jin
---

# `Article`

Article datatype contains meta information (like `ArticleMeta`), together with the content of the article.

=== "Python"

    ```python
    class Article(BaseModel):
        id: str = Field(default="")
    
        title: str
        description: str
        date_modified: str = Field(default="")
        similarity: float = Field(default=-1)
     
        keywords: List[str] = Field(default=[])
        labels: List[str] = Field(default=[])

        pr_name: str
        content_category: str
        url: str = Field(default="")
        cover_image_url: str = Field(default="")
    
        status: str = Field(default="")
        engagement_rate: float = Field(default=-1.0)
        number_of_views: int = Field(default=-1)

        content: str = Field(default="")
    ```

=== "Actual Implementation"

    The actual implementation of this class inheirits `ArticleMeta`.

    ```python
    class Article(ArticleMeta):
        content: str = Field(default="")
    ```

## Attributes

### `attr` id
`string` Unique identifier for the article.

### `attr` title
`string` Title text of the article.

### `attr` description
`string` SEO description of the article.

### `attr` date_modified
`string` The date when the article was last modified.

### `attr` similarity
`float` A numerical value representing how similar this article is to others.

### `attr` keywords
`List[str]` A list of keywords associated with the article.

### `attr` labels
`List[str]` A list of labels or tags associated with the article.

### `attr` pr_name
`string` The name of the content author for the article.

### `attr` content_category
`string` The category of content the article belongs to.

### `attr` url
`string` The URL where the article can be accessed.

### `attr` cover_image_url
`string` The URL of the cover image for the article.

### `attr` status

`string` The status of the article.

### `attr` engagement_rate
`float` The engagement rate of the article, typically a percentage.

### `attr` number_of_views
`int` The number of times the article has been viewed.

### `attr` content
`string` The article content extracted as a string.