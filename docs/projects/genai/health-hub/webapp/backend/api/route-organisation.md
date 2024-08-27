---
updated: 17 July 2024
authors: Ong Tsien Jin
---

# Route Organisation

Routes are organised by [`subrouters`](https://fastapi.tiangolo.com/tutorial/bigger-applications/) to organise routes related to their respective categories. The following `subrouters` have been set up, but not all have been fully configured with endpoints.

- Check (for system health checks)
- Groups
- Articles
- Harmonise
- Ignore
- Optimise

!!! NOTE "Endpoints"

    Available endpoints can be found [here](endpoints.md)

???+ WARNING "Revisiting of Endpoints"

    Should this `webapp` be revisited, the logic to handle 'job' submission should be considered when designing the endpoints, taking into account the logic required to store 'job' transactions.
