# Salesforce push topic

## Overview

[Salesforce](https://www.salesforce.com/ca/products/what-is-salesforce/) is a cloud-based CRM software designed for service, marketing, and sales.

Push Topic events provide a secure and scalable way to receive notifications for changes to Salesforce data that match a SOQL (Salesforce Object Query Language) query you define.

You can use Push Topic events to:

* Receive notifications of Salesforce record changes, including create, update, delete, and undelete operations.
* Capture changes for the fields and records that match a SOQL query.
* Receive change notifications for only the records a user has access to based on sharing rules.
* Limit the stream of events to only those events that match a subscription filter

{% hint style="success" %}
The Salesforce Push Topic source supports real-time syncs.
{% endhint %}

## Real-time sync scenarios

You can use a Push Topic already configured in Salesforce, or have Cinchy Event Listener create the Push Topic for you.

#### Scenario 1: Push Topic already exists in Salesforce.

Cinchy will compare the JSON with the properties on the push topic in Salesforce by name. If the attributes match, the listener will start listening on the push topic.

#### Scenario 2: Push Topic already exists in Salesforce and the configuration doesn't match.

Cinchy will compare the JSON with the properties on the push topic in Salesforce by name. If any of the attributes don't match, Cinchy will sync the push topic from Salesforce into Cinchy and disable the listener.

#### Scenario 3: Push Topic doesn't exist in Salesforce.

If the Push Topic name doesn't exist in Salesforce, Cinchy will attempt to create the Push Topic. If it's successful, it will sync in the Id from Salesforce and start listening on the push topic.

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | Salesforce Push Topic |
| Variables   | **Optional.** Review our documentation on [Variables here ](../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                         |                       |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.** |                       |

<figure><img src="../../.gitbook/assets/image (674).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Salesforce Push Topic</td></tr></tbody></table>
{% endtab %}

{% tab title="Listener  Configuration" %}
To set up a real-time sync, you must configure your Listener values. You can do so through the Connections UI.

Note that If there is more than one listener associated with your data sync, you will need to configure the addition listeners via [the Listener Configuration table.](../supported-real-time-sources/the-listener-configuration-table.md)

**Reset Behaviour**

#### Auto Offset Reset table

| Parameter         | Description                                                                                                                                                                                                                                                                                                                                                        | Default Value |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| Auto Offset Reset | <p>Determines the starting point for event reading. Options:</p><ul><li><strong>Earliest</strong>: Starts from the queue beginning. Useful for recoverable or re-runnable use cases.</li><li><strong>Latest</strong>: Starts after the last processed event.</li><li><strong>None</strong>: No events are read.</li></ul><p>You can change this setting later.</p> | None          |

**Topic JSON**

The below table can be used to help create your Topic JSON needed to set up a real-time sync.

| Parameter                  | Description                                                                                                                                                                                                                         | Example                          |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| Id                         |                                                                                                                                                                                                                                     |                                  |
| Name                       | Mandatory. Descriptive name of the PushTopic. Note that there is a 25 character limit on this field.                                                                                                                                | LeadsTopic                       |
| Query                      | Mandatory. The SOQL query statement that determines which record changes trigger events to be sent to the channel. This field has a 1,300 character limit.                                                                          | SELECT Id, Name, Email FROM Lead |
| ApiVersion                 | Mandatory. The API version to use for executing the query specified in Query. It must be an API version greater than 20.0. If your query applies to a custom object from a package, this value must match the package's ApiVersion. | 47.0                             |
| NotifyForOperationCreate   | Set this to true if a create operation should generate a notification, otherwise, false. Defaults to true.                                                                                                                          | true                             |
| NotifyForOperationUpdate   | Set this to true if an update operation should generate a notification, otherwise, false. Defaults to true.                                                                                                                         | true                             |
| NotifyForOperationUndelete | Set this to true if an undelete operation should generate a notification, otherwise, false. Defaults to true.                                                                                                                       | true                             |
| NotifyForOperationDelete   | Set this to true if a delete operation should generate a notification, otherwise, false. Defaults to true.                                                                                                                          | true                             |
| NotifyForFields            | Specifies which fields are evaluated to generate a notification. Possible values are:AllReferenced (default)SelectWhere                                                                                                             | Referenced                       |

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

**Connection Attributes**

The below table can be used to help create your Connection Attributes JSON needed to set up a real-time sync.

| Parameter       | Description                                                                                                                             | Example                                                                                    |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| ApiVersion      | Mandatory. Your Salesforce API Version. Note that this needs to be an exact match; for instance `47.0` can't be written as simply `47`. | 47.0                                                                                       |
| GrantType       | This value should be set to `password`.                                                                                                 | password                                                                                   |
| ClientId        | The encrypted Salesforce Client ID. You can encrypt this value using the Cinchy CLI.                                                    | Bn8UmtiLydmYQV6//qCL5dqfNUMhqchdk959hu0XXgauGMYAmYoyWN8FD+voGuMwGyJa7onrc60q1Hu6QFsQXHVA== |
| ClientSecret    | The encrypted Salesforce Client Secret. You can encrypt this value using the Cinchy CLI.                                                | DyU1hqde3cWwkPOwK97T6rzwqv6t3bgQeCGq/fUx+tKI=                                              |
| Username        | The encrypted Salesforce username. You can encrypt this value using the Cinchy CLI.                                                     | dXNlcm5hbWVAZW1haWwuY29t                                                                   |
| Password        | The encrypted Salesforce password You can encrypt this value using the Cinchy CLI.                                                      | cGFzc3dvcmRwYXNzd29yZA==                                                                   |
| InstanceAuthUrl | The authorization URL of the Salesforce instance.                                                                                       | https://login.salesforce.com/services/oauth2/token                                         |

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

Select **Show Advanced** for more options for the Schema section.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.** For Text data types, you can choose whether to **trim the whitespace**.\_                                                                                                                                                                                                                                                                                                                                                                           |         |
| Max Length      | **Optional if data type = text.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution Log).                                                                                                                                                                                                                  |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                       | Example |
| ----------- | --------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                       |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (738).png" alt=""><figcaption><p>Image 2: The Source Tab</p></figcaption></figure>

## Next steps

* Configure your [Destination](../supported-data-sync-destinations/)
* Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* If more than one listener is needed for a real-time sync, configure it/them via [the Listener Config table.](../supported-real-time-sources/the-listener-configuration-table.md)
* To run a real-time sync, enable your Listener from [the Execution tab.](../building-data-syncs/)
