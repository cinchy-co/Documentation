# Kafka Topic

## Overview

The page outlines the parameters that should be included in your listener configuration when setting up a real time sync with a Kafka stream source. This process currently supports **JSON string or AVRO serialized format.**To listen in on multiple topics, you will need to configure multiple listener configs.

{% hint style="info" %}
To listen in on multiple topics, you will need to configure multiple listener configs.
{% endhint %}

## The Listener Config Table

To set up an Stream Source, you must  navigate to the Listener Config table and insert a new row for your data sync _(Image 1)_. Most of the columns within the Listener Config table persist across all Stream Sources, however exceptions will be noted. You can find all of these parameters and their relevant descriptions in the tables below.

<figure><img src="../../.gitbook/assets/image (472).png" alt=""><figcaption><p>Image 1: The Listener Config table</p></figcaption></figure>

{% tabs %}
{% tab title="General Parameters" %}
The following column parameters can be found in the Listener Config table:

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> Provide a name for your Listener Config.</td><td>Kafka Real-Time Sync</td></tr><tr><td><strong>Event Connector Type</strong></td><td><strong>Mandatory.</strong> Select your Connector type from the drop down menu.</td><td>Kafka Topic</td></tr><tr><td><strong>Topic</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Topic</strong> tab.</td></tr><tr><td><strong>Connection Attributes</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Connection Attributes</strong> tab.</td></tr><tr><td><strong>Status</strong></td><td><strong>Mandatory.</strong> This value refers to whether your listener config/real-time sync is turned on or off. Make sure you keep this set to Disabled until you are confident you have the rest of your data sync properly configured first.</td><td>Disabled</td></tr><tr><td><strong>Data Sync Config</strong></td><td><strong>Mandatory.</strong> This drop down will list all of the data syncs on your platform. Select the one that you want to use for your real-time sync.</td><td>Kafka Data Sync</td></tr><tr><td><strong>Subscription Expires On</strong></td><td><strong>This value is only relevant for Salesforce Stream Sources.</strong> This field is a timestamp that is auto-populated when it has successfully subscribed to a topic. </td><td></td></tr><tr><td><strong>Message</strong></td><td><strong>Leave this value blank when setting up your configuration.</strong> This field will auto-populate during the running of your sync with any relevant messages. For instance "Cinchy listener is running", or "Listener is disabled". </td><td></td></tr><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the below steps:<br>1. Navigate to the <strong>Listener Config table.</strong><br>2. Re-configure the <strong>Auto Offset Reset value.</strong><br>3. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Disabled".</strong><br>4. Navigate to the <strong>Event Listener State table.</strong><br>5. Find the column that pertains to your data sync's Listener Config and <strong>delete it.</strong><br>6. Navigate back to the <strong>Listener Config table.</strong><br>7. Set the <strong>"Status"</strong> column of the Listener Config to <strong>"Enabled"</strong> in order for your new Auto Offset Reset configuration to take effect.</p></td><td>Latest</td></tr></tbody></table>
{% endtab %}

{% tab title="Topic" %}
The below table can be used to help create your Topic JSON needed to set up a real-time sync.

| Parameter     | Description                                                                                                                                                           | Example |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| topicName     | **Mandatory.** This is the Kafka topic name to listen messages on.                                                                                                    |         |
| messageFormat | **Optional.** Put **"AVRO"** if your messages are [serialized in AVRO](../supported-data-sync-sources/kafka-topic/apache-avro-data-format.md), otherwise leave blank. |         |

**Example Topic JSON**

```
{
  "topicName": "<(mandatory) kafka topic name to listen messages on>",
  "messageFormat": "<(optional) Put "AVRO" if the messages are serialized in AVRO>"
}
```
{% endtab %}

{% tab title="Connection Attributes" %}
The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bootstrapServers           | List the Kafka bootstrap servers in a comma-separated list. This should be in the form of **host:port**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| saslMechanism              | <p>This will be either <strong>PLAIN, SCRAM-SHA-256, or SCRAM-SHA-512.</strong><br><br>SCRAM-SHA-256 must be formatted as: SCRAMSHA256<br><br>SCRAM-SHA-512 must be formatted as: SCRAMSHA512</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| saslPassword               | The password for your chosen SASL mechanism                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| saslUsername               | The username for your chosen SASL mechanism.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| url                        | **This is required if your data follows a schema** [**when serialized in AVRO.** ](../supported-data-sync-sources/kafka-topic/apache-avro-data-format.md)It is a comma-separated list of URLs for schema registry instances that are used to register or lookup schemas.                                                                                                                                                                                                                                                                                                                                                                                               |
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
{% endtabs %}
