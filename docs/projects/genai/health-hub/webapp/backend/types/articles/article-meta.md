---
updated: 17 July 2024
authors: Ong Tsien Jin
---

# @label(type) Article Meta

This datatype contains the high level attributes of articles in the system.

=== "Python"

    ```python
    class ArticleMeta(BaseModel):
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
    ```

## Attributes

### @label(attr) id

`string` Unique identifier for the article.

### @label(attr) title

`string` Title text of the article.

### @label(attr) description

`string` SEO description of the article.

### @label(attr) date_modified

`string` The date when the article was last modified.

### @label(attr) similarity

`float` A numerical value representing how similar this article is to others.

### @label(attr) keywords

`List[str]` A list of keywords associated with the article.

### @label(attr) labels

`List[str]` A list of labels or tags associated with the article.

### @label(attr) pr_name

`string` The name of the content author for the article.

### @label(attr) content_category

`string` The category of content the article belongs to.

### @label(attr) url

`string` The URL where the article can be accessed.

### @label(attr) cover_image_url

`string` The URL of the cover image for the article.

### @label(attr) status

`string` The status of the article.

### @label(attr) engagement_rate

`float` The engagement rate of the article, typically a percentage.

### @label(attr) number_of_views

`int` The number of times the article has been viewed.
