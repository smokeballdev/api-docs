---
tags: [Documentation]
---

# Making Requests

All requests to the API require a valid `api-key` issued by Smokeball and `id_token` (see [Authorization](1-Authentication.md)).

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

``` json
{
  "href": "https://stagingapi.smokeball.com/contacts",
  "offset": 0,
  "limit": 500,
  "size": 761,
  "first": {
    "href": "https://stagingapi.smokeball.com/contacts"
  },
  "next": {
    "href": "https://stagingapi.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "last": {
    "href": "https://stagingapi.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "value": [
    {
      "href": "https://stagingapi.smokeball.com/contacts/323f9755-045a-4b68-a536-eb566b8428ff",
      "id": "323f9755-045a-4b68-a536-eb566b8428ff",
      "groupOfPeople": {
        "people": [
          {
            "href": "https://stagingapi.smokeball.com/contacts/a1d868da-1243-4b9f-a8e3-cfc5c4abda50"
          },
          {
            "href": "https://stagingapi.smokeball.com/contacts/ea4ca7b9-b826-4840-a8b5-94e0c6977c65"
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



### Paged Responses

You will notice that some requests will have the optional parameters `limit` and `offset`. These are used to control how many results are returned in a request that returns a list of results. 

In the response above, you can see the details of the paging properties in the first section of the response:

``` json
"href": "https://stagingapi.smokeball.com/contacts",
  "offset": 0,
  "limit": 500,
  "size": 761,
  "first": {
    "href": "https://stagingapi.smokeball.com/contacts"
  },
  "next": {
    "href": "https://stagingapi.smokeball.com/contacts?limit=500&offset=500",
    "rel": "collection"
  },
  "last": {
    "href": "https://stagingapi.smokeball.com/contacts?limit=500&offset=500",
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
