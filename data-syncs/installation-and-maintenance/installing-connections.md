# Installing Connections

## 1. Overview <a href="#setup" id="setup"></a>

The Connections Experience is designed to facilitate the creation of data syncs through an easy to use Cinchy UI. Once installed, you can access Connections directly through your Cinchy platform using the applet _(Image 1)._

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption><p>Image 1: The Connections Applet</p></figcaption></figure>

{% hint style="warning" %}
**In a Kubernetes deployment of the Cinchy Platform,** Connections is automatically installed. The below steps refer only to an IIS deployment of the Cinchy Platform.
{% endhint %}

{% hint style="warning" %}
**Before proceeding, ensure that you have read the** [**Prerequisites**](broken-reference) **section.**
{% endhint %}

## 2. Downloading the Resources <a href="#setup" id="setup"></a>

1. Navigate to the[ Cinchy Releases table ](https://cinchy.net/Tables/1477?rowHeight=Expanded)and download the latest **Cinchy Connections.zip** file from the **Release Artifacts** column.

{% hint style="info" %}
The Connections.zip file contains: Worker.zip, WebApi.zip, and CLI.zip.
{% endhint %}

2\. Extract the **WebApi.zip** to the folder where you want to host the applet.

{% hint style="info" %}
We suggest to create the following path and extract it there: _C:\Connections\\_
{% endhint %}

## **3. Deploying Connections**

1. On a Windows Server machine, launch an instance of PowerShell as the Administrator.
2. Run the below commands to create the IIS application pool and set its properties.

```
Import-Module WebAdministration

$applicationPoolNameConnections="Cinchy Connections"

New-WebAppPool -Name $applicationPoolNameConnections

$appPath = "IIS:\AppPools\"+ $applicationPoolNameConnections

$appPool = Get-IISAppPool $applicationPoolNameConnections

$appPool.managedRuntimeVersion = ""

Set-ItemProperty -Path $appPath -Name managedRuntimeVersion $appPool.managedRuntimeVersion

Set-ItemProperty "IIS:\AppPools\$applicationPoolNameConnections" -Name Recycling.periodicRestart.time -Value 0.00:00:00

Set-ItemProperty "IIS:\AppPools\$applicationPoolNameConnections" -Name ProcessModel.idleTimeout -Value 1.05:00:00
```

{% hint style="warning" %}
_**Steps 4 and 5 are only needed if you deployed your Cinchy instance along a base path.**_
{% endhint %}

4. Within the Cinchy platform, navigate to the **\[Cinchy].\[Integrated Clients]** table _(Image 2)._

<figure><img src="../../.gitbook/assets/image (548).png" alt=""><figcaption><p>Image 2: Integrated Clients table</p></figcaption></figure>

4. Navigate to the row where the **Client ID** column is **"cinchy\_connections\_experience"** _(Image 3)._

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption><p>Image 3: Navigate to the row where the Client ID column is " cinchy_connections_experience"</p></figcaption></figure>

4. In that same row, update the columns **“Permitted Login Redirect URLs”** and **“Permitted Logout Redirect URLs”** to **“\<url>/connections”** _(Image 4)._

<figure><img src="../../.gitbook/assets/image (446).png" alt=""><figcaption><p>Image 4: Update Permitted Redirects</p></figcaption></figure>

5. Within the Cinchy platform, navigate to the **\[Cinchy].\[Applets]** table _(Image 5)._&#x20;

<figure><img src="../../.gitbook/assets/image (380).png" alt=""><figcaption><p>Image 5: The Applets table</p></figcaption></figure>

5. Navigate to the row where the Name column is **"Connections"** _(Image 6)._&#x20;

<figure><img src="../../.gitbook/assets/image (518).png" alt=""><figcaption><p>Image 6: Navigate to the row where the Name column is "Connections" </p></figcaption></figure>

5. Update the column **“Application URL”** to **“\<baseurl>/connections”** _(Image 7)._

<figure><img src="../../.gitbook/assets/image (434).png" alt=""><figcaption><p>Image 7: Update the Application URL column</p></figcaption></figure>

5. Navigate to _C:\Connections\appsettings.json_ and update the below properties to match your environment:

<table><thead><tr><th width="381">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><strong>"URL"</strong></td><td>This is the private URL of your Cinchy environment.</td></tr><tr><td><strong>“TempDirectory”</strong></td><td>This should be a path pointing to an existing folder that will hold all of your log and error files.</td></tr><tr><td><strong>"StorageType"</strong></td><td>Select either Local, S3, or AzureBlobStorage.</td></tr><tr><td><strong>"BasePathOverride"</strong></td><td><strong>(Optional)</strong> Connections should be hosted on "/connections". In the case that this is hosted on another url path, this property must be populated with the relative path (eg. "/dev-aurora-2/connections" or "/" if deployed to the root)</td></tr></tbody></table>

6. Navigate to _C:\Connections\ClientApp\dist\assets\config.json_ update the below properties to match your environment:

| Parameter                    | Description                                                                          |
| ---------------------------- | ------------------------------------------------------------------------------------ |
| **“authority”**              | Your public CinchySSO URL in lowercase (ex: \<base-url>/cinchysso)                   |
| **“cinchyRootUrl”**          | Your public Cinchy URL (ex: \<base-url>/Cinchy)                                      |
| **“redirectUrl”**            | The applet’s public URL (ex: \<base-url>/connections)                                |
| **silentRefreshRedirectUri** | The applet’s public URL plus the silent refresh path.                                |
| **“model”**                  | The model where your data sync configs are. Keep this as “Cinchy” if you don’t know. |
| **“domain”**                 | The model where your data sync configs are. Keep this as “Cinchy” if you don’t know. |
| **“useHttps”**               | Should be true if your Cinchy platform is hosted on a secure environment             |
| **“server”**                 | Your private Cinchy URL without “http://” or “https://”                              |

7. Create the IIS Application by running the following command in an instance of Powershell:

{% code overflow="wrap" %}
```
New-WebApplication -Name connections -Site 'Default Web Site' -PhysicalPath C:\Connections -ApplicationPool 'Cinchy Connections'
```
{% endcode %}
