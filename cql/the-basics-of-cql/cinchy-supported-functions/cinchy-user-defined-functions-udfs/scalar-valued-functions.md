# Scalar-Valued Functions

## Overview

Similar to the SQL construct of scalar-valued functions, you can use Scalar-valued functions to return a single value of any CQL data type. The function body can execute any JavaScript logic.

## Using a Scalar-Valued UDF in CQL

Select a scalar value UDF as a column _(Image 1)._

#### Sample UDF

```javascript
function demo(param1, param2) { 
    return param1 + ' ' + param2;
}
```

#### Sample query

```sql
SELECT [Domain], [Name], demo([Domain],[Name]) as 'Full Name'
FROM [Cinchy].[Tables]
WHERE [Domain] = 'Cinchy'
```

<figure><img src="../../../../.gitbook/assets/image (622).png" alt=""><figcaption><p>Image 1: Using a Scalar-Valued UDF in CQL</p></figcaption></figure>

{% hint style="info" %}
Scalar-valued functions have to be invoked with a parameter, even if the definition of the function does not require a parameter. You can pass a string:

SELECT my\_scalar('a') FROM \[Cinchy].\[Tables] WHERE \[Deleted] IS NULL AND \[Cinchy Id]=1
{% endhint %}

## Use a Scalar-Valued UDF in a Calculated Column

Once you confirm that the UDF returns the expected result though a query, a calculated column can be include. &#x20;

### Calculations

Depending on how intensive or live you want the calculation to be, choose whether to make it a live or cached calculated column.

Simply add your UDF to the calculated column _(Image 2)._

<figure><img src="../../../../.gitbook/assets/image (419).png" alt=""><figcaption><p>Image 2: Using a Scalar-Valued UDF for Calculations</p></figcaption></figure>

### Trigger an action

To use the UDF to trigger an action (such as creating a row in another table), it should be a cached calculated column. 

#### Trigger best practices

Watch out for the following scenarios:

* Do not trigger when you do not have all the necessary fields.
* Do not trigger when non-relevant data on the row changes.
  * Make sure to appropriately insert and/or update in another table.
