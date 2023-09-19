# Return date and time difference values

## Overview

The return date and time difference value functions covered in this section are:

- [DATEDIFF](return-date-and-time-difference-values.md#datediff-transact-sql)
- [DATEDIFF_BIG](return-date-and-time-difference-values.md#datediff_big-transact-sql)

## DATEDIFF <a href="#datediff-transact-sql" id="datediff-transact-sql"></a>

The `DATDIFF` function returns the count of the specified datepart boundaries crossed between the specified _startdate_ and _enddate_.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
DATEDIFF ( datepart , startdate , enddate )
```

#### Arguments

`datepart`\
The units in which `DATEDIFF` reports the difference between the _startdate_ and _enddate_. Commonly used _datepart_ units include `month` or `second`.

The _datepart_ value can't be specified in a variable, nor as a quoted string like `'month'`.

The following table lists all the valid _datepart_ values. `DATEDIFF` accepts either the full name of the _datepart_, or any listed abbreviation of the full name.

| _datepart_  |
| ----------- |
| year        |
| quarter     |
| month       |
| dayofyear   |
| day         |
| week        |
| hour        |
| minute      |
| second      |
| millisecond |
| microsecond |
| nanosecond  |

`startdate`\
An expression that can resolve to one of the following values:

- date
- datetime
- datetimeoffset
- smalldatetime
- time

Use four-digit years to avoid ambiguity.

#### Return Types

```sql
int
```

#### Remarks

Use `DATEDIFF` in the `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` and `ORDER BY` clauses.

`DATEDIFF` implicitly casts string literals as a **datetime2** type. This means that `DATEDIFF` doesn't support the format YDM when the date is passed as a string. You must explicitly cast the string to a **datetime** or **smalldatetime** type to use the YDM format.

Specifying `SET DATEFIRST` has no effect on `DATEDIFF`. `DATEDIFF` always uses Sunday as the first day of the week to ensure the function operates in a deterministic way.

`DATEDIFF` may overflow with a precision of **minute** or higher if the difference between _enddate_ and _startdate_ returns a value that's out of range for **int**.

#### Example 1

Finding the number of days between two dates.

```sql
SELECT
    DATEDIFF(day, [Created], [Modified]) AS 'Duration'
FROM
    [Cinchy].[Tables]
WHERE
	[Deleted] IS NULL
```

#### Example 2

Specifying user-defined variables for _startdate_ and _enddate_

```sql
DECLARE @startdate datetime2 = '2007-05-05 12:10:09.3312722'
DECLARE @enddate datetime2 = '2007-05-04 12:10:09.3312722'
SELECT DATEDIFF(day, @startdate, @enddate)
```

Example 3: Specifying scalar system functions for startdate and enddate

```sql
SELECT DATEDIFF(millisecond, GETDATE(), SYSDATETIME())
```

## DATEDIFF_BIG <a href="#datediff_big-transact-sql" id="datediff_big-transact-sql"></a>

**`DATEIFF_BIG** function returns the count of the specified _datepart_ boundaries crossed between the specified _startdate_ and _enddate_.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
DATEDIFF_BIG ( datepart , startdate , enddate )
```

#### Arguments

`datepart`\
The part of _startdate_ and _enddate_ that specifies the type of boundary crossed.

This table lists all valid _datepart_ argument names and abbreviations.

| _datepart_ name |
| --------------- |
| year            |
| quarter         |
| month           |
| dayofyear       |
| day             |
| week            |
| hour            |
| minute          |
| second          |
| millisecond     |
| microsecond     |
| nanosecond      |

`startdate`\
An expression that can resolve to one of the following values:

- date
- datetime
- datetimeoffset
- smalldatetime
- time

For _date_, `DATEDIFF_BIG` will accept a column expression, expression, string literal, or user-defined variable. A string literal value must resolve to a **datetime**. Use four-digit years to avoid ambiguity issues. `DATEDIFF_BIG` subtracts _startdate_ from _enddate_. To avoid ambiguity, use four-digit years.

#### Return Types

```sql
Signed bigint
```

#### Remarks

Use `DATEDIFF_BIG` in the `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` and `ORDER BY` clauses.

`DATEDIFF_BIG` implicitly casts string literals as a **datetime2** type. This means that `DATEDIFF_BIG` doesn't support the format YDM when the date is passed as a string. You must explicitly cast the string to a **datetime** or **smalldatetime** type to use the YDM format.

Specifying `SET DATEFIRST` has no effect on `DATEDIFF_BIG`. `DATEDIFF_BIG` always uses Sunday as the first day of the week to ensure the function operates in a deterministic way.

`DATEDIFF_BIG` may overflow with a precision of **nanosecond** if the difference between _enddate_ and _startdate_ returns a value that's out of range for **BigInt**.

#### Example

This example uses different types of expressions as arguments for the _startdate_ and _enddate_ parameters. It calculates the number of day boundaries crossed between dates in two columns of a table.

```sql
SELECT
    DATEDIFF_BIG(day, [Created], [Modified]) AS 'Duration'
FROM
    [Cinchy].[Tables]
WHERE
	[Deleted] IS NULL
```
