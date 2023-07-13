# Mathematical Functions

## Overview

Mathematical functions perform calculations based on input values provided as parameters to the functions, and return numeric values.

The mathematical functions covered in this section are:

<table data-header-hidden><thead><tr><th></th><th width="127"></th><th width="152"></th><th></th></tr></thead><tbody><tr><td></td><td>​</td><td>​</td><td>​</td></tr><tr><td>​<a href="mathematical-functions.md#abs">ABS​</a></td><td><a href="mathematical-functions.md#acos-transact-sql">ACOS</a><a href="mathematical-functions.md#acos-transact-sql">​</a>​</td><td>​<a href="mathematical-functions.md#asin-transact-sql">ASIN​</a></td><td><a href="mathematical-functions.md#atan-transact-sql">ATAN​</a></td></tr><tr><td>​<a href="mathematical-functions.md#atn2-transact-sql">ATN2​</a></td><td><a href="mathematical-functions.md#ceiling-transact-sql">​CEILING​</a></td><td>​<a href="mathematical-functions.md#ceiling-transact-sql-1">COS​</a></td><td><a href="mathematical-functions.md#cot-transact-sql">COT​</a></td></tr><tr><td>​<a href="mathematical-functions.md#degrees-transact-sql">DEGREES​</a></td><td><a href="mathematical-functions.md#exp-transact-sql">​EXP​</a></td><td><a href="mathematical-functions.md#floor-transact-sql">FLOOR​</a></td><td><a href="mathematical-functions.md#log-transact-sql">LOG​</a></td></tr><tr><td><a href="mathematical-functions.md#log10-transact-sql">LOG10​</a></td><td><a href="mathematical-functions.md#pi">​PI​</a></td><td><a href="mathematical-functions.md#power">POWER​</a></td><td><a href="mathematical-functions.md#radians-transact-sql">RADIANS​</a></td></tr><tr><td><a href="mathematical-functions.md#rand-transact-sql">RAND​</a></td><td>​<a href="mathematical-functions.md#round-transact-sql">ROUND​</a></td><td>​<a href="mathematical-functions.md#sign">SIGN​</a></td><td><a href="mathematical-functions.md#sin">SIN​</a></td></tr><tr><td><a href="mathematical-functions.md#sqrt-transact-sql">SQRT​</a></td><td><a href="mathematical-functions.md#square-transact-sql">​SQUARE​</a></td><td><a href="mathematical-functions.md#tan-transact-sql">TAN​​</a></td><td>​</td></tr></tbody></table>

## ABS

A mathematical function that returns the absolute (positive) value of the specified numeric expression. (`ABS` changes negative values to positive values. `ABS` has no effect on zero or positive values.)

#### Syntax

```sql
ABS ( numeric_expression )
```

#### Arguments

`numeric_expression`\
An expression of the exact numeric or approximate numeric data type category.

#### Return Types

Returns the same type as _numeric_expression_.

#### Example

This example shows the results of using the `ABS` function on three different numbers.

```sql
SELECT ABS(-5.2), ABS(0.0), ABS(5.2);
```

## ACOS <a href="#acos-transact-sql" id="acos-transact-sql"></a>

A function that returns the angle, in radians, whose cosine is the specified float expression. This is also called arccosine.

#### Syntax

```sql
ACOS ( float_expression )
```

#### Arguments

`float_expression`\
An expression of either type **float** or of a type that can implicitly convert to float. Only a value ranging from -1.00 to 1.00 is valid. Values outside this range return NULL and ACOS will report a domain error.

#### Return Types

float

#### Example

This example returns the `ACOS` value of the specified angle.

```sql
SELECT 'The ACOS of ' + @number + ' is: ' + CONVERT(varchar, ACOS(@number))
```

## ASIN <a href="#asin-transact-sql" id="asin-transact-sql"></a>

A function that returns the angle, in radians, whose sine is the specified **float** expression. This is also called arcsine.

#### Syntax

```sql
ASIN ( float_expression )
```

#### Arguments

`float_expression`\
An expression of either type **float** or of a type that can implicitly convert to float. Only a value ranging from -1.00 to 1.00 is valid. Values outside this range return NULL and ASIN will report a domain error.

#### Return types

float

#### Example

This example returns the `ASIN` value of the specified angle.

```sql
SELECT 'The ASIN of ' + @number + ' is: ' + CONVERT(varchar, ASIN(@number))
```

## ATAN <a href="#atan-transact-sql" id="atan-transact-sql"></a>

A function that returns the angle, in radians, whose tangent is a specified **float** expression. This is also called arctangent.

#### Syntax

```sql
ATAN ( float_expression )
```

#### Arguments

`float_expression`\
An expression of either type **float** or of a type that implicitly converts to **float**.

#### Return Types

float

#### Example

This example returns the `ATAN` value of the specified angle.

```sql
SELECT 'The ATAN of ' + @number + ' is: ' + CONVERT(varchar, ATAN(@number))
```

## ATN2 <a href="#atn2-transact-sql" id="atn2-transact-sql"></a>

Returns the angle, in radians, between the positive x-axis and the ray from the origin to the point (y, x), where x and y are the values of the two specified float expressions.

#### Syntax

```sql
ATN2 ( float_expression , float_expression )
```

#### Arguments

`float_expression`\
An expression of data type **float**.

#### Return Types

float

#### Example

The following example calculates the `ATN2` for the specified `x` and `y` components.

```sql
SELECT 'The ATAN from the x-axis to point (' + @x + ',' + @y + ') is: ' + CONVERT(varchar, ATN2(@y,@x))
```

## CEILING <a href="#ceiling-transact-sql" id="ceiling-transact-sql"></a>

This function returns the smallest integer greater than, or equal to, the specified numeric expression.

#### Syntax

```sql
CEILING ( numeric_expression )
```

#### Arguments

`numeric_expression`\
An expression of the exact numeric or approximate numeric data type category. For this function, the **bit** data type is invalid

#### Return Types

Return values have the same type as _numeric_expression_.

#### Example

This example shows positive numeric, negative numeric, and zero value inputs for the CEILING function.

```sql
SELECT CEILING(1.2), CEILING(-1.2), CEILING(0)
```

## COS <a href="#ceiling-transact-sql" id="ceiling-transact-sql"></a>

A mathematical function that returns the trigonometric cosine of the specified angle - measured in radians - in the specified expression.

#### Syntax

```sql
COS ( float_expression )
```

#### Arguments

`float_expression`\
An expression of type **float**.

#### Return Types

float

#### Example

This example returns the `COS` value of the specified angle.

```sql
SELECT 'The COS of ' + @number + ' is: ' + CONVERT(varchar, COS(@number))

```

## COT <a href="#cot-transact-sql" id="cot-transact-sql"></a>

A mathematical function that returns the trigonometric cotangent of the specified angle - in radians - in the specified **float** expression.

#### Syntax

```sql
COT ( float_expression )
```

#### Arguments

`float_expression`\
An expression of type **float**, or of a type that can implicitly convert to **float**.

#### Return Types

float

#### Example

This example returns the `COT` value for the specific angle.

```sql
SELECT 'The COT of ' + @number + ' is: ' + CONVERT(varchar, COT(@number))
```

## DEGREES <a href="#degrees-transact-sql" id="degrees-transact-sql"></a>

This function returns the corresponding angle, in degrees, for an angle specified in radians.

#### Syntax

```sql
DEGREES ( numeric_expression )
```

#### Arguments

`numeric_expression`\
An expression of the exact numeric or approximate numeric data type category, except for the **bit** data type.

#### Return Types

Returns a value whose data type matches the data type of _numeric_expression_.

#### Example

This example returns the number of degrees in a specified radian.

```sql
SELECT 'The number of degrees in ' + @radians + ' radians is: '
       + CONVERT(VARCHAR, DEGREES(@radians))
```

## EXP <a href="#exp-transact-sql" id="exp-transact-sql"></a>

Returns the exponential value of the specified **float** expression.

#### Syntax

```sql
EXP ( float_expression )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to **float**.

#### Return Types

float

#### Example

The following example uses a compounding interest example to illustrate the use of EXP.

```sql
SELECT 'With continuous compounding interest, your principal amount of $'
  + @principal + ' will turn into $'
  + CONVERT(VARCHAR,@principal * EXP(@years * CAST(@interestRate AS FLOAT)))
  +' after ' + @years + ' years at the interest rate of '
  + CONVERT(VARCHAR,CAST(@interestRate AS FLOAT) * 100) + '%'
```

## FLOOR <a href="#floor-transact-sql" id="floor-transact-sql"></a>

Returns the largest integer less than or equal to the specified numeric expression.

#### Syntax

```sql
FLOOR ( numeric_expression )
```

#### Arguments

`numeric_expression`\
Is an expression of the exact numeric or approximate numeric data type category, except for the **bit** data type.

#### Return Types

Returns the same type as _numeric_expression_.

#### Example

The following example shows positive numeric, negative numeric, and zero value inputs with the `FLOOR` function.

```sql
SELECT FLOOR(1.2), FLOOR(-1.2), FLOOR(0)
```

## LOG <a href="#log-transact-sql" id="log-transact-sql"></a>

Returns the natural logarithm of the specified **float** expression in SQL Server.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here.](https://cinchy.gitbook.io/cql/the-basics-of-cql/cql-functions-master-list)
{% endhint %}

#### Syntax

```sql
LOG ( float_expression [, base ] )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to **float**.

`base`\
Optional integer argument that sets the base for the logarithm.

**Applies to**: SQL Server 2012 (11.x) and later

#### Return Types

float

#### Remarks

By default, **LOG()** returns the natural logarithm. Starting with SQL Server 2012 (11.x), you can change the base of the logarithm to another value by using the optional _base_ parameter.

The natural logarithm is the logarithm to the base **e**, where **e** is an irrational constant approximately equal to 2.718281828.

The natural logarithm of the exponential of a number is the number itself: LOG( EXP( _n_ ) ) = _n_. And the exponential of the natural logarithm of a number is the number itself: EXP( LOG( _n_ ) ) = _n_.

#### Example

The following example calculates the `LOG` for a specified number.

```sql
SELECT 'The log base ' + @base + ' of ' + @number + ' is: '
  + CONVERT(varchar, LOG(@number,@base))

SELECT 'The log of ' + @number + ' is: ' + CONVERT(varchar, LOG(@number))
```

## LOG10 <a href="#log10-transact-sql" id="log10-transact-sql"></a>

Returns the base-10 logarithm of the specified **float** expression.

#### Syntax

```sql
LOG10 ( float_expression )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to **float**.

#### Return Types

float

#### Remarks

The LOG10 and POWER functions are inversely related to one another. For example, 10 ^ LOG10(_n_) = _n_.

#### **Example 1**

Calculating the base 10 logarithm for a variable.

The following example calculates the `LOG10` of the specified number.

```sql
SELECT 'The log base 10 of ' + @number + ' is: ' + CONVERT(varchar, LOG10(@number))
```

#### **Example 2**

Calculating the result of raising a base-10 logarithm to a specified power.

The following example returns the result of raising a base-10 logarithm to a specified power.

```sql
SELECT POWER (10, LOG10(5))
```

## PI

Returns the constant value of PI.

#### Syntax

```sql
PI ( )
```

#### Return Types

float

#### Example

The following example returns the value of `PI`.

```sql
SELECT PI()
```

## POWER

Returns the value of the specified expression to the specified power.

#### Syntax

```sql
POWER ( float_expression , y )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to **float**.

`y`\
Is the power to which to raise _float_expression_. _y_ can be an expression of the exact numeric or approximate numeric data type category, except for the **bit** data type.

#### Return Types

The return type depends on the input type of _float_expression_:

<!-- vale off -->

| Input type                          | Return type      |
| ----------------------------------- | ---------------- |
| float, real                         | float            |
| decimal(_p_, _s_)                   | decimal(38, _s_) |
| int, mallint, tinyint               | int              |
| bigint                              | bigint           |
| money, smallmoney                   | money            |
| bit, char, nchar, varchar, nvarchar | float            |

<!-- vale on -->

If the result doesn't fit in the return type, an arithmetic overflow error occurs.

#### Example

The following example demonstrates raising a specified number to a specified power.

```sql
SELECT @x + ' to the power of ' + @y + ' is: ' + CONVERT(VARCHAR, POWER(@x,@y))
```

## RADIANS <a href="#radians-transact-sql" id="radians-transact-sql"></a>

Returns radians when a numeric expression, in degrees, is entered.

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here.](https://cinchy.gitbook.io/cql/the-basics-of-cql/cql-functions-master-list)
{% endhint %}

#### Syntax

```sql
RADIANS ( numeric_expression )
```

#### Arguments

`numeric_expression`\
Is an expression of the exact numeric or approximate numeric data type category, except for the **bit** data type.

#### Return Types

Returns the same type as _numeric_expression_.

#### Example

The following example shows the number of radians based on a specified degree.

```sql
SELECT @degrees + ' degrees in radians is: ' + CONVERT(VARCHAR, RADIANS(@degrees))
```

## RAND <a href="#rand-transact-sql" id="rand-transact-sql"></a>

{% hint style="warning" %}
This function isn't currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here.](https://cinchy.gitbook.io/cql/the-basics-of-cql/cql-functions-master-list)
{% endhint %}

Returns a pseudo-random **float** value from 0 through 1, exclusive.

#### Syntax

```sql
RAND ( [ seed ] )
```

#### Arguments

`seed`\
Is an integer expression (**tinyint**, **smallint**, or **int**) that gives the seed value. If _seed_ isn't specified, the SQL Server Database Engine assigns a seed value at random. For a specified seed value, the result returned is always the same.

#### Return Types

float

#### Remarks

Repetitive calls of RAND() with the same seed value return the same results.

For one connection, if RAND() is called with a specified seed value, all subsequent calls of RAND() produce results based on the seeded RAND() call. For example, the following query will always return the same sequence of numbers.

```sql
SELECT RAND(100), RAND(), RAND()
```

#### Example

```sql
SELECT RAND(100), RAND(), RAND(5), RAND(), RAND(100), RAND()
```

## ROUND <a href="#round-transact-sql" id="round-transact-sql"></a>

Returns a numeric value, rounded to the specified length or precision.

#### Syntax

```sql
ROUND ( numeric_expression , length [ ,function ] )
```

#### Arguments

`numeric_expression`\
Is an expression of the exact numeric or approximate numeric data type category, except for the **bit** data type.

`length`\
Is the precision to which _numeric_expression_ is to be rounded. _length_ must be an expression of type **tinyint**, **smallint**, or **int**. When _length_ is a positive number, _numeric_expression_ is rounded to the number of decimal positions specified by _length_. When _length_ is a negative number, _numeric_expression_ is rounded on the left side of the decimal point, as specified by _length_.

`function`\
Is the type of operation to perform. _function_ must be **tinyint**, **smallint**, or **int**. When _function_ is omitted or has a value of 0 (default), _numeric_expression_ is rounded. When a value other than 0 is specified, _numeric_expression_ is truncated.

#### Return Types

Returns the following data types.

| Expression result                   | Return type   |
| ----------------------------------- | ------------- |
| tinyint                             | int           |
| smallint                            | int           |
| int                                 | int           |
| bigint                              | bigint        |
| decimal and numeric category (p, s) | decimal(p, s) |
| money and smallmoney category       | money         |
| float and real category             | float         |

#### Remarks

ROUND always returns a value. If _length_ is negative and larger than the number of digits before the decimal point, ROUND returns 0.

| Examples          | Results |
| ----------------- | ------- |
| ROUND(748.58, -4) | 0       |

ROUND returns a rounded _numeric_expression_, regardless of data type, when _length_ is a negative number.

| Examples                                                                                                                                             | Results                                                                                                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| ROUND(748.58, -1)                                                                                                                                    | 750.00                                                                                                   |
| ROUND(748.58, -2)                                                                                                                                    | 700.00                                                                                                   |
| ROUND(748.58, -3)                                                                                                                                    | Results in an arithmetic overflow, because 748.58 defaults to decimal(5,2), which can't return 1000.00. |
| <p>To round up to 4 digits, change the data type of the input. For example:<br><br><code>SELECT ROUND(CAST (748.58 AS decimal (6,2)),-3);</code></p> | 1000.00                                                                                                  |

#### **Example 1**

Using ROUND and estimates

The following example shows two expressions that demonstrate by using `ROUND` the last digit is always an estimate.

```sql
SELECT ROUND(123.9994, 3), ROUND(123.9995, 3)
```

#### **Example 2**

Using ROUND and rounding approximations

The following example shows rounding and approximations.

```sql
SELECT ROUND(123.4545, 2), ROUND(123.45, -2)
```

#### **Example 3**

Using ROUND to truncate

The following example uses two `SELECT` statements to demonstrate the difference between rounding and truncation. The first statement rounds the result. The second statement truncates the result.

```sql
SELECT ROUND(150.75, 0)

SELECT ROUND(150.75, 0, 1)
```

## SIGN

Returns the positive (+1), zero (0), or negative (-1) sign of the specified expression.

#### Syntax

```sql
SIGN ( numeric_expression )
```

#### Arguments

`numeric_expression`\
Is an expression of the exact numeric or approximate numeric data type category, except for the **bit** data type.

#### Return Types

| Specified expression | Return type     |
| -------------------- | --------------- |
| bigint               | bigint          |
| int/smallint/tinyint | int             |
| money/smallmoney     | money           |
| numeric/decimal      | numeric/decimal |
| Other types          | float           |

#### Example

The following example returns the SIGN values of a positive number, negative number, and zero.

```sql
SELECT SIGN(5), SIGN(-5), SIGN(0)
```

## SIN

Returns the trigonometric sine of the specified angle, in radians, and in an approximate numeric, **float**, expression.

#### Syntax

```sql
SIN ( float_expression )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to float, in radians.

#### Return Types

float

#### Example

The following example calculates the SIN for a specified angle.

```sql
SELECT 'The SIN of ' + @number + ' is: ' + CONVERT(varchar, SIN(@number))
```

## SQRT <a href="#sqrt-transact-sql" id="sqrt-transact-sql"></a>

Returns the square root of the specified float value.

#### Syntax

```sql
SQRT ( float_expression )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to float.

#### Return Types

float

#### Example

The following example returns the square root of a number.

```sql
SELECT 'The square root of ' + @number + ' is: ' + CONVERT(varchar, SQRT(@number))
```

## SQUARE <a href="#square-transact-sql" id="square-transact-sql"></a>

Returns the square of the specified float value.

#### Syntax

```sql
SQUARE ( float_expression )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to float.

#### Return Types

float

#### Example

The following example returns the square of a specified number.

```sql
SELECT @number + ' squared (to the power of 2) is: '
  + CONVERT(varchar, SQUARE(@number))
```

## TAN <a href="#tan-transact-sql" id="tan-transact-sql"></a>

Returns the tangent of the input expression.

#### Syntax

```sql
TAN ( float_expression )
```

#### Arguments

`float_expression`\
Is an expression of type **float** or of a type that can be implicitly converted to **float**, interpreted as a number of radians.

#### Return Types

float

#### Example

The following example returns the tangent of a specified angle.

```sql
SELECT 'The TAN of ' + @number + ' is: ' + CONVERT(varchar, TAN(@number))
```
