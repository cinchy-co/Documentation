# Build Data Syncs in Cinchy

Data syncs in Cinchy allow you to synchronize data between different systems. This guide will walk you through the process of setting up your data syncs.

## Preparation Checklist

- [Install Necessary Components](link-to-installation-doc).
- [Choose Data Sync Type and Design Pattern](link-to-sync-type-pattern).
- [Define and Configure Data Sync Source](link-to-configure-source).
- [Set Up Data Sync Destination](link-to-setup-destination).
- [Specify Sync Actions](link-to-sync-actions).
- [Configure Event Stream Source (For Real-time Syncs)](link-to-event-stream).

### Overview

You have two ways to set up a data sync in Cinchy:

1. Use the Connections UI to input and save configuration details. The data will be stored as an XML file in the **Data Sync Configurations** table.
2. Directly upload an XML config into the **Data Sync Configurations** table.

For real-time syncs, you'll also need to set up a lister configuration in the **Listener Configuration** table.

### Use the Connections UI

To use the Connections UI, open the **Connections Experience**.
<figure><img src="../../.gitbook/assets/image (413).png" alt=""><figcaption>Image 1: Cinchy's Connections Experience</figcaption></figure>

#### Connections UI workflow
The UI has six tabs. Each tab requires data for your connection setup.
<figure><img src="../../.gitbook/assets/image (389).png" alt=""><figcaption>Image 2: Overview of the Tabs</figcaption></figure>

#### **Info Tab** 
The **Info Tab** has fundamental details about your data sync, such as its name and access controls. You must add a name and select an Admin Group. You can also [use variables(advanced-settings/variables.md)] for advanced functionality. [Learn about Variables](advanced-settings/variables.md).
<figure><img src="../../.gitbook/assets/image (680).png" alt=""><figcaption>Image 3: Working with the Info Tab</figcaption></figure>

#### **Source Tab** 
The **Source Tab** defines the origin of your data sync. Each data source type, from specific file formats to integrated software systems, require unique parameters. You can see the available options in the [Supported Data Sources list](../supported-data-sync-sources/).
<figure><img src="../../.gitbook/assets/image (741).png" alt=""><figcaption>Image 4: Setting up the Source Tab</figcaption></figure>

{% hint style="info" %}
When working with real-time sync sources, you'll notice an extra tab for Listener configurations. The adjustments you make here directly influence the **Listener Config** table. Navigate through the options and set up as needed. For more info, see the [Listener Config](../supported-real-time-sync-stream-sources/) and [Sync Source](../supported-data-sync-sources/) pages.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (693).png" alt=""><figcaption></figcaption></figure>

#### **Destination Tab** 
The **Destination Tab** where your data sync should point to. Each destination comes with its set of parameters. Ensure data is mapped correctly from the source. Consult the [Supported Destinations directory](../supported-data-sync-destinations/) for specifics.
<figure><img src="../../.gitbook/assets/image (493).png" alt=""><figcaption>Image 5: Setting the Destination Course</figcaption></figure>

#### **Sync Actions Tab** 
In the **Sync Actions Tab**, you can choose your preferred data action. Your main options are Full File Sync and Delta Sync. Not sure about the differences? Check out this [comparison](sync-actions.md).
<figure><img src="../../.gitbook/assets/image (713).png" alt=""><figcaption>Image 6: Sync Action Choices</figcaption></figure>

#### **Post Sync Tab (Optional)** 
In the **Post Sync Tab**, you can use [Cinchy Query Language (CQL)](/cql/the-basics-of-cql/README.md) to refine the post-sync data. For example, you could set up a post-sync script to push retrieved data values into a specific Cinchy table. You can find more on this in the [Post-sync scripts](advanced-settings/post-sync-scripts.md) page.
<figure><img src="../../.gitbook/assets/image (660).png" alt=""><figcaption>Image 7: Mastering Post Sync Operations</figcaption></figure>

#### **Jobs Tab (For Batch Data Syncs)** 
In the **Jobs tab**, you can start, monitor, and troubleshoot batch jobs. You can also view sync outputs or download detailed logs for analysis. For non-default user operations, ensure you have the right credentials and permissions.
<figure><img src="../../.gitbook/assets/image (698).png" alt=""><figcaption>Image 8: Handling Batch Data Jobs</figcaption></figure>

#### **Execution Tab (For Real-time Syncs)** 
Track potential issues with your real-time syncs. Syncs become operational once the Listener Config is enabled—no need for manual job starts.
<figure><img src="../../.gitbook/assets/image (678).png" alt=""><figcaption>Image 9: Keeping an Eye on Execution</figcaption></figure>

### Use a Config XML

You can also set up a data sync by uploading a correctly formatted XML into the Data Sync Configs table within Cinchy.

We only recommend doing so after you have a good understanding of how data syncs work. Not all sources and targets follow the same XML pattern. You can review a basic version that uses a Delimited File source into a Cinchy Table [in this batch data sync example.](batch-data-sync-example.md)

To set up a data sync using a config XML:

1. In the Cinchy platform, navigate to the Data Sync Config table _(Image 10)._

<figure><img src="../../.gitbook/assets/image (177).png" alt=""><figcaption><p>Image 10: Data Sync Configurations table</p></figcaption></figure>

2. In a new row, paste your **Data Sync XML** into the **Config XML** column.
3. Define your group permissions in the applicable columns.
4. Once you have completed your Data Sync XML, navigate to the Data Sync Configurations table in Cinchy _(Image 11)._

{% hint style="info" %}
The Name and Config Version columns will be auto populated as the values are coming from the Config XML.
{% endhint %}

{% hint style="success" %}
Tip: Click on the below image to enlarge it.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption><p>Image 11: Config XML</p></figcaption></figure>

{% hint style="info" %}
Be sure when you are pasting into the Config XML column that you double click into the column before pasting, otherwise each line of the XML will appear as an individual record in the Data Sync Configurations table.
{% endhint %}

1. Use the CLI to execute your data sync. If you don't have this downloaded, [refer to the CLI installation page](../installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)
2. In this example we will be using the following Data Sync Commands. See the [CLI commands list](../cli-commands-list.md) for more information.

<!-- vale off -->

| Parameter     | Description                                                                                                                                                                    | Example                                                  |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| -s (server)   | Required. The full path to the Cinchy server without the protocol (cinchy.co/Cinchy).                                                                                          | "pilot.cinchy.co/Training/Cinchy/"                       |
| -u (user id)  | Required. The user id to login to Cinchy that has execution access to the data sync.                                                                                           | "admin"                                                  |
| -p (password) | Required. The password of the above User ID parameter. This must be encrypted. For a walkthrough on how to use the CLI to encrypt the password, refer to the Appendix section. | "DESuEGqfffsamx55yl256hjuPYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" |
| -f (feed)     | Required. The name of the Data Sync Configuration as defined in Cinchy                                                                                                         | "Data Sync Name"                                         |

<!-- vale on -->

5. Launch PowerShell and navigate to the Cinchy CLI directory.
6. Enter and execute the following into PowerShell:

```
.\Cinchy.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Data Sync Name"
```

### Set Up a listener config (real-time syncs)

You must set up a listener configuration when doing a real-time data sync. You will configure your Event Stream Source with your data sync information. You can review an more on the [Listener Config here.](/data-syncs/)

1. Navigate to the **Listener Config table** in Cinchy _(Image 12)._

<figure><img src="../../.gitbook/assets/image (503).png" alt=""><figcaption><p>Image 12: Listener Config table</p></figcaption></figure>

2. In a new row, add in your listener config configuration data. [Review the documentation here](../supported-real-time-sync-stream-sources/the-listener-configuration-table.md) for more information.
3. Make sure to set the config to **Enabled**.

## Examples

The following pages provide basic examples of both batch and real-time data syncs. Use these examples as a reference point for learning more about Cinchy data syncs.

- [Batch Data Sync Example](batch-data-sync-example.md)
- [Real-Time Data Sync Example](real-time-sync-example.md)
