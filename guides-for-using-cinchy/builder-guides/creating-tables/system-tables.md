# System Tables

System tables are included out-of-the-box with your Cinchy platform, and can be used to track and manage a variety of data.

![Some examples of system tables](<../../../.gitbook/assets/image (365).png>)

You can easily query for a list of your system tables using the below:

```sql
SELECT [Name]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NULL
AND [Domain] = 'Cinchy'
```

The system tables included are:

- **Applets:** This system table manages a list of all your integrated applications
- **Data Experience Definitions:** This is a system table for managing data experience definitions
- **Data Experience References:** This is a system table for managing reference data for data experiences
- **Data Experience Release Artifacts:** This is a system table for maintaining data experience release artifacts
- **Data Experience Releases:** This is a system table for maintaining data experience releases
- **Data Security Classifications:** This is a system table for maintaining data security classifications
- **Data Sync Configurations:** This system table manages a list of all your data sync configurations.
- **Domains:** This system table manages a list of all the domains in your instance.
- **Execution Log:** This system table tracks the execution logs of data syncs
- **Formatting Rules:** This system table manages your formatting rules
- **Groups:** System table for managing all groups
- **Literal Groups:** This system table maintains a list of groups
- **Literal Translations:** This system table maintains a list of literal translations
- **Literals:** This system table maintains a list of literals
- **Regions:** This system table maintains a list of regions
- **Saved Queries:** This system table manages a list of all user saved queries that can be exposed via the REST API
- **System Colours:** System table for maintaining colours
- **Table Access Control:** This system table maintains a list of all table access controls in your instance
- **Table Columns:** This system table manages a list of all the system column definitions
- **Tables:** This system table manages a list of all the tables in your instance.
- **User Defined Functions:** This system table manages a list of all your user defined functions
- **Users:** System table for managing all user information including enabling/disabling the ability to create tables, queries, etc.
- **Views:** This system table manages a list of all the views in your instance.
