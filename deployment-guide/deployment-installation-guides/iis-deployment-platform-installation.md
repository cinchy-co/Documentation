---
description: This guide serves as a walkthrough of how to deploy v5 on IIS.
---

# IIS Deployment Platform Installation

## Table of Contents

| Table of Contents                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------- |
| [#1.-create-a-database](iis-deployment-platform-installation.md#1.-create-a-database "mention")                                         |
| [#2.-create-an-iis-application-pool](iis-deployment-platform-installation.md#2.-create-an-iis-application-pool "mention")               |
| [#3.-create-the-application-directories](iis-deployment-platform-installation.md#3.-create-the-application-directories "mention")       |
| [#4.-update-the-cinchysso-appsettings.json](iis-deployment-platform-installation.md#4.-update-the-cinchysso-appsettings.json "mention") |
| [#5.-update-the-cinchy-appsettings.json](iis-deployment-platform-installation.md#5.-update-the-cinchy-appsettings.json "mention")       |
| [#6.-create-the-iis-applications](iis-deployment-platform-installation.md#6.-create-the-iis-applications "mention")                     |
| [#7.-test-the-application](iis-deployment-platform-installation.md#7.-test-the-application "mention")                                   |

## Overview and Prerequisites

Cinchy version 5 on IIS comes bundled with common components such as Connections, Meta Forms, and the Event Listener. This page details the configuration and deployment instructions for **the Cinchy Platform, including SSO.** Click on the links below to be taken to the appropriate pages for other components:

* [Connections Deployment](../../data-syncs/installation-and-maintenance/installing-connections.md)
* [Event Listener/Worker Deployment](../../data-syncs/installation-and-maintenance/installing-the-worker-listener.md)
* [Meta Forms Deployment](../../meta-forms/meta-forms-deployment-installation-guide/)
* [Maintenance CLI](../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)

**Ensure that you review the** [**prerequisites listed here**](deployment-planning-overview-and-checklist/deployment-prerequisites/#2.-iis-deployment-prerequisites) prior to performing an IIS Deployment, including downloading all necessary artifacts from the [Cinchy Releases Table.](https://cinchy.net/Cinchy/Tables/1477?rowHeight=Expanded)

Please contact [Cinchy Support](../../getting-help.md) if you do not have the credentials required to access the artifacts table.

{% hint style="success" %}
**Step 1** of this guide refers to the **SQL Server**. **Step 2** onwards refers to the **Web Server.**
{% endhint %}

## 1. Create a Database

1. On your SQL Server 2017+ instance, **create a new database** named Cinchy (or any other name you prefer).&#x20;
   1. If you choose an alternate name, in the remaining instructions wherever the database name is referenced, replace the word Cinchy with the name you chose.
2. A single user account with **db\_owner privileges** is required for the Cinchy application to connect to the database. If you choose to use Windows Authentication instead of SQL Server Authentication, the account that is granted access must be the same account under which the IIS Application Pool runs.

## 2. Create an IIS Application Pool

1. On the Windows Server machine, launch an instance of PowerShell as Administrator.
2. Run the below commands to create the application pool and set its properties.

```
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

3\. If you chose to use Windows Authentication in the database or want to run the application under a different user account, execute the below commands to change the application pool identity.

```
$credentials = (Get-Credential -Message "Please enter the Login credentials including your Domain Name").GetNetworkCredential()
$userName = $credentials.Domain + '\' + $credentials.UserName
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.identityType -Value SpecificUser
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.userName -Value $username
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameWeb" -name processModel.password -Value $credentials.Password
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -name processModel.identityType -Value SpecificUser
Set-ItemProperty "IIS:\AppPools\$applicationPoolNameSSO" -name processModel.userName -Value $username
```

{% hint style="info" %}
You may use an alternate application pool name (i.e. instead of Cinchy) if you prefer.
{% endhint %}

## 3. Create the Application Directories

1. Unzip the **"Cinchy vX.X"** application package (for the Cinchy Platform) that you downloaded from the [Releases Table](https://cinchy.net/Cinchy/Tables/1477?rowHeight=Expanded) into your C drive. This will create 2 directories, C:\Cinchy and C:\CinchySSO. Ensure your application pool accounts has read and execute access to these directories (default accounts are IIS AppPool\CinchyWeb and IIS AppPool\CinchySSO).
2. Run the below commands in the Administrator instance of PowerShell to create directories for the application logs. Ensure your application pool account has write access to these directories.

```
md C:\CinchyLogs\Cinchy
md C:\CinchyLogs\CinchySSO
md C:\CinchyErrors
```

## 4. Update the CinchySSO appsettings.json

1. Open the **C:\CinchySSO\appsettings.json** file in a text editor and update the values below.

### 4.1 App Settings

1\. Under **AppSettings** section, update the values outlined in the table.&#x20;

2\. Wherever you see **\<base url>** in the value, replace this with the actual protocol (i.e. http or https) and the domain name (or ip address) you plan to use.&#x20;

Ex:. if you're using https with the domain app.cinchy.co, then **\<base url>** should be replaced with **https://app.cinchy.co**

| Key                           | Value                                                                                                                                                                                                                                |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **CinchyUri**                 | _\<base url>/Cinchy_                                                                                                                                                                                                                 |
| **CertificatePath**           | <p>Adjust the certificate path to point to the CinchySSO v5 folder.<br><br><em>Ex: C:\CinchySSO\\cinchyidentitysrv.pfx</em></p>                                                                                                      |
| **StsPublicOriginUri**        | <p>The Base URL used by the .well-known discovery.</p><p><br><em>Ex: &#x3C;base url>/cinchySSO</em></p>                                                                                                                              |
| **StsPrivateOriginUri**       | <p>The Private Base URL used by the .well-known discovery. <br><br><em>Ex: &#x3C;base url>/cinchySSO</em></p>                                                                                                                        |
| **CinchyAccessTokenLifetime** | <p>The duration for the Cinchy Access Token. This determines how long a user can be inactive until they need to re-enter their credentials.</p><p><br>In Cinchy v5.4+ <strong>it defaults to "7.00:00:00", i.e. 7 days.</strong></p> |
| **DB Type**                   | Either "PostgreSQL" or "TSQL"                                                                                                                                                                                                        |

{% hint style="info" %}
4.18.0+ includes session expiration based on the CinchyAccessTokenLifetime. So for the default of "7.00:00:00", this means that if you have been inactive in Cinchy for 7 days, your session will expire and you will need to log in again.
{% endhint %}

#### Below values are only required for SSO, otherwise leave them as blank <a href="#below-values-are-only-required-for-sso-otherwise-leave-them-as-blank" id="below-values-are-only-required-for-sso-otherwise-leave-them-as-blank"></a>

| Key                 | Value                                                                                                                                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SAMLClientEntityId  | Client Entity Id                                                                                                                                                                                                                      |
| SAMLIDPEntityId     | Identity Provider Entity Id                                                                                                                                                                                                           |
| SAMLMetadataXmlPath | Identity Provider metadata XML file path                                                                                                                                                                                              |
| SAMLSSOServiceURL   | Configure service endpoint for SAML authentication                                                                                                                                                                                    |
| AcsURLModule        | This parameter is needs to be configured as per your SAML ACS URL. For example, if your ACS URL looks like this - "https:///CinchySSO/identity/AuthServices/Acs", then the value of this parameter should be "/identity/AuthServices" |

### **4.2 Connection String**

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

Example:&#x20;

```json
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;TrustServerCertificate=True"
```
{% endhint %}

### 4.3 External Identity Claim Section

Under the **"ExternalIdentityClaimSection"** section you'll see the following values.

These values are used for SAML SSO. **If you are not using SSO, keep these values as blank**

<table><thead><tr><th width="546">Key</th><th>Value</th></tr></thead><tbody><tr><td>ExternalIdentityClaim > FirstName > ExternalClaimName</td><td></td></tr><tr><td>ExternalIdentityClaim > LastName > ExternalClaimName</td><td></td></tr><tr><td>ExternalIdentityClaim > Email > ExternalClaimName</td><td></td></tr><tr><td>ExternalIdentityClaim -> MemberOf -> ExternalClaimName</td><td></td></tr></tbody></table>

### 4.4 Serilog

1.  There is a "serilog" property that allows you to configure where it logs to. In the below code, update the following:

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

## 5. Update the Cinchy appsettings.json

1. Navigate to **C:\Cinchy**
2. Navigate to the **appsettings.json file** and update the following properties:

### 5.1 AppSettings

| Key                        | Value                                                                                                                                                                                                                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **StsPrivateAuthorityUri** | <p>This should match your private Cinchy SSO URL.<br><br><em>Ex: &#x3C;baseURL>/CinchySSO</em></p>                                                                                                                                                                                                      |
| **StsPublicAuthorityUri**  | <p>This should match your public Cinchy SSO URL.<br><br><em>Ex: &#x3C;baseURL>/CinchySSO</em></p>                                                                                                                                                                                                       |
| **CinchyPrivateUri**       | <p>This should match your private Cinchy URL.<br><br><em>Ex: &#x3C;baseURL>/Cinchy</em></p>                                                                                                                                                                                                             |
| **CinchyPublicUri**        | <p>This should match your public Cinchy URL.<br><br><em>Ex: &#x3C;baseURL>/Cinchy</em></p>                                                                                                                                                                                                              |
| **UseHttps**               | This is "true" by default.                                                                                                                                                                                                                                                                              |
| **DB Type**                | Either "PostgreSQL" or "TSQL"                                                                                                                                                                                                                                                                           |
| **“MaxRequestBodySize”**   | <p><strong>This capability was introduced in Cinchy v5.4</strong></p><p></p><p>This configurable  property to allow you to set your own file upload size for the Files API, should you wish. <strong>It is defaulted to 1G.</strong></p><pre><code>“MaxRequestBodySize”: 1073741824 // 1g
</code></pre> |

### **5.2 Connection String**

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
"SqlServer" : "Server=MyServer;Database=Cinchy;Trusted_Connection=True;Connection Timeout=30;
```

{% hint style="danger" %}
If you are deploying Cinchy **v5.4+ on an SQL Server Database,** you will need to make an addition to your connectionString. Adding [**TrustServerCertificate=True**](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate?view=dotnet-plat-ext-6.0) will allow you to bypass the certificate chain during validation.

Example:&#x20;

```json
"SqlServer" : "Server=MyServer;Database=Cinchy;User ID=cinchy;Password=password;Trusted_Connection=False;Connection Timeout=30;Min Pool Size=10;TrustServerCertificate=True"
```
{% endhint %}

### 5.3 Serilog

1. There is a "serilog" property that allows you to configure where it logs to. In the below code, update the following:
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
          "path": "C:\\CinchyLogs\\Cinchy\\log.json",
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

## 6. Create the IIS Applications

1. Open an administrator instance of PowerShell
2. Execute the below commands to create the IIS applications and enable anonymous authentication. (This is required in order to allow authentication to be handled by the application)

```
New-WebApplication -Name Cinchy -Site 'Default Web Site' -PhysicalPath C:\Cinchy -ApplicationPool CinchyWeb
New-WebApplication -Name CinchySSO -Site 'Default Web Site' -PhysicalPath C:\CinchySSO -ApplicationPool CinchySSO
Set-WebConfigurationProperty -Filter "/system.webServer/security/authentication/anonymousAuthentication" -Name Enabled -Value True -PSPath IIS:\ -Location "Default Web Site"
```

{% hint style="info" %}
To enable HTTPS, the server certificate must be loaded and the standard IIS configuration completed at the Web Site level to add the binding.
{% endhint %}

## 7. Test the Application

1. Access the **\<base url>/Cinchy (e.g. http://app.cinchy.co/Cinchy)** through Google Chrome.&#x20;
2. Once the login screen appears, enter the credentials:
   1. The default username is **admin** and the password is **cinchy**.
   2. You will be prompted to change your password the first time you log in.

{% hint style="info" %}
To avoid users from having to access the application at a url that contains /Cinchy, you can use a downloadable IIS extension called URL Rewrite to remap requests hitting the \<base url> to \<base url>/Cinchy. The extension is available [here](https://www.iis.net/downloads/microsoft/url-rewrite).
{% endhint %}

## 8. Next Steps

Navigate to the following sub-pages to deploy the following bundled v5 components:

* [Connections Deployment](../../data-syncs/installation-and-maintenance/installing-connections.md)
* [Event Listener/Worker Deployment](../../data-syncs/installation-and-maintenance/installing-the-worker-listener.md)
* [Maintenance CLI](../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)
