---
description: This page details how to execute CQL directly without creating a Saved Query.
---

# ExecuteCQL

You can execute CQL directly without creating a Saved Query using the below endpoint:

#### POST: Https://\<Cinchy Web URL>/API/ExecuteCQL

#### Query Parameters:

| Name           | Data Type | Description                                                                                                                                                                                     |
|----------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CompressJSON   | Boolean   | Default is true. Add this parameter and set to false if you want the JSON that's returned to be expanded rather than having the schema being returned separately.                               |
| ResultFormat   | string    | XML JSONCSVTSVPSVPROTOBUF                                                                                                                                                                       |
| Type           | string    | <br/> QUERY- Query (Approved Data Only) <br/> DRAFT_QUERY- Query (Include Draft Changes) <br/>SCALAR- See [Scalar functions](../../cql/the-basics-of-cql/cinchy-supported-functions/cinchy-user-defined-functions-udfs/scalar-valued-functions.md) for more inforamtion. <br/>NONQUERY- Non Query, such as an insert or delete <br/> VERSION_HISTORY_QUERY- Query (Include Version History) |
| ConnectionId   | string    |
| TransactionId  | string    | When one or more requests share the same TransactionId, they're considered to be within the scope of a single transaction.                                                                     |
| Query          | string    | The CQL query statement to execute                                                                                                                                                              |
| Parameters     | Boolean   | See below on format for the parameters.                                                                                                                                                         |
| SchemaOnly     | integer   | Defaults to false.                                                                                                                                                                              |
| StartRow       | integer   | When implementing pagination, specify a starting offset. Combine with RowCount to set the size of the data window.                                                                              |
| RowCount       | integer   | When implementing pagination, specify the number of rows to retrieve for the current page. Combine with StartRow to set the paging position.                                                    |
| CommandTimeout | string    | Use this parameter to override the default timeout (30s) for long running queries. In seconds.                                                                                                  |
| UserId         | string    | The ID of a user with authorization to run the saved query.                                                                                                                                     |

{% hint style="info" %}
StartRow and RowCount work with API Saved Queries. When using these parameters with ExecuteCQL, the ResultFormat needs to be set to **DELIMITED_FILE** and an additional Delimiter parameter needs to be added. The delimiter could be a character of your choosing like "," or "|"
{% endhint %}

#### Header Parameters:

| Name          | Data Type | Description                                                                                                 |
| ------------- | --------- | ----------------------------------------------------------------------------------------------------------- |
| Authorization | string    | <p>Bearer <access_token></p><p>See <a href="api-authentication.md">Authentication </a>for details.</p> |

#### Responses:

- <mark style="color:green;">200 (OK)</mark>

## Parameters <a href="#parameters" id="parameters"></a>

To pass in parameters in your executeCQL, you will need to pass in sets of parameters in the following format. If you have one parameter then you would pass in 3 query parameters beginning with `Parameters[0].` , and if you have a second parameter you would include an additional 3 query parameters beginning with `Parameters[1].` .

| Query String Parameter Name       | Content                                                                                                         |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Parameters\[n].ParameterName      | <p>Name of the parameter that's in your query, including the '@'.</p><p>Ex. <code>@name</code></p>             |
| Parameters\[n].XmlSerializedValue | <p>XML Serialized version of the value of that parameter.</p><p>Ex. <code>&#x26;quot;test&#x26;quot;</code></p> |
| Parameters\[n].ValueType          | <p>Datatype of the value.</p><p>Ex. <code>System.String</code></p>                                              |
