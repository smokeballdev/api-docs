---
tags: [Documentation]
---
 
# Plugins

Smokeball offers a simple Plugin framework for developers to create custom web views within Smokeball desktop. A Plugin is defined by an integration partner, and can be assigned or subscribed to by multiple Smokeball firms.

Smokeball offers two implementations of a Plugin - Button or Tab. Partners can customize the appearance of the button/tab for what is shown to users.

Plugins are managed through two resources:

[Plugins](https://smokeball.stoplight.io/docs/api-docs/1f775a2b8e24d-create-a-new-plugin) for creating or managing Plugins

[PluginSubscriptions](https://smokeball.stoplight.io/docs/api-docs/e451fe7575947-subscribe-account-to-plugin) for assigning a Plugin to one or many Smokeball firms.

## 1. Placement Keys
One of the most important fields of a plugin is its `placement` key, which defines where the plugin appears in Smokeball.

The syntax of a placement key for the Plugins API is `page-zone`. Pages and Zones are described in further detail below:

### 1.1 Desktop Pages
Smokeball desktop supports two pages where plugins can be placed:

1. Main Window, denoted by `Window` key

2. Matter Window, denoted by `MatterWindow` key

### 1.2 Desktop Zones
The zones are unique places on a page where content can appear. For instance, a page can have both button zones and tab zones and the placement key determines where the Plugin appears in the page.

Smokeball  determines how a set of plugins appear in the zone, generally at the end of the existing list. For ribbon buttons, they will be placed into their own ribbon group at the end of the zone.

Below are the supported Matter window zones:

`MatterWindow-MatterDetailsRibbon` - Places a Tab in Matter page

`MatterWindow-MatterDetailsRibbonTab` - Places a Button in the 'Matter' Tab of Matter page

Below are the supported Main window zones:

`Window-SmokeballRibbonTab` - Places a Button in the 'Smokeball' Tab of Main page

`Window-MainTabs` - Places a navigation tab inside the Smokeball left-side tabs

`Window-MainRibbon` - Places a tab in the main window ribbon

## 2. SDK Support
All browsers opened by Plugins support integration with the Smokeball SDK as long as the internal browser is used, when `attributes.page.useDefaultBrowser` is `true`.
See [SDK Documentation](https://smokeball.stoplight.io/docs/sdk-docs/) for usage information.

## 3. Creating a Plugin
Currently, Plugins can only be created and maintained by select partners. Smokeball is currently controlling the process of which accounts can access plugins.

## 4. Subscribing Smokeball Firms to Plugins
Smokeball firms are subscribed to plugins through the [PluginSubscriptions](https://smokeball.stoplight.io/docs/api-docs/e451fe7575947-subscribe-account-to-plugin) resource. Partners authorized through the Client Credentials Grant can then subscribe accounts to their plugins. Available plugins are scoped to API client ids, meaning that you can only subscribe to plugins that were created using the same API client id.

Smokeball firms that have subscribed to plugins will always receive the latest version of your plugins, and would be automatically updated to the latest changes.

## 5. Loading your Plugin
When a Plugin is accessed in Smokeball, a request is dynamically made to the endpoint defined in the Plugins `requestEndpointUrl` field. The url returned by this request is then loaded into Smokeball.

For more detailed usage documentation please refer to the below.

### 5.1 Requesting your URL
Plugins support two fields which help load your view into Smokeball.

Field | Description
---------|----------
 `requestEndpointUrl` | Used to request a URL from your backend.
 `key` | Used to calculate a signature which must be used by your backend to verify that the request is from Smokeball.
 
When a Plugin view is loaded, a `POST` request is made to the supplied `requestEndpointUrl` url with the a payload similar to below:
``` json
{
  "method": "POST",
  "path": "/",
  "query": {},
  "url": "https://your.app",
  "headers": {
    "signature": "3cafe40f92be6ac77d2792b4b267c2da11e3f3087b93bb19c6c5133786984b44",
    "requestid": "e56f2c3f-b6de-4310-a7a2-c139d62f9711",
    "timestamp": "638609288928990639",
    "content-type": "application/json; charset=utf-8"
  },
  "body": {
    "accountId": "a58f2f17-f138-4432-9965-d2cb539cde4d",
    "userId": "b3d70217-791f-4d82-9fb8-2fcff5c04e58",
    "userEmail": "user@organization.com"
  }
}
```
Your endpoint should use this information to generate a URL to be viewed by the user. This url should be returned in the JSON response like so:
``` json
{
  "url": "https://your-site.com/app"
}
```
> It is critical that the above structure is used in the response payload. If an incorrect format is used or the URL is missin, the user will not be able to use your app.

###  5.2 Request Validation
It is important that you verify that the URL request is from Smokeball to prevent malicious usage of your endpoint.

If you supplied a `Key` property when creating a Plugin, a header named `Signature` will be included when Smokeball calls your `requestEndpointUrl`. The signature is a HMAC SHA256 hash that was created with the key that you provided. You can perform the same hashing process on your processing side to verify that the request came from Smokeball.

### 5.3 Computing the HMAC
The `Signature` hash is calculated using these three bits of information:
1. The `Timestamp` header which is the ticks representation of the UTC datetime (.Net format) the webhook was sent
2. The `RequestId` header
3. Your `ClientId` that you use to authenticate with the API

These three parameters need to be concatenated into a single string, seperated by a `|` and can then be used to create the hash. 
##### C# Example
``` csharp
// Key you provided in the Plugion creation
var key = "ei7641529ue420n8b9aa";

var timestamp = "638609288928990639";
var requestId = "e56f2c3f-b6de-4310-a7a2-c139d62f9711";
var clientId = "lou1qnn0llav95f";

var payload = $"{timestamp}|{requestId}|{clientId}";
var hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
var hash = hmac.ComputeHash(Encoding.UTF8.GetBytes(payload));
return BitConverter.ToString(hash).Replace("-", "").ToLower();
```

##### JavaScript Example
``` javascript
const crypto = require('crypto');

const key = "ei7641529ue420n8b9aa";

const timestamp = "638609288928990639";
const requestId = "e56f2c3f-b6de-4310-a7a2-c139d62f9711";
const clientId = "lou1qnn0llav95f";

const payload = `${timestamp}|${requestId}|${clientId}`;
const hmac = crypto.createHmac('sha256', key);
hmac.update(payload);
const hash = hmac.digest('hex');

return hash;
```

This produces the hash string `58817681863148b0e624c00f3094f99e1af31cd7b99a3c2e0655d64a2764d650`

### 5.4 Guarding against replay attacks
A replay attack is when an attacker intercepts a valid payload and its signature, then re-transmits them. To mitigate such attacks Smokeball provides a `Timestamp` header which is the ticks representation (.NET format) of when the request was sent. It is also part of the verification signature so an attacker can not change the timestamp without invalidating the signature.

If the signature is valid but the timestamp is too old, you may choose to reject the request.

## 6 Limitations

### 6.1 API
Not all Plugin API fields are currently in use. For now, please assume these fields are not in use by Smokeball and may be supported in future:

-  `endpoint` field has been deprecated. Use `RequestEndpointUrl` for all new plugins. You can migrate an existing app to the new field by updating the `endpoint` field to null, and set `RequestEndpointUrl` to your new url.
-  `availability` - availability settings are currently not supported.

### 6.2 Web App
Plugins are currently not supported in the Smokeball web app.