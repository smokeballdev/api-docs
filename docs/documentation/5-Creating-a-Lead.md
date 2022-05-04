---
tags: [Documentation]
---

# Creating a Lead

### 1. Lead Matter Types

Smokeball comes with a range of pre-configured lead matter types for different areas of law. These are provided by Smokeball’s internal content team and are not user editable.

A lead matter type is used to assign the lead being created to a relevant area of law and state / location.

> The matter types used when creating a 'Lead' are different from the one's used to create a 'Matter'.

![Lead Matter Types](../../assets/images/leadmattertypes.png)

When creating a lead in Smokeball we can see a list of lead Matter Types. Here, _Company Formation_ is a lead matter type, under the _Business_ category.


#### API - Getting a Lead Matter Type

```json http
{
  "method": "get",
  "url": "https://stagingapi.smokeball.com/mattertypes",
  "query": {
    "Location": "IL",
    "Category": "Real Estate",
    "Type": 1   // use this parameter to get lead specific matter types - where 1=LeadType
  }
}
```

**Response**

```json
{
  "href": "https://stagingapi.smokeball.com/mattertypes",
  "offset": 0,
  "limit": 500,
  "size": 18,
  "first": {
    "href": "https://stagingapi.smokeball.com/mattertypes"
  },
  "value": [
    {
      "href": "https://stagingapi.smokeball.com/mattertypes/8aca3574-aecb-4a6f-991d-680861bece1d_il",
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
      "href": "https://stagingapi.smokeball.com/mattertypes/2d5aca9b-9469-44d4-8616-766a864432aa",
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

> Here the important information to note is the `id` and `representativeOptions` of the lead matter type. We’ll need those when creating a lead. 

---

### 2. Contacts

In order to successfully create a lead, we’re going to need to provide a contact that is the potential client.

In Smokeball’s desktop app you can create a contact as a Person, Firm/Business/Organization or Trust.
Right now, the API supports the following contact types

- Person
- Company

![New Contact](../../assets/images/newcontact.png)

#### API - Creating a Contact

To create a contact you can make the following request which would create a person called ‘Test Contact’.

```json http
{
  "method": "POST",
  "url": "https://stagingapi.smokeball.com/contacts",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{\n  \"person\": {\n    \"firstName\": \"Test\",\n    \"lastName\": \"Contact\",    \n    \"email\": \"test@gmail.com\",\n  }\n}\n\n"
}
```

**Response**

```json
202 Accepted

{
  "id": "9a9ce552-6bee-45e4-8eb1-afe2c18c489f",
  "href": "https://stagingapi.smokeball.com/contacts/9a9ce552-6bee-45e4-8eb1-afe2c18c489f"
}
```

Take note of the Contact’s `id` returned in the response, we’ll need that when creating a lead.
Alternatively, if the contact already exists in Smokeball, you can query it to find it’s id.

> ### _Tip_
>
> When creating a contact you can pass in an `externalSystemId` if you want to store your own reference id with this contact.

---

### 3. Staff

In Smokeball, staff can be assigned to a lead as the **Attorney Responsible** or the **Person Assisting**. The lead list can then be filtered by Attorney responsible, etc.

![Staff](../../assets/images/staff.png)

If you want to specify the Attorney Responsible or Person Assisting when creating a lead via the API, then we will first need to retrieve the staff member’s id.

```json http
{
  "method": "get",
  "url": "https://stagingapi.smokeball.com/staff"
}
```

**Response**

```json
{
  "href": "https://stagingapi.smokeball.com/staff",
  "size": 28,
  "first": {
    "href": "https://stagingapi.smokeball.com/staff"
  },
  "value": [

    {
      "href": "https://stagingapi.smokeball.com/staff/66f7fa24-bb03-4d89-ac28-e19620cc1e45",
      "id": "66f7fa24-bb03-4d89-ac28-e19620cc1e45",
      "versionId": "cfdbb798-1f2e-4355-87bc-620cbc62acc2",
      "firstName": "Test",
      "lastName": "Staff",
      "phone": {
        "number": "200518540309"
      },
      "cell": {},
      "email": "teststaff@gmail.com",
      "avatar": "https://staging.smokeball.com/FirmManagement/Firm/RedirectToStaffProfileImageUrl?accountId=2f3143de-2bfe-4742-9e9c-7c16b48bdef7&staffId=66f7fa24-bb03-4d89-ac28-e19620cc1e45",
      "former": false,
      "enabled": true
    },

    {
      "href": "https://stagingapi.smokeball.com/staff/c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "id": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "versionId": "23dbdc50-6171-4408-8150-413cb293a249",
      "title": "",
      "firstName": "Hunter",
      "lastName": "Steele",
      "email": "hunter@gmail.com",
      "avatar": "https://staging.smokeball.com/FirmManagement/Firm/RedirectToStaffProfileImageUrl?accountId=2f3143de-2bfe-4742-9e9c-7c16b48bdef7&staffId=c85d28cb-a760-4627-aa59-0a853c2e65ed",
      "former": false,
      "enabled": false
    },
    ...
  ]
}
```

---

### 4. Lead Creation

Now we have everything we need to create a lead.

```json http
{
  "method": "POST",
  "url": "https://stagingapi.smokeball.com/matters",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "matterTypeId": "8aca3574-aecb-4a6f-991d-680861bece1d_IL",
    "clientIds": [
      "9a9ce552-6bee-45e4-8eb1-afe2c18c489f"
    ],
    "clientRole": "Borrower",
    "description": "Created via API",
    "status": "Open",
    "dateOpened": "2020-05-20T04:20:55.035Z",
    "personResponsibleStaffId": "c85d28cb-a760-4627-aa59-0a853c2e65ed",
    "personAssistingStaffId": "66f7fa24-bb03-4d89-ac28-e19620cc1e45",
    "isLead": true
  }
}
```

Here we have used:


Parameter | Description
---------|----------
 `matterTypeId` | We're using the id for `Financing` which we retrieved earlier 
 `clientIds` | An array containing the id of the `Test Contact` we created 
 `clientRole` |  Set to _Borrower_ which is one of the lead matter type’s representativeOptions retrieved earlier
 `personResponsibleStaffId`| Id of a staff member we retrieved from the GET Staff call
 `personAssistingStaffId` | Id of a staff member we retrieved from the GET Staff call
 `description` | A free text entry field for the description of the lead
 `status` | Can be `Open`, `Pending`, `Closed`, `Deleted` or `Cancelled`
 `isLead` | This is a boolean flag to indicate that the matter is being created as a `Lead`. This must be set to `true` when creating a `Lead`
 `dateOpened` | when the lead was opened (this can be backdated if required)

**Response**

```json
202 Accepted

{
  "id": "51cefb72-6247-4ca2-8926-5d14d65f7cb9",
  "href": "https://stagingapi.smokeball.com/matters/51cefb72-6247-4ca2-8926-5d14d65f7cb9"
}
```
