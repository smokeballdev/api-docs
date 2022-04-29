<!-- Do not edit this file. It is automatically generated by API Documenter. -->

[Home](./index.md) &gt; [smokeball-client-sdk](./smokeball-client-sdk.md) &gt; [contacts](./smokeball-client-sdk.contacts.md) &gt; [Api](./smokeball-client-sdk.contacts.api.md) &gt; [update](./smokeball-client-sdk.contacts.api.update.md)

## contacts.Api.update() method

Updates the contact associated to the specified contact id.

<b>Signature:</b>

```typescript
update(request: UpdateContactRequest): Promise<void>;
```

## Parameters

|  Parameter | Type | Description |
|  --- | --- | --- |
|  request | [UpdateContactRequest](./smokeball-client-sdk.contacts.updatecontactrequest.md) | the update request. |

<b>Returns:</b>

Promise&lt;void&gt;

## Example


```
const request: UpdateContactRequest = {
 contactId: 'e9b9084b-c9b4-4f3c-9f5a-4c83ed3ac265'
 // Specify the fields to update here. Fields that are missing are ignored.
};
await sdk.contacts.update(request);
```
