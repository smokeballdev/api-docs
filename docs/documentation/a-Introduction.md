---
tags: [Documentation]
---

# Introduction

The Smokeball API conforms to [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Our API has predictable resource-oriented URLS, accepts [JSON](https://www.json.org/json-en.html) request bodies, returns [JSON](https://www.json.org/json-en.html) and uses standard HTTP status codes and verbs.

If you've developed a software solution for the Legal Industry, an opportunity may exist to work with Smokeball to help our clients 'Run Their Best Firm'.

To start your integration journey with Smokeball, please follow this [link](https://smokeball.atlassian.net/servicedesk/customer/portal/3/create/13) to complete the registration of interest form, and a member from our Partnerships or Product Teams will be in touch.

### Base URLs

Refer to the below base urls for different environments and regions.

### 1. Authentication

Use this base URL for [authentication](3-Authentication.md).

|     | Production                    | Test                                      |
| --- | ----------------------------- | ----------------------------------------- |
| US  | https://auth.smokeball.com    | https://datastaging-auth.smokeball.com    |
| AU  | https://auth.smokeball.com.au | https://datastaging-auth.smokeball.com.au |

### 2. API

Use this base URL for [making api requests](4-Making-Requests.md).

|     | Production                   | Test                                |
| --- | ---------------------------- | ----------------------------------- |
| US  | https://api.smokeball.com    | https://stagingapi.smokeball.com    |
| AU  | https://api.smokeball.com.au | https://stagingapi.smokeball.com.au |
