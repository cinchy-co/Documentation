---
description: >-
  This page will help you to understand the fundamentals of getting started with
  Cinchy Query Language.
---

# The Basics of CQL

### 1. **Introduction** <a href="#1.-introduction" id="1.-introduction"></a>

**Cinchy Query Language (CQL)** is a query language unique to dataware that is used to retrieve and manage/modify data and metadata from tables in your network. While data can reside across many tables, a query can isolate it to a single output, making the possibilities of CQL endlessly powerful.

**Cinchy Query Language can be used in many ways, including but not limited to:**

* ​Building queries through the query editor that can return, insert, delete, and otherwise manage your data.
* ​Creating, altering or dropping views for tables​
* ​Creating, altering or dropping indexes​

For an overview of the supported functions of CQL, [see here.​](cql-functions-master-list.md)

{% hint style="success" %}
All queries on Cinchy are automatically protected by universal data access controls. This means that if you run a CQL query, you will only see the data that you have been given access to.
{% endhint %}

### 2. Basic Rules of CQL <a href="#2.-basic-rules-of-cql" id="2.-basic-rules-of-cql"></a>

The following is a non-exhaustive list of some of the common things to keep in mind while using CQL. While CQL pulls many similarities to other queries languages such as SQL or PGSQL that you may already be familiar with, there are still differences to make note of.

* Cinchy comes with an intuitive Query Builder UI. When using the builder, the below basic syntax is pre-written for you. You can then use the drag and drop interface or type in your own search terms to build your query.​![](https://762429502-files.gitbook.io/\~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MBtHkNqYteSDPDzpqqZ%2Fuploads%2FXPwQAuVPRZiEd8cM1N6p%2Fimage.png?alt=media\&token=90cfce6d-cc3f-46dd-98a1-8392b7d88938)​

{% hint style="info" %}
[See here for more information on using the Query Builder.](https://cinchy.gitbook.io/cinchy-v5.0.0/guides-for-using-cinchy/builder-guides/saved-queries)
{% endhint %}

* In all queries built using the Cinchy Query Builder, you will note that it includes a **"WHERE \[Deleted] IS NULL"** clause. This will prevent any data that has been deleted from a table from ending up in your query. If you want to include deleted data, you must delete this clause.
* When querying a table, you must use the **\[Domain].\[Table]** syntax. For example, to query the _Product Content Backlog_ table, you would use **\[Product].\[Product Content Backlog].**
* When querying a linked column, you must similarly use the **\[Column Name].\[Linked Column Name]** syntax.
  * For example, in the below query we are pulling from the **\[Product].\[Product Content Backlog]** table, and are looking for data from a linked column called **"Requester",** which points to the **Users** table. In order to return the **\[Full Name]** column from the linked **Users** table, we must use the **\[Requester].\[Full Name]** syntax.

```sql
SELECT [Requester].[Full Name]
FROM [Product].[Product Content Backlog]
WHERE [Deleted] IS NULL
```

* Multi-level links are also possible using a similar syntax of **\[Column Name].\[Linked Column Name].\[Linked Column Name]**. For example, if you wanted to see the _Manager ID_ of a specific employee, you could use **\[Employee].\[Reports To].\[Employee ID],** which will find the Employee in question, then who they report to, then the ID number of that person.

```sql
SELECT [Employee].[Reports To].[Employee ID]
FROM [Employee Success].[Employees]
WHERE [Deleted] IS NULL
```

* When writing queries, single notation marks are used to denote string/text data. For example, if you wanted to specify the Sandbox domain, you would use **\[Domain] = 'Sandbox'**
* When trying to query using a **"does not equal"** syntax, you would use **!=**. For example, the following denotes to only return results where the **\[Domain] does not equal 'Sandbox'**

```sql
[Domain] != 'Sandbox'
```

* If you want to query data from a table where certain rows are still in draft/Create Request format, you would use the syntax of **Draft(\[Column Name])** to see the draft changes. Make sure to also include **\[Column Name]** as well to include non-draft rows.
* The default **ORDER BY** function will always set **ascending** unless specified.
* When using a boolean query, **1** = true, and **0** = false.
* Version history in Cinchy is labeled differently in other SQL systems. Normally a version history is shown through, for example, "version 1.2.4". Cinchy follows these conventions but splits them up. The two **ORDER BY** options, **Version** and **Draft Data**. are the version numbers. Version is the first number, and Draft Data is the second number in the sequence Example: \[Version]: 2 and \[Draft Data]: 5 means the overall version of the policy is 2.5
* If there is an error in your CQL code an error message will appear in your Query Results in the query builder indicating what column and row your error(s) reside in.
* When using an **INSERT INTO** clause, the order of the columns input must match the same order as in the **VALUES** section.
* Putting a **\*** symbol after a **SELECT** statement in the Query Builder will return a series of system columns attached to each table entry.

```sql
SELECT *
```

* Use **RESOLVELINK** when inserting or updating values for link columns. In the below example, we use **(RESOLVELINK(@Manager,'Full Name'))** because the Manager column is a linked column pulling from the Employee table. The syntax for using Resolve Link is: _ResolveLink('value(s)','column in target table')_

```sql
INSERT INTO [Human Resources].[Employee RY] ([First Name], [Last Name], [Employee ID], [Manager])
VALUES (@FirstName, @LastName, @EmployeeID, (RESOLVELINK(@Manager, ‘Full Name’)) 
```

### 3. Query Return Results <a href="#3.-query-return-results" id="3.-query-return-results"></a>

You can specify what your results return as in the Query Builder

| Query Return Results                                                                    | Description                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Query Result (Approved Data Only) (\*This is the default when creating a new query)** | This is the default return type, it returns a table from a select query with only approved data for Maker/Checker-enabled tables, or all data for tables without Maker/Checker-enabled. This is generally used for external APIs as you will want to query approved data, rather than drafts. |
| **Query Result (Including Draft Data)**                                                 | This return type returns a table from a select query with only draft data for Maker/Checker-enabled tables. Use this return type when looking to display results of records that are pending approval**.**                                                                                    |
| **Query Result (Including Version History)**                                            | This return type returns a table from a select query with historical data for all tables, as seen in the Collaboration Log of any record. This data includes all changes that happened to all records within the scope of the select query.                                                   |
| **Number of Rows Affected**                                                             | This return type returns a single string response with the number of rows affected if the last statement in the query is an INSERT, UPDATE, or DELETE statement.                                                                                                                              |
| **Execute DDL Script**                                                                  | Use this return type when your query contains DDL commands that implement schema changes such as CREATE\|ALTER\|DROP TABLE, CREATE\|ALTER\|DROP VIEW, or CREATE\|DROP INDEX.                                                                                                                  |
| **Single Value (First Column of First Row)**                                            | This return type returns a result of 1 row x 1 column, irrespective of the underlying result set.                                                                                                                                                                                             |

### 3. Data Return Types <a href="#3.-data-return-types" id="3.-data-return-types"></a>

The following tables provide the data type(s) that a **Cinchy Data Type** translates to in the database:

| Number     | Text     | Date     |
| ---------- | -------- | -------- |
| Int        | NVarChar | DateTime |
| BigInt     | VarChar  | Date     |
| Decimal    | Char     | ​        |
| Float      | NChar    | ​        |
| Money      | NText    | ​        |
| Numeric    | Text     | ​        |
| Real       | ​        | ​        |
| SmallInt   | ​        | ​        |
| SmallMoney | ​        | ​        |
| TinyInt    | ​        | ​        |

| Binary    | Text |
| --------- | ---- |
| VarBinary | Bit  |
