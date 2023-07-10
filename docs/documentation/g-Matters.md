---
tags: [Documentation]
---

# Matters

### 1. Matter Types

Smokeball comes with a range of pre-configured matter types for different areas of law. These are provided by Smokeball’s internal content team and are not user editable.

A Matter Type defines what information can be saved to the matter, relevant to that area of law and state / location.

![Matter Types](../../assets/images/mattertypes.png)

When creating a matter in Smokeball we can see

1. A list of Matter Types. Here, _Financing_ is a matter type, under the _Real Estate_ category.
2. Representative options for the _Financing_ matter type. This specifies who the Smokeball firm is acting for. In other words, who is their client.

#### API - Getting a Matter Type

```json http
{
  "method": "get",
  "url": "https://api.smokeball.com/mattertypes",
  "query": {
    "Location": "IL",
    "Category": "Real Estate"
  }
}
```

**Response**

```json
{
  "href": "https://api.smokeball.com/mattertypes",
  "offset": 0,
  "limit": 500,
  "size": 18,
  "first": {
    "href": "https://api.smokeball.com/mattertypes"
  },
  "value": [
    {
      "href": "https://api.smokeball.com/mattertypes/8aca3574-aecb-4a6f-991d-680861bece1d_il",
      "id": "8aca3574-aecb-4a6f-991d-680861bece1d_IL",
      "versionId": "32e86c38-534d-4362-95a1-f5c287a8d4b7",
      "name": "Financing",
      "category": "Real Estate",
      "representativeOptions": [
        "Borrower",
        "Lender",
        "MortgageBroker",
        "Seller",
        "CooperativeApartment"
      ],
      "location": "IL",
      "deleted": false
    },
    {
      "href": "https://api.smokeball.com/mattertypes/2d5aca9b-9469-44d4-8616-766a864432aa",
      "id": "2d5aca9b-9469-44d4-8616-766a864432aa",
      "versionId": "1410cfea-723e-4514-8901-c92dab1bd5f2",
      "name": "Buy",
      "category": "Real Estate",
      "representativeOptions": [
        "Buyer"
      ],
      "location": "IL",
      "deleted": false
    },
    ...
  ]
}
```

> Here the important information to note is the `id` and `representativeOptions` of the matter type. We’ll need those when creating a matter.

---

### 2. Contacts

In order to successfully create a matter, we’re going to need to provide one or more contacts that are clients and/or other-sides (if applicable).

In Smokeball’s desktop app you can create a contact as a Person, Firm/Business/Organization or Trust.
Right now, the API supports the following contact types

- Person
- Company

![New Contact](../../assets/images/newcontact.png)

#### API - Creating contacts

To create a client contact you can make the following request which would create a person called ‘Test Contact’.

```json http
{
  "method": "POST",
  "url": "https://api.smokeball.com/contacts",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{\n  \"person\": {\n    \"firstName\": \"Test\",\n    \"lastName\": \"Contact\",    \n    \"email\": \"test@gmail.com\",\n  }\n}\n\n"
}
```

To create an other-side contact you can make the following request which would create a person called ‘Other Contact’.

```json http
{
  "method": "POST",
  "url": "https://api.smokeball.com/contacts",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{\n  \"person\": {\n    \"firstName\": \"Other\",\n    \"lastName\": \"Contact\",    \n    \"email\": \"other@gmail.com\",\n  }\n}\n\n"
}
```

**Response**

```json
202 Accepted

{
  "id": "9a9ce552-6bee-45e4-8eb1-afe2c18c489f",
  "href": "https://api.smokeball.com/contacts/9a9ce552-6bee-45e4-8eb1-afe2c18c489f"
}
```

Take note of the Contact’s `id` returned in the response, we’ll need that when creating a matter.
Alternatively, if the contact already exists in Smokeball, you can query it to find it’s id.

> ### _Tip_
>
> When creating a contact you can pass in an `externalSystemId` if you want to store your own reference id with this contact.

---

### 3. Staff

In Smokeball, staff can be assigned to a matter as the **Attorney Responsible** or the **Person Assisting**. The matter list can then be filtered by Attorney responsible, etc.

![Staff](../../assets/images/staff.png)

If you want to specify the Attorney Responsible or Person Assisting when creating a matter via the API, then we will first need to retrieve the staff member’s id.

```json http
{
  "method": "get",
  "url": "https://api.smokeball.com/staff"
}
```

**Response**

```json
{
  "href": "https://api.smokeball.com/staff",
  "size": 28,
  "first": {
    "href": "https://api.smokeball.com/staff"
  },
  "value": [

    {
      "href": "https://api.smokeball.com/staff/66f7fa24-bb03-4d89-ac28-e19620cc1e45",
      "id": "66f7fa24-bb03-4d89-ac28-e19620cc1e45",
      "versionId": "cfdbb798-1f2e-4355-87bc-620cbc62acc2",
      "firstName": "Test",
      "lastName": "Staff",
      "phone": {
        "number": "200518540309"
      },
      "cell": {},
      "email": "teststaff@gmail.com",
      "avatar": "https://toolbar.smokeball.com/FirmManagement/Firm/RedirectToStaffProfileImageUrl?accountId=2f3143de-2bfe-4742-9e9c-7c16b48bdef7&staffId=66f7fa24-bb03-4d89-ac28-e19620cc1e45",
      "former": false,
      "enabled": true
    },

    {
      "href": "https://api.smokeball.com/staff/c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "id": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "versionId": "23dbdc50-6171-4408-8150-413cb293a249",
      "title": "",
      "firstName": "Hunter",
      "lastName": "Steele",
      "email": "hunter@gmail.com",
      "avatar": "https://toolbar.smokeball.com/FirmManagement/Firm/RedirectToStaffProfileImageUrl?accountId=2f3143de-2bfe-4742-9e9c-7c16b48bdef7&staffId=c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "former": false,
      "enabled": false
    },
    ...
  ]
}
```

---

### 4. Creating a matter

Now we have everything we need to create a matter.

```json http
{
  "method": "POST",
  "url": "https://api.smokeball.com/matters",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "number": "Matter-001",
    "matterTypeId": "8aca3574-aecb-4a6f-991d-680861bece1d_IL",
    "clientIds": [
      "9a9ce552-6bee-45e4-8eb1-afe2c18c489f"
    ],
    "otherSidesIds": [
      "52f551d9-6c1c-44d1-8bf7-ddce5a856134"
    ],
    "clientRole": "Borrower",
    "otherSideRole": "Lender",
    "description": "Created via API",
    "status": "Open",
    "dateOpened": "2020-05-20T04:20:55.035Z",
    "personResponsibleStaffId": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
    "personAssistingStaffId": "66f7fa24-bb03-4d89-ac28-e19620cc1e45",
    "isLead": false
  }
}
```

Here we have used:


Parameter | Description
---------|----------
 `matterTypeId` | We're using the id for `Financing` which we retrieved earlier 
 `clientIds` | An array containing the id of the `Test Contact` we created
 `otherSideIds` | An array containing the id of the `Other Contact` we created 
 `clientRole` |  Set to _Borrower_ which is one of the matter type’s representativeOptions retrieved earlier
 `otherSideRole` |  Set to _Lender_ which is one of the matter type’s representativeOptions retrieved earlier
 `personResponsibleStaffId`| Id of a staff member we retrieved from the GET Staff call
 `personAssistingStaffId` | Id of a staff member we retrieved from the GET Staff call
 `number` | Internal reference number for the matter. Some firms have enabled ‘auto numbering’ in Smokeball settings. If you provide a value for this field via the API then the matter number will be overwritten
`description` | A free text entry field for the description of the matter
`status` | Can be `Open`, `Pending`, `Closed`, `Deleted` or `Cancelled`
`isLead` | This is an optional boolean flag to indicate if the matter is being created as a `Lead` (see `Creating a Lead`). Must be set to `false` when creating a `Matter` (defaults to `false` if not provided)
`dateOpened` | when the matter was opened (this can be backdated if required)

**Response**

```json
202 Accepted

{
  "id": "51cefb72-6247-4ca2-8926-5d14d65f7cb9",
  "href": "https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9"
}
```

The matter is now shown in Smokeball:

![Matter](../../assets/images/newmatter.png)

---

### 5. Retrieving a matter

> New enhanced endpoint version available to retrieve matters. See section 8 for more details.

```http
GET https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9
```

**Response**

```json
{
  "href": "https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9?MatterId=51cefb72-6247-4ca2-8926-5d14d65f7cb9",
  "id": "51cefb72-6247-4ca2-8926-5d14d65f7cb9",
  "versionId": "637256184791434000",
  "number": "Matter-001",
  "matterType": {
    "id": "8aca3574-aecb-4a6f-991d-680861bece1d_IL",
    "href": "/mattertypes/8aca3574-aecb-4a6f-991d-680861bece1d_IL",
    "rel": "MatterTypes"
  },
  "clients": [
    {
      "id": "9a9ce552-6bee-45e4-8eb1-afe2c18c489f",
      "href": "/contact/9a9ce552-6bee-45e4-8eb1-afe2c18c489f",
      "rel": "Contact"
    }
  ],
  "otherSides": [
    {
      "id": "52f551d9-6c1c-44d1-8bf7-ddce5a856134",
      "href": "/contact/52f551d9-6c1c-44d1-8bf7-ddce5a856134",
      "rel": "Contact"
    }
  ],
  "description": "Created via API",
  "status": "Open",
  "personResponsible": {
    "id": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
    "href": "/staff/c85d28cb-a760-4627-aa59-0a853c2e65ed",
    "rel": "Staff"
  },
  "items": {
    "id": "51cefb72-6247-4ca2-8926-5d14d65f7cb9",
    "href": "/matter/51cefb72-6247-4ca2-8926-5d14d65f7cb9/items",
    "rel": "collection"
  }
}
```

Most of this information is unchanged from what we passed in when creating the matter. You’ll notice that the clients, other-sides, staff and matter type properties have been expanded as links to resources with more information.

There is also the items collection which we’ll explore in more detail next.

---

### 6. Matter Items

A matter can contain various items and sub items that store hierarchical data relevant to that area of law.

![Matter Items](../../assets/images/matteritems.png)

The matter items list is ordered as follows:

1. Matter Info
2. Borrower (Client)

    a. Client’s Attorney - this is always the Smokeball firm. This relationship is hidden in the desktop UI but can  be seen in the API.
3. Matter Type
4. Lender (Other Side)

    a. Other Side’s Attorney
5. Other Roles, Relationships and Layouts

Matter sub items are shown indented under the matter item. Here `Lender` is a matter item, and `Attorney` and `Loan Details` are its subitems.
Here's an example request to GET the Matter Items:

```http
GET https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/items
```

**Response**

```json
{
  "versionId": "9843e3cf-9720-4ac5-94cf-1b265c5e54d5",
  "items": [
    {
      "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/info",
      "name": "Matter Info",
      "index": 0,
      "visible": true
    },
    {
      "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/441225f5-0803-412b-be45-c81fb893231f",
      "id": "441225f5-0803-412b-be45-c81fb893231f",
      "name": "Borrower",
      "index": 0,
      "visible": true,
      "subItems": [
        {
          "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/441225f5-0803-412b-be45-c81fb893231f/relationships/04f40670-898c-4e47-9565-df8cca2569c8",
          "id": "04f40670-898c-4e47-9565-df8cca2569c8",
          "name": "Attorney",
          "index": 0,
          "visible": true
        }
      ]
    },
    {
      "href": "/mattertypes/8aca3574-aecb-4a6f-991d-680861bece1d",
      "name": "Matter Type",
      "index": 0,
      "visible": true
    },
    {
      "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/e8ea6745-399b-4234-8d62-5dd891e0ae9d",
      "id": "e8ea6745-399b-4234-8d62-5dd891e0ae9d",
      "name": "Co-Counsel",
      "index": 0,
      "visible": false
    },
    {
      "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/f377da1c-8f48-40cd-8d23-0c3b9483c6b1",
      "id": "f377da1c-8f48-40cd-8d23-0c3b9483c6b1",
      "name": "Lender",
      "index": 0,
      "visible": true,
      "subItems": [
        {
          "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/f377da1c-8f48-40cd-8d23-0c3b9483c6b1/relationships/31b6e000-9f39-47d4-8ee8-9fed5f6901f0",
          "id": "31b6e000-9f39-47d4-8ee8-9fed5f6901f0",
          "name": "Attorney",
          "index": 0,
          "visible": true
        },
        {
          "name": "Loan Details",
          "index": 0,
          "visible": true
        }
      ]
    },
    {
      "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/429371ae-a577-40a0-bf4d-ed157764ed6c",
      "id": "429371ae-a577-40a0-bf4d-ed157764ed6c",
      "name": "Mortgage Broker",
      "index": 0,
      "visible": true,
      "subItems": [
        {
          "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/429371ae-a577-40a0-bf4d-ed157764ed6c/relationships/bbf42ca4-cf70-48e2-a737-86e1f196ebc2",
          "id": "bbf42ca4-cf70-48e2-a737-86e1f196ebc2",
          "name": "Attorney",
          "index": 0,
          "visible": true
        }
      ]
     },
    ...
}
```

Where available, the matter item will have a link to the relevant resource containing additional information.

> Some matter items can be hidden on the matter by default. This is usually done for roles that aren’t common on the matter. They are still exposed in the api as "visible: false”. Currently the visibility can only be changed in the Smokeball desktop app under Matter Settings (1).

![Matter Roles](../../assets/images/matterparties.png)

There are different types of matter items and sub items, including:

#### 

###### Roles and Relationships

A contact on a matter will be assigned to a specific role or relationship, based on how they relate to the matter. For example, in the above image, `Test Contact` is assigned to the `Borrower` role **(2)**. By convention, the first Role in the list is the firm’s client. The client role has an `Attorney` relationship that links to the Smokeball Firm (hidden in the Smokeball UI)

Contacts can also be related to other contacts on the matter. For example in the above image, `Lender` is a Role with an `Attorney` relationship **(3)**. This attorney acts for this specific Lender. There could be other Lenders relationships on this matter that are related to other roles (not shown in image)

Roles and relationships can be set in the API.

#### 

###### Layouts

Layouts are data entry screens that are tailored to the area of law. In this example there is a `Loan Details` layout **(4)** that would store information about that Lender’s loan. A layout can either be a sub item linked to a matter item (e.g. Loan Details as shown here) or a matter item itself (e.g. Property Details, not shown).

Following is a typical flow to get/set layout data.

1. Get the layout design ID and currently set data by retrieving all the layout matter items

```http
GET https://api.smokeball.com/matters/{matterId}/layouts
```

or a single layout matter item.

```http
GET https://api.smokeball.com/matters/{matterId}/layouts/{itemId}
```

Example response:

```json
{
    "id": "60efd655-cd1a-42cd-a170-a228cc09b5b1",
    "href": "https://api.smokeball.com/matters/3f0cfc3d-be5d-4a70-88ac-e099a800d46d/layouts/60efd655-cd1a-42cd-a170-a228cc09b5b1",
    "itemId": "60efd655-cd1a-42cd-a170-a228cc09b5b1",
    "index": 0,
    "layoutDesign": {
        "id": "f2654144-ab55-45ec-9852-489a5caaa111",
        "href": "https://api.smokeball.com/layouts/f2654144-ab55-45ec-9852-489a5caaa111",
        "rel": "Layouts"
    },
    "values": [],
    "events": []
}
```

2. Retrieve the full list of fields for the given layout design.

```http
GET https://api.smokeball.com/layouts/{layoutDesignId}
```

Example response:

```json
{
    "id": "f2654144-ab55-45ec-9852-489a5caaa111",
    "href": "https://api.smokeball.com/layouts/f2654144-ab55-45ec-9852-489a5caaa111",
    "fields": [
        {
            "name": "Matter/SettlementNegotiations/SettlementNegotiationsDetails/Note",
            "type": "Text"
        },
        {
            "name": "Matter/SettlementNegotiations/SettlementNegotiationsDetails/OfferDate",
            "type": "DateTime"
        }
    ],
    "name": "Settlement Negotiations - Personal Injury"
}
```

3. Update the layout with new data values.

```http
PATCH https://api.smokeball.com/matters/{matterId}/layouts/{itemId}

{
    "values": [
        {
            "key": "Matter/SettlementNegotiations/SettlementNegotiationsDetails/Note",
            "value": "New note"
        }
    ]
}
```

> Note: a field with type `Role` expects an ID of a role. The name of the role needs to be the same as the layout field (e.g. for a field with key `Matter/CaseDetails/StandardCaseDetails/Judge` expected role name is `Judge`). 

> Note: a field with type `DateTime` expects a UTC date/time in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format (e.g. `2021-10-12T13:00:00.0000000Z`).

---

### 7. Roles

To find out which contact is assigned to a role, we can get retrieve the role details:

```http
GET https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/441225f5-0803-412b-be45-c81fb893231f
```

**Response**

```json
{
  "id": "441225f5-0803-412b-be45-c81fb893231f",
  "name": "Borrower",
  "contact": {
    "id": "9a9ce552-6bee-45e4-8eb1-afe2c18c489f",
    "href": "/contact/9a9ce552-6bee-45e4-8eb1-afe2c18c489f",
    "rel": "Contact"
  },
  "representatives": [],
  "relationships": [
    {
      "id": "04f40670-898c-4e47-9565-df8cca2569c8",
      "name": "Attorney",
      "contact": {
        "href": "/firm/",
        "rel": "Firm"
      },
      "representatives": []
    }
  ]
}
```

Here we can see the Borrower role has a contact assigned, and we can follow the link to the contact resource to find out more details. We can also see that there is an Attorney relationship that links to the Firm resource.

#### Setting a Role

In our example, the Lender role did not have a contact assigned. To set this, we first need to know the id / link to the role we want to update. This was retrieved earlier with the MatterItems api call:

```http
GET https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/items
```

**Response**

```json
{
  "versionId": "9843e3cf-9720-4ac5-94cf-1b265c5e54d5",
  "items": [
    ...
    {
      "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/f377da1c-8f48-40cd-8d23-0c3b9483c6b1",
      "id": "f377da1c-8f48-40cd-8d23-0c3b9483c6b1",
      "name": "Lender",
      "index": 0,
      "visible": true,
      "subItems": [
        {
          "href": "/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/f377da1c-8f48-40cd-8d23-0c3b9483c6b1/relationships/31b6e000-9f39-47d4-8ee8-9fed5f6901f0",
          "id": "31b6e000-9f39-47d4-8ee8-9fed5f6901f0",
          "name": "Attorney",
          "index": 0,
          "visible": true
        },
        {
          "name": "Loan Details",
          "index": 0,
          "visible": true
        }
      ]
    },
    ...
}
```

Let’s say we’ve already created a contact called ‘ABC Lender’ with id ` dcafdb9e-e46d-4720-a3f8-638b3dd4ca38` and now we want to assign it to this matter in the Lender role

```json http
{
  "method": "PUT",
  "url": "https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/f377da1c-8f48-40cd-8d23-0c3b9483c6b1",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "contactId": "dcafdb9e-e46d-4720-a3f8-638b3dd4ca38"
  }
}
```

**Response**

```json
202 Accepted

{
  "id": "f377da1c-8f48-40cd-8d23-0c3b9483c6b1",
  "href": "https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9/roles/f377da1c-8f48-40cd-8d23-0c3b9483c6b1"
}
```

![New Role](../../assets/images/newroleadded.png)

---

### 8. Retrieving matters (v2)

#### Retrieving a matter

```http
GET https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9
```

Set header variable `Api-Version` with value `2.0` to access new version.

This is an enhanced version of the endpoint which combines the following APIs:
- Matters API
- MatterItems API
- Roles API
- Relationships API

**Response**

```json
{
  "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/matters/b7982ad5-0d43-4cd1-9b0e-185caf30f05e",
  "id": "b7982ad5-0d43-4cd1-9b0e-185caf30f05e",
  "externalSystemId": "EXT01",
  "versionId": "638138489289266248",
  "number": "AM-0323-0005",
  "matterType": {
    "id": "0623643a-48a4-41d7-8c91-d35915b291cd_ACT",
    "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/mattertypes/0623643a-48a4-41d7-8c91-d35915b291cd_ACT",
    "rel": "MatterTypes"
  },
  "clients": [
    {
      "id": "ec499947-9d3d-442a-b431-05c8ee280687",
      "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/contacts/ec499947-9d3d-442a-b431-05c8ee280687",
      "rel": "Contacts"
    }
  ],
  "otherSides": [
    {
      "id": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
      "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/contacts/71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
      "rel": "Contacts"
    }
  ],
  "title": "AM-0323-0005 - Smith - Sale - Jones",
  "description": "1, Flat 23 Gerald Street, Greystanes NSW",
  "status": "Open",
  "personResponsible": {
    "id": "526670af-acd8-401e-bc64-6d4cafb4a12a",
    "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/staff/526670af-acd8-401e-bc64-6d4cafb4a12a",
    "rel": "Staff"
  },
  "personAssisting": {
    "id": "526670af-acd8-401e-bc64-6d4cafb4a12a",
    "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/staff/526670af-acd8-401e-bc64-6d4cafb4a12a",
    "rel": "Staff"
  },
  "originatingStaff": {
    "id": "526670af-acd8-401e-bc64-6d4cafb4a12a",
    "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/staff/526670af-acd8-401e-bc64-6d4cafb4a12a",
    "rel": "Staff"
  },
  "supervisor": {
    "id": "526670af-acd8-401e-bc64-6d4cafb4a12a",
    "href": "https://api.smokeball.com/884a9809-c997-41bc-b1d2-e9f8c317cc29/staff/526670af-acd8-401e-bc64-6d4cafb4a12a",
    "rel": "Staff"
  },
  "clientCode": "Client A",
  "openedDate": "2023-03-08T04:18:22.3044791Z",
  "isLead": false,
  "items": {
    "vendors": [
      {
        "id": "5c65358d-1502-463c-9464-f36f21c7e610",
        "type": "role",
        "name": "Vendor",
        "contact": {
          "id": "ec499947-9d3d-442a-b431-05c8ee280687",
          "href": "https://api.smokeball.com.au/884a9809-c997-41bc-b1d2-e9f8c317cc29/contacts/ec499947-9d3d-442a-b431-05c8ee280687",
          "rel": "Contacts"
        },
        "subItems": {
          "solicitors": [
            {
              "id": "4a31c76f-0952-4069-8259-840cd6106246",
              "type": "role",
              "contact": {
                "id": "MyFirm"
              },
              "name": "Solicitor",
            }
          ],
          "relationshipDetails": [
            {
              "id": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
              "href": "https://stagingapi.smokeball.com.au/884a9809-c997-41bc-b1d2-e9f8c317cc29/matters/b7982ad5-0d43-4cd1-9b0e-185caf30f05e/layouts/71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
              "rel": "layout",
              "type": "layout"
            }
          ]
        }
      }
    ],
    "purchasers": [
      {
        "id": "f794090d-183e-462a-9654-930d64a5f882",
        "name": "Purchaser",
        "contact": {
          "id": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
          "href": "https://stagingapi.smokeball.com.au/884a9809-c997-41bc-b1d2-e9f8c317cc29/contacts/71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
          "rel": "Contacts"
        },
        "subItems": {
          "solicitors": [
            {
              "id": "2761eb3a-caeb-4b6d-911e-9daf302f6799",
              "name": "Solicitor",
              "contact": {
                "id": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
                "href": "https://stagingapi.smokeball.com.au/884a9809-c997-41bc-b1d2-e9f8c317cc29/contacts/71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
                "rel": "Contacts"
              }
            }
          ]
        }
      }
    ],
    "marriageDetails": [
      {
        "id": "0ea6eb6e-b078-4de3-8639-26c99ec9c32c",
        "href": "https://stagingapi.smokeball.com.au/884a9809-c997-41bc-b1d2-e9f8c317cc29/matters/b7982ad5-0d43-4cd1-9b0e-185caf30f05e/layouts/0ea6eb6e-b078-4de3-8639-26c99ec9c32c",
        "rel": "layout",
        "type": "layout",
        "subItems": {
          "solicitors": [
            {
              "id": "5c65358d-1502-463c-9464-f36f21c7e610",
              "type": "role",
              "name": "Solicitor",
              "contact": {
                "id": "ec499947-9d3d-442a-b431-05c8ee280687",
                "href": "https://stagingapi.smokeball.com.au/884a9809-c997-41bc-b1d2-e9f8c317cc29/contacts/ec499947-9d3d-442a-b431-05c8ee280687",
                "rel": "Contacts"
              }
            }
          ]
        }
      }
    ]
  }
}
```  

#### Retrieving multiple matters

This endpoint can be used to sort, filter and retrieve one or more matters.

```http
GET https://api.smokeball.com/matters
```

Set header variable `Api-Version` with value `2.0` to access new version.

The endpoint returns a paginated list of matters based on the action/filter paramaters specified in the request.

List of request parameters currently supported:

Parameter | Description
---------|----------
 `limit` | Use this to limit number of records to be returned. Defaults to 500.
 `offset` | Use this to set offset for the records to be returned.
 `matterTypeId` | Use this to filter by matter type identifier.
 `contactId` |  Use this to filter by the contact identifier.
 `status` |  Use this to filter by current status of the matter. Possible values: `Open`, `Pending`, `Closed`, `Deleted` or `Cancelled`
 `isLead`| Boolean flag to restrict search to `Leads`.
 `updatedSince` | Use this to filter by matters updated since a specified time (.net ticks representation of the UTC datetime).
 `sort` | Use this to provide comma separated list of sort fields. Expected format `asc/desc(field)`. Sort fields currently supported: `Status`, `LastUpdated` - e.g. `asc(Status)`, `desc(LastUpdated)`
`fields` | Use this to provide comma separated list of optional fields to include in the result. Optional fields currently supported: `Items`.

> The matter `items` field is not returned by default. Use the `fields` in the request to include this in the result.

**Response**

```json
{
    "href": "https://api.smokeball.com.au/f4ff1eff-2351-489c-9470-01d985838d76/matters",
    "offset": 0,
    "limit": 100,
    "size": 1396,
    "first": {
        "href": "https://api.smokeball.com.au/f4ff1eff-2351-489c-9470-01d985838d76/matters?limit=100",
        "rel": "collection"
    },
    "next": {
        "href": "https://api.smokeball.com.au/f4ff1eff-2351-489c-9470-01d985838d76/matters?limit=100&offset=100",
        "rel": "collection"
    },
    "last": {
        "href": "https://api.smokeball.com.au/f4ff1eff-2351-489c-9470-01d985838d76/matters?limit=100&offset=1300",
        "rel": "collection"
    },
    "value": [
        {
            "href": "https://api.smokeball.com.au/f4ff1eff-2351-489c-9470-01d985838d76/matters/04db8d78-f222-42de-b3d6-10809642402e",
            "id": "04db8d78-f222-42de-b3d6-10809642402e",
            "versionId": "638193848837974931",
            "title": "Company Incorporation",
            "matterType": {
                "name": "Company Incorporation",
                "id": "0d157e91-f3e1-4161-ae7d-03e9c23be912_NSW",
                "href": "https://api.smokeball.com.au/f4ff1eff-2351-489c-9470-01d985838d76/mattertypes/0d157e91-f3e1-4161-ae7d-03e9c23be912_NSW",
                "rel": "MatterTypes"
            },
            "clients": [],
            "otherSides": [],
            "status": "Open",
            "openedDate": "2017-08-09T22:01:27.373097Z",
            "isLead": false,
            "clientIds": [],
            "otherSideIds": [],
            "items": {}
        },
        ...
        ...
    ]
}
```

### 9. Updating a matter

There are two ways to update a matter.

#### Using PUT endpoint

This endpoint is similar to the endpoint used to create a new matter.

```json http
{
  "method": "PUT",
  "url": "https://api.smokeball.com/matters/{matterId}",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "number": "Matter-001",
    "matterTypeId": "8aca3574-aecb-4a6f-991d-680861bece1d_IL",
    "clientIds": [
      "9a9ce552-6bee-45e4-8eb1-afe2c18c489f"
    ],
    "otherSidesIds": [
      "52f551d9-6c1c-44d1-8bf7-ddce5a856134"
    ],
    "clientRole": "Borrower",
    "otherSideRole": "Lender",
    "description": "Updated via API",
    "status": "Open",
    "dateOpened": "2020-05-20T04:20:55.035Z",
    "personResponsibleStaffId": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
    "personAssistingStaffId": "66f7fa24-bb03-4d89-ac28-e19620cc1e45",
    "isLead": false
  }
}
```

**Response**

```json
202 Accepted

{
  "id": "51cefb72-6247-4ca2-8926-5d14d65f7cb9",
  "href": "https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9"
}
```

Limitations:

- The `matterTypeId` field can only be modified when converting a `Lead` to a `Matter`.
- The `clientIds` field cannot be modified.
- The `otherSideIds` field cannot be modified.
- The `Matter` cannot be converted back to `Lead`.


#### Using PATCH endpoint (beta)

This endpoint can be used to update specific properties and also supports updating matter items, roles and relationships.

```json http
{
  "method": "PATCH",
  "url": "https://api.smokeball.com/matters/{matterId}",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
      "externalSystemId": "EXT123",
      "number": "AM-API-001",
      "matterTypeId": "0623643a-48a4-41d7-8c91-d35915b291cd_NSW", 
      "description": "Matter description",
      "status": "Open",
      "openedDate": "2022-04-23T14:00:00Z",
      "personResponsibleStaffId": "526670af-acd8-401e-bc64-6d4cafb4a12a",
      "personAssistingStaffId": "526670af-acd8-401e-bc64-6d4cafb4a12a",
      "originatingStaffId": "526670af-acd8-401e-bc64-6d4cafb4a12a",
      "supervisorStaffId": "526670af-acd8-401e-bc64-6d4cafb4a12a",    
      "clientCode": "Client A",   
      "branchId": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "branchProviderId": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "clientRole": "Buyer",
      "isLead": false,    
      "clientIds": [
          "5bc557e8-2a14-4c23-8d35-4f0fd1bff40e"
      ],
      "otherSideIds": [
          "9aec5a99-be4d-441a-80c3-21dcb549dd5a"
      ],
      "items": {
        "vendors": [
          {
            "id": "5c65358d-1502-463c-9464-f36f21c7e610",
            "contact": {
              "id": "ec499947-9d3d-442a-b431-05c8ee280687",
            },
            "subItems": {
              "solicitors": [
                {
                  "id": "4a31c76f-0952-4069-8259-840cd6106246",
                  "contact": {
                    "id": "MyFirm"
                  }
                }
              ],
              "relationshipDetails": [
                {
                  "id": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
                }
              ]
            }
          }
        ]
    }
  }
}
```

**Response**

```json
202 Accepted

{
  "id": "51cefb72-6247-4ca2-8926-5d14d65f7cb9",
  "href": "https://api.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9"
}
```

**Rule and limitations**

The endpoint supports the following actions:

- Updating one or more supported properties (keeping in mind the other rules that follow).
- Converting a lead to a matter - by setting `isLead: true -> isLead: false`.
- Updating `matterTypeId` and `clientRole` when a lead is being converted to matter.
- Updating the clients and/or other sides using the `clientIds` and `otherSideIds` properties.
- Updating matter items of provider type `role` and `layout` using the `items` property.

The endpoint **does not** support the following actions:

- Converting a `Matter` back to a `Lead` is not supported.
- Updating `matterTypeId` and `clientRole` on matters or on leads that are not being converted to matter is not supported.
- Updating `items` in a patch request where a lead is being converted to a matter is not supported.
- Updating `items` in a patch request where clients and/or other sides are being patched via `clientIds` and `otherSideIds` properties is not supported.
- Updating matter `items` of provider type other than `role` or `layout` is not supported.
- Removing all items of a type is not supported.

How updating `items` works:

- If the `items` property is not passed in, they will be left unchanged.
- There is **no need** to pass items of all types in a patch request.
- If one of more items of a type are being added/updated in the patch request, we must include all relevant items under that type (i.e., including the ones that are unchanged) in order to preserve them. Any existing item that is **not included** will be removed.
- The consumer **cannot update** the name on an item, e.g., the Vendor name.
- Contact Id **must be provided** when adding/update Items of role provider type. 
- A new item/sub item of a 'type' can only be added if there is an existing item/sub item of that type already present in the lead/matter.

*Also see examples in the usage scenarios section below.*


**Usage scenarios**

Common scenarios are described below with clear examples.

*Patch request to update the status and description.*

```json
{
    "status": "Closed",
    "description": "Hectic Matter updated"
}
```

*Patch request to convert lead to matter.*

```json
{
    "matterTypeId": "16052e57-8a05-44ca-b22f-4adf96132095_NSW",
    "clientRole": "Client",
    "isLead": false
}
```
*Patch request to update the client.*

There are 2 ways we could do this. The first is to simply set the clientIds property, like so:

```json
{
    "clientIds": [
      "9aec5a99-be4d-441a-80c3-21dcb549dd5a"
    ],
}
```
Alternatively, we can update the client in the items property, like so:

```json
{
  "items": {
    "purchasers": [
      {
        "id": "f794090d-183e-462a-9654-930d64a5f882",
        // Contact Id can be provided in one of two ways: 
        // Option 1: 
        "contactId": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
        // Option 2:
        "contact": {
            "id": "71b597c3-c1dd-4fe8-91c1-3c731a1e4394",
        }
        // Note: As subitems are omitted here, they will not be updated 
      }
    ]
  }
}
```

*Patch request to add a new client/item.*

There are 2 ways we could do this. The first is to simply set the clientIds property, like so:

```json
{
    "clientIds": [
      // Existing client must be provided here or it will be registered as a removal
      "9aec5a99-be4d-441a-80c3-21dcb549dd5a", // Existing client
      "728cb5aa-a9d2-46e5-b75a-149f6c4fae0b" // New client
    ],
}
```
Alternatively, we can add the role in the items property, like so:

```json
{
  "items": {
    "vendors": [
      // The existing vendors must be here or it will be registered as a removal
      {
        "id": "5c65358d-1502-463c-9464-f36f21c7e610",
        "contactId": "9aec5a99-be4d-441a-80c3-21dcb549dd5a"
      },
      {
        // Id is not required here if there is no existing vendors role item available
        // Id must be provided if the contact is being assigned to an existing role item 
        "contactId": "728cb5aa-a9d2-46e5-b75a-149f6c4fae0b"
      }
    ]
  }
}
```

*Patch request to remove a client/item.*

There are 2 ways we could do this. The first is to simply set the clientIds property where the missing items are removed, like so:

```json
{
    // Assuming there were two clients here: 
    // This patch request will remove the client who's Id was omitted here
    "clientIds": [
      "728cb5aa-a9d2-46e5-b75a-149f6c4fae0b"
    ],
}
```

The client/item we are removing is omitted from the payload:

```json
{
  "items": {
    // Assuming there were two vendors here: 
    // This patch request will remove the vendor item that was omitted 
    "vendors": [
      {
        "id": "5c65358d-1502-463c-9464-f36f21c7e610",
        "contactId": "9aec5a99-be4d-441a-80c3-21dcb549dd5a"
      }
    ]
  }
}
```

*Patch request to update contact id on an existing item.*

```json
{
  "items": {
    "purchasers": [
      {
        "id": "5c65358d-1502-463c-9464-f36f21c7e610",
        "contactId": "c555185b-4062-4e74-9ecf-f0b87dd4ee79",
        // Contact Id can also be set this way (as mentioned in a previous example)
        "contact": {
            "id": "c555185b-4062-4e74-9ecf-f0b87dd4ee79",
        },
        "subItems": {
            "Solicitor": [
                {
                    "id": "e46248a6-f71a-4991-b791-9f03e402a937",
                    // Contact Id also updated for sub item here
                    "contact": {
                        "id": "9aec5a99-be4d-441a-80c3-21dcb549dd5a"
                    }
                }
            ]
        }
      }
    ]
  }
}
```

*Patch request to add a new layout item.*

```json
{
  "items": {
        "Client": [
            {
                "id": "32663bf8-7e12-4b34-8ea4-596ff9350d2d",
                "contact": {
                    "id": "77b73607-761a-43a0-bdaa-a43e4568011d"
                },
                "subItems": {
                    "Details": [
                        {
                            "id": "5dd582e1-222b-4cf1-b0d7-1daf70d3627a"
                        },
                        // New `Details` sub item added here
                        {
                        }
                    ]
                }
            }
        ],
        "Case Details": [
            {
                "id": "52f51059-2a0c-4b5e-a86d-55bf5638a10d"
            },
            // New 'Case details' item added here
            {
            }
        ]
    }
}
```

*Patch request to remove an existing layout item.*

```json
{
  "items": {
        
        // Assuming there were two Case Details layout items: 
        // This patch request will remove the one that was omitted
        "Case Details": [
            {
                "id": "52f51059-2a0c-4b5e-a86d-55bf5638a10d"
            }
        ]
    }
}

```

---