---
tags: [Documentation]
---

# Files and Folders

### 1. Structure

Every Matter has a familiar folder structure used to store files and documents:

![Folders and Files](/assets/images/filesandfolders.png)

If you want to work with the folder/file structure, then you will need to use the [Folders](../reference/swagger.json/paths/~1matters~1{matterId}~1documents~1folders) endpoint.
Alternatively, if you prefer to work with a flat list of all Files on the Matter, you can use the [Files](../reference/swagger.json/paths/~1matters~1{matterId}~1documents~1files) endpoint.
 
The root folder of a matter will always have an **id** of `null` and if no **folder id** is specified when adding a File, this is the folder that will be used.

Every folder including the root folder, will have a a number of sub-folders and files. When retrieveing a folder from the API you will get a list of the sub-folders and files. Here is a response for the Matter shown above:

`GET https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/folders`

``` json
{
  "value": [
    {
      "folders": [
        {
          "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/folders/27e52eaf-fa51-4582-bd59-ec3ea5ab664b",
          "id": "27e52eaf-fa51-4582-bd59-ec3ea5ab664b",
          "name": "Correspondence"
        },
        {
          "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/folders/c2f7a537-4052-4bab-a4fb-84e0d7e7cef5",
          "id": "c2f7a537-4052-4bab-a4fb-84e0d7e7cef5",
          "name": "Contracts"
        },
        {
          "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/folders/b97f0fe6-32fb-427f-b6c7-30ea286736f7",
          "id": "b97f0fe6-32fb-427f-b6c7-30ea286736f7",
          "name": "Other"
        },
        {
          "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/folders/6f447d01-5379-493b-a60d-25fb4d64ff8a",
          "id": "6f447d01-5379-493b-a60d-25fb4d64ff8a",
          "name": "Emails"
        }
      ],
      "files": [
        {
          "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/files/95df25eb-dc99-45dd-881f-4608e00f551b",
          "id": "95df25eb-dc99-45dd-881f-4608e00f551b",
          "folder": {},
          "name": "1. Agenda",
          "fileExtension": ".docx",
          "ownerId": "e3f545b4-34cf-49e5-b3e2-55ab0876a84c",
          "dateCreated": "2017-02-20T05:50:30.1568284Z",
          "sizeBytes": 44775,
          "downloadInfo": {
            "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/files/95df25eb-dc99-45dd-881f-4608e00f551b/download"
          }
        },
        {
          "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/files/6fd46dfa-a46d-4de4-9553-dae30f7f523a",
          "id": "6fd46dfa-a46d-4de4-9553-dae30f7f523a",
          "folder": {},
          "name": "Engagement Letter",
          "fileExtension": ".docx",
          "ownerId": "e3f545b4-34cf-49e5-b3e2-55ab0876a84c",
          "dateCreated": "2020-05-22T01:36:17.4355416Z",
          "sizeBytes": 20793,
          "downloadInfo": {
            "href": "https://stagingapi.smokeball.com/matters/5bbc4d72-3001-46bf-acd7-c90dff4dc9fa/documents/files/6fd46dfa-a46d-4de4-9553-dae30f7f523a/download"
          }
        }
      ]
}
```


### 2. Adding a File



### 3. Uploading a new version

### 4. Downloading a File
