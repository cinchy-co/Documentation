# Cinchy Event Broker/CDC

## Overview

The Cinchy Event Broker/CDC (Change Data Capture) source allows you to capture data changes on your table and use these events in your data syncs.

**Example Use Case:** To mitigate the labour and time costs of hosting information in a silo, as well as remove the costly integration tax plaguing your IT teams, you want to connect your legacy systems into Cinchy to take advantage of the platform's sync capabilities. To do so, you want to set up a real-time sync between a Cinchy Table and Salesforce that updates Salesforce any time data is added, updated, or deleted on the Cinchy side. As long as you enable change notifications on your Cinchy table, you can do so by setting up a data sync and listener config with your source as the Cinchy Event Broker/CDC.

{% hint style="success" %}
The Cinchy Event Broker/CDC supports both batch syncs and real-time syncs (most common).

Remember to set up your Listener Config if you are creating a real-time sync.
{% endhint %}

## Info tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

<table><thead><tr><th>Parameter</th><th width="236.33333333333331">Description</th><th>Example</th></tr></thead><tbody><tr><td>Title</td><td><strong>Mandatory.</strong> Input a name for your data sync</td><td>CDC</td></tr><tr><td>Variables</td><td><strong>Optional.</strong> Review our documentation on <a href="../../building-data-syncs/advanced-settings/variables.md">Variables here </a>for more information about this field.</td><td></td></tr><tr><td>Permissions</td><td>Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. <strong>Inputting at least an Admin Group is mandatory.</strong></td><td></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (688).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th width="201">Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Cinchy Event Broker/CDC</td></tr><tr><td>Run Query</td><td><strong>Optional.</strong> If true, executes a saved query, using the Cinchy ID of the changed record as a parameter. These query results are then used as the sync source, rather than using the Cinchy table where the data change originated. Review <a href="./#appendix-a-misc.-parameters">Appendix A</a> for further details on this feature.</td><td></td></tr><tr><td>Path to Iterate</td><td><strong>Optional.</strong> For the Cinchy Event Broker/CDC, the Path to Iterate function can be used to provide the JSON path to the array of items that you want to sync (provided that your event message contains JSON values).</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Listener Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)

If you are creating a CDC listener config for a **Cinchy Event Triggered REST API data source**, pay in mind the following unique constraints:

* **Column names** in the listener config should not contain spaces. If they do, they will be automatically removed. _E.g. a column named First Name will become @FirstName_
* The replacement variable names are **case sensitive.**
* **Column names** in the listener config should not be prefixes of other column names. E_.g. if you have a column called "Name", you shouldn't have another called "Name2" as the value of @Name2 may end up being replaced by the value of @Name suffixed with a "2"._

#### Reset Behaviour

| Parameter             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Example |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- |
| **Auto Offset Reset** | <p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../../error-logging-and-troubleshooting.md">here.</a></p> | None    |

#### Topic JSON

| Parameter  | Description                                                                                                                                                                                                                                                                                                                                                                             | Example                                                                                |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Table GUID | **Mandatory.** The GUID of the table whose notifications you wish to consume. You can find this in the **Design Table** screen.                                                                                                                                                                                                                                                         | 16523e54-4242-4156-835a-0e572e862304                                                   |
| Column(s)  | <p><strong>Mandatory.</strong> The names of the column(s) you wish to include in your sync.<br><br><strong>Note:</strong> If you will be using the <a href="https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/cinchy-event-broker">runQuery=true</a> parameter in your data sync, you only need to include the Cinchy Id in the topic JSON. </p> | <p>Name<br>Age</p>                                                                     |
| BatchSize  | The desired result batch size. This will **default to 1** if not passed in. The maximum batch size is 1000; using a number higher than that will result in a **Bad Request** response.                                                                                                                                                                                                  | 10                                                                                     |
| Filter     | <p><strong>Optional.</strong> When CDC is enabled, you can set a filter on columns where you are capturing changes in order to receive specific data.<br><br>More information on using Filters can be found in <a href="./#old-vs-new-filter">Appendix B.</a></p>                                                                                                                       | "filter": "New.\[Approval State] = 'Approved' AND Old.\[Approval State] != 'Approved'" |

#### Connection Attributes

You do not need to provide Connections Attributes when using the Cinchy CDC Stream Source. However if you inputting your configuration via the Listener Config table, rather than through the Connections UI, you cannot leave the field blank. Instead, insert the below text into the column:

```
{}
```
{% endtab %}

{% tab title="Schema" %}
**The**[ **Schema** ](../../building-data-syncs/columns-and-mappings/#2.-schema-columns)**section** is where you define which source columns you want to sync in your connection. You have the option to add in a Standard, Calculated, Conditional, or JavaScript column. You can repeat the values for multiple columns.

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
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespace** _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                   |         |

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

<figure><img src="../../../.gitbook/assets/image (677).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

</div>

## 4. Next Steps

* Configure your [Destination](../../supported-data-sync-destinations/).
* Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../../building-data-syncs/)

## Appendix A - Source Parameters

The following sections outline more information about specific parameters you can find on this source.

## Run Query

The Run Query parameter is available as an optional value for the Cinchy Event Broker/CDC connector. If set to true it executes a saved query; whichever record triggered the event becomes a parameter in that query. Thus the query now becomes the source instead of the table itself.

You are able to use any parameters defined in your listener config.

<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption><p>Image 3: Run Query</p></figcaption></figure>

#### Example

In the below example, we have a data sync using the Event Broker/CDC as a source. Our Listener Config has been set with the CinchyID attribute _(Image 4)._

<figure><img src="../../../.gitbook/assets/image (314).png" alt=""><figcaption><p>Image 4: Run Query</p></figcaption></figure>

We can enable the Run Query function to use the saved query "CDC Product Ticket Datestamps" as our source instead _(Image 5)._ If we change the data from Record A in our source table to trigger our event, the Query Parameters below show that the Cinchy ID of Record A will be used in the query. This query is now our source.

<figure><img src="../../../.gitbook/assets/image (115).png" alt=""><figcaption><p>Image 5: Run Query</p></figcaption></figure>

It would appear in the data sync config XML as follows:

```xml
  <CinchyEventBrokerDataSource runQuery="true" domain="Product" name="CDC Product Ticket Datestamp" parameters="{&quot;@id&quot;: &quot;Cinchy Id&quot; }">
```

## Appendix B - Listener Configuration Parameters

### messageKeyExpression

Each of your Event Listener message keys a message key. **By default, this key is dictated by the Cinchy ID of the record being changed.**

When the worker processes your Event Listener messages, it does so in batches, and for efficiency and to guarantee order, messages that contain the same key will not be processed in the same batch.

The **messageKeyExpression** property allows you to change the default message key to something else.

#### Possible Use Case

* Ensuring records with the same message key can be updated with the proper ordering to reflect an accurate collaboration log history.

#### Example Syntax

In this example, we want the message key to be based on the \[Employee Id] and \[Name] column of the table that CDC is enabled on.

```
{ "messageKeyExpression": "CONCAT(New.[Employee Id], '-', New.[Name])", … }
```

### Old vs New Filter

The Cinchy Event Broker/CDC Stream Source has the unique capability to use "Old" and "New" parameters when filtering data. This filter can be a powerful tool for ensuring that you sync only the specific data that you want.

The "New" and "Old" parameters are based on updates to single records, not columns/rows.

**"New" Example:**

In the below filter, we only want to sync data where the \[Approval State] of a record is newly 'Approved'. For example, if a record was changed from 'Draft' to 'Approved', the filter would sync the record.

{% hint style="info" %}
Due to internal logic, newly created records will be tagged as both "New" **and** "Old".
{% endhint %}

```
"filter": "New.[Approval State] = 'Approved'
```

**"Old" Example:**

In the below filter, we only want to sync data where the \[Status] of a record was 'In Progress' but has since been updated to any other \[Status]. For example, if a record was changed from 'In Progress' to 'Done', the filter would sync the record.

{% hint style="info" %}
Due to internal logic, newly created records will be tagged as both "New" **and** "Old".
{% endhint %}

```
"filter": "Old.[Status] = 'In Progress'
```
