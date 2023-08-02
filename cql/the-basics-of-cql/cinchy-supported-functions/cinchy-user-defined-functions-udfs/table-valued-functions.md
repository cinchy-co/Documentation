# Table-Valued Functions

## Overview

Similar to the SQL construct of table-valued functions, you can `SELECT` or `CROSS JOIN` from a Cinchy UDF as if it's a table.

## Use a Table Valued UDF in CQL <a href="#static-table" id="static-table"></a>

The SELECT and FROM clause work the same for a table-valued UDF as they would for a regular Cinchy table.

```sql
SELECT u.*
FROM tableUDF() u
```

## Create a Table in a UDF <a href="#static-table" id="static-table"></a>

To generate a table within a UDF for use in CQL, you need to create a data table in the same format as the default Cinchy JSON Saved Query response _(Image 1)._

```sql
function tableUDF()
{
  var result = {};
  result['schema'] = [
    {
      "columnName": "Text Column",
      "type": "String"
    },
    {
      "columnName": "Number Column",
      "type": "Double"
    },
    {
      "columnName": "Date Column",
      "type": "DateTime"
    },
    {
      "columnName": "Yes/No Column",
      "type": "Boolean"
    }
  ];
  result['data'] = [];
  result['data'].push(['Record 1',1,'01/01/2020',false]);
  result['data'].push(['Record 2',2.0,'February 2, 2020',true]);
  result['data'].push(['Record 3',-3,'Mar 03, 2020 12:00:00 AM',false]);
  return JSON.stringify(result);
}
```

<figure><img src="../../../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Image 1: Table Valued Function</p></figcaption></figure>