# Real-Time Sync Example

## 1. Overview

This example will take you through the creation and execution of a real-time data sync where data will be synced between two Cinchy tables based on real-time changes.

**Use Case:** Your **People table** captures a view of various personnel information. Any time a new hire is added to the table, you want that information to be immediately synced into the **New Employees** table. We can solve this use case using the **Cinchy Change Data Capture (CDC)** function on our tables. This helps you to better keep track of all incoming people within your company.

**Example Use Case:**&#x20;

{% hint style="success" %}
You can review our documentation on Cinchy Table Sources[ here.](https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/delimited-file)\
\
You can review our documentation on Cinchy Table destinations here.
{% endhint %}

## 2. Sample Files and Code

This section contains steps on how to:

* Create the People table.
* Create the New Employees table.

{% hint style="warning" %}
When creating tables to be used with real-time syncs, you need to ensure that the **Cinchy Change Data Capture** feature is turned on through the **Design Table > Change Notifications tab**. This will ensure real-time updates are captured.
{% endhint %}

### 2.1 People Table Creation

1. Login to your Cinchy platform
2. From under **My Network,** click the **create** button
3. Select **Table**
4. Select **From Scratch**
5. Create a table with the following properties _(Image 1):_

| Table Details | Values                                                                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Table Name    | People                                                                                                                                      |
| Icon + Colour | Default                                                                                                                                     |
| Domain        | Sandbox _(if this domain does not exist please either create it or make sure to update this parameter where required during the data sync)_ |

<figure><img src="../../.gitbook/assets/image (300).png" alt=""><figcaption><p>Image 1: The People table</p></figcaption></figure>

6\.  Click **Columns** in the left hand navigation to create the columns for the table

7\.  Click the **"Click Here to Add"** button and add the following columns:

| Column Details | Values                                                 |
| -------------- | ------------------------------------------------------ |
| Column 1       | <p>Column Name: Name</p><p>Data Type: Text</p>         |
| Column 2       | <p>Column Name: Title</p><p>Data Type: Text</p>        |
| Column 3       | <p>Column Name: Phone Number</p><p>Data Type: Text</p> |
| Column 4       | <p>Column Name: City<br>Data Type: Text</p>            |

8. Click **Change Notifications** in the left hand navigation and ensure **"Publish Change Notifications"** is checked.
9. Click the **Save** button to save your table.

### 2.2 New Employees Table Creation

1. Within the Cinchy platform, from under **My Network**, click the **create** button
2. Select **Table**
3. Select F**rom Scratch**
4. Create a table with the following properties _(Image 2):_

| Table Details | Values                                                                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Table Name    | New Employees                                                                                                                               |
| Icon + Colour | Default                                                                                                                                     |
| Domain        | Sandbox _(if this domain does not exist please either create it or make sure to update this parameter where required during the data sync)_ |

<figure><img src="../../.gitbook/assets/image (653).png" alt=""><figcaption><p>Image 2: New Employees Table</p></figcaption></figure>

6\.  Click **Columns** in the left hand navigation to create the columns for the table.

7\.  Click the **"Click Here to Add"** button and add the following columns:

| Column Details | Values                                          |
| -------------- | ----------------------------------------------- |
| Column 1       | <p>Column Name: Name</p><p>Data Type: Text</p>  |
| Column 3       | <p>Column Name: Title</p><p>Data Type: Text</p> |

9. Click the **Save** button to save your table.

## 3. Creating the Data Sync

There are two options when you want to create a data sync in Cinchy.&#x20;

* You can input all of your necessary information through the intuitive **Connections UI.** Once saved, all of this data is uploaded as an XML into the Data Sync configurations table.
* Or, you can bypass the UI and **upload your XML config directly** into the Data Sync configuration table yourself.

This example will walk you through both options.

### 3.1 Using the Connections UI

1. Within your Cinchy platform, navigate to the **Connections Experience** _(Image 3)._

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption><p>Image 3: The Connections Experience</p></figcaption></figure>

2. In the **Info** tab, input the name of your data sync. For this example we are using **"New Hires"** _(Image 4)._

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption><p>Image 4: The Info Tab</p></figcaption></figure>

3. Navigate to the **Source** tab.
4. Under **"Select a Source"**, choose "Cinchy Event Broker" _(Image 5)._&#x20;

<figure><img src="../../.gitbook/assets/image (759).png" alt=""><figcaption><p>Image 5: Select your Source</p></figcaption></figure>

5. In the **Schema** section, input the columns from your source table (in this case the People table) that you will want to use in your data sync. In this example we are using the **Name** and **Title** columns, both of which are data type **Text** _(Image 6)_. You may also choose to set any Aliases or add a Description.

{% hint style="info" %}
We are not using the RunQuery functionality in this example but you can review it's properties here.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption><p>Image 6: Define your source schema</p></figcaption></figure>

6. Navigate to the **Destination** tab and select **Cinchy Table** from the drop down _(Image 7)._
7. In the **Load Metadata** pop-up, input the **Domain** and **Table** name for your destination. In this example, we are using the **Sandbox** domain and the **People** table.
8. Click **Load.**

<figure><img src="../../.gitbook/assets/image (512).png" alt=""><figcaption><p>Image 7: Load the Metadata</p></figcaption></figure>

12. &#x20;Select the columns that you wish to use in your data sync _(Image 8)._ These will be the columns that your source syncs to your target. In this example, we are using the **Name** and **Title** columns. Note that you will have many Cinchy system table available to use as well.&#x20;
13. &#x20;Click **Load.**

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption><p>Image 8: Select your columns</p></figcaption></figure>

14. The Connections experience will attempt to automatically map your source and destination columns based on matching names. In the below screenshot, it has been able to correctly match the **Name** and **Title** columns _(Image 9)._

<figure><img src="../../.gitbook/assets/image (640).png" alt=""><figcaption><p>Image 9: Map your columns</p></figcaption></figure>

15. Navigate to the **Sync Actions** tab. There are two options for data syncs: Full File and Delta. In this example, select **Full File.**

{% hint style="info" %}
Full load processing means that the entire amount of data is imported iteratively the first time a data source is loaded into the data studio. Delta processing, on the other hand, means loading the data incrementally, loading the source data at specific pre-established intervals.

**Read more about** [**Sync Behaviour types here.**](broken-reference)
{% endhint %}

16. Set the following parameters _(Image 10):_

| Parameter                 | Description                                                                                                                                                                                                                      | Example |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Sync Key Column Reference | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. It is used to match data between the source and the target. This allows for updates to occur on changed records. | Name    |
| New Record Behaviour      | This defines what action is taken when a new record is found in the sync source. This can be either Insert or Ignore.                                                                                                            | Insert  |
| Dropped Record Behaviour  | <p>This defines what action is taken when a dropped record is found in the sync source.</p><p>This can be either Delete, Ignore, or Expire.</p>                                                                                  | Delete  |
| Changed Record Behaviour  | <p>This defines what action is taken when a changed record is found in the sync source.</p><p>This can be either Update, Ignore, or Conditional.</p>                                                                             | Update  |

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption><p>Image 10: Sync Behaviour</p></figcaption></figure>

17. Navigate to the **Permissions** tab. Here you will define your group access controls for your data sync. You can set this how you like. For this example, we are giving **all users** access to Execute, Write, and Read our sync _(Image 11)._

{% hint style="info" %}
Any groups given Admin Access will have the ability to Execute, Write, and Read the data sync.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (535).png" alt=""><figcaption><p>Image 11: Sync Permissions</p></figcaption></figure>

18. Click **Save.**
19. [Navigate to the Cinchy Listener Config table and set up your configuration. ](real-time-sync-example.md#setting-the-listener-config)Ensure it is set to Enabled. Your real-time data sync should now be listening to your People table ready to push updates to your New Employees table.
20. &#x20;Test your data sync by adding a new row to your People table. Ensure that the data is then updated across to the New Employees table _(Images 12 & 13)._

<figure><img src="../../.gitbook/assets/image (180).png" alt=""><figcaption><p>Image 12: Test your Sync</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (490).png" alt=""><figcaption><p>Image 13: Test your Sync</p></figcaption></figure>

### 3.2 Using a Data Sync XML

In lieu of using the Connections UI, you can also set up a data sync by uploading a correctly formatted XML into the Data Sync Configs table within Cinchy.

We recommend only doing so once you have a good grasp on how data sync work. Note that not all sources/targets follow the same XML pattern.

Below is the completed batch data sync configuration for this example. Review the XMLs and then proceed to section 3.2.3 for further instructions.

#### 3.2.1 Blank XML Example

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
</BatchDataSyncConfig
```

#### 3.2.2 Filled XML Example

The below filled XML example matches the Connections UI configuration we made in step 3.1. You can review the parameters used in the table below.

| Parameter                      | Description                                                                                                                                                                                                                      | Example                  |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| Name                           | The name of your data sync.                                                                                                                                                                                                      | New Hites                |
| Column Name                    | The name(s) of the source columns that you wish to sync.                                                                                                                                                                         | <p>"Name"<br>"Title"</p> |
| Column Data Type               | The data type that corresponds to our selected source columns.                                                                                                                                                                   | "Text"                   |
| Domain                         | The domain of your Cinchy Target table.                                                                                                                                                                                          | Sandbox                  |
| Table                          | The name of your Cinchy Target table.                                                                                                                                                                                            | New Employees            |
| Column Mapping Source Column   | The name(s) of the source columns that you are syncing.                                                                                                                                                                          | <p>"Name"<br>"Title"</p> |
| Column Mapping Target Column   | The name(s) of the target column as it maps to the specified source column.                                                                                                                                                      | <p>"Name"<br>"Title"</p> |
| Sync Key Column Reference Name | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. It is used to match data between the source and the target. This allows for updates to occur on changed records. | "Name"                   |
| New Record Behaviour Type      | This defines what will happen when new records are found in the source.                                                                                                                                                          | INSERT                   |
| Dropped Record Behaviour Type  | This defines what will happen when dropped records are found in the source.                                                                                                                                                      | DELETE                   |
| Changed Record Behaviour Type  | This defines what will happen when changed records are found in the source.                                                                                                                                                      | UPDATE                   |

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
</BatchDataSyncConfig
```

#### 3.2.3 Using the Data Sync XML

1. Once you have completed your Data Sync XML, navigate to the **Data Sync Configurations** table in Cinchy _(Image 14)._

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption><p>Image 14: Data Sync Config Table</p></figcaption></figure>

2. In a new row, **paste** the Data Sync XML into the **Config XML column.**&#x20;
3. Define your group permissions in the applicable columns. In our example, we have given All Users the Admin Access.

{% hint style="info" %}
The Name and Config Version columns will be auto populated as they values are coming from the Config XML.
{% endhint %}

{% hint style="info" %}
Be sure when you are pasting into the Config XML column that you double click into the column before pasting, otherwise each line of the XML will appear as an individual record in the Data Sync Configurations table.
{% endhint %}

3. [Navigate to the Cinchy Listener Config table and set up your configuration.](real-time-sync-example.md#setting-the-listener-config) Ensure it is set to Enabled. Your real-time data sync should now be listening to your People table ready to push updates to your New Employees table.
4. To execute your Data Sync you will use the CLI. If you do not have this downloaded, [refer to the documentation here.](broken-reference)
5. In this example we will be using the following Data Sync Commands, however, for the full list of commands click [here](https://app.gitbook.com/@cinchy/s/draft-data-sync/\~/drafts/-MEYg-7T93WGsJ-TwPLy/builder-guide/data-sync-commands).

<table><thead><tr><th width="199.33333333333331">Parameter</th><th width="241">Description</th><th>Example</th></tr></thead><tbody><tr><td>-s (server)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The full path to the Cinchy server without the protocol (e.g. cinchy.co/Cinchy).</td><td>"pilot.cinchy.co/Training/Cinchy/"</td></tr><tr><td>-u (userid)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The user id to login to Cinchy that has execution access to the data sync.</td><td>"admin"</td></tr><tr><td>-p (password)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The password of the above User ID parameter. This can optionally be encrypted. For a walkthrough on how to use the CLI to encrypt the password, refer to the Appendix section. </td><td>"DESuEGqfffsamx55yl256hjuPYxa4ncc+5+bLkoVIFpgs0Lq6hkcU="</td></tr><tr><td>-f (feed)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The name of the Data Sync Configuration as defined in Cinchy</td><td>"Contact Import"</td></tr></tbody></table>

5. Launch Powershell and navigate to the Cinchy CLI directory.
6. Enter and execute the following into Powershell:

```
.\Cinchy.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Contact Import"
```

7. Test your data sync by adding a new row to your People table. Ensure that the data is then updated across to the New Employees table _(Images 15 & 16)._

<figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption><p>Image 15: Test your Sync</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption><p>Image 16: Test your Sync</p></figcaption></figure>

## 4. Setting the Listener Config

Setting up a Listener Configuration is a required step when doing a real-time data sync. While this example shows how to configure the sync using the Cinchy Event Broker/CDC, Cinchy also supports other Event Stream Sources. You can review the full list here.

1. Navigate to the **Listener Config table** in Cinchy _(Image 17)._

<figure><img src="../../.gitbook/assets/image (520).png" alt=""><figcaption><p>Image 17: Listener Config table</p></figcaption></figure>

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
</code></pre></td></tr><tr><td>Connection Attributes</td><td>This section is not required for data syncs using the Cinchy Event Broker/CDC, so we can just enter "{}"</td><td>{}</td></tr><tr><td>Status</td><td>This sets where your config is Enabled or Disabled. You can leave this blank until you want to turn on your config.</td><td></td></tr><tr><td>Data Sync Config</td><td>The name of the Data Sync Config you created in the Connections UI or via XML.</td><td>New Hires</td></tr><tr><td>Auto Offset Reset</td><td>In the case where the listener is started and either there is no last message ID, or when the last message ID is invalid (due to it being deleted or it's just a new listener), it will use this column as a fallback to determine where to start reading events from.<br><br><strong>Earliest</strong> will start reading from the beginning on the queue (when the CDC was enabled on the table). This might be a suggested configuration if your use case is recoverable or re-runnable and if you need to reprocess all events to ensure accuracy.<br><br><strong>Latest</strong> will fetch the last value after whatever was last processed This is the typical configuration.<br><br><strong>None</strong> will not read start reading any events.</td><td><strong>Latest</strong></td></tr></tbody></table>

#### 4.1 JSON Topic Column

For the topic JSON, you need to provide the following:

| Parameter  |                                                                                                                                                                                                                                                                                                                                              |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Table GUID | The GUID of the table whose notifications you wish to consume. You can find this in the **Design Table** screen for Cinchy v5.5+, and in the **Tables table** otherwise.                                                                                                                                                                     |
| Column(s)  | <p>The names of the columns you wish to include.<br><br><strong>Note:</strong> If you will be using the <a href="https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/cinchy-event-broker">runQuery=true</a> parameter in your data sync, you only need to include the Cinchy Id in the topic JSON. </p> |
| BatchSize  | The desired result batch size. This will **default to 1** if not passed in. The maximum batch size is 1000; using a number higher than that will result in a **Bad Request** response.                                                                                                                                                       |
| Filter     | Optional. When CDC is enabled, you can set a filter on columns where you are capturing changes in order to receive specific data.                                                                                                                                                                                                            |

#### 4.2 Topic JSON Example

```
{
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
```

## 5. Appendix

### 5.1 Password Encryption

Prior to executing the data sync command, let's encrypt the password using Powershell

To encrypt a password using Powershell, complete the following:

1. Launch Powershell and navigate to the Cinchy CLI directory (note, you can always type powershell in the windows explore path for the Cinchy CLI directory)
2. Enter the following into Powershell  `.\Cinchy.CLI.exe encrypt -t "password"`&#x20;
3. Hit enter to execute the command
4. Copy the password (e.g. notepad, Visual Studio Code, word etc.) so you have it accessible at batch execution time

{% hint style="info" %}
Please note, you will need to replace "password" with your specific password.
{% endhint %}

### 5.2 Execution Logs

The Execution Log table is a system table in Cinchy that logs the outputs of all data syncs. You can always review the entries in this table for information on the progression of your syncs _(Image 18)._

<figure><img src="../../.gitbook/assets/image (83).png" alt=""><figcaption><p>Image 18: Execution Logs</p></figcaption></figure>

### 5.3 Execution Errors

The Execution Errors table is a system table in Cinchy that logs any errors that may occur in a data sync _(Image 19)._ Any data sync errors will also be logged in the temp directory outlined in the data sync execution command (e.g. `-d "C:\Cinchy\temp"`)

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Image 19: Execution Errors table</p></figcaption></figure>
