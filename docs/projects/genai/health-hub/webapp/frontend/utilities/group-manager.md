---
updated: 19 July 2024
authors: Ong Tsien Jin
---

# Group Manager

The core functionality of this class was designed to abstract the logic for `Articles` and their relation to "sub-groups" for harmonisation and marking for the various jobs.

!!! WARNING "Incomplete"

    Code in this class is incomplete due to the migration of V1 to V2. While it works for V1, it has not been updated to V2.

---

## @label(class) GroupManager

=== "TypeScript"

    ```
    GroupManager.constructor(cluster: Cluster)
    ```

=== "Example"

    ```
    const myGroupManager = new GroupManager(cluster)
    ```

### Attributes

#### @label(attr) $groups

`BehaviorSubject<Groups>` to keep track of groups and their respective articles.

### Methods

#### @label(meth) Get Grouping

    getGrouping(): BehaviorSubject<Groups>

Description
: Method to get current (and future) groupings for articles.

Returns
: `BehaviorSubject<Groups>`

#### @label(meth) Get Addable Grouping Names

    getAddableGroupingNames(): BehaviorSubject<string[]>

Description
: Method to get a `BehaviorSubject` of existing group names that articles can be added to for harmonisation.

Returns
: `BehaviorSubject<string[]>`

#### @label(meth) Assign Article

    assignArticle(id:string, group:string):void

Description
: Method to assign an article to a specified sub-group and removes it from its current sub-group. This method is also used when adding the article to a new sub-group.

Parameters
: id(string): Article ID
: group(string): Name of the subgroup. This includes "remove" and "ignore"

!!! WARNING "Outdated terminology"

    Parameter `group` should be termed "sub-group" instead.

#### @label(meth) Find Article Group

    findArticleGroup(id:string): string

Description
: Finds the name of the group that contains the article with the specified ID.

Parameters
: id(string): Article ID

Returns
: `string` The name of the group containing the article, or a default status if not found.

#### @label(meth) Find Article Group (Behavior Subject)

    findArticleGroupBehaviourSubject(id:string): BehaviorSubject<string>

Description
: Retrieves a BehaviorSubject that emits the current article group and updates when the group changes.

Parameters
: id(string): Article ID

Returns
: `BehaviorSubject<string>` BehaviorSubject emitting the current article group.
