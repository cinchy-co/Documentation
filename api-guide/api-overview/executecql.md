---
description: This page details how to execute CQL directly without creating a Saved Query.
---

# ExecuteCQL

You can execute CQL directly without creating a Saved Query using the below endpoint:

#### POST: https://\<Cinchy Web URL>/API/ExecuteCQL

#### Query Parameters:

<table><thead><tr><th width="197">Name</th><th width="187.4945567651633">Data Type</th><th width="308">Description</th></tr></thead><tbody><tr><td>CompressJSON</td><td>boolean</td><td>Default is true.<br>Add this parameter and set to false if you want the JSON that is returned to be expanded rather than having the schema being returned separately.</td></tr><tr><td>ResultFormat</td><td>string</td><td><p>XML </p><p>JSON</p><p>CSV</p><p>TSV</p><p>PSV</p><p>PROTOBUF</p></td></tr><tr><td>Type</td><td>string</td><td>QUERY<br><em>- Query (Approved Data Only)</em><br>DRAFT_QUERY<br><em>- Query (Include Draft Changes)</em><br>SCALAR<br><em>- Scalar</em><br>NONQUERY<br><em>- Non Query, such as an insert or delete</em><br>VERSION_HISTORY_QUERY<br><em>- Query (Include Version History)</em></td></tr><tr><td>ConnectionId</td><td>string</td><td></td></tr><tr><td>TransactionId</td><td>string</td><td>When one or more requests share the same TransactionId, they are considered to be within the scope of a single transaction.</td></tr><tr><td>Query</td><td>string</td><td>The CQL query statement to execute</td></tr><tr><td>Parameters</td><td>boolean</td><td>See below on format for the parameters.</td></tr><tr><td>SchemaOnly</td><td>integer</td><td>Defaults to false.</td></tr><tr><td>StartRow</td><td>integer</td><td>When implementing pagination, specify a starting offset. Combine with <code>RowCount</code> to set the size of the data window.</td></tr><tr><td>RowCount</td><td>integer</td><td>When implementing pagination, specify the number of rows to retrieve for the current page. Combine with <code>StartRow</code> to set the paging position.<br></td></tr><tr><td>CommandTimeout</td><td>string</td><td>Use this parameter to override the default timeout (30s) for long running queries. In seconds.</td></tr><tr><td>UserId</td><td>string</td><td>The ID of a user with authorization to run the saved query.</td></tr></tbody></table>

{% hint style="info" %}
StartRow and RowCount work with API Saved Queries. When using these parameters with ExecuteCQL, the ResultFormat needs to be set to **DELIMITED\_FILE** and an additional Delimiter parameter needs to be added. The delimiter could be a character of your choosing like "," or "|"
{% endhint %}

#### Header Parameters:

| Name          | Data Type | Description                                                                                                 |
| ------------- | --------- | ----------------------------------------------------------------------------------------------------------- |
| Authorization | string    | <p>Bearer &#x3C;access_token></p><p>See <a href="api-authentication.md">Authentication </a>for details.</p> |

#### Responses:

* <mark style="color:green;">200 (OK)</mark>

## Parameters <a href="#parameters" id="parameters"></a>

To pass in parameters in your executeCQL, you will need to pass in sets of parameters in the following format. So if you have one parameter then you would pass in 3 query parameters beginning with `Parameters[0].` , and if you have a second parameter you would include an additional 3 query parameters beginning with `Parameters[1].` .

| Query String Parameter Name       | Content                                                                                                         |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Parameters\[n].ParameterName      | <p>Name of the parameter that is in your query, including the '@'.</p><p>Ex. <code>@name</code></p>             |
| Parameters\[n].XmlSerializedValue | <p>XML Serialized version of the value of that parameter.</p><p>Ex. <code>&#x26;quot;test&#x26;quot;</code></p> |
| Parameters\[n].ValueType          | <p>Datatype of the value.</p><p>Ex. <code>System.String</code></p>                                              |
