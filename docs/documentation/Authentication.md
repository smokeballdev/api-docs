---
tags: [Documentation]
---

# Authentication

## Client Application Details

Once you have supplied your oauth2 callback url(s), Smokeball will provide you with your **client_id** and **client_secret** that you will use throughout the oauth2 flow.

## Obtaining Authorization

### 1. Request User Authorization

Direct the user to `https://datastaging-auth.smokeball.com/oauth2/authorize` with the following parameters:

Example:

```json http
{
  "method": "get",
  "url": "https://datastaging-auth.smokeball.com/oauth2/authorize",
  "query": {
    "response_type": "code",
    "client_id": "xxxxx",
    "scope": "openid",
    "redirect_uri": "https://xxxxxxxx"
  }
}
```

The user will be directed to a login page where they will need to use their Smokeball credentials to login.

If authorized, the user will be redirected to:

```html
 <https://your_redirect_uri?code=xxxxx>
```

> See <https://docs.aws.amazon.com/cognito/latest/developerguide/authorization-endpoint.html> for more information.

---

### 2. Request an Id Token

Once you have obtained an authorization code, you can now use it to get an **id_token** which may be used to make requests to the API.

Make a POST request to <https://datastaging-auth.smokeball.com/oauth2/token> with the following parameters and headers:

```json http
{
  "method": "post",
  "url": "https://datastaging-auth.smokeball.com/oauth2/token",
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
 "id_token":"dmcxd329ujdmkemkd349r",
 "token_type":"Bearer", 
 "expires_in":3600
}
```

The returned **id_token** is valid for 60 minutes but the **refresh_token** is valid for the time specified in the **expires_in** field. 

<!-- theme: warning -->

> ### \*\*Note
>
> The Smokeball API will _only_ accept the **id_token** and not the access_token
>
> See <https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html> for more information

---

### 3. Refreshing an Id Token

You can use the **refresh_token** returned in the previous request to continuosly retrieve a valid **id_token** for use with the API. To exchange a **refresh_token** for an **id_token**, make the following request:

```json http
{
  "method": "post",
  "url": "https://datastaging-auth.smokeball.com/oauth2/token",
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
 "refresh_token":"dn43ud8uj32nk2je",
 "id_token":"dmcxd329ujdmkemkd349r",
 "token_type":"Bearer", 
 "expires_in":3600
}
```

<https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-user-pool-oauth-2-0-grants/>
