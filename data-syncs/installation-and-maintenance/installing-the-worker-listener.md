# Installing the Worker/Listener

## Overview

The Worker and Listener are important components for using Real-Time Data syncs. At a high level, the Worker/Listener follows this process:

- Once the Listener is successfully subscribed, it waits for events from streaming source.
- The Listener receives a message from a streaming source and pushes it to SQL Server Broker.
- The Worker then picks up message from SQL Server Broker.
- The Worker fetches the matching record from the target based on the sync key.
- If there are changes detected, the Worker pushes them to the target system. Successes and failures are logged in the worker's log file.

{% hint style="warning" %}
**In a Kubernetes deployment of the Cinchy Platform**, the Worker/Listener is automatically installed. The below steps refer only to an IIS deployment of the Cinchy Platform.
{% endhint %}

{% hint style="warning" %}
**Before proceeding, make sure that you have read the** [**Prerequisites**](broken-reference) **section.**
{% endhint %}

## Prerequisites

- Windows Server 2012+
  - [.NET Core 3.1 Hosting Bundle](https://dotnet.microsoft.com/download/dotnet-core/2.1)
- SQL Server 2012+
  - Service Broker enabled
- Cinchy Platform

## SQL service broker setup

1. On a Windows Server machine, launch an instance of PowerShell as Administrator.
2. Set up the SQL Service Broker by executing the following command:

```sql
ALTER DATABASE [Your Cinchy Database Name] SET ENABLE_BROKER WITH ROLLBACK IMMEDIATE;
```

## Downloading the resources

1. Navigate to the[ Cinchy Releases table.](https://cinchy.net/Tables/1477?rowHeight=Expanded)

### Event listener

1. Download the latest **Cinchy Event Listener.zip** file from the **Release Artifacts** column.
2. Extract the .zip to the folder to \<your event listener folder>
3. Execute the `create-cinchy-event-listener-windows-service.ps1` PowerShell script located in the installation directory. Pass in `filePath` parameter _-filePath \<Your Listener/Worker Path>_ to the **agent.exe file.**

### Worker

1. Download the latest **Cinchy Connections.zip** file from the **Release Artifacts** column.
2. Extract the content of the **Cinchy Worker** folder to C:\\\<your CLI worker folder>
3. Execute `create-cinchy-cli-worker-windows-service.ps1` PowerShell script located in the installation directory. Pass in `filePath` parameter _filePath = path_ to the **Cinchy\.CLI.exe file.**

## Event listener deployment

1. Navigate to the _appSettings.json_ in your Event Listener directory and make the following configurations:

#### ClientSettings

<!-- vale off -->

| Parameter | Value                                               |
| --------- | --------------------------------------------------- |
| URL       | Cinchy Web URL (ex. https://cinchy.net/Cinchy)      |
| Password  | The password for the user eventlistener@cinchy.com. |

<!-- vale on -->

#### AppSettings

| Parameter                     | Value                                                                                                             |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| GetNewListenerConfigsInterval | (seconds) How often the listener polls for new configs in the \[Cinchy].\[Listener Configs] table. Default is 60. |

#### ConnectionStrings

| Parameter | Value                                                                        |
| --------- | ---------------------------------------------------------------------------- |
| SqlServer | Fill in the connection string to the SQL server hosting the Cinchy database. |

2. To start the service, open the **Run box (Windows + R) > services.msc**

3. In the list of services, find the **Cinchy Event Listener** service. Right-click on the service and click **Start.**

## Worker deployment

1. Navigate to the _appSettings.json_ in your Worker directory and make the following configurations:

#### ClientSettings

<!-- vale off -->

| Parameter | Value                                             |
| --------- | ------------------------------------------------- |
| URL       | Cinchy Web URL (ex. https://cinchy.net/Cinchy)    |
| Password  | The password for the user connections@cinchy.com. |

<!-- vale on -->

#### AppSettings

| Parameter     | Value                                      |
| ------------- | ------------------------------------------ |
| Model         | Cinchy. This is the model for the CLI.     |
| TempDirectory | Temp directory for the CLI to store files. |

#### ConnectionStrings

| Parameter | Value                                                                        |
| --------- | ---------------------------------------------------------------------------- |
| SqlServer | Fill in the connection string to the SQL server hosting the Cinchy database. |

2. To start the service, open the **Run box** on your machine **(Windows + R) >** type in **services.msc**
3. In the list of services, find the **Cinchy Worker**.
4. Right click on the service and click **Start.**
