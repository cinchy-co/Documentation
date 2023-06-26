# Salesforce Object

## 1. Overview

Salesforce is a cloud-based CRM software designed for service, marketing, and sales.

Prior to setting up your data sync destination, [ensure that you've configured your Source.](../supported-data-sync-sources/)

{% hint style="success" %}
The Salesforce destination supports batch and real-time syncs.
{% endhint %}

## 2. Destination Tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Destination</td><td><strong>Mandatory.</strong> Select your destination from the drop down menu.</td><td>Salesforce</td></tr><tr><td>Object</td><td><strong>Mandatory.</strong> The API name of the platform event (with the __e).</td><td>Notification__e</td></tr><tr><td>Auth URL</td><td><strong>Mandatory.</strong> The URL that issues your Salesforce token.</td><td><a href="https://test.salesforce.com/services/oauth2/token">https://test.salesforce.com/services/oauth2/token</a> </td></tr><tr><td>Client ID</td><td><strong>Mandatory.</strong> The encrypted Client ID for the connected app. The Connection UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Client Secret</td><td><strong>Mandatory.</strong> The encrypted Client Secret for the connected app. The Connection UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Username</td><td><strong>Mandatory.</strong> The Username of a Salesforce account with the permissions to connect to your Object. The Connection UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Password</td><td><strong>Mandatory.</strong> The Password of the above Salesforce account. The Connection UI will automatically encrypt this value for you.</td><td></td></tr><tr><td>Auto Assign</td><td><strong>Optional.</strong> You can set this to true to leverage<a href="https://help.salesforce.com/s/articleView?id=000385136&#x26;language=en_US&#x26;type=1"> auto assignment</a> in Salesforce.</td><td></td></tr><tr><td>Platform Event</td><td><strong>Optional.</strong> You can set this to true to enable <a href="https://www.salesforceben.com/salesforce-platform-events/">Platform Events.</a></td><td></td></tr></tbody></table>
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

<figure><img src="../../.gitbook/assets/image (366).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

## 4. Next Steps

* Define your[ Sync Behaviour](../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
