# Salesforce Platform Event

1\. Overview

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Salesforce Platform Events are secure and scalable messages that contain data. Publishers push out event messages that subscribers receive in real time.

{% hint style="success" %}
The Salesforce Platform Event source supports real-time syncs.
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                       | Example                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                    | Salesforce Platform Event |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                          |                           |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**  |                           |

<figure><img src="../../.gitbook/assets/image (699).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Salesforce Platform Event</td></tr></tbody></table>
{% endtab %}

{% tab title="Listener Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)

#### Reset Behaviour

<table><thead><tr><th width="265.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../error-logging-and-troubleshooting.md">here.</a></p></td><td>None</td></tr></tbody></table>

#### Topic JSON

The below table can be used to help create your Topic JSON needed to set up a real-time sync.

<table><thead><tr><th width="264.66666666666663">Parameter</th><th width="272">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td><strong>Mandatory.</strong> The name of the Platform Event, as it appears in Salesforce, that you want to subscribe to.</td><td>Notification__e</td></tr></tbody></table>

**Example Topic JSON**

<pre class="language-json"><code class="lang-json">{  
        "Name": "Notification__e"
<strong>}
</strong></code></pre>

#### Connection Attributes

The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter       | Description                                                                                                                                  | Example                                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| ApiVersion      | **Mandatory.** Your Salesforce API Version. Note that this needs to be an exact match; for instance "47.0" cannot be written as simply "47". | 47.0                                                                                                     |
| GrantType       | This value should be set to "password".                                                                                                      | password                                                                                                 |
| ClientId        | The encrypted Salesforce Client ID. You can encrypt this value using the Cinchy CLI.                                                         | Bn8UmtiLydmYQV6//qCL5dqfNUMhqchdk959hu0XXgauGMYAmYoyWN8FD+voGuMwGyJa7onrc60q1Hu6QFsQXHVA==               |
| ClientSecret    | The encrypted Salesforce Client Secret. You can encrypt this value using the Cinchy CLI.                                                     | DyU1hqde3cWwkPOwK97T6rzwqv6t3bgQeCGq/fUx+tKI=                                                            |
| Username        | The encrypted Salesforce username. You can encrypt this value using the Cinchy CLI.                                                          | dXNlcm5hbWVAZW1haWwuY29t                                                                                 |
| Password        | The encrypted Salesforce password You can encrypt this value using the Cinchy CLI.                                                           | cGFzc3dvcmRwYXNzd29yZA==                                                                                 |
| InstanceAuthUrl | The authorization URL for Salesforce instance.                                                                                               | [https://login.salesforce.com/services/oauth2/token](https://login.salesforce.com/services/oauth2/token) |

```json
{
 "ApiVersion": 47.0,
 "GrantType": "password",
 "ClientId": "Bn8UmtiLydmYQV6//qCL5dqfNUMhqchdk959hu0XXgauGMYAmYoyWN8FD+voGuMwGyJa7onrc60q1Hu6QFsQXHVA==",
 "ClientSecret": "DyU1hqde3cWwkPOwK97T6rzwqv6t3bgQeCGq/fUx+tKI=",
 "UserName": "dXNlcm5hbWVAZW1haWwuY29t",
 "Password": "cGFzc3dvcmRwYXNzd29yZA=="
 "InstanceAuthUrl": "https://login.salesforce.com/services/oauth2/token"
}
```
{% endtab %}

{% tab title="Schema" %}
**The**[ **Schema**](../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

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

<figure><img src="../../.gitbook/assets/image (460).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../building-data-syncs/)
