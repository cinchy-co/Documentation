# Cinchy Table

## Overview

Cinchy Tables are commonly used data sync targets.

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}

{% hint style="success" %}
The Cinchy Table destination supports batch and real-time syncs.
{% endhint %}

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

| Parameter   | Description                                                 | Example      |
|-------------|-------------------------------------------------------------|--------------|
| Destination | Mandatory. Select your destination from the drop down menu. | Cinchy Table |
| Domain      | Mandatory. The domain where your destination table resides. | Product      |
| Table Name  | Mandatory. The name of you destination table.               | Q1 Sales     |
| Suppress Duplicate Errors | Optional. This field determines whether duplicate keys are reported as warnings (unchecked) or ignored (checked). The default is unchecked. Checking this box can be useful in the event that you only want to load the distinct values from a collection of columns in the source. |
| Degree of Parallelism     | The number of batch inserts and updates that can be run in parallel. This defaults to 1.                                                                                                                                                                                                 | 1            |

{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.

| Parameter     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Example |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| Source Column | **Mandatory.** The name of your column as it appears in the source.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Name    |
| Link Column   | <p>This value is only used when utilizing a Cinchy Table destination with a linked column.</<br><br>For example:</p><p>Your destination column is an Employee's favourite colour that links into the System Colours table.<br></p><p>If your source data contains hex values (#00B050) for that mapping, then you'd put Hex Value in the link column field.<br></p><p>If your source data contains the colour names (Green), then you'd put Name in the link column field.</p> |         |

{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<figure><img src="../../.gitbook/assets/image (704).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

## Next steps

- Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
- Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
- Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
- If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sources/) and enable it to begin your sync.
- If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
