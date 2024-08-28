---
updated: 16 July 2024
authors: Ong Tsien Jin
---

# Database Connector

Database Connector is an interface that has to be implemented by classes dedicated for database interactions.

As the backend architecture is intended to be modular (closely following OOP concepts), **all** database interactions
must be abstracted by `DbConnector` interface.

The general direction for Database Connector is to be a query executor, whereby all necessary queries are handled by
the implemented class. This means that classes that implement this interface must handle parsing of inputs and outputs
to match the corresponding `Pydantic` models.

## Overview

The DbConnector interface provides an abstract class for a query engine abstraction, focusing on executing and parsing database queries tailored for specific data manipulation needs. It encapsulates methods for connecting to the database and managing various entities like articles, groups, edges, and jobs.

---

## @label(interface) DbConnector

### Initialisation

#### @label(meth) Connect

```python
async def connect() -> None
```

Description
: Initializes the database connection and completes the setup process.

### Group Operations

#### @label(meth) Create Group

```python
async def create_group_from_articles(group_name: str, article_ids: List[str]) -> str
```

Parameters
: group_name (str): The name of the group to create.
: article_ids (List[str]): List of article IDs to include in the group.

Returns
: ID of the newly created group.

#### @label(meth) Get All Groups

```python
async def get_all_groups() -> List[Group]
```

Returns
: A list of all groups, each populated with their respective ArticleMeta and Edges.

#### @label(meth) Get by ID

```python
async def get_group(group_id: str) -> Group
```

Parameters
: group_id (str): The ID of the group to retrieve.

Returns
: The specified group.

### Article Operations

#### @label(meth) Create Articles

```python
async def create_articles(articles: List[Article]) -> List[str]
```

Parameters
: articles (List[Article]): List of articles to be created in the database.

Returns
: List of IDs for the newly created articles.

#### @label(meth) Get All Articles

```python
async def get_all_articles() -> List[ArticleMeta]
```

Returns
: A list of all articles along with their metadata, excluding article contents to save on memory.

#### @label(meth) Get Articles

```python
async def get_articles(article_ids: List[str]) -> List[Article]
```

Parameters
: article_ids (List[str]): List of article IDs to fetch.

Returns
: List of articles with their content.

### Edge Operations

#### @label(meth) Create Edges

```python
async def create_edges(edges: List[Edge]) -> List[str]
```

Parameters
: edges (List[Edge]): List of edges to be created between articles.

Returns
: List of IDs for the created edges.

#### @label(meth) Get Edges

```python
async def get_edges(article_ids: List[str]) -> List[Edge]
```

Parameters
: article_ids (List[str]): List of article IDs to fetch edges for.

Returns
: List of edges between the specified articles.

### Generated Article Operations

#### @label(meth) Create Generated Articles

```python
async def create_generated_article(generated_articles: List[GeneratedArticle]) -> List[str]
```

Parameters
: generated_articles (List[GeneratedArticle]): List of generated articles to be inserted.

Returns
: List of IDs for the inserted generated articles.

### Job Operations

#### @label(meth) Create Job

```python
async def create_job(group_id:str, remove_jobs: List[str], optimise_jobs: List[str], ignore_jobs: List[str], combine_jobs: List[str]) -> str
```

Parameters
: group_id (str): ID of the associated group
: remove_jobs (List[str]): List of `RemoveJob` IDs.
: Optimise_jobs (List[str]): List of `OptimiseJob` IDs.
: ignore_jobs (List[str]): List of `IgnoreJob` IDs.
: combine_jobs (List[str]): List of `CombineJob` IDs.

#### @label(meth) Get Job

```python
async def get_job(job_id: str) -> Job
```

Parameters
: job_id

Returns
: Job

### Combine Job Operations

#### @label(meth) Create Combine Job

```python
async def create_combine_job(
        group_id: str,
        sub_group_name: str,
        article_ids: List[str],
        remarks: str = "",
        context: str = ""
) -> str
```

Parameters
: group_id (str): ID of the parent group.
: sub_group_name (str): Name of the subgroup to be combined.
: article_ids (List[str]): IDs of articles to be combined.
: remarks (str): Optional remarks for the subgroup.
: context (str): Additional context for the subgroup.

Returns
: ID of the created combine job.

#### @label(meth) Get Combine Job

```python
async def get_combine_job(job_combine_id: str) -> JobCombine
```

Parameters
: job_combine_id (str): ID of `JobCombine`

Returns
: Specified combine job record.

#### @label(meth) Get All Combine Jobs

```python
async def get_all_combine_jobs() -> List[JobCombine]
```

Returns
: List of all combine job records.

### Optimisation Job Operations

#### @label(meth) Create Optimise Job

```python
async def create_optimise_job(
    article_id: str,
    optimise_title: bool,
    optimise_meta: bool,
    optimise_content: bool,
    title_remarks: str = "",
    meta_remarks: str = "",
    content_remarks: str = ""
) -> str
```

Parameters
: article_id (str): ID of the article to be optimised.
: optimise_title (bool): True if the title needs to be optimised.
: optimise_meta (bool): True if the meta description needs to be optimised.
: optimise_content (bool): True if the content needs to be optimised.
: title_remarks (str): Optional remarks for title optimisation.
: meta_remarks (str): Optional remarks for meta optimisation.
: content_remarks (str): Optional remarks for content optimisation.

Returns
: ID of the created optimisation job.

#### @label(meth) Get Optimise Job

```python
async def get_optimise_job(job_optimise_id: str) -> JobOptimise
```

Parameters
: job_optimise_id (str): ID of the optimise job.

Returns
: Specified optimise job record.

#### @label(meth) Get All Optimise Jobs

```python
async def get_all_optimise_jobs() -> List[JobOptimise]
```

Returns
: List of all optimisation job records.

### Article Ignoring Operations

#### @label(meth) Create Ignore Job

```python
async def create_ignore_job(article_id: str) -> str
```

Parameters
: article_id (str): ID of the article to be ignored.

Returns
: ID of the created ignore job.

#### @label(meth) Get Ignore Job

```python
async def get_ignore_job(job_ignore_id: str) -> JobIgnore
```

Parameters
: job_ignore_id (str): ID of the ignore job.

Returns
: Specified ignore job record.

#### @label(meth) Get All Ignore Jobs

```python
async def get_all_ignore_jobs() -> List[JobIgnore]
```

Returns
: List of all ignore job records.

### Article Removal Operations

#### @label(meth) Create Remove Job

```python
async def create_remove_job(article_id: str, remarks: str) -> str
```

Parameters
: article_id (str): ID of the article to be removed.
: remarks (str): Remarks about why the article is being removed.

Returns
: ID of the created remove job.

#### @label(meth) Get Remove Job

```python
async def get_remove_job(job_remove_id: str) -> JobRemove
```

Parameters
: job_remove_id (str): ID of the remove job.

Returns
: Specified remove job record.

#### @label(meth) Get All Remove Jobs

```python
async def get_all_remove_jobs() -> List[JobRemove]
```

Returns
: List of all remove job records.
