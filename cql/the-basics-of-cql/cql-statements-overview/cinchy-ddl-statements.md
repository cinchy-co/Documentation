# Cinchy DDL Statements

## Overview

DDL is used to create a database schema and define constraints.The Cinchy DDL functions covered on this page are:

* [​Create Table​](cinchy-ddl-statements.md#create-table)
* [​Alter Table​](cinchy-ddl-statements.md#alter-table)
* [​Drop Table​](cinchy-ddl-statements.md#drop-table)
* ​[Create View​](cinchy-ddl-statements.md#create-view)
* [​Alter View​](cinchy-ddl-statements.md#alter-view)
* ​[Drop View​](cinchy-ddl-statements.md#drop-view)
* ​[Create Index](cinchy-ddl-statements.md#create-index)​
* ​[Drop Index​](cinchy-ddl-statements.md#drop-index)

## Data Type Translations <a href="#data-type-translations" id="data-type-translations"></a>

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

| Binary    | Yes/No |
| --------- | ------ |
| VarBinary | Bit    |

## Create Table <a href="#create-table" id="create-table"></a>

The **CREATE TABLE** statement is used to create a new table in a database.

The column parameters specify the names of the columns of the table.The datatype parameter specifies the data type the column can hold (such as varchar, char, int, date).

**Syntax**

```sql
CREATE TABLE [Domain].[Table_Name] (
 [Column1] [datatype],
 [Column2] [datatype],
 [Column3] [datatype]
)
```

**Example**

```sql
CREATE TABLE [HR].[Employees] (
 [Employee_ID] INT,
 [LastName] VARCHAR(255),
 [FirstName] VARCHAR(255),
 [Start Date] SMALLDATETIME
)
```

## Alter Table <a href="#alter-table" id="alter-table"></a>

The **ALTER TABLE** statement is used to add, delete, or modify columns in an existing table.

The **ALTER TABLE** statement is also used to add and drop various constraints on an existing table.

**Syntax**

```sql
ALTER TABLE [Domain].[Table_Name]
ADD [column_name] datatype
```

**Example**

```sql
ALTER TABLE [HR].[Employees]
ADD [Email] VARCHAR(255)
```

## Drop Table <a href="#drop-table" id="drop-table"></a>

The **DROP TABL**E statement is used to drop an existing table in a database.

**Syntax**

```sql
DROP TABLE [Domain].[Table_Name]
```

**Example**

```sql
DROP TABLE [HR].[Employees]
```

## Create View <a href="#create-view" id="create-view"></a>

In SQL, a view is a virtual table based on the result-set of an SQL statement.

A view has rows and columns, taken from fields of an original table and presents them with a different structure and organization. This is useful when only some columns of a table may be relevant to a user, so they don't have to see information from other columns that may be irrelevant to them.

SQL functions and the **WHERE** statement can be added to a view to present the data as if the data was coming from one single table.

**Syntax**

```sql
CREATE VIEW [Domain].[View_Name] AS
SELECT [Column1], [Column2], ...
FROM [Domain].[Table_Name]
WHERE [condition]
```

**Example**

```sql
CREATE VIEW [Detractors] AS
SELECT [Client Name].[Full Name], [Client Name].[Eamil Address], [Advisor].[Full Name], [Q1], [Q2]
FROM [Wealth].[Net Promoter Score]
WHERE [Q1] BETWEEN 0 AND 6
```

## Alter View <a href="#alter-view" id="alter-view"></a>

Modifies a previously created view, including indexed views. **ALTER VIEW** doesn't affect dependent stored procedures or triggers and doesn't change permissions.

**Syntax**

```sql
ALTER VIEW [Domain].[View_Name]   
AS <select_statement>
```

**Example**

```sql
ALTER VIEW [Wealth].[Detractors]
AS
SELECT [Client Name].[Full Name], [Client Name].[Eamil Address], [Advisor].[Full Name], [Q1], [Q2]
FROM [Wealth].[Net Promoter Score]
WHERE [Q1] BETWEEN 0 AND 6
```

## Drop View <a href="#drop-view" id="drop-view"></a>

Use the **DROP VIEW** command to delete a view.

**Syntax**

```sql
DROP VIEW [Domain].[View_Name]
```

**Example**

```sql
DROP VIEW [Wealth].[Detractors]
```

## Create Index <a href="#create-index" id="create-index"></a>

The **CREATE INDEX** statement is used to create[ indexes](../../../guides-for-using-cinchy/builder-guides/creating-tables/indexing-and-partitioning.md) in tables.

**Syntax**

```sql
CREATE INDEX [index_name]
ON [Domain].[Table_Name] ([Column1],[Column2], ...)
```

**Example**

```sql
CREATE INDEX [idx_lastname]
ON [HR].[Employees] ([LastName])
```

## Drop Index <a href="#drop-index" id="drop-index"></a>

The **DROP INDEX** statement is used to delete an index on a table.

**Syntax**

```sql
DROP INDEX [index_name] ON [Domain].[Table_Name]
```

**Example**

```sql
DROP INDEX [idx_lastname] ON [HR].[Employees]
```
