# MS SQL Server Table

## Overview

Microsoft SQL Server is one of the main relational database management systems on the market that serves a wide range of software applications for business intelligence and analysis in corporate environments.

Based on the Transact-SQL language, it incorporates a set of standard language programming extensions and its application is available for use both on premise and in the cloud.

Microsoft SQL Server is ideal for storing all the desired information in relational databases, as well as to manage such data without complications, thanks to its visual interface and the options and tools it has.

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}

{% hint style="success" %}
The MS SQL Server Table destination supports batch and real-time syncs.
{% endhint %}

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

| Parameter         | Description                                                                                                                                          | Example                                         |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| Destination       | Mandatory. Select your destination from the drop down menu.                                                                                          | MS SQL Server Table                             |
| Connection String | Mandatory. The encrypted Connection String used to connect to your MS SQL Server. The Connections UI will automatically encrypt this value for you. | You can find an example Connection String here. |
| Table             | Mandatory. The name of the MS SQL Server table that you want to sync your data to, including the schema                                              | dbo.employees                                   |
| ID Column           | The name of the identity column that exists in the destination (or a single column that's guaranteed to be unique and automatically populated for every new record)                                                                                                                                                                           |
| ID Column Data Type | The data type of the above ID Column.                                                                                                                                                                                                                                                                                                          |
| Test Connection     | You can use the "Test Connection" button to ensure that your credentials are properly configured to access your destination.If configured correctly, a "Connection Successful" pop-up will appear. If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot. |
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

| Parameter     | Description                                                              | Example |
|---------------|--------------------------------------------------------------------------|---------|
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |
{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (760).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

</div>

## 4. Next Steps

* Define your [Sync Actions.](../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
