# Return Date and Time Values From Their Parts

## Overview

The following return date and time values from their parts functions covered in this section are:

* [DATEFROMPARTS](return-date-and-time-values-from-their-parts.md#datefromparts)
* [DATETIME2FROMPARTS](return-date-and-time-values-from-their-parts.md#datetime2fromparts)
* [DATETIMEFROMPARTS ](return-date-and-time-values-from-their-parts.md#datetimefromparts)
* [DATETIMEOFFSETFROMPARTS](return-date-and-time-values-from-their-parts.md#datetimeoffsetfromparts)
* [SMALLDATETIMEFROMPARTS](return-date-and-time-values-from-their-parts.md#smalldatetimefromparts)
* [TIMEFROMPARTS](return-date-and-time-values-from-their-parts.md#timefromparts)

## DATEFROMPARTS

This function returns a **date** value that maps to the specified year, month, and day values.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
You can review the full list of in-progress function translations[ on the CQL functions master list page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
DATEFROMPARTS ( year, month, day )
```

### Arguments

`year`\
An integer expression that specifies a year.

`month`\
An integer expression that specifies a month, from 1 to 12.

`day`\
An integer expression that specifies a day.

### Return types

```sql
date
```

### Remarks

`DATEFROMPARTS` returns a **date** value, with the date portion set to the specified year, month and day, and the time portion set to the default. For invalid arguments, `DATEFROMPARTS` will raise an error. `DATEFROMPARTS` returns null if at least one required argument has a null value.

#### Example

```sql
SELECT DATEFROMPARTS(2010, 12, 31) AS Result
```

## DATETIME2FROMPARTS

`DATETIME2FROMPARTS` function returns a **datetime2** value for the specified date and time arguments. The returned value has a precision specified by the precision argument.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
You can review the full list of in-progress function translations[ on the CQL functions master list page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )
```

### Arguments

`year`\
An integer expression that specifies a year.

`month`\
An integer expression that specifies a month.

`day`\
An integer expression that specifies a day.

`hour`\
An integer expression that specifies the hours.

`minute`\
An integer expression that specifies the minutes.

`seconds`\
An integer expression that specifies the seconds.

`fractions`\
An integer expression that specifies a fractional seconds value.

`percision`\
An integer expression that specifies the precision of the **datetime2** value that `DATETIME2FROMPARTS` will return.

### Return types

```sql
datetime2( precision )
```

### Remarks

`DATETIME2FROMPARTS` returns a fully initialized **datetime2** value. `DATETIME2FROMPARTS` will raise an error if at least one required argument has an invalid value. `DATETIME2FROMPARTS` returns null if at least one required argument has a null value. However, if the _precision_ argument has a null value, `DATETIME2FROMPARTS` will raise an error.

The _fractions_ argument depends on the _precision_ argument. For example, for a _precision_ value of 7, each fraction represents 100 nanoseconds; for a _precision_ of 3, each fraction represents a millisecond. For a _precision_ value of zero, the value of _fractions_ must also be zero; otherwise, `DATETIME2FROMPARTS` will raise an error.

#### Example 1: Without fractions of a second

```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;
```

#### Example 2: With fractions of a second

1. When _fractions_ have a value of 5, and _precision_ has a value of 1, the value of _fractions_ represents 5/10 of a second.
2. When _fractions_ have a value of 50, and _precision_ has a value of 2, the value of _fractions_ represents 50/100 of a second.
3. When _fractions_ have a value of 500, and _precision_ has a value of 3, then the value of _fractions_ represents 500/1000 of a second.

```sql
SELECT DATETIME2FROMPARTS(2011, 8, 15, 14, 23, 44, 5, 1)
```

## DATETIMEFROMPARTS

DATETIMEFROMPARTS function returns a **datetime** value for the specified date and time arguments.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
You can review the full list of in-progress function translations[ on the CQL functions master list page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
DATETIMEFROMPARTS ( year, month, day, hour, minute, seconds, milliseconds )
```

### Arguments

`year`\
An integer expression that specifies a year.

`month`\
An integer expression that specifies a month.

`day`\
An integer expression that specifies a day.

`hour`\
An integer expression that specifies hours.

`minute`\
An integer expression that specifies minutes.

`seconds`\
An integer expression that specifies seconds.

`milliseconds`\
An integer expression that specifies milliseconds.

### Return types

```sql
datetime
```

### Remarks

`DATETIMEFROMPARTS` returns a fully initialized **datetime** value. `DATETIMEFROMPARTS` will raise an error if at least one required argument has an invalid value. `DATETIMEFROMPARTS` returns null if at least one required argument has a null value.

#### Example

```sql
SELECT DATETIMEFROMPARTS( 2010, 12, 31, 23, 59, 59, 0 ) AS Result
```

## DATETIMEOFFSETFROMPARTS

Returns a **datetimeoffset** value for the specified date and time arguments. The returned value has a precision specified by the precision argument and an offset as specified by the offset arguments.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
You can review the full list of in-progress function translations[ on the CQL functions master list page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )
```

### Arguments

`year`\
An integer expression that specifies a year.

`month`\
An integer expression that specifies a month.

`day`\
An integer expression that specifies a day.

`hour`\
An integer expression that specifies hours.

`minute`\
An integer expression that specifies minutes.

`seconds`\
An integer expression that specifies seconds.

`frcations`\
An integer expression that specifies a fractional seconds value.

`hour_offset`\
An integer expression that specifies the hour portion of the time zone offset.

`minute_offset`\
An integer expression that specifies the minute portion of the time zone offset.

`precision` \
An integer literal value that specifies the precision of the **datetimeoffset** value that `DATETIMEOFFSETFROMPARTS` will return.

### Return types

```sql
datetimeoffset( precision )
```

#### Remarks

`DATETIMEOFFSETFROMPARTS` returns a fully initialized **datetimeoffset** data type. The offset arguments represent the time zone offset. For omitted offset arguments, `DATETIMEOFFSETFROMPARTS` assumes a time zone offset of `00:00` - in other words, no time zone offset. For specified offset arguments, `DATETIMEOFFSETFROMPARTS` expects values for both arguments, and both values positive or negative. If _minute\_offset_ has a value and _hour\_offset_ has no value, `DATETIMEOFFSETFROMPARTS` will raise an error. `DATETIMEOFFSETFROMPARTS` will raise an error if the other arguments have invalid values. If at least one required arguments have a `NULL` value, then `DATETIMEOFFSETFROMPARTS` will return `NULL`. However, if the _precision_ argument has a `NULL` value, then `DATETIMEOFFSETFROMPARTS` will raise an error.

The _fractions_ argument depends on the precision argument. For example, for a precision value of 7, each fraction represents 100 nanoseconds; for a precision of 3, each fraction represents a millisecond. For a precision value of zero, the value of fractions must also be zero; otherwise, `DATETIMEOFFSETFROMPARTS` will raise an error.

#### Example 1: Without fractions of a second

```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;
```

#### Example 2: With fractions of a second

This example shows the use of the _fractions_ and _precision_ parameters:

1. When _fractions_ have a value of 5, and _precision_ has a value of 1, the value of _fractions_ represents 5/10 of a second.
2. When _fractions_ have a value of 50, and _precision_ has a value of 2, the value of _fractions_ represents 50/100 of a second.
3. When _fractions_ have a value of 500, and _precision_ has a value of 3, then the value of _fractions_ represents 500/1000 of a second.

```sql
SELECT DATETIMEOFFSETFROMPARTS( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 )
```

## SMALLDATETIMEFROMPARTS

SMALLDATETIMEFROMPARTS returns a **smalldatetime** value for the specified date and time.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
SMALLDATETIMEFROMPARTS ( year, month, day, hour, minute )
```

### Arguments

`year`\
Integer expression specifying a year.

`month`\
Integer expression specifying a month.

`day`\
Integer expression specifying a day.

`hour`\
Integer expression specifying hours.

`minute`\
Integer expression specifying minutes.

### Return types

```sql
smalldatetime
```

### Remarks

This function acts as a constructor for a fully initialized **smalldatetime** value. If the arguments aren't valid, then an error is thrown. If required arguments are null, then null is returned.

#### Example

```sql
SELECT SMALLDATETIMEFROMPARTS( 2010, 12, 31, 23, 59 ) AS Result
```

## TIMEFROMPARTS

TIMEFROMPARTS returns a **time** value for the specified time and with the specified precision.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform. Please check back at a later time.
You can review the full list of in-progress function translations[ on the CQL functions master list page](../../cql-functions-master-list.md).
{% endhint %}

### Syntax

```sql
TIMEFROMPARTS ( hour, minute, seconds, fractions, precision )
```

### Arguments

`hour`\
Integer expression specifying hours.

`minute`\
Integer expression specifying minutes.

`seconds`\
Integer expression specifying seconds.

`fractions`\
Integer expression specifying fractions.

`precision`\
Integer literal specifying the precision of the **time** value to be returned.

### Return types

```sql
time( precision )
```

#### Remarks

TIMEROMPARTS returns a fully initialized time value. If the arguments are invalid, then an error is raised. If any of the parameters are null, null is returned. However, if the _precision_ argument is null, then an error is raised.

The _fractions_ argument depends on the _precision_ argument. For example, if _precision_ is 7, then each fraction represents 100 nanoseconds; if _precision_ is 3, then each fraction represents a millisecond. If the value of _precision_ is zero, then the value of _fractions_ must also be zero; otherwise, an error is raised.

#### Example 1: Without fractions of a second

```sql
SELECT TIMEFROMPARTS( 23, 59, 59, 0, 0 ) AS Result
```

#### Example 2: With fractions of a second

The following example demonstrates the use of the _fractions_ and _precision_ parameters:

1. When _fractions_ have a value of 5 and _precision_ has a value of 1, then the value of _fractions_ represents 5/10 of a second.
2. When _fractions_ have a value of 50 and _precision_ has a value of 2, then the value of _fractions_ represents 50/100 of a second.
3. When _fractions_ have a value of 500 and _precision_ has a value of 3, then the value of _fractions_ represents 500/1000 of a second.

```sql
SELECT TIMEFROMPARTS ( 14, 23, 44, 5, 1 )
```
