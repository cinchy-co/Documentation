# Cinchy Table

## 1. Overview

Cinchy Tables are commonly used data sync targets.

Prior to setting up your data sync destination, [ensure that you've configured your Source.](../supported-data-sync-sources/)

{% hint style="success" %}
The Cinchy Table destination supports batch and real-time syncs.
{% endhint %}

## 2. Destination Tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Destination</td><td><strong>Mandatory.</strong> Select your destination from the drop down menu.</td><td>Cinchy Table</td></tr><tr><td>Domain</td><td><strong>Mandatory.</strong> The domain where your destination table resides.</td><td>Product</td></tr><tr><td>Table Name</td><td><strong>Mandatory.</strong> The name of you destination table.</td><td>Q1 Sales</td></tr><tr><td>Suppress Duplicate Errors</td><td><strong>Optional.</strong> This field determines whether duplicate keys are to be reported as warnings <em>(unchecked)</em> or ignored <em>(checked)</em>. The default is unchecked.<br>Checking this box can be useful in the event that  you only want to load the distinct values from a collection of columns in the source.</td><td></td></tr><tr><td>Degree of Parallelism</td><td>The number of batch inserts and updates that can be run in parallel. This defaults to 1.</td><td>1</td></tr></tbody></table>
{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

| Parameter     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Example |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Source Column | **Mandatory.** The name of your column as it appears in the source.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Name    |
| Link Column   | <p><strong>This value is only used when utilizing a Cinchy Table destination with a linked column.</strong> <br><br>For example:</p><p>Your destination column is an Employee's favourite colour that links into the System Colours table.<br></p><p>If your source data contains hex values (eg. #00B050) for that mapping, then you'd put <strong>Hex Value in the link column field.</strong><br></p><p>If your source data contains the colour names (eg. Green), <strong>then you'd put Name in the link column field.</strong></p> |         |
{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (417).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

## 4. Next Steps

* Define your[ Sync Behaviour](../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
* If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
* If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
