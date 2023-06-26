# Validate Date and Time Values

## 1. Overview

The validate date and time value function covered in this section is:

* [​ISDATE](validate-date-and-time-values.md#isdate-transact-sql)​

## ISDATE <a href="#isdate-transact-sql" id="isdate-transact-sql"></a>

ISDATE checks an expression to see if it is correct.&#x20;

It will return 1 if the _expression_ is a valid **date**, **time**, or **datetime** value; otherwise, it will return 0. ISDATE will also return 0 if the _expression_ is a **datetime2** value.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.&#x20;

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../../cql-functions-master-list.md).
{% endhint %}

#### Syntax&#x20;

```sql
ISDATE ( expression )
```

#### Arguments

`expression`\
Is a character string or expression that can be converted to a character string. The expression must be less than 4,000 characters. Date and time data types, except datetime and smalldatetime, are not allowed as the argument for ISDATE.

#### Return Types&#x20;

```sql
int
```

#### Remarks&#x20;

ISDATE is deterministic only when used with the CONVERT function, if the CONVERT style parameter is specified, and style is not equal to 0, 100, 9, or 109.

The return value of ISDATE depends on the settings set by SET DATEFORMAT, SET LANGUAGE and Configure the default language Server Configuration Option.

#### Example

Using ISDATE to Test Valid datetime Expression

```sql
IF ISDATE('2009-05-12 10:19:41.177') = 1
     SELECT 'VALID'
ELSE
     SELECT 'INVALID'
```
