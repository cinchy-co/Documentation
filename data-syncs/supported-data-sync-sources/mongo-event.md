# Mongo Event

## 1. Overview

The MongoDB Event stream source works similar to Cinchy's Change Data Capture functionality. The listener subscribes to monitor the change stream of a specific collection in the database of the MongoDB server. Any actions performed on document(s) inside of that collection are picked up by the listener and sent to the queue.

### 1.1 Limitations

In order to use change streams in MongoDB, there are a few requirements your environment must meet.

* The database must be in a [replica set](https://docs.mongodb.com/manual/replication/) or [sharded cluster](https://docs.mongodb.com/manual/sharding/).
* The database must use the [WiredTiger](https://docs.mongodb.com/manual/core/wiredtiger/#storage-wiredtiger) storage engine.
* The replica set or sharded cluster must use replica set protocol [version 1](https://docs.mongodb.com/manual/reference/replica-configuration/#rsconf.protocolVersion).

{% hint style="success" %}
The MongoDB Event source supports real-time syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                       | Example               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                    | Mongo Event to Cinchy |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                          |                       |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**  |                       |

<figure><img src="../../.gitbook/assets/image (689).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>MongoDB Event</td></tr></tbody></table>
{% endtab %}

{% tab title="Listener Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)

#### Reset Behaviour

<table><thead><tr><th width="265.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../error-logging-and-troubleshooting.md">here.</a></p></td><td>None</td></tr></tbody></table>

#### Topic JSON

The below table can be used to help create your Topic JSON needed to set up a real-time sync.

<table><thead><tr><th width="272.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td>Database</td><td><strong>Mandatory.</strong> The name of your MongoDB <a href="https://www.mongodb.com/docs/manual/core/databases-and-collections/">database.</a></td><td>Cinchy</td></tr><tr><td>Collection</td><td><strong>Mandatory.</strong> The name of your MongoDB <a href="https://www.mongodb.com/docs/manual/core/databases-and-collections/">collection.</a></td><td>Employee</td></tr><tr><td>Pipeline Stages</td><td><p></p><p><strong>Optional.</strong> This parameter allows you to specify pipeline stages with filters.<br></p><p>In MongoDB, an aggregation pipeline consists of one or more <a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/#std-label-aggregation-pipeline-operator-reference">stages</a> that process documents:</p><ul><li>Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values.</li><li>The documents that are output from a stage are passed to the next stage.</li><li>An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.</li></ul></td><td>See the Example Topic JSON below.<br><br><em>Our example config uses a filter to return documents with an ID between 0 and 10,000 AND documents with the location set to Montreal, OR where the operation type is 'delete'</em></td></tr></tbody></table>

**Example Topic JSON**

```json
{
	"database": "cinchy",
	"collection": "employee",
        "changeStreamSettings": {
		"pipelineStages": [
			"{ $match: {
                                       $or: [ 
                                           { $and: [
                                                { 'fullDocument.id': { $gt: 0, $lt: 10000 } }, 
                                                { 'fullDocument.location': 'Montreal' } ] 
                                           }, 
                                           { 'operationType': 'delete' } 
                                      ]
                                } }"
		]
	}
}
```

#### Connection Attributes

The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter        | Description                                                                                                    | Example                  |
| ---------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------ |
| connectionString | **Mandatory.** Your MongoDB [connection string.](https://www.mongodb.com/docs/guides/atlas/connection-string/) | mongodb://localhost:9877 |

```json
{
	"connectionString": "mongodb://localhost:9877"
}
```
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                                                                                                 | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | <p><strong>Mandatory.</strong> The name of your column as it appears in the source.<br><br><strong>This field is case sensitive and preserves spaces.</strong></p>                          | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync.                                                                               |         |
| Data Type   | <p><strong>Mandatory.</strong> The data type of the column values. <br><br><a href="mongo-event.md#data-types">You can review the supported data types and their translations here.</a></p> | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                                                                                                           |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement, i.e. the string that will be searched and replaced. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (731).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

</div>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../building-data-syncs/)
