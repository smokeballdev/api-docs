---
tags: [Documentation]
---

# Introduction

The Smokeball API conforms to [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Our API has predictable resource-oriented URLS, accepts [JSON](https://www.json.org/json-en.html) request bodies, returns [JSON](https://www.json.org/json-en.html) and uses standard HTTP status codes and verbs.

### Base URLs

Refer to the below base urls for different environments and regions.

### 1. Authentication

Use this base URL for [authentication](3-Authentication.md)

|                                                                                                                     | Production                    | Test                                      |
| ------------------------------------------------------------------------------------------------------------------- | ----------------------------- | ----------------------------------------- |
| <img src="https://raw.githubusercontent.com/smokeballdev/api-docs/master/assets/svg/us.svg" width="20" height="20"> | https://auth.smokeball.com    | https://datastaging-auth.smokeball.com    |
| <img src="https://raw.githubusercontent.com/smokeballdev/api-docs/master/assets/svg/au.svg" width="20" height="20"> | https://auth.smokeball.com.au | https://datastaging-auth.smokeball.com.au |

### 2. API

Use this base URL for [making api requests](4-Making-Requests.md)

|                                                                                                                     | Production                   | Test                                |
| ------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ----------------------------------- |
| <img src="https://raw.githubusercontent.com/smokeballdev/api-docs/master/assets/svg/us.svg" width="20" height="20"> | https://api.smokeball.com    | https://stagingapi.smokeball.com    |
| <img src="https://raw.githubusercontent.com/smokeballdev/api-docs/master/assets/svg/au.svg" width="20" height="20"> | https://api.smokeball.com.au | https://stagingapi.smokeball.com.au |
