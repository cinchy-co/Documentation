# Kafka Topic

## 1. Overview

[Apache Kafka ](https://kafka.apache.org/intro)is an end-to-end **event streaming platform** that:

* **Publishes** (writes) and **subscribes to** (reads) streams of events from sources like databases, cloud services, and software applications.
* **Stores** these events durably and reliably for as long as you want.
* **Processes and reacts** to the event streams in real-time and retrospectively.

Those events are organized and durably stored in **topics.** These topics are then partitioned over a number of buckets located on different Kafka brokers.&#x20;

Event streaming thus ensures a continuous flow and interpretation of data so that the right information is at the right place, at the right time [for your key use cases](https://kafka.apache.org/powered-by).

Prior to setting up your data sync destination, [ensure that you've configured your Source.](../supported-data-sync-sources/)

{% hint style="success" %}
The Kafka Topic destination supports batch and real-time syncs.
{% endhint %}

## 2. Destination Tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination

* Define your[ Sync Behaviour](../building-data-syncs/sync-actions.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click Jobs > Start a Job to begin your sync.

&#x20;and how it functions.

* Define your[ Sync Behaviour](../building-data-syncs/sync-actions.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click Jobs > Start a Job to begin your sync.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Destination</td><td><strong>Mandatory.</strong> Select your destination from the drop down menu.</td><td>Kafka Topic</td></tr><tr><td>Bootstrap Servers</td><td><p><strong>Mandatory.</strong> Bootstrap Servers are a list of host/port pairs to use for establishing the initial connection to the Kafka cluster.</p><p><br>This parameter should a CSV list of <strong>"broker host" or "host:port"</strong></p></td><td>localhost:9092,another.host:9092</td></tr><tr><td>Topic Name</td><td><strong>Mandatory.</strong> The name of the Kafka Topic that messages will be produced to.</td><td></td></tr><tr><td>Use SSL</td><td>Check this if you want to connect to Kafka over SSL</td><td></td></tr><tr><td>SASL Mechanism</td><td><strong>Mandatory.</strong> Select the SASL (<em>Simple Authentication and Security Layer)</em> Mechanism to use for authentication:<br><strong>- None</strong><br><strong>- PLAIN</strong><br><strong>- SCRAM-SHA-256</strong><br><strong>- SCRAM-SHA-512</strong><br><strong>- OATHBEARER (default)</strong><br><strong>- OATHBEARER (oidc)</strong></td><td></td></tr><tr><td>Test Connection</td><td><p>You can use the "Test Connection" button to ensure that your credentials are properly configured to access your destination. </p><p></p><p>If configured correctly, a "Connection Successful" pop-up will appear.</p><p></p><p>If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot.</p></td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping** ](../building-data-syncs/columns-and-mappings/#3.-column-mappings)**section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

\
The names you specify in your "Target Column" value will turn into attributes in a **JSON payload** that will be constructed and pushed to Kafka. The name of this target column can be whatever you choose, but we recommend maintaining your naming convention across columns for simplicity.

| Parameter     | Description                                                              | Example |
| ------------- | ------------------------------------------------------------------------ | ------- |
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (438).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

</div>

## 4. Next Steps

* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
