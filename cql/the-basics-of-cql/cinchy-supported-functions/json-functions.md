# JSON Functions

## Overview

This page details the available JSON functions in Cinchy. The JSON functions covered in this section are:

* [​ISJSON​](json-functions.md#isjson)
* [​JSON\_VALUE ​](json-functions.md#json\_value)
* [​JSON\_QUERY​](json-functions.md#json\_query)
* [​JSON\_MODIFY​](json-functions.md#json\_modify)

{% hint style="warning" %}
These functions aren't currently available in PostGres deployments.
{% endhint %}

## ISJSON <a href="#isjson" id="isjson"></a>

This function tests whether a string contains valid JSON.

#### Syntax

```sql
ISJSON ( expression )  
```

| Argument   | Description         |
| ---------- | ------------------- |
| Expression | The string to test. |

#### Return Value

| Return Value | Description                                                |
| ------------ | ---------------------------------------------------------- |
| 1            | Returned if the input is a valid JSON object or array.     |
| 0            | Returned if the input isn't a valid JSON object of array. |
| Null         | Returned if the expression is null.                        |

#### Example 1

This example will return all rows from the **\[Expression]** column in the **\[Product].\[Function Table]** that contain valid JSON.

```sql
SELECT [Expression]
FROM [Product].[Function Table]
WHERE ISJSON = 1 
```

#### Example 2

This example would return a 1 since the expression ('true') is valid JSON.

```sql
SELECT ISJSON('true')
```

## JSON\_VALUE

This functions extracts a scalar value from a JSON string.

#### Syntax

```sql
JSON_VALUE ( expression , path )
```

#### Arguments

| Argument   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Expression | <p>An expression. Typically the name of a variable or a column that contains JSON text.<br><br>If <strong>JSON_VALUE</strong> finds JSON that's not valid in <em>expression</em> before it finds the value identified by <em>path</em>, the function returns an error. If <strong>JSON_VALUE</strong> doesn't find the value identified by <em>path</em>, it scans the entire text and returns an error if it finds JSON that's not valid anywhere in <em>expression</em>.</p> |
| Path       | <p>A <a href="https://learn.microsoft.com/en-us/sql/relational-databases/json/json-path-expressions-sql-server?view=sql-server-ver16">JSON path </a>that specifies the property to extract.<br><br>If the format of <em>path</em> isn't valid, <strong>JSON_VALUE</strong> returns an error.</p>                                                                                                                                                                                 |

#### Return Value

Returns a single text value of type nvarchar(4000). The collation of the returned value is the same as the collation of the input expression.

#### Example 1

The following example extracts the value of the JSON property into a local variable.

```sql
SET @city = JSON_VALUE(@payload, '$.properties.city.value')
```

## JSON\_QUERY

This function extracts an object or an array from a JSON string.

#### Syntax

```sql
JSON_QUERY ( expression [ , path ] )
```

#### Arguments

| Argument   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Expression | <p>An expression. Typically the name of a variable or a column that contains JSON text.<br><br>If <strong>JSON_QUERY</strong> finds JSON that's not valid in <em>expression</em> before it finds the value identified by <em>path</em>, the function returns an error. If <strong>JSON_QUERY</strong> doesn't find the value identified by <em>path</em>, it scans the entire text and returns an error if it finds JSON that's not valid anywhere in <em>expression</em>.</p>                                                                                                                                  |
| Path       | <p>A <a href="https://learn.microsoft.com/en-us/sql/relational-databases/json/json-path-expressions-sql-server?view=sql-server-ver16">JSON path </a>that specifies the property to extract.<br><br>The default value for <em>path</em> is '$'. As a result, if you don't provide a value for <em>path</em>, <strong>JSON_QUERY</strong> returns the input <em>expression</em>.<br><br></p><p>It follows zero-based indexing. Using <strong>employees[0]</strong> will return the first value in our JSON.<br></p><p>If the format of <em>path</em> isn't valid, <strong>JSON_QUERY</strong> returns an error.</p> |

#### Return Value

Returns a JSON fragment of type nvarchar(max). The collation of the returned value is the same as the collation of the input expression.

#### Example 1

```sql
DECLARE @data NVARCHAR(4000);
SET @data = N'{
"employees":
[      {
         "name":"Kevin",
         "email":"kevin@gmail.com",
         "age":42
          
}
]
}';
SELECT JSON_QUERY(@data, '$.employees[0]') AS 'Result';
```

It would return the following result:

```sql
{
         "name":"Kevin",
         "email":"kevin@gmail.com",
         "age":42
 }
```

## JSON\_MODIFY

This function updates the value of a property in a JSON string and returns the updated JSON string.

#### Syntax

```sql
JSON_MODIFY ( expression , path , newValue )
```

#### Arguments

| Argument   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Expression | <p>An expression. Typically the name of a variable or a column that contains JSON text.<br><br><strong>JSON_MODIFY</strong> returns an error if <em>expression</em> doesn't contain valid JSON.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Path       | <p>A <a href="https://learn.microsoft.com/en-us/sql/relational-databases/json/json-path-expressions-sql-server?view=sql-server-ver16">JSON path </a>that specifies the property to extract. It takes the following format:<br></p><p><code>[append] [ lax | strict ] $.<json path></code></p><ul><li><em>append</em><br>Optional modifier that specifies that the new value should be appended to the array referenced by <em><json path></em>.</li><li><em>lax</em><br>Specifies that the property referenced by <em><json path></em> does not have to exist. If the property is not present, JSON_MODIFY tries to insert the new value on the specified path. Insertion may fail if the property can't be inserted on the path. If you don't specify <em>lax</em> or <em>strict</em>, <em>lax</em> is the default mode.</li><li><em>strict</em><br>Specifies that the property referenced by <em><json path></em> must be in the JSON expression. If the property is not present, JSON_MODIFY returns an error.</li><li><em><json path></em><br>Specifies the path for the property to update. For more info, see <a href="https://learn.microsoft.com/en-us/sql/relational-databases/json/json-path-expressions-sql-server?view=sql-server-ver16">JSON Path Expressions (SQL Server)</a>.<br><br><strong>JSON_MODIFY</strong> returns an error if the format of <em>path</em> isn't valid.</li></ul> |
| newValue   | <p>The new value for the property specified by <em>path</em>.<br><br>The new value must be a [n]varchar or text.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

#### Return Value

Returns the updated value of _expression_ as properly formatted JSON text.

#### Example 1

The following example sets the surname to Smith.

```sql
SET @info=JSON_MODIFY(@info,'$.surname','Smith')
```

It would return the following formatted JSON text:

```sql
{
    "surname": "Smith"
}
```
