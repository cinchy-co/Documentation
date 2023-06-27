---
description: >-
  This page will help you to understand the fundamentals of getting started with
  Cinchy Query Language.
---

# The Basics of Cinchy Query Language (CQL)

### Introduction <a href="#1.-introduction" id="1.-introduction"></a>

**Cinchy Query Language (CQL)** is a query language unique to dataware that's used to retrieve and manage/modify data and metadata from tables in your network. While data can reside across many tables, a query can isolate it to a single output, making the possibilities of CQL endlessly powerful.

For example, you can use CQL in the following ways:

- ​Build queries through the query editor that can return, insert, delete, and otherwise manage your data.
- ​Create, alter, or drop views for tables​.
- ​Create, alter, or drop indexes​.

For an overview of the supported functions of CQL, [see the CQL functions master list.​](cql-functions-master-list.md)

{% hint style="success" %}
All queries in Cinchy are automatically protected by universal data access controls. This means that if you run a CQL query, you only see the data that you have access to.
{% endhint %}

### Basic rules <a href="#2.-basic-rules-of-cql" id="2.-basic-rules-of-cql"></a>

The following is a non-exhaustive list of some things to keep in mind while using CQL. While CQL pulls many similarities to other queries languages (like SQL or PGSQL) that you might be familiar with, there are still key differences.

- Cinchy comes with an intuitive Query Builder UI. When you use the builder, the basic syntax is pre-written for you, as seen in the image below. You can then use the drag-and-drop interface, or type in your own search terms to build your query.​![](https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MBtHkNqYteSDPDzpqqZ%2Fuploads%2FXPwQAuVPRZiEd8cM1N6p%2Fimage.png?alt=media&token=90cfce6d-cc3f-46dd-98a1-8392b7d88938)​

{% hint style="info" %}
[See the Saved Queries page for more information about using the Query Builder.](https://cinchy.gitbook.io/guides-for-using-cinchy/builder-guides/saved-queries)
{% endhint %}

- All queries built using the Cinchy Query Builder include a **"WHERE \[Deleted] IS NULL"** clause. This prevents any deleted table data from ending up in your query. If you want to include deleted data, you must delete this clause.
- When querying a table, you must use the **\[Domain].\[Table]** syntax. For example, to query the _Product Content Backlog_ table, you would use **\[Product].\[Product Content Backlog].**
- When querying a linked column, you must similarly use the **\[Column Name].\[Linked Column Name]** syntax.
  - For example, the query below pulls from the **\[Product].\[Product Content Backlog]** table, and searches for data from a linked column called **"Requester",** which points to the **Users** table. To return the **\[Full Name]** column from the linked **Users** table, use the **\[Requester].\[Full Name]** syntax.

```sql
SELECT [Requester].[Full Name]
FROM [Product].[Product Content Backlog]
WHERE [Deleted] IS NULL
```

- Multi-level links are also possible using a similar syntax of **\[Column Name].\[Linked Column Name].\[Linked Column Name]**. For example, if you wanted to see the _Manager ID_ of a specific employee, you could use **\[Employee].\[Reports To].\[Employee ID],** which will find the Employee in question, then who they report to, then the ID number of that person.

```sql
SELECT [Employee].[Reports To].[Employee ID]
FROM [Employee Success].[Employees]
WHERE [Deleted] IS NULL
```

- When writing queries, use single notation marks to denote string/text data. For example, if you want to specify the Sandbox domain, you would use **\[Domain] = 'Sandbox'**
- When trying to query using a **"does not equal"** syntax, use **!=**. For example, the following denotes to only return results where the **\[Domain] does not equal 'Sandbox'**

```sql
[Domain] != 'Sandbox'
```

- If you want to query data from a table where certain rows are still in draft/Create Request format,  use the syntax of **Draft(\[Column Name])** to see the draft changes. Make sure to also include **\[Column Name]** as well to include non-draft rows.
- The default **ORDER BY** function will always set **ascending** unless specified.
- When using a Boolean query, **1** = true, and **0** = false.
- Cinchy labels version history differently in other SQL systems. For example, most SQL systems use the following convention: "version 1.2.4". Cinchy follows these conventions but splits them up. The two **ORDER BY** options, **Version** and **Draft Data**. are the version numbers. Version is the first number, and Draft Data is the second number in the sequence. For Example: `\[Version]: 2` and `\[Draft Data]: 5` means the overall version of the policy is 2.5
- If there is an error in your CQL code, an error message will appear in your Query Results in the query builder indicating the column and row your error location.
- When using an **INSERT INTO** clause, the order of the columns input must match the same order as in the **VALUES** section.
- Putting a **\*** symbol after a **SELECT** statement in the Query Builder will return a series of system columns attached to each table entry.

```sql
SELECT *
```

- Use **RESOLVELINK** when inserting or updating values for link columns. In the below example, we use **(RESOLVELINK(@Manager,'Full Name'))** because the Manager column is a linked column pulling from the Employee table. The syntax for using Resolve Link is: _ResolveLink('value(s)','column in target table')_

```sql
INSERT INTO [Human Resources].[Employee RY] ([First Name], [Last Name], [Employee ID], [Manager])
VALUES (@FirstName, @LastName, @EmployeeID, (RESOLVELINK(@Manager, ‘Full Name’))
```

### Query return results <a href="#3.-query-return-results" id="3.-query-return-results"></a>

You can specify what your results return as in the Query Builder

| Query Return Results                                                                    | Description                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Query Result (Approved Data Only) (\*This is the default when creating a new query)** | This is the default return type, it returns a table from a select query with only approved data for Maker/Checker-enabled tables, or all data for tables without Maker/Checker-enabled. Use this to query approved data, rather than drafts. For example, external APIS.|
| **Query Result (Including Draft Data)**                                                 | This return type returns a table from a select query with only draft data for Maker/Checker-enabled tables. Use this return type when looking to display results of records that are pending approval**.**                                                                                    |
| **Query Result (Including Version History)**                                            | This return type returns a table from a select query with historical data for all tables, as seen in the Collaboration Log of any record. This data includes all changes that happened to all records within the scope of the select query.                                                   |
| **Number of Rows Affected**                                                             | This return type returns a single string response with the number of rows affected if the last statement in the query is an INSERT, UPDATE, or DELETE statement.                                                                                                                              |
| **Execute DDL Script**                                                                  | Use this return type when your query has DDL commands that make schema changes such as CREATE\|ALTER\|DROP TABLE, CREATE\|ALTER\|DROP VIEW, or CREATE\|DROP INDEX.                                                                                                                  |
| **Single Value (First Column of First Row)**                                            | This return type returns a result of 1 row x 1 column, irrespective of the underlying result set.                                                                                                                                                                                             |

### Data return types <a href="#3.-data-return-types" id="3.-data-return-types"></a>

The following tables show the data types that a **Cinchy Data Type** translates to in the database:

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
