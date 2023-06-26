# Cinchy DML Statements

## 1. Overview

DML is used to add, retrieve, update and manipulate data.The Cinchy DML statements covered on this page are:

- [​SELECT](cinchy-dml-statements.md#select)​
- [​INSERT](cinchy-dml-statements.md#insert-1)​
- [​UPDATE​](cinchy-dml-statements.md#update)
- [​DELETE​](cinchy-dml-statements.md#delete)
- ​[IF](cinchy-dml-statements.md#if)​
- [​Declare Variables​](cinchy-dml-statements.md#declare-variable)
- ​[Set Variables](cinchy-dml-statements.md#set-variable)​

## SELECT <a href="#select" id="select"></a>

Use the `SELECT` statement to select data from a database. The data returned is stored in a result table, called the result-set.

_Note: Table name requires domain prefix._

### Syntax

```sql
SELECT [Column1],[Column2], ...
FROM [Domain].[Table_Name]
```

### Example

```sql
SELECT [Full Name], [Email Address], [Start Date]
FROM [HR].[Employee]
```

## Nested JSONs <a href="#insert" id="insert"></a>

You can create a query that will produce a nested JSON by wrapping it in an outer `SELECT` statement, such as in the example below.

{% hint style="warning" %}
Note that you should set the return type to _"Single Value (First Column of First Row)"._
{% endhint %}

### Example

```sql
SELECT (
SELECT
(SELECT p1.[Parent A],p1.[Parent B]
FROM [QA].[Parents 5080] p1 WHERE p1.[Parents]=p.[Parents]
FOR JSON PATH) As Parents,
(SELECT c.[Name] AS [Child Name]
FROM [QA].[Children 5080] c
WHERE c.[Deleted] IS NULL AND c.[Parents]= p.[Parents]
FOR JSON PATH) AS Children
FROM [QA].[Parents 5080] p
WHERE p.[Deleted] IS NULL
FOR JSON PATH, INCLUDE_NULL_VALUES)

```

### Output

```json
[
  {
    "Parents": [{ "Parent A": "Tom", "Parent B": "Maria" }],
    "Children": [
      { "Child Name": "John" },
      { "Child Name": "Theodor" },
      { "Child Name": "Lynette" }
    ]
  },
  {
    "Parents": [{ "Parent A": "Ray", "Parent B": "Sofia" }],
    "Children": [{ "Child Name": "Stephen" }]
  },
  {
    "Parents": [{ "Parent A": "Greg", "Parent B": "Mona" }],
    "Children": [{ "Child Name": "Elizabeth" }, { "Child Name": "Martin" }]
  },
  {
    "Parents": [{ "Parent A": "Henry", "Parent B": "Susanne" }],
    "Children": null
  }
]
```

## INSERT <a href="#insert" id="insert"></a>

Use an `INSERT` statement to add new rows to a table or view. You can also include a `SELECT` statement to identify that another table or view contains the data for the new row or rows.

{% hint style="info" %}
The table name requires the domain prefix.
{% endhint %}


```sql
INSERT INTO [Domain].[Table_Name] ([Column1],[Column2],[Column3], ...)
VALUES ([Value1],[Value2],[Value3], ...)
```

### Example

```sql
INSERT INTO [Contacts].[People] ([First Name],[Last Name],[Address],[Job Title])
VALUES (@firstname, @lastname, @address, @jobtitle)
```

## UPDATE <a href="#update" id="update"></a>

Use the `UPDATE` statement to change the data in a table. The `UPDATE` statement modifies zero or more rows of a table, depending on how many rows meet the search condition  specified in the `WHERE` clause. You can also use an `UPDATE` statement to specify the values to be updated in a single row. To do this, specify the constants, host variables, expressions, `DEFAULT`, or `NULL`. Specify `NULL` to remove a value from a row's column (without removing the row).

{% hint style="info" %}
The table name requires the domain prefix.
{% endhint %}

### Syntax

```sql
UPDATE [Domain].[Table_Name]
SET [Column1] = [Value1], [Column2] = [value2], ...
WHERE [condition]
```

### Example

```sql
UPDATE [Revenue].[Customers]
SET [ContactName] = 'Alfred Schmidt', [City]= 'Frankfurt'
WHERE [Customer_ID] = 1;
```

## DELETE <a href="#delete" id="delete"></a>

Use the `DELETE` statement to remove entire rows from a table. The number of rows deleted depends on how many rows satisfy the search condition specified in the `WHERE` statement.

{% hint style="info" %}
The table name requires the domain prefix.
{% endhint %}

### Syntax

```sql
DELETE FROM [Domain].[Table_Name] WHERE [condition]
```

### Example

```sql
DELETE FROM [Revenue].[Customers] WHERE [CustomerName] = 'Alfreds Futterkiste';
```

## IF <a href="#if" id="if"></a>

Use the IF statement to execute a condition. If the condition is satisfied, then the Boolean expressions returns `TRUE` value. The optional `ELSE` keyword introduces another statement that executes when the `IF` condition is not satisfied and returns `FALSE` value.

### Syntax

```sql
IF [condition] THEN [value_if_true] ELSE [value_if_false]
```

### Example

```sql
IF 500<1000 THEN 'YES' ELSE 'NO'
```

## DECLARE <a href="#declare-variable" id="declare-variable"></a>

Use the `DECLARE` statement to declare a variable.

### Syntax

```sql
DECLARE @variable_name variable_type;
```

### Example

```sql
DECLARE @var varchar(50);
```

## Set Variable <a href="#set-variable" id="set-variable"></a>

Use the `SET` variable to assign a value to a variable.

### Syntax

```sql
SET @variable_name = variable_value
```

### Example

```sql
SET @var = 'Alfreds Futterkiste'
```
