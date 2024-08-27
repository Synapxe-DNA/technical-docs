---
updated: 16 July 2024
authors: Lyndon Lim
---

# Mongo Connector

Mongo Connector is a concrete class that implements all the abstract method of the Database Connector that is dedicated for intereactions with MongoDB

All necessary MongoDB queries are handled by this class. This class also handle parsing of inputs and outputs to match the corresponding `Pydantic` models.

---

## Overview

This Python file defines a MongoConnector class that extends a DbConnector base class, specifically tailored for interacting with a MongoDB database using asynchronous operations. It provides methods to connect to the database, initialize the Beanie ODM with specific document models, and perform CRUD operations on various entities such as articles, groups, jobs, and edges. The class includes functionality to create and retrieve articles, groups, and jobs (tasks related to articles), as well as to manage relationships between articles through edges. The connector also converts MongoDB documents into Python objects for easier manipulation within the application.

## Methods

### Connection

- **connect**

```python
async def connect() -> None
```

Description: Initializes the connection to MongoDB and completes the setup process.

### Group Operations

- **create_group_from_articles**

```python
async def create_group_from_articles(group_name: str, article_ids: List[str]) -> str
```

Parameters:

- group_name (str): The name of the group to create.
- article_ids (List[str]): List of article IDs to include in the group.

Description:
Returns: ID of the newly created group.

- **get_all_groups**

```python
async def get_all_groups() -> List[Group]
```

Returns: A list of all `Group`, each populated with their respective `ArticleMeta` and `Edge`.

- **get_group**

```python
async def get_group(group_id: str) -> Group
```

Parameters:

- group_id (str): The ID of the group to retrieve.

Returns: A `Group` representing the specified group.

### Article Operations

- **create_articles**

```python
async def create_articles(articles: List[Article]) -> List[str]
```

Parameters:

- articles (List[Article]): List of `Article` to be created in the database.

Returns: List of IDs for the newly created articles.

- **get_all_articles**

```python
async def get_all_articles() -> List[ArticleMeta]
```

Returns: A list of all `Article` along with their metadata, excluding article contents to save on memory.

- **get_articles**

```python
async def get_articles(article_ids: List[str]) -> List[Article]
```

Parameters:

- article_ids (List[str]): List of article IDs to fetch.

Returns: List of `Article` with their content.

### Edge Operations

- **create_edges**

```python
async def create_edges(edges: List[Edge]) -> List[str]
```

Parameters:

- edges (List[Edge]): List of `Edge` to be created between articles.

Returns: List of IDs for the created edges.

- **get_edges**

```python
async def get_edges(article_ids: List[str]) -> List[Edge]
```

Parameters:

- article_ids (List[str]): List of article IDs to fetch edges for.

Returns: List of `Edge` between the specified articles.

### Generated Article Operations

- **create_generated_article**

```python
async def create_generated_article(generated_articles: List[GeneratedArticle]) -> List[str]
```

Parameters:

- generated_articles (List[GeneratedArticle]): List of `GeneratedArticle` to be inserted.

Returns: List of IDs for the inserted generated articles.

### Job Operations

- **create_job**

```python
async def async def create_job(
        self,
        group_id: str,
        remove_jobs: List[str],
        optimise_jobs: List[str],
        ignore_jobs: List[str],
        combine_jobs: List[str],
    ) -> str
```

Description: This function is triggered upon user submission to create a new job entry in the database.
It updates the associated group's job attribute to ensure that the group is linked to only the most recent job.

Parameters:

- group_id (str): ID of group that this job is created for
- remove_jobs (List[str]): List of article IDs to be marked as removed
- optimise_jobs (List[str]): List of article IDs to be marked as optimised
- ignore_jobs (List[str]): List of article IDs to be marked as ignored
- combine_jobs (List[str]): List of article IDs to be marked as combine

Returns: ID of newly create job

- **get_job**

```python
async def get_job(self, job_id: str) -> Job
```

Parameters:

- job_id (str): The ID of the job to retrieve.

Returns: The specified `Job`.

### Job Combine Operations

- **create_combine_job**

```python
async def create_combine_job(
    self,
    group_id: str,
    sub_group_name: str,
    article_ids: List[str],
    remarks: str = "",
    context: str = "",
) -> str
```

Parameters:

- group_id (str): ID of group that this combine job was created from
- sub_group_name (str): Name of subgroup
- article_ids (List[str]): List of article IDs to be added in this subgroup
- remarks (str): Remarks for the generated article from this job given by the user [Optional]
- context (str): Context given by the user to add on to the harmonisation process [Optional]

Returns: ID of newly create combine job

Note: Checks are implemented to check if a combine job with the same values are already present in the database. If so, the existing combine job ID is returned instead of creating a new one.

- **get_combine_job**

```python
async def get_combine_job(self, job_combine_id) -> JobCombine
```

Parameters:

- job_combine_id (str): ID of the combine job to retrieve.

Reutrns: The specified `JobCombine`.

- **get_all_combine_jobs**

```python
async def get_all_combine_jobs() -> List[JobCombine]
```

Returns: A list of all `JobCombine` entries.

### Job Optimise Operations

- **create_optimise_job**

```python
    async def create_optimise_job(
        self,
        article_id: str,
        optimise_title: bool,
        optimise_meta: bool,
        optimise_content: bool,
        title_remarks: str = "",
        meta_remarks: str = "",
        content_remarks: str = "",
    ) -> str
```

Parameters:

- article_id (str): ID of article to be optimised
- optimise_title (bool): Flag to indicate if title should be optimised
- optimise_meta (bool): Flag to indicate if meta description should be optimised
- optimise_content (bool): Flag to indicate if content should be optimised
- title_remarks (str): Remarks for the optimised title given by the user [Optional]
- meta_remarks (str): Remarks for the optimised meta description given by the user [Optional]
- content_remarks (str): Remarks for the optimised content given by the user [Optional]

Returns: ID of newly created optimise job

Note: Checks are implemented to check if an optimise job with the same values are already present in the database. If so, the existing optimise job ID is returned instead of creating a new one.

- **get_optimise_job**

```python
async def get_optimise_job(self, job_optimise_id) -> JobOptimise
```

Parameters:

- job_optimise_id (str): ID of the optimise job to retrieve.

Returns: The specified `JobOptimise`.

- **get_all_optimise_jobs**

```python
async def get_all_optimise_jobs(self) -> List[JobOptimise]:
```

Returns: A list of all `JobOptimise` entries.

### Job Ignore Operations

- **create_ignore_job**

```python
async def create_ignore_job(self, article_id: str) -> str
```

Parameters:

- article_id (str): ID of article to be ignored

Returns: ID of newly created ignore job

Note: Checks are implemented to check if an ignore job with the same values are already present in the database. If so, the existing ignore job ID is returned instead of creating a new one.

- **get_ignore_job**

```python
async def get_ignore_job(self, job_ignore_id) -> JobIgnore:
```

Parameters:

- job_ignore_id (str): ID of the ignore job to retrieve.

Returns: The specified `JobIgnore`.

- **get_all_ignore_jobs**

```python
async def get_all_ignore_jobs(self) -> List[JobIgnore]:
```

Returns: A list of all `JobIgnore` entries.

### Job Remove Operations

- **create_remove_job**

```python
async def create_remove_job(self, article_id: str, remarks: str) -> str
```

Parameters:

- article_id (str): ID of article to be removed
- remarks (str): Remarks for the removing the article given by the user

Returns: ID of newly created remove job

- **get_remove_job**

```python
async def get_remove_job(self, job_remove_id) -> JobRemove
```

Parameters:

- job_remove_id (str): ID of the remove job to retrieve.

Returns: The specified `JobRemove`.

- **get_all_remove_jobs**

```python
async def get_all_remove_jobs(self) -> List[JobRemove]
```

Returns: A list of all `JobRemove` entries.

### Helper functions

- **\_\_getArticleSimilarity**

```python
async def __getArticleSimilarity(articleDoc: ArticleDocument) -> float
```

Parameters:

- articleDoc (ArticleDocument): The article document to get the edge with the highest similarity

Returns: The highest similarity score from the edges for a given article.
