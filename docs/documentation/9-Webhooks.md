---
tags: [Documentation]
---

# Webhooks

## 1. Overview

You can use webhook subscriptions to receive notifications about particular events that occur in an account. These event notifications are sent as a HTTP POST to the callback URL that you specify in the subscription.

## 2. Subscribing to events

### 2.1 Overview

Webhook subscriptions can be managed using the `/webhooks` endpoint. To see a list of available events make a GET request to `/webhooks/events` which will return a list of strings representing the events:

```json
{
  "value": [
    "firm.updated",
    "staff.created",
    "staff.updated",
    "matter.created",
    "matter.updated",
    "roles.updated",
    "contact.created",
    "contact.updated",
    "contact.deleted",
    "contact.restored"
  ]
}
```

The `/webhooks` endpoint will also allow you to list, edit and delete any subscriptions that you have created.

### 2.2 Creating a subscription

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

** Response **

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

## 3. Event handling

### 3.1 Handling duplicate events

Webhook endpoints may occasionally receive the same event more than once. We advise that you guard against this by making your event processing idempotent.

### 3.2 Order of events

Smokeball does not guarantee delivery of events in the order in which they are generated. It is rare but possible that you may receive a `contact.updated` event before a `contact.created` event.

### 3.3 Preventing infinite update loops

If you initiate a change to a resource via the API you will more than likely receive one or more webhook events caused by that change. If you are not careful you might find yourself in an update loop like the following scenario:
1. A change is made to a resource in your system
2. This triggers your code to call the Smokeball API to update the corresponding resource in Smokeball
3. You receive a webhook event from Smokeball for the resource you updated
4. You update the resource in your system
5. This triggers you to call the Smokabll API to update the corresponding resource
6. and so on....

To avoid this scenario it is recommended that you set the `RequestId` header with all of you API requests. If supplied, this header will always be returned in your API responses as well as all webhook events that were triggered from your original request. This allows you to filter or ignore webhook events that were initiaited by your changes.


## 4. Verifying webhooks

If you supplied a `key` when you created a subscription, a header called `Signature` will be included when Smokeball calls your callback URL. The signature is a HMAC SHA256 hash that was created with the key that you provided. You can perform the same hashing process on your event processing side to verify that the request came from Smokeball.

### 4.1 Computing the HMAC

The `Signature` hash is calculated using these three bits of information:
1. The `Timestamp` header which is the ticks representation of the UTC datetime (.Net format) the webhook was sent
2. The `RequestId` header
3. Your `ClientId` that you use to authenticate with the API

These three parameters need to be concatenated into a single string, seperated by a `|` and can then be used to create the hash. 
For example:

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
This produces the hash string `feb4b838a272884f6d2c2580b2c7ebb0b2f725b90e8baa6f9b5e1a17a9faec2d`

### 4.2 Guarding against replay attacks

A replay attack is when an attacker intercepts a valid payload and its signature, then re-transmits them. To mitigate such attacks Smokeball provides a `Timestamp` header which is the ticks representation (.Net format) of when the webhook event was sent. It is also part of the verification signature so an attacker can not change the timestamp without invalidating the signature. 

If the signature is valid but the timestamp is too old, you may choose to reject the message.
