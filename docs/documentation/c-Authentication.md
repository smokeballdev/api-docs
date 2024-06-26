---
tags: [Documentation]
---

# Authentication

## Client Application Details

Smokeball will provide you with your **client_id** and **client_secret** that you will use throughout the oauth2 flow. 

There are two OAuth 2.0 flows used to authenticate with the API based on your integration scenario:

**[Authorization Code Grant](#1-authorization-code-grant)** - This is used to connect to the Smokeball API as an authenticated Smokeball user. The Smokeball user will authenticate using their credentials and an authorization code is passed back to your supplied redirect uri(s) which you can then exchange for an **access_token**.

**[Client Credentials Grant](#2-client-credentials-grant)** - This flow is used when connecting to the Smokeball API for server-to-server requests outside the context of a Smokeball user.

## 1. Authorization Code Grant

### 1.1 Request User Authorization

Direct the user to `https://auth.smokeball.com/oauth2/authorize` with the following parameters: 

Example:

```json http
{
  "method": "get",
  "url": "https://auth.smokeball.com/oauth2/authorize",
  "query": {
    "response_type": "code",
    "client_id": "xxxxx",
    "redirect_uri": "https://xxxxxxxx"
  }
}
```

> Note: Please **do not include** the `scope` query parameter when making an authorize request. This will ensure that all assigned scopes are automatically included in the access token.

The user will be directed to a login page where they will need to use their Smokeball credentials to login.

If authorized, the user will be redirected to:

```html
 <https://your_redirect_uri?code=xxxxx>
```

The response returns a one time use code that is valid for five minutes.

> See <https://docs.aws.amazon.com/cognito/latest/developerguide/authorization-endpoint.html> for more information.

---

### 1.2 Request an Access Token

Once you have obtained an authorization code, you can now use it to get an **access_token** which may be used to make requests to the API.

> Note: The use of **id_token** to make API requests has been deprecated. API requests can now also be made using the **access_token**. For further information, see *section 1.4* below on migration from **id_token** to **access_token**. Please note that the **id_token** will continue to work for the time being and **no immediate action is required**.

Make a POST request to <https://auth.smokeball.com/oauth2/token> with the following parameters and headers:

```json http
{
  "method": "post",
  "url": "https://auth.smokeball.com/oauth2/token",
  "headers": {
    "Authorization": "Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=",
    "Content-Type": "application/x-www-form-urlencoded"    
  },
  "body": {
    "grant_type": "authorization_code",
    "client_id": "xxxx",
    "redirect_uri": "https://xxxxx",
    "code": "the code acquired from step 1"
  }
}
```

> The **Authorization** header must be the string "Basic" followed by your client_id and client_secret with a colon : in between, Base64 Encoded. For example, client_id:client_secret is Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=

**Sample Response**

```json
HTTP/1.1 200 OK
Content-Type: application/json

{ 
 "refresh_token":"dn43ud8uj32nk2je", 
 "access_token":"dmcxd329ujdmkemkd349r",
 "token_type":"Bearer", 
 "expires_in":3600
}
```

The returned **access_token** is valid for 60 minutes.

> See <https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html> for more information

---

### 1.3 Refreshing an Access Token

You can use the **refresh_token** returned in the previous request to continuosly retrieve a valid **access_token** for use with the API. To exchange a **refresh_token** for an **access_token**, make the following request:

```json http
{
  "method": "post",
  "url": "https://auth.smokeball.com/oauth2/token",
  "headers": {
    "Authorization": "Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=",
    "Content-Type": "application/x-www-form-urlencoded"    
  },
  "body": {
    "grant_type": "refresh_token",
    "client_id": "xxxxxx",
    "refresh_token": "xxxxxx"
  }
}
```

**Sample Response**

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
 "access_token":"dmcxd329ujdmkemkd349r",
 "token_type":"Bearer", 
 "expires_in":3600
}
```

> See <https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-user-pool-oauth-2-0-grants/> for more information

### 1.4 Migrating from Id Token to Access Token

The use of **id_token** to make API requests has been deprecated.
We will be phasing out issuing an **id_token** when you request a **token** in the future.

If you have been using an **id_token** for your API requests, **no immediate action is required**. You will be contacted with instructions for transitioning to the **access_token**. You can also [contact us via this link](https://marketplace.smokeball.com/application_forms/api-scope-request-form/partner_applications/new) and provide us with a list of endpoints that you need access to so we can verify that your **access_token** is appropriately configured with the necessary permissions.

> Note: When you transition to **access_token** you will no longer require to include the `scope` query parameter in your user authorization requests. Please **remember to omit** `scope=openid` from your requests to `https://auth.smokeball.com/oauth2/authorize`, to ensure seamless operation. See *section 1.1* above for more information.
---

## 2. Client Credentials Grant

The **Client Credentials** grant type is typically used for server-to-server API operations. In this scenario, you are not acting as a Smokeball user but a type of "service" that can perform operations on an Account level.

### 2.1 Request an Access Token

Make a POST request to <https://auth.smokeball.com/oauth2/token> with the following parameters and headers:

```json http
{
  "method": "post",
  "url": "https://auth.smokeball.com/oauth2/token",
  "headers": {
    "Authorization": "Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=",
    "Content-Type": "application/x-www-form-urlencoded"    
  },
  "body": {
    "grant_type": "client_credentials",
    "client_id": "xxxx"
  }
}
```

> The **Authorization** header must be the string "Basic" followed by your client_id and client_secret with a colon : in between, Base64 Encoded. For example, client_id:client_secret is Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=

**Sample Response**

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "dmcxd329ujdmkemkd349r...",
    "expires_in": 3600,
    "token_type": "Bearer"
}
```

> See <https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html> for more information

---
## 3. Logout Endpoint

### 3.1 Overview

The `/logout` endpoint is a redirection endpoint. It signs the user out and redirects either to an authorized sign-out URL for your app client, or to the /login endpoint. The available parameters in a GET request to the `/logout` endpoint are tailored to Amazon Cognito hosted UI use cases.

> See <https://docs.aws.amazon.com/cognito/latest/developerguide/logout-endpoint.html> for more information

### 3.2 Logout Request

Make a GET request to <https://auth.smokeball.com/oauth2/logout> with the following query parameters:

```json http
{
  "method": "get",
  "url": "https://auth.smokeball.com/oauth2/logout",
  "query": {
    "response_type": "code",
    "client_id": "xxxxx",
    "redirect_uri": "https://xxxxxxxx"
  }
}
```
### 3.3 Request Headers

- **Authorization**: Bearer token

### 3.4 Response

The logout endpoint responds with a status code indicating the success (200) or failure of the logout operation.
