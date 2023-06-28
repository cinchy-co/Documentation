# Parquet

## 1. Overview

Apache Parquet is a file format designed to support fast data processing for complex data, with several notable characteristics:

**1.** **Columnar:** Unlike row-based formats such as CSV or Avro, Apache Parquet is column-oriented – meaning the values of each table column are stored next to each other, rather than those of each record:

**2. Open-source:** Parquet is free to use and open source under the Apache Hadoop license, and is compatible with most Hadoop data processing frameworks. To quote the [project website](https://parquet.apache.org/), “Apache Parquet is… available to any project… regardless of the choice of data processing framework, data model, or programming language.”

**3. Self-describing:** In addition to data, a Parquet file contains metadata including schema and structure. Each file stores both the data and the standards used for accessing each record – making it easier to decouple services that write, store, and read Parquet files.

**Example Use Case:** You have a parquet file that contains your Employee information. You want to use a batch sync to pull this info into a Cinchy table and liberate your data.

{% hint style="success" %}
The Parquet source supports **batch syncs.**
{% endhint %}

## 2. Info Tab

You can review the parameters that can be found in the info tab below _(Image 1)._

#### Values

| Parameter  | Description                                                                                                                                                   | Example                                               |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| Title      | **Mandatory.** Input a name for your data sync                                                                                                                | Employee Sync                                         |
| Version    | **Mandatory.** This is a pre-populated field containing a version number for your data sync. You can override it if you wish.                                 | 1.0.0                                                 |
| Parameters | **Optional.** Review our documentation on [Parameters here ](../../building-data-syncs/advanced-settings/parameters.md)for more information about this field. | Since we are doing a local upload, we use "@Filepath" |

<figure><img src="../../../.gitbook/assets/image (426).png" alt=""><figcaption><p>Image 1: The Info Tab</p></figcaption></figure>

## 3. Source Tab

The following table outlines the mandatory and optional parameters you will find on the Source tab _(Image 2)._

{% tabs %}
{% tab title="Source Details" %}
The following parameters will help to define your data sync source and how it functions.

<table><thead><tr><th>Parameter</th><th width="289.66666666666663">Description</th><th>Example</th></tr></thead><tbody><tr><td>(Sync) Source</td><td><strong>Mandatory.</strong> Select your source from the drop down menu.</td><td>Parquet</td></tr><tr><td>Source</td><td>The location of the source file. Either a <strong>Local upload, Amazon S3, or Azure Blob Stora</strong>ge<br><br>The following authentication methods are supported per source:<br><br><strong>Amazon S3:</strong> Access Key ID/Secret Access Key<br><br><strong>Azure Blob Storage:</strong> Connection String</td><td>Local</td></tr><tr><td>Row Group Size</td><td><strong>Mandatory.</strong> The size of your Parquer Row Groups. <a href="https://parquet.apache.org/docs/file-format/configurations/#row-group-size">Review the documentation here </a>for more on Row Group sizing.</td><td>The recommended disk block/row group/file size is 512 to 1024 MB on HDFS.</td></tr><tr><td>Path</td><td><strong>Mandatory.</strong> The path to the source file to load. To upload a local file, you must first insert a <strong>Parameter</strong> in the <strong>Info tab</strong> of the connection <em>(ex: filepath).</em> Then, you would reference that same value in this location <em>(Ex: @Filepath)</em>. This will then trigger a <strong>File Upload</strong> option to import your file.</td><td>@Filepath</td></tr><tr><td>Auth Type</td><td><p></p><p>This field defines the authentication type for your data sync. Cinchy supports "Access Key" and "IAM" role.<br><br>When selecting "Access Key", you must provide the key and key secret.<br><br>When selecting "IAM role", a new field will appear for you to paste in the role's Amazon Resource Name (ARN). You also must ensure that:</p><ul><li>The role <a href="https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/F1vvLbEMfTF1UqCFU9hs/~/changes/320/deployment-guide/deployment-installation-guides/kubernetes-deployment-installation/configuring-aws-iam-for-connections">must be configured</a> to have at least read access to the source</li><li>The Connections pods' role must <a href="https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/F1vvLbEMfTF1UqCFU9hs/~/changes/320/deployment-guide/deployment-installation-guides/kubernetes-deployment-installation/configuring-aws-iam-for-connections#1.2-authorizing-for-data-syncs">have permission to assume the role</a> specified in the data sync config</li></ul></td><td></td></tr></tbody></table>
{% endtab %}

{% tab title="Schema" %}
**The** [**Schema**](../../building-data-syncs/columns-and-mappings/#2.-schema-columns) **section** is where you define which source columns you want to sync in your connection. You can repeat the values for multiple columns.

| Parameter   | Description                                                                                                   | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------- | ------- |
| Name        | **Mandatory.** The name of your column as it appears in the source.                                           | Name    |
| Alias       | **Optional.** You may choose to use an alias on your column so that it has a different name in the data sync. |         |
| Data Type   | **Mandatory.** The data type of the column values.                                                            | Text    |
| Description | **Optional.** You may choose to add a description to your column.                                             |         |



There are other options available for the Schema section if you click on **Show Advanced.**

| Parameter       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Example |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Mandatory       | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Mandatory is checked</strong> on a column, then all rows are synced with the execution log status of failed, and the source error of <strong>"Mandatory Rule Violation"</strong></li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul> |         |
| Validate Data   | <ul><li><strong>If both Mandatory and Validated</strong> <strong>are checked</strong> on a column, then rows where the column is empty are rejected</li></ul><ul><li><strong>If just Validated is checked</strong> on a column, then all rows are synced.</li></ul>                                                                                                                                                                                                                   |         |
| Trim Whitespace | **Optional if data type = text.**  If your data type was chosen as "text", you can choose whether to **trim the whitespac**e _(that is, spaces and other non-printing characters)._                                                                                                                                                                                                                                                                                                   |         |

You can choose to add in a **Transformation > String Replacement** by inputting the following:

| Parameter   | Description                                                                                                                           | Example |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Pattern     | **Mandatory if using a Transformation.** The pattern for your string replacement, i.e. the string that will be searched and replaced. |         |
| Replacement | What you want to replace your pattern with.                                                                                           |         |

{% hint style="info" %}
Note that you can have more than one String Replacement
{% endhint %}
{% endtab %}

{% tab title="Filter" %}
You have the option to add a source filter to your data sync. Please review the documentation here for more information on [source filters.](../../building-data-syncs/advanced-settings/filters.md)
{% endtab %}
{% endtabs %}

## 4. Next Steps

* Configure your [Destination](../../supported-data-sync-destinations/)
* Define your[ Sync Behaviour](../../building-data-syncs/sync-behaviour.md).
* Add in your [Post Sync Scripts](../../building-data-syncs/advanced-settings/post-sync-scripts.md), if required.
* Define your [Permissions](../../building-data-syncs/#2.-create-a-data-sync-configuration).
* Click **Jobs > Start a Job** to begin your sync.
