---
description: This guide serves as a walkthrough of how to deploy v5 on IIS.
---

# IIS

## Overview 

Cinchy version 5 on IIS comes bundled with common components such as Connections, Meta Forms, and the Event Listener. This page details the configuration and deployment instructions for the Cinchy Platform, including SSO.

## Prerequisites

### System Requirements

- SQL SERVER 2017+
- SSMS (optional)
- Install IIS 7.5+ / enable IIS from Windows features
- Dotnet 6

### DotNet 6 Installation

- [DotNet Core 6 SDK which includes ASP.NET Core /.NET Core Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
- [DotNet Core 6 Hosting Bundle](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-aspnetcore-6.0.23-windows-hosting-bundle-installer)

{% hint style="warning" %}
Dotnet 7 is not supported with Cinchy 5.x
{% endhint %}

### Minimum Hardware Requirements

- 2 × 2 GHz Processor
- 8 GB RAM
- 4 GB Hard Disk storage available

### Minimum Database Server Hardware Recommendations

- 4 × 2 GHz Processor
- 12 GB RAM
- Hard disk storage dependent upon use case. Cinchy maintains historical versions of data and performs soft deletes which will add to the storage requirements.

## Get Access to Cinchy.net (Cinchy Prod Access)

- Access to Cinchy.net (Cinchy Prod) can be obtained during onboarding.
- Alternatively, users can request access by sending an email to [support@cinchy.com](mailto:support@cinchy.com).

### Access Cinchy.Releases Table from Cinchy UI

1. Navigate to the Cinchy.Releases table from the Cinchy user interface.

### Download Release Artifacts

Download the following items from the "Release Artifacts" column:

- [Cinchy V5.6.2.zip](#) 
- Cinchy Connection
- Cinchy Event Listener
- Cinchy Meta-Forms (optional)
- Cinchy Maintenance CLI (optional)

Please note that the link to "Cinchy V5.6.2.zip" should be replaced with the actual download link when available.

## Create a Database

{% hint style="success" %}
For more information about creating a database in SQL server, see the [Microsoft Create a database page](https://learn.microsoft.com/en-us/sql/relational-databases/databases/create-a-database?view=sql-server-ver16).
{% endhint %}

1. On your SQL Server 2017+ instance, **create a new database** named Cinchy (or any other name you prefer).
   1. If you choose an alternate name, in the remaining instructions wherever the database name is referenced, replace the word Cinchy with the name you chose.
2. Create a single user account **db\_owner privileges** for Cinchy to connect to the database. If you choose to use Windows Authentication instead of SQL Server Authentication, the authorized account must be the same account that runs the IIS Application Pool.

## Create an IIS application pool

1. On the Windows Server machine, launch an instance of PowerShell as Administrator.
2. Run the commands below to create the application pool and set its properties.

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

```powershell
$credentials = (Get-Credential -Message "Please enter the Login credentials including your Domain Name").GetNetworkCredential()
$userName = $credentials.Domain + '\' + $credentials.UserName
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.identityType -Value SpecificUser
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.userName -Value $username
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.password -Value $credentials.Password
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -name processModel.identityType -Value SpecificUser
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -name processModel.userName -Value $username
```

{% hint style="info" %}
You may use an alternate application pool name if you prefer.
{% endhint %}

## Create the application directories

1. Unzip the **"Cinchy vX.X"** application package (for the Cinchy Platform) that you downloaded from the [Releases Table](https://cinchy.net/Cinchy/Tables/1477?rowHeight=Expanded) into your C drive. This will create 2 directories, C:\Cinchy and C:\CinchySSO. Ensure your application pool accounts has read and execute access to these directories (default accounts are IIS AppPool\CinchyWeb and IIS AppPool\CinchySSO).
2. Run the below commands in the Administrator instance of PowerShell to create directories for the application logs. Ensure your application pool account has write access to these directories.

```powershell
md C:\CinchyLogs\Cinchy
md C:\CinchyLogs\CinchySSO
md C:\CinchyErrors
```

## Update the CinchySSO appsettings.json

1. Open the **C:\CinchySSO\appsettings.json** file in a text editor and update the values below.

### App Settings

1. Under **AppSettings** section, update the values outlined in the table.
2. Wherever you see **\<base url>** in the value, replace this with the actual protocol (HTTP or HTTPS) and the domain name (or IP address) you plan to use.
<!-- markdown-link-check-disable -->
Ex:. if you're using HTTPS with the domain app.cinchy.co, then **\<base url>** should be replaced with **https://app.cinchy.co**
<!-- markdown-link-check-enable -->
| Key                           | Value                                                                                                                                                                                                                              |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CinchyUri**                 | _\<base url>/Cinchy_                                                                                                                                                                                                               |
| **CertificatePath**           | <p>Adjust the certificate path to point to the CinchySSO v5 folder.<br><br><em>Ex: C:\CinchySSO\cinchyidentitysrv.pfx</em></p>                                                                                                     |
| **StsPublicOriginUri**        | <p>The Base URL used by the .well-known discovery.</p><p><br><em>Ex: /cinchySSO</em></p>                                                                                                                                           |
| **StsPrivateOriginUri**       | <p>The Private Base URL used by the .well-known discovery.<br><br><em>Ex: /cinchySSO</em></p>                                                                                                                                      |
| **CinchyAccessTokenLifetime** | <p>The duration for the Cinchy Access Token. This determines how long a user can be inactive until they need to re-enter their credentials.</p><p><br>In Cinchy v5.4+ <strong>it defaults to <code>7.00:00:00</code>.</strong></p> |
| **DB Type**                   | Either "PostgreSQL" or "TSQL"                                                                                                                                                                                                      |

{% hint style="info" %}
4.18.0+ includes session expiration based on the CinchyAccessTokenLifetime. For the default of `7.00:00:00`, if you have been inactive in Cinchy for 7 days your session will expire and you will need to log in again.
{% endhint %}

#### Below values are only required for SSO, otherwise leave them as blank <a href="#below-values-are-only-required-for-sso-otherwise-leave-them-as-blank" id="below-values-are-only-required-for-sso-otherwise-leave-them-as-blank"></a>

| Key                 | Value                                                                                                                                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SAMLClientEntityId  | Client Entity Id                                                                                                                                                                                                                      |
| SAMLIDPEntityId     | Identity Provider Entity Id                                                                                                                                                                                                           |
| SAMLMetadataXmlPath | Identity Provider metadata XML file path                                                                                                                                                                                              |
| SAMLSSOServiceURL   | Configure service endpoint for SAML authentication                                                                                                                                                                                    |
| AcsURLModule        | This parameter is needs to be configured as per your SAML ACS URL. For example, if your ACS URL looks like this - `https:///CinchySSO/identity/AuthServices/Acs`, then the value of this parameter should be "/identity/AuthServices" |

### Connection string

In order for the application to connect to the database, the "SqlServer" value needs to be set.

1.  Find and update the value under the **"ConnectionStrings"** section:

    ```
    "SqlServer" : ""
    ```

[SQL Server Authentication Example:](https://www.connectionstrings.com/sql-server/)

```
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;"
```

[SQL Server Windows Authentication Example:](https://www.connectionstrings.com/sql-server/)

```
"SqlServer" : "Server=MyServer;Database=Cinchy;Trusted_Connection=True;Connection Timeout=30;Min Pool Size=10;"
```

{% hint style="danger" %}
If you are deploying Cinchy **v5.4+ on an SQL Server Database,** you will need to make an addition to your connectionString. Adding [**TrustServerCertificate=True**](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate?view=dotnet-plat-ext-6.0) will allow you to bypass the certificate chain during validation.

Example:

```json
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;TrustServerCertificate=True"
```
{% endhint %}

### External identity claim section

Under the **"ExternalIdentityClaimSection"** section you'll see the following values.

These values are used for SAML SSO. **If you aren't using SSO, keep these values as blank**

<table><thead><tr><th width="546">Key</th><th>Value</th></tr></thead><tbody><tr><td>ExternalIdentityClaim > FirstName > ExternalClaimName</td><td></td></tr><tr><td>ExternalIdentityClaim > LastName > ExternalClaimName</td><td></td></tr><tr><td>ExternalIdentityClaim > Email > ExternalClaimName</td><td></td></tr><tr><td>ExternalIdentityClaim -> MemberOf -> ExternalClaimName</td><td></td></tr></tbody></table>

### Serilog

1. Cinchy has a **serilog** property that allows you to configure where it logs to. In the below code, update the following:
   1. "Name" must be set to "File" so it writes to a physical file on the disk.
   2. Set "path" to the file path to where you want it to log.

{% hint style="info" %}
This configuration makes a log every day (defined by the _"rollingInterval"_ value) and keeps your file count to 30 (defined by the _"retainedFileCountLimit"_ value).
{% endhint %}

```json
    "Serilog": {
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Microsoft": "Warning",
        "System.Net": "Warning"
      }
    },
    "WriteTo": [
      {
        "Name": "File",
        "Args": {
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
  },
```

## Update the Cinchy appsettings.json

1. Navigate to **C:\Cinchy**
2. Navigate to the **appsettings.json file** and update the following properties:

### AppSettings

| Key                        | Value                                                                                                                                                                                                                                                                                          |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **StsPrivateAuthorityUri** | <p>This should match your private Cinchy SSO URL.<br><br><em>Ex: /CinchySSO</em></p>                                                                                                                                                                                                           |
| **StsPublicAuthorityUri**  | <p>This should match your public Cinchy SSO URL.<br><br><em>Ex: /CinchySSO</em></p>                                                                                                                                                                                                            |
| **CinchyPrivateUri**       | <p>This should match your private Cinchy URL.<br><br><em>Ex: /Cinchy</em></p>                                                                                                                                                                                                                  |
| **CinchyPublicUri**        | <p>This should match your public Cinchy URL.<br><br><em>Ex: /Cinchy</em></p>                                                                                                                                                                                                                   |
| **UseHttps**               | This is "true" by default.                                                                                                                                                                                                                                                                     |
| **DB Type**                | Either "PostgreSQL" or "TSQL"                                                                                                                                                                                                                                                                  |
| **“MaxRequestBodySize”**   | <p><strong>This capability was introduced in Cinchy v5.4</strong></p><p>This configurable property to allow you to set your own file upload size for the Files API, should you wish. <strong>It's defaulted to 1G.</strong></p><pre><code>“MaxRequestBodySize”: 1073741824 // 1g
</code></pre> |
|                            |                                                                                                                                                                                                                                                                                                |

### Connection string

To connect the application to the database, the `SqlServer` value needs to be set.

1.  Find and update the value under the **"ConnectionStrings"** section:

    ```
    "SqlServer" : ""
    ```

[SQL Server Authentication Example:](https://www.connectionstrings.com/sql-server/)

```
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;"
```

[SQL Server Windows Authentication Example:](https://www.connectionstrings.com/sql-server/)

```
"SqlServer" : "Server=MyServer;Database=Cinchy;Trusted_Connection=True;Connection Timeout=30;
```

{% hint style="danger" %}
If you are deploying Cinchy **v5.4+ on an SQL Server Database,** you will need to make an addition to your `connectionString`. Adding [**TrustServerCertificate=True**](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate?view=dotnet-plat-ext-6.0) will allow you to bypass the certificate chain during validation.

Example:

```json
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;TrustServerCertificate=True"
```
{% endhint %}

## Create the IIS applications

1. Open an administrator instance of PowerShell
2. Execute the below commands to create the IIS applications and enable anonymous authentication. (This is required to allow authentication to be handled by the application)

```
New-WebApplication -Name Cinchy -Site 'Default Web Site' -PhysicalPath C:\Cinchy -ApplicationPool CinchyWeb
New-WebApplication -Name CinchySSO -Site 'Default Web Site' -PhysicalPath C:\CinchySSO -ApplicationPool CinchySSO
Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"
```

{% hint style="info" %}
To enable HTTPS, the server certificate must be loaded and the standard IIS configuration completed at the Web Site level to add the binding.
{% endhint %}

## Test the application
<!-- markdown-link-check-disable -->
1. Access the **\<base url>/Cinchy (http://app.cinchy.co/Cinchy)** through Google Chrome.
2. Once the login screen appears, enter the credentials:
   1. The default username is **admin** and the password is **cinchy**.
   2. You will be prompted to change your password the first time you log in.
<!-- markdown-link-check-enable -->
{% hint style="info" %}
To avoid users from having to access the application at a URL that contains /Cinchy, you can use a downloadable IIS extension called URL Rewrite to remap requests hitting the \<base url> to \<base url>/Cinchy. The extension is available [here](https://www.iis.net/downloads/microsoft/url-rewrite).
{% endhint %}

## Next steps

Navigate to the following sub-pages to deploy the following bundled v5 components:

* [Connections Deployment](../../data-syncs/installation-and-maintenance/installing-connections.md)
* [Event Listener/Worker Deployment](../../data-syncs/installation-and-maintenance/installing-the-worker-listener.md)
* [Maintenance CLI](../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)
