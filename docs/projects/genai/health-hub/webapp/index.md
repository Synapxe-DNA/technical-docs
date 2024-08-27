---
updated: 17 July 2024
authors:
  - Ong Tsien Jin
---

# Introduction (@label(deprecated))

## Purpose of Webapp

This webapp was created with the intention to allow content creators and stakeholders to annotate similar articles for clustering (grouping) and optimisation. Article harmonisation and optimisation would then happen asynchronously after exporting the annotated articles.

!!! NOTE "Latest branch on GitHub"

    The branch containing the latest backend can be found on the [webapp](https://github.com/Wilsven/healthhub-content-optimization/tree/webapp) branch.

    ??? WARNING "Disclaimer"

        At the time of writing, `webapp` is in a rough stage as development was halted half way due to a change in priority. The development stage of the `backend` is slightly better developed as compared to the `frontend`.

## Project Structure

The file structure for the webapp is split into the `frontend` and `backend` subfolders, each with its own subdirectory called `app`.

    .
    ├── backend
    │   └── app
    └── frontend
        └── app

Other project files pertaining to content optimisation and harmonisation are also located in the root directory.
