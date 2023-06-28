# Dynamics

## 1. Overview

[Microsoft Dynamics 365 ](https://dynamics.microsoft.com/en-us/)functions as an interconnected CRM, ERP, and productivity suite that integrates processes, data, and business logic.

Prior to setting up your data sync destination, [ensure that you've configured your Source.](../supported-data-sync-sources/)

{% hint style="success" %}
The Dynamics destination supports batch and real-time syncs.
{% endhint %}

## 2. Destination Tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Destination</td><td><strong>Mandatory.</strong> Select your destination from the drop down menu.</td><td>Dynamics</td></tr><tr><td>Entity</td><td><strong>Mandatory.</strong> The name of the entity you want to sync to as it appears in your Dynamics CRM.</td><td></td></tr><tr><td>Service URL</td><td><strong>Mandatory.</strong> The <a href="https://community.dynamics.com/365/b/dynamics-365-education-and-knowledge/posts/get-the-web-api-url-for-a-dynamics-365-organization">Web API URL</a> for your instance.</td><td><a href="https://org.api.crm.dynamics.com/api/data/v9.0/">https://org.api.crm.dynamics.com/api/data/v9.0/</a></td></tr><tr><td>Client ID</td><td><strong>Mandatory.</strong> The encrypted Client ID found in your Azure AD app registration. The Connection UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Client Secret</td><td><strong>Mandatory.</strong> The encrypted Client Secret found in your Azure AD app registration. The Connection UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>ID Column</td><td><strong>Mandatory.</strong> The unique ID Column name of the Entity that you wish to sync to.</td><td></td></tr><tr><td>ID Column Insert</td><td>Setting this value to true will allow direct inserts into the ID Column indicated above. The default state is false: ID values will be matched but no data will be inserted.</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

| Parameter     | Description                                                              | Example |
| ------------- | ------------------------------------------------------------------------ | ------- |
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (357).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

## 4. Next Steps

* Define your[ Sync Behaviour](../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
