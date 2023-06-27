# Apache AVRO Data Format

## Introduction

{% hint style="info" %}
Apache AVRO was added as an inbound data format in [Cinchy v5.3.](https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/F1vvLbEMfTF1UqCFU9hs/\~/changes/gpj2lScIZwnF95763KiL/release-notes/release-notes/5.3-release-notes#new-connector)
{% endhint %}

[**Apache AVRO (inbound)**](https://avro.apache.org/) is a **data format** with added integration with the Kafka Schema Registry, which helps enforce data governance within a Kafka architecture.

Avro is an open source data serialization system that helps with data exchange between systems, programming languages, and processing frameworks. **Avro stores both the data definition and the data together in one message or file**. Avro stores the data definition in JSON format making it easy to read and interpret; the data itself is stored in **binary format** making it compact and efficient.

Some of the benefits for using AVRO as a data format are:

* It is compact
* It has a direct mapping to/from JSON
* It's fast
* It has bindings for a wide variety of programming languages.

For more about AVRO and Kafka, [read the documentation here. ](https://www.confluent.io/blog/avro-kafka-data/)

## Setting up the Connection to a Kafka Schema Registry

To set up the Apache AVRO connection to a Kafka Schema Registry, you will need to configure your Listener Configs table with the below specified attributes.

<figure><img src="../../../.gitbook/assets/image (568).png" alt=""><figcaption></figcaption></figure>

### Listener Config Attributes

#### Topic Column

| Name            | Description                                                        |
| --------------- | ------------------------------------------------------------------ |
| "topicName"     | **Mandatory.** This is the Kafka topic name to listen messages on. |
| "messageFormat" | Put **"AVRO"** if your messages are serialized in AVRO             |

```json
{ 
"topicName": "sampleavro", 
"messageFormat": "AVRO" 
}
```

#### Connections Attributes Column

| Name                         | Description                                                                                                                                                                                 |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "bootstrapServers"           | **Mandatory.** List the Kafka bootstrap servers in a comma-separated list. Should be in the form of **host:port**                                                                           |
| "url"                        | **This is required if your data follows a schema when serialized in AVRO.** It is a comma-separated list of URLs for schema registry instances that are used to register or lookup schemas. |
| "basicAuthCredentialsSource" | Specifies the Kafka configuration property "schema.registry.basic.auth.credentials.source" that provides the basic authentication credentials. This can be **"UserInfo" \| "SaslInherit"**  |
| "basicAuthUserInfo"          | Basic Auth credentials specified in the form of **username:password**                                                                                                                       |
| "sslKeystorePassword"        | The client keystore (PKCS#12) password                                                                                                                                                      |

```json
{
  "bootstrapServers": "host:port",
  "schemaRegistrySettings": {
    "url": "URL"
    "basicAuthCredentialsSource": "UserInfo" | "SaslInherit",
    "basicAuthUserInfo": "username:password",
    "sslKeystorePassword": "password"
  }
}
```
