# Package the data experience

## Overview

This section covers everything you need to export a release package from a data experience.

## Before you begin

Before you start, make sure you have access to the following tables:

1. [**Data Experience Definitions Table**](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/References/data-experience-definitions.md)
1. **Data Experience Releases Table** 
1. [**Data Experience Reference Data Table**](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/References/data-experience-reference.md)
1. **Data Experience Release Artifact Table**

## Define the data experience

You must define your data experience in the **Data Experience Definitions Table**. Each row in the table is a definition for each Data Experience you want to package and export. 

A definition contains the entities that make up the experience. Some examples of entities are tables, domains, reference data, and user defined functions (UDFs) . 

For a complete list of all fields, see the [Data Experience Definitions](../cinchydxd-utility/References/data-experience-definitions.md) reference page.


### Definition best practices

Cinchy recommends packaging entities based on their dependencies on one another. For example entities such as Applets, System Colours, Literal Groups, and Models can be packaged separately. This makes versioning these entities easier when exporting to other environments.

### Updating data experiences

If you make changes to the DX in the future, update the relevant Data Experience definition. You don't need to create a new definition. If you need to review what the definition looked like historically, you can view it via the Collaboration log.


## Define the reference data

For each table data reference, you must define an entry in the **Data Experience Reference Data** system table. This entry will be deployed alongside your Data Experience Definition (DX).

Treat this reference data similarly to a Data Sync Configuration for batch synchronization. It should move data from a CSV file to a Cinchy Table with matching attributes. The sync key column should contain unique values and shouldn't be a system or calculated column.

For a complete list of all 