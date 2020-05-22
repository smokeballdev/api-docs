---
tags: [Documentation]
---

# 2. Making Requests

All requests to the API require a valid `api-key` issued by Smokeball and `id_token` (see [Authorization](Authentication.md)).

### Example Request

```json http
{
  "method": "get",
  "url": "https://stagingapi.smokeball.com/contacts/",
  "headers": {
    "x-api-key": "Y2xpZW50X2lkOmNsaWV",
    "Authorization": "Bearer Y2xpZW50X2lkOmNsaWV"
  }
}
```

**Response**




### Paged Responses

You will notice that some requests will have the optional parameters `limit` and `offset`. These are used to control how many results are returned in a request that returns a list of results.

An example respoonse