---
tags: [Documentation]
---

# Webhooks

## 1. Overview

You can use webhook subscriptions to receive notifications about particular events that occur in an account. These event notifications are sent as a HTTP POST to the callback URL that you specify in the subscription.

## 2. Subscribing to events

### 2.1 Overview

Webhook subscriptions can be managed using the `/webhooks` endpoint. To see a list of available events make a GET request to `/webhooks/types` which will return a list of strings representing the events:

```json
{
    "value": [
        "activitycode.created",
        "activitycode.deleted",
        "activitycode.updated",
        "archive.updated",
        "billingconfiguration.updated",
        "contact.created",
        "contact.deleted",
        "contact.restored",
        "contact.updated",
        "expense.created",
        "expense.deleted",
        "expense.updated",
        "event.updated",
        "files.updated",
        "fee.created",
        "fee.deleted",
        "fee.updated",
        "firm.created",
        "firm.updated",
        "firmuser.updated",
        "invoice.finalized",
        "layoutdesign.updated",
        "layoutmatter.created",
        "layoutmatter.updated",
        "mattertype.created",
        "mattertype.deleted",
        "mattertype.restored",
        "mattertype.updated",
        "matter.created",
        "matter.updated",
        "matteritems.updated",
        "matterstage.updated",
        "memo.created",
        "memo.updated",
        "memo.deleted",
        "memo.restored",
        "roles.updated",
        "staff.created",
        "staff.updated",
        "stageset.created",
        "stageset.updated",
        "task.created",
        "task.updated",
        "task.deleted",
        "user.registered",
        "user.unregistered",
        "error"
    ]
}
```

The `/webhooks` endpoint will also allow you to list, edit and delete any subscriptions that you have created.

### 2.2 Creating a subscription
<!-- theme: warning -->
> #### URL Timeout
>
> We will cancel requests to event notification URLs after 10 seconds.
> Please ensure that your event notification URL responds in time.

To create a subscription, make a POST request to the `/webhooks` endpoint:
```json http
{
  "method": "POST",
  "url": "https://api.smokeball.com/webhooks",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{  \r\n  \"name\": \"MyContactsWebhook\",\r\n  \"eventTypes\": [\r\n    \"contact.created\",\r\n    \"contact.updated\",\r\n    \"contact.deleted\",\r\n    \"contact.restored\"\r\n  ],    \r\n  \"eventNotificationUrl\": \"https:\/\/mydomain.com\/webhooks\/contacts\",  \r\n}"
}
```

#### Response

``` json
{
  "id": "82336fc0-8208-406f-b11c-f2b520d081a3",
  "key": "i7641529ue420n8b9",
  "name": "MyContactsWebhook",
  "eventTypes": [
    "contact.created",
    "contact.updated",
    "contact.deleted",
    "contact.restored"
  ],
  "eventNotificationUrl": "https://mydomain.com/webhooks/contacts",
  "createdDateUtc": "2021-05-05T01:36:43.0503643Z",
  "updatedDateUtc": "2021-05-05T01:36:43.0503944Z"
}
```

> **Warning**  
> **Effective May 30, 2025**, Webhook subscriptions will be automatically removed if:  
> - The acquired refresh token has expired.  
> - The user or firm explicitly revokes access to the integration.  
>  
> **Action Required:**  
> - Do not assume Webhook subscriptions persist after a user re-authenticates.  
> - After obtaining a new access token, check if Webhook subscriptions exist.  
> - If they do not exist, recreate them; otherwise, no action is needed.  
> - **Avoid creating duplicate Webhook subscriptions** to prevent redundant load on your endpoints.  
>  
> Ensure your integration follows this workflow to maintain Webhook functionality. If you have any questions, please contact **support@smokeball.com.au**.

## 3. Event handling

### 3.1 Notification schema

A webhook notification captures changes related to the subscribed event and includes specific information about the event. The `payload` in notifications are dependant on the event type. See respective API docs for payload examples.

Here is an example what a typical webhook notification would look like for a `matter.updated` event.

``` json
{    
    "accountId": "1e53979d-9269-4159-b104-2e603670f59d",
    "subscriptionId": "6543c46b-d1d5-4a00-8732-36a63a20c3b4",
    "type": "matter.updated",
    "source": "API",
    "payload": {
        "id": "68df1d38-b9a3-4855-b32f-6af1aae2f258",
        "externalSystemId": null,
        "number": "10001",
        "title": "John Smith | Sale",
        "matterType": {
            "id": "602b31b2-da47-4b17-9ff5-f84f3f3efeec",
            "rel": "MatterTypes"
        },
        "clients": [
            {
                "id": "6aeee3fb-7587-4b33-8570-9d34dbde5d8f",
                "rel": "Contacts"
            }
        ],
        "description": "Sale of Franchise",
        "status": "Open"
    },
    "timestamp": 637981232724209300
}
```  
  
  
Field | Description
---------|----------
 `accountId` | Identifies the specific account associated with the subscription.
 `subscriptionId` | Represents the unique identifier for the subscription.
 `type` | Indicates the nature of the subscribed event.
 `source` |  Specifies the origin of the event - e.g. `Smokeball`, `API`
 `payload` |  This section holds the detailed information pertinent to the event.
 `timestamp`| Captures the exact moment when the event occurred. This is useful for the subscriber to ensure events are processed in order.

### 3.2 Handling duplicate events

Webhook endpoints may occasionally receive the same event more than once. We advise that you guard against this by making your event processing idempotent.

### 3.3 Order of events

Smokeball does not guarantee delivery of events in the order in which they are generated. It is rare but possible that you may receive a `contact.updated` event before a `contact.created` event.

### 3.4 Preventing infinite update loops

If you initiate a change to a resource via the API you will more than likely receive one or more webhook events caused by that change. If you are not careful you might find yourself in an update loop like the following scenario:
1. A change is made to a resource in your system
2. This triggers your code to call the Smokeball API to update the corresponding resource in Smokeball
3. You receive a webhook event from Smokeball for the resource you updated
4. You update the resource in your system
5. This triggers you to call the Smokabll API to update the corresponding resource
6. and so on....

To avoid this scenario it is recommended that you set the `RequestId` header with all of you API requests. If supplied, this header will always be returned in your API responses as well as all webhook events that were triggered from your original request. This allows you to filter or ignore webhook events that were initiaited by your changes.

### 3.5 Error Handling

Because our API request handling is an eventually consistent operation, we supply a `error` webhook event so you can be notified when processing your API request has failed. We provide the name and id of the failed resource and a description of the problem.

A sample of the `error` payload is as follows:
``` json
{
   "accountId": "1e53979d-9269-4159-b104-2e603670f59d",
    "subscriptionId": "6543c46b-d1d5-4a00-8732-36a63a20c3b4",
    "type": "error",
    "source": "API",
    "payload": {
      "message": "Contact does not belong to account 1e53979d-9269-4159-b104-2e603670f59d.",
      "resourceName": "contact",
      "resourceId": "fbd61c97-ee92-49b7-ba8e-84e0ed857078"
    },
    "timestamp": 637981232724209300
}
```

## 4. Verifying webhooks

If you supplied a `key` when you created a subscription, a header called `Signature` will be included when Smokeball calls your callback URL. The signature is a HMAC SHA256 hash that was created with the key that you provided. You can perform the same hashing process on your event processing side to verify that the request came from Smokeball.

### 4.1 Computing the HMAC

The `Signature` hash is calculated using these three bits of information:
1. The `Timestamp` header which is the ticks representation of the UTC datetime (.Net format) the webhook was sent
2. The `RequestId` header
3. Your `ClientId` that you use to authenticate with the API

These three parameters need to be concatenated into a single string, seperated by a `|` and can then be used to create the hash. 
For example:

#### C# Example
``` csharp
//your key you provided in the subscription
var key = "ei7641529ue420n8b9aa";

var timestamp = "637558795239278688";
var requestId = "38583489-09c4-49ef-b58c-ef1b34208cca";
var clientId = "lou1qnn0llav95f";

var payload = "637558795239278688|38583489-09c4-49ef-b58c-ef1b34208cca|lou1qnn0llav95";

var hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
var hash = hmac.ComputeHash(Encoding.UTF8.GetBytes(payload));
return BitConverter.ToString(hash).Replace("-", "").ToLower();
```

#### JavaScript Example
``` javascript
const crypto = require('crypto');

const key = "ei7641529ue420n8b9aa";

const timestamp = "637558795239278688";
const requestId = "38583489-09c4-49ef-b58c-ef1b34208cca";
const clientId = "lou1qnn0llav95f";

const payload = `${timestamp}|${requestId}|${clientId}`;

const hmac = crypto.createHmac('sha256', key);
hmac.update(payload);
const hash = hmac.digest('hex');

return hash;
````


This produces the hash string `feb4b838a272884f6d2c2580b2c7ebb0b2f725b90e8baa6f9b5e1a17a9faec2d`

### 4.2 Guarding against replay attacks

A replay attack is when an attacker intercepts a valid payload and its signature, then re-transmits them. To mitigate such attacks Smokeball provides a `Timestamp` header which is the ticks representation (.Net format) of when the webhook event was sent. It is also part of the verification signature so an attacker can not change the timestamp without invalidating the signature.

If the signature is valid but the timestamp is too old, you may choose to reject the message.
