# CinchyDXD 2.0 workflow

## Overview

The CinchyDXD 2.0 workflow depends on four system tables within Cinchy:

1. [**Data Experience Definitions Table**](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/References/data-experience-definitions.md): This is where you define the data experience. This includes tables, queries, views, formatting rules, and user defined functions (UDFs).
3. **Data Experience Releases Table:** Once a Data Experience is exported, an entry is created in this table for the export containing:
2. [**Data Experience Reference Data Table**](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/References/data-experience-reference.md):This table defines data that must move with the data experience for it to function. This includes lookup values and static values that might exist in tables—it typically would not be the physical data itself. It also includes the following items:
   * **Version Number**
   * **Release Binary**
   * **Release Name**
   * **Data Experience**
4. **Data Experience Release Artifact Table**: Stores all files that are part of the Data Experience package as individual records along with the binary for each record.

## Basic workflow 

The basic workflow for using CinchyDXD 2.0 is the following:

1. Define your data references in the **Data Experience Definitions** table. This definition creates a GUID.
1. Create the references to data you want to move with the data experience in the **Data Experience Releases** table.
1. Package the data experience in PowerShell using .\CinchyDXD.ps1
1. Upload the package to a version control system for further development work.
1. 