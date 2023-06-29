# Kafka Topic

## Overview

[Apache Kafka ](https://kafka.apache.org/intro)is an end-to-end **event streaming platform** that:

* **Publishes** (writes) and **subscribes to** (reads) streams of events from sources like databases, cloud services, and software applications.
* **Stores** these events durably and reliably for as long as you want.
* **Processes and reacts** to the event streams in real-time and retrospectively.

Those events are organized and stored in **topics.** These topics are then partitioned over buckets located on different Kafka brokers.&#x20;

Event streaming thus ensures a continuous flow and interpretation of data so that the right information is at the right place, at the right time [for your key use cases](https://kafka.apache.org/powered-by).

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}

{% hint style="success" %}
The Kafka Topic destination supports batch and real-time syncs.
{% endhint %}

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

| Parameter         | Description                                                                                                                                                                                     | Example                          |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------|
| Destination       | Mandatory. Select your destination from the drop down menu.                                                                                                                                     | Kafka Topic                      |
| Bootstrap Servers | Mandatory. Bootstrap Servers are a list of host/port pairs to use for establishing the initial connection to the Kafka cluster.This parameter should a CSV list of "broker host" or "host:port" | localhost:9092,another.host:9092 |
| Topic Name        | Mandatory. The name of the Kafka Topic that messages will be produced to.                                                                                                                                                                                                                                                                       |
| Use SSL           | Check this if you want to connect to Kafka over SSL                                                                                                                                                                                                                                                                                             |
| SASL Mechanism    | Mandatory. Select the SASL (Simple Authentication and Security Layer) Mechanism to use for authentication:- None- PLAIN- SCRAM-SHA-256- SCRAM-SHA-512- OATHBEARER (default)- OATHBEARER (oidc)                                                                                                                                                  |
| Test Connection   | You can use the "Test Connection" button to ensure that your credentials are properly configured to access your destination. If configured correctly, a "Connection Successful" pop-up will appear. If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot. |
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping** ](../building-data-syncs/columns-and-mappings/#3.-column-mappings)**section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

\
The names you specify in your "Target Column" value will turn into attributes in a **JSON payload** that will be constructed and pushed to Kafka. The name of this target column can be whatever you choose, but we recommend maintaining your naming convention across columns for simplicity.

| Parameter     | Description                                                              | Example |
|---------------|--------------------------------------------------------------------------|---------|
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (438).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

</div>

## Next steps

* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
