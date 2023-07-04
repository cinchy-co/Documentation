# Cinchy Table

## Overview

Cinchy Tables are commonly used data sync sources.&#x20;

**Example Use Case:** You want to set up batch sync between a Cinchy Table and HubSpot to sync important sales analytics information. You can do so by using the Cinchy Table as your source, and a REST API as your target.

{% hint style="success" %}
The Cinchy Table source supports batch syncs. To do a real-time sync from a Cinchy Table, you would use the Cinchy Event Broker/CDC Source instead.
{% endhint %}

## Info tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

<table><thead><tr><th>Parameter</th><th width="236.33333333333331">Description</th><th>Example</th></tr></thead><tbody><tr><td>Title</td><td><strong>Mandatory.</strong> Input a name for your data sync</td><td>Cinchy to HubSpot</td></tr><tr><td>Variables</td><td><strong>Optional.</strong> Review our documentation on <a href="../../building-data-syncs/advanced-settings/variables.md">Variables here </a>for more information about this field.</td><td></td></tr><tr><td>Permissions</td><td>Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. <strong>Inputting at least an Admin Group is mandatory.</strong></td><td></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (679).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Cinchy Table</td></tr><tr><td>Domain</td><td><strong>Mandatory.</strong> The domain where your source table resides.</td><td>Product</td></tr><tr><td>Table Name</td><td><strong>Mandatory.</strong> The name of you source table.</td><td>Q1 Sales</td></tr><tr><td>Suppress Duplicate Errors</td><td><strong>Optional.</strong> This field determines whether duplicate keys in the source are to be reported as warnings <em>(unchecked)</em> or ignored <em>(checked)</em>. The default is unchecked.<br>Checking this box can be useful in the event that  you only want to load the distinct values from a collection of columns in the source.</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
|-------------|---------------------------------------------------------------------------------------------------------------|---------|
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | If your data type was chosen as "text", you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                                                                                                           |         |
| Max Length      | **Optional.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                                      |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                       | Example |
|-------------|-----------------------------------------------------------------------------------|---------|
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                       |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../../.gitbook/assets/image (673).png" alt=""><figcaption><p>Image 2: Define your Source</p></figcaption></figure>

</div>

## Next steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Click **Jobs > Start a Job** to begin your sync.
