---
tags: [Documentation]
---

# Sorting

### 1. Overview

Some endpoints will have a query parameter called `sort` _(array[[string]])_ which allows you to specify one or more comma-separated sort expressions. Sorting will be applied in the order in which they are provided.

In the Swagger documentation for an endpoint that supports sorting, the description will detail what fields can be used for sorting.

This **GET** Matters endpoint supports sorting by *status* and *lastUpdated*.

### 2. Syntax

The basic syntax for a sort expressions is:

```
sort=[asc/desc]([field])
```

Example expression| Description 
---------|----------
 asc(status) | Will do an ascending sort on matter *status*
 desc(lastUpdated) | Will do a descending sort on matter *lastUpdated* date  
 
#### Examples:

If you wanted to sort by last updated matters (in descending order):
```
GET ./matters?sort=desc(lastUpdated)
```

If you wanted to first sort matters by status (in ascending order) and then by last updated date (in descending order):
```
GET ./matters?sort=asc(status),desc(lastUpdated)
```
