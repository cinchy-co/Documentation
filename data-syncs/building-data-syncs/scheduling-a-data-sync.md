# Scheduling a Data Sync

## 1. Overview

Any task/job scheduling application can be used to run data sync automatically, on a schedule. This page will walk you through how to achieve this goal using Windows Task.

## 2. Setting Up a Task in Windows Task&#x20;

Scheduler Data synchronization commands can be scheduled to run automatically based on your data synchronization requirements (e.g. certain date/times/intervals). The CLI command that is used to execute the data synchronization can be saved as a script in PowerShell, Batch or a Command file.

Here's an example of how to schedule the CLI with Windows Task Scheduler:

1. Launch Windows Task Scheduler&#x20;
2. Create a folder to contain the CLI jobs (options)&#x20;
3. Right click on CLI job folder&#x20;
4. Select Create Task&#x20;
5. On the General tab, enter the Name of the job you want to schedule
6. Click on the Trigger Tab
7. Click New and set your schedule preferences
8. Click the Action Tab&#x20;
9. Click New button&#x20;
10. Click Browse and navigate to the folder that contains the data sync scheduling script for execution
11. Copy and Paste the Start In (optional) filed the path for your Cinchy CLI folder

### Sample Action Script

```bash
SET server="URL" SET user="UserID" SET password="EncryptedPassword" SET tempdir="Error Log Folder" SET file="File Path for Source"

dotnet Cinchy.Connections.CLI.dll syncdata -s %server% -u %user% -p %password% -m "ModelName" -d %tempdir% -f "Feed" -v "filePath":%file%
```
