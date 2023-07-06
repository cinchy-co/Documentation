# String Functions

## Overview

The string functions covered in this section perform operations on a string (char or varchar) input value and return a string or numeric value.

<table data-header-hidden><thead><tr><th width="155"></th><th width="134"></th><th></th><th></th></tr></thead><tbody><tr><td>​</td><td>​</td><td>​</td><td>​</td></tr><tr><td><a href="string-functions.md#ascii-transact-sql">ASCII​</a></td><td><a href="string-functions.md#char-transact-sql">​CHAR​</a></td><td><a href="string-functions.md#charindex-transact-sql">​CHARINDEX​</a></td><td><a href="string-functions.md#concat-transact-sql">​</a><a href="string-functions.md#concat-transact-sql">CONCAT​</a></td></tr><tr><td><a href="string-functions.md#difference-transact-sql">DIFFERENCE​</a></td><td><a href="string-functions.md#format-transact-sql">FORMAT​</a></td><td><a href="string-functions.md#left-transact-sql">LEFT​</a></td><td><a href="string-functions.md#len-transact-sql">​LEN​</a></td></tr><tr><td><a href="string-functions.md#lower-transact-sql">LOWER​</a></td><td><a href="string-functions.md#ltrim-transact-sql">LTRIM​</a></td><td>​<a href="string-functions.md#patindex-transact-sql">PATINDEX​</a></td><td><a href="string-functions.md#replace-transact-sql">REPLACE​</a></td></tr><tr><td>​<a href="string-functions.md#reverse-transact-sql">REVERSE​</a></td><td>​<a href="string-functions.md#right">RIGHT​</a></td><td>​<a href="string-functions.md#rtrim-transact-sql">RTRIM​</a></td><td><a href="string-functions.md#soundex-transact-sql">​SOUNDEX​</a></td></tr><tr><td><a href="string-functions.md#space-transact-sql">​SPACE​</a></td><td><a href="string-functions.md#str-transact-sql">STR​</a></td><td><a href="string-functions.md#string_split-transact-sql">​STUFF​</a></td><td><a href="string-functions.md#substring-transact-sql">SUBSTRING​</a></td></tr><tr><td><a href="string-functions.md#upper">UPPER​</a></td><td>​</td><td>​</td><td>​</td></tr></tbody></table>

## ASCII <a href="#ascii-transact-sql" id="ascii-transact-sql"></a>

ASCII (**A**merican **S**tandard **C**ode for **I**nformation **I**nterchange) returns the ASCII code value of the leftmost character of a character expression.‌

#### Syntax <a href="#syntax" id="syntax"></a>

```sql
ASCII (character_expression) return_type int
```

#### Example <a href="#examples" id="examples"></a>

```sql
SELECT ASCII('A') SELECT ASCII(1)
```

## CHAR  <a href="#char-transact-sql" id="char-transact-sql"></a>

This function converts an **int** between 0 to 255 to a character value. Outside of this range, the CHAR function will return a NULL value.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-1" id="syntax-1"></a>

```sql
CHAR (integer_expression) return_type char(1)
```

#### Arguments <a href="#arguments" id="arguments"></a>

_`integer_expression`_

An integer from 0 through 255.

#### Return Types <a href="#return-types" id="return-types"></a>

char(1)

#### Example <a href="#examples-1" id="examples-1"></a>

```sql
SELECT CHAR(65)
SELECT CHAR(100)
```

## CHARINDEX  <a href="#charindex-transact-sql" id="charindex-transact-sql"></a>

This function searches for one character expression inside another character string. If found, the function will return the starting position of the first expression.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-2" id="syntax-2"></a>

```sql
CHARINDEX ( expressionToFind , expressionString [ , start_location ] )
```

#### Arguments <a href="#arguments-1" id="arguments-1"></a>

_`expressionToFind`_\
The expression that needs to be found in the ExpressionString

_`expressionString`_\
The string that contains the expression

_`start_Location`_\
Start location from where the search will start

If CHARINDEX does not find _expressionToFind_ within _expressionString_, CHARINDEX will return 0.

#### **Example 1**

Returning the starting position of an expression‌

This example searches for `a` in the string value.

```sql
SELECT CHARINDEX('a', 'this is a beautiful day');
```

#### **Example 2**

Returning the starting position of an expression with an optional start location‌

This example searches for `a` in the string value starting from 15th position.

```sql
SELECT CHARINDEX('a', 'this is a beautiful day', 15);
```

## CONCAT <a href="#concat-transact-sql" id="concat-transact-sql"></a>

The CONCAT function concatenates two or more string values one after the other. This function requires at least 2 strings and no more than 254 strings to concatenate.‌

#### Syntax <a href="#syntax-3" id="syntax-3"></a>

```sql
CONCAT ( string1, string2 [, stringN ] 
```

#### Arguments <a href="#arguments-2" id="arguments-2"></a>

_`string`_\
A string to concatenate to the other strings.

#### Return Types <a href="#return-types-1" id="return-types-1"></a>

_`string`_\
A string with all the concatenated strings.

#### Example <a href="#example" id="example"></a>

```sql
SELECT CONCAT ( 'Happy ', 'Birthday ', 11, '/', '25' ) AS Result;
```

## DIFFERENCE  <a href="#difference-transact-sql" id="difference-transact-sql"></a>

This function returns an integer value measuring the difference between the SOUNDEX () values of two different character expressions.strings.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-4" id="syntax-4"></a>

```sql
DIFFERENCE ( string , string )
```

#### Arguments <a href="#arguments-3" id="arguments-3"></a>

_`string`_

An alphanumeric expression of character data. _string_ can be a constant, variable, or column.‌

#### Return Types <a href="#return-types-2" id="return-types-2"></a>

int

#### Example <a href="#examplesexample" id="examplesexample"></a>

```sql
SELECT SOUNDEX('day'), SOUNDEX('monday'), DIFFERENCE('day', 'monday');
```

## FORMAT  <a href="#format-transact-sql" id="format-transact-sql"></a>

Returns a value formatted with the specified format. Use the FORMAT function for locale-aware formatting of date/time and number values as strings. For general data type conversions, use CAST or CONVERT.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-5" id="syntax-5"></a>

```sql
FORMAT ( value, format )
```

#### Arguments <a href="#arguments-4" id="arguments-4"></a>

_`value`_

Expression of a supported data type to format. For a list of valid types, see the table in the following Remarks section.‌

_`format`_

**nvarchar** format pattern.‌

The _format_ argument must contain a valid .NET Framework format string, either as a standard format string (for example, "C" or "D"), or as a pattern of custom characters for dates and numeric values (for example, "MMMM DD, yyyy (dddd)").

#### Return Types <a href="#return-types-3" id="return-types-3"></a>

nvarchar or null‌

The length of the return value is determined by the _format_.‌

The following table lists the acceptable data types for the _value_ argument together with their .NET Framework mapping equivalent types.

| Category      | Type           | .NET type      |
| ------------- | -------------- | -------------- |
| Numeric       | bigint         | Int64          |
| Numeric       | int            | Int32          |
| Numeric       | smallint       | Int16          |
| Numeric       | tinyint        | Byte           |
| Numeric       | decimal        | SqlDecimal     |
| Numeric       | numeric        | SqlDecimal     |
| Numeric       | float          | Double         |
| Numeric       | real           | Single         |
| Numeric       | smallmoney     | Decimal        |
| Numeric       | money          | Decimal        |
| Date and Time | date           | DateTime       |
| Date and Time | time           | TimeSpan       |
| Date and Time | datetime       | DateTime       |
| Date and Time | smalldatetime  | DateTime       |
| Date and Time | datetime2      | DateTime       |
| Date and Time | datetimeoffset | DateTimeOffset |

#### Example 1 <a href="#examples-3" id="examples-3"></a>

FORMAT with  strings‌

The following example shows formatting date values by specifying a custom format.

```sql
SELECT FORMAT( GETDATE(), 'dd/MM/yyyy') AS 'DateTime Format'
```

#### **Example 2**

FORMAT with numerics

The following example shows formatting numeric values by specifying a custom format.

```sql
SELECT FORMAT(123456789,'###-##-####') AS 'Numeric Format';
```

## LEFT  <a href="#left-transact-sql" id="left-transact-sql"></a>

Returns the left part of a character string with the specified number of characters.‌

#### Syntax <a href="#syntax-6" id="syntax-6"></a>

```sql
LEFT ( string , integer )
```

#### Arguments <a href="#arguments-5" id="arguments-5"></a>

_`string`_

Is an expression of character or binary data. It can be of any data type, except **text** or **ntext**, that can be implicitly converted to **varchar** or **nvarchar**. Otherwise, use the CAST function to explicitly convert _string_.‌

_`integer`_

Is a positive integer that specifies how many characters of the _string_ will be returned.

#### Return Types <a href="#return-types-4" id="return-types-4"></a>

Returns a **string** ‌

#### Example <a href="#examples-4" id="examples-4"></a>

The following example returns the two leftmost characters from the string.

```sql
SELECT LEFT('abcdefghi, 2) FROM domain.table
```

## LEN  <a href="#len-transact-sql" id="len-transact-sql"></a>

Returns the number of characters of the specified string expression, excluding trailing spaces.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-7" id="syntax-7"></a>

```sql
LEN ( string )
```

#### Arguments <a href="#arguments-6" id="arguments-6"></a>

_`string`_

Is the string expression.‌

#### Return Types <a href="#return-types-5" id="return-types-5"></a>

**bigint** if _expression_ is of the **varchar(max)**, **nvarchar(max)** or **varbinary(max)** data types; otherwise, **int**.‌

#### Example <a href="#examples-5" id="examples-5"></a>

The following example selects the number of characters and the data in string `abcde`.

```sql
SELECT LEN('abcde')
```

## LOWER  <a href="#lower-transact-sql" id="lower-transact-sql"></a>

Returns a character expression after converting uppercase character data to lowercase.‌

#### Syntax <a href="#syntax-8" id="syntax-8"></a>

```sql
LOWER ( string )
```

#### Arguments <a href="#arguments-7" id="arguments-7"></a>

_`string`_

Is an expression of character or binary data.

#### Return Types <a href="#return-types-6" id="return-types-6"></a>

varchar or nvarchar‌

#### Example <a href="#examples-6" id="examples-6"></a>

The following example uses the `LOWER` function.

```sql
SELECT LOWER('ABCDE') AS LowerString
```

## LTRIM  <a href="#ltrim-transact-sql" id="ltrim-transact-sql"></a>

Returns a character expression after it removes leading blanks.‌

#### Syntax <a href="#syntax-9" id="syntax-9"></a>

```sql
LTRIM ( string )
```

#### Arguments <a href="#arguments-8" id="arguments-8"></a>

_`string`_

Is an expression of character or binary data.‌

#### Return Types <a href="#return-types-7" id="return-types-7"></a>

varchar or nvarchar‌

**Example: Using LTRIM**

The following example uses LTRIM to remove leading spaces from a string

```sql
SELECT LTRIM(' Remove trailing spaces.')
```

## PATINDEX  <a href="#patindex-transact-sql" id="patindex-transact-sql"></a>

Returns the starting position of the first occurrence of a pattern in a specified expression.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-11" id="syntax-11"></a>

```sql
PATINDEX ( '%pattern%' , expression )
```

#### Arguments <a href="#arguments-10" id="arguments-10"></a>

_`pattern`_

Is a character expression that contains the sequence to be found. Wildcard characters can be used; however, the % character must come before and follow _pattern._

_`expression`_

Is an expression, typically a column that is searched for the specified pattern. _expression_ is of the string data type category.‌

#### Return Types <a href="#return-types-9" id="return-types-9"></a>

**int** or **bigint**

**Example**

The following example checks a short character string (`this is a great day`) for the starting location of the characters `eat`.

```sql
SELECT PATINDEX('%eat%', 'this is a great day')
```

## REPLACE  <a href="#replace-transact-sql" id="replace-transact-sql"></a>

Replaces all occurrences of a specified string value with another string value.‌

#### Syntax <a href="#syntax-13" id="syntax-13"></a>

```sql
REPLACE ( string , string_toBeReplaced , string_replacedBy )
```

#### Arguments <a href="#arguments-12" id="arguments-12"></a>

_`string`_

Is the string expression to be searched.‌

_`string_toBeReplaced`_

Is the string to be found in the _string_.

_`string_replacedBy`_

Is the replacement string.

#### Return Types <a href="#return-types-11" id="return-types-11"></a>

Returns **nvarchar** if one of the input arguments is of the **nvarchar** data type; otherwise, REPLACE returns **varchar**.‌

#### Example <a href="#examples-11" id="examples-11"></a>

The following example replaces the string `cde` in `abcdefghi` with `xyz`.

```sql
SELECT REPLACE('abcdefghicde','cde','xyz');
GO
```

## REVERSE  <a href="#reverse-transact-sql" id="reverse-transact-sql"></a>

Returns the reverse order of a string value.‌

#### Syntax <a href="#syntax-15" id="syntax-15"></a>

```sql
REVERSE ( string )
```

#### Arguments <a href="#arguments-14" id="arguments-14"></a>

_`string`_

It is an expression of a string or binary data type.

#### Return Types <a href="#return-types-13" id="return-types-13"></a>

varchar or nvarchar‌

#### Example <a href="#examples-13" id="examples-13"></a>

The following example returns the reverse of the sting.

```sql
SELECT SELECT reverse('123456789')
```

## RIGHT <a href="#right" id="right"></a>

Returns the right part of a character string with the specified number of characters.‌

#### Syntax <a href="#syntax-16" id="syntax-16"></a>

```sql
RIGHT ( string , integer )
```

#### Arguments <a href="#arguments-15" id="arguments-15"></a>

_`string`_

Is an expression of character or binary data. ‌

_`integer`_

Is a positive integer that specifies how many characters of _string_ will be returned.

#### Return Types <a href="#return-types-14" id="return-types-14"></a>

Returns **varchar** when _character\_expression_ is a non-Unicode character data type.‌

Returns **nvarchar** when _character\_expression_ is a Unicode character data type.‌

#### Example <a href="#examples-14" id="examples-14"></a>

Using RIGHT with a string

```sql
SELECT RIGHT('Good Day', 5)
```

## RTRIM  <a href="#rtrim-transact-sql" id="rtrim-transact-sql"></a>

Returns a character string after truncating all trailing spaces.‌

#### Syntax <a href="#syntax-17" id="syntax-17"></a>

```sql
RTRIM ( string )
```

#### Arguments <a href="#arguments-16" id="arguments-16"></a>

_`string`_

Is an expression of character data. ‌

#### Return Types <a href="#return-types-15" id="return-types-15"></a>

varchar or nvarchar‌

#### Example <a href="#examples-15" id="examples-15"></a>

The following example takes a string of characters that has spaces at the end of the sentence and returns the text without the spaces at the end of the sentence.

```sql
SELECT RTRIM('Removes trailing spaces. ');
```

## SOUNDEX  <a href="#soundex-transact-sql" id="soundex-transact-sql"></a>

Returns a four-character (SOUNDEX) code to evaluate the similarity of two strings.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-18" id="syntax-18"></a>

```sql
SOUNDEX ( string )
```

#### Arguments <a href="#arguments-17" id="arguments-17"></a>

_`string`_

Is an alphanumeric expression of character data. SOUNDEX converts an alphanumeric string to a four-character code that is based on how the string sounds when spoken.

#### Return Types <a href="#return-types-16" id="return-types-16"></a>

varchar‌

#### Example <a href="#examples-16" id="examples-16"></a>

The following example shows the standard `SOUNDEX` values are returned for all consonants. Returning the `SOUNDEX` for `Raul` and `Rahul` returns the same SOUNDEX result because all vowels, the letter `y`, doubled letters, and the letter `h`, are not included.

```sql
SELECT SOUNDEX ('Raul'), SOUNDEX ('Rahul');
```

## SPACE  <a href="#space-transact-sql" id="space-transact-sql"></a>

Returns a string of repeated spaces.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-19" id="syntax-19"></a>

```sql
SPACE ( integer_expression )
```

#### Arguments <a href="#arguments-18" id="arguments-18"></a>

_`integer_expression`_

Is a positive integer that indicates the number of spaces.

#### Return Types <a href="#return-types-17" id="return-types-17"></a>

varchar‌

#### Example <a href="#examples-17" id="examples-17"></a>

The following example concatenates a comma, two spaces, and the first name of the person.

```sql
SELECT 'John' + ',' + SPACE(2) + 'Doe'
```

## STR  <a href="#str-transact-sql" id="str-transact-sql"></a>

Returns character data converted from numeric data. The character data is right-justified, with a specified length and decimal precision.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-20" id="syntax-20"></a>

```sql
STR ( float_expression [ , length [ , decimal ] ] )
```

#### Arguments <a href="#arguments-19" id="arguments-19"></a>

_`float_expression`_

Is an expression of approximate numeric (**float**) data type with a decimal point.‌

_`length`_

Is the total length. This includes decimal point, sign, digits, and spaces. The default is 10.‌

_`decimal`_

Is the number of places to the right of the decimal point. _decimal_ must be less than or equal to 16. If _decimal_ is more than 16 then the result is truncated to sixteen places to the right of the decimal point.‌

#### Return Types <a href="#return-types-18" id="return-types-18"></a>

varchar‌

#### Example <a href="#examples-18" id="examples-18"></a>

The following example converts an expression that is made up of five digits and a decimal point to a six-position character string. The fractional part of the number is rounded to one decimal place.

```sql
SELECT STR(345.67, 6, 1);
GO
```

## ‌STUFF  <a href="#string_split-transact-sql" id="string_split-transact-sql"></a>

The STUFF function inserts a string into another string. It deletes a specified length of characters in the first string at the start position and then inserts the second string into the first string at the start position.‌

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax <a href="#syntax-24" id="syntax-24"></a>

```sql
STUFF ( string , start , length , replaceWith )
```

#### Arguments <a href="#arguments-23" id="arguments-23"></a>

_`string`_

Is an expression of character data.

_`start`_

Is an integer value that specifies the location to start deletion and insertion.

_`length`_

Is an integer that specifies the number of characters to delete.

_`replaceWith`_

Is an expression of character data.

#### Return Types <a href="#return-types-22" id="return-types-22"></a>

Returns character data if _string_ is one of the supported character data types. Returns binary data if _string_ is one of the supported binary data types.‌

#### Example <a href="#examples-22" id="examples-22"></a>

The following example returns a character string created by deleting three characters from the first string, `abcdef`, starting at position `2`, at `b`, and inserting the second string at the deletion point.

```sql
SELECT STUFF('abcdef', 2, 3, 'ijklmn');
GO
```

## SUBSTRING  <a href="#substring-transact-sql" id="substring-transact-sql"></a>

Returns part of a character, binary, text, or image expression in SQL Server.‌

#### Syntax <a href="#syntax-25" id="syntax-25"></a>

```sql
SUBSTRING ( expression , start , length )
```

#### Arguments <a href="#arguments-24" id="arguments-24"></a>

_`expression`_

Is a **character**, **binary**, **text**, **ntext**, or **image** expression.‌

_`start`_

Is an integer or **bigint** expression that specifies where the returned characters start.

_`length`_

Is a positive integer or **bigint** expression that specifies how many characters of the _expression_ will be returned.

#### Return Types <a href="#return-types-23" id="return-types-23"></a>

Returns character data if _expression_ is one of the supported character data types. Returns binary data if _expression_ is one of the supported **binary** data types. The returned string is the same type as the specified expression with the exceptions shown in the table.S

**Example**‌

The following example shows how to return only a part of a character string.

```sql
SELECT SUBSTRING('Rahul', 1, 1) AS FirstCharOfName
```

## UPPER <a href="#upper" id="upper"></a>

Returns a character expression with lowercase character data converted to uppercase.‌

#### Syntax <a href="#syntax-29" id="syntax-29"></a>

```sql
UPPER ( string )
```

#### Arguments <a href="#arguments-28" id="arguments-28"></a>

_`string`_

Is an expression of character data.

#### Return Types <a href="#return-types-27" id="return-types-27"></a>

varchar or nvarchar‌

#### Examples <a href="#examples-27" id="examples-27"></a>

The following example uses the `UPPER` function to return the name in uppercase.

```sql
SELECT UPPER('rahul')
```
