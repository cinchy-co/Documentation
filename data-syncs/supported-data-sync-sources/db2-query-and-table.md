# DB2 (Query and Table)

## 1. Overview

[DB2](https://www.ibm.com/products/db2) (Formerly _Db2_ for LUW) is a relational database that delivers advanced data management and analytics capabilities for transactional workloads.&#x20;

{% hint style="success" %}
The DB2 source supports batch syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example       |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | DB2 to Cinchy |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |               |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |               |

<figure><img src="../../.gitbook/assets/image (695).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>DB2</td></tr><tr><td>Connection String</td><td><strong>Mandatory.</strong> The encrypted connection string used to access your DB2 database. <strong>The Connection UI will automatically encrypt this value for you.</strong></td><td><a href="https://www.connectionstrings.com/ibm-db2/">See here</a> for sample connection strings.</td></tr><tr><td>Object</td><td><strong>Mandatory.</strong> The type of object you want to use as your data sync. <strong>This will be either Table or Query.</strong></td><td>Table</td></tr><tr><td>Table</td><td><strong>Appears when "Table" is selected as the Object Type.</strong><br>The name of your table as it appears in your DB2 database.</td><td>dbo.employees</td></tr><tr><td>Query</td><td><strong>Appears when "Query" is selected as the Object Type.</strong><br>This should be a SELECT statement indicating the data you want to sync out of your DB2 database.</td><td>Select * from dbo.employees</td></tr><tr><td>Test Connection</td><td><p>You can use the "Test Connection" button to ensure that your credentials are properly configured to access your source.</p><p> </p><p>If configured correctly, a "Connection Successful" pop-up will appear.</p><p></p><p>If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot.</p></td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                                                                                                                                                                                                | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- |
| Name        | <p><strong>Mandatory.</strong> The name of your column as it appears in the source. This should be in all caps.<br><br><strong>EXCEPTION: If you chose "query" as your object and use double quotes around the column names, then this value should should match that casing.</strong></p> | NAME    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync.                                                                                                                                                                              |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                                                                                                                                                                                                         | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                                                                                                                                                                                                          |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |
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

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (397).png" alt=""><figcaption><p>Image 2: Source Tab</p></figcaption></figure>

</div>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Click **Jobs > Start a Job** to begin your sync.
