---
description: >-
  This page outlines Step 2 of Deploying CinchyDXD: Packaging the Data
  Experience
---

# Package the data experience (CinchyDXD)

## Download the CinchyDXD utility

The CinchyDXD utility takes all the components (tables, queries, views, formatting rules) of a DX and package them up so they can be moved from one environment to another.

{% hint style="danger" %}
**Remember that a**ll objects need to be created in one source environment (ex: DEV). From there, DXD will be used to push them into others (ex: SIT, UAT, Production).
{% endhint %}

The CinchyDXD utility is only required (made accessible) for the environment that's packing up the data experience. It's not required for the destination (or target) environment.

For CinchyDXD to work, you must have CinchyCLI installed. For further installation instructions please refer to CLI ([https://cli.docs.cinchy.com/](https://cli.docs.cinchy.com/)) documentation

To access the Data Experience Deployment utility please contact Cinchy support (support@cinchy.com).

To download the Utility:

1. Login to Cinchy
2. Navigate to the **Releases Table**
3. Select the **Experience Deployment Utility View**
4. Locate and download the utility **(Cinchy DXD v1.7.0.zip)**

{% hint style="warning" %}
The CinchyDXD utility is only upwards compatible with Cinchy version 4.6+
{% endhint %}

5. Unzip the utility and place the folder at any location on a computer that also has CinchyCLI installed
6. Create a new folder in the same directory that will hold all of the DX exports generated (CinchyDXD\*Output) \*(Image 1).\_

![Image 1: Creating your new folder](<../../../.gitbook/assets/image (598).png>)

This folder will then hold all your deployment packages.

7. Launch a PowerShell console window
8. From the console, navigate to the CinchyDXD directory _(Image 2 and 3)._

{% hint style="success" %}
From within your file explorer window, type “PowerShell” into the file path. It will launch a PowerShell window already at the folder path
{% endhint %}

![Image 2: Navigate to your directory](<../../../.gitbook/assets/image (121).png>)

![Image 3: Navigate to your directory](<../../../.gitbook/assets/image (544).png>)

## Initial setup: PowerShell

PowerShell requires an initial setup when using CinchyDXD.

1. From your PowerShell window type `cin`
2. Hit Tab on your keyboard _(Image 4)._

![Image 4: Setting up](<../../../.gitbook/assets/image (289).png>)

3. Hit **Enter** on your keyboard _(Image 5)._

![Image 5: Setting up, cont.](<../../../.gitbook/assets/image (310).png>)

**You will get an error message** (above) that `CinchyDXD.ps1 cannot be loaded because the running script is disabled`.

To resolve this error:

4. From your **start menu**, search for **PowerShell** and select **Run as Administrator** _(Image 6)._

![Image 6: Run as administrator](<../../../.gitbook/assets/image (226).png>)

5. When prompted **if you want to allow this app to make changes on your device**, select Yes.
6. In your PowerShell Administrator window enter **Set-ExecutionPolicy RemoteSigned** _(Image 7)._

![Image 7: Set-ExecutionPolicy RemoteSigned](<../../../.gitbook/assets/image (460).png>)

7. Hit **Enter** on your keyboard _(Image 8)._

![Image 8: Hit Enter](<../../../.gitbook/assets/image (462).png>)

8. When prompted with the _Execution Policy Changes_, enter **A for “Yes to All”**
9. Close the PowerShell Administrator window
10. Navigate back to your PowerShell window for the _CinchDXD window_
11. From your PowerShell window type `cin`
12. Hit **Tab** and then **Enter** on your keyboard _(Image 9)._

![Image 9: Finishing your set up](<../../../.gitbook/assets/image (703).png>)

The basic CinchyDXD instructions will be displayed. You will be able to execute commands such as exporting and installing a Data Experience.

## Cinchy DXD tables overview

Cinchy uses four tables for packing up and deploying a Data Experience _(Image 10)._

![Image 10: Data Experience tables](<../../../.gitbook/assets/image (461).png>)

{% hint style="info" %}
The Data Experience is defined and packed in what will be referred to moving forward as the **Source Environment**. Where the environment that the Data Experience will be deployed to will be referenced to as the **Target Environment**.
{% endhint %}

1. **Data Experience Definition Table**: Where the data experience is defined (tables, queries, views, formatting rules, UDF’s etc.)
2. **Data Experience Reference Data Table:** Where we define any data that needs to move with the Data Experience for the experience to work (lookup values, static values that may need to exist in tables - it typically would not be the physical data itself)
3. **Data Experience Releases Table:** Once a Data Experience is exported, an entry is created in this table for the export containing:
   * **Version Number**
   * **Release Binary** is the location where you can archive/backup your release history in Cinchy\
     Please Note: if you have your own release management system, you do have the option to opt out of archiving the releases in Cinchy and check the release into your own source control
   * **Release Name**
   * **Data Experience**
4. **Data Experience Release Artifact Table**: Stores all of the files that are part of the Data Experience package as individual records along with all of the binary for each record

### Define the data experience

When setting up a Data Experience definition, you will need one definition for each Data Experience you wish to package and deploy to a given number of Target Environments.

1. Locate and open the **Data Experience Definitions** table _(Image 11)._

![Image 11: Data Experience Definitions table](<../../../.gitbook/assets/image (443).png>)

| Column                   | Definition                                                                                                                                                                                                                                                                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| GUID                     | This value is calculated, please note this value will be required as one of your export parameters in PowerShell                                                                                                                                                               |
| Name                     | This is the Name of your Data Experience                                                                                                                                                                                                                                       |
| Tables                   | Select all tables that are part of the Data Experience                                                                                                                                                                                                                         |
| Views                    | Select all views (in the data browser) that are a part of the Data Experience                                                                                                                                                                                                  |
| Integrated Clients       | Select any integrated clients (For example: Tableau, PowerBI, custom integrations) that are part of the Data Experience                                                                                                                                                        |
| Data Sync Configurations | Select any data syncs (CLI’s experience needs to work) that are part of the Data Experience                                                                                                                                                                                    |
| Listener Configurations  | Select any Listener Config rows that refer to a Data Sync Configuration which is a part of the Data Experience                                                                                                                                                                 |
| Reference Data           | Select any reference data that's part of the Data Experience. Please note that the setup of the reference data is done in the table called Data Experience Reference Data (see step 2 below for setup details)                                                                 |
| Secrets                  | Select any Secrets you'd like to include that are used Data Sync Configurations or Listener Configs which are a part of this Data Experience.                                                                                                                                  |
| Webhooks                 | Select any Webhooks that are a part of this data experience                                                                                                                                                                                                                    |
| User Defined Functions   | Select any user defined functions (For example: validate phone, validate email) that are part of the Data Experience                                                                                                                                                           |
| Models                   | Select any custom models that override columns or tables in your Data Experience, if there are none - leave blank                                                                                                                                                              |
| Groups                   | Select any groups that are part of the Data Experience (when moving groups, it will also move all table access \[design] controls)                                                                                                                                             |
| System Colours           | Select a system colour (if defined) for the Data Experience                                                                                                                                                                                                                    |
| Saved Queries            | Select any queries that are part of the Data Experience                                                                                                                                                                                                                        |
| Applets                  | Select any applets that are part of the Data Experience                                                                                                                                                                                                                        |
| Pre-install Scripts      | Select any Pre-install Scripts (Saved Queries) that should run **before** the installation of this Data Experience.                                                                                                                                                            |
| Post-install Scripts     | <p>Select any Post-install Scripts (Saved Queries) that should run <strong>after</strong> to the installation of this Data Experience.<br><br>A common use-case is to rectify data that may be different between environments.</p>                                             |
| Formatting Rules         | Select any formatting rules that are part of the Data Experience                                                                                                                                                                                                               |
| Literal Groups           | Select any literals associated to the Data Experience (For example: key values with English and French definitions)                                                                                                                                                            |
| Builders                 | Select the builder(s) who have permission to export the Data Experience                                                                                                                                                                                                        |
| Builder Groups           | <p>Select the builder group(s) that have permission to export the Data Experience<br></p><p>Note: Best Practice is to use a Group over a User. Users within groups can fluctuate, where the Group (or Role) will remain. This will require less maintenance moving forward</p> |
| Sync GUID                | Leave this column blank                                                                                                                                                                                                                                                        |

2\. Complete the following _(Image 12):_

| Column         | Value                            |
| -------------- | -------------------------------- |
| Name           | Currency Converter               |
| Tables         | Currency Exchange Rate (Sandbox) |
| Saved Queries  | Currency Converter               |
| Builder Groups | Currency Converters              |

![Image 12: Enter in information](<../../../.gitbook/assets/image (669).png>)

{% hint style="info" %}
If you make changes to the DX in the future, you aren't required to build a new Data Experience Definition in this table, you will update the existing definition. If you need to review what the definition looked like historically, you can view it via the Collaboration log.
{% endhint %}

### Define the reference data

When setting up a **Data Experience Reference Data** definition, you will need one (1) definition for each Reference Data table you wish to package and deploy with your Data Experience to the Target Environment.

{% hint style="info" %}
This table set up is similar to setting up a CLI.
{% endhint %}

1. Locate and open the **Data Experience Reference Data** table _(Image 13)._

![Image 13: Data Experience Reference Data table](<../../../.gitbook/assets/image (68).png>)

| Column                     | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name                       | This is the Name of your Reference Data Table, note this name can be anything and doesn't have to replicate the actual table name                                                                                                                                                                                                                                                                                                                                                        |
| Ordinal                    | The ordinal number assigned will identify the order in which the data is loaded and required based on dependencies within the data experience. For example if you have tables that have hierarchies in them, you will need to load the parent records first and then load your child records which would then resolve any links in the table.                                                                                                                                            |
| Filter                     | This is where a WHERE clause would be required. For example, if you have a table that has hierarchies, you would require two rows within the Data Experience Reference Data table. One to load the parent data and one to load the children data. In the parent record a filter WHERE clause would be needed to filter all parent records. In the second record in the filter column a WHERE clause in another in the second record that would be needed to filter the children records. |
| New Records                | Identify the behaviour of a new record (INSERT, UPDATE, DELETE, IGNORE)                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Change Records             | Identify the behaviour of a changed record (INSERT, UPDATE, DELETE, IGNORE)                                                                                                                                                                                                                                                                                                                                                                                                              |
| Dropped Records            | Identify the behaviour of a dropped record (INSERT, UPDATE, DELETE, IGNORE)                                                                                                                                                                                                                                                                                                                                                                                                              |
| Table                      | Identify the table that you are exporting data from                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Sync Key                   | Required (need definition)                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Expiration Timestamp Field | If Dropped Records is set to “Expire” then a timestamp column is required                                                                                                                                                                                                                                                                                                                                                                                                                |

Based on the configuration set up in this table, Cinchy will export the data and create CSV and CLI files.

This example doesn't have Reference Data as part of the Data Experience.

## Export the data experience

Using PowerShell you will now export the Data Experience you have defined within Cinchy.

1. Launch **PowerShell** and navigate to your **CinchyDXD folder** _(Image 14)._

![Image 14: Launch PowerShell](<../../../.gitbook/assets/image (722).png>)

{% hint style="info" %}
Reminder: you can launch PowerShell right from your file explorer window in the CinchyDXD folder by entering in the folder path “PowerShell” and hitting enter on your keyboard. Saving you an extra step of navigating to the CinchyDXD folder manually in PowerShell _(Image 15)._
{% endhint %}

![Image 15: Launch PowerShell](<../../../.gitbook/assets/image (198).png>)

2. In the PowerShell window type in `cin` and hit **Tab** on your keyboard _(Image 16)._

![Image 16: Type cin](<../../../.gitbook/assets/image (98).png>)

3. Hit **Enter** on your keyboard, you will see a list of commands that are available to execute _(Image 17)._

![Image 17: List of commands](<../../../.gitbook/assets/image (371).png>)

4. In the PowerShell command line hit your “up” arrow key to bring back the last command and type export next to it _(Image 18)._

![Image 18](<../../../.gitbook/assets/image (132).png>)

5. Hit **Enter** on your keyboard _(Image 19)._

![Image 19](<../../../.gitbook/assets/image (762).png>)

The PowerShell window will provide you with the required and optional components to export the data experience.

6. You must now set up any mandatory export parameters

{% hint style="warning" %}
The parameters executed in PowerShell can exist on one line in PowerShell, however for legibility (below) the parameters have been put on separate lines. If you are putting your parameters on separate lines you will be required to have backticks quote \` for the parameters to execute.
{% endhint %}

{% hint style="warning" %}
Please ensure that you are using the sample below as a sample. You will be required to provide values that correspond to:

* the URL of the source environment
* the User ID for the user who is performing the export
* the Password for the user who is performing the export
* your folder path for where CLI is stored
* your folder path for where the CLI output files are written to
* the GUID for the Data Experience that's generated in the Data Experience Definition table
* your own version naming convention
* your folder path for where your CinchyDXD output files are written to
{% endhint %}

Sample: `` .\CinchyDXD.ps1 export ` ``\
`` -s "<cinchy source url>" ` ``\
`` -u "<source user id>" ` ``\
`` -p "<source passsword>" ` ``\
`` -c "C:\Cinchy CLI v4.0.2" ` ``\
`` -d "C:\CLI Output Logs" ` ``\
`` -g "8C4D08A1-C0ED-4FFC-A695-BBED068507E9" ` ``\
`` -v "1.0.0" ` ``\
`` -o "C:\CinchyDXD_Output" ` ``\\

7. Enter the export parameters into the PowerShell window _(Image 20)._

![Image 20: Enter your export parameters](<../../../.gitbook/assets/image (152).png>)

8. Hit **Enter** on your keyboard to run the export command.

PowerShell will begin to process the export. Once the export is complete, PowerShell will provide you with an export complete message _(Image 21)._

![Image 21: Wait for the export to complete](<../../../.gitbook/assets/image (151).png>)

### Validate export

1. Ensure that the **DXD Export Folder** is populated _(Image 22)._

![Image 22: Validate that your DXD Export Folder is populated](<../../../.gitbook/assets/image (266).png>)

2\. Ensure that the **Data Experience Release** table is populated in the source environment _(Image 23)._

![Image 23: Validate that the Data Experience Release Table is populated](<../../../.gitbook/assets/image (142).png>)

3\. Ensure that the **Data Experience Release Artifacts** table is populated in the source environment _(Image 24)._

![Image 24: Validate that the Data Experience Release Artifacts table is populated](<../../../.gitbook/assets/image (12).png>)