# Cinchy User Defined Functions

{% embed url="https://vimeo.com/577296512?embedded=true&owner=63499904&source=vimeo_logo" %}

## 1. Overview

User Defined Functions provide customers with a way to specify and use more particular logic in your solutions than plain CQL may allow. It can be used to simplify calculations and orchestrate automations to accommodate your business requirements.

UDFs are written in Javascript.

There are two (2) groups of Cinchy User Defined Functions (UDF's):

* [**Table-Valued Functions**](table-valued-functions.md) - Similar to the SQL construct of table-valued functions, a Cinchy User Defined Function can be SELECTED or CROSS JOINED from as if it is a table.
* [**Scalar-Valued Functions** ](scalar-valued-functions.md)- Similar to the SQL construct of scalar-valued functions. A Scalar-valued function in Cinchy is used to return a single value of any CQL data type. The function body can execute any JavaScript logic.

Cinchy UDFs run [https://github.com/sebastienros/jint](https://github.com/sebastienros/jint), which uses **ECMAScript 5.1.**

{% hint style="info" %}
If you are having issues with your script, we suggest pasting it into [https://jshint.com/](https://jshint.co), a tool that helps to detect errors and potential problems in your JavaScript code, as it also runs ECMAScript 5.1.
{% endhint %}

User Defined Functions (UDFs) are registered in the **Cinchy User Defined Functions** table _(Image 1)._

<figure><img src="../../../../.gitbook/assets/image (660).png" alt=""><figcaption><p>Image 1: UDFs Table</p></figcaption></figure>

| Column Name | Description                                                                                                                                                                                                                                                                                         |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name        | <p>The <strong>Name</strong> column contains the name of the User Defined Function.<br><br><mark style="background-color:red;">WARNING:</mark> Do not name UDFs the same names as SQL or CQL functions (for example, do not name your UDF "CONCAT"). Doing so may cause your platform to break.</p> |
| Script      | The **Script** column can contain any number of JavaScript functions that are necessary and referenced by the _single_ function that is registered in the Name column                                                                                                                               |

## 2. Structure

A user defined function in Cinchy is written in Javascript, and comes in the form of:

```javascript
function name(parameter1, parameter2, parameter3,...) {
  // code to be executed
  return something;
}
```

It can perform external API calls and execute Cinchy Queries. Generally, at least something should be returned as an indication of success or failure even if you do not want to return any values.

Helper functions can be created within a UDF, however you cannot reference other UDFs in your UDF.

## 3. Imports

To use advanced functions in UDFs, import the following.

```javascript
var helpers = importNamespace('Cinchy.UDFExtensions');
var adoNet = importNamespace('Cinchy.AdoNet');
var sysData = importNamespace('System.Data');
```

## 4. Cinchy Functions in UDFs

The following functions can be used in a Cinchy User Defined Function (but not in CQL directly).

### 4.1 External API Call

An XMLHttpRequest() helper can be used to help POST or GET data from an external API.  Note that Cinchy basicAuthAPIs can also be accessed this way.

#### Supported Methods

| Method           | Description                                                             |
| ---------------- | ----------------------------------------------------------------------- |
| Open             | This creates the HttpClient()                                           |
| setRequestHeader | This adds the header to the client                                      |
| Send             | This uses the client to call Get()                                      |
| Send (postdata)  | This uses the client to call POST, PUT, etc.                            |
| Status           | This is attributed to show the status of a client response              |
| responseText     | This is attributed to show the response text after the client is called |

```javascript
var xmlHttp = new helpers.XMLHttpRequest();
    xmlHttp.open('POST', 'yourURLhere', false);
    xmlHttp.setRequestHeader('Content-Type', 'application/json');
    xmlHttp.setRequestHeader('apikey', 'yourapikeyhere');
    xmlHttp.send(JSON.stringify(payload));
    if (xmlHttp.status === 200) {
        var response = JSON.parse(xmlHttp.responseText);
        return response.result;
    } else {
        return 'Request failed.';
    }
```

### 4.2 Cinchy Query Call

A Cinchy query or a non query (not expecting a result back) can be executed in a UDF as well.

#### Supported Methods

| Method             | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| executeNonQuery    | This is used for INSERTS, DELETES, and UPDATES. It returns a Long value. |
| executeQuery       | This is used for SELECT statement. It returns system.data values.        |
| executeBatchUpsert | This performs a batch upsert into Cinchy. It returns int values.         |

```javascript
function mainFunction(query,p1,p2) {
    var sysData = importNamespace('System.Data');
    var param = [];
    param.push(generateCinchyParam('parameterName1', sysData.DbType.String, p1.toString()));
    param.push(generateCinchyParam('parameterName2', sysData.DbType.Double, p2));

    var dataTable = [];
    dataTable = Query.executeQuery(query, param, null, null);
    
    return getSingleValue(dataTable,'colName');
}

function generateCinchyParam(name, type, value){
    var adoNet = importNamespace('Cinchy.AdoNet');
    cinchyParam = new adoNet.CinchyParameter('@' + name, type);
    cinchyParam.value = value;
    return cinchyParam;
}

function getSingleValue(dataTable,colName) {
    var sysData = importNamespace('System.Data');
    var result = '';
    var enumerator = dataTable.Rows.GetEnumerator();
    while (enumerator.MoveNext()) {
        var record = enumerator.Current;
        result = record[colName];
    }
    return result;
}
```
