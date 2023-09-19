# Modify Date and Time values

## Overview

The modify date and time value functions covered in this section are:

- ​[DATEADD](modify-date-and-time-values.md#dateadd-transact-sql)
- [​EOMONTH](modify-date-and-time-values.md#eomonth-transact-sql)
- ​[SWITCHOFFSET](modify-date-and-time-values.md#switchoffset-transact-sql)
- ​[TODATETIMEOFFSET​](modify-date-and-time-values.md#todatetimeoffset-transact-sql)

## DATEADD <a href="#dateadd-transact-sql" id="dateadd-transact-sql"></a>

DATEADD function adds a specified _number_ value to a specified _datepart_ of an input _date_ value, and then returns that modified value.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
DATEADD (datepart , number , date )
```

### Arguments

`datepart`\
The part of _date_ to which `DATEADD` adds an **integer** _number_. This table lists all valid _datepart_ arguments.

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

`number`\
An expression that can resolve to an int that `DATEADD` adds to a _datepart_ of _date_. `DATEADD` accepts user-defined variable values for _number_. `DATEADD` will truncate a specified _number_ value that has a decimal fraction. It won't round the _number_ value in this situation.

`date`\
An expression that can resolve to one of the following values:

- date
- datetime
- datetimeoffset
- datetime2
- smalldatetime
- time

For _date_, `DATEADD` will accept a column expression, expression, string literal, or user-defined variable. A string literal value must resolve to a *datetime*. Use four-digit years to avoid ambiguity issues.

### Return types

The return value data type for this method is dynamic. The return type depends on the argument supplied for `date`. If the value for `date` is a string literal date, `DATEADD` returns a *datetime* value. If another valid input data type is supplied for `date`, `DATEADD` returns the same data type. `DATEADD` raises an error if the string literal seconds scale exceeds three decimal place positions (.nnn) or if the string literal contains the time zone offset part.

#### Example 1

Incrementing datepart by an interval of 1

```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111'

SELECT 'year', DATEADD(year,1,@datetime2)
UNION ALL
SELECT 'quarter',DATEADD(quarter,1,@datetime2)
UNION ALL
SELECT 'month',DATEADD(month,1,@datetime2)
UNION ALL
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)
UNION ALL
SELECT 'day',DATEADD(day,1,@datetime2)
UNION ALL
SELECT 'week',DATEADD(week,1,@datetime2)
UNION ALL
SELECT 'weekday',DATEADD(weekday,1,@datetime2)
UNION ALL
SELECT 'hour',DATEADD(hour,1,@datetime2)
UNION ALL
SELECT 'minute',DATEADD(minute,1,@datetime2)
UNION ALL
SELECT 'second',DATEADD(second,1,@datetime2)
UNION ALL
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)
UNION ALL
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)
UNION ALL
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2)
```

#### Example 2

Incrementing more than one level of datepart in one statement

Each of these statements increments _datepart_ by a _number_ large enough to additionally increment the next higher _datepart_ of _date_:

```sql
DECLARE @datetime2 datetime2
SET @datetime2 = '2007-01-01 01:01:01.1111111'

--Statement                                           Result
--------------------------------------------------------------------------
SELECT DATEADD(quarter,4,@datetime2)         --2008-01-01 01:01:01.1111111
SELECT DATEADD(month,13,@datetime2)          --2008-02-01 01:01:01.1111111
SELECT DATEADD(dayofyear,365,@datetime2)     --2008-01-01 01:01:01.1111111
SELECT DATEADD(day,365,@datetime2)           --2008-01-01 01:01:01.1111111
SELECT DATEADD(week,5,@datetime2)            --2007-02-05 01:01:01.1111111
SELECT DATEADD(weekday,31,@datetime2)        --2007-02-01 01:01:01.1111111
SELECT DATEADD(hour,23,@datetime2)           --2007-01-02 00:01:01.1111111
SELECT DATEADD(minute,59,@datetime2)         --2007-01-01 02:00:01.1111111
SELECT DATEADD(second,59,@datetime2)         --2007-01-01 01:02:00.1111111
SELECT DATEADD(millisecond,1,@datetime2)     --2007-01-01 01:01:01.1121111
```

#### Example 3

Using expressions as arguments for the number and date parameters

_Specifying a column as date_

This example adds `2` (two) days to each value in the `OrderDate` column, to derive a new column named `PromisedShipDate`:

```sql
SELECT
    [Order Number],
    [Order Date],
    DATEADD(day, 2, [Order Date]) AS [Promised Ship Date]
FROM
    [Sales].[Order Header]
WHERE
    [Deleted] IS NULL
```

## EOMONTH <a href="#eomonth-transact-sql" id="eomonth-transact-sql"></a>

The EOMONTH function returns the last day of the month containing a specified date, with an optional offset.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
EOMONTH ( start_date [, month_to_add ] )
```

### Arguments

`start_date`\
A date expression that specifies the date for which to return the last day of the month.

`month_to_add`\
An optional integer expression that specifies the number of months to add to _start_date_.

If the _month_to_add_ argument has a value, then `EOMONTH` adds the specified number of months to _start_date_, and then returns the last day of the month for the resulting date. If this addition overflows the valid range of dates, then `EOMONTH` will raise an error.

### Return Types

```sql
date
```

#### Remarks

The `EOMONTH` function can remote to SQL Server 2012 (11.x) servers and higher. It can't be remote to servers with a version lower than SQL Server 2012 (11.x).

#### Example 1

EOMONTH with explicit datetime type

```sql
DECLARE @date DATETIME = '12/1/2011'
SELECT EOMONTH( @date ) AS Result
```

#### Example 2

EOMONTH with string parameter and implicit conversion

```sql
DECLARE @date VARCHAR(255) = '12/1/2011'
SELECT EOMONTH( @date ) AS Result
```

#### Example 3

EOMONTH with and without the *month_to_add* parameter

```sql
DECLARE @date DATETIME = GETDATE()

SELECT
    EOMONTH ( @date ) AS 'This Month',
    EOMONTH ( @date, 1 ) AS 'Next Month',
    EOMONTH ( @date, -1 ) AS 'Last Month'
```

## SWITCHOFFSET <a href="#switchoffset-transact-sql" id="switchoffset-transact-sql"></a>

The SWITCHOFFSEET function returns a **datetimeoffset** value that's changed from the stored time zone offset to a specified new time zone offset.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
SWITCHOFFSET ( DATETIMEOFFSET, time_zone )
```

### Arguments

`DATETIMEOFFSET`_DATETIMEOFFSET_\
Is an expression that can be resolved to a **datetimeoffset(n)** value.

`time_zone`\
Is a character string in the format \[+|-]TZH:TZM or a signed integer (of minutes) that represents the time zone offset, and is assumed to be daylight-saving aware and adjusted.

### Return Types

**datetimeoffset** with the fractional precision of the _DATETIMEOFFSET_ argument.

#### Remarks

Use SWITCHOFFSET to select a **datetimeoffset** value into a time zone offset that's different from the time zone offset that was originally stored. SWITCHOFFSET doesn't update the stored *time_zone* value.

SWITCHOFFSET can be used to update a **datetimeoffset** column.

Using SWITCHOFFSET with the function GETDATE() can cause the query to run slowly. This is because the query optimizer is unable to obtain accurate cardinality estimates for the datetime value. To resolve this problem, use the OPTION (RECOMPILE) query hint to force the query optimizer to recompile a query plan the next time the same query is executed. The optimizer will then have accurate cardinality estimates and will produce a more efficient query plan.

#### Example

```sql
SELECT SWITCHOFFSET(ColDatetimeoffset, '-08:00')
```

## TODATETIMEOFFSET <a href="#todatetimeoffset-transact-sql" id="todatetimeoffset-transact-sql"></a>

TODATETIMEOFFSET function returns a **datetimeoffset** value that's translated from a **datetime2** expression.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
For a full list of in-progress function translations, see [the CQL functions reference page](../../cql-functions-master-list.md).
{% endhint %}
### Syntax

```sql
TODATETIMEOFFSET ( expression , time_zone )
```

### Arguments

`expression`\
Is an expression that resolves to a datetime2 value.

`time_zone`\
Is an expression that represents the time zone offset in minutes (if an integer), for example -120, or hours and minutes (if a string), for example '+13:00'. The range is +14 to —14 (in hours). The expression is interpreted in local time for the specified time_zone.

### Return Types

**datetimeoffset**. The fractional precision is the same as the _datetime_ argument.

#### Example 1

Changing the time zone offset of the current date and time

The following example changes the zone offset of the current date and time to time zone `-07:00`.

```sql
DECLARE @todaysDateTime datetime2
SET @todaysDateTime = GETDATE()

SELECT TODATETIMEOFFSET (@todaysDateTime, '-07:00')

-- RETURNS 2019-04-22 16:23:51.7666667 -07:00
```

#### Example 2

Changing the time zone offset in minutes

The following example changes the current time zone to `-120` minutes.

```sql
SELECT TODATETIMEOFFSET(SYSDATETIME(), -120)

-- RETURNS: 2019-04-22 11:39:21.6986813 -02:00
```

#### Example 3

Adding a 13-hour time zone offset

The following example adds a 13-hour time zone offset to a date and time.

```sql
SELECT TODATETIMEOFFSET(SYSDATETIME(), '+13:00')

-- RETURNS: 2019-04-22 11:39:29.0339301 +13:00
```
