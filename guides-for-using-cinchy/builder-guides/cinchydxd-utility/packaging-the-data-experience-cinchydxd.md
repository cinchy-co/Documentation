---
description: >-
  This page outlines Step 2 of Deploying CinchyDXD: Packaging the Data
  Experience
---

# Packaging the Data Experience (CinchyDXD)

## 1. Download the CinchyDXD Utility

The CinchyDXD utility is used to take all of the components (e.g. tables, queries, views, formatting rules, UDF’s etc…) of a DX and package them up so they can be moved from one environment to another. &#x20;

{% hint style="danger" %}
**Remember that a**ll objects need to be created in one source environment (ex: DEV). From there, DXD will be used to push them into others (ex: SIT, UAT, Production).
{% endhint %}

The CinchyDXD utility is only required (made accessible) for the environment that is packing up the data experience.  It is not required for the destination (or target) environment.

For CinchyDXD to work, you must have CinchyCLI installed.  For further installation instructions please refer to CLI ([https://cli.docs.cinchy.com/](https://cli.docs.cinchy.com/)) documentation

To access the Data Experience Deployment utility please contact Cinchy support (support@cinchy.com).\


To download the Utility:

1. Login to Cinchy
2. Navigate to the **Releases Table**
3. Select the **Experience Deployment Utility View**
4. Locate and download the utility **(e.g. Cinchy DXD v1.3.1.zip)**

{% hint style="warning" %}
The CinchyDXD utility is only upwards compatible with Cinchy version 4.6+
{% endhint %}

5\. Unzip the utility and place the folder at any location on a computer that also has CinchyCLI installed

6\. Create a new folder in the same directory that will hold all of the DX exports generated (e.g. CinchyDXD\_Output) _(Image 1)._

![Image 1: Creating your new folder](<../../../.gitbook/assets/image (237).png>)

This folder will then hold all of your deployment packages.

7\. Launch a Powershell console window&#x20;

8\. From the console, navigate to the CinchyDXD directory _(Image 2 and 3)._

{% hint style="success" %}
From within your file explorer window (folder: Cinchy DXD v.X) type “Powershell” into the file path.  It will launch a Powershell window already at the folder path
{% endhint %}

![Image 2: Navigate to your directory](<../../../.gitbook/assets/image (591).png>)

![Image 3: Navigate to your directory](<../../../.gitbook/assets/image (275).png>)

## 2. One Time Setup: Powershell

There is a one-time powershell setup that is required when using CinchyDXD.

1. From your Powershell window type **cin**&#x20;
2. Hit Tab on your keyboard _(Image 4)._

![Image 4: Setting up](<../../../.gitbook/assets/image (201).png>)

3\. Hit **Enter** on your keyboard _(Image 5)._

![Image 5: Setting up, cont.](<../../../.gitbook/assets/image (323).png>)

**You will get an error message** (above) that _CinchyDXD.ps1 cannot be loaded because the running script is disabled._ &#x20;

To resolve this error:

4\. From your **start menu**, search for **Powershell** and select **Run as Administrator** _(Image 6)._

![Image 6: Run as administrator](<../../../.gitbook/assets/image (606).png>)

5\. When prompted _“if you want to allow this app to make changes on your device”,_ select Yes.

6\. In your Powershell Administrator window enter **Set-ExecutionPolicy RemoteSigned** _(Image 7)._

![Image 7: Set-ExecutionPolicy RemoteSigned ](<../../../.gitbook/assets/image (473).png>)

7\. Hit **Enter** on your keyboard _(Image 8)._

![Image 8: Hit Enter](<../../../.gitbook/assets/image (475).png>)

8\. When prompted with the _Execution Policy Changes_, enter **A for “Yes to All”**

9\. Close the Powershell Administrator window

10\. Navigate back to your Powershell window for the _CinchDXD v.X window_

11\. From your Powershell window type **cin**&#x20;

12\. Hit **Tab** and then **Enter** on your keyboard _(Image 9)._

![Image 9: Finishing your set up](<../../../.gitbook/assets/image (134).png>)

The basic CinchyDXD instructions will be displayed. You will be able to execute commands such as exporting and installing a Data Experience.

## 3. Cinchy DXD Tables Overview

There are four tables in Cinchy that are used for packing up and deploying a Data Experience _(Image 10)._

![Image 10: Data Experience tables](<../../../.gitbook/assets/image (474).png>)

{% hint style="info" %}
The Data Experience is defined and packed in what will be referred to moving forward as the “Source Environment”.  Where the environment that the Data Experience will be deployed to will be referenced to as the “Target Environment”.
{% endhint %}

1. **Data Experience Definition Table**: Where the data experience is defined (e.g. tables, queries, views, formatting rules, UDF’s etc.)
2. **Data Experience Reference Data Table:** Where we define any data that needs to move with the Data Experience for the experience to work (e.g. lookup values, static values that may need to exist in tables - it typically would not be the physical data itself)
3. **Data Experience Releases Table:** Once a Data Experience is exported, an entry is created in this table for the export containing:&#x20;
   * **Version Number**
   * **Release Binary** is the location where you can archive/backup your release history in Cinchy\
     Please Note: if you have your own release management system, you do have the option to opt out of archiving the releases in Cinchy and check the release into your own source control
   * **Release Name**
   * **Data Experience**
4. **Data Experience Release Artifact Table**: Stores all of the files that are part of the Data Experience package as individual records along with all of the binary for each record

### 3.1 Define the Data Experience

When setting up a Data Experience definition, you will need one definition for each Data Experience you wish to package and deploy to a given number of Target Environments.

1. Locate and open the **Data Experience Definitions** table _(Image 11)._

![Image 11: Data Experience Definitions table](<../../../.gitbook/assets/image (261).png>)

| Column                   | Definition                                                                                                                                                                                                                                                                       |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GUID                     | This value is calculated, please note this value will be required as one of your export parameters in Powershell                                                                                                                                                                 |
| Name                     | This is the Name of your Data Experience                                                                                                                                                                                                                                         |
| Tables                   | Select all tables that are part of the Data Experience                                                                                                                                                                                                                           |
| Integrated Clients       | Select any integrated clientes (e.g. Tableau, PowerBI, custom integrations) that are part of the Data Experience                                                                                                                                                                 |
| Data Sync Configurations | Select any data syncs (e.g. CLI’s experience needs to work) that are part of the Data Experience                                                                                                                                                                                 |
| Reference Data           | Select any reference data that is part of the Data Experience.  Please note that the setup of the reference data is done in the table called Data Experience Reference Data (see step 2 below for setup details)                                                                 |
| User Defined Functions   | Select any user defined functions (e.g. validate phone, validate email) that are part of the Data Experience                                                                                                                                                                     |
| Models                   | Select any custom models that override columns or tables in your Data Experience, if there are none - leave blank                                                                                                                                                                |
| Groups                   | Select any groups that are part of the Data Experience (when moving groups, it will also move all table access \[design] controls)                                                                                                                                               |
| System Colours           | Select a system colour (if defined) for the Data Experience                                                                                                                                                                                                                      |
| Saved Queries            | Select any queries that are part of the Data Experience                                                                                                                                                                                                                          |
| Applets                  | Select any appletes that are part of the Data Experience                                                                                                                                                                                                                         |
| Formatting Rules         | Select any formatting rules that are part of the Data Experience                                                                                                                                                                                                                 |
| Literal Groups           | Select any literals that are associated to the Data Experience (e.g. key values with English and French definitions)                                                                                                                                                             |
| Builder                  | Select the builder(s) who have permission to export the Data Experience                                                                                                                                                                                                          |
| Builder Groups           | <p>Select the builder group(s) that have permission to export the Data Experience<br></p><p>Note: Best Practice is to use a Group over a User.  Users within groups can fluctuate, where the Group (or Role) will remain.  This will require less maintenance moving forward</p> |
| Sync GUID                | Leave this column blank                                                                                                                                                                                                                                                          |

2\. Complete the following _(Image 12):_

| Column         | Value                            |
| -------------- | -------------------------------- |
| Name           | Currency Converter               |
| Tables         | Currency Exchange Rate (Sandbox) |
| Saved Queries  | Currency Converter               |
| Builder Groups | Currency Converters              |

![Image 12: Enter in information](<../../../.gitbook/assets/image (86).png>)

{% hint style="info" %}
If you make changes to the DX in the future, you are NOT required to build a new Data Experience Definition in this table, you will update the existing definition.  If you need to review what the definition looked like historically, you can view it via the Collaboration log.
{% endhint %}

### 3.2 Define the Reference Data

When setting up a **Data Experience Reference Data** definition, you will need one (1) definition for each Reference Data table you wish to package and deploy with your Data Experience to the Target Environment.

{% hint style="info" %}
This table set up will be similar to how you would set up a CLI.
{% endhint %}

1. Locate and open the **Data Experience Reference Data** table _(Image 13)._

![Image 13: Data Experience Reference Data table](<../../../.gitbook/assets/image (37).png>)

| Column                     | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name                       | This is the Name of your Reference Data Table, note this name can be anything and does not have to replicate the actual table name                                                                                                                                                                                                                                                                                                                                                           |
| Ordinal                    | The ordinal number assigned will identify the order in which the data is loaded and required based on dependencies within the data experience.  For example if you have tables that have hierarchies in them, you will need to load the parent records first and then load your child records which would then resolve any links in the table.                                                                                                                                               |
| Filter                     | This is where a WHERE clause would be required.  For example, if you have a table that has hierarchies, you would require two rows within the Data Experience Reference Data table.  One to load the parent data and one to load the children data.  In the parent record a filter WHERE clause would be needed to filter all parent records.  In the second record in the filter column a WHERE clause in another in the secord record that would be needed to filter the children records. |
| New Records                | Identify the behaviour of a new record (e.g. insert, update, delete, ignore)                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Change Records             | Identify the behaviour of a changed record  (e.g. insert, update, delete, ignore)                                                                                                                                                                                                                                                                                                                                                                                                            |
| Dropped Records            | Identify the behaviour of a dropped record  (e.g. insert, update, delete, ignore)                                                                                                                                                                                                                                                                                                                                                                                                            |
| Table                      | Identify the table that you are exporting data from                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Sync Key                   | Required (need definition)                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Expiration Timestamp Field | If Dropped Records is set to “Expire” then a timestamp column is required                                                                                                                                                                                                                                                                                                                                                                                                                    |

Based on the configuration set up in this table, Cinchy will export the data and create CSV and CLI files.&#x20;

Please note in this example we do not have Reference Data as part of our Data Experience.

## 4. Export the Data Experience

Using Powershell you will now export the Data Experience you have defined within Cinchy.

1. Launch **Powershell** and navigate to your **CinchyDXD folder** _(Image 14)._

![Image 14: Launch powershell](<../../../.gitbook/assets/image (470).png>)

{% hint style="info" %}
Reminder: you can launch Powershell right from your file explorer window in the CinchyDXD folder by entering in the folder path “powershell” and hitting enter on your keyboard. Saving you an extra step of navigating to the CinchyDXD folder manually in Powershell _(Image 15)._
{% endhint %}

![Image 15: Launch Powershell](<../../../.gitbook/assets/image (203).png>)

2\. In the Powershell window type in **cin** and hit **Tab** on your keyboard _(Image 16)._

![Image 16: Type cin](<../../../.gitbook/assets/image (654).png>)

3\. Hit **Enter** on your keyboard, you will see a list of commands that are available to execute _(Image 17)._

![Image 17: List of commands](<../../../.gitbook/assets/image (161).png>)

4\. In the Powershell command line hit your “up” arrow key to bring back the last command and type export next to it _(Image 18)._

![Image 18](<../../../.gitbook/assets/image (101).png>)

5\. Hit **Enter** on your keyboard _(Image 19)._

![Image 19](<../../../.gitbook/assets/image (310).png>)

The Powershell window will provide you with the required and optional components to export the data experience.

6\. You must now set up any mandatory export parameters

{% hint style="warning" %}
The parameters executed in Powershell can exist on one line in powershell, however for legibility (below) the parameters have been put on separate lines.  If you are putting your parameters on separate lines you will be required to have backticks quote \`  for the parameters to execute.
{% endhint %}

{% hint style="warning" %}
Please ensure that you are using the sample below as a sample.  You will be required to provide values that correspond to:

* the URL for the source environment
* the User ID for the user who is performing the export
* the Password for the user who is performing the export
* your folder path for where CLI is stored&#x20;
* your folder path for where the CLI output files are written to
* the GUID for the Data Experience that is generated in the Data Experience Definition table
* your own version naming convention
* your folder path for where your CinchyDXD output files are written to
{% endhint %}

Sample:\
&#x20;`` .\CinchyDXD.ps1 export ` ``\
`` -s "<cinchy source url>" ` ``\
`` -u "<source user id>" ` ``\
`` -p "<source passsword>" ` ``\
`` -c "C:\Cinchy CLI v4.0.2" ` ``\
`` -d "C:\CLI Output Logs" ` ``\
`` -g "8C4D08A1-C0ED-4FFC-A695-BBED068507E9" ` ``\
`` -v "1.0.0" ` ``\
`` -o "C:\CinchyDXD_Output" ` ``\


7\. Enter the export parameters into the Powershell window _(Image 20)._

![Image 20: Enter your export parameters](<../../../.gitbook/assets/image (121).png>)

8\. Hit **Enter** on your keyboard to run the export command.

Powershell will begin to process the export. Once the export is complete, Powershell will provide you with an export complete message _(Image 21)._

![Image 21: Wait for the export to complete](<../../../.gitbook/assets/image (119).png>)

### 4.1 Validate Export

1. Ensure that the **DXD Export Folder** is populated _(Image 22)._

![Image 22: Validate that your DXD Export Folder is populated](<../../../.gitbook/assets/image (366).png>)

2\. Ensure that the **Data Experience Release** table is populated in the source environment _(Image 23)._

![Image 23: Validate that the Data Experience Release Table is populated ](<../../../.gitbook/assets/image (110).png>)

3\. Ensure that the **Data Experience Release Artifacts** table is populated in the source environment _(Image 24)._

![Image 24: Validate that the Data Experience Release Artifacts table is populated](<../../../.gitbook/assets/image (399).png>)
