# Authentication

![stuff](../assets/images/smokeball-logo-r.png)

## Client Application Details

Once you have supplied your oauth2 callback url(s), Smokeball will supply you with your **client_id** that you will use throughout the oauth2 flow.

## Obtaining Authorization

### 1. Request User Authorization

Direct the user to https://datastaging-auth.smokeball.com/oauth2/authorize with the following parameters:


Parameter | Description
---------|----------
 response_type | code
 redirect_uri | The URI that was provided to Smokeball when creating your oauth client
 client_id | your client_id provided by Smokeball

Example:

`https://datastaging-auth.smokeball.com/oauth2/authorize?response_type=code&state=&client_id=1e3u4loorcnmnrikmra17928vn&scope=openid&redirect_uri=https%3A%2F%2Fstagingapi.smokeball.com%2Fswagger%2Foauth2-redirect.html`



https://docs.aws.amazon.com/cognito/latest/developerguide/authorization-endpoint.html

https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html


https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-user-pool-oauth-2-0-grants/

