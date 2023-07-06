# Logical Functions

## Overview

The logical functions covered in this section are:

* [​CHOOSE](logical-functions.md#choose)​
* [​IIF​](logical-functions.md#iif)

## CHOOSE <a href="#choose" id="choose"></a>

The CHOOSE function returns an item at the specified index from a list of values in Cinchy.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
CHOOSE ( index, val_1, val_2 [, val_n ] )
```

#### Arguments

`index`\
Is an integer expression that represents a 1-based index into the list of the items following it.

If the provided index value has a numeric data type other than **int**, then the value is implicitly converted to an integer. If the index value exceeds the bounds of the array of values, then CHOOSE returns null.

#### Return Types

Returns the data type with the highest precedence from the set of types passed to the function.

#### Remarks

CHOOSE acts like an index into an array, where the array is composed of the arguments that follow the index argument. The index argument determines which of the following values will be returned.

#### **Example 1**

Simple CHOOSE example

```sql
SELECT CHOOSE( 3, 'Manager', 'Director', 'Developer', 'Tester' ) AS Result
```

#### **Example 2**

Simple CHOOSE example based on column

```sql
SELECT
    [Category ID],
    CHOOSE ([Category ID], 'A', 'B', 'C', 'D', 'E') AS Expression1
FROM
    [Production].[Product Category]
```

#### **Example 3**

CHOOSE in combination with MONTH

The following example returns the season in which a user was added to Cinchy. The MONTH function is used to return the month value from the column `HireDate`.

```sql
SELECT
    [Display Name],
    [Created],
    CHOOSE(
        MONTH([Created]),
        'Winter',
        'Winter',
        'Spring',
        'Spring',
        'Spring',
        'Summer',
        'Summer',
        'Summer',
        'Autumn',
        'Autumn',
        'Autumn',
        'Winter'
    ) AS Quarter
FROM
    [Cinchy].[Users]
WHERE
    [Deleted] IS NULL
    AND YEAR([Created]) > 2005
ORDER BY
    YEAR([Created])
```

## IIF

IFF returns one of two values which is depending on if the Boolean expression evaluates TRUE or FALSE in the Cinchy.

{% hint style="warning" %}
This function is not currently supported in PostgreSQL deployments of the Cinchy platform.

New function translations are actively being worked on by the development team; please check back at a later time.

You can review the full list of in-progress function translations[ here](../cql-functions-master-list.md).
{% endhint %}

#### Syntax

```sql
IIF ( condition, true_value, false_value )
```

#### Arguments

_`boolean_expression`_\
A valid Boolean expression.

If this argument is not a Boolean expression, then a syntax error is raised.

_`true_value`_\
Value to return if _boolean\_expression_ evaluates to true.

_`false_value`_\
Value to return if _boolean\_expression_ evaluates to false.

#### Return Types

Returns the data type with the highest precedence from the types in _`true_value`_ and _`false_value`_.

#### Remarks

IIF is a second version of writing a CASE expression. It evaluates the Boolean expression which was passed as the first argument and then returns either TRUE or FALSE based on the result of the evaluation. The_`true_value`_is returned if the Boolean expression is TRUE, and the_`false_value`_is returned if the Boolean expression is FALSE or unknown.

#### Example

Return 5 if the condition is TRUE, or 10 if the condition is FALSE:

```sql
DECLARE @Value CHAR(1) = 'B'
SELECT IIF(@Value = 'A',5,10)
```
