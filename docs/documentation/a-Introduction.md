---
tags: [Documentation]
---

# Introduction

The Smokeball API conforms to [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Our API has predictable resource-oriented URLS, accepts [JSON](https://www.json.org/json-en.html) request bodies, returns [JSON](https://www.json.org/json-en.html) and uses standard [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) status codes and verbs.

If you've developed a software solution for the Legal Industry, an opportunity may exist to work with Smokeball to help our clients 'Run Their Best Firm'.

To start your integration journey with Smokeball, please follow this [link](https://smokeball.atlassian.net/servicedesk/customer/portal/3/create/13) to complete the registration of interest form, and a member from our Partnerships or Product Teams will be in touch.

## 1. Base URLs

Refer to the below base urls for different environments and regions.

### 1.1 Authentication

Use this base URL for [authentication](c916c683c136e-authentication).

|    | Production                    | Test                                      |
| -- | ----------------------------- | ----------------------------------------- |
| US | https://auth.smokeball.com    | https://datastaging-auth.smokeball.com    |
| AU | https://auth.smokeball.com.au | https://datastaging-auth.smokeball.com.au |
| UK | https://auth.smokeball.co.uk  | https://datastaging-auth.smokeball.co.uk  |

### 1.2 API

Use this base URL for [making api requests](91f30a1eaee08-making-requests).

|    | Production                   | Test                                |
| -- | ---------------------------- | ----------------------------------- |
| US | https://api.smokeball.com    | https://stagingapi.smokeball.com    |
| AU | https://api.smokeball.com.au | https://stagingapi.smokeball.com.au |
| UK | https://api.smokeball.co.uk  | https://stagingapi.smokeball.co.uk  |

## 2. Throttling & Limits

Throttling and limiting is a policy that affects the frequency an API can be called. They are put in place to protect server infrastructure from abuse or misuse. Smokeball employs rate limits to enable consistent load allocation across our platform.

The Smokeball API uses the [token bucket algorithm](https://en.wikipedia.org/wiki/Token_bucket). Smokeball API partners are assigned a "Burst" and "Rate" Limit.

The __burst__ limit defines the number of requests the  API can handle concurrently.

The __rate__ limit defines the number of allowed requests per second.

Unless otherwise specified, Smokeball API partners are assigned the limits below:

|    | Rate  | Burst |
| -- | ----- | ----- |
| US | 5     | 5     |
| AU | 5     | 5     |
| UK | 5     | 5     |