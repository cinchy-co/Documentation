---
description: This page gives an overview of APIs.
---

# API Overview

## **1.** Introduction

There are various APIs available and supported by Cinchy, each performing important functions for your use cases.

For example, using the [MyQuery](./#2.7-api-mydomain-myquery) API will allow you to turn any Saved Query on your platform into a REST API.

Many of the APIs listed on this page can utilize **Bearer Tokens** or **Personal Access Tokens** to authenticate. Please review the [API Authentication](api-authentication.md) page for further information.

## 2. **Master** List of Endpoints

The following is a list of common API endpoints. These follow the format of \<baseurl>/endpoint

* [/admin/index](./#2.1-admin-index)
* [/apps/modelloader](./#2.2-apps-modelloader)
* [/healthcheck](./#2.3-healthcheck)
* [/metaforms/healthcheck](./#2.4-metaforms-healthcheck)
* [/cache/clear](./#2.4-cache-clear)
* [/API/ExecuteCQL](./#2.5-api-executecql)
* [/API/MyDomain/MyQuery](./#2.6-api-mydomain-myquery)
* [/identity/connect/token](./#2.7)
* [/api/getstsauthorityuri](./#2.8-api-getstsauthorityuri)
* [/api/jobs](./#2.10-api-jobs)

### **2.1 **_**/admin/index**_

| Description                                                             |
| ----------------------------------------------------------------------- |
| This endpoint will bring you to a page with your logs and health check. |

### **2.2 **_**/apps/modelloader**_

| Description                               |
| ----------------------------------------- |
| This endpoint will open the model loader. |

### 2.3 _/healthcheck_

| Description                                                             |
| ----------------------------------------------------------------------- |
| This endpoint will bring you to a page with your platform health check. |

### **2.4** /metaforms/healthcheck

| Description                                                               |
| ------------------------------------------------------------------------- |
| This endpoint will bring you to a page with your Meta-Forms health check. |

### **2.5 **_**/cache/clear**_

| Description                                         |
| --------------------------------------------------- |
| This endpoint will delete your platform cache data. |

### 2.6 [_**/API/ExecuteCQL**_](executecql.md)

[**POST: https://\<Cinchy Web URL>/API/ExecuteCQL**](executecql.md)

<table><thead><tr><th width="150">Description</th></tr></thead><tbody><tr><td>You can execute CQL directly without creating a Saved Query using the below endpoint:</td></tr></tbody></table>

**Query Parameters:**



<table><thead><tr><th width="187.33333333333331">Name</th><th width="170">Data Type</th><th>Description</th></tr></thead><tbody><tr><td>CompressJSON</td><td>boolean</td><td>Default is true. Add this parameter and set to false if you want the JSON that is returned to be expanded rather than having the schema being returned separately.</td></tr><tr><td>ResultFormat</td><td>string</td><td>XML<br>JSON<br>CSV<br>TSV<br>PSV<br>PROTOBUF</td></tr><tr><td>Type</td><td>string</td><td>QUERY <br><em>- Query (Approved Data Only)</em> <br>DRAFT_QUERY <br><em>- Query (Include Draft Changes)</em> <br>SCALAR <br><em>- Scalar</em> <br>NONQUERY <br><em>- Non Query, such as an insert or delete</em> <br>VERSION_HISTORY_QUERY <br><em>- Query (Include Version History)</em></td></tr><tr><td>ConnectionId</td><td>string</td><td>​</td></tr><tr><td>TransactionId</td><td>string</td><td>When one or more requests share the same TransactionId, they are considered to be within the scope of a single transaction.</td></tr><tr><td>Query</td><td>string</td><td>The CQL query statement to execute</td></tr><tr><td>Parameters</td><td>boolean</td><td>See below on format for the parameters.</td></tr><tr><td>SchemaOnly</td><td>integer</td><td>Defaults to false.</td></tr><tr><td>StartRow</td><td>integer</td><td>When implementing pagination, specify a starting offset. Combine with <code>RowCount</code> to set the size of the data window.</td></tr><tr><td>RowCount</td><td>integer</td><td>When implementing pagination, specify the number of rows to retrieve for the current page. Combine with <code>StartRow</code> to set the paging position.</td></tr><tr><td>CommandTimeout</td><td>string</td><td>Use this parameter to override the default timeout (30s) for long running queries. In seconds.</td></tr><tr><td>UserId</td><td>string</td><td>ID of a user with authorization to run the saved query.</td></tr></tbody></table>

**Header Parameters:**

| Name          | Data Type | Description                                                                                                                                                                                                                                                                                      |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Authorization | string    | <p>Bearer &#x3C;access_token><br><br>The access token can be either a Bearer token or a <a href="../../guides-for-using-cinchy/user-guides/user-preferences/personal-access-tokens.md">Personal Access token.</a><br><br>See <a href="api-authentication.md">Authentication</a> for details.</p> |

#### Responses:

* <mark style="color:green;">200 (OK)</mark>

### Parameters <a href="#parameters" id="parameters"></a>

To pass in parameters in your executeCQL, you will need to pass in sets of parameters in the following format. So if you have one parameter then you would pass in 3 query parameters beginning with `Parameters[0].` , and if you have a second parameter you would include an additional 3 query parameters beginning with `Parameters[1].` .

| Query String Parameter Name       | Content                                                                                                      |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Parameters\[n].ParameterName      | <p>Name of the parameter that is in your query, including the '@'.<br>Ex. <code>@name</code></p>             |
| Parameters\[n].XmlSerializedValue | <p>XML Serialized version of the value of that parameter.<br>Ex. <code>&#x26;quot;test&#x26;quot;</code></p> |
| Parameters\[n].ValueType          | <p>Datatype of the value.<br>Ex. <code>System.String</code></p>                                              |

### **2.7** [_**/API/MyDomain/MyQuery**_](api-saved-queries.md#how-to-use-the-access-token-to-call-cinchy-apis)

**GET: https://\<Cinchy Web URL>/API/MyDomain/MyQuery**

| Description                                                                                                                                                                        |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| To access any Cinchy Saved Query API, pass the access token, received from either the Bearer Token Request or created as a PAT, in the Authorization header, prefixed by "Bearer". |

**Query Parameters**

<table><thead><tr><th width="276.3333333333333">Name</th><th>Data Type</th><th>Description</th></tr></thead><tbody><tr><td>WrapSingleRecordInArray</td><td>boolean</td><td>Default is true. Add this parameter and set to false if you want single record results returned as an object instead of within an array.</td></tr><tr><td>@param</td><td>string</td><td>If you have parameters in your query, you pass them indirectly as query parameters.</td></tr><tr><td>CompressJSON</td><td>boolean</td><td>Default is true. Add this parameter and set to false if you want the JSON that is returned to be expanded rather than having the schema being returned separately.</td></tr></tbody></table>

**Header Parameters**

| Name          | Data Type | Description                                                                                                                                                                                                                                                                                      |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Authorization | string    | <p>Bearer &#x3C;access_token><br><br>The access token can be either a Bearer token or a <a href="../../guides-for-using-cinchy/user-guides/user-preferences/personal-access-tokens.md">Personal Access token.</a><br><br>See <a href="api-authentication.md">Authentication</a> for details.</p> |

**Responses**

<mark style="color:green;">**200:**</mark>** The request has successfully returned the record set.**

```
< HTTP/2 200 
< cache-control: private, s-maxage=0
< content-type: application/json; charset=utf-8
< server: Microsoft-IIS/10.0
< x-aspnetmvc-version: 5.2
< access-control-allow-origin: *
< x-aspnet-version: 4.0.30319
< x-powered-by: ASP.NET
< date: Wed, 1 Aug 2020 17:40:13 GMT
< content-length: 2985

```

<mark style="color:orange;">**400:**</mark>** The request could not be understood, the client is sending a request with incomplete data, poorly constructed data or invalid data.**

Optional Validation Logic: To validate query business / control conditional logic failure can be added at the beginning of the API which can intentionally generate a 400 error code (using RAISERROR) and stopping (using RETURN) the API. If there is no RETURN the errors will accumulate and will be provided at the end of running the API.

An attribute (X-Cinchy Error) will be returned in the HTTP header with the custom RAISERROR message indicated (see example)

```
Example:
--given the following CQL used within a saved query:

IF (ISNULL(@Int,0) = 0)
BEGIN
  RAISERROR('Invalid parameter: @Int cannot be NULL or zero',18,0)
  RETURN
END
IF (ISNULL(@String,'') = '' OR ISNULL(@String,'') = 'X')
BEGIN
  RAISERROR('Invalid parameter: @String cannot be NULL or empty or X',18,0)
  RETURN
END

--note "X-Cinchy-Error" in the sample response:

< HTTP/1.1 400 Bad Request
< Cache-Control: private
< Content-Type: text/html; charset=utf-8
< Server: Microsoft-IIS/10.0
< X-AspNetMvc-Version: 5.2
< Access-Control-Allow-Origin: *
< X-Cinchy-Error: Invalid parameter: @Int cannot be NULL or zero
< X-AspNet-Version: 4.0.30319
< X-Powered-By: ASP.NET
< Date: Wed, 1 Aug 2020 17:43:04 GMT
< Content-Length: 4517

```

<mark style="color:orange;">**401:**</mark>** The saved query API endpoint will return a 401 error code when you provide invalid credentials, this includes not providing credentials, expired credentials and incorrect credentials.**

```
< HTTP/1.1 401 Unauthorized
< Cache-Control: private
< Content-Type: text/html; charset=utf-8
< Server: Microsoft-IIS/10.0
< X-AspNetMvc-Version: 5.2
< X-AspNet-Version: 4.0.30319

```

### 2.8 [_**/identity/connect/token**_](api-authentication.md)

**POST: https://\<Cinchy SSO URL>/identity/connect/token**

| Definition                                                                            |
| ------------------------------------------------------------------------------------- |
| The Post Request will return an access token which can be used to access Cinchy APIs. |

**Header Parameters**

| Name         | Data Type | Description                       |
| ------------ | --------- | --------------------------------- |
| Content-type | string    | application/x-www-form-urlencoded |

**Body Parameters**

| Name           | Data Type | Description                                                                                           |
| -------------- | --------- | ----------------------------------------------------------------------------------------------------- |
| Token          | string    | You can pass in your base64 encoded SAML token instead of your Cinchy username and password           |
| Client\_id     | string    | Client Id value from Integrated Clients table                                                         |
| Client\_secret | string    | Guid value from Integrated Clients table                                                              |
| Username       | string    | Username of Cinchy user                                                                               |
| Password       | string    | Password for Cinchy user in plain text                                                                |
| Grant\_Type    | string    | Set as "password" for username/password authentication. Set as "saml2" for saml token authentication. |
| Scope          | string    | Set as "js\_api"                                                                                      |

**Responses**

<mark style="color:green;">**200:**</mark>** The request is successful**

```
// {
    "access_token": "eyUzI1NiIsImtpZCI6IkE4M0UwQTFEQTY1MzE0NkZENUQxOTFDMzRDNTQ0RDJDODYyMzMzMzkiLCJ0eXAiOiJKV1QiLCJ4NXQiOiJxRDRLSGFaVEZHX1YwWkhEVEZSTkxJWWpNemsifQ.eyJuYmYiOjE1NTQxMzE4MjAsImV4cCI6MTU1NDEzNTQyMCwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgxIiwiYXVkIjpbImh0dHA6Ly9sb2NhbGhvc3Q6ODA4MS9yZXNvdXJjZXMiLCJqc19hcGkiXSwiY2xpZW50X2lkIjoiYWJjIiwic3ViIjoiMSIsImF1dGhfdGltZSI6MTU1NDEzMTgyMCwiaWRwIjoibG9jYWwiLCJwcm9maWxlIjoiQWRtaW5pc3RyYXRvciIsImVtYWlsIjoiYWRtaW5AY2luY2h5LmNvIiwicm9sZSI6IkNpbmNoeSBVc2VyIEFjY291bnQiLCJpZCI6ImFkbWluIiwic2NvcGUiOlsianNfYXBpIl0sImFtciI6WyJjdXN0b20iXX0.N7drAlvtFiQoN4njs1rd5ZnTvJ_x8ZEnUEi6G1GjR4FS5FyS4hC6xdsT-Zhn1yRJQMkI2HA7HMPWwjsfkZ0IlBwuC25ECkGhbjv7DlK6baHQIkqeB0aTB9aDZSxWfDhV66O0dhby6EIEa4YuGspyjQMsDpx_LimmE9alfsUU-608944ZZkS6lBJlJ9LFCC5hYKARQIMZavrftz0tFUBsDU0T2fHpLNGo5GGwG1f9jUZTWTu7s3C05EsgboW3scUfDzjS_Wf55ExwhopIg9SD6ktHYYNRaCPtfMhU-e43l6a2LH-XrmP7OfoxJP2bvTMcvQCQWUEizKHuxKLl-ehWBw",
    "expires_in": 3600,
    "token_type": "Bearer"
}
```

<mark style="color:orange;">**400:**</mark>** For invalid parameters, a 400 error will be returned with the following JSON response with a description of the error.**

```
Example:
{
  "error": "invalid_grant",
  "error_description": "Invalid username or password"
}
```

### 2.9 /api/getstsauthorityuri

| Description                             |
| --------------------------------------- |
| This will return your IDP URL endpoint. |

### **2.10 /**api/jobs

**POST: https://\<Connections-URL>/api/jobs**

| Description                                                                                    |
| ---------------------------------------------------------------------------------------------- |
| This endpoint can be used to trigger a batch data sync job that has been configured in Cinchy. |

**Request Header Parameters**

| Name          | Data Type | Description                                                                                                                                                                          |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Authorization | string    | <p>Bearer &#x3C;access_token><br><br>Note that this endpoint does not support Personal Access Tokens.<br><br>See <a href="api-authentication.md">Authentication</a> for details.</p> |
| Content-Type  | String    | multipart/form-data                                                                                                                                                                  |

**Body Parameters**

{% hint style="warning" %}
Note that the parameter names **"files"** and **"options"** are both case sensitive and must use lowercase in your JSON. The contents of the parameters themselves are not case sensitive.
{% endhint %}

| Name    | Data Type | Description                                                                                                          |
| ------- | --------- | -------------------------------------------------------------------------------------------------------------------- |
| files   | binary    | The URL to any file(s) needed to be uploaded to be used in place of a parameter in the data sync                     |
| options | string    | A JSON string of the data sync options. [See here](./#options-json-structure) for the structure of this JSON string. |

#### **Options - JSON Structure**

```json
 {
     "paramValues": {
       "<name of parameter in sync>": {
         "isFile": <set to true if this parameter corresponds to a file, otherwise set it to false>,
         "value": "<value of the parameter, if the parameter corresponds to a file, then it should match the file name that was uploaded>"
       }
     },
     "server": "<cinchy url without protocol>",
     "useHttps": <true or false, depending on the server's protocol>,
     "userId": "<username of the user you want to run the data sync as, leave as null if you want to run it as the user associated with the access token used in the Authorization header>",
     "password": "<password of the user you want to run the data sync as, leave as null if you want to run it as the user associated with the access token used in the Authorization header>",
     "model": "Cinchy",
     "feed": "<name of the data sync configuration>",
     "batchsize": <Set to 5000 for the default behaviour - the size of the batches when performing inserts/updates/deletes in the target, set to 5000 if you want the default behaviour>,
     "retrievalbatchsize": <Set to 5000 for the default behaviour -  this is the size of the batches when retrieving records from the source to process>,
     "writeToFile": <Set to true for the default behaviour - This will ensure it will write records to temp files on disk to save memory on larger syncs. If this is set to false, it'll store source records in memory, which will be faster for smaller data syncs>
   }
```

#### Endpoint Example

```
curl --request POST \
  --url <cinchy.com>/connections/api/jobs \
  --header 'Authorization: Bearer xxx’ \
  --header 'Content-Type: multipart/form-data' \
  --form files=@/Users/shawn/Worker.zip \
  --form 'options={ "paramValues": {  "file": {   "isFile": true,   "value": "FBL costing model with upload to SF.xlsx"  } }, "server": "sandbox.cinchy.net/technicalsales", "useHttps": true, "password": null, "userId": null, "model": "Cinchy", "feed": "Excel -> Cinchy", "batchsize": 5000, "retrievalbatchsize": 5000, "writeToFile": true}'
```

**Responses**

<mark style="color:green;">**200:**</mark>** The request was successful.**

```
{
   executionId = "<The Cinchy Id of the data sync execution, it will correspond go the Cinchy Id of the newly created record in the [Cinchy.[Execution Log] table>"
}
```

<mark style="color:orange;">**400:**</mark>** The request could not be completed.**

```
{
   status = <Http status code>,
   message = "<Error message>"
}
```
