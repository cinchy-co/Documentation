# Binary file

## Overview

A binary file is a computer file that is **not a text file**, and whose content is in a binary format consisting of a series of sequential bytes, each of which is eight bits in length.

You can use binary files from a **Local upload, Amazon S3, or Azure Blob Storage** in your data syncs.

Some benefits of using binary files include:

* **Better efficiency via compression**
* **Better Security** through the ability to create custom encoding standards.
* **Unmatched Speed,** since the data is stored in a raw format, and is not encoded using any character encoding standards, it is faster to read and store.

**Example Use Case:** You have a binary file that contains your Employee information. You want to use a batch sync to pull this info into a Cinchy table and liberate your data.

{% hint style="success" %}
The Binary File source supports batch syncs.
{% endhint %}

## Info tab

You can find the parameters in the **Info** tab below _(Image 1)_.

#### Values

| Parameter   | Description                                                                                                                                                                                                     | Example       |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| Title       | **Mandatory.** Input a name for your data sync                                                                                                                                                                  | Employee Sync |
| Variables   | **Optional.** Review our documentation on [Variables here ](../../building-data-syncs/advanced-settings/variables.md)for more information about this field. When uploading a local file, set this to filepath.  | @Filepath     |
| Permissions | Data syncs are role based access systems where you can give specific groups read, write, execute, and/or all of the above with admin access. **Inputting at least an Admin Group is mandatory.**                |               |

<figure><img src="../../../.gitbook/assets/image (640).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## Source tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>(Sync) Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Binary File</td></tr><tr><td>Source</td><td>The location of the source file. Either a <strong>Local upload, Amazon S3, or Azure Blob Storage</strong><br><br>The following authentication methods are supported per source:<br><br><strong>Amazon S3:</strong> Access Key ID/Secret Access Key<br><br><strong>Azure Blob Storage:</strong> Connection String</td><td>Local</td></tr><tr><td>Header Rows to Ignore</td><td><strong>Mandatory.</strong> The number of records from the top of the file to ignore before the data starts (includes column header).</td><td>1</td></tr><tr><td>Footer Rows to Ignore</td><td><strong>Mandatory.</strong> The number of records from the bottom of the file to ignore</td><td>0</td></tr><tr><td>Encoding</td><td><strong>Optional.</strong> The encoding of the file. This default to UTF8, however also supports: <strong>UTF8_BOM, UTF16, ASCII.</strong></td><td></td></tr><tr><td>Path</td><td><strong>Mandatory.</strong> The path to the source file to load. To upload a local file, you must first insert a <strong>Variable</strong> in the <strong>Info tab</strong> of the connection <em>(ex: filepath).</em> Then, you would reference that same value in this location <em>(Ex: @Filepath)</em>. This will then trigger a <strong>File Upload</strong> option to import your file.</td><td>@Filepath</td></tr><tr><td>AuthType</td><td><p></p><p>This field defines the authentication type for your data sync. Cinchy supports "Access Key" and "IAM" role.<br><br>When selecting "Access Key", you must provide the key and key secret.<br><br>When selecting "IAM role", a new field will appear for you to paste in the role's Amazon Resource Name (ARN). You also must ensure that:</p><ul><li>The role <a href="https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/F1vvLbEMfTF1UqCFU9hs/~/changes/320/deployment-guide/deployment-installation-guides/kubernetes-deployment-installation/configuring-aws-iam-for-connections">must be configured</a> to have at least read access to the source</li><li>The Connections pods' role must <a href="https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/F1vvLbEMfTF1UqCFU9hs/~/changes/320/deployment-guide/deployment-installation-guides/kubernetes-deployment-installation/configuring-aws-iam-for-connections#1.2-authorizing-for-data-syncs">have permission to assume the role</a> specified in the data sync config</li></ul></td><td></td></tr><tr><td>Test Connection</td><td>You can use the <strong>"Test Connection"</strong> button to ensure that your credentials are properly configured to access your source. <br><br>If configured correctly, a "Connection Successful" pop-up will appear.<br><br>If configured incorrectly, a "Connection Failed" pop-up will appear along with a link to the applicable error logs to help you troubleshoot.</td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The**[ **Schema**](../../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter                                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Example     |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| Name                                                   | **Mandatory.** The name of your column as it appears in the source.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Name        |
| Alias                                                  | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |             |
| Data Type                                              | **Mandatory.** The data type of the column values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Text        |
| Description                                            | **Optional.** You may choose to add a description to your column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |             |
| <p>Parse Content By<br>(Only for Standard Columns)</p> | <p></p><p>Binary File sources have a unique, mandatory parameter for Standard Columns:</p><p><br><strong>Parse Content By</strong> - Choose from the following three options to define how you want to parse your content:</p><ul><li><strong>Byte Length</strong> - The content length in number of bytes</li><li><strong>Trailing Byte Sequence</strong> - the trailing sequence in base64 that indicates the end of the field</li><li><strong>Succeeding Byte Sequence</strong> - the trailing sequence in base64 that indicates the start of the next field, and thus the end of this one.</li></ul> | Byte Length |



Select **Show Advanced** for more options for the Schema section.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  For Text data types, you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |
| Max Length      | **Optional if data type = text.**  For Text data types, you can choose whether to **trim the whitespace**._                                                                                                                                                                                                                                                                                                   |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

<div data-full-width="true">

<figure><img src="../../../.gitbook/assets/image (398).png" alt=""><figcaption><p>Image 2: Define your Source</p></figcaption></figure>

</div>

## Next steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ ](../../building-data-syncs/sync-actions.md)[Sync Actions.](../../building-data-syncs/sync-actions.md)
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Click **Jobs > Start a Job** to begin your sync.
