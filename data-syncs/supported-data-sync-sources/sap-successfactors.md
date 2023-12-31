# SAP SuccessFactors

## 1. Overview

[SAP SuccessFactors ](https://www.sap.com/products/hcm.html)solutions are cloud-based HCM software applications that support core HR and payroll, talent management, HR analytics and workforce planning, and employee experience management.

{% hint style="success" %}
The SAP SuccessFactors source support batch syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter  | Description                                                                                                                                                | Example            |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| Title      | **Mandatory.** Input a name for your data sync                                                                                                             | SAP SuccessFactors |
| Version    | **Mandatory.** This is a pre-populated field containing a version number for your data sync. You can override it if you wish.                              | 1.0.0              |
| Parameters | **Optional.** Review our [documentation on Parameters here](../building-data-syncs/advanced-settings/parameters.md) for more information about this field. |                    |

<figure><img src="../../.gitbook/assets/image (643).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>SAP SuccessFactors</td></tr><tr><td>API Key</td><td><strong>Mandatory.</strong> The encrypted API Key needed to connect to your SuccessFactors entity. The Connections UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>User ID</td><td><strong>Mandatory.</strong> A User ID with access to connect to your SuccessFactors entity.</td><td></td></tr><tr><td>Company ID</td><td><strong>Mandatory.</strong> The Company ID associated with the above User ID and with access to connect to your SuccessFactors entity.</td><td></td></tr><tr><td>SAML Assertion URL</td><td><strong>Mandatory.</strong>  The URL used for SAML insertion.</td><td><a href="https://apisalesdemo4.successfactors.com/oauth/idp">https://apisalesdemo4.successfactors.com/oauth/idp</a></td></tr><tr><td>OAUTH Token URL</td><td><strong>Mandatory.</strong> The URL that issued your oauth token.</td><td><a href="https://apisalesdemo4.successfactors.com/oauth/token">https://apisalesdemo4.successfactors.com/oauth/token</a></td></tr><tr><td>Private Key</td><td><strong>Mandatory.</strong> The key to connect to your SuccessFactors entity. The Connections UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>OData API URL</td><td><strong>Mandatory.</strong>  The URL for the OData API.</td><td><a href="https://apisalesdemo4.successfactors.com/odata/v2">https://apisalesdemo4.successfactors.com/odata/v2</a></td></tr><tr><td>Entity</td><td><strong>Mandatory.</strong>  The name of your SuccessFactors entity.</td><td></td></tr></tbody></table>
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

<figure><img src="../../.gitbook/assets/image (213).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ Sync Behaviour](../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* Click **Jobs > Start a Job** to begin your sync.
