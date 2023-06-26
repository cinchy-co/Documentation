# Cinchy Event Broker/CDC

## 1. Overview

The Cinchy Event Broker/CDC (Change Data Capture) source allows you to capture data changes on your table and use these events in your data syncs.

**Example Use Case:** To mitigate the labour and time costs of hosting information in a silo, as well as remove the costly integration tax plaguing your IT teams, you want to connect your legacy systems into Cinchy to take advantage of the platform's sync capabilities. To do so, you want to set up a real-time sync between a Cinchy Table and Salesforce that updates Salesforce any time data is added, updated, or deleted on the Cinchy side. As long as you enable change notifications on your Cinchy table, you can do so by setting up a data sync and listener config with your source as the Cinchy Event Broker/CDC.

{% hint style="success" %}
The Cinchy Event Broker/CDC supports both batch syncs and real-time syncs (most common).

Remember to set up your Listener Config if you are creating a real-time sync.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter  | Description                                                                                                                                                   | Example                     |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| Title      | **Mandatory.** Input a name for your data sync                                                                                                                | Cinchy People -> Salesforce |
| Version    | **Mandatory.** This is a pre-populated field containing a version number for your data sync. You can override it if you wish.                                 | 1.0.0                       |
| Parameters | **Optional.** Review our documentation on [Parameters here](../../building-data-syncs/advanced-settings/parameters.md) for more information about this field. |                             |

<figure><img src="../../../.gitbook/assets/image (642).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Cinchy Event Broker/CDC</td></tr><tr><td>Run Query</td><td><strong>Optional.</strong> If true, executes a saved query, using the Cinchy ID of the changed record as a parameter. These query results are then used as the sync source, rather than using the Cinchy table where the data change originated. Review <a href="./#appendix-a-misc.-parameters">Appendix A</a> for further details on this feature.</td><td></td></tr><tr><td>Path to Iterate</td><td><strong>Optional.</strong> For the Cinchy Event Broker/CDC, the Path to Iterate function can be used to provide the JSON path to the array of items that you want to sync (provided that your event message contains JSON values).</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The**[ **Schema** ](../../building-data-syncs/columns-and-mappings/#2.-schema-columns)**section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

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
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespac**e _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                   |         |

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

<figure><img src="../../../.gitbook/assets/image (624).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ Sync Behaviour](../../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../../building-data-syncs/#2.-create-a-data-sync-configuration).
* To run a real-time sync, [set up your Listener Config](../../supported-real-time-sync-stream-sources/) and enable it to begin your sync.

## Appendix A - Misc. Parameters

The following sections outline more information about specific parameters you can find on this source.

## Run Query

The Run Query parameter is available as an optional value for the Cinchy Event Broker/CDC connector. If set to true it executes a saved query; whichever record triggered the event becomes a parameter in that query. Thus the query now becomes the source instead of the table itself.

You are able to use any parameters defined in your listener config.

<figure><img src="../../../.gitbook/assets/image (207).png" alt=""><figcaption><p>Image 3: Run Query</p></figcaption></figure>

#### Example

In the below example, we have a data sync using the Event Broker/CDC as a source. Our Listener Config has been set with the CinchyID attribute _(Image 4)._

<figure><img src="../../../.gitbook/assets/image (608).png" alt=""><figcaption><p>Image 4: Run Query</p></figcaption></figure>

We can enable the Run Query function to use the saved query "CDC Product Ticket Datestamps" as our source instead _(Image 5)._ If we change the data from Record A in our source table to trigger our event, the Query Parameters below show that the Cinchy ID of Record A will be used in the query. This query is now our source.

<figure><img src="../../../.gitbook/assets/image (680).png" alt=""><figcaption><p>Image 5: Run Query</p></figcaption></figure>

It would appear in the data sync config XML as follows:

```xml
  <CinchyEventBrokerDataSource runQuery="true" domain="Product" name="CDC Product Ticket Datestamp" parameters="{&quot;@id&quot;: &quot;Cinchy Id&quot; }">
```
