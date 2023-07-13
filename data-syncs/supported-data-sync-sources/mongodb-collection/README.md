# MongoDB collection

## Overview

[MongoDB](https://www.mongodb.com/what-is-mongodb/features) is a scalable, flexible NoSQL document database platform known for its horizontal scaling and load balancing capabilities, which has given application developers an unprecedented level of flexibility and scalability.

### Considerations

Please review the following considerations before you set up your MongoDB Collection data sync source:

- We currently only support SCRAM authentication (Mongo 4.0+).
- Syncs are column based. This means that **you must flatten the MongoDB source document** prior to sync by using a projection _(See section 2: Projection (JSON Object))_.
- The column names used in the source must match elements on the root object, with the exception of **"$"** which can be used to retrieve the full document.
- By default, MongoDB batch size is 101.
- By default, bulk operations size is 5000.
- Due to a conversion of doubles to decimals that occurs during the sync process, minor data losses may occur.
- The following data types aren't supported:
  - **Binary Data**
  - **Regular Expression**
  - **DBPointer**
  - **JavaScript**
  - **JavaScript code with scope**
  - **Symbol**
  - **Min Key**
  - **Max Key**
- The following data types are supported with conversions:
  - **ObjectID** is supported, but converted to **string**
  - **Object** is supported, but converted to **JSON**
  - **Array** is supported, but converted to **JSON**
  - **Timestamp** is supported, but converted to **64-bit integers**

{% hint style="success" %}
The MongoDB Collection source supports batch syncs. (To enable real-time syncs with MongoDB, use the MongoDB Collection (Cinchy Event Triggered) or Mongo Event source instead.)
{% endhint %}

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example                      |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | MongoDB Collection to Cinchy |
| Variables   | **Optional.** Review our documentation on [Variables here ](../../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                      |                              |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |                              |

<figure><img src="../../../.gitbook/assets/image (299).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

| Parameter                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Example                                                                                                                                                                                                                           |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source                           | Mandatory. Select your source from the drop down menu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | MongoDB Collection                                                                                                                                                                                                                |
| Connection String                | Mandatory. This is the encrypted connection string. You can review MongoDB's Connection String guide and parameter descriptions here. Don't include the /[database] in your connection URL. By default services like MongoDB Atlas will automatically include it when copying the connection string. If authenticating against a database other than the admin db, please provide the name of the database associated with the user’s credentials using the `authSource` parameter.                                                                                                                                     | Example (Default):mongodb+srv://<username>:<password>@<mongo host URI>Example (Against different database):mongodb+srv://<username>:<password>@<mongo host URI>?authSource=<authentication_db> |
| Database                         | Mandatory. The name of the MongoDB database that contains the collection listed in the "Collection" parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Blog                                                                                                                                                                                                                              |
| Collection                       | Mandatory. The name of your MongoDB collection.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Article                                                                                                                                                                                                                           |
| Type                             | Mandatory. The method for retrieving your data. This will be either:- db.collection.find(): This method is used to select documents in a collection when there is no need to transform (flatten or aggregate) the data. It's used for basic queries where query and projection are sufficient.- db.collection.aggregate(): This method is used when there is a need to transform the data in a collection. It's used for more complex scenarios with single or multi-stages pipelines.In general, you will yield the quickest performance by using the find method, unless you need a specific aggregation operator. |
| Query (JSON Object)              | A query for retrieving your data. This option appears if you have selected db.collection.find().                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Example Query                                                                                                                                                                                                                     |
| Projection (JSON Object)         | This option appears if you have selected db.collection.find().Syncs are column based. This means that you must flatten the MongoDB source document prior to sync using a projection.                                                                                                                                                                                                                                                                                                                                                                                                                                 | Example Projection                                                                                                                                                                                                                |
| Pipeline (JSON Array of Objects) | An aggregation pipeline consists of one or more stages that process documents. This option appears if you have selected db.collection.aggregate().                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Use SSL                          | This checkbox can be used to define the use of x.509 certificate authentication for your sync. If checked, you will need to input the following values taken from your cert:- SSL Key PEM- SSL Certificate PEM- SSL CLA PEM                                                                                                                                                                                                                                                                                                                                                                                          |

{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                                                                                     | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | <p><strong>Mandatory.</strong> The name of your column as it appears in the source.<br><br><strong>This field is case sensitive and preserves spaces.</strong></p>              | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync.                                                                   |         |
| Data Type   | <p><strong>Mandatory.</strong> The data type of the column values. <br><br><a href="./#data-types">You can review the supported data types and their translations here.</a></p> | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                                                                                               |         |

Select **Show Advanced** for more options for the Schema section.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.** For Text data types, you can choose whether to **trim the whitespace**.\_                                                                                                                                                                                                                                                                                                                                                                           |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                       | Example |
| ----------- | --------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                       |         |

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

<figure><img src="../../../.gitbook/assets/image (694).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## Next steps

- Configure your [Destination](../../supported-data-sync-destinations/)
- Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
- Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
- To run a batch sync, select **Jobs > Start Job.**

## Appendix A

### Data types

The MongoDB Collection Data Source obtains BSON documents from MongoDB. BSON, short for Binary JSON, is a binary-encoded serialization of JSON-like documents. Like JSON, BSON sup­ports the em­bed­ding of doc­u­ments and ar­rays

with­in other documents and arrays. BSON also has extensions that allow representation of data types that aren't part of the JSON spec. For ex­ample, BSON makes a distinction between `Int32` and `Int64`.

The following table shows how MongoDB data types are translated in Cinchy.

| MongoDB        | Cinchy      | Notes       |
| -------------- | ----------- | ----------- |
| Double         | Number      | Supported   |
| String         | Text        | Supported   |
| Object         | Text (JSON) | Supported   |
| Array          | Text (JSON) | Supported   |
| Binary Data    | Binary      | Unsupported |
| ObjectId       | Text        | Supported   |
| Boolean        | Boolean     | Supported   |
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

### Retry configuration

A retry configuration will automatically retry HTTP Requests on failure based on a defined set of conditions. This capability provides a mechanism to recover from transient errors such as network disruptions or temporary service outages.

{% hint style="warning" %}
**Note: the maximum number of retries is capped at 10.**
{% endhint %}

To set up a retry specification:

1. Select "Add Retry Configuration" from the Source tab.
2. Select your Delay Strategy.

- **Linear Backoff:** Defines a delay of approximately n seconds where n = current retry attempt.
- **Exponential Backoff:** A strategy where every new retry attempt is delayed exponentially by 2^n seconds, where n = current retry attempt.
  - _Example: you defined Max Attempts = 3. Your first retry is going to be in 2^1 = 2, second: 2^2 = 4, third: 2^3 = 8 sec._

3\. Input your Max Attempts. The maximum number of retries allowed is 10.

<figure><img src="../../../.gitbook/assets/image (431).png" alt=""><figcaption></figcaption></figure>

4\. Define your Retry Conditions. You must define the conditions under which a retry should be attempted. For the Retry to trigger, **at least one** of the "Retry Conditions" has to evaluate to true.

{% hint style="info" %}
Retry conditions are only evaluated if the response code isn't 2xx Success.
{% endhint %}

Each Retry Condition contains **one or more "Attribute Match" sections**. This defines a Regex to evaluate against a section of the HTTP response. The following are the three areas of the HTTP response that can be inspected:

- Response Code
- Header
- Body

If there are multiple "Attribute Match" blocks within a Retry Condition, **all have to match for the retry condition to evaluate to true.**

{% hint style="warning" %}
The Regex value should be entered as a regular expression. The Regex engine is .NET and expressions can be tested by using [this online tool](http://regexstorm.net/tester). In the below example, the Regex is designed to match any HTTP 5xx Server Error Codes, using a Regex value of `5[0-9][0-9]`.
\
**For Headers,** the format of the Header string which the Regex is applied against is {Header Name}={Header Value}. For example, `Content-Type=application/json`.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (750).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (261).png" alt=""><figcaption></figcaption></figure>
