---
tags: [Documentation]
---
 
# Plugins
### 1. Plugins Framework
Smokeball offers a simple plugin framework for developers to create custom web views within Smokeball desktop. A Plugin is defined by an integration partner and then accounts can subscribe to the plugin.

Smokeball offers two implementations of a Plugin - Button or Tab. Partners can customize the appearance of the button/tab for what is shown to users.

Plugins are managed through two resources [Plugins](https://smokeball.stoplight.io/docs/api-docs/1f775a2b8e24d-create-a-new-plugin) and [PluginSubscriptions](https://smokeball.stoplight.io/docs/api-docs/e451fe7575947-subscribe-account-to-plugin)

#### 1.1 Placement Keys
One of the most important fields of a plugin is its `placement` key, which defines where the plugin appears in app.

The syntax of a placement key for the plugins API is `page-zone`

##### Desktop Pages
Smokeball desktop supports two pages where plugins can be placed:

1. Main Window, denoted by `Window` key

2. Matter Window, denoted by `MatterWindow` key

##### Desktop Zones
The zones are unique places on a page where content can appear. For instance, a page can have both button zones and tab zones and the placement key determines where the plugin appears in the page.

Smokeball desktop determines how a set of plugins appear in the zone, generally at the end of the existing list. For ribbon buttons, they will be placed into their own ribbon group at the end of the zone.

Matter window zones:

`MatterWindow-MatterDetailsRibbon` - Places a Tab in Matter page

`MatterWindow-MatterDetailsRibbonTab` - Places a Button in the 'Matter' Tab of Matter page

Main window zones:

`Window-SmokeballRibbonTab` - Places a Button in the 'Smokeball' Tab of Main page

`Window-MainTabs` - Places a navigation tab inside the Smokeball left-side tabs

`Window-MainRibbon` - Places a tab in the main window ribbon

#### 1.2 SDK Support

All browsers opened by plugins support integration with the Smokeball SDK as long as the SB browser is used, when `attributes.page.useDefaultBrowser` is `true`.

#### 1.3 Plugins Permissions
Plugins can only be developed by select partners. Smokeball is currently controlling the process of which accounts can access plugins

#### 1.4 Subscribing Accounts
Smokeball accounts are subscribed to plugins through the [PluginSubscriptions](https://smokeball.stoplight.io/docs/api-docs/e451fe7575947-subscribe-account-to-plugin) resource. Partners authorized through the Client Credentials Grant can then subscribe accounts to their plugins. Available plugins are scoped to API client ids, meaning that you can only subscribe to plugins that were created using the same API client id.

#### 1.5 Desktop Synchronization
Users that have subscribed to plugins will always receive the latest version of your plugins, and would be automatically updated to the latest changes.

#### 1.6 Secure Views and URL Loading
A plugin's secure view is one where we can trust to load the plugin because it has been validated by your backend.
##### 1.6.1 Secure View
On a plugin, we support two fields which help establish a secure view to your app.

 `Version.RequestEndpointUrl` and `Key`
 
The `RequestEndpointUrl` is used to request a URL from your backend. 
When a plugin view is loaded, we send a `POST` request to your url with the following payload:
````javascript
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
````
This way your backend can receive information about the current account and user and factor that into your response.
We expect your server to reply with a JSON payload and we will show the `url` value to the user:
``
{
  "url": "https://your-site.com/app"
}
``
#####  1.6.2 Request Validation
If you supplied a `Key` property when you created a Plugin, a header called `Signature` will be included when Smokeball calls your `RequestEndpointUrl`. The signature is a HMAC SHA256 hash that was created with the key that you provided. You can perform the same hashing process on your processing side to verify that the request came from Smokeball.
##### 1.6.3 Computing the HMAC
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

##### 1.6.4 Guarding against replay attacks
A replay attack is when an attacker intercepts a valid payload and its signature, then re-transmits them. To mitigate such attacks Smokeball provides a `Timestamp` header which is the ticks representation (.NET format) of when the request was sent. It is also part of the verification signature so an attacker can not change the timestamp without invalidating the signature.

If the signature is valid but the timestamp is too old, you may choose to reject the request.

### 1.7 API Limitations
We have defined the API payloads, but not all fields are currently in use. For now, please assume these fields are not in use by Smokeball and may be supported in future:

-  `endpoint` field has been deprecated. Use `RequestEndpointUrl` for all new plugins. You can migrate an existing app to the new field by updating the `endpoint` field to null, and set `RequestEndpointUrl` to your new url.

-  `availability` - availability settings are currently not supported
