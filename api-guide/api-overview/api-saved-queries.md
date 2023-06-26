---
description: This page outlines how to use saved queries with APIs.
---

# API Saved Queries

## Table of Contents

| Table of Contents                                                          |
| -------------------------------------------------------------------------- |
| [#1.-overview](api-saved-queries.md#1.-overview "mention")                 |
| [#2.-example](api-saved-queries.md#2.-example "mention")                   |
| [#3.-anonymous-access](api-saved-queries.md#3.-anonymous-access "mention") |

## 1. Overview

Cinchy queries are automatically available as REST APIs to allow for external application integration.

See [Saved Queries](../../guides-for-using-cinchy/builder-guides/saved-queries.md) on how to create a saved query. The URL for the REST API can be found on the Execute Query screen (Image 1).

![Image 1: Getting the REST API for a Saved Query](<../../.gitbook/assets/API Saved Queries - Image 1.png>)



{% swagger baseUrl="https://<Cinchy Web URL>/API/MyDomain/MyQuery" path=" " method="get" summary="How to Use the Access Token to Call Cinchy APIs" %}
{% swagger-description %}
To access any Cinchy Saved Query API, pass the access token received from the Bearer Token Request in the Authorization header, prefixed by "Bearer".
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer <token goes here>
{% endswagger-parameter %}

{% swagger-parameter in="query" name="WrapSingleRecordInArray" type="boolean" %}
Default is true.

\


Add this parameter and set to false if you want single record results returned as an object instead of within an array.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="@param" type="string" %}
If you have parameters in your query, you pass them indirectly as query parameters.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="CompressJSON" type="boolean" %}
Default is true.

\


Add this parameter and set to false if you want the JSON that is returned to be expanded rather than having the schema being returned separately.
{% endswagger-parameter %}

{% swagger-response status="200" description="The request has successfully returned the record set." %}
```bash
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
{% endswagger-response %}

{% swagger-response status="400" description="The request could not be understood, the client is sending a request with incomplete data, poorly constructed data or invalid data.

Optional Validation Logic:
To validate query business / control conditional logic failure can be added at the beginning of the API which can intentionally generate a 400 error code (using RAISERROR) and stopping (using RETURN) the API.  If there is no RETURN the errors will accumulate and will be provided at the end of running the API.

An attribute (X-Cinchy Error) will be returned in the HTTP header with the custom RAISERROR message indicated (see example below).

EXAMPLE:" %}
```bash
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
{% endswagger-response %}

{% swagger-response status="401" description="The saved query API endpoint will return a 401 error code when you provide invalid credentials, this includes not providing credentials, expired credentials and incorrect credentials." %}
```bash
< HTTP/1.1 401 Unauthorized
< Cache-Control: private
< Content-Type: text/html; charset=utf-8
< Server: Microsoft-IIS/10.0
< X-AspNetMvc-Version: 5.2
< X-AspNet-Version: 4.0.30319
```
{% endswagger-response %}
{% endswagger %}

## 2. Example

The following example shows how to use API Saved Queries _(Image 2.)_

![Image 2: Using API Saved Queries](<../../.gitbook/assets/image (27).png>)

{% hint style="info" %}
In the above image, **"%40"** is the URL encoded version of **"@". I**f you are passing them in as parameters you will need to include the %40 in front of your parameter name.
{% endhint %}

### Use Basic Authentication to Call Cinchy APIs <a href="#use-access-token-to-call-cinchy-apis" id="use-access-token-to-call-cinchy-apis"></a>

`https://<Cinchy Web URL>/BasicAuthAPI/MyDomain/MyQuery`

## 3. Anonymous Access

The following instructions detail how to allow anonymous access to your saved query API endpoint.

1. Navigate to the table your query will be referencing. In this example, it is the **Accessibility Assessments** table _(Image 1)_.
2. Navigate to **Data Controls > Entitlements.**
3. On a new row, add in the **Anonymous user** and ensure that either **"View All Comuns"** (to expose all the data) or **"View Selected Columns"** (to select individual columns) is checked off _(Image 3)._

{% hint style="info" %}
Clicking on inline images in Gitbook will open a larger version.
{% endhint %}

![Image 3: The Entitlements Table](<../../.gitbook/assets/image (420).png>)

4\. Design your query _(Image 4)_. For more information on creating new saved queries, [click here](../../guides-for-using-cinchy/builder-guides/saved-queries.md#2.-creating-a-saved-query).

![Image 4: Design your Query](<../../.gitbook/assets/image (11).png>)

5\. Once you have written your query, navigate to **Design Query > Info**, on the left navigation bar.

6\. Change your **API Result Format** to **JSON** _(Image 5)_.

![Image 5: Change your API Result Format to JSON](<../../.gitbook/assets/image (215).png>)

7\. Navigate to **Design Controls** from the left navigation bar.

8\. To ensure that anonymous users have the correct permission needed to execute the query that generates the API response, add the **"Anonymous"** user to the users permission group uiunder **"Who can execute this query?"** _(Image 6)._

![Image 6: Ensuring the anonymous user has access](<../../.gitbook/assets/image (6).png>)

9\. Navigate to **"Execute Query"** from the left navigation bar.

10\. Copy your **REST API endpoint URL** _(Image 7)._

![Image 7: Copy your REST API endpoint URL](<../../.gitbook/assets/image (526).png>)

11\. To confirm that anonymous access has been successfully set up, paste the URL into an **incognito/private browser** _(Image 8)._

![Image 8: Testing your API](<../../.gitbook/assets/image (491).png>)

### 3.1 Troubleshooting

* **`401`** errors likely mean you have not added the **`Anonymous`** user to the list of **users** (not "Groups") that can **execute the query**.
* **`400`** errors likely mean that you have not added the **`Anonymous`** user to the list of **users** that (not "Groups") that can **view column data** from the **tables that your query uses**.
