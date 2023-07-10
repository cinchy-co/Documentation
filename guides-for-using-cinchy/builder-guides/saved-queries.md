---
description: This page explores Saved Queries
---

# Saved Queries

## Access a Saved Query

Saved queries allows you to query any data within Cinchy (respecting entitlements) and save them to be used as APIs by external systems.

You can access your Saved Query directly by either CinchyID or the domain and name of the Saved Query.

`<baseurl>/Query/Execute?queryId=<cinchyid>`

`<baseurl>/Query/Execute/<domain>/<saved query name>`

You can find this information in the **Saved Queries table.**

You can also search your Cinchy Homepage to find your Saved Query.

## Create a Saved Query

1. From the homepage, select **Create > Query** _(Image 1)_

![Image 1: Step 1, Click Create](<../../.gitbook/assets/image (202).png>)

2. Fill out the following information:

### The Info Tab

Under the Info tab, you can fill out information on the query if you wish to save it (Image 2):

![Image 2: Step 2, The Info Tab](<../../.gitbook/assets/image (229).png>)

- Query Name: ‌Mandatory field. Must be unique within the Domain.‌
- Icon: ‌You can optionally pick a non-default icon, as well as color for your table. This will be displayed in My Network.
- Domain: ‌You need to select a Domain your query will reside in. As an admin, you can also create new domains in this screen.
- Description: ‌You can give your query a description. This description will be displayed on the home screen to users browsing the marketplace. It will also be searchable.
- Return Type: Queries have six different return types:

  - **Query Results (Approved Data Only)**

    This is the default return type, it returns a table from a select query with only approved data for Maker/Checker-enabled tables, or all data for tables without Maker/Checker-enabled. This is generally used for external APIs as you will want to query approved data, rather than drafts.

  - **Query Results (Including Draft Data)**

    This return type returns a table from a select query with only draft data for Maker/Checker-enabled tables. Use this return type when looking to display results of records that are pending approval.

  - **Query Results (Including Version History)**

    This return type returns a table from a select query with historical data for all tables, as seen in the Collaboration Log of any record. This data includes all changes that happened to all records within the scope of the select query.

  - **Number of Rows Affected**

    This return type returns a single string response with the number of rows affected if the last statement in the query is an INSERT, UPDATE, or DELETE statement.

  - **Execute DDL Script**

    Use this return type when your query contains DDL commands that implement schema changes such as CREATE|ALTER|DROP TABLE, CREATE|ALTER|DROP VIEW, or CREATE|DROP INDEX.

  - **Single Value (First Column of First Row)**

    This return type returns a result of 1 row x 1 column, irrespective of the underlying result set.

### The Query Tab

In the Query screen, you can modify and run your query _(Image 3)._

![Image 3](<../../.gitbook/assets/image (567).png>)

On the left hand side you have the Object tree, which shows you all the domains, tables, and columns you have access to query within Cinchy. You can search or simply navigate by expanding the domains and tables.

You can drag and drop the columns or table you're looking for into the Query Builder.

Once you are satisfied with your query, you can click save to keep a copy. You can then find your query in the "Saved Queries" table _(Image 4):_

![Image 4: You can find saved queries in the Saved Queries table.](<../../.gitbook/assets/image (561).png>)

## Display a Saved Query

1. Once you've set up your saved query, you can find it on your homepage _(Image 5)._

![Image 5](<../../.gitbook/assets/image (387).png>)

2\. Clicking the query will allow you to **"Execute Query"** and show you the result set (if there is a SELECT at the end). Sometimes the query will have parameters you need to fill out first before executing _(Image 6)._

![Image 6](<../../.gitbook/assets/image (119).png>)

### Pivot reports

1. Once you execute a query, you can switch the Display to **Pivot Mode** to see different visualizations of the data _(Image 7)._

![Image 7](<../../.gitbook/assets/image (712).png>)

### Pivot URLs

If you want to share the report, you can click the Pivot URL button on the top right to copy the URL to that pivoted report. Simply [add it as an applet ](../additional-guides/application-experiences/setting-up-experiences.md)and bookmark it to return to the pivoted view!

## Additional information

For more information and documentation on Cinchy Query Language (CQL), please see [the CQL basics page ](../../cql/the-basics-of-cql/)
