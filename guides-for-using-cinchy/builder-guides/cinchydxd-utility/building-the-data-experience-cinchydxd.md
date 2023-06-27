---
description: 'This page outlines Step 1 of Deploying CinchyDXD: Building the Data Experience'
---

# Building the Data Experience (CinchyDXD)

## Table of Contents

| Table of Contents                                                                                 |
| ------------------------------------------------------------------------------------------------- |
| [#1.-table-creation](building-the-data-experience-cinchydxd.md#1.-table-creation "mention")       |
| [#2.-enter-sample-data](building-the-data-experience-cinchydxd.md#2.-enter-sample-data "mention") |
| [#3.-create-the-query](building-the-data-experience-cinchydxd.md#3.-create-the-query "mention")   |

{% hint style="danger" %}
**Remember that a**ll objects need to be created in one source environment (ex: DEV). From there, DXD will be used to push them into others (ex: SIT, UAT, Production).
{% endhint %}

## **1.** Table Creation

Create your data experience (DX) in a virtual data network.

1. Log in to Cinchy:\
   **URL: \<cinchy source URL>** \
   **User ID: \<source user id>**\
   **Password: \<source password>**
2. From the Homepage, select **Create**
3. Select **Table > From Scratch**
4. Create the table with the following properties _(Image 1)._

| **Table Details** | **Values**                                                                                                                                                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Table Name        | Currency Exchange Rate                                                                                                                                                                                                             |
| Icon + Colour     | Choose your own icon                                                                                                                                                                                                               |
| Domain            | <p>Sandbox (if the domain does not exist, create it)<br></p><p>To create a domain on the fly:</p><ol><li>Enter domain name in Domain field</li><li>Hit enter on keyboard</li><li>On the Confirm Domain window, click Yes</li></ol> |
| Description       | This table is a test table for building and deploying a data experience for currency conversion                                                                                                                                    |

![Image 1: Creating your Table](<../../../.gitbook/assets/image (336).png>)

6\. Click **Columns** in the left hand navigation to create the columns for the table

7\. Click the **“Click Here to Add”** a column tab to add a column

| Column Details | Values                                                                                                                                                                          |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Column 1       | <p>Column Name: Currency 1</p><p>Data Type: Text</p><p>Advanced Settings: </p><ul><li>Select Mandatory </li><li>Leave all other defaults </li></ul>                             |
| Column 2       | <p>Column Name: Currency 2</p><p>Data Type: Text</p><p>Advanced Settings:</p><ul><li>Select Mandatory </li><li>Leave all other defaults </li></ul>                              |
| Column 3       | <p>Column Name: Rate</p><p>Data Type: Number</p><p>Advanced Settings: </p><ul><li>Set Decimal Places to 4</li><li>Select Mandatory </li><li>Leave all other defaults </li></ul> |

8\. Click the Save button to save your table

## 2. Enter Sample Data

1. In your newly created table, enter the following sample data _(Image 2)._

| Currency 1 | Currency 2 | Rate |
| ---------- | ---------- | ---- |
| CAD        | USD        | 0.71 |
| USD        | CAD        | 1.40 |

![Image 2: Filling in your table](<../../../.gitbook/assets/image (92).png>)

## 3. Create the Query

Create a simple query that pulls information from the **Currency Exchange Rate table** that will allow a simple currency exchange calculation.

1. From the Homepage, select **Create.**
2. Select **Query.**

3\. In the query builder locate the Currency Exchange Rate table and drag it to the **“FROM”** line _(Image 3)._

{% hint style="info" %}
You will find the Currency Exchange Rate table in the “Sandbox” domain. To expand the “Sandbox” domain, click on the gray arrow (or double click)
{% endhint %}

![Image 3: Creating your query](<../../../.gitbook/assets/image (248).png>)

4\. In the **“SELECT”** line drag and drop the **“Rate”** column and enter in the following _(Image 4):_\
`SELECT [Rate] * @Amount AS 'Converted Amount'`

{% hint style="info" %}
You will find the Rate column by expanding the Currency Exchange Rate table, similarly to expanding the “Sandbox” domain
{% endhint %}

![Image 4: Creating your query, cont.](<../../../.gitbook/assets/image (758).png>)

5\. Enter in the following for the WHERE clause _(Image 5):_

`WHERE [Deleted] IS NULL` \
`AND [Currency 1] = @Currency_1` \
`AND [Currency 2] = @Currency_2`

![Image 5: Creating your query, cont.](<../../../.gitbook/assets/image (724).png>)

6\. Click the Execute (or play) icon to run the query _(Image 6):_

![Image 6: Creating your query, cont.](<../../../.gitbook/assets/image (521).png>)

7\. Test the query by entering in the following and clicking the submit button _(Image 7):_

`@Amount: 100`\
`@Currency_1: CAD`\
`@Currency_2: USD`

![Image 7: Testing your query](<../../../.gitbook/assets/image (457).png>)

8\. Save the Query by clicking on the Info tab (Left Navigation)\
9\. Enter in the following details for the query _(Image 8):_

| Query Details     | Values                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------ |
| Query Name        | Currency Converter                                                                               |
| Icon + Colour     | Choose your own icon                                                                             |
| Return            | Query Results (Approved Data Only)                                                               |
| Domain            | Sandbox                                                                                          |
| API Result Format | JSON                                                                                             |
| Description       | This query is a test query for building and deploying a data experience for currency conversion  |

![Image 8: Adding in info about your query](<../../../.gitbook/assets/image (236).png>)

10\. Click the Save button _(Image 9)._

![Image 9: Saving your query](<../../../.gitbook/assets/image (736).png>)
