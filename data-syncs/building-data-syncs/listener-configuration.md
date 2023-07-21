# Listener Configuration

### 1. Overview <a href="#1.-listener-configs-table-columns" id="1.-listener-configs-table-columns"></a>

Setting up a Listener Configuration **is a required step** when doing a real-time data sync. You will [configure your Event Stream Source ](../supported-real-time-sync-stream-sources/)with your data sync information. You can review an example Listener Config setup [here.](broken-reference)

### 2. Setting up the Listener Config

1. Navigate to the **Listener Config table** in Cinchy _(Image 1)._

<figure><img src="../../.gitbook/assets/image (448).png" alt=""><figcaption><p>Image 1: Listener Config table</p></figcaption></figure>

2. In a new row, add in your listener config configuration data. Review [section 3](listener-configuration.md#1.-listener-configs-table-columns-1) for more information.
3. Ensure that it is set to Enabled in order for your real-time data sync to run successfully.

### 3. Listener Configs Table Columns <a href="#1.-listener-configs-table-columns" id="1.-listener-configs-table-columns"></a>

To subscribe to an event stream, make sure you have a Cinchy Listener running connected to the Cinchy instance you are on. You will need to create a record in the Listener Configs table and Enable it to subscribe to events.

The example values in this section follow the use case outlined [here.](broken-reference)

Refer to the table below for information about the columns in the Listener Config.

<table><thead><tr><th width="192">Column</th><th width="249.33333333333331">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td>The name of your Listener Config</td><td>New Hire Sync</td></tr><tr><td>Event Connector Type</td><td>Select from the drop-down list which event stream you are listening in on.</td><td>Cinchy CDC</td></tr><tr><td>Topic</td><td>This column expects a JSON value with certain specific information. Please review the **Topic Column** table below for details.</td><td><pre><code>{
    "tableGuid": "3daba5da-5e07-4d35-8d7c-d451a2c9068e",
    "fields": [
        {
            "column": "Name"
        },
        {
            "column": "Title"
        }
    ],
}
</code></pre></td></tr><tr><td>Connection Attributes</td><td>This field will change depending on your stream source. For this example, we are using the attribute expected for the Cinchy Event Broker/CDC stream source.</td><td>{}</td></tr><tr><td>Status</td><td>This sets where your config is <strong>Enabled or Disabled.</strong><br><br><strong>Make sure you set the value to Disabled when creating the config for the first time</strong>. This will give you a chance to correctly configure the listener before it is picked up by the service. After configuring the listener correctly you should flip the value to Enabled.</td><td>Disabled</td></tr><tr><td>Data Sync Config</td><td>The name of the Data Sync Config you created in the Connections UI or via XML.</td><td>New Hires</td></tr><tr><td>Subscription Expires On</td><td>This field is a timestamp that is populated by some Event Listeners (eg. Salesforce) service when it has successfully subscribed to a topic.</td><td></td></tr><tr><td>Auto Offset Reset</td><td>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br><br><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.</td><td><strong>Latest</strong></td></tr></tbody></table>

#### 2.1 JSON Topic Column

For the topic JSON, you need to provide the following:

| Parameter  |                                                                                                                                                                                                                                                                                                                                              |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Table GUID | The GUID of the table whose notifications you wish to consume. You can find this in the **Design Table** screen for Cinchy v5.5+, and in the **Tables table** otherwise.                                                                                                                                                                     |
| Column(s)  | <p>The names of the columns you wish to include.<br><br><strong>Note:</strong> If you will be using the <a href="https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/cinchy-event-broker">runQuery=true</a> parameter in your data sync, you only need to include the Cinchy Id in the topic JSON. </p> |
| BatchSize  | The desired result batch size. This will **default to 1** if not passed in. The maximum batch size is 1000; using a number higher than that will result in a **Bad Request** response.                                                                                                                                                       |
| Filter     | **Optional.** When CDC is enabled, you can set a filter on columns where you are capturing changes in order to receive specific data.                                                                                                                                                                                                        |

#### 2.2 Topic JSON Example

```
{
    "tableGuid": "3daba5da-5e07-4d35-8d7c-d451a2c9068e",
    "fields": [
        {
            "column": "Name"
        },
        {
            "column": "Title"
        }
    ],
}
```

### Appendix A - Additional Attributes <a href="#3.-listener-topics-additional-attributes" id="3.-listener-topics-additional-attributes"></a>

### messageKeyExpression

Each of your Event Listener message keys a message key. **By default, this key is dictated by the Cinchy ID of the record being changed.**

When the worker processes your Event Listener messages, it does so in batches, and for efficiency and to guarantee order, messages that contain the same key will not be processed in the same batch.

The **messageKeyExpression** property allows you to change the default message key to something else.

#### Possible Use Case

* Ensuring records with the same message key can be updated with the proper ordering to reflect an accurate collaboration log history.

#### Example Syntax

In the below example, we want the message key to be based on the \[Employee Id] and \[Name] column of the table that CDC is enabled on.

```
{ "messageKeyExpression": "CONCAT(New.[Employee Id], '-', New.[Name])", … }
```
