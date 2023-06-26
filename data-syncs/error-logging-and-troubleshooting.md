# Error Logging and Troubleshooting

## Troubleshooting

### General Troubleshooting

If your data sync configuration has failed, here are a few items of consideration:

* Have your credentials changed in either the source or target (e.g. expired password)?
* Is your sync key unique in your source and target?
* Is the configuration entered in the \[Cinchy].\[Data Sync Configurations] table?
* If source is a file, does it exist at the specified location?

### Changing your Auto Offset Reset configuration

You are able to switch between Auto Offset Reset types after your initial configuration through the below steps:&#x20;

1. Navigate to the **Listener Config table.**
2. Re-configure the Auto Offset Reset value to whatever you want.&#x20;
3. Set the **"Status"** column of the Listener Config to **"Disabled".**&#x20;
4. Navigate to the **Event Listener State table.**&#x20;
5. Find the column that pertains to your data sync's Listener Config and delete it.&#x20;
6. Navigate back to the **Listener Config table.**&#x20;
7. Set the **"Status"** column of the Listener Config to **"Enabled"** in order for your new Auto Offset Reset configuration to take effect.

### Skipping Messages

If there are a large number of messages that have been queued **after a listener config is turned off**, you can skip processing them by following the below steps:

1. Navigate to the **Event Listener State table** on your Cinchy platform.
2. Navigate to the row containing the state corresponding to your listener config.
3. Delete the above row.
4. Navigate to the **Listener Config table** on your Cinchy platform.
5. Navigate to the row containing the data sync configuration you want to configure.
6. Set the **"Auto Offset Reset"** column to **"Latest".** This will ensure that when you turn your listener back on it will start listening from the latest messages, thus skipping the queued ones.

## Error Logging

### Cinchy Tables

When running a datasync interactively, the output screen will display the result of the job on the first line, there are two (2) potential outcomes:

* Data sync completed successfully
* Data sync completed with errors (see \<temp folder>for error logs)

If the data sync runs on a schedule, there are two (2) tables in the Cinchy domain that can be reviewed to determine the outcome:

1.  **Execution Log Table** - this is where you can find the output and status of any job executed

    **Please note**, you can see when the job ran by the Create timestamp on the record. (Display Columns  -> add Created column to the view)
2. **Execution Errors Table** - this table may have one or more records for a job that failed with synchronization or validation errors



### Execution Log Table Schema

| Column           | Definition                                                                                                                                                                                                                                                                                    |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Execution ID     | This is the number assigned to the job that has been executed and is incremented by one (1) for each subsequent job that is executed                                                                                                                                                          |
| Command          | This column will display the CLI command that has been executed (e.g.Data Sync, Data Export etc.)                                                                                                                                                                                             |
| Server Name      | This is the name of the server where the CLI was executed. If you run the CLI from a personal computer this is the name of your computer.                                                                                                                                                     |
| File Path        | <p>In case of a Data Sync, if the source is a file, this field will contain a link to the file.</p><p></p><p>In case of a Data Export, the field will be a link to the file created by the export.</p><p></p><p>Note that these are paths local to the server where the CLI was executed.</p> |
| Parameters       | This column will display any parameters passed to the command line                                                                                                                                                                                                                            |
| State            | This column will display the state of the job (e.g. Succeeded, Failed or Running)                                                                                                                                                                                                             |
| Execution Output | This column will display the output that would have been displayed if the job was executed from the command prompt.                                                                                                                                                                           |
| Execution Time   | This column will display how long it took to execute the job                                                                                                                                                                                                                                  |
| Data Sync Config | This column will have a link to the name of your configuration file                                                                                                                                                                                                                           |

![](https://lh5.googleusercontent.com/SZ-qgCoacJOREhPUyjK6XH0z7OZ-cK\_dg1liNGNRgoSkAvOjqRKD0HSreuvoFTGKAfYQ\_i0P44PDORI0GvuicLAqbYi05qqrkTr9cBQIDZQ6KtIxlXQszIh\_40yLsQ5nQTSX4lEu)

### Execution Error Table Defined

| Column       | Description                                                                                                                                 |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Error Class  | This column will display the category of errors that has been generated (e.g. Invalid Row, Invalid Column, Invalid File etc.)               |
| Error Type   | This column will display the reason for the error (e.g. Unresolved Link, Invalid Format Exception, Malformed Row, Max Length Violation etc) |
| Column       | This column will display the name of the column that generated the error                                                                    |
| Row          | This column will display the row number(s) of records from the source that generated the error                                              |
| Row Count    | This column will display the number of records affected by this error                                                                       |
| Execution ID | This column will have a link that ties back to the error to the Execution Log                                                               |

![](https://lh3.googleusercontent.com/rupFhLK-vn2sXyxwNG18Dihzv\_ZOfBmFhqLXU7ex3N06VKIMrMSvmXeqO9ddo7A-8T9MV9gfXiO966Bh9qI0R1E1W\_iaDRZdO04Eo4J5VQJKmMXLdxjPGvLeFMizblTjl40vjx3R)

#### Tip

To automatically check if the job was successful, you have three (3) exit codes that can be checked for the job:

* **0** - Completed without errors
* **1** - Execution failed
* **2** - Completed with validation errors

#### Sample Code

```bash
$CLICommand = "dotnet C:\CinchyCLI\Cinchy.CLI.dll syncdata -q ..." 
Invoke-Expression $CLICommand 
switch ($LASTEXITCODE) { 
0 { Write-Host "Completed without errors" } 
1 { Write-Host "Execution failed" } 
2 { Write-Host "Completed with validation errors" } 
}
```

### Logs

The syncdata command will use the folder, indicated after the -d parameter in the command line, to create and store temporary files. If the data sync is successful, all the temporary files are automatically purged. However, if there is an error the following CSV files will exist:

* ExecutionLogID\_SourceErrors.csv
* ExecutionLogID\_SyncErrors.csv
* ExecutionLogID\_TargetErrors.csv

#### Source & Target Error Logs

The SourceErrors and TargetErrors CSV files will have the following three (3) columns:

* **Row** - this column identifies the row number of the rejected record. **Please note,** only data rows are counted, if the source is a file with a number of header rows, this number needs to be added to the row number to get the actual row in the source that is causing the failure.
* **Rejected** - will be either a Yes or No. If the field is Yes, this indicates that full record has been skipped. If the field is No, valid fields are inserted/updated and fields with validation errors are not inserted / updated
* **Errors** - this column contains a list of fields causing validation errors or an error affecting the whole record, like “Malformed Row”

#### Sync Error Log

The SyncErrors file also has three (3) columns:

* **Failed Operations** - this column will display the operation (e.g. INSERT, UPDATE or DELETE)
* **Error** - this column will provide a error message as to why the operation failed
* **Record Id** - Unique ID in the target system. For Cinchy this is the Cinchy ID. Most systems will have their own unique identifier (e.g. Salesforce ID).

### Error Messages

#### Source/Target Errors

| Error                    | Description                                                                                                                                                                 |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Duplicate Key            | The sync key values are not unique & duplicated records are rejected                                                                                                        |
| Malformed Row            | The row could not be parsed based on the source schema. For example the record may not have the number of columns mentioned in the source section of the CLI configuration. |
| Invalid Format Exception | Check the value for this column, there may be a mismatched data type (e.g.inserting a non-digit character in a number column)                                               |
| Max Length Violation     | The text you are trying to insert or update a target field with is too long                                                                                                 |
| Mandatory Rule Violation | No (or incorrect) value provided for a mandatory column                                                                                                                     |
| Unresolved Link          | Check if the values the CLI is trying to insert/update exist in the linked Cinchy table target                                                                              |

#### Sync Errors

Records may fail to insert, update or get deleted due to sync errors, these come from the target system when the CLI tries to write data to it. Each target system will return its own errors, here are some examples from Cinchy, note that these are the same errors you see when doing a paste in the Manage Data screen to that table:

| Error                                             | Description                                                                                                                           |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Value must be a number                            | Check the value for this column, there may be a mismatched data type, like trying to insert an non-digit character in a Number column |
| Value must be a valid date                        | No (or incorrect) value provided for a mandatory column                                                                               |
| Value must be Yes or No                           | The value passed was not a Bool                                                                                                       |
| Value must be selected from the available options | The value from the source does not correspond to the values in the Cinchy Target choice column                                        |
