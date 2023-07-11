---
description: >-
  This page goes over the several ways to work with (enter, update, remove, load
  and extract) data from Cinchy tables.
---

# Data Management

## Data entry

Users are only able to enter data into Cinchy based on their access. Users can also copy and paste data from external sources.

## Insert/Delete data rows <a href="#insert-delete-data-rows" id="insert-delete-data-rows"></a>

Users are only able to insert or delete rows based on their access. If you have the ability to insert and/or delete a row of data it will be visible when right-clicking on a row of data _(Image 1)._

![Image 1: Inserting/Deleting Rows](<../../.gitbook/assets/image (313).png>)

## Import data <a href="#import-data" id="import-data"></a>

Importing data allows you to add new rows of data into a table. If you want to perform a sync, [refer to the CLI](../../data-syncs/cli-commands-list.md). Importing data acts as a smart copy-and-paste of new data into an existing table.

Importing the first row of your CSV as a header row will match the headers to the column names within your table. Columns that can't be matched are ignored, as well as any columns you don't have edit permissions for.

Users can import data from a CSV file to an existing table in Cinchy. Importing data into a Cinchy table only adds records to the table. This data import type doesn't update or append existing records.

To import data into a table, complete the following:

1. From within the table, click the Import button on the top toolbar of the table _(Image 2)._

![Image 2: Step 1, Clicking the import button](<../../.gitbook/assets/image (322).png>)

2. Click **Choose File** to locate and import your file.

3. Validate the imported columns and click **next** _(Image 3)._

![Image 3: Step 3, validate your columns](<../../.gitbook/assets/image (510).png>)

4. Click the **Import** button

5. Click the **OK** button on the Import confirmation window

### Import errors <a href="#import-errors" id="import-errors"></a>

If there are import errors, click the download button next to Rejected Rows on the **Import Succeeded with Errors** window _(Image 4)._

![Image 4: Import Errors](<../../.gitbook/assets/image (671).png>)

You will get a file back with all the rejected rows, as well as the 2 columns added called **‘Cinchy Import Errors'** and **'Cinchy Import Original Row Number’.**

### Cinchy original row number

This provides a reference to the row number in the original file you imported in case you need to check it.

You can simply fix any errors in your error log followed by importing the error log since successful rows are omitted.

## Export data <a href="#export-data" id="export-data"></a>

Users are only able to export data in CSV or TSV format. In tables with over 1000 records, you will need to export each page of data separately, or you can use the [CLI to export your entire table at once.](../../data-syncs/cli-commands-list.md)

{% hint style="warning" %}
When data is exported out of the network, it's now just a copy and no longer connected to Cinchy.
{% endhint %}

To export data from a table, complete the following:

1. From within the table, click the Export button in the table toolbar
2. Select the Export file type (CSV or TSV) _(Image 5)._
3. Open your file in Excel, or any other CSV software, to view.

![Image 5: Exporting Data](<../../.gitbook/assets/image (20).png>)

## 5. Approve/Reject Data <a href="#approve-reject-data" id="approve-reject-data"></a>

<!-- vale off -->

{% embed url="https://cinchy.tv/all-content/1540" %}

<!-- vale on -->

Cinchy can have data change approvals for when data is added or removed from a table view. A change approval process can be put into place for the addition or removal of specific data. If you have been identified as an "Approval" of data you will have the ability to:

- Approve a cell of data
- Approve a row of data
- Reject a row of data

To approve or reject a cell/row of data, complete the following:

1. Right-click on the desired row/cell
2. Select Approve row/cell or Reject row/cell

## Collaboration log

<!-- vale off -->

{% embed url="https://cinchy.tv/all-content/1544" %}

<!-- vale on -->

The Collaboration log is accessible from every table within Cinchy (including metadata). It shows the version history of ALL changes that have been made to an individual row of data.

To access Cinchy’s Collaboration Log:

1. Open the desired table
2. Locate the desired row **> Right Click > View Collaboration Log** _(Image 6)._

![Image 6: Step 2, Open the Collaboration Log](<../../.gitbook/assets/image (514).png>)

Once the Collaboration Log is open you have the ability to view ALL changes with a version history for the row selected within the table.

Users have the ability to revert to a prior version of the record. To do so, click the Revert button for the desired version _(Image 7)._

![Image 7: Reverting Data in the Collaboration Log](<../../.gitbook/assets/image (634).png>)

{% hint style="info" %}
A record can have a Revert button. This indicates that version record is identical to the current version of the record in the table. Hovering over the Revert button displays a tool-tip.
{% endhint %}

### Data erasure and compression policies

By default, Cinchy doesn't delete any data or metadata from within the Data Fabric.

Click here for more information on [Data Erasure](../builder-guides/creating-tables/data-controls/data-erasure.md) & [Compression](../builder-guides/creating-tables/data-controls/data-compression.md) Policies in Cinchy

### Audit for data synchronization

Audit Logging of data loaded into Cinchy via Data Synchronization such as batch or real-time using the [Cinchy CLI](../../data-syncs/cli-commands-list.md), or through data changes by any Saved Queries exposed as APIs to external clients, is recorded the same way as if a user entered the data into Cinchy. All data synced into Cinchy will have corresponding line items in the Collaboration Log similarly to how it's handled when data is entered / modified in Cinchy by a User.

### Collaboration log performance considerations

The Collaboration Log data is also stored within Cinchy as data, allowing the logs to be available for use through a query or for any downstream consumers. The logs have no separate performance considerations needed, as it relies on the Cinchy platform’s performance measures.

## Recycle bin <a href="#recycle-bin" id="recycle-bin"></a>

All data records that have been deleted are put into Cinchy’s **Recycle Bin**. Data that resides in the Recycle Bin can be restored if required.

To restore data from the recycle bin:

1. From the left-hand navigation, click Recycle Bin _(Image 8)_

![Image 8: The Recycle Bin](<../../.gitbook/assets/image (140).png>)

2. Locate the row for restoring

3. Right-click and select **Restore Row.**

The restored row will now be visible in your table.

{% hint style="info" %}
If Change Approvals are turned on, that row will need to be approved.
{% endhint %}
