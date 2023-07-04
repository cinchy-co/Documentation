# MongoDB Collection (CDC Triggered)

## Overview

Data changes in the Cinchy Event Broker (CDC) can be used to trigger a data sync from a MongoDB data source to a specified target. The attributes of the CDC Event are available to use as parameters within the  Data Source Definition to narrow the scope of the request, e.g. a lookup.

{% hint style="success" %}
The MongoDB Collection (Cinchy Event Triggered) Source supports real-time syncs.
{% endhint %}

### Considerations

Please review the following considerations before you set up your MongoDB Collection data sync source:

* We currently only support SCRAM authentication (Mongo 4.0+).
* Syncs are column based. This means that **you must flatten the MongoDB source document** prior to sync by using a projection _(See section 2: Projection (JSON Object))_.
* The column names used in the source must match elements on the root object, except for **"$"** which can be used to retrieve the full document.
* By default, MongoDB batch size is 101.
* By default, bulk operations size is 5000.
* Due to a conversion of doubles to decimals that occurs during the sync process, minor data losses may occur.
* The following data types are not supported:
  * **Binary Data**
  * **Regular Expression**
  * **DBPointer**
  * **JavaScript**
  * **JavaScript code with scope**
  * **Symbol**
  * **Min Key**
  * **Max Key**
* The following data types are supported with conversions:
  * **ObjectID** is supported, but converted to **string**
  * **Object** is supported, but converted to **JSON**
  * **Array** is supported, but converted to **JSON**
  * **Timestamp** is supported, but converted to **64-bit integers**

## Connection definitions

The following sections in the Source configuration of the Connections experience can reference attributes of the CDC Event as parameters:

* **Connection String**
* **Database Name**
* **Collection Name**
* **Query**
* **Projection**
* **Pipeline**

{% hint style="success" %}
In Cinchy v5.6+, you can also reference attributes of the CDC Event in Calculated Columns.

**Note that syncs making use of this must limit their batch size to 1.**
{% endhint %}

Parameters use the column name or alias as defined in the CDC Event Listener Config prefixed with an "@", e.g. @CompanyName would be the parameter name for the following Cinchy CDC listener Topic configuration.

<pre class="language-json"><code class="lang-json"><strong>{
</strong>	"tableGuid": "420c1851-31ed-4ada-a71b-31659bca6f92",
	"fields": [
		{
			"column": "Cinchy Id",
                        "alias":"CinchyId"
                },
		{
			"column": "Company Name",
                        "alias":"CompanyName"
		}
	]
}
</code></pre>

{% hint style="danger" %}
Parameter names are case sensitive when used in the Connection configuration. Parameter matching is performed using literal string replacements. Names should not contain spaces (spaces are automatically removed), and should have differing prefixes.&#x20;
{% endhint %}

The following set of parameters will be available on every event even if they're not present in the listener config

* **@Version**
* **@DraftVersion**
* **@CinchyRecordType**
* **@ApprovalState**
* **@ModifiedBy**
* **@Modified**
* **@Deleted**

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                       | Example                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                    | MongoDB Cinchy Event to Cinchy |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                          |                                |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**  |                                |

<figure><img src="../../.gitbook/assets/image (675).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>MongoDB Collection (Cinchy Event Triggered)</td></tr><tr><td>Connection String</td><td><strong>Mandatory.</strong> This is the encrypted connection string.<br><br>You can review MongoDB's Connection String guide and parameter descriptions <a href="https://www.mongodb.com/docs/manual/reference/connection-string/">here.</a><br><br><mark style="color:red;"><strong>Do not include the /[database] in your connection URL.</strong></mark> By default services like MongoDB Atlas will automatically include it when copying the connection string.<br><br>If authenticating against a database <strong>other than the admin db</strong>, please provide the name of the database associated with the user’s credentials using the authSource parameter.</td><td><p><em>Example (Default):</em></p><p>mongodb+srv://&#x3C;username>:&#x3C;password>@&#x3C;mongo host URI><br></p><p><em>Example (Against different database):</em></p><p>mongodb+srv://&#x3C;username>:&#x3C;password>@&#x3C;mongo host URI>?<strong>authSource=&#x3C;authentication_db></strong></p></td></tr><tr><td>Database</td><td><strong>Mandatory.</strong> The name of the <a href="https://www.mongodb.com/docs/manual/core/databases-and-collections/">MongoDB database</a> that contains the collection listed in the "Collection" parameter.</td><td>Blog</td></tr><tr><td>Collection</td><td><strong>Mandatory.</strong> The name of your <a href="https://www.mongodb.com/docs/manual/core/databases-and-collections/">MongoDB collection.</a></td><td>Article</td></tr><tr><td>Type</td><td><strong>Mandatory.</strong> The method for retrieving your data. This will be either:<br><br><strong>- db.collection.find():</strong> This method is used to select documents in a collection when there is no need to transform (e.g. flatten or aggregate) the data. It is used for basic queries where query and projection are sufficient.<br><br><strong>- db.collection.aggregate():</strong> This method is used when there is a need to transform the data in a collection. It is used for more complex scenarios with single or multi-stages pipelines.<br><br>In general, you will yield the quickest performance by using <strong>the find method</strong>, unless you need a <a href="https://www.mongodb.com/docs/manual/aggregation/">specific aggregation operator.</a></td><td></td></tr><tr><td>Query (JSON Object)</td><td>A query for retrieving your data. This option appears if you have selected <strong>db.collection.find().</strong></td><td><a href="mongodb-collection-cinchy-event-triggered.md#example-query">Example Query</a></td></tr><tr><td>Projection (JSON Object)</td><td>This option appears if you have selected <strong>db.collection.find().</strong><br><br>Syncs are column based. This means that <strong>you must flatten the MongoDB source document</strong> prior to sync using a projection.</td><td><a href="mongodb-collection-cinchy-event-triggered.md#example-projection">Example Projection</a></td></tr><tr><td>Pipeline (JSON Array of Objects)</td><td><a href="https://www.mongodb.com/docs/manual/core/aggregation-pipeline/">An aggregation pipeline </a>consists of one or more stages that process documents.<br><br>This option appears if you have selected <strong>db.collection.aggregate().</strong></td><td></td></tr><tr><td>Use SSL</td><td>This checkbox can be used to define the use of<a href="https://sectigo.com/resource-library/what-is-x509-certificate"> x.509 certificate</a> <strong>authentication</strong> for your sync. If checked, you will need to input the following values taken from your cert:<br>- SSL Key PEM<br>- SSL Certificate PEM<br>- SSL CLA PEM</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                                                                                                                               | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | <p><strong>Mandatory.</strong> The name of your column as it appears in the source.<br><br><strong>This field is case sensitive and preserves spaces.</strong></p>                                                        | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync.                                                                                                             |         |
| Data Type   | <p><strong>Mandatory.</strong> The data type of the column values. <br><br><a href="mongodb-collection-cinchy-event-triggered.md#data-types">You can review the supported data types and their translations here.</a></p> | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                                                                                                                                         |         |



Select **Show Advanced** for more options for the Schema section.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  For Text data types, you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}
{% endtabs %}

#### Query example

```sql
// query: where "Price" is less than 10

blog> db.Articles.find({ "Price": { "$lt": 10 } })
[
  {
    _id: ObjectId("63d8137bd755fcdeed234403"),
    Name: 'Shirt',
    Price: 9.95,
    Details: { Color: 'White', Size: 'Small' },
    Stock: 61
  }
]
```

#### Projection example

```json
// Flatten the document

blog> db.Articles.find({}, { Name: 1, Price: 1, Color: "Details.Color", Size: "Details.Size", Stock: 1 })
[
  {
    _id: ObjectId("63d812afd755fcdeed234402"),
    Name: 'Shirt',
    Price: 19.95,
    Stock: 12,
    Color: 'Details.Color',
    Size: 'Details.Size'
  },
  {
    _id: ObjectId("63d8137bd755fcdeed234403"),
    Name: 'Shirt',
    Price: 9.95,
    Stock: 61,
    Color: 'Details.Color',
    Size: 'Details.Size'
  }
]
```

<figure><img src="../../.gitbook/assets/image (311).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## Next steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* To run a real-time sync (using the Cinchy Event Triggered MongoDB Source), [set up your Listener Config](../supported-real-time-sync-stream-sources/)  using the Cinchy Event Broker/CDC and enable it to begin your sync.

## Appendix A

### Data types

The MongoDB Collection Data Source obtains BSON documents from MongoDB. BSON, short for Binary JSON, is a binary-encoded serialization of JSON-like documents. Like JSON, BSON sup­ports the em­bed­ding of doc­u­ments and ar­rays&#x20;

with­in other documents and arrays. BSON also con­tains extensions that allow representation of data types that are not part of the JSON spec. For ex­ample BSON makes a distinction between Int32 and Int64.

The following table shows how MongoDB data types are translated in Cinchy.

| MongoDB        | Cinchy      | Notes       |
| -------------- | ----------- | ----------- |
| Double         | Number      | Supported   |
| String         | Text        | Supported   |
| Object         | Text (JSON) | Supported   |
| Array          | Text (JSON) | Supported   |
| Binary Data    | Binary      | Unsupported |
| ObjectId       | Text        | Supported   |
| Boolean        | Bool        | Supported   |
| Date           | Date        | Supported   |
| Null           | -           | Supported   |
| RegEx          | -           | Unsupported |
| JavaScript     | -           | Unsupported |
| Timestamp      | Number      | Supported   |
| 32-bit Integer | Number      | Supported   |
| 64-bit Integer | Number      | Supported   |
| Decimal28      | Number      | Supported   |
| Min Key        | -           | Unsupported |
| Max Key        | -           | Unsupported |
| -              | Geography   | Unsupported |
| -              | Geometry    | Unsupported |

### Listener Config

To configure a MongoDB Collection (Cinchy Event Triggered) connection, **a listener must be configured via the Listener Config table with an Event Connector Type of Cinchy CDC.**

**Review the** [**Cinchy Event Broker/CDC Listener Configuration values here**](cinchy-event-broker-cdc/)**, and then navigate to** [**the Listener Config table** ](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)**to input a new row.**

When setting up your listener configuration for your data sync, keeping the following constraints in mind:

* **Column names** in the listener config should not contain spaces. If they do, they will be automatically removed. _E.g. a column named Company Name will become the replacement  parameter @CompanyName_
* The replacement parameter names are **case sensitive.**
* **Column names** in the listener config should not be prefixes of other column names. _E.g. if you have a column called "Name", you shouldn't have another called "Name2" as the value of @Name2 may end up being replaced by the value of @Name suffixed with a "2"._

#### Example Listener Configuration

```json
{
	"tableGuid": "420c1851-31ed-4ada-a71b-31659bca6f92",
	"fields": [
		{
			"column": "Cinchy Id",
                        "alias":"CinchyId"
                },
		{
			"column": "Company Name",
                        "alias":"CompanyName"
		}
	]
}
```
