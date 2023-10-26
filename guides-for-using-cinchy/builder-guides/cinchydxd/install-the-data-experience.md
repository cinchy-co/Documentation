# Install the data experience

## Overview

This section covers everything you need to install a release package from a data experience for CinchyDXD 2.0.

## Prerequisites

Before you start an installation, you must make sure that your source and target environment version of Cinchy are the same. For example, Source Version 5.5, Target Version 5.5.

### Installation overview

The installation for a release package typically proceeds in the following steps:

1. Build Data Experience from repository artifacts.
1. Prepare connections for Cinchy Tables.
1. Prepare CSVs.
1. Prepare Models.
1. Prepare reference data.
1. Prepare pre/post-install scripts.
1. Initialize the target Cinchy instance.
1. Execute pre-install scripts.
1. Load Cinchy Table Connections.
1. Load the Data Experience and Reference Data CLIs.
1. Install the Data Experience Model.
1. Continue installation of Data Experience metadata.
1. Install the Reference Data.
1. Execute post-install scripts
1. Record the Release.
1. Clean up install.

### Versioning

CinchyDXD uses version arguments for installations. These
## Install the release package

1. Open PowerShell and navigate to the location of the release package created from your CinchyDXD export.
1. Run `.\CinchyDXD.ps1 install` with the required and optional parameters.
1. The installation will run through the steps listed above.

### Install arguments

Use the following arguments to create your installation with CinchyDXD.

```pwsh

-s "<target Cinchy url>" 
-u "<target user name>" 
-p "<target user password>" 
-d "C:\CLI Output Logs"
-v "Data Experience Definition Version"

```

For a list of the available parameters, see the **Install** section of the [CinchyDXD commands](../cinchydxd/References/Cinchy-DXD-commands.md) reference page for more information.
### Example

The following example shows the use of arguments for a data package export in Cinchy.

```pwsh
.\CinchyDXD.ps1 install -s "sandbox.cinchy.net/target-url-environment" -u "JohnDoe" -p "123456" -d "C:\Logs\CLI-Output-Logs" -v "1.0.0"
```
## Validate install

To validate the install:

1. Make sure all entities that were present were installed on the target environment. For example, make sure your tables, data sync configurations, and saved queries are present.
1. Make sure that the **Data Experience Definitions** table has the DX parameters from the source environment.