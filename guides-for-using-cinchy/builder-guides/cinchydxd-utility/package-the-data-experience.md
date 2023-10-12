# Package the data experience

## Overview

This section covers everything you need to export a release package from a data experience.

## Prerequisites

Before you start, make sure you have access to the following tables:

1. [**Data Experience Definitions Table**](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/References/data-experience-definitions.md)
1. [**Data Experience Reference Data Table**](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/References/data-experience-reference.md)

## Define the data experience

You must define your data experience in the **Data Experience Definitions Table**. Each row in the table is a definition for each Data Experience you want to package and export. 

A definition contains the entities that make up the experience. Some examples of entities are tables, domains, reference data, and user defined functions (UDFs) . 

For a complete list of all fields, see the [Data Experience Definitions](../cinchydxd-utility/References/data-experience-definitions.md) reference page.


### Best practices

Cinchy recommends packaging entities based on their dependencies on one another. For example entities such as Applets, System Colours, Literal Groups, and Models can be packaged separately. This makes versioning these entities easier when exporting to other environments.

### Updating data experiences

If you make changes to the DX in the future, update the relevant Data Experience definition. You don't need to create a new definition. If you need to review what the definition looked like historically, you can view it via the Collaboration log.


## Define the reference data

For each table data reference, you must define an entry in the **Data Experience Reference Data** system table. This entry will be deployed alongside your Data Experience Definition (DX).

Treat this reference data similarly to a Data Sync Configuration for batch synchronization. It should move data from a CSV file to a Cinchy Table with matching attributes. The sync key column should contain unique values and shouldn't be a system or calculated column.

For a complete list of all columns, please see the [Data Experience Reference Table](../cinchydxd-utility/References/data-experience-reference.md).

## Export the data experience

After you define the data experience and the reference data you want with it, you can now use PowerShell to export your experience.

{% hint style="info" %}
You must have PowerShell 7 or later to use CinchyDXD
{% endhint %}

To export your data experience:

1. Launch PowerShell and navigate to your CinchyDXD folder (such asC:\DxDvX.X.X) 
1. Run `.\CinchyDXD.ps1 export` with the required and optional parameters.

### Export arguments

Use the arguments below to create your data export with CinchyDXD.

```pwsh
-s "<cinchy source url>" 
-u "<source user name>" 
-p "<source password>" 
-d "<folder path for where the CLI output files are written to>" 
-g "<the GUID for the DX that is generated in the Data Experience Definition table." 
-v "<version of the experience package, update every time if there are changes in the DX>" 
-o "<folder path for where your CinchyDXD output files to install are written to>"
```

For a list of the available parameters, see the **Export** section of the [CinchyDXD commands](../cinchydxd-utility/References/Cinchy-DXD-commands.md) reference page for more information.

### Example

The following example shows the use of arguments for a data package export in Cinchy.

```pwsh
.\CinchyDXD.ps1 export -s "sandbox.cinchy.net/source-url-environment" -u "JohnDoe" -p "123456" -d "C:\Logs\CLI-Output-Logs -g "d98a1f18-f4bf-8695-ef04ghfa47" -v "1.0.0" -o "C:\CinchyDXDOutput"
```
## Validate export process

To validate your export, navigate to the target output path and make sure it's populated with the necessary files.

### Export failures list

#### Error: Release already exists

The export will fail if a record with the same Name, GUID, and Version already exists in the Data Experience Releases system table in the lower environment.

##### Solutions:

1. **Update package version**: Required if the Data Experience definition has been modified since the last installed version in the higher environment.
2. **Delete the release**: Required if the version isn't yet installed in the higher environment.

#### Error: References to deleted metadata

This error indicates that Data Experience Definitions are pointing to a deleted entity (such as a saved query in the Recycle Bin).

##### Solution:
Manually update the definition to remove the reference to the deleted data.


## Next steps

- [Install the data experience](../cinchydxd-utility/install-the-data-experience.md)
