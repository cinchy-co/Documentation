# The listener configuration table

## Overview

You are able to set up your stream source configurations for real-time syncs via the Connections UI, and you can find that information on the relative [Data Sync Source page.](../supported-data-sync-sources/)

If your data sync requires more than one listener, however, you must set up additional configurations via the Listener Config table on your Cinchy platform. You can also use this table to manage and review all the configurations currently set up on your system _(Image 1)_. Most of the columns within the Listener Config table persist across all Stream Sources, however exceptions will be noted. You can find all of these parameters and their relevant descriptions in the tables below.

<figure><img src="../../.gitbook/assets/image (468).png" alt=""><figcaption><p>Image 1: The Listener Config table</p></figcaption></figure>

## Configure the listener

{% tabs %}
{% tab title="General Parameters" %}
The following column parameters can be found in the Listener Config table:

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> Provide a name for your listener config.</td><td>CDC Real-Time Sync</td></tr><tr><td><strong>Event Connector Type</strong></td><td><strong>Mandatory.</strong> Select your Connector type from the drop down menu.</td><td>Cinchy CDC</td></tr><tr><td><strong>Topic</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Topic</strong> tab.</td></tr><tr><td><strong>Connection Attributes</strong></td><td><strong>Mandatory.</strong> This field is expecting a JSON formatted value specific to the connector type you are configuring.</td><td>See the <strong>Connection Attributes</strong> tab.</td></tr><tr><td><strong>Status</strong></td><td><strong>Mandatory.</strong> This value refers to whether your listener config/real-time sync is turned on or off. Make sure you keep this set to Disabled until you are confident you have the rest of your data sync properly configured first.</td><td>Disabled</td></tr><tr><td><strong>Data Sync Config</strong></td><td><strong>Mandatory.</strong> This drop down will list all of the data syncs on your platform. Select the one that you want to use for your real-time sync.</td><td>CDC Data Sync</td></tr><tr><td><strong>Subscription Expires On</strong></td><td><strong>This value is only relevant for Salesforce Stream Sources.</strong> This field is a timestamp that is auto-populated when it has successfully subscribed to a topic. </td><td></td></tr><tr><td><strong>Message</strong></td><td><strong>Leave this value blank when setting up your configuration.</strong> This field will auto-populate during the running of your sync with any relevant messages. For instance "Cinchy listener is running", or "Listener is disabled". </td><td></td></tr><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../error-logging-and-troubleshooting.md">here.</a></p></td><td>Latest</td></tr></tbody></table>
{% endtab %}

{% tab title="Topic" %}
Information regarding setting up your Topic JSON can be found on the individual real-time sync stream source configuration pages.\
\
\- [Cinchy Event Broker/CDC](../supported-data-sync-sources/#cinchy-event-broker)\
\- [Data Polling Source](../supported-data-sync-sources/#polling-event)\
\- [Kafka Topic](../supported-data-sync-sources/#kafka-topic)\
\- [MongoDB (Event Triggered)](../supported-data-sync-sources/#mongodb-collection-cinchy-event-triggered)\
\- [Salesforce Push Topic](../supported-data-sync-sources/#salesforce-push-topic)\
\- [Salesforce Platform Event](../supported-data-sync-sources/#salesforce-platform-event)
{% endtab %}

{% tab title="Connections Attributes" %}
Information regarding setting up your Connection Attribute(s) can be found on the individual real-time sync stream source configuration pages.\
\
\- [Cinchy Event Broker/CDC](../supported-data-sync-sources/#cinchy-event-broker)\
\- [Data Polling Source](../supported-data-sync-sources/#polling-event)\
\- [Kafka Topic](../supported-data-sync-sources/#kafka-topic)\
\- [MongoDB (Event Triggered)](../supported-data-sync-sources/#mongodb-collection-cinchy-event-triggered)\
\- [Salesforce Push Topic](../supported-data-sync-sources/#salesforce-push-topic)\
\- [Salesforce Platform Event](../supported-data-sync-sources/#salesforce-platform-event)
{% endtab %}
{% endtabs %}
