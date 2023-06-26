---
description: This page outlines API Authentication methods and details.
---

# API Authentication

## Table of Contents

| Table of Contents                                                                       |
| --------------------------------------------------------------------------------------- |
| [#1.-authentication-methods](api-authentication.md#1.-authentication-methods "mention") |

## 1. Authentication Methods

The APIs in Cinchy use bearer token based authentication. This token is issued by the Cinchy SSO using the **OAuth 2.0 Resource Owner Password Flow** and can be retrieved for any Cinchy User Account or SSO Account. API calls made using a bearer token will run under the privileges of the authenticated user, and are driven by the configured data level access controls. **You must include** the token in each request in the Authorization header.

APIs that are dynamically generated through a Saved Query in Cinchy also allow for basic authentication. In this case, the url to the saved query is different, it will be:

**`https://<Cinchy Web URL>/BasicAuthAPI/MyDomain/MyQuery`**

The **Resource Owner Password Flow** uses a combination of a client id, client secret, username, and password to authenticate both the calling application as well as the user. To get started with, you must register a client in Cinchy. You should use a different client id for each calling application to distinguish activity from each source.

**If you are on Cinchy v5.5+,** you can also use Cinchy's **Personal Access Token** based authentication. These can be used in [APIs](./) the same way bearer tokens can be.  [Review the documentation here](https://platform.docs.cinchy.com/guides-for-using-cinchy/user-guides/user-preferences/personal-access-tokens) for information on generating PATs.

### 1.1 Registering a New Client

Clients are managed in the **`Integrated Clients`** table within the `Cinchy` domain. To register a client, create a new record in this table. In a fresh install, only members of the **`Cinchy Administrators`** group will have access to perform this function.

Below is a description of the value that should be used for each column in the `Integrated Clients` table.

| Column                          | Description                                                                                                             |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Client Id                       | A unique identifier for each client. The client will use this identifier when retrieving a bearer token.                |
| Client Name                     | A friendly name for the client to help users maintaining this record.                                                   |
| Grant Type                      | The OAuth 2.0 flow that will be used during authentication. "Resource Owner Password" should be selected for API calls. |
| Permitted Login Redirect URLs   | N/A for the Resource Owner Password flow - leave this blank                                                             |
| Permitted Logout Redirect URLs  | N/A for the Resource Owner Password flow - leave this blank                                                             |
| Permitted Scopes                | The list of permitted OAuth scopes, please check all available options.                                                 |
| Access Token Lifetime (seconds) | The time after with the token expires. If left blank, the default is 3600 seconds.                                      |
| Show Cinchy Login Screen        | N/A for the Resource Owner Password flow                                                                                |
| Enabled                         | This is used to enable or disable a client                                                                              |
| Guid                            | This is a calculated field that will auto-generate the client secret                                                    |

### 1.2 API Method Table

#### <mark style="color:green;">**POST:**</mark>** Bearer Token Request**

**https://\<Cinchy SSO URL>/identity/connect/token**

The Post Request will return an access token which can be used to access Cinchy APIs.

#### Header Parameters:

| Name         | Data Type | Description                       |
| ------------ | --------- | --------------------------------- |
| Content-Type | string    | application/x-www-form-urlencoded |

#### Body Parameters:

| Name           | Data Type | Description                                                                                           |
| -------------- | --------- | ----------------------------------------------------------------------------------------------------- |
| token          | string    | You can pass in your base64 encoded SAML token instead of your Cinchy username and password           |
| client\_id     | string    | Client Id value from Integrated Clients table                                                         |
| client\_secret | string    | Guid value from Integrated Clients table                                                              |
| username       | string    | Username of Cinchy user                                                                               |
| password       | string    | Password for Cinchy user in plain text                                                                |
| grant\_type    | string    | Set as "password" for username/password authentication. Set as "saml2" for saml token authentication. |
| scope          | string    | Set as "js\_api"                                                                                      |

#### Responses:

* <mark style="color:green;">200:</mark> The request is successful

{% hint style="info" %}
The expiration time is denoted in seconds.
{% endhint %}

```
// {
    "access_token": "eyUzI1NiIsImtpZCI6IkE4M0UwQTFEQTY1MzE0NkZENUQxOTFDMzRDNTQ0RDJDODYyMzMzMzkiLCJ0eXAiOiJKV1QiLCJ4NXQiOiJxRDRLSGFaVEZHX1YwWkhEVEZSTkxJWWpNemsifQ.eyJuYmYiOjE1NTQxMzE4MjAsImV4cCI6MTU1NDEzNTQyMCwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgxIiwiYXVkIjpbImh0dHA6Ly9sb2NhbGhvc3Q6ODA4MS9yZXNvdXJjZXMiLCJqc19hcGkiXSwiY2xpZW50X2lkIjoiYWJjIiwic3ViIjoiMSIsImF1dGhfdGltZSI6MTU1NDEzMTgyMCwiaWRwIjoibG9jYWwiLCJwcm9maWxlIjoiQWRtaW5pc3RyYXRvciIsImVtYWlsIjoiYWRtaW5AY2luY2h5LmNvIiwicm9sZSI6IkNpbmNoeSBVc2VyIEFjY291bnQiLCJpZCI6ImFkbWluIiwic2NvcGUiOlsianNfYXBpIl0sImFtciI6WyJjdXN0b20iXX0.N7drAlvtFiQoN4njs1rd5ZnTvJ_x8ZEnUEi6G1GjR4FS5FyS4hC6xdsT-Zhn1yRJQMkI2HA7HMPWwjsfkZ0IlBwuC25ECkGhbjv7DlK6baHQIkqeB0aTB9aDZSxWfDhV66O0dhby6EIEa4YuGspyjQMsDpx_LimmE9alfsUU-608944ZZkS6lBJlJ9LFCC5hYKARQIMZavrftz0tFUBsDU0T2fHpLNGo5GGwG1f9jUZTWTu7s3C05EsgboW3scUfDzjS_Wf55ExwhopIg9SD6ktHYYNRaCPtfMhU-e43l6a2LH-XrmP7OfoxJP2bvTMcvQCQWUEizKHuxKLl-ehWBw",
    "expires_in": 3600,
    "token_type": "Bearer"
}
```

*   <mark style="color:orange;">400:</mark> For invalid parameters, a 400 error will be returned with the following JSON response with a description of the error.

    * Example:

    ```
    {
      "error": "invalid_grant",
      "error_description": "Invalid username or password"
    }
    ```

{% hint style="info" %}
#### **To get a bearer token from Cinchy, you can provide either:**

* Username and password (`username`, `password`), or
* SAML token (`token`)

Failure to provide a valid set of one of the above will not return a token.
{% endhint %}
