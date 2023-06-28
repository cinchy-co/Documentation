---
description: This page contains various Integration Guides
---

# Integration Guides

## 1. Excel

There are various methods you can use to establish a connection between Cinchy and Microsoft Excel, such as using **Basic Auth, Personal Access Tokens, or Bearer Tokens.**

Review each section below for further details.&#x20;

### 1.1 Prerequisites

Excel connects to queries within Cinchy, therefore prior to using any of the connection methods below you will need to create one that represents your dataset. Once created, you will need to copy down the [REST API URL endpoint](../../api-guide/api-overview/api-saved-queries.md#1.-overview), located as a green button on the right-hand side of the Execute Query screen.

The structure of the URL endpoint is _\<your Cinchy instance URL>/API/\<the name of your query>._ You might optionally have querystring parameters at the end as well.

For example: `http://your.cinchy.instance.domain/API/YourQueryDomain/API Test`&#x20;

{% hint style="warning" %}
Note that for **Basic Authentication** with a result format of CSV we will use a slightly different URL endpoint.\
\
**For Basic Auth:** **/API/** becomes **/BasicAuthAPI/** \
\
**For CSV results** you will add the querystring parameter of **ResultFormat=CSV**

Our example URL for an basic auth using CSV results would then become: `http://your.cinchy.instance.domain/BasicAuthAPI/YourQueryDomain/API Test?ResultFormat=CSV`
{% endhint %}

### 1.2 Using Basic Auth

1. Launch Excel and navigate to **Data > Get Data > From Other Sources > Blank Query** _(Image 1)._

<figure><img src="../../.gitbook/assets/image (253).png" alt=""><figcaption><p>Image 1: Blank Query</p></figcaption></figure>

2. In the expression box that appears, enter the below text to add in your query as your data source _(Image 2)_:

`=Csv.Document(Web.Contents("API ENDPOINT URL"))`

Example:

`=Csv.Document(Web.Contents("http://your.cinchy.instance.domain/BasicAuthAPI/YourQueryDomain/API Test?ResultFormat=CSV"))`

<figure><img src="../../.gitbook/assets/image (490).png" alt=""><figcaption><p>Image 2: Add the query as your source</p></figcaption></figure>

3. Once you've entered that text either **click the check mark to the left of the input box** or **click away** and it will automatically attempt to run the expression.&#x20;
4. The data may return in HTML format initially and not be what you're expecting. To correct this:&#x20;
   1. Click the **Data Source Settings.**
   2. Select **Basic** and enter the credentials for a Cinchy User Account that has access to run this query.&#x20;
   3. Click **OK.**&#x20;
   4. Within the Edit Permissions dialogue, click **OK**.
   5. Within the **Data Source Settings dialogue**, click **Close.**
   6. Click **Refresh Preview.**
   7. Click **Close & Load** and your dataset will be displayed in the Excel worksheet.

### &#x20;1.3 Using a Personal Access Token

1. If needed, [follow the documentation here](../user-guides/user-preferences/personal-access-tokens.md) to **generate a new Personal Access Token (PAT).**
2. Launch Excel and navigate to **Data > From Web.**
3. Click **Advanced** and input the following values _(Image 3)_:
   1. **URL Parts:** This is the Query API URL that you created in the [Prerequisites](integration-guides.md#1.1-prerequisites) section.
   2. **HTTP Request Header Parameters:**
      1. In the first text box input **Authorization**
      2. In the second text box type **Bearer + your PAT.** For example: **"Bearer BGFHFHOJDF76DFDFD777"**

<figure><img src="https://lh3.googleusercontent.com/pFPcw1mUPWZpZv1yz4Cd42qmkSXI7GLlHYU8jE-953oefqyxwU6XG3v9A38L0DyEuKyNRV3aiHxZskRK59gyuzyOn2q3s3WjuGM_IUGa-x3jHDBMQYa8Foke-hfbCKszqkCWYd5C45JXFg8oYfVXnIc" alt=""><figcaption><p>Image 3: Advanced Settings</p></figcaption></figure>

4. Click **OK.**
5. Click **Load** to use the query data in Excel _(Image 4)._

<figure><img src="https://lh3.googleusercontent.com/0R_lgMZEozKXiBbjVNEN1-QKni2LkqMTBii_ejdMtWiNPouY7GFY7ThCPeLl_QIZIYFQU2y54HNyfsICc18XIMZCoEnd0ElyEQvMvvk2gSseGPtgJ9ptX1RHpWFtMfkCbbBzt_7cz8eALzfy9aRQPDA" alt=""><figcaption><p>Image 4: Load</p></figcaption></figure>

### 1.4 Using a Bearer Token

1. If needed, [follow the documentation here](../user-guides/user-preferences/personal-access-tokens.md)[ ](https://platform.docs.cinchy.com/api-guide/api-overview/api-authentication)to **generate a Bearer Token.**
2. Launch Excel and navigate to **Data > From Web.**
3. Click **Advanced** and input the following values _(Image 5)_:
   1. **URL Parts:** This is the Query API URL that you created in the [Prerequisites](integration-guides.md#1.1-prerequisites) section.
   2. **HTTP Request Header Parameters:**
      1. In the first text box input **Authorization**
      2. In the second text box type **Bearer + your token.** For example: **"Bearer eyUzI1NiIsImtpZCI6IkE4M0UwQTFEQTY1MzE0NkZENUQxOTFDMzRDNTQ0RDJDODYyMzMzMzkiLCJ0eXAiO"**

<figure><img src="https://lh3.googleusercontent.com/pFPcw1mUPWZpZv1yz4Cd42qmkSXI7GLlHYU8jE-953oefqyxwU6XG3v9A38L0DyEuKyNRV3aiHxZskRK59gyuzyOn2q3s3WjuGM_IUGa-x3jHDBMQYa8Foke-hfbCKszqkCWYd5C45JXFg8oYfVXnIc" alt=""><figcaption><p>Image 5: Advanced Settings</p></figcaption></figure>

4. Click **OK.**
5. Click **Load** to use the query data in Excel _(Image 6)._

<figure><img src="https://lh3.googleusercontent.com/0R_lgMZEozKXiBbjVNEN1-QKni2LkqMTBii_ejdMtWiNPouY7GFY7ThCPeLl_QIZIYFQU2y54HNyfsICc18XIMZCoEnd0ElyEQvMvvk2gSseGPtgJ9ptX1RHpWFtMfkCbbBzt_7cz8eALzfy9aRQPDA" alt=""><figcaption><p>Image 6: Load</p></figcaption></figure>

## 2. Power BI

There are various methods you can use to establish a connection between Cinchy and Power BI, such as using **Basic Auth, Personal Access Tokens, or Bearer Tokens.**

Review each section below for further details.&#x20;

### 2.1 Prerequisites

Power BI connects to queries within Cinchy, therefore prior to using any of the connection methods below you will need to create one that represents your dataset. Once created, you will need to copy down the [REST API URL endpoint](../../api-guide/api-overview/api-saved-queries.md#1.-overview), located as a green button on the right-hand side of the Execute Query screen.

The structure of the URL endpoint is _\<your Cinchy instance URL>/API/\<the name of your query>._ You might optionally have querystring parameters at the end as well.

For example: `http://your.cinchy.instance.domain/API/YourQueryDomain/API Test`&#x20;

{% hint style="warning" %}
Note that for **Basic Authentication** with a result format of CSV we will use a slightly different URL endpoint.\
\
**For Basic Auth:** **/API/** becomes **/BasicAuthAPI/** \
\
**For CSV results** you will add the querystring parameter of **ResultFormat=CSV**

Our example URL for an basic auth using CSV results would then become: `http://your.cinchy.instance.domain/BasicAuthAPI/YourQueryDomain/API Test?ResultFormat=CSV`
{% endhint %}

### 2.2 Using Basic Auth

1. Launch Power BI and navigate **Get Data > Web** _(Image 7)_\


<figure><img src="../../.gitbook/assets/image (577).png" alt=""><figcaption><p>Image 7: Get Data > Web</p></figcaption></figure>

6\. In the window that launches, you will enter the below text, using your own URL endpoint where highlighted _(Image 8):_\
`=Csv.Document(Web.Contents(`<mark style="color:yellow;background-color:yellow;">`"http://your.cinchy.instance.domain/BasicAuthAPI/YourQueryDomain/API Test?ResultFormat=CSV"`</mark>`))`

![Image 8: Enter your expression](<../../.gitbook/assets/image (637).png>)

7\. Click on the checkmark icon and Power BI will automatically attempt to run the expression _(Image 9)._

![Image 9](<../../.gitbook/assets/image (177).png>)

8\. Click **Edit Credentials > Basic** _(Image 10)._ Enter the credentials for a Cinchy User Account that has access to run this query and select the level at which to apply these settings. By default it's the root URL.

This process of entering your credentials won't occur with each query, it's just the first time and then they're saved locally.

![Image 10](<../../.gitbook/assets/image (564).png>)

10\. Click **Connect** to see your data _(Image 11)._

![Image 11](<../../.gitbook/assets/image (126).png>)

11\. You can now apply any transformations to the dataset.

In this example we also changed the name from Query1 to Product Roadmap and have edited to use the first row as a header _(Image 12)._

![Image 12](<../../.gitbook/assets/image (236).png>)

12\. Click **Close & Apply.** The metadata now shows up on the right hand side and you can begin to use it to create your visualizations _(Image 13)._

![Image 13](<../../.gitbook/assets/image (686).png>)

### 2.3 Using a Personal Access Token

1. If needed, [follow the documentation here](../user-guides/user-preferences/personal-access-tokens.md) to **generate a new Personal Access Token (PAT).**
2. Launch Power BI and navigate to **Get Data > Web.**
3. Click **Advanced** and input the following values _(Image 14)_:
   1. **URL Parts:** This is the Query API URL that you created in the [Prerequisites](integration-guides.md#2.1-prerequisites) section.
   2. **HTTP Request Header Parameters:**
      1. In the first text box input **Authorization**
      2. In the second text box type **Bearer + your PAT.** For example: **"Bearer BGFHFHOJDF76DFDFD777"**

<figure><img src="../../.gitbook/assets/image (190).png" alt=""><figcaption><p>Image 14</p></figcaption></figure>

4. Click **OK.**
5. Click **Load** to use the query data in Power BI_._
6. You can now apply any transformations to the dataset.

In this example we also changed the name from Query1 to Product Roadmap and have edited to use the first row as a header _(Image 15)._

![Image 15](<../../.gitbook/assets/image (236).png>)

7. Click **Close & Apply.** The metadata now shows up on the right hand side and you can begin to use it to create your visualizations _(Image 16)._

![Image 13](<../../.gitbook/assets/image (686).png>)

### 2.4 Using a Bearer Token

1. If needed, [follow the documentation here](../user-guides/user-preferences/personal-access-tokens.md)[ ](https://platform.docs.cinchy.com/api-guide/api-overview/api-authentication)to **generate a Bearer Token.**
2. Launch Power BI and navigate to **Get Data > Web.**
3. Click **Advanced** and input the following values _(Image 17)_:
   1. **URL Parts:** This is the Query API URL that you created in the [Prerequisites](integration-guides.md#2.1-prerequisites) section.
   2. **HTTP Request Header Parameters:**
      1. In the first text box input **Authorization**
      2. In the second text box type **Bearer + your token.** For example: **"Bearer eyUzI1NiIsImtpZCI6IkE4M0UwQTFEQTY1MzE0NkZENUQxOTFDMzRDNTQ0RDJDODYyMzMzMzkiLCJ0eXAiO"**

<figure><img src="https://lh3.googleusercontent.com/pFPcw1mUPWZpZv1yz4Cd42qmkSXI7GLlHYU8jE-953oefqyxwU6XG3v9A38L0DyEuKyNRV3aiHxZskRK59gyuzyOn2q3s3WjuGM_IUGa-x3jHDBMQYa8Foke-hfbCKszqkCWYd5C45JXFg8oYfVXnIc" alt=""><figcaption><p>Image 17</p></figcaption></figure>

4. Click **OK.**
5. Click **Load** to use the query data in Power BI.

## 3. Tableau

Cinchy exposes a Tableau Web Data Connector that provides access to Cinchy Saved Queries as data sources in Tableau. Tableau versions 2019.2+ are supported.

{% hint style="info" %}
&#x20;An active internet connection is required in order to use the Web Data Connector.
{% endhint %}

### 3.1 Prerequisites

To get started, you must add a record into the `Integrated Clients` table in the `Cinchy` domain with the below values.

| Column                          | Value                                                                                       |
| ------------------------------- | ------------------------------------------------------------------------------------------- |
| Client Id                       | tableau-connector                                                                           |
| Client Name                     | Tableau                                                                                     |
| Grant Type                      | Implicit                                                                                    |
| Permitted Login Redirect URLs   | [https://\<your Cinchy URL>/Tableau/Connector](http://qa1-app1.cinchy.co/Tableau/Connector) |
| Permitted Logout Redirect URLs  | [https://\<your Cinchy URL>/Tableau/Connector](http://qa1-app1.cinchy.co/Tableau/Connector) |
| Permitted Scopes                | Id, OpenId, Email, Profile, Roles                                                           |
| Access Token Lifetime (seconds) | 3600                                                                                        |
| Show Cinchy Login Screen        | Checked                                                                                     |
| Enabled                         | Checked                                                                                     |

### 3.2 Connecting From Tableau

1. Launch Tableau
2. Under **`Connect` -> `To a Server`** select the **`Web Data Connector`** option
3. Enter the URL from the **`Permitted Login Redirect URLs`** field on the **`Integrated Clients`** record created under the **Prerequisites** section above
4. The Cinchy login screen will appear, enter your credentials
5. Select one or more queries to add to your data set. The result of each query will be available as a Table in Tableau. If a query has parameters, you will be prompted to provide the parameter values before you can add it to your collection.
6. Click the **Load** button

The Cinchy query results will now be accessible for you to create your visualization.
