# Batch data sync example

## Overview

This example will take you through the creation and execution of a batch data sync where data will be loaded into the Cinchy via a CSV. In this example, we will be loading information into the **People table** in Cinchy. This is a self-contained example you can recreate in any Cinchy environment without dependencies.

**Use Case:** You have historically maintained a record of all your employees in a spreadsheet. Knowing that this significantly hinders your data and data management capabilities, you want to sync your file into Cinchy. Once synced, you can manage your employee information through the Cinchy data browser, instead of through a data silo.

{% hint style="success" %}
For more information, see the documentation on [Delimited File Sources](https://cli.docs.cinchy.com/builder-guide/configuring-a-data-sync/supported-data-sources/delimited-file).
{% endhint %}

## Sample files and code

This section contains:

- The People Table XML schema.
- A sample source CSV data file to load into Cinchy.

### Cinchy Table XML

To create the People table used in this example, you can use the below is the XML. You can also create the table manually, as shown in the section below.

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

#### Manual table creation

1. Log in to your Cinchy platform.
2. From under **My Network**, click the **create** button.
3. Select **Table.**
4. Select **From Scratch.**
5. Create a table with the following properties _(Image 1):_

| Table Details | Values                                                                                                                               |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Table Name    | People                                                                                                                               |
| Icon + Colour | Default                                                                                                                              |
| Domain        | Sandbox _(if this domain doesn't exist, either create it or make sure to update this parameter where required during the data sync)_ |

<figure><img src="../../.gitbook/assets/image (447).png" alt=""><figcaption><p>Image 1: The People table</p></figcaption></figure>

6. Select **Columns** in the left hand navigation to create the columns for the table.

7. Select the **"Click Here to Add"** button and add the following columns:

| Column Details | Values                                            |
| -------------- | ------------------------------------------------- |
| Column 1       | <p>Column Name: Name</p><p>Data Type: Text</p>    |
| Column 2       | <p>Column Name: Title</p><p>Data Type: Text</p>   |
| Column 3       | <p>Column Name: Company</p><p>Data Type: Text</p> |

9. Select **Save** to save your table.

### Data file

You can download the sample CSV file used in this example below.

{% file src="../../.gitbook/assets/People Load File.csv" %}
Sample People Load File
{% endfile %}

{% hint style="info" %}
If you are downloading this file to recreate this exercise, the file path and the file name must be the following:

**C:\Data\contacts.csv**

You can also update the `path` parameter in the data sync configuration to match the file path and name of your choosing.
{% endhint %}

The source file contains the following information which will sync into the target Cinchy table _(Image 2)._

<figure><img src="../../.gitbook/assets/image (421).png" alt=""><figcaption><p>Image 2: The source file.</p></figcaption></figure>

As we can see, the file has the following columns:

- First Name
- Last Name
- Email Address
- Title
- Company

The People table created only has the following columns:

- Name
- Title
- Company

When syncing the data from the source (CSV file) to the target (Cinchy People table), the batch data sync must consider the following:

- The first and last name from the source must merge into one column in the target (**Name**).
- The email address from the sources isn't a column in the target, so this column won't sync into the target.
- The title column will be an exact match from source to target.
- The company column will also be an exact match from source to target.

## Create the data sync

You have two options when you create a data sync in Cinchy:

1. You can input all of your necessary information through the intuitive Connections UI. Once saved, all of this data is uploaded as an XML into the Data Sync configurations table.
2. You can bypass the UI and upload your XML config directly into the Data Sync configuration table.

This example will walk you through option one.

### Use the Connections UI

1. Within your Cinchy platform, navigate to the Connections Experience _(Image 3)._

<figure><img src="../../.gitbook/assets/image (477).png" alt=""><figcaption><p>Image 3: The Connections Experience</p></figcaption></figure>

2. In the **Info** tab, input the name of your data sync. This example uses **"Contact Import"** _(Image 4)._

<figure><img src="../../.gitbook/assets/image (754).png" alt=""><figcaption><p>Image 4: Name your data sync</p></figcaption></figure>

3. Since this is a local file upload, we also need to set a **Parameter**. This value will be referenced in the **"path"** value of the **Load Metadata** box in step 5. For this example, we will set it to `filepath` _(Image 5)._

<figure><img src="../../.gitbook/assets/image (492).png" alt=""><figcaption><p>Image 5: Set your Parameter</p></figcaption></figure>

3. Navigate to the **Source** tab. This example uses the .CSV file you downloaded at the beginning of this example as our source.
4. Under **Select a Source**, select **Delimited File** _(Image 6)._

<figure><img src="../../.gitbook/assets/image (498).png" alt=""><figcaption><p>Image 6: Select a Source</p></figcaption></figure>

5. The **"Load Metadata"** box will appear; this is where you will define some important values about your source needed for the data sync to execute. Using the below table as your guide, fill in your metadata parameters _(Image 7):_

| Parameter             | Description                                                                                                               | Example                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Source                | The source location of your file. This can be either Local, S3, or Azure Blob Storage.                                    | Local                                                                         |
| Delimiter             | The type of delimiter on your source file.                                                                                | Since our file is a CSV, the delimiter is a comma, and we uses the ',' value. |
| Text Qualifier        | A text qualifier is a character used to distinguish the point at which the contents of a text field should begin and end. | "\&quot;                                                                      |
| Header Rows to Ignore | The number of records from the top of the file to ignore before the data starts (includes column header).                 | 1                                                                             |
| Path                  | The path to your source file (See step 3).                                                                                | @filepath                                                                     |
| Choose File           | This option will appear once you've correctly set your Path value.                                                        | Upload the sample CSV for this example.                                       |

<figure><img src="../../.gitbook/assets/image (491).png" alt=""><figcaption><p>Image 7: Load Metadata</p></figcaption></figure>

6. Click **Load.**
7. In the **Available Columns** pop-up, select all of the columns that you want to import from the CSV. For this example, we will select them all (noting, however, that we will only map a few of them later) _(Image 8)._

<figure><img src="../../.gitbook/assets/image (501).png" alt=""><figcaption><p>Image 8: Available Columns</p></figcaption></figure>

8. Click **Load.**
9. Once you load your source, the schema section of the page will auto populate with the columns that you selected in step 7 _(Image 9)._ Review the schema to ensure it has the correct Name and Data Type. You may also choose to set any Aliases or add a Description.

<figure><img src="../../.gitbook/assets/image (580).png" alt=""><figcaption><p>Image 9: Source Schema</p></figcaption></figure>

10. Navigate to the **Destination tab** and select **Cinchy Table** from the drop down.
11. In the **Load Metadata** pop-up, input the **Domain** and **Table** name for your destination. This example uses the `Sandbox` domain and the `People` table _(Image 10)._
12. Select **Load Metadata**.

<figure><img src="../../.gitbook/assets/image (496).png" alt=""><figcaption><p>Image 10: Load Metadata</p></figcaption></figure>

12. Select the columns that you wish to use in your data sync _(Image 11)_. These will be the columns that your source syncs to. This example uses the Name, Title, and Company columns. _Note that you will have many Cinchy system table available to use as well._ Click **Load.**

<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption><p>Image 11: Select your columns</p></figcaption></figure>

1.  The Connections experience will attempt to automatically map your source and destination columns based on matching names. In the below screenshot, it matched the "Company" and "Title" columns _(Image 12)_. The "Name" target column isn't an exact match for any of the source columns, so you must match that one manually.

<figure><img src="../../.gitbook/assets/image (504).png" alt=""><figcaption><p>Image 12: Column Mappings</p></figcaption></figure>

14. Select **"First Name"** from the Source Column drop down to finish mapping our data sync _(Image 13)._

<figure><img src="../../.gitbook/assets/image (401).png" alt=""><figcaption><p>Image 13: Column Mapping</p></figcaption></figure>

15. Navigate to the **Sync Actions tab**. Sync actions have two options: **Full File and Delta.** In this example, select **Full File**.

{% hint style="info" %}
Full load processing means that the entire amount of data is imported iteratively the first time a data source is loaded into the data studio. Delta processing means loading the data incrementally, loading the source data at specific pre-established intervals.
{% endhint %}

1.  Set the following parameters _(Image 14):_

| Parameter                 | Description                                                                                                                                                                                                            | Example |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| Sync Key Column Reference | The sync key is a unique key reference when syncing the data from the data source into the Cinchy table. Use this to match data between the source and the target. This allows for updates to occur on changed records. | Name    |
| New Record Behaviour      | This defines the action taken when a new record is found in the sync source. This can be either Insert or Ignore.                                                                                                      | Insert  |
| Dropped Record Behaviour  | <p>This defines the action taken when a dropped record is found in the sync source.</p><p>This can be either Delete, Ignore, or Expire.</p>                                                                            | Delete  |
| Changed Record Behaviour  | <p>This defines the action taken when a changed record is found in the sync source.</p><p>This can be either Update, Ignore, or Conditional.</p>                                                                       | Update  |

<figure><img src="../../.gitbook/assets/image (410).png" alt=""><figcaption><p>Image 14: Sync Behaviour</p></figcaption></figure>

1.  Navigate to the **Permissions tab**. Here you will define your group access controls for your data sync _(Image 15)_. You can set this how you like. This example gives all users access to Execute, Write, and Read our sync.

{% hint style="info" %}
Any groups given Admin Access will have the ability to Execute, Write, and Read the data sync.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (262).png" alt=""><figcaption><p>Image 15: Permissions</p></figcaption></figure>

18. Navigate to the **Jobs tab**. Here you will see a record of all successful or failed jobs for this data sync.
19. Select **"Start a Job"** _(Image 16)._

<figure><img src="../../.gitbook/assets/image (506).png" alt=""><figcaption><p>Image 16: Start your Job</p></figcaption></figure>

20. **Load** your sample .CSV file in the pop-up window _(Image 17)._

<figure><img src="../../.gitbook/assets/image (701).png" alt=""><figcaption><p>Image 17: Load your CSV</p></figcaption></figure>

21. The job will commence. The Execution window that pops up will help you to verify that your data sync is progressing _(Image 18)._

<figure><img src="../../.gitbook/assets/image (749).png" alt=""><figcaption><p>Image 18: Job execution</p></figcaption></figure>

22. Navigate to your destination table to ensure that your data populated correctly _(Image 19)._

<figure><img src="../../.gitbook/assets/image (435).png" alt=""><figcaption><p>Image 19: Confirming your changes</p></figcaption></figure>

### Use a data sync XML

Instead of the Connections UI, you can also set up a data sync by uploading a formatted XML into the **Data Sync Configs** table within Cinchy.

We recommend only doing so once you have an understanding of how data syncs work. Not all sources/targets follow the same XML pattern.

The example below is the completed batch data sync configuration. Review the XML and then refer to the filled XML example.

#### Blank XML example

The below XML shows a blank data sync for a Delimited File source to a Cinchy Table target.

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

#### Filled XML example

The below filled XML example matches the Connections UI configuration made in **Use the Connections UI**. You can review the parameters used in the table below.

| Parameter                      | Description                                                                                                                                                                                                                  | Example                                                                       |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Name                           | The name of your data sync.                                                                                                                                                                                                  | Contact Import                                                                |
| Parameter                      | Since this is a local file upload, we also need to set a **Parameter**. This value will be referenced in the **"path"** value of the **Load Metadata** box                                                                   | Parameter                                                                     |
| Source                         | Defines whether your source is Local (PATH), S3, or Azure.                                                                                                                                                                   | PATH                                                                          |
| Path                           | Since this is a local upload, this is the path to your source file. In this case, it's the value that was set for the "Parameter" value, preceded by the '@' sign.                                                           | @Parameter                                                                    |
| Delimiter                      | The delimiter type on your source file.                                                                                                                                                                                      | Since our file is a CSV, the delimiter is a comma, and we uses the ',' value. |
| Text Qualifier                 | A text qualifier is a character used to distinguish the point at which the contents of a text field should begin and end.                                                                                                    | "\&quote;                                                                     |
| Header Rows to Ignore          | The number of records from the top of the file to ignore before the data starts (includes column header).                                                                                                                    | 1                                                                             |
| Column Name                    | The name(s) of the source columns that you wish to sync. In this example there are more selected columns than mapped to show how Connections ignores unmapped data.                                                          | <p>"First Name"<br>"Last Name"<br>"Email Address:<br>"Title"<br>"Company"</p> |
| Column Data Type               | The data type that corresponds to our selected source columns.                                                                                                                                                               | "Text"                                                                        |
| Domain                         | The domain of your Cinchy Target table.                                                                                                                                                                                      | Sandbox                                                                       |
| Table                          | The name of your Cinchy Target table.                                                                                                                                                                                        | People                                                                        |
| Column Mapping Source Column   | The name(s) of the source columns that you are syncing.                                                                                                                                                                      | <p>"Company"<br>"Title"<br>"First Name"</p>                                   |
| Column Mapping Target Column   | The name(s) of the target column as it maps to the specified source column.                                                                                                                                                  | <p>"Company"<br>"Title"<br>"Name"</p>                                         |
| Sync Key Column Reference Name | The SyncKey is used as a unique key reference when syncing the data from the data source into the Cinchy table. Use it to match data between the source and the target. This allows for updates to occur on changed records. | "Name"                                                                        |
| New Record Behaviour Type      | This defines what will happen when new records are found in the source.                                                                                                                                                      | INSERT                                                                        |
| Dropped Record Behaviour Type  | This defines what will happen when dropped records are found in the source.                                                                                                                                                  | DELETE                                                                        |
| Changed Record Behaviour Type  | This defines what will happen when changed records are found in the source.                                                                                                                                                  | UPDATE                                                                        |

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

#### Using the data sync XML

1. Once you have completed your Data Sync XML, navigate to the Data Sync Configurations table in Cinchy _(Image 20)._

<figure><img src="../../.gitbook/assets/image (444).png" alt=""><figcaption><p>Image 20: Data Sync Configurations table</p></figcaption></figure>

2. In a new row, paste the **Data Sync XML** into the **Config XML** column _(Image 21)_.
3. Define your group permissions in the applicable columns. This example gives all Users the Admin Access*.*

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

3. To execute your Data Sync you will use the CLI. If you don't have this downloaded, [refer to the CLI commands list page.](../cli-commands-list.md)
4. In this example we will be using the following Data Sync Commands, however, for the full list of commands click [here](https://app.gitbook.com/@cinchy/s/draft-data-sync/~/drafts/-MEYg-7T93WGsJ-TwPLy/builder-guide/data-sync-commands).
   <!-- vale off -->
   | Parameter     | Description                                                                                                                                                                              | Example                                                  |
   | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
   | -s (server)   | Required. The full path to the Cinchy server without the protocol (cinchy.co/Cinchy).                                                                                                    | "pilot.cinchy.co/Training/Cinchy/"                       |
   | -u (user id)  | Required. The user id to login to Cinchy that has execution access to the data sync.                                                                                                     | "admin"                                                  |
   | -p (password) | Required. The password of the above User ID parameter. This can optionally be encrypted. For a walkthrough on how to use the CLI to encrypt the password, refer to the Appendix section. | "DESuEGqfffsamx55yl256hjuPYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" |
   | -f (feed)     | Required. The name of the Data Sync Configuration as defined in Cinchy                                                                                                                   | "Contact Import"                                         |
   <!-- vale off -->
5. Launch PowerShell and navigate to the Cinchy CLI directory.
6. Enter and execute the following into PowerShell:

```
.\Cinchy.CLI.exe syncdata -s "pilot.cinchy.co/Training/Cinchy/" -u "admin" -p "DESuEGqmx55yl2PYxa4ncc+5+bLkoVIFpgs0Lq6hkcU=" -f "Contact Import"
```

7. Once executed, navigate to your destination table to validate that your data synced correctly _(Image 22)._

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption><p>Image 22: Validate your sync</p></figcaption></figure>

## Appendix

### Password encryption

To encrypt a password using PowerShell, complete the following:

1. Launch PowerShell and navigate to the Cinchy CLI directory (note, you can always type `PowerShell` in the windows explore path for the Cinchy CLI directory)
2. Enter the following into PowerShell `.\Cinchy.CLI.exe encrypt -t "password"`
3. Hit enter to execute the command
4. Copy the password so it's accessible at batch execution time

{% hint style="info" %}
Please note, you will need to replace "password" with your specific password.
{% endhint %}

### Execution Log table

The Execution Log table is a system table in Cinchy that logs the outputs of all data syncs _(Image 23_). You can always review the entries in this table for information on the progression of your syncs.

<figure><img src="../../.gitbook/assets/image (189).png" alt=""><figcaption><p>Image 23: Execution Log</p></figcaption></figure>

### Execution Error table

The Execution Errors table is a system table in Cinchy that logs any errors that may occur in a data sync _(Image 24)_. Any data sync errors log to the temp directory outlined in the data sync execution command. For example, `-d "C:\Cinchy\temp"`.

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption><p>Image 24: Execution Errors</p></figcaption></figure>
