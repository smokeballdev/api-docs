---
tags: [Documentation]
---

# Making Requests

All requests to the API require a valid `api-key` issued by Smokeball and `id_token` (see [Authentication](1-Authentication.md)).

### 1. Example Request

```json http
{
  "method": "get",
  "url": "https://api.smokeball.com/contacts/",
  "headers": {
    "x-api-key": "Y2xpZW50X2lkOmNsaWV",
    "Authorization": "Bearer Y2xpZW50X2lkOmNsaWV"
  }
}
```

**Response**

``` json
{
  "href": "https://api.smokeball.com/contacts",
  "offset": 0,
  "limit": 500,
  "size": 761,
  "first": {
    "href": "https://api.smokeball.com/contacts"
  },
  "next": {
    "href": "https://api.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "last": {
    "href": "https://api.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "value": [
    {
      "href": "https://api.smokeball.com/contacts/323f9755-045a-4b68-a536-eb566b8428ff",
      "id": "323f9755-045a-4b68-a536-eb566b8428ff",
      "groupOfPeople": {
        "people": [
          {
            "href": "https://api.smokeball.com/contacts/a1d868da-1243-4b9f-a8e3-cfc5c4abda50"
          },
          {
            "href": "https://api.smokeball.com/contacts/ea4ca7b9-b826-4840-a8b5-94e0c6977c65"
          }
        ],
        "residentialAddress": {
          "addressLine1": "1247 N Clark Street",
          "city": "Chicago",
          "state": "IL",
          "zipCode": "60607",
          "county": "Cook",
          "country": "United States"
        }
      },
    ...
  ]
  } 
 } 
```

### 2. Paged Responses

You will notice that some requests will have the optional parameters `limit` and `offset`. These are used to control how many results are returned in a request that returns a list of results. 

In the response above, you can see the details of the paging properties in the first section of the response:

``` json
"href": "https://api.smokeball.com/contacts",
  "offset": 0,
  "limit": 500,
  "size": 761,
  "first": {
    "href": "https://api.smokeball.com/contacts"
  },
  "next": {
    "href": "https://api.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "last": {
    "href": "https://api.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
```

**Paging Properties**

Property | Description 
---------|----------
 `offset` | The number at which the results set begins 
 `limit` | The number of results in the response 
 `size` | The total number of results
 `first` | A link to the first result set
 `next` | A link to the next result set
 `last` | A link to the last result set

>By default, the `limit` will be set to a maximum of 500 results. 

### 3. Server-to-Server requests

When using the [Client Credentials Grant](1-Authentication.md#2-client-credentials-grant) to perform server-to-server requests, you must explicitly specify which account you are using by prefixing the url with the `accountId` in this format:

> https://api.smokeball.com/{accountId}/{resource-path}

Using the above contacts request as an example, the url is https://api.smokeball.com/contacts/
For a server-to-server request using the `accountId: ea4ca7b9-b826-4840-a8k5-94e6c6937c65` the request would be:

```json http
{
  "method": "get",
  "url": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts/",
  "headers": {
    "x-api-key": "Y2xpZW50X2lkOmNsaWV",
    "Authorization": "Bearer Y2xpZW50X2lkOmNsaWV"
  }
}
```

**Response**

> Note: all links in the repsonse will include the accountId prefix:

``` json
{
  "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts",
  "offset": 0,
  "limit": 500,
  "size": 761,
  "first": {
    "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts"
  },
  "next": {
    "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "last": {
    "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "value": [
    {
      "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts/323f9755-045a-4b68-a536-eb566b8428ff",
      "id": "323f9755-045a-4b68-a536-eb566b8428ff",
      "groupOfPeople": {
        "people": [
          {
            "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts/a1d868da-1243-4b9f-a8e3-cfc5c4abda50"
          },
          {
            "href": "https://api.smokeball.com/ea4ca7b9-b826-4840-a8k5-94e6c6937c65/contacts/ea4ca7b9-b826-4840-a8b5-94e0c6977c65"
          }
        ],
        "residentialAddress": {
          "addressLine1": "1247 N Clark Street",
          "city": "Chicago",
          "state": "IL",
          "zipCode": "60607",
          "county": "Cook",
          "country": "United States"
        }
      },
    ...
  ]
  } 
 } 
```

### 4. Tracking your requests

Since all POST/PUT requests are handled asynchronously by the Smokeball API, it is useful to be able to track the changes especially when using [Webhooks](6-Webhooks.md). This is acheived  by supplying the `RequestId` header with your requests. 

If the `RequestId` header is supplied, it will be returned as a response header for every request you make. It will also be included in all webhook callbacks so you can track what data was impacted by your request.


