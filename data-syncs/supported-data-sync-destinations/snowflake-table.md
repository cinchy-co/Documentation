# Snowflake table

## Overview

t provides a single platform for data warehousing, data lakes, data engineering, data science, data application development, and secure sharing and consumption of real-time/shared data.

Snowflake enables data storage, processing, and analytic solutions.

{% hint style="info" %}
Before you set up your data sync destination, [make sure to configure your Source.](../supported-data-sync-sources/)
{% endhint %}
{% hint style="success" %}
The Snowflake Table destination supports batch and real-time syncs.
{% endhint %}

### Loading data into Snowflake

**For batch syncs of 10 records or less,** single Insert/Update/Delete statements are executed to perform operations against the target Snowflake table.

**For batch syncs exceeding 10 records,** the operations are performed in bulk.

The bulk operation process consists of:

1. Generating a CSV containing a batch of records
2. Creating a temporary table in Snowflake
3. Copying the generated CSV into the temp table
4. If needed, **performing Insert operations** against the **target** Snowflake table using the temp table
5. If needed, **performing Update operations** against the **target** Snowflake table using the temp table
6. If needed, **performing Delete operations** against the **target** Snowflake table using the temp table
7. Dropping the temporary table

{% hint style="info" %}
**Real time sync volume size** is based on a dynamic batch size up to configurable threshold.
{% endhint %}

## Considerations

- The temporary table generated in the bulk flow process for high volume scenarios transforms all columns of data type **Number** to be of type **NUMBER(38, 18)**. This may cause precision loss if the number scale in the target table is higher

## Destination tab

The following table outlines the mandatory and optional parameters you will find on the Destination tab _(Image 1)._

{% tabs %}
{% tab title="Destination Details" %}
The following parameters will help to define your data sync destination and how it functions.

| Parameter           | Description                                                                                                                                                                                                                                                                                                                                    | Example                                                                                                       |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Destination         | Mandatory. Select your destination from the drop down menu.                                                                                                                                                                                                                                                                                    | Snowflake Table                                                                                               |
| Connection String   | Mandatory. The encrypted connection string used to connect to your Snowflake instance. You can review Snowflake's Connection String guide and parameter descriptions here.                                                                                                                                                                     | Unencrypted example: account=wr38353.ca-central-1.aws;user=myuser;password=mypassword;db=CINCHY;schema=PUBLIC |
| Table               | Mandatory. The name of the Table in Snowflake that you wish to sync.                                                                                                                                                                                                                                                                           | Employees                                                                                                     |
| ID Column           | Mandatory if you want to use "Delete" action in your sync behaviour configuration. The name of the identity column that exists in the target (OR a single column that's guaranteed to be unique and automatically populated for every new record).                                                                                            | Employee ID                                                                                                   |
| ID Column Data Type | Mandatory if using the ID Column parameter. The data type of the above ID Column. Either: Text, Number, Date, Boolean, Geography, or Geometry                                                                                                                                                                                                      | Number                                                                                                        |
| Test Connection     | You can use the "Test Connection" button to ensure that your credentials are properly configured to access your destination. If configured correctly, a "Connection Successful" pop-up will appear. If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot. |

{% endtab %}

{% tab title="Column Mapping" %}
**The** [**Column Mapping**](../building-data-syncs/columns-and-mappings/#3.-column-mappings) **section** is where you define which source columns you want to sync to which destination columns. You can repeat the values for multiple columns.\
\
When specifying the Target Column in the Column Mappings section, **all names are case-sensitive.**

| Parameter     | Description                                                              | Example |
| ------------- | ------------------------------------------------------------------------ | ------- |
| Source Column | **Mandatory.** The name of your column as it appears in the source.      | Name    |
| Target Column | **Mandatory.** The name of your column as it appears in the destination. | Name    |

{% endtab %}

{% tab title="Filter" %}
You have the option to add a destination filter to your data sync. Please review the documentation here for more information on [destination filters.](../building-data-syncs/advanced-settings/filters.md#target-filters)
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (632).png" alt=""><figcaption><p>Image 2: Define your Destination</p></figcaption></figure>

</div>

## Next steps

- Define your[ ](../building-data-syncs/sync-actions.md)[Sync Actions.](../building-data-syncs/sync-actions.md)
- Add in your [Post Sync Scripts](../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
- Define your [Permissions](../building-data-syncs/#2.-create-a-data-sync-configuration).
- If you are running a real-time sync, [set up your Listener Config](../supported-real-time-sync-stream-sources/) and enable it to begin your sync.
- If you are running a batch sync, click **Jobs > Start a Job** to begin your sync.
