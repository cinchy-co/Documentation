# Batch Data Sync Example

## 1. Overview

This example will take you through the creation and execution of a batch data sync where data will be loaded into the Cinchy via a CSV. In this example, we will be loading information into the **People table** in Cinchy. This is a self-contained example and can be replicated in any Cinchy environment without dependencies.

**Example Use Case:** You have historically maintained a record of all of your employees in a spreadsheet. Knowing that this significantly hinders your data and data management capabilities, you want to sync your file into Cinchy. Once synced, you can manage your employee information through the Cinchy data browser, instead of through a data silo.

{% hint style="success" %}
You can review our documentation on Delimited File Sources[ here.](https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/delimited-file)
{% endhint %}

## 2. Sample Files and Code

This section contains:

* The People Table XML schema. Alternatively, this can be manually created through the Cinchy Data Browser
* A sample source CSV data file to load into Cinchy.

### 2.1 Cinchy Table XML

To create the People table used in this example, you can use the below is the XML  Alternatively, you can create the table manually using the steps in section 2.1.1.

```sql
<?xml version="1.0" encoding="utf-16"?>
<Model xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="People Model" version="1.0.0" xmlns="http://www.cinchy.co">
  <Schema>
    <Tables>
      <Table name="People" domain="Sandbox" guid="d5e6409c-471e-4a19-ba9c-df6ff2b606aa" icon="fa fa-table" iconColour="#002060">
        <Columns>
          <Text name="Name" guid="d37935e7-0ebb-4267-ad10-758067221951" allowLinking="false" />
          <Text name="Title" guid="afebfe66-1d59-445d-9373-0f8bc2a5e804" allowLinking="false" />
          <Text name="Company" guid="aaa5aeb5-4be2-4dc7-89a6-2237ae5b9c74" allowLinking="false" />
        </Columns>
        <UniqueConstraints />
      </Table>
    </Tables>
  </Schema>
</Model>
```

#### 2.1.1 Manual Table Creation

1. Log in to your Cinchy platform.
2. From under **My Network**, click the **create** button.
3. Select **Table.**
4. Select **From Scratch.**
5. Create a table with the following properties _(Image 1):_

| Table Details | Values                                                                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Table Name    | People                                                                                                                                      |
| Icon + Colour | Default                                                                                                                                     |
| Domain        | Sandbox _(if this domain does not exist please either create it or make sure to update this parameter where required during the data sync)_ |

<figure><img src="../../.gitbook/assets/image (447).png" alt=""><figcaption><p>Image 1: The People table</p></figcaption></figure>

6\.  Click **Columns** in the left hand navigation to create the columns for the table.

7\.  Click the **"Click Here to Add"** button and add the following columns.

| Column Details | Values                                            |
| -------------- | ------------------------------------------------- |
| Column 1       | <p>Column Name: Name</p><p>Data Type: Text</p>    |
| Column 2       | <p>Column Name: Title</p><p>Data Type: Text</p>   |
| Column 3       | <p>Column Name: Company</p><p>Data Type: Text</p> |

9\.  Click the **Save** button to save your table.

### 2.2 Data File&#x20;

The sample CSV file used in this example can be downloaded below.

{% file src="../../.gitbook/assets/People Load File.csv" %}
Sample People Load File
{% endfile %}

{% hint style="info" %}
Please note, if you are downloading this file to recreate this exercise the file path and the file name will need to be:

**C:\Data\contacts.csv**

Alternatively, you can update the `path` parameter in the data sync configuration to match the file path and name of your choosing.&#x20;
{% endhint %}

The source file contains the following information which will be synced into our target Cinchy table _(Image 2)._

<figure><img src="../../.gitbook/assets/image (421).png" alt=""><figcaption><p>Image 2: The source file.</p></figcaption></figure>

As we can see, the file has the following columns:

* First Name
* Last Name
* Email Address
* Title
* Company

The People table created only has the following columns:

* Name&#x20;
* Title
* Company&#x20;

When syncing the data from our source (CSV file) to our target (Cinchy People table) our batch data sync will need to take into consideration the following:

* The first and last name from the source will need to end up merged into one column in our target (Name)
* Email address from our sources is not a column in the target, therefore his column will not be synced into the target&#x20;
* The title column will be a one to one match from source to target
* The company column will also be a one to one match from source to target

## 3. Creating the Data Sync

There are two options when you want to create a data sync in Cinchy.&#x20;

* You can input all of your necessary information through the intuitive Connections UI. Once saved, all of this data is uploaded as an XML into the Data Sync configurations table.
* Or, you can bypass the UI and upload your XML config directly into the Data Sync configuration table yourself.

This example will walk you through option one.

### 3.1 Using the Connections UI

1. Within your Cinchy platform, navigate to the Connections Experience _(Image 3)._

<figure><img src="../../.gitbook/assets/image (477).png" alt=""><figcaption><p>Image 3: The Connections Experience</p></figcaption></figure>

2. In the **Info** tab, input the name of your data sync. For this example we are using **"Contact Import"** _(Image 4)._

<figure><img src="../../.gitbook/assets/image (754).png" alt=""><figcaption><p>Image 4: Name your data sync</p></figcaption></figure>

3. Since this is a local file upload, we also need to set a **Parameter**. This value will be referenced in the **"path"** value of the **Load Metadata** box in step 5. For this example, we will set it to **filepath** _(Image 5)._

<figure><img src="../../.gitbook/assets/image (492).png" alt=""><figcaption><p>Image 5: Set your Parameter</p></figcaption></figure>

3. Navigate to the **Source** tab. As a reminder, we are using the .CSV file you downloaded at the beginning of this example as our source.
4. Under **"Select a Source"**, choose **Delimited File** _(Image 6)._

<figure><img src="../../.gitbook/assets/image (498).png" alt=""><figcaption><p>Image 6: Select a Source</p></figcaption></figure>

5. The **"Load Metadata"** box will appear; this is where you will define some important values about your source needed for the data sync to execute. Using the below table as your guide, fill in your metadata parameters _(Image 7):_

| Parameter             | Description                                                                                                               | Example                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Source                | The source location of your file. This can be either Local, S3, or Azure Blob Storage.                                    | Local                                                                         |
| Delimiter             | The type of delimiter on your source file.                                                                                | Since our file is a CSV, the delimiter is a comma, and we uses the ',' value. |
| Text Qualifier        | A text qualifier is a character used to distinguish the point at which the contents of a text field should begin and end. | "\&quot;                                                                      |
| Header Rows to Ignore | The number of records from the top of the file to ignore before the data starts (includes column header).                 | 1                                                                             |
| Path                  | The path to your source file. In this case, it is the Parameter that was set in step 3.                                   | @filepath                                                                     |
| Choose File           | This option will appear once you've correctly set your Path value.                                                        | Upload the sample CSV for this example.                                       |

<figure><img src="../../.gitbook/assets/image (491).png" alt=""><figcaption><p>Image 7: Load Metadata</p></figcaption></figure>

6. Click **Load.**
7. In the **Available Columns** pop-up, select all of the columns that you want to import from the CSV. For this example, we will select them all (noting, however, that we will only map a few of them later) _(Image 8)._

<figure><img src="../../.gitbook/assets/image (501).png" alt=""><figcaption><p>Image 8: Available Columns</p></figcaption></figure>

8. Click **Load.**
9. Once you load your source, the schema section of the page will auto-populate with the columns that you selected in step 7 _(Image 9)._ Review the schema to ensure it has the correct Name and Data Type. You may also choose to set any Aliases or add a Description.

<figure><img src="../../.gitbook/assets/image (580).png" alt=""><figcaption><p>Image 9: Source Schema</p></figcaption></figure>

10. Navigate to the **Destination tab** and select **Cinchy Table** from the drop down.
11. In the **Load Metadata** pop-up, input the **Domain** and **Table** name for your destination. In this example, we are using the Sandbox domain and the People table _(Image 10)._
12. Click **Load.**

<figure><img src="../../.gitbook/assets/image (496).png" alt=""><figcaption><p>Image 10: Load Metadata</p></figcaption></figure>

12. &#x20;Select the columns that you wish to use in your data sync _(Image 11)_. These will be the columns that your source syncs to. In this example, we are using the Name, Title, and Company columns. _Note that you will have many Cinchy system table available to use as well._ Click **Load.**

<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption><p>Image 11: Select your columns</p></figcaption></figure>

13. The Connections experience will attempt to automatically map your source and destination columns based on matching names. In the below screenshot, it has been able to correctly match the "Company" and "Title" columns _(Image 12)_. The "Name" target column is not a 1:1 match for any of our source columns, so we must match that one manually.

<figure><img src="../../.gitbook/assets/image (504).png" alt=""><figcaption><p>Image 12: Column Mappings</p></figcaption></figure>

14. Select **"First Name"** from the Source Column drop down to finish mapping our data sync _(Image 13)._

<figure><img src="../../.gitbook/assets/image (401).png" alt=""><figcaption><p>Image 13: Column Mapping</p></figcaption></figure>

15. Navigate to the **Sync Actions tab**. There are two options for data syncs: **Full File and Delta.** In this example, select Full File.

{% hint style="info" %}
Full load processing means that the entire amount of data is imported iteratively the first time a data source is loaded into the data studio. Delta processing, on the other hand, means loading the data incrementally, loading the source data at specific pre-established intervals.

[Read more about Sync Behaviour types here.](broken-reference)
{% endhint %}

16. Set the following parameters _(Image 14):_

| Parameter                 | Description                                                                                                                                                                                                                      | Example |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Sync Key Column Reference | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. It is used to match data between the source and the target. This allows for updates to occur on changed records. | Name    |
| New Record Behaviour      | This defines what action is taken when a new record is found in the sync source. This can be either Insert or Ignore.                                                                                                            | Insert  |
| Dropped Record Behaviour  | <p>This defines what action is taken when a dropped record is found in the sync source.</p><p>This can be either Delete, Ignore, or Expire.</p>                                                                                  | Delete  |
| Changed Record Behaviour  | <p>This defines what action is taken when a changed record is found in the sync source.</p><p>This can be either Update, Ignore, or Conditional.</p>                                                                             | Update  |

<figure><img src="../../.gitbook/assets/image (410).png" alt=""><figcaption><p>Image 14: Sync Behaviour</p></figcaption></figure>

17. Navigate to the **Permissions tab**. Here you will define your group access controls for your data sync _(Image 15)_. You can set this how you like. For this example, we are giving all user access to Execute, Write, and Read our sync.

{% hint style="info" %}
Any groups given Admin Access will have the ability to Execute, Write, and Read the data sync.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (262).png" alt=""><figcaption><p>Image 15: Permissions</p></figcaption></figure>

18. Navigate to the **Jobs tab**. Here you will see a record of all successful or failed jobs for this data sync.
19. Select **"Start a Job"** _(Image 16)._

<figure><img src="../../.gitbook/assets/image (506).png" alt=""><figcaption><p>Image 16: Start your Job</p></figcaption></figure>

20. &#x20;**Load** your sample .CSV file in the pop-up window _(Image 17)._

<figure><img src="../../.gitbook/assets/image (701).png" alt=""><figcaption><p>Image 17: Load your CSV</p></figcaption></figure>

21. The job will commence. The Execution window that pops up will help you to verify that your data sync is progressing _(Image 18)._

<figure><img src="../../.gitbook/assets/image (749).png" alt=""><figcaption><p>Image 18: Job execution</p></figcaption></figure>

22. &#x20;Navigate to your destination table to ensure that your data populated correctly _(Image 19)._

<figure><img src="../../.gitbook/assets/image (435).png" alt=""><figcaption><p>Image 19: Confirming your changes</p></figcaption></figure>

### 3.2 Using a Data Sync XML

In lieu of using the Connections UI, you can also set up a data sync by uploading a correctly formatted XML into the Data Sync Configs table within Cinchy.

We recommend only doing so once you have a good grasp on how data sync work. Note that not all sources/targets follow the same XML pattern.

Below is the completed batch data sync configuration for this example. Review the XMLs and then proceed to section 3.2.3 for further instructions.

#### 3.2.1 Blank XML Example

The below XML shows what a blank data sync could look like for a Delimited File source to a Cinchy Table target.

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="" version="1.0.0" xmlns="http://www.cinchy.co">
    <Parameters>
        <Parameter name=""/>
    </Parameters>
    <DelimitedDataSource source="" path="" delimiter="" textQualifier="" headerRowsToIgnore="" encoding="" useHeaderRecord="">
        <Schema>
            <Column name="" dataType="" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </DelimitedDataSource>
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

#### 3.2.2 Filled XML Example

The below filled XML example matches the Connections UI configuration we made in step 3.1. You can review the parameters used in the table below.

| Parameter                      | Description                                                                                                                                                                                                                      | Example                                                                       |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Name                           | The name of your data sync.                                                                                                                                                                                                      | Contact Import                                                                |
| Parameter                      | Since this is a local file upload, we also need to set a **Parameter**. This value will be referenced in the **"path"** value of the **Load Metadata** box                                                                       | Parameter                                                                     |
| Source                         | Defines whether your source is Local (PATH), S3, or Azure.                                                                                                                                                                       | PATH                                                                          |
| Path                           | Since this is a local upload, this is the path to your source file. In this case, it is the value that was set for the "Parameter" value, preceded by the '@' sign.                                                              | @Parameter                                                                    |
| Delimiter                      | The type of delimiter on your source file.                                                                                                                                                                                       | Since our file is a CSV, the delimiter is a comma, and we uses the ',' value. |
| Text Qualifier                 | A text qualifier is a character used to distinguish the point at which the contents of a text field should begin and end.                                                                                                        | "\&quote;                                                                     |
| Header Rows to Ignore          | The number of records from the top of the file to ignore before the data starts (includes column header).                                                                                                                        | 1                                                                             |
| Column Name                    | The name(s) of the source columns that you wish to sync. Note that in this example we have selected more columns than we will map, in order to show how Connections ignores unmapped data.                                       | <p>"First Name"<br>"Last Name"<br>"Email Address:<br>"Title"<br>"Company"</p> |
| Column Data Type               | The data type that corresponds to our selected source columns.                                                                                                                                                                   | "Text"                                                                        |
| Domain                         | The domain of your Cinchy Target table.                                                                                                                                                                                          | Sandbox                                                                       |
| Table                          | The name of your Cinchy Target table.                                                                                                                                                                                            | People                                                                        |
| Column Mapping Source Column   | The name(s) of the source columns that you are syncing.                                                                                                                                                                          | <p>"Company"<br>"Title"<br>"First Name"</p>                                   |
| Column Mapping Target Column   | The name(s) of the target column as it maps to the specified source column.                                                                                                                                                      | <p>"Company"<br>"Title"<br>"Name"</p>                                         |
| Sync Key Column Reference Name | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. It is used to match data between the source and the target. This allows for updates to occur on changed records. | "Name"                                                                        |
| New Record Behaviour Type      | This defines what will happen when new records are found in the source.                                                                                                                                                          | INSERT                                                                        |
| Dropped Record Behaviour Type  | This defines what will happen when dropped records are found in the source.                                                                                                                                                      | DELETE                                                                        |
| Changed Record Behaviour Type  | This defines what will happen when changed records are found in the source.                                                                                                                                                      | UPDATE                                                                        |

```xml
<?xml version="1.0" encoding="utf-16"?>
<BatchDataSyncConfig name="Contact Import" version="1.0.0" xmlns="http://www.cinchy.co">
    <Parameters>
        <Parameter name="parameter"/>
    </Parameters>
    <DelimitedDataSource source="PATH" path="@parameter" delimiter="," textQualifier="&quot;" headerRowsToIgnore="1" encoding="UTF8" useHeaderRecord="false">
        <Schema>
            <Column name="First Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Last Name" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Email Address" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Title" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
            <Column name="Company" dataType="Text" trimWhitespace="true" isMandatory="false" validateData="false"/>
        </Schema>
    </DelimitedDataSource>
    <CinchyTableTarget reconcileData="true" domain="sandbox" table="People" suppressDuplicateErrors="false" degreeOfParallelism="1">
        <ColumnMappings>
            <ColumnMapping sourceColumn="Company" targetColumn="Company"/>
            <ColumnMapping sourceColumn="Title" targetColumn="Title"/>
            <ColumnMapping sourceColumn="First Name" targetColumn="Name"/>
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

#### 3.2.3 Using the Data Sync XML

1. Once you have completed your Data Sync XML, navigate to the Data Sync Configurations table in Cinchy _(Image 20)._

<figure><img src="../../.gitbook/assets/image (444).png" alt=""><figcaption><p>Image 20: Data Sync Configurations table</p></figcaption></figure>

2. In a new row, paste the **Data Sync XML** into the **Config XML** column _(Image 21)_.&#x20;
3. Define your group permissions in the applicable columns. In our example, we have given All Users the Admin Access_._

{% hint style="info" %}
The Name and Config Version columns will be auto populated as they values are coming from the Config XML.
{% endhint %}

{% hint style="success" %}
Tip: Click on the below image to enlarge it.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (648).png" alt=""><figcaption><p>Image 21: Config XML</p></figcaption></figure>

{% hint style="info" %}
Be sure when you are pasting into the Config XML column that you double click into the column before pasting, otherwise each line of the XML will appear as an individual record in the Data Sync Configurations table.
{% endhint %}

3. To execute your Data Sync you will use the CLI. If you do not have this downloaded, [refer to the documentation here.](broken-reference)
4. In this example we will be using the following Data Sync Commands, however, for the full list of commands click [here](https://app.gitbook.com/@cinchy/s/draft-data-sync/\~/drafts/-MEYg-7T93WGsJ-TwPLy/builder-guide/data-sync-commands).

<table><thead><tr><th width="199.33333333333331">Parameter</th><th width="241">Description</th><th>Example</th></tr></thead><tbody><tr><td>-s (server)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The full path to the Cinchy server without the protocol (e.g. cinchy.co/Cinchy).</td><td>"pilot.cinchy.co/Training/Cinchy/"</td></tr><tr><td>-u (userid)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The user id to login to Cinchy that has execution access to the data sync.</td><td>"admin"</td></tr><tr><td>-p (password)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The password of the above User ID parameter. This can optionally be encrypted. For a walkthrough on how to use the CLI to encrypt the password, refer to the Appendix section. </td><td>"DESuEGqfffsamx55yl256hjuPYxa4ncc+5+bLkoVIFpgs0Lq6hkcU="</td></tr><tr><td>-f (feed)</td><td><mark style="color:orange;"><strong>Required</strong>.</mark> The name of the Data Sync Configuration as defined in Cinchy</td><td>"Contact Import"</td></tr></tbody></table>

5. Launch Powershell and navigate to the Cinchy CLI directory.
6. Enter and execute the following into Powershell:

```
.\Cinchy.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Contact Import"
```

7. Once executed, navigate to your destination table to validate that your data synced correctly _(Image 22)._

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption><p>Image 22: Validate your sync</p></figcaption></figure>

## 4. Appendix

### 4.1 Password Encryption

To encrypt a password using Powershell, complete the following:

1. Launch Powershell and navigate to the Cinchy CLI directory (note, you can always type powershell in the windows explore path for the Cinchy CLI directory)
2. Enter the following into Powershell  `.\Cinchy.CLI.exe encrypt -t "password"`&#x20;
3. Hit enter to execute the command
4. Copy the password (e.g. notepad, Visual Studio Code, word etc.) so you have it accessible at batch execution time

{% hint style="info" %}
Please note, you will need to replace "password" with your specific password.
{% endhint %}

### 4.2 Execution Logs

The Execution Log table is a system table in Cinchy that logs the outputs of all data syncs _(Image 23_). You can always review the entries in this table for information on the progression of your syncs.

<figure><img src="../../.gitbook/assets/image (189).png" alt=""><figcaption><p>Image 23: Execution Log</p></figcaption></figure>

### 4.3 Execution Errors

The Execution Errors table is a system table in Cinchy that logs any errors that may occur in a data sync _(Image 24)_. Any data sync errors will also be logged in the temp directory outlined in the data sync execution command (e.g. `-d "C:\Cinchy\temp"`)

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption><p>Image 24: Execution Errors</p></figcaption></figure>
