# MS SQL Server (Query and Table)

## 1. Overview

Microsoft SQL Server is one of the main relational database management systems on the market that serves a wide range of software applications for business intelligence and analysis in corporate environments.

Based on the Transact-SQL language, it incorporates a set of standard language programming extensions and its application is available for use both on premise and in the cloud.

Microsoft SQL Server is ideal for storing all the desired information in relational databases, as well as to manage such data without complications, thanks to its visual interface and the options and tools it has.

{% hint style="success" %}
The MS SQL Server Query and Table sources support batch syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter  | Description                                                                                                                                                | Example     |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| Title      | **Mandatory.** Input a name for your data sync                                                                                                             | MS SQL Sync |
| Version    | **Mandatory.** This is a pre-populated field containing a version number for your data sync. You can override it if you wish.                              | 1.0.0       |
| Parameters | **Optional.** Review our [documentation on Parameters here](../building-data-syncs/advanced-settings/parameters.md) for more information about this field. |             |

<figure><img src="../../.gitbook/assets/image (602).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>MS SQL Server</td></tr><tr><td>Connection String</td><td><strong>Mandatory.</strong> The Connection String to connect to your MS SQL Server. The Connections UI will encrypt this value.</td><td><a href="https://www.connectionstrings.com/sql-server/">Please see here </a>for example Connection Strings.</td></tr><tr><td>Object</td><td><strong>Mandatory.</strong> The type of Object you want to use in your data sync.</td><td>This can be either <strong>Table</strong> or <strong>Query.</strong></td></tr><tr><td>Table</td><td><strong>Appears when Object = Table.</strong> The name of the Table (including the schema) you want to sync out of your MS SQL Server.</td><td>Employees</td></tr><tr><td>Query</td><td><strong>Appears when Object = Query.</strong> This should be a SELECT statement indicating the data you want to sync out of your MS SQL Server.</td><td>Select * from dbo.employees</td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

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
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespac**e _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                   |         |
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
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (623).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ Sync Behaviour](../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* Click **Jobs > Start a Job** to begin your sync.
