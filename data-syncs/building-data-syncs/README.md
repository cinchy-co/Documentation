# Build data syncs

A data sync in Cinchy synchronizes information between a source and a destination. This guide walks you through the process of setting up your data syncs.

## Basic workflow

The basic workflow of a data sync in Cinchy is:

1. [Install the necessary components](../installation-and-maintenance/).
2. [Choose your data sync type](types-of-data-syncs.md).
3. [Choose your design pattern](common-design-patterns.md).
4. [Define and configure your data sync source](../supported-data-sync-sources/).
5. [Set up your data sync destination](../supported-data-sync-destinations/).
6. [Specify Sync Actions](sync-actions.md).

### Overview

You have two ways to set up a data sync in Cinchy:

1. Use the Connections UI to input and save configuration details. The data will be stored as an XML file in the **Data Sync Configurations** table.
2. Directly upload an XML config into the **Data Sync Configurations** table.

### Set up a data sync with the Connections UI

To use the Connections UI, open the **Connections Experience**.

<figure><img src="../../.gitbook/assets/image (413).png" alt=""><figcaption><p>Image 1: Cinchy's Connections Experience</p></figcaption></figure>

#### Connections UI workflow

The UI has six tabs. Each tab requires data for your connection setup:

1. Info
2. Source
3. Destination
4. Sync Actions
5. Post Sync (Optional)
6. Jobs (Optional)

<figure><img src="../../.gitbook/assets/image (389).png" alt=""><figcaption><p>Image 2: Overview of the Tabs</p></figcaption></figure>

#### **Info Tab**

The **Info Tab** has fundamental details about your data sync, such as its name and access controls. You must add a name and select an Admin Group. You can also [use variables](advanced-settings/variables.md) for advanced functionality.

<figure><img src="../../.gitbook/assets/image (680).png" alt=""><figcaption><p>Image 3: Working with the Info Tab</p></figcaption></figure>

#### **Source Tab**

The **Source Tab** defines the origin of your data sync. Each data source type, from specific file formats to integrated software systems, requires unique parameters. See the [Supported Data Sources list](../supported-data-sync-sources/) for available options.

<figure><img src="../../.gitbook/assets/image (741).png" alt=""><figcaption><p>Image 4: Setting up the Source Tab</p></figcaption></figure>

{% hint style="info" %}
When working with real-time sync sources, you'll notice an extra tab for Listener configuration. The adjustments you make here directly influence the **Listener Config** table. Navigate through the options and set up as needed. For more info, see the [Listener Config](../supported-real-time-sources/) and [Sync Source](../supported-data-sync-sources/) pages.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (693).png" alt=""><figcaption></figcaption></figure>

#### **Destination Tab**

The **Destination Tab** identifies where your data sync goes. Each destination comes with its own set of parameters. You must map each destination to its source. Consult the [Supported Destinations directory](../supported-data-sync-destinations/) for specifics.

<figure><img src="../../.gitbook/assets/image (493).png" alt=""><figcaption><p>Image 5: Setting the Destination Course</p></figcaption></figure>

#### **Sync Actions Tab**

In the **Sync Actions Tab**, you can choose your preferred data action. Your main options are **Full File Sync** and **Delta Sync**. Not sure about the differences? Check out this [comparison](sync-actions.md) for more details.

<figure><img src="../../.gitbook/assets/image (713).png" alt=""><figcaption><p>Image 6: Sync Action Choices</p></figcaption></figure>

#### **Post Sync Tab (Optional)**

In the **Post Sync Tab**, you can use [Cinchy Query Language (CQL)](../../cql/the-basics-of-cql/) to refine the post-sync data. For example, you could set up a post-sync script to push retrieved data values into a specific Cinchy table. You can find more on this in the [Post-sync scripts](advanced-settings/post-sync-scripts.md) page.

<figure><img src="../../.gitbook/assets/image (660).png" alt=""><figcaption><p>Image 7: Mastering Post Sync Operations</p></figcaption></figure>

#### **Jobs Tab (For Batch Data Syncs)**

In the **Jobs tab**, you can start, monitor, and troubleshoot batch jobs. You can also view sync outputs or download detailed logs for analysis. For non-default user operations, ensure you have the right credentials and permissions.

<figure><img src="../../.gitbook/assets/image (698).png" alt=""><figcaption><p>Image 8: Handling Batch Data Jobs</p></figcaption></figure>

#### **Execution Tab (For Real-time Syncs)**

Track potential issues with your real-time syncs. Syncs become operational once the Listener Config is enabled—no need for manual job starts.

<figure><img src="../../.gitbook/assets/image (678).png" alt=""><figcaption><p>Image 9: Keeping an Eye on Execution</p></figcaption></figure>

### Set up a data sync with Config XML

You can also set up a data sync in Cinchy by uploading a formatted XML into the **Data Sync Configurations** table. This method is only recommended for those with advanced knowledge in data sync operations.

{% hint style="warning" %}
Unique XML patterns may exist across different sources and targets. If you're unfamiliar with this process, check out the [Delimited File source to Cinchy Table batch data sync example](batch-data-sync-example.md) first.
{% endhint %}

1. **Access the Data Sync Config Table**: In the Cinchy platform, open the **Data Sync Configurations** table.

<figure><img src="../../.gitbook/assets/image (177).png" alt=""><figcaption><p>Image 10: Data Sync Configurations table</p></figcaption></figure>

2. **Insert Data Sync XML**: For a new row, double-click the **Config XML** column and paste your **Data Sync XML**.
3. **Define Group Permissions**: Adjust the required permissions in the appropriate columns.
4.  **Review the XML Data**: After finalizing your Data Sync XML, return to the **Data Sync Configurations** table.

    <figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption><p>Image 11: Config XML Entry</p></figcaption></figure>

{% hint style="info" %}
```
    The Config XML auto populates columns like Name and Config Version.
    
```
{% endhint %}

5. **Initiate Sync with the CLI**: If you haven't installed the CLI, refer to the [CLI installation guide](../installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md). Otherwise, launch PowerShell and navigate to the Cinchy CLI directory.
6. **Run the CLI Command**:

```bash
.\Cinchy.Connections.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Data Sync Name"
```

More details on CLI commands can be found in the [CLI commands list](../cli-commands-list.md).

### Set Up a listener config (real-time syncs)

If you are setting up a real-time sync, you must set up a listener configuration. You must configure your Event Stream Source with your data sync information. You can review an more on the [Listener Config here.](../supported-real-time-sources/the-listener-configuration-table.md)

1. Navigate to the **Listener Config table** in Cinchy _(Image 12)._

<figure><img src="../../.gitbook/assets/image (503).png" alt=""><figcaption><p>Image 12: Listener Config table</p></figcaption></figure>

2. In a new row, add in your listener config configuration data. See [Supported real-time sync sources](../supported-real-time-sources/the-listener-configuration-table.md) for more information.
3. Make sure to set the config to **Enabled**.

## Examples

The following pages show basic examples of both batch and real-time data syncs. Use these examples as a reference point for learning more about Cinchy data syncs.

* [Batch Data Sync Example](batch-data-sync-example.md)
* [Real-Time Data Sync Example](real-time-sync-example.md)
