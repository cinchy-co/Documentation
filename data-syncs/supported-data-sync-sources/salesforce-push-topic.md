# Salesforce Push Topic

## 1. Overview

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Push Topic events provide a secure and scalable way to receive notifications for changes to Salesforce data that match a SOQL (Salesforce Object Query Language) query you define.&#x20;

You can use Push Topic events to:

* Receive notifications of Salesforce record changes, including create, update, delete, and undelete operations.&#x20;
* Capture changes for the fields and records that match a SOQL query.&#x20;
* Receive change notifications for only the records a user has access to based on sharing rules.&#x20;
* Limit the stream of events to only those events that match a subscription filter

{% hint style="success" %}
The Salesforce Push Topic source supports real-time syncs.
{% endhint %}

## 1.1 Real-Time Sync Scenarios

You can use a Push Topic already configured in Salesforce, or have Cinchy Event Listener create the Push Topic for you.

#### Scenario 1: Push Topic already exists in Salesforce.

Cinchy will compare the JSON with the properties on the push topic in Salesforce by name. If the attributes match, the listener will start listening on the push topic.

#### Scenario 2: Push Topic already exists in Salesforce and the configuration does not match.

Cinchy will compare the JSON with the properties on the push topic in Salesforce by name. If any of the attributes do not match, Cinchy will sync the push topic from Salesforce into Cinchy and disable the listener.

#### Scenario 3: Push Topic does not exist in Salesforce.

If the Push Topic name does not exist in Salesforce, Cinchy will attempt to create the Push Topic. If it is successful, it will sync in the Id from Salesforce and start listening on the push topic.&#x20;

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                       | Example               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                    | Salesforce Push Topic |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                          |                       |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**  |                       |

<figure><img src="../../.gitbook/assets/image (674).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Salesforce Push Topic</td></tr></tbody></table>
{% endtab %}

{% tab title="Listener  Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)

#### Reset Behaviour

<table><thead><tr><th width="265.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><strong>Auto Offset Reset</strong></td><td><p><strong>Earliest, Latest or None.</strong> <br><br>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br></p><p><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed. This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.<br><br>You are able to switch between Auto Offset Reset types after your initial configuration through the process outlined <a href="../error-logging-and-troubleshooting.md">here.</a></p></td><td>None</td></tr></tbody></table>

#### Topic JSON

The below table can be used to help create your Topic JSON needed to set up a real-time sync.

<table><thead><tr><th width="264.66666666666663">Parameter</th><th width="272">Description</th><th>Example</th></tr></thead><tbody><tr><td>Id</td><td></td><td></td></tr><tr><td>Name</td><td><strong>Mandatory.</strong> Descriptive name of the PushTopic. Note that there is a 25 character limit on this field.</td><td>LeadsTopic</td></tr><tr><td>Query</td><td><p><strong>Mandatory.</strong> The SOQL query statement that determines which record changes trigger events to be sent to the channel.</p><p>Note that there is a 1,300 character limit on this field.</p><p></p></td><td>SELECT Id, Name, Email FROM Lead</td></tr><tr><td>ApiVersion</td><td><strong>Mandatory.</strong> The API version to use for executing the query specified in Query. It must be an API version greater than 20.0. If your query applies to a custom object from a package, this value must match the package's ApiVersion.</td><td>47.0</td></tr><tr><td>NotifyForOperationCreate</td><td>Set this to true if a create operation should generate a notification, otherwise, false. Defaults to true.</td><td>true</td></tr><tr><td>NotifyForOperationUpdate</td><td>Set this to true if an update operation should generate a notification, otherwise, false. Defaults to true.</td><td>true</td></tr><tr><td>NotifyForOperationUndelete</td><td>Set this to true if an undelete operation should generate a notification, otherwise, false. Defaults to true.</td><td>true</td></tr><tr><td>NotifyForOperationDelete</td><td>Set this to true if a delete operation should generate a notification, otherwise, false. Defaults to true.</td><td>true</td></tr><tr><td>NotifyForFields</td><td><p></p><p>Specifies which fields are evaluated to generate a notification. Possible values are:</p><ul><li>All</li><li>Referenced (default)</li><li>Select</li><li>Where</li></ul></td><td>Referenced</td></tr></tbody></table>

**Example Topic JSON**

```json
{
  "Id": "",
  "Name": "LeadsTopic",
  "Query": "SELECT Id, Name, Email FROM Lead",
  "ApiVersion": 47.0,
  "NotifyForOperationCreate": true,
  "NotifyForOperationUpdate": true,
  "NotifyForOperationUndelete": true,
  "NotifyForOperationDelete": true,
  "NotifyForFields": "Referenced"
}
```

#### Connection Attributes

The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

<table><thead><tr><th width="261.66666666666663">Parameter</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td>ApiVersion</td><td><strong>Mandatory.</strong> Your Salesforce API Version. Note that this needs to be an exact match; for instance "47.0" cannot be written as simply "47".</td><td>47.0</td></tr><tr><td>GrantType</td><td>This value should be set to "password".</td><td>password</td></tr><tr><td>ClientId</td><td>The encrypted Salesforce Client ID. You can encrypt this value using the Cinchy CLI.</td><td>Bn8UmtiLydmYQV6//qCL5dqfNUMhqchdk959hu0XXgauGMYAmYoyWN8FD+voGuMwGyJa7onrc60q1Hu6QFsQXHVA==</td></tr><tr><td>ClientSecret</td><td>The encrypted Salesforce Client Secret. You can encrypt this value using the Cinchy CLI.</td><td>DyU1hqde3cWwkPOwK97T6rzwqv6t3bgQeCGq/fUx+tKI=</td></tr><tr><td>Username</td><td>The encrypted Salesforce username. You can encrypt this value using the Cinchy CLI.</td><td>dXNlcm5hbWVAZW1haWwuY29t</td></tr><tr><td>Password</td><td>The encrypted Salesforce password You can encrypt this value using the Cinchy CLI.</td><td>cGFzc3dvcmRwYXNzd29yZA==</td></tr><tr><td>InstanceAuthUrl</td><td>The authorization URL for Salesforce instance.</td><td><a href="https://login.salesforce.com/services/oauth2/token">https://login.salesforce.com/services/oauth2/token</a></td></tr></tbody></table>

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

<figure><img src="../../.gitbook/assets/image (738).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## 4. Next Steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../building-data-syncs/)
