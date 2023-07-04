# Kafka Topic

## 1. Overview

[Apache Kafka ](https://kafka.apache.org/intro)is an end-to-end **event streaming platform** that:

* **Publishes** (writes) and **subscribes to** (reads) streams of events from sources like databases, cloud services, and software applications.
* **Stores** these events durably and reliably for as long as you want.
* **Processes and reacts** to the event streams in real-time and retrospectively.

Those events are organized and durably stored in **topics.** These topics are then partitioned over a number of buckets located on different Kafka brokers.&#x20;

Event streaming thus ensures a continuous flow and interpretation of data so that the right information is at the right place, at the right time [for your key use cases](https://kafka.apache.org/powered-by).

**Example Use Case:** You currently use Kafka to store the metrics for user logins, but being stuck in the Kafka silo means that you can't easily use this data across a range of business use cases or teams. You can use a batch sync in order to liberate your data into Cinchy.

{% hint style="success" %}
The Kafka Topic source supports real-time syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                       | Example         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                    | Website Metrics |
| Variables   | **Optional.** Review our documentation on [Variables here ](../../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                       |                 |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**  |                 |

<figure><img src="../../../.gitbook/assets/image (721).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Kafka Topic</td></tr></tbody></table>
{% endtab %}

{% tab title="LIstener Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)

#### Reset Behaviour

<table><thead><tr><th width="265.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../../error-logging-and-troubleshooting.md">here.</a></p></td><td>None</td></tr></tbody></table>

#### Topic JSON

The below table can be used to help create your Topic JSON needed to set up a real-time sync.

| Parameter     | Description                                                                                                                | Example |
| ------------- | -------------------------------------------------------------------------------------------------------------------------- | ------- |
| topicName     | **Mandatory.** This is the Kafka topic name to listen messages on.                                                         |         |
| messageFormat | **Optional.** Put **"AVRO"** if your messages are [serialized in AVRO](apache-avro-data-format.md), otherwise leave blank. |         |

**Example Topic JSON**

```json
{
  "topicName": "<(mandatory) kafka topic name to listen messages on>",
  "messageFormat": "<(optional) Put "AVRO" if the messages are serialized in AVRO>"
}
```

#### Connection Attributes

The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bootstrapServers           | List the Kafka bootstrap servers in a comma-separated list. This should be in the form of **host:port**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| saslMechanism              | <p>This will be either <strong>PLAIN, SCRAM-SHA-256, or SCRAM-SHA-512.</strong><br><br>SCRAM-SHA-256 must be formatted as: SCRAMSHA256<br><br>SCRAM-SHA-512 must be formatted as: SCRAMSHA512</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| saslPassword               | The password for your chosen SASL mechanism                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| saslUsername               | The username for your chosen SASL mechanism.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| url                        | **This is required if your data follows a schema** [**when serialized in AVRO.** ](apache-avro-data-format.md)It is a comma-separated list of URLs for schema registry instances that are used to register or lookup schemas.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| basicAuthCredentialsSource | Specifies the Kafka configuration property "schema.registry.basic.auth.credentials.source" that provides the basic authentication credentials. This can be **"UserInfo" \| "SaslInherit"**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| basicAuthUserInfo          | Basic Auth credentials specified in the form of **username:password**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| sslKeystorePassword        | This is the client keystore (PKCS#12) password.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| securityProtocol           | <p>Kafka supports cluster encryption and authentication, which can encrypt data-in-transit between your applications and Kafka.</p><p></p><p>Use this field to specify which protocol will be used for communication between client and server. Cinchy currently supports the following options: <strong>Plaintext, SaslPlaintext, or SaslSsl.</strong><br><br><strong>Paintext:</strong> Unauthenticated, non-encrypted.<br><strong>SaslPlaintext</strong>: SASL-based authentication, non-encrypted.<br><strong>SaslSSL</strong>: SASL-based authentication, TLS-based encryption.<br><br><em>If no parameter is specified, this will default to Plaintext.</em></p> |

```json
{
  "bootstrapServers": "< (mandatory) kafka bootstrap servers in a comma-separated list in the form of host:port>",
  "saslMechanism": "<PLAIN|SCRAM-SHA-256|SCRAM-SHA-512>",
  "saslPassword": "",
  "saslUsername": "",
  "schemaRegistrySettings": {
    "url": "<(optional) This is required if your data follows a schema when serialized in Avro. A comma-separated list of URLs for schema registry instances that are used to register or lookup schemas. >",
   "basicAuthCredentialsSource": "<(optional) Specifies the Kafka configuration property "schema.registry.basic.auth.credentials.source" that provides the basic authentication credentials, this can be "UserInfo" | "SaslInherit">",
   "basicAuthUserInfo": "<(optional) Basic Auth credentials specified in the form of username:password>",
   "sslKeystorePassword": "<(optional) The client keystore (PKCS#12) password>"
  }
  "securityProtocol": "Plaintext | SaslPlaintext | SaslSsl"
}
```
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



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

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../../.gitbook/assets/image (686).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

</div>

## 4. Next Steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../../building-data-syncs/)
