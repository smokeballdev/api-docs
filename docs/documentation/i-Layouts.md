---
tags: [Documentation]
---

# Layouts

## 1. Overview

Layouts are User Interfaces that look different from each other, and are designed for a specific/unique purpose. They are designed and maintained by our product team. Each user interface has different fields pertinent to their unique purpose.

For example, we have a **Property Details** Layout for **Conveyancing** Matter Types:

![Property Details](../../assets/images/propertydetails.png)

Or, **Case Details** Layout for some **Litigation** Matter Types:

![Case Details](../../assets/images/casedetails.png)

## 2. Data Mapping

How to get a list of layout fields that you can set up a mapping for:

1. Get a reference for the layout design ID by querying a matter type.

```http request
GET https://api.smokeball.com.au/mattertypes/{matterTypeId}
```

Example response:

```json
{
  "href": "https://api.smokeball.com.au/mattertypes/9c6f59b6-3871-4fbd-a508-8fc43f03c6b4",
  "items": [
    {
      "href": "https://api.smokeball.com.au/layouts/b571beb4-9175-436c-a70e-e70acb0f4ed2",
      "rel": "layouts",
      "id": "b571beb4-9175-436c-a70e-e70acb0f4ed2",
      "providerId": "LayoutProvider",
      "name": "Workers Comp Details"
    }
  ],
  "id": "9c6f59b6-3871-4fbd-a508-8fc43f03c6b4",
  "versionId": "1bd57eaa-089c-4a92-8136-aed74bf5c389",
  "name": "Workers Compensation",
  "category": "Personal Injury",
  "representativeOptions": [
    "Worker",
    "Employer"
  ],
  "location": "NSW"
}
```

In this example, `/items/0/id` contains the layout design ID and `/items/0/href` contains a full URL to use in the next
step.

2. Retrieve the full list of fields for the given layout design.

```http
GET https://api.smokeball.com/layouts/{layoutDesignId}
```

Example response:

```json
{
  "id": "b571beb4-9175-436c-a70e-e70acb0f4ed2",
  "href": "https://api.smokeball.com.au/layouts/b571beb4-9175-436c-a70e-e70acb0f4ed2",
  "fields": [
    {
      "name": "Matter/WorkersCompDetails/BodyPartInjured",
      "type": "Text"
    },
    {
      "name": "Matter/WorkersCompDetails/DateOfInjury",
      "type": "DateTime"
    },
    {
      "name": "Matter/WorkersCompDetails/DescriptionOfInjury",
      "type": "Text"
    }
  ],
  "name": "Workers Comp Details"
}
```

In this example, `/fields/*/name` contain the layout fields that you can set up a mapping for.