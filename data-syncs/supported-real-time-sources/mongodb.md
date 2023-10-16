# MongoDB

## Overview

The MongoDB stream source works similar to Cinchy's Change Data Capture functionality. The listener subscribes to monitor the change stream of a specific collection in the database of the MongoDB server. Any actions performed on document(s) inside of that collection are picked up by the listener and sent to the queue.

### Limitations

In order to use change streams in MongoDB, there are a few requirements your environment must meet.

* The database must be in a [replica set](https://docs.mongodb.com/manual/replication/) or [sharded cluster](https://docs.mongodb.com/manual/sharding/).
* The database must use the [WiredTiger](https://docs.mongodb.com/manual/core/wiredtiger/#storage-wiredtiger) storage engine.
* The replica set or sharded cluster must use replica set protocol [version 1](https://docs.mongodb.com/manual/reference/replica-configuration/#rsconf.protocolVersion).

## The Listener Config Table

To set up an Stream Source, you must navigate to the Listener Config table and insert a new row for your data sync _(Image 1)_. Most of the columns within the Listener Config table persist across all Stream Sources, however exceptions will be noted. You can find all of these parameters and their relevant descriptions in the tables below.

<figure><img src="../../.gitbook/assets/image (472).png" alt=""><figcaption><p>Image 1: The Listener Config table</p></figcaption></figure>

{% tabs %}
{% tab title="General Parameters" %}
The following column parameters can be found in the Listener Config table:

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> Provide a name for your Listener Config.</td><td>MongoDB Real-Time Sync</td></tr><tr><td><strong>Event Connector Type</strong></td><td><strong>Mandatory.</strong> Select your Connector type from the drop down menu.</td><td>MongoDB</td></tr><tr><td><strong>Topic</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Topic</strong> tab.</td></tr><tr><td><strong>Connection Attributes</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Connection Attributes</strong> tab.</td></tr><tr><td><strong>Status</strong></td><td><strong>Mandatory.</strong> This value refers to whether your listener config/real-time sync is turned on or off. Make sure you keep this set to Disabled until you are confident you have the rest of your data sync properly configured first.</td><td>Disabled</td></tr><tr><td><strong>Data Sync Config</strong></td><td><strong>Mandatory.</strong> This drop down will list all of the data syncs on your platform. Select the one that you want to use for your real-time sync.</td><td>MongoDB Data Sync</td></tr><tr><td><strong>Subscription Expires On</strong></td><td><strong>This value is only relevant for Salesforce Stream Sources.</strong> This field is a timestamp that is auto-populated when it has successfully subscribed to a topic. </td><td></td></tr><tr><td><strong>Message</strong></td><td><strong>Leave this value blank when setting up your configuration.</strong> This field will auto-populate during the running of your sync with any relevant messages. For instance "Cinchy listener is running", or "Listener is disabled". </td><td></td></tr><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the below steps:<br>1. Navigate to the <strong>Listener Config table.</strong><br>2. Re-configure the <strong>Auto Offset Reset value.</strong><br>3. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Disabled".</strong><br>4. Navigate to the <strong>Event Listener State table.</strong><br>5. Find the column that pertains to your data sync's Listener Config and <strong>delete it.</strong><br>6. Navigate back to the <strong>Listener Config table.</strong><br>7. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Enabled"</strong> in order for your new Auto Offset Reset configuration to take effect.</p></td><td>Latest</td></tr></tbody></table>
{% endtab %}

{% tab title="Topic" %}
The below table can be used to help create your Topic JSON needed to set up a real-time sync.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Example                                                                                                                                                                                                                                |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Database        | **Mandatory.** The name of your MongoDB [database.](https://www.mongodb.com/docs/manual/core/databases-and-collections/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Cinchy                                                                                                                                                                                                                                 |
| Collection      | **Mandatory.** The name of your MongoDB [collection.](https://www.mongodb.com/docs/manual/core/databases-and-collections/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Employee                                                                                                                                                                                                                               |
| Pipeline Stages | <p></p><p><strong>Optional.</strong> This parameter allows you to specify pipeline stages with filters.<br></p><p>In MongoDB, an aggregation pipeline consists of one or more <a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation-pipeline/#std-label-aggregation-pipeline-operator-reference">stages</a> that process documents:</p><ul><li>Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values.</li><li>The documents that are output from a stage are passed to the next stage.</li><li>An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.</li></ul> | <p>See the Example Topic JSON below.<br><br><em>Our example config uses a filter to return documents with an ID between 0 and 10,000 AND documents with the location set to Montreal, OR where the operation type is 'delete'</em></p> |

**Example Topic JSON**

```
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
{% endtab %}

{% tab title="Connection Attributes" %}
The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter        | Description                                                                                                    | Example                  |
| ---------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------ |
| connectionString | **Mandatory.** Your MongoDB [connection string.](https://www.mongodb.com/docs/guides/atlas/connection-string/) | mongodb://localhost:9877 |

```
{
	"connectionString": "mongodb://localhost:9877"

```
{% endtab %}
{% endtabs %}
