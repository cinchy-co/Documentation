# Return Date and Time Parts

## Overview

The return date and time part functions covered in this section are:

- [DATENAME](return-date-and-time-parts.md#datename-transact-sql)
- [DATEPART](return-date-and-time-parts.md#datepart-transact-sql)
- [DAY](return-date-and-time-parts.md#day-transact-sql)
- [MONTH](return-date-and-time-parts.md#month-transact-sql)
- [YEAR](return-date-and-time-parts.md#year-transact-sql)

## DATENAME <a href="#datename-transact-sql" id="datename-transact-sql"></a>

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

The `DATENAME` function returns a character string representing the specified _datepart_ of the specified _date_.

#### Syntax

```sql
DATENAME ( datepart , date )
```

#### Arguments

`datepart`\
The specific part of the _date_ argument that `DATENAME` will return. This table lists all valid _datepart_ arguments.

| _datepart_  |
| ----------- |
| year        |
| quarter     |
| month       |
| dayofyear   |
| day         |
| week        |
| weekday     |
| hour        |
| minute      |
| second      |
| millisecond |
| microsecond |
| nanosecond  |
| TZoffset    |
| ISO_WEEK    |

`date`

An expression that can resolve to one of the following data types:

- date
- datetime
- datetimeoffset
- datetime2
- smalldatetime
- time

For _date_, `DATENAME` will accept a column expression, expression, string literal, or user-defined variable. Use four-digit years to avoid ambiguity issues.

#### Return types

```sql
nvarchar
```

#### Remarks

Use `DATENAME` in the following clauses:

- GROUP BY
- HAVING
- ORDER BY
- SELECT \<list>
- WHERE

#### Examples

`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`

Result Set

| _datepart_  | Return value |
| ----------- | ------------ |
| year        | 2007         |
| quarter     | 4            |
| month       | October      |
| dayofyear   | 303          |
| day         | 30           |
| week        | 44           |
| weekday     | Tuesday      |
| hour        | 12           |
| minute      | 15           |
| second      | 32           |
| millisecond | 123          |
| microsecond | 123456       |
| nanosecond  | 123456700    |
| TZoffset    | +05:10       |
| ISO_WEEK    | 44           |

## DATEPART <a href="#datepart-transact-sql" id="datepart-transact-sql"></a>

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

DATEPART function returns an integer representing the specified _datepart_ of the specified _date_.

### Syntax

```sql
DATEPART ( datepart , date )
```

### Arguments

`datepart`\
The specific part of the _date_ argument for which `DATEPART` will return an **integer**. This table lists all valid _datepart_ arguments.

`date`\
An expression that resolves to one of the following data types:

- date
- datetime
- datetimeoffset
- datetime2
- smalldatetime
- time

For _date_, `DATEPART` will accept a column expression, expression, string literal, or user-defined variable. Use four-digit years to avoid ambiguity issues.

### Return type

```sql
int
```

### Remarks

`DATEPART` can be used in the select list, WHERE, HAVING, GROUP BY, and ORDER BY clauses.

DATEPART implicitly casts string literals as a **datetime2** type in SQL Server 2019 (15.x). This means that DATENAME does not support the format YDM when the date is passed as a string. You must explicitly cast the string to a **datetime** or **smalldatetime** type to use the YDM format.

#### Example 1

This example returns the Base Year.

```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0)

-- Returns: 1900 1 1
```

#### Example 2

This example returns the Day part of the Date.

```sql
SELECT TOP(1) DATEPART(day,[Modified])
FROM [Cinchy].[Domains]
WHERE [Deleted] IS NULL

-- Returns: 20
```

#### Example 3

This example returns the Year part of the Date.

```sql
SELECT TOP(1) DATEPART(year,[Modified])
FROM [Cinchy].[Domains]
WHERE [Deleted] IS NULL

-- Returns: 2020
```

## DAY <a href="#day-transact-sql" id="day-transact-sql"></a>

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}
DAY function returns an integer that represents the day (day of the month) of the specified _date_.

### Syntax

```sql
DAY ( date )
```

### Arguments

`date`\
An expression that resolves to one of the following data types:

- date
- datetime
- datetimeoffset
- datetime2
- smalldatetime
- time

For _date_, `DAY` will accept a column expression, expression, string literal, or user-defined variable.

### Return types

```sql
int
```

#### Example 1

This returns `30` - the number of the day itself

```sql
SELECT DAY('2015-04-30 01:01:01.1234567')
```

#### Example 2

This statement returns `1900, 1, 1`. The _date_ argument has a number value of `0`. SQL Server interprets `0` as January 1, 1900.

```sql
SELECT YEAR(0), MONTH(0), DAY(0)
```

## MONTH <a href="#month-transact-sql" id="month-transact-sql"></a>

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

MONTH returns an integer that represents the month of the specified _date_.

### Syntax

```sql
MONTH ( date )
```

### Arguments

`date`\
Is an expression that can be resolved to a **time**, **date**, **smalldatetime**, **datetime**, **datetime2**, or **datetimeoffset** value. The _date_ argument can be an expression, column expression, user-defined variable, or string literal.

### Return types

```sql
int
```

#### Example 1

The following statement returns `4`. This is the number of the month.

```sql
SELECT MONTH('2007-04-30T01:01:01.1234567 -07:00')
```

#### Example 2

The following statement returns `1900, 1, 1`. The argument for _date_ is the number `0`. SQL Server interprets `0` as January 1, 1900.

```sql
SELECT YEAR(0), MONTH(0), DAY(0)
```

## YEAR <a href="#year-transact-sql" id="year-transact-sql"></a>

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

YEAR function returns an integer that represents the year of the specified _date_.

#### Syntax

```sql
YEAR ( date )
```

#### Arguments

`date`\
Is an expression that can be resolved to a **time**, **date**, **smalldatetime**, **datetime**, **datetime2**, or **datetimeoffset** value. The _date_ argument can be an expression, column expression, user-defined variable or string literal.

#### Return Types

```sql
int
```

**Example 1:**

The following statement returns `2020`. This is the number of the year.

```sql
SELECT YEAR('2020-04-30T01:01:01.1234567-07:00')
```

**Example 2:**

The following statement returns `1900, 1, 1`. The argument for _date_ is the number `0`. SQL Server interprets `0` as January 1, 1900.

```sql
SELECT YEAR(0), MONTH(0), DAY(0)
```
