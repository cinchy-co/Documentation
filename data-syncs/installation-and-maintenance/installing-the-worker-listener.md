# Install the Worker/Listener

## Overview

The Worker and Event Listener are vital components for real-time Data syncs. Here's a brief overview of how the Worker/Event Listener operates:

- The Event Listener, once successfully subscribed, awaits events from the streaming source.
- Upon receiving a message from a streaming source, the Event Listener pushes it to the message queue.
- The Worker then retrieves the message from the message queue.
- The Worker fetches the corresponding record from the target based on the sync key.
- If changes are detected, the Worker pushes them to the target system. Both successes and failures get logged in the worker's log file.

{% hint style="warning" %}
**For a Kubernetes deployment of the Cinchy Platform**, the Worker/Event Listener is automatically installed. The steps below are specific to an IIS deployment of the Cinchy Platform.
{% endhint %}

## Prerequisites

- Windows Server 2012+
  - [.NET Core 3.1 Hosting Bundle](https://dotnet.microsoft.com/download/dotnet-core/2.1)
- SQL Server 2012+
  - Service Broker enabled
- Cinchy Platform

## SQL Service Broker setup

1. On a Windows Server machine, launch an instance of PowerShell as Administrator.
2. Enable the SQL Service Broker using the command:

```sql
ALTER DATABASE [Your Cinchy Database Name] SET ENABLE_BROKER WITH ROLLBACK IMMEDIATE;
```

## Download the resources

1. Navigate to the [Cinchy Releases table](https://cinchy.net/Tables/1477?rowHeight=Expanded).

### Event Listener

1. Download the latest **Cinchy Event Listener.zip** file from the **Release Artifacts** column.
2. Extract the .zip file to your designated directory. For example, `C:\your event listener folder`.
3. Run the `create-cinchy-event-listener-windows-service.ps1` PowerShell script from the installation directory. Use the `filePath` parameter as `-filePath <Path to your agent.exe file>`.

### Worker

1. Download the latest **Cinchy Connections.zip** file from the **Release Artifacts** column.
2. Unzip the content of the **Cinchy Worker** folder to, for example, `C:\your CLI worker folder`.
3. Run the `create-cinchy-cli-worker-windows-service.ps1` PowerShell script from the installation directory. Pass the `filePath` parameter pointing to the **Cinchy\.CLI.exe file**.

## Event Listener deployment

1. Open the `appSettings.json` file in your Event Listener directory and configure:

  #### ClientSettings

  <!-- vale off -->

  | Parameter | Value                                               |
  | --------- | --------------------------------------------------- |
  | URL       | Cinchy Web URL (e.g., https://cinchy.net/Cinchy)    |
  | Password  | Password for the user: eventlistener@cinchy.com     |

  <!-- vale on -->

  #### AppSettings

  | Parameter                     | Value                                                                                                             |
  | ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
  | GetNewListenerConfigsInterval | (seconds) Frequency at which the listener checks for new configs in the \[Cinchy].\[Listener Configs] table. Default is 60 seconds. |

  #### ConnectionStrings

  | Parameter | Value                                                                        |
  | --------- | ---------------------------------------------------------------------------- |
  | SqlServer | Connection string to the SQL server that hosts the Cinchy database.          |

2. To start the Event Listener service:
    - Press `Windows + R` to access the Run box.
    - Type `services.msc` and press Enter.
    - From the services list, locate **Cinchy Event Listener**.
    - Right-click on it and select **Start**.

## Worker Deployment

1. Open the _appSettings.json_ file in your Worker directory and configure:

  #### ClientSettings

  <!-- vale off -->

  | Parameter | Value                                            |
  | --------- | ------------------------------------------------ |
  | URL       | Cinchy Web URL (e.g., https://cinchy.net/Cinchy) |
  | Password  | Password for the user: connections@cinchy.com    |

  <!-- vale on -->

  #### AppSettings

  | Parameter     | Value                                      |
  | ------------- | ------------------------------------------ |
  | Model         | Cinchy. This specifies the model for the CLI. |
  | TempDirectory | Temporary directory for the CLI to store files. |

  #### ConnectionStrings

  | Parameter | Value                                                                        |
  | --------- | ---------------------------------------------------------------------------- |
  | SqlServer | Connection string to the SQL server that hosts the Cinchy database.          |

2. To start the Worker service:
    - Press `Windows + R` to open the Run box.
    - Input `services.msc` and hit Enter.
    - In the list of services, find **Cinchy Worker**.
    - Right-click on the service and select **Start**.
