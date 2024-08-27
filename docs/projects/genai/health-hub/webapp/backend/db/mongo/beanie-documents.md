---
updated: 16 July 2024
authors: Lyndon Lim
---

# Beanie Documents

This file contains the Beanie document types used to interacting with the DB.

> **_NOTE:_** In Beanie, a Link is a special field type used to create a reference between two document types in a MongoDB database. It allows for the establishment of relationships between documents, similar to foreign keys in relational databases.

## Group Document

```python
class GroupDocument(Document):
    name: str
    articles: List[Link["ArticleDocument"]]
    job: Link["JobDocument"] = None
```

Description: A Group refers to a cluster of similar articles.

Class variables:

- `name`: The name of the group.
- `articles`: A list of articles that belong to the group.
- `job`: The **latest** job associated with the group.
