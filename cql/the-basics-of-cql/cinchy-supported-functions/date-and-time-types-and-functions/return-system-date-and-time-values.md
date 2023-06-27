# Return System Date and Time Values

## 1. Overview

SQL derives all system date and time values from the operating system of the computer on which the instance of SQL Server runs.

SQL Server 2019 (15.x) derives the date and time values through use of the GetSystemTimeAsFileTime() Windows API. The accuracy depends on the computer hardware and version of Windows on which the instance of SQL Server running. This API has a precision fixed at 100 nanoseconds. Use the GetSystemTimeAdjustment() Windows API to determine the accuracy.The return system date and time value functions covered in this section are:

* ​[SYSDATETIME​](return-system-date-and-time-values.md#sysdatetime)
* [​SYSDATETIMEOFFSET​](return-system-date-and-time-values.md#sysdatetimeoffset)
* [​SYSUTCDATETIME​](return-system-date-and-time-values.md#sysutcdatetime)

## SYSDATETIME

SYSDATETIME returns a **datetime2(7)** value that contains the date and time of the computer on which the instance of SQL Server is running.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.&#x20;

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
SYSDATETIME ( )
```

#### Return Types

```sql
datetime2(7)
```

#### Remarks

SQL statements can refer to SYSDATETIME anywhere they can refer to a datetime2(7) expression.

SYSDATETIME is a nondeterministic function. Views and expressions that reference this function in a column cannot be indexed.

**Example 1: Getting the Current System Date and Time**

```sql
SELECT
  SYSDATETIME(),
  SYSDATETIMEOFFSET(),
  SYSUTCDATETIME(),
  GETDATE(),
  GETUTCDATE()

/*
Returned:
SYSDATETIME()        2020-04-30 13:10:02.0474381
SYSDATETIMEOFFSET()  2020-04-30 13:10:02.0474381 -07:00
SYSUTCDATETIME()     2020-04-30 20:10:02.0474381
GETDATE()            2020-04-30 13:10:02.047
GETUTCDATE()         2020-04-30 20:10:02.047  
*/
```

**Example 2: Getting the Current System Date**

```sql
SELECT
    CONVERT(date, SYSDATETIME()),
    CONVERT(date, SYSDATETIMEOFFSET()),
    CONVERT(date, SYSUTCDATETIME()),
    CONVERT(date, GETDATE()),
    CONVERT(date, GETUTCDATE())

/* All returned 2020-04-30 */
```

**Example 3: Getting the Current System Time**

```sql
SELECT
    CONVERT(time, SYSDATETIME()),
    CONVERT(time, SYSDATETIMEOFFSET()),
    CONVERT(time, SYSUTCDATETIME()),
    CONVERT(time, GETDATE()),
    CONVERT(time, GETUTCDATE())

/*
Returned:
SYSDATETIME()        13:18:45.3490361
SYSDATETIMEOFFSET()  13:18:45.3490361
SYSUTCDATETIME()     20:18:45.3490361
GETDATE()            13:18:45.3470000
GETUTCDATE()         20:18:45.3470000  
*/
```

## SYSDATETIMEOFFSET

SYSDATETIMEOFFSET returns a **datetimeoffset(7)** value that contains the date and time of the computer on which the instance of SQL Server is running. The time zone offset is included.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.&#x20;

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
SYSDATETIMEOFFSET()
```

#### Return Types

```sql
datetimeoffset(7)
```

#### Remarks

SQL statements can refer to SYSDATETIMEOFFSET anywhere they can refer to a **datetimeoffset** expression.

SYSDATETIMEOFFSET is a nondeterministic function. Views and expressions that reference this function in a column cannot be indexed.

**Example 1: Showing Formats Returned by the Date and Time Functions**

```sql
SELECT
    SYSDATETIME() AS [SYSDATETIME()],
    SYSDATETIMEOFFSET() AS [SYSDATETIMEOFFSET()],
    SYSUTCDATETIME() AS [SYSUTCDATETIME()],
    GETDATE() AS [GETDATE()],
    GETUTCDATE() AS [GETUTCDATE()]
```

**Example 2: Converting Date and Time to Date**

```sql
SELECT
    CONVERT(date, SYSDATETIME()),
    CONVERT(date, SYSDATETIMEOFFSET()),
    CONVERT(date, SYSUTCDATETIME()),
    CONVERT(date, GETDATE()),
    CONVERT(date, GETUTCDATE())
```

**Example 3: Converting Date and Time to Times**

```sql
SELECT
    CONVERT(time, SYSDATETIME()) AS [SYSDATETIME()],
    CONVERT(time, SYSDATETIMEOFFSET()) AS [SYSDATETIMEOFFSET()],
    CONVERT(time, SYSUTCDATETIME()) AS [SYSUTCDATETIME()],
    CONVERT(time, GETDATE()) AS [GETDATE()],
    CONVERT(time, GETUTCDATE()) AS [GETUTCDATE()]
```

## SYSUTCDATETIME

SYSUTCDATETIME returns a **datetime2** value that contains the date and time of the computer on which the instance of SQL Server is running. The date and time are returned as UTC time (Coordinated Universal Time). The fractional second precision specification has a range from 1 to 7 digits. The default precision is 7 digits.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.&#x20;

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../../cql-functions-master-list.md)[.](../)
{% endhint %}

#### Syntax

```sql
SYSUTCDATETIME()
```

#### Return Types

```sql
datetime2
```

#### Remarks

SQL statements can refer to SYSUTCDATETIME anywhere they can refer to a **datetime2** expression.

SYSUTCDATETIME is a nondeterministic function. Views and expressions that reference this function in a column cannot be indexed.

**Example 1: Showing Formats Returned by Date and Time functions**

```sql
SELECT
    SYSDATETIME() AS [SYSDATETIME()],
    SYSDATETIMEOFFSET() AS [SYSDATETIMEOFFSET()],
    SYSUTCDATETIME() AS [SYSUTCDATETIME()],
    GETDATE() AS [GETDATE()],
    GETUTCDATE() AS [GETUTCDATE()]
```

**Example 2: Converting Date and Time to Date**

```sql
SELECT
    CONVERT(date, SYSDATETIME()),
    CONVERT(date, SYSDATETIMEOFFSET()),
    CONVERT(date, SYSUTCDATETIME()),
    CONVERT(date, GETDATE()),
    CONVERT(date, GETUTCDATE())
```

**Example 3: Converting Date and Time to Time**

```sql
DECLARE @DateTime DATETIME = GETDATE()
DECLARE @Time TIME

SELECT
    @Time = CONVERT(time, @DateTime)

SELECT
    @Time AS 'Time',
    @DateTime AS 'Date Time'
```
