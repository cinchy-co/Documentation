# Schedule a data sync

## Overview

You can use any task/job scheduling application to automatically run data syncs on a schedule. This page describes how to do this using Windows Task Scheduler.

## Set up a task in Windows Task Scheduler

You can set Scheduler Data synchronization commands to run automatically based on your data synchronization requirements, such as certain date/times/intervals. You can save the CLI command used to execute the data synchronization as a script in PowerShell, Batch or a Command file.

Here's an example of how to schedule the CLI with Windows Task Scheduler:

1. Launch Windows Task Scheduler
2. Create a folder to contain the CLI jobs (options)
3. Right click on CLI job folder
4. Select Create Task
5. On the General tab, enter the Name of the job you want to schedule
6. Click on the Trigger Tab
7. Click New and set your schedule preferences
8. Click the Action Tab
9. Click New button
10. Click Browse and navigate to the folder that contains the data sync scheduling script for execution
11. Copy and Paste the Start In (optional) filed the path for your Cinchy CLI folder

### Sample action script

```bash
SET server="URL" SET user="UserID" SET password="EncryptedPassword" SET tempdir="Error Log Folder" SET file="File Path for Source"

dotnet Cinchy.Connections.CLI.dll syncdata -s %server% -u %user% -p %password% -m "ModelName" -d %tempdir% -f "Feed" -v "filePath":%file%
```
