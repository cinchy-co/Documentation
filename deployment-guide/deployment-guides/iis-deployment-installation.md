---
description: This guide serves as a walkthrough of how to deploy v5 on IIS.
---

# IIS

## Overview

Cinchy version 5 on IIS comes bundled with common components such as Connections, Meta Forms, and the Event Listener. This page details the configuration and deployment instructions for the Cinchy Platform, including SSO.

## Prerequisites

### System Requirements

* SQL SERVER 2017+
* SSMS (optional)
* Install IIS 7.5+ / enable IIS from Windows features
* Dotnet 6

### DotNet 6 Installation

* [DotNet Core 6 SDK which includes ASP.NET Core /.NET Core Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
* [DotNet Core 6 Hosting Bundle](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-aspnetcore-6.0.23-windows-hosting-bundle-installer)

{% hint style="warning" %}
Dotnet 7 isn't supported with Cinchy 5.x
{% endhint %}

### Minimum Hardware Requirements

* 2 × 2 GHz Processor
* 8 GB RAM
* 4 GB Hard Disk storage available

### Minimum Database Server Hardware Recommendations

* 4 × 2 GHz Processor
* 12 GB RAM
* Hard disk storage dependent upon use case. Cinchy maintains historical versions of data and performs soft deletes which will add to the storage requirements.

## Get Access to Cinchy.net (Cinchy Prod Access)

* Access to Cinchy.net (Cinchy Prod) can be obtained during onboarding.
* Alternatively, users can request access by sending an email to [support@cinchy.com](mailto:support@cinchy.com).

### Access Cinchy Releases Table from Cinchy UI

Navigate to the **Cinchy Releases** table from the Cinchy user interface.

### Download Release Artifacts

Download the following items from the "Release Artifacts" column:

* Cinchy VX.X.zip
* Cinchy Connection
* Cinchy Event Listener
* Cinchy Meta-Forms (optional)
* Cinchy Maintenance CLI (optional)

## Create a Database

{% hint style="success" %}
For more information about creating a database in SQL server, see the [Microsoft Create a database page](https://learn.microsoft.com/en-us/sql/relational-databases/databases/create-a-database?view=sql-server-ver16).
{% endhint %}

1. On your SQL Server 2017+ instance, **create a new database** and name it **Cinchy**.

{% hint style="success" %}
If you choose an alternate name, use the name in the rest of the instructions instead of \*\*Cinchy\*\*.
{% endhint %}

2. Create a single user account with `db_owner privileges` for Cinchy to connect to the database. If you choose to use Windows Authentication instead of SQL Server Authentication, the authorized account must be the same account that runs the IIS Application Pool.

## Create an IIS application pool

1. On the Windows Server machine, launch an instance of PowerShell as Administrator.
2. Copy and run the PowerShell snippet below to create the application pool and set its priorities. You can also manually create the app pool via the [IIS Manager](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/add/).
3. Verify Db\_name → Security → Users → select the user → properties → membership

```powershell
Import-Module WebAdministration
$applicationPoolNameSSO="CinchySSO"
$applicationPoolNameWeb="CinchyWeb"
New-WebAppPool -Name $applicationPoolNameSSO
$appPath = "IIS:\AppPools\"+ $applicationPoolNameSSO
$appPool = Get-IISAppPool $applicationPoolNameSSO
$appPool.managedRuntimeVersion = ""
Set-ItemProperty -Path $appPath -Name managedRuntimeVersion $appPool.managedRuntimeVersion
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -Name Recycling.periodicRestart.time -Value 0.00:00:00
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -Name ProcessModel.idleTimeout -Value 1.05:00:00
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -Name Recycling.periodicRestart.privateMemory -Value 0
New-WebAppPool -Name $applicationPoolNameWeb
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -Name Recycling.periodicRestart.time -Value 0.00:00:00
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -Name ProcessModel.idleTimeout -Value 1.05:00:00
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -Name Recycling.periodicRestart.privateMemory -Value 0
```

3. If you use Windows Authentication in the database or want to run the application under a different user account, execute the commands below to change the application pool identity.

{% hint style="info" %}
You can also use an alternate name in the application pool.
{% endhint %}

```powershell
$credentials = (Get-Credential -Message "Please enter the Login credentials including your Domain Name").GetNetworkCredential()
$userName = $credentials.Domain + '\' + $credentials.UserName
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.identityType -Value SpecificUser
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.userName -Value $username
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.password -Value $credentials.Password
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -name processModel.identityType -Value SpecificUser
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -name processModel.userName -Value $username
```

## Create the application directories

1. Download and unzip the **"Cinchy vX.X"** application package from the [Releases Table](https://cinchy.net/Cinchy/Tables/1477?rowHeight=Expanded). This will create two directories: `Cinchy` and `CinchySSO`. For example, if you unzip at the root of your C drive, the two directories will be `C:\Cinchy` and `C:\CinchySSO`.
2. Make sure your application pool accounts has read and execute access to these directories.
3. Run the below commands in the Administrator instance of PowerShell to create separate directories for Errorlogs and Logs.

```powershell
md C:\CinchyLogs\Cinchy
md C:\CinchyLogs\CinchySSO
md C:\CinchyErrors
```

{% hint style="info" %}
You can create it under your single folder as well. For example, `md C:\your_folder_name\CinchyLogs\Cinchy`. If you do, make sure to replace any related directory instructions with the your folder path. pool.
{% endhint %}

## Update the CinchySSO appsettings.json

1. Open the `C:\CinchySSO\appsettings.json` file in a text editor and update the values below.

### App Settings

1. Under **AppSettings** section, update the values outlined in the table.

{% hint style="info" %}
Replace `<base url>` with your chosen protocol and domain. For example, if using HTTPS on `app.cinchy.co`, substitute `<base url>` with `https://app.cinchy.co`. For localhost, use `http://localhost/Cinchy`.
{% endhint %}

| Parameter                   | Description                                                                       | Example                                                     |
| --------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `CinchyUri`                 | The base URL appended with `/Cinchy`.                                             | `http://localhost/Cinchy`, `{base_cinchy_url}/Cinchy`       |
| `CertificatePath`           | Path to the CinchySSO v5 folder for the certificate.                              | `C:\\CinchySSO\\cinchyidentitysrv.pfx`                      |
| `StsPublicOriginUri`        | Base URL of the `.well-known` discovery.                                          | `http://localhost/CinchySSO`, `{base_cinchy_url}/CinchySSO` |
| `StsPrivateOriginUri`       | Private Base URL of the `.well-known` discovery.                                  | `http://localhost/CinchySSO`, `{base_cinchy_url}/CinchySSO` |
| `CinchyAccessTokenLifetime` | Duration for the Cinchy Access Token in v5.4+. Defaults to `7.00:00:00` (7 days). | `7.00:00:00`                                                |
| `DB Type`                   | Database type. Either `PostgreSQL` or `TSQL`.                                     | For SQLSERVER installation:`TSQL`                           |

#### SSO installation

For more information on the SSO installation, please see the [SSO installation page](deployment-planning-overview-and-checklist/deployment-prerequisites/single-sign-on-sso-integration/)

### Connection string

To connect the application to the database, you must set the `SqlServer` value.

1.  Find and update the value under the **"ConnectionStrings"** section:

    ```js
    "SqlServer" : ""
    ```

#### SQL Server Authentication example

```js
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;TrustServerCertificate=True;"
```

#### SQL Server Windows Authentication example

```js
"SqlServer" : "Server=MyServer;Database=Cinchy;Trusted_Connection=True;Connection Timeout=30;Min Pool Size=10;"
```

### Serilog

Cinchy has a `serilog` property that configures where the logs are located. In the below code, update the following:

* `"Name"` must be set to "File" so it writes to a physical file on the disk.
* Set `"path"` to the file path to where you want it to log.
* Replace `"WriteTo"` section with following:

```json
"WriteTo": [
      {
        "Name": "File",
        "Args": {
// For the "path" variable, please refer to the original path in your system where these log folders were created.
          "path": "C:\\CinchyLogs\\CinchySSO\\log.json",
          "preserveLogFilename": true,
          "shared": "true",
          "rollingInterval": "Day",
          "rollOnFileSizeLimit": true,
          "fileSizeLimitBytes": 100000000,
          "retainedFileCountLimit": 30,
          "formatter": "Serilog.Formatting.Compact.CompactJsonFormatter, Serilog.Formatting.Compact"
        }
      }
    ]
```

## Update appsettings.json

1. Navigate to the installation folder for Cinchy (**C:\Cinchy**).
2. Open the **appsettings.json** file and update the following properties:

### AppSettings

| Key                      | Description                                                                         | Example                                                     |
| ------------------------ | ----------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `StsPrivateAuthorityUri` | Match your private Cinchy SSO URL.                                                  | `http://localhost/CinchySSO`, `{base_cinchy_url}/CinchySSO` |
| `StsPublicAuthorityUri`  | Match your public Cinchy SSO URL.                                                   | `http://localhost/CinchySSO`, `{base_cinchy_url}/CinchySSO` |
| `CinchyPrivateUri`       | Match your private Cinchy URL.                                                      | `http://localhost/Cinchy`, `{base_cinchy_url}/CinchySSO`    |
| `CinchyPublicUri`        | Match your public Cinchy URL.                                                       | `http://localhost/Cinchy`, `{base_cinchy_url}/Cinchy`       |
| `UseHttps`               | Use HTTPS.                                                                          | `false`                                                     |
| `DB Type`                | Database type.                                                                      | `TSQL`                                                      |
| `MaxRequestBodySize`     | Introduced in Cinchy v5.4. Sets file upload size for the Files API. Defaults to 1G. | `1073741824 // 1g`                                          |
| `LogDirectoryPath`       | Match your Web/IDP logs folder path.                                                | `C:\\CinchyLogs\\CinchyWeb`                                 |
| `SSOLogPath`             | Match your SSO log folder path.                                                     | `C:\\CinchyLogs\\CinchySSO\\log.json`                       |

### Setup the connection string

To connect the application to the database, the `SqlServer` value needs to be set.

#### SQL Server Authentication example

```json
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;TrustServerCertificate=True;"
```

#### SQL Server Windows Authentication example

```json
"SqlServer" : "Server=MyServer;Database=Cinchy;Trusted_Connection=True;Connection Timeout=30;Min Pool Size=10;"
```

## Create the IIS applications

1. Open an administrator instance of PowerShell.
2. Execute the below commands to create the IIS applications and enable anonymous authentication. (This is required to allow authentication to be handled by the application).

```powershell
New-WebApplication -Name Cinchy -Site 'Default Web Site' -PhysicalPath C:\Cinchy -ApplicationPool CinchyWeb
New-WebApplication -Name CinchySSO -Site 'Default Web Site' -PhysicalPath C:\CinchySSO -ApplicationPool CinchySSO
Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"
```

{% hint style="info" %}
To enable HTTPS, you must load the server certificate and the standard IIS configuration completed at the Web Site level to add the binding.
{% endhint %}

## Test the application

1. Access the `<base url>/Cinchy` (http://app.cinchy.co/Cinchy) through a web browser.
2. Once the login screen appears, enter the credentials:
   * The default username is **admin** and the password is **cinchy**.
   * You will be prompted to change your password the first time you log in.

## Next steps

Navigate to the following sub-pages to deploy the following bundled v5 components:

* [Connections Deployment](../../data-syncs/installation-and-maintenance/installing-connections.md)
* [Event Listener/Worker Deployment](../../data-syncs/installation-and-maintenance/installing-the-worker-listener.md)
* [Maintenance CLI](../../data-syncs/installation-and-maintenance/install-the-cli.md)
