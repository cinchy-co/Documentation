# Conversion Functions

## Overview

These functions support data type casting and conversion. Conversion functions convert an expression of one data type to another data type. The conversion functions covered in this section are:

- ​[CAST](conversion-functions.md#cast)​
- [​CONVERT​](conversion-functions.md#convert)

## CAST <a href="#cast" id="cast"></a>

This function is used with CONVERT to convert an expression of one data type to another.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
CAST ( expression AS data_type [ ( length ) ] )
```

#### **Arguments**

`expression`\
Any valid expression

`data_type`\
The target data type.

`length`\
An optional integer that specifies the length of the target data type, for data types that allow a user specified length.

#### **Return Types**

Returns `expression`, translated to `data_type`

#### Example

```sql
SELECT CAST(1.5 AS int)
```

## CONVERT <a href="#convert" id="convert"></a>

This function is used with CAST to convert an expression of one data type to another.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### **Example**

```sql
CONVERT ( data_type [ ( length ) ] , expression [ , style ] )
```

#### Arguments

`expression`\
Any valid expression

`data_type`\
The target data type.

`length`\
An optional integer that specifies the length of the target data type, for data types that allow a user specified length.

`style`\
An optional integer expression that specifies how the CONVERT function will translate _expression_. For a style value of NULL, NULL is returned. _data_type_ determines the range.

#### Converting datetime to character
<!-- vale off -->
| Without century | With century | Input/Output                       | Standard                       |
| --------------- | ------------ | ---------------------------------- | ------------------------------ |
| 0               | 100          | mon dd yyyy hh:miAM/PM             | Default                        |
| 1               | 101          | mm/dd/yyyy                         | US                             |
| 2               | 102          | yyyy.mm.dd                         | ANSI                           |
| 3               | 103          | dd/mm/yyyy                         | British/French                 |
| 4               | 104          | dd.mm.yyyy                         | German                         |
| 5               | 105          | dd-mm-yyyy                         | Italian                        |
| 6               | 106          | dd mon yyyy                        | -                              |
| 7               | 107          | Mon dd, yyyy                       | -                              |
| 8               | 108          | hh:mm:ss                           | -                              |
| 9               | 109          | mon dd yyyy hh:mi:ss:mmmAM (or PM) | Default + millisec             |
| 10              | 110          | mm-dd-yyyy                         | USA                            |
| 11              | 111          | yyyy/mm/dd                         | Japan                          |
| 12              | 112          | yyyymmdd                           | ISO                            |
| 13              | 113          | dd mon yyyy hh:mi:ss:mmm           | Europe (24 hour clock)         |
| 14              | 114          | hh:mi:ss:mmm                       | 24 hour clock                  |
| 20              | 120          | yyyy-mm-dd hh:mi:ss                | ODBC canonical (24 hour clock) |
| 21              | 121          | yyyy-mm-dd hh:mi:ss.mmm            | ODBC canonical (24 hour clock) |
| -               | 126          | yyyy-mm-ddThh:mi:ss.mmm            | ISO8601                        |
| -               | 127          | yyyy-mm-ddThh:mi:ss.mmmZ           | ISO8061 (with time zone Z)     |
| -               | 130          | dd mon yyyy hh:mi:ss:mmmAM         | Hijiri                         |
| -               | 131          | dd/mm/yy hh:mi:ss:mmmAM            | Hijiri                         |
<!-- vale on -->
#### Converting float to real

| Value | Explanation                |
| ----- | -------------------------- |
| 0     | Maximum 6 digits (default) |
| 1     | 8 digits                   |
| 2     | 16 digits                  |

#### Converting money to character

| Value | Explanation                                           |
| ----- | ----------------------------------------------------- |
| 0     | No comma delimiters, 2 digits to the right of decimal |
| 1     | Comma delimiters, 2 digits to the right of decimal    |
| 2     | No comma delimiters, 4 digits to the right of decimal |

#### Example

```sql
SELECT CONVERT(datetime, '2020-01-01')
```
