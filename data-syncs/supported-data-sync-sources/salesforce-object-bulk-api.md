# Salesforce Object (Bulk API)

## Overview

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Salesforce objects are database tables that permit you to store data that's specific to an organization. Salesforce objects are of two types:

* Standard Objects: Standard objects are the kind of objects that are provided by salesforce.com such as users, contracts, reports, dashboards, etc.
* Custom Objects: Custom objects are those objects that are created by users. They supply information that's unique and essential to their organization. Custom objects are the heart of any application and provide a structure for sharing data.

{% hint style="success" %}
The Salesforce Object (Bulk API) source supports batch syncs.
{% endhint %}

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example             |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | Salesforce Bulk API |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |                     |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |                     |

<figure><img src="../../.gitbook/assets/image (159).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Salesforce Object (Bulk API)</td></tr><tr><td>Object</td><td><strong>Mandatory.</strong> The name of your Salesforce Object.</td><td></td></tr><tr><td>Auth URL</td><td><strong>Mandatory.</strong> The URL that issues your Salesforce auth token.</td><td><a href="https://test.salesforce.com/services/oauth2/token">https://test.salesforce.com/services/oauth2/token</a></td></tr><tr><td>Client ID</td><td><strong>Mandatory.</strong> The encrypted Client ID to connect to your Salesforce Object. The Connections UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Client Secret</td><td><strong>Mandatory.</strong>  The encrypted Client Secret for the above Client ID. The Connections UI will automatically encrypt this value for yo.</td><td></td></tr><tr><td>Username</td><td><strong>Mandatory.</strong> The encrypted Username of an account that can connect to your Salesforce Object. The Connections UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Password</td><td><strong>Mandatory.</strong> The encrypted Password associated with the above account that can connect to your Salesforce Object. The Connections UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Test Connection</td><td><p>You can use the "Test Connection" button to ensure that your credentials are properly configured to access your source.</p><p> </p><p>If configured correctly, a "Connection Successful" pop-up will appear.</p><p></p><p>If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot.</p></td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



Select **Show Advanced** for more options for the Schema section.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  For Text data types, you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
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

<figure><img src="../../.gitbook/assets/image (354).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

</div>

## Next steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Click **Jobs > Start a Job** to begin your sync.
