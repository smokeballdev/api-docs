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

By default, the returned **refresh_troken** is valid for 30 days. If you require a longer expiration period, please contact us to discuss your specific needs.

> **Important**: Your application is responsible for monitoring the expiration of the refresh token and prompting the user to re-authenticate when it expires. This ensures continuous access to the application without interruptions. See <https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html> for more information

### 1.2.1 PKCE (Proof Key for Code Exchange)
PKCE is an extension of the Authorization Code Grant flow, designed for mobile and single-page applications where storing a client secret securely is difficult. It ensures a more secure flow by requiring a dynamically generated code verifier and code challenge. It is recommended that all authorization code flows are authorized using PKCE and it is *required* for all public api clients, which do not have a client secret.

### 1.2.1.1 Generate a Code Verifier and Code Challenge
To initiate the PKCE flow, you must generate a **code_verifier**, which is a random string, and a **code_challenge**, which is derived by hashing the **code_verifier** using SHA-256 and then Base64-URL encoding it.

##### C# Example
``` csharp
using System;
using System.Security.Cryptography;
using System.Text;

class PKCEGenerator
{
    public static void Main()
    {
        string codeVerifier = GenerateCodeVerifier();
        string codeChallenge = GenerateCodeChallenge(codeVerifier);

        Console.WriteLine("Code Verifier: " + codeVerifier);
        Console.WriteLine("Code Challenge: " + codeChallenge);
    }

    // Generate a random code verifier
    private static string GenerateCodeVerifier()
    {
        var randomBytes = new byte[32]; // 32-byte random string
        using (var rng = RandomNumberGenerator.Create())
        {
            rng.GetBytes(randomBytes);
        }
        return Base64UrlEncode(randomBytes);
    }

    // Create code challenge using SHA256
    private static string GenerateCodeChallenge(string codeVerifier)
    {
        using (var sha256 = SHA256.Create())
        {
            byte[] challengeBytes = sha256.ComputeHash(Encoding.UTF8.GetBytes(codeVerifier));
            return Base64UrlEncode(challengeBytes);
        }
    }

    // Base64 URL Encoding (removes padding, uses '-' and '_')
    private static string Base64UrlEncode(byte[] bytes)
    {
        return Convert.ToBase64String(bytes)
            .Replace("+", "-")
            .Replace("/", "_")
            .TrimEnd('=');
    }
}
```

##### JavaScript Example
``` javascript
// Function to generate a random code verifier
function generateCodeVerifier() {
    const array = new Uint8Array(32); // 32-byte random string
    window.crypto.getRandomValues(array);
    return base64UrlEncode(array);
}

// Function to create a code challenge from the code verifier using SHA256
async function generateCodeChallenge(codeVerifier) {
    const encoder = new TextEncoder();
    const data = encoder.encode(codeVerifier);
    const digest = await crypto.subtle.digest('SHA-256', data);
    return base64UrlEncode(new Uint8Array(digest));
}

// Function to Base64 URL encode (removes padding, uses '-' and '_')
function base64UrlEncode(arrayBuffer) {
    let base64 = btoa(String.fromCharCode.apply(null, arrayBuffer))
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=+$/, '');
    return base64;
}

// Example usage
(async () => {
    const codeVerifier = generateCodeVerifier();
    const codeChallenge = await generateCodeChallenge(codeVerifier);

    console.log('Code Verifier:', codeVerifier);
    console.log('Code Challenge:', codeChallenge);
})();
```
### 1.2.1.2 Request Authorization Code for Access Token
Direct the user to `https://auth.smokeball.com/oauth2/authorize` with the following parameters: 

```json
{
  "method": "get",
  "url": "https://auth.smokeball.com/oauth2/authorize",
  "query": {
    "response_type": "code",
    "client_id": "xxxxx",
    "redirect_uri": "https://your_redirect_uri",
    "code_challenge_method": "S256",
    "code_challenge": "your_code_challenge"
  }
}
```
For Smokeball API, only `S256` method is allowed for `code_challenge_method`

The user will be directed to the login page, and if authorized, redirected back with a code.

### 1.2.1.3 Exchange Authorization Code for Access Token
After receiving the authorization code, use it along with the code_verifier to request an access token:
```json
{
  "method": "post",
  "url": "https://auth.smokeball.com/oauth2/token",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  "body": {
    "grant_type": "authorization_code",
    "client_id": "xxxxx",
    "code": "authorization_code_from_previous_step",
    "redirect_uri": "https://your_redirect_uri",
    "code_verifier": "your_code_verifier"
  }
}
```
You will receive an access token that can be used to authenticate API requests.

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
