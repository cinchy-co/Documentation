# Cinchy query

## Overview

Cinchy queries are commonly used data sync sources that leverage the platform's Saved Query functionality. For more on creating Saved Queries,[ see the Saved queries page](../../../guides-for-using-cinchy/builder-guides/saved-queries.md).

**Example Use Case:** You want to set up batch sync between a **Cinchy Query** and a **Cinchy Table**. You query polls for any unapproved timesheets, out of office requests, or sick hours and, if found, adds them to an "Open Approval Tasks" table.&#x20;



## Info tab

You can review the parameters in the info tab below _(Image 1)._

#### Values

| Parameter   | Description                                                                                                                                                                                      | Example             |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                   | Open Approval Tasks |
| Variables   | **Optional.** Review our documentation on [Variables here ](../../building-data-syncs/advanced-settings/variables.md)for more information about this field.                                      |                     |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, or execute permissions. **Inputting at least an Admin Group is mandatory.** |                     |

<figure><img src="../../../.gitbook/assets/image (699).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Cinchy Query</td></tr><tr><td>Domain</td><td><strong>Mandatory.</strong> The domain where your source query resides.</td><td>Compliance</td></tr><tr><td>Query Name</td><td><strong>Mandatory.</strong> The name of you source query.</td><td>Open Tasks</td></tr><tr><td>Timeout</td><td><strong>Optional.</strong> The timeout, in number of seconds, for your source query. If not entered this value will default to 30.</td><td>120</td></tr><tr><td>Parameters</td><td><strong>Optional.</strong> Review our documentation on <a href="../../building-data-syncs/advanced-settings/variables.md">Parameters here</a> for more information about this field.</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example         |
|-------------|---------------------------------------------------------------------------------------------------------------|-----------------|
| Name        | **Mandatory.** The name of your column as it appears in the source query.                                     | Owner Cinchy ID |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |                 |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text            |
| Description | **Optional.** You may choose to add a description to your column.                                             |                 |



Select **Show Advanced**  for more options for the Schema section. 

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | If your data type was chosen as "text", you can choose whether to **trim the whitespace** _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                                                      |         |
| Max Length      | **Optional.** You can input a numerical value in this field that represents the maximum length of the data that can be synced in your column. If the value is exceeded, the row will be rejected (you can find this error in the Execution. Log).                                                                                                                                                                                                                                     |         |
{% endtab %}
{% endtabs %}

<figure><img src="../../../.gitbook/assets/image (335).png" alt=""><figcaption><p>Image 2: Define your Source</p></figcaption></figure>

## Next steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Click **Jobs > Start a Job** to begin your sync.
