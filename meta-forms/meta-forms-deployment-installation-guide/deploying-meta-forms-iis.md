# Deploying Meta-Forms (IIS)

## 1. Prerequisites

To install Cinchy Meta-Forms you will need:

* **A deployed Cinchy platform.** Note that these instructions are specific to an IIS deployment; for instructions relevant to an Kubernetes deployment [please see here.](broken-reference)
* **The Cinchy CLI.** [Please see here](https://cli.docs.cinchy.com/connections-installation-guide/v5-connections-and-cli-installation-guide#3.-running-the-cli) if you don't have this running.
* Ensure that the IIS extension **"URL Rewrite"** is installed on the server hosting IIS. The extension is available here: [https://www.iis.net/downloads/microsoft/url-rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)

## 2. Download the Resources

The Cinchy Meta-Forms code is [hosted on GitHub here.](https://github.com/cinchy-co/meta-releases/tree/main/Meta-Forms) For an IIS deployment, you need to download the **Meta-Forms-Data-Experience zip file** and the **Meta-Forms-App-Experience zip file.**

Ensure that you download the latest version.

## 3. Deploying the App Experience

1. Create a ‘Cinchy Applets’ folder if it does not exist (eg. _**C:/Cinchy Applets**_) and check in the App Experience code.
2. If you have multiple instances, create a folder that corresponds to the name of your Cinchy instance (eg. _**C:/CinchyApplets/DXdemo**_) and place the extracted AApp Experience folder here.

<figure><img src="../../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

3. Open your IIS Manager.&#x20;
4. Navigate to IIS Connections.&#x20;
5. Right Click the Cinchy Server name.

<figure><img src="../../.gitbook/assets/image (593).png" alt=""><figcaption></figcaption></figure>

6. Expand Sites such that “Default Web Site” is visible.
7. Navigate to the site containing your Cinchy application and select “Add Virtual Directory”.
8. Name the Alias of this directory “dx”.

<figure><img src="../../.gitbook/assets/image (600).png" alt=""><figcaption></figcaption></figure>

9. Input the path to _Cinchy Applets_ directory in the Physical Path field.
10. Right click on the _meta-forms_ folder under the _dx_ virtual directory and click _Convert to Application_.

**Update Configurations**&#x20;

1\. Locate "\assets\config.json" and input your Cinchy domain as specified below.

<figure><img src="../../.gitbook/assets/image (565).png" alt=""><figcaption></figcaption></figure>

**Additional Changes**&#x20;

_If you have not deployed Cinchy at the root of your domain on IIS, then you will need to also complete the steps below._

1. Specify the path to your 'meta-forms' application as instructed below.&#x20;
2. &#x20;Locate the _"C:\CinchyApplets\\\<Cinchy Environment>\Meta-Forms-App-Experience\index.html"_ file and update the base href to the path to your edit-form application on IIS and save.

## 4. Deployment the Data Experience

1. Navigate to the Meta-Forms-Data-Experience package.
2.  Open "\post-install\post-install-1.sql" and replace the below with your own URLs.

    ```sql
    SET @CinchyURL = 'https://pilot.cinchy.net/DXdemo/Cinchy'
    SET @MetaFormsURL = 'https://pilot.cinchy.net/DXdemo/dx/metaforms'
    ```
3. Open an instance of Powershell or similar terminal from within the Meta-Forms-Data-Experience.
4. Run the below command, using the table as a guide and inputting your own parameters.

```
./CinchyDXD.ps1 install -s "cinchy.com/cinchy" -h -sso "cinchy.com/sso" -u user -p pass -c "C:\Users\Downloads\Cinchy Connections v5.5.3\Cinchy Connections CLI\Cinchy Connections CLI (win-x64)" -d "C:\sometempdirectory" -e
```

| Required Command               | Description                                                                                                                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -s, --Cinchy Server            | **Required**. The full path to the Cinchy server without the protocol (e.g. cinchy.com/Cinchy).                                                                       |
| -h, --https                    | Flag indicating connections to Cinchy should be over https.                                                                                                           |
| -sso                           | The full path the the CinchySSO server without the protocol (e.g. cinchy.com/CinchySSO). This is only required if your CinchySSO server is different than /CinchySSO. |
| -u, --Username                 | **Required**. The user id for accessing Cinchy.                                                                                                                       |
| -p, --Password                 | **Required**. The clear text password (unencrypted) of the specified user.                                                                                            |
| -c, --CinchyCLI Directory      | **Required**. The path to the Cinchy CLI.                                                                                                                             |
| -d, --CinchyCLI Temp Directory | **Required**. The path to the directory that the CLI can use for storing temporary files to support the sync (e.g. partitioned data).                                 |
| -e                             | **Required** if you are running the CLI via exe instead of dll.                                                                                                       |

## 5. Confirm your Installation

Once you have installed the experience, your environment will be populated with these tables and queries:&#x20;

* Forms table
* Forms Sections table
* Form Fields table
* Get Form Metadata query
* Get Form Sections Metadata query
* Get Form Fields Metadata

<figure><img src="../../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

**Forms Table:** This table houses all of the existing forms in your environment. Adding an entry to this table, will add a form to the complete list of forms. The table is used to link form fields and form sections to one specific form, and it connects to the table which contains all of the form responses/data.

**Form Sections Table:**  This table is used to create separation between parts of the form. For instance, when creating a quiz based on compliance, you can use this table to create a section for the technological questions, one for the policy based questions, and one for any possible quiz feedback. A new form section can be created by adding a new entry to this table. The form sections need to be connected to the form created in the Forms table.

**Form Fields Table:** This table is used to create all of the possible data entry fields associated with the form sections within the form. To create a new form field, simply add an entry to the table and connect it to the form section. In the example of the quiz, add an entry for every question and link it to the appropriate form section.

**Get Form MetaData Query:** This query will return the metadata of your form, using the form ID present in the Forms table. The results will show such data as whether a form field is mandatory or not, the choices present for each question, the column names and types present in the table containing your form data, the form fields and sections, the JSON data, and more.

**Get Form Sections Query:** This query uses the form ID, present in the Forms table, in order to return all of the form sections present within the form.&#x20;

For any users who want to access forms/want forms to execute properly, they need to have access to all of the following tables and queries. These include the three tables and two queries shown above, as well as the Tables, Table Columns, and Domains tables within that user's specific environment.&#x20;
