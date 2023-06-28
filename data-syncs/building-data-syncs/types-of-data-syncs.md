---
description: This page outlines the two different types of Data Syncs available in Cinchy.
---

# Types of Data Syncs

## 1. Batch Syncs

Batch syncs work by processing a group or a ‘batch’ of data all together rather than each piece of data individually. When the data sync is triggered  it will compare the contents of the source to the target. The Cinchy Worker will decide if data needs to be added, deleted or updated. Batch sync can either be run as a one-time data load operation, or it can be scheduled to run periodically using an external Enterprise Scheduler

**Batch Sync is ideally used in situations where the results and updates don’t need to occur immediately but they can occur periodically.**

For example, a document that will only be reviewed once a month doesn’t necessarily need to be updated every single time a change is made

### 1.1 Execution Flow

<figure><img src="../../.gitbook/assets/image (494).png" alt=""><figcaption><p>Image 1: Batch sync basic execution flow</p></figcaption></figure>

At a high level, running a batch data sync operation performs these steps _(Image 1):_

1. The sync connects to Cinchy and creates a log entry in the Execution Log table with a status of running.
2. It streams the source and target into the CLI. Any malformed records or duplicate sync keys are written to source and target errors csvs (based on the temp directory)
3. It compares the sync keys to match up source and target records
4. The sync checks if there are changes between the matched records
5. For the records where there are changes, groups them into insert, update, and delete batches.
6. It sends the batches to the target, records failures in sync errors csv and Execution Errors table.
7. Once complete, it updates Execution Log entry with final status and execution output.

{% tabs %}
{% tab title="Step 1" %}
<figure><img src="../../.gitbook/assets/image (492).png" alt=""><figcaption><p>The sync connects to Cinchy and creates a log entry in the Execution Log table with a status of running.</p></figcaption></figure>
{% endtab %}

{% tab title="2" %}
<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption><p>It streams the source and target into the CLI. Any malformed records or duplicate sync keys are written to source and target errors csvs (based on the temp directory)</p></figcaption></figure>
{% endtab %}

{% tab title="3" %}
<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption><p>It compares the sync keys to match up source and target records</p></figcaption></figure>
{% endtab %}

{% tab title="4" %}
<figure><img src="../../.gitbook/assets/image (466).png" alt=""><figcaption><p>The sync checks if there are changes between the matched records</p></figcaption></figure>
{% endtab %}

{% tab title="5 " %}
<figure><img src="../../.gitbook/assets/image (500).png" alt=""><figcaption><p>For the records where there are changes, groups them into insert, update, and delete batches.</p></figcaption></figure>
{% endtab %}

{% tab title="6" %}
<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption><p>It sends the batches to the target, records failures in sync errors csv and Execution Errors table.</p></figcaption></figure>
{% endtab %}

{% tab title="7" %}
<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption><p>Once complete, it updates Execution Log entry with final status and execution output.</p></figcaption></figure>
{% endtab %}
{% endtabs %}

## 2. Real-Time Data Sync

In real-time syncs, the Cinchy Listener picks up changes in the source **immediately as they occur.** These syncs do not need to be manually triggered or scheduled using an external scheduler. Setting up a real-time sync does require an extra step of defining a listener configuration in order to execute properly.

**Real-time sync is ideally used in situations where results and responses must be immediate.**

For example, a document that is constantly checked and referred to should have the most up-to-date and recent information.

The following sources can be used in real-time syncs:

* **Cinchy Event Broker/CDC**
* **MongoDB Collection (Event Triggered)**
* **Polling Event**
* **REST API (Event Triggered)**
* **Salesforce Platform Event**

### 2.1 Execution Flow

At a high level, running a real-time data sync operation performs these steps _(Image 2):_

1. The Listener is successfully subscribed and waiting for events from streaming source
2. The Listener receives a message from a streaming source and pushes it to SQL Server Broker.
3. The Worker picks up message from SQL Server Broker
4. The Worker fetches the matching record from the target based on the sync key
5. If there are changes detected, the worker pushes them to the target system. Logs successes and failures in the worker's log file.

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption><p>Image 2: Real-time sync basic execution flow</p></figcaption></figure>
