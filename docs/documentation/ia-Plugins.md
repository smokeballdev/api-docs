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
The zones are unique places on a page where content can appear. For instance, a page can have both button zones and tab zones and the  placement key determines where the plugin appears in the page.
Smokeball desktop determines how a set of plugins appear in the zone, generally at the end of the existing list. For ribbon buttons, they will be placed into their own ribbon group at the end of the zone.

Matter window zones:

`MatterWindow-MatterDetailsRibbon` - Places a Tab in Matter page

`MatterWindow-MatterDetailsRibbonTab` - Places a Button in the 'Matter' Tab of Matter page

Main window zones:

`Window-SmokeballRibbonTab` - Places a Button in the 'Smokeball' Tab of Main page

`Window-LeftDockedTabControl` - Places a navigation tab inside the Smokeball left-side tabs

`Window-Ribbon` - Places a tab in the main window ribbon


#### 1.2 SDK Support
All browsers opened by plugins support integration with the Smokeball SDK as long as the SB browser is used, when `attributes.page.useDefaultBrowser` is `true`.
#### 1.3 Plugins Permissions
Plugins can only be developed by select partners. Smokeball is currently controlling the process of which accounts can access plugins
#### 1.4 Subscribing Accounts
Smokeball accounts are subscribed to plugins through the [PluginSubscriptions](https://smokeball.stoplight.io/docs/api-docs/e451fe7575947-subscribe-account-to-plugin) resource. Partners authorized through the Client Credentials Grant can then subscribe accounts to their plugins. Available plugins are scoped to API client ids, meaning that you can only subscribe to plugins that were created using the same API client id.
#### 1.5 Desktop Synchronization
Users that have subscribed to plugins will always receive the latest version of your plugins, and would be automatically updated to the latest changes.
#### 1.6 Current API Limitations
We have defined the API payloads, but not all fields are currently in use. For now, please assume these fields are not in use by Smokeball and may be supported in future:

 - `endpoint.staging` - just always use `endpoint.production` for now as the API endpoints are region specific already
 - `availability` - availability settings are currently not supported 
