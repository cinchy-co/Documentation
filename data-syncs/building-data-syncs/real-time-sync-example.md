# Real-time sync example

## Overview

This example will take you through the creation and execution of a real-time data sync where data will be synced between two Cinchy tables based on real-time changes.

## Use case

Your **People table** captures a view of various personnel information. Any time a new hire is added to the table, you want that information to be immediately synced into the **New Employees** table. We can solve this use case using the **Cinchy Change Data Capture (CDC)** function on our tables. This helps you to better keep track of all incoming people within your company.

{% hint style="success" %}
You can review our documentation on Cinchy Table Sources[ here.](https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/delimited-file)\
\
You can review our documentation on Cinchy Table destinations [here.](../supported-data-sync-destinations/)
{% endhint %}

## Sample files and code

This section has steps on how to:

- Create the People table.
- Create the New Employees table.

{% hint style="warning" %}
When you create tables to use with real-time syncs, make sure you turn on the **Cinchy Change Data Capture** feature through the **Design Table > Change Notifications tab**. This makes sure you capture real-time updates.
{% endhint %}

### People table creation

1. Login to your Cinchy platform.
1. From under **My Network,** click the **Create** > **Standard Table** > **From Scratch**.
1. Create a table with the following properties _(Image 1)_:

   | Table Details | Values  |
   | ------------- | ------- |
   | Table Name    | People  |
   | Icon + Colour | Default |
   | Domain        | Sandbox |

   {% hint style="info" %}
   If this domain doesn't exist, either create it or make sure to update this parameter where required during the data sync.
   {% endhint %}

   <figure><img src="../../.gitbook/assets/image (293).png" alt=""><figcaption><p>Image 1: The People table</p></figcaption></figure>

1. Click **Columns** in the left hand navigation to create the columns for the table.

1. Click the **"Click Here to Add"** button and add the following columns:

   | Column Details | Values                                                 |
   | -------------- | ------------------------------------------------------ |
   | Column 1       | <p>Column Name: Name</p><p>Data Type: Text</p>         |
   | Column 2       | <p>Column Name: Title</p><p>Data Type: Text</p>        |
   | Column 3       | <p>Column Name: Phone Number</p><p>Data Type: Text</p> |
   | Column 4       | <p>Column Name: City<br>Data Type: Text</p>            |

1. Select **Change Notifications** in the left hand navigation and select **Publish Change Notifications**.
1. Select **Save** to save your table.

### New Employees table creation

1. Within the Cinchy platform, from under **My Network**, select **Create**
1. Select **Table**
1. Select **From Scratch**
1. Create a table with the following properties _(Image 2):_

| Table Details | Values                                                                                                                               |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Table Name    | New Employees                                                                                                                        |
| Icon + Colour | Default                                                                                                                              |
| Domain        | Sandbox _(if this domain doesn't exist, either create it or make sure to update this parameter where required during the data sync)_ |

<figure><img src="../../.gitbook/assets/image (748).png" alt=""><figcaption><p>Image 2: New Employees Table</p></figcaption></figure>

6.  Select **Columns** in the left hand navigation to create the columns for the table.

7.  Select the **"Click Here to Add"** button and add the following columns:

| Column Details | Values                                          |
| -------------- | ----------------------------------------------- |
| Column 1       | <p>Column Name: Name</p><p>Data Type: Text</p>  |
| Column 3       | <p>Column Name: Title</p><p>Data Type: Text</p> |

9. Click the **Save** button to save your table.

## Create the data sync

You have two options when you create a data sync in Cinchy.

1. You can input all the necessary information through the Connections UI. Once saved, this data uploads as an XML file into the Data Sync configurations table.
2. You can bypass the UI and upload your XML config directly into the Data Sync configuration table yourself.

This example will walk you through both options.

### Use the Connections UI

1. Within your Cinchy platform, navigate to the **Connections Experience** _(Image 3)._

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption><p>Image 3: The Connections Experience</p></figcaption></figure>

1. In the **Info** tab, input the name of your data sync. This example uses **"New Hires"** _(Image 4)._

<figure><img src="../../.gitbook/assets/image (187).png" alt=""><figcaption><p>Image 4: The Info Tab</p></figcaption></figure>

#### Configure the source

{% hint style="success" %}
As of 5.7, you can now configure the Topic JSON in the **Source** tab under the **Listener** section. If you still need to manually configure the Topic JSON, see the appendix for more information.
{% endhint %}

1. Navigate to the **Source** tab.
1. Under **Select a Source**, select **Cinchy Event Broker** _(Image 5)_.
1. Under **Listener**, select the **People** table.
1. For **Return Columns**, select **All**.
   ![Image 5: Configure Topic JSON](../../.gitbook/assets/DataSyncs/Cinchy-CDC-Listener-Config.png)

#### Configure the destination

1. Navigate to the **Destination** tab and select **Cinchy Table** from the drop down _(Image 6)_.
1. In the **Connection** section, input the **Domain** and **Table** name for your destination. This example uses the **Sandbox** domain and the **People** table.
1. Click **Load.**

<figure><img src="../../.gitbook/assets/image (323).png" alt=""><figcaption><p>Image 6: Load the Metadata</p></figcaption></figure>

1. Select the columns that you wish to use in your data sync _(Image 8)._ These will be the columns that your source syncs to your target. This example uses the **Name** and **Title** columns. You also have many Cinchy system table available to use.
1. Click **Load.**

<figure><img src="../../.gitbook/assets/image (194).png" alt=""><figcaption><p>Image 7: Select your columns</p></figcaption></figure>

1. The Connections experience will try to automatically map your source and destination columns based on matching names. In the below screenshot, it has been able to correctly match the **Name** and **Title** columns _(Image 8)._

<figure><img src="../../.gitbook/assets/image (719).png" alt=""><figcaption><p>Image 8: Map your columns</p></figcaption></figure>

#### Sync actions

1.  Navigate to the **Sync Actions** tab. Two options are available for data syncs: Full File and Delta. For this example, select **Full File.**

    {% hint style="info" %}
    Full load processing means that the entire amount of data is imported iteratively the first time a data source is loaded into the data studio. Delta processing means loading the data incrementally (loading the source data at specific pre-established intervals).
    {% endhint %}

1.  Set the following parameters _(Image 9):_

    | Parameter                 | Description                                                                                                                                                                                                                     | Example |
    | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
    | Sync Key Column Reference | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. It's used to match data between the source and the target. This allows for updates to occur on changed records. | Name    |
    | New Record Behaviour      | This defines the action taken when a new record is found in the sync source. This can be either Insert or Ignore.                                                                                                               | Insert  |
    | Dropped Record Behaviour  | <p>This defines the action taken when a dropped record is found in the sync source.</p><p>This can be either Delete, Ignore, or Expire.</p>                                                                                     | Delete  |
    | Changed Record Behaviour  | <p>This defines the action taken when a changed record is found in the sync source.</p><p>This can be either Update, Ignore, or Conditional.</p>                                                                                | Update  |

    <figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption><p>Image 9: Sync Behaviour</p></figcaption></figure>

1.  Navigate to the **Permissions** tab. Here you will define your group access controls for your data sync. You can set this how you like. This example gives **all users** access to Execute, Write, and Read our sync _(Image 10)._

    {% hint style="info" %}
    Any groups given Admin Access will have the ability to Execute, Write, and Read the data sync.
    {% endhint %}

    <figure><img src="../../.gitbook/assets/image (346).png" alt=""><figcaption><p>Image 10: Sync Permissions</p></figcaption></figure>

1.  Click **Save.**
1.  [Navigate to the Cinchy Listener Config table and validate your configuration.](real-time-sync-example.md#setting-the-listener-config) Make sure it's set to **Enabled**. Your real-time data sync should now be listening to your **People** table ready to push updates to your **New Employees** table.
1.  Test your data sync by adding a new row to your People table. Ensure that the data is then updated across to the New Employees table _(Images 11 & 12)._

    <figure><img src="../../.gitbook/assets/image (637).png" alt=""><figcaption><p>Image 11: Test your Sync</p></figcaption></figure>

    <figure><img src="../../.gitbook/assets/image (301).png" alt=""><figcaption><p>Image 12: Test your Sync</p></figcaption></figure>

### Use a data sync XML

Instead of using the Connections UI, you can also set up a data sync by uploading a correctly formatted XML into the Data Sync Configs table within Cinchy.

We only recommend doing so once you have a good understanding of how data syncs work. Not all sources and targets follow the same XML pattern.

#### Blank XML example

The below XML shows what a blank data sync could look like for our Cinchy Event Broker/CDC to Cinchy Table real-time sync with full file synchronization.

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="" version="1.0.0" xmlns="http://www.cinchy.co">
    <CinchyEventBrokerDataSource runQuery="false">
        <Schema>
            <Column name="" dataType="" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </CinchyEventBrokerDataSource>
    <CinchyTableTarget reconcileData="true" domain="" table="" suppressDuplicateErrors="false" degreeOfParallelism="1">
        <ColumnMappings>
            <ColumnMapping sourceColumn="" targetColumn=""/>
        </ColumnMappings>
        <SyncKey readonly="false">
            <SyncKeyColumnReference name=""/>
        </SyncKey>
        <NewRecordBehaviour type=""/>
        <DroppedRecordBehaviour type=""/>
        <ChangedRecordBehaviour type=""/>
        <PostSyncScripts/>
    </CinchyTableTarget>
</BatchDataSyncConfig>
```

#### Filled XML example

The below filled XML example matches the Connections UI configuration made in **Use the Connections UI** . You can review the parameters used in the table below.

| Parameter                      | Description                                                                                                                                                                                                                     | Example                  |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| Name                           | The name of your data sync.                                                                                                                                                                                                     | New Hires                |
| Column Name                    | The name(s) of the source columns that you wish to sync.                                                                                                                                                                        | <p>"Name"<br>"Title"</p> |
| Column Data Type               | The data type that corresponds to our selected source columns.                                                                                                                                                                  | "Text"                   |
| Domain                         | The domain of your Cinchy Target table.                                                                                                                                                                                         | Sandbox                  |
| Table                          | The name of your Cinchy Target table.                                                                                                                                                                                           | New Employees            |
| Column Mapping Source Column   | The name(s) of the source columns that you are syncing.                                                                                                                                                                         | <p>"Name"<br>"Title"</p> |
| Column Mapping Target Column   | The name(s) of the target column as it maps to the specified source column.                                                                                                                                                     | <p>"Name"<br>"Title"</p> |
| Sync Key Column Reference Name | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. It's used to match data between the source and the target. This allows for updates to occur on changed records. | "Name"                   |
| New Record Behaviour Type      | This defines what will happen when new records are found in the source.                                                                                                                                                         | INSERT                   |
| Dropped Record Behaviour Type  | This defines what will happen when dropped records are found in the source.                                                                                                                                                     | DELETE                   |
| Changed Record Behaviour Type  | This defines what will happen when changed records are found in the source.                                                                                                                                                     | UPDATE                   |

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="New Hires" version="1.0.0" xmlns="http://www.cinchy.co">
    <CinchyEventBrokerDataSource runQuery="false">
        <Schema>
            <Column name="Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Title" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </CinchyEventBrokerDataSource>
    <CinchyTableTarget reconcileData="true" domain="sandbox" table="New Employees" suppressDuplicateErrors="false" degreeOfParallelism="1">
        <ColumnMappings>
            <ColumnMapping sourceColumn="Name" targetColumn="Name"/>
            <ColumnMapping sourceColumn="Title" targetColumn="Title"/>
        </ColumnMappings>
        <SyncKey readonly="false">
            <SyncKeyColumnReference name="Name"/>
        </SyncKey>
        <NewRecordBehaviour type="INSERT"/>
        <DroppedRecordBehaviour type="DELETE"/>
        <ChangedRecordBehaviour type="UPDATE"/>
        <PostSyncScripts/>
    </CinchyTableTarget>
</BatchDataSyncConfig>
```

#### Use the data sync XML

1. Once you have completed your Data Sync XML, navigate to the **Data Sync Configurations** table in Cinchy _(Image 13)_.

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption><p>Image 13: Data Sync Config Table</p></figcaption></figure>

2. In a new row, **paste** the Data Sync XML into the **Config XML column.**
3. Define your group permissions in the applicable columns. This example gives All Users the Admin Access.

{% hint style="info" %}
The **Name** and **Config Version** columns will be auto populated as they values are coming from the Config XML.
{% endhint %}

{% hint style="info" %}
Be sure when you are pasting into the Config XML column that you double click into the column before pasting, otherwise each line of the XML will appear as an individual record in the Data Sync Configurations table.
{% endhint %}

3. [Navigate to the Cinchy Listener Config table and set up your configuration.](real-time-sync-example.md#setting-the-listener-config) Ensure it's set to Enabled. Your real-time data sync should now be listening to your People table ready to push updates to your New Employees table.
4. To execute your Data Sync you will use the CLI. If you don't have this downloaded, [refer to the CLI commands list page](../cli-commands-list.md)
5. In this example we will be using the following Data Sync Commands, however, for the full list of commands click [here](https://app.gitbook.com/@cinchy/s/draft-data-sync/~/drafts/-MEYg-7T93WGsJ-TwPLy/builder-guide/data-sync-commands).

<!-- vale off -->

| Parameter     | Description                                                                                                                                                                              | Example                                                  |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| -s (server)   | Required. The full path to the Cinchy server without the protocol (cinchy.co/Cinchy).                                                                                                    | "pilot.cinchy.co/Training/Cinchy/"                       |
| -u (user id)  | Required. The user id to login to Cinchy that has execution access to the data sync.                                                                                                     | "admin"                                                  |
| -p (password) | Required. The password of the above User ID parameter. This can optionally be encrypted. For a walkthrough on how to use the CLI to encrypt the password, refer to the Appendix section. | "DESuEGqfffsamx55yl256hjuPYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" |
| -f (feed)     | Required. The name of the Data Sync Configuration as defined in Cinchy                                                                                                                   | "Contact Import"                                         |

<!-- vale on -->

5. Launch PowerShell and navigate to the Cinchy CLI directory.
6. Enter and execute the following into PowerShell:

```bash
.\Cinchy.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Contact Import"
```

7. Test your data sync by adding a new row to your People table. Ensure that the data is then updated across to the New Employees table _(Images 14 & 15)._

<figure><img src="../../.gitbook/assets/image (418).png" alt=""><figcaption><p>Image 14: Test your Sync</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption><p>Image 15: Test your Sync</p></figcaption></figure>

## Appendix

## Manually set the listener config

This section provides information on how to manually set up the listener config using the Listener Config table. While this example shows how to configure the sync using the Cinchy Event Broker/CDC, Cinchy also supports other Event Stream Sources. For more information, see [the supported real-time sync stream sources](/data-syncs/supported-real-time-sources/README.md).

1. Navigate to the **Listener Config table** in Cinchy _(Image 16)._

<figure><img src="../../.gitbook/assets/image (331).png" alt=""><figcaption><p>Image 16: Listener Config table</p></figcaption></figure>

2. In a new row, add in your listener config data using the below table as a guide:

<table><thead><tr><th width="192">Column</th><th width="249.33333333333331">Description</th><th>Example</th></tr></thead><tbody><tr><td>Name</td><td>The name of your Listener Config</td><td>New Hire Sync</td></tr><tr><td>Event Connector Type</td><td>Select from the drop-down list which event stream you are listening in on.</td><td>Cinchy CDC</td></tr><tr><td>Topic</td><td>This column expects a JSON value with certain specific information. Please review the Topic Column table below for details.</td><td><pre><code>{
    "tableGuid": "3daba5da-5e07-4d35-8d7c-d451a2c9068e",
    "fields": [
        {
            "column": "Name"
        },
        {
            "column": "Title"
        }
    ],
}
</code></pre></td></tr><tr><td>Connection Attributes</td><td>This section isn't required for data syncs using the Cinchy Event Broker/CDC, so we can just enter `{}`</td><td>{}</td></tr><tr><td>Status</td><td>This sets where your config is Enabled or Disabled. You can leave this blank until you want to turn on your config.</td><td></td></tr><tr><td>Data Sync Config</td><td>The name of the Data Sync Config you created in the Connections UI or via XML.</td><td>New Hires</td></tr><tr><td>Auto Offset Reset</td><td>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br><br><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed This is the typical configuration.<br><br><strong>None</strong> won't read start reading any events.</td><td><strong>Latest</strong></td></tr></tbody></table>

                                                                                    |

#### Topic JSON example

```json
{
  "tableGuid": "3daba5da-5e07-4d35-8d7c-d451a2c9068e",
  "fields": [
    {
      "column": "Name"
    },
    {
      "column": "Title"
    }
  ]
}
```

### Password Encryption

Before executing the data sync command, encrypt the password using PowerShell.

To encrypt a password using PowerShell, complete the following:

1. Launch PowerShell and navigate to the Cinchy CLI directory (note, you can always type PowerShell in the windows explore path for the Cinchy CLI directory).
2. Enter the following into PowerShell `.\Cinchy.CLI.exe encrypt -t "password"`.
3. Hit enter to execute the command.
4. Copy the password so you have it accessible at batch execution time.

{% hint style="info" %}
You will need to replace `password` with your specific password.
{% endhint %}

### Execution logs

The **Execution Log** table is a system table in Cinchy that logs the outputs of all data syncs. You can always review the entries in this table for information on the progression of your syncs _(Image 17)._

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption><p>Image 17: Execution Logs</p></figcaption></figure>

### Execution errors

The **Execution Errors** table is a system table in Cinchy that logs any errors that may occur in a data sync _(Image 18)._ Any data sync errors will also be logged in the temp directory outlined in the data sync execution command (`-d "C:\Cinchy\temp"`)

<figure><img src="../../.gitbook/assets/image (570).png" alt=""><figcaption><p>Image 18: Execution Errors table</p></figcaption></figure>
