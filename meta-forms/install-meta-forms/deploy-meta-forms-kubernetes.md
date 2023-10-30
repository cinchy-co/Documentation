# Deploy Meta-Forms (Kubernetes)

## Prerequisites

To install Cinchy Meta-Forms on Kubernetes you will need:

- **A deployed Cinchy platform on Kubernetes.**These instructions are specific to a Kubernetes deployment.
- [The Cinchy CLI](https://cli.docs.cinchy.com/connections-installation-guide/v5-connections-and-cli-installation-guide#3.-running-the-cli).

## Download the Resources

The Cinchy Meta-Forms code is [on GitHub here.](https://github.com/cinchy-co/meta-releases/tree/main/Meta-Forms) For a Kubernetes deployment, you need to download the **Meta-Forms-Data-Experience zip file.**

Ensure that you download the latest version.

## Deployment instructions

1. Navigate to the Meta-Forms-Data-Experience package.
2. Open "\post-install\post-install-1.sql" and replace the below with your own URLs.

   ```sql
   SET @CinchyURL = 'https://pilot.cinchy.net/DXdemo/Cinchy'
   SET @MetaFormsURL = 'https://pilot.cinchy.net/DXdemo/dx/metaforms'
   ```

3. Open an instance of PowerShell or similar terminal.
4. Run the below command, using the table as a guide and inputting your own parameters.

```bash
./CinchyDXD.ps1 install -s "cinchy.com/cincy" -h -sso "cinchy.com/sso" -u user -p pass -c "C:\Users\Downloads\Cinchy Connections v5.5.3\Cinchy Connections CLI\Cinchy Connections CLI (win-x64)" -d "C:\sometempdirectory" -e
```

<!-- vale off -->

| Required Command               | Description                                                                                                                                                     |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -s, --Cinchy Server            | **Required**. The full path to the Cinchy server without the protocol (cinchy.com/Cinchy).                                                                      |
| -h, --https                    | Flag indicating connections to Cinchy should be over HTTPS.                                                                                                     |
| -sso                           | The full path to the CinchySSO server without the protocol (cinchy.com/CinchySSO). This is only required if your CinchySSO server is different than /CinchySSO. |
| -u, --Username                 | **Required**. The user id for accessing Cinchy.                                                                                                                 |
| -p, --Password                 | **Required**. The clear text password (unencrypted) of the specified user.                                                                                      |
| -c, --CinchyCLI Directory      | **Required**. The path to the Cinchy CLI.                                                                                                                       |
| -d, --CinchyCLI Temp Directory | **Required**. The path to the directory that the CLI can use for storing temporary files to support the sync (partitioned data).                                |
| -e                             | **Required** if you are running the CLI via .exe instead of .dll.                                                                                               |

<!-- vale on -->

## Confirm the installation

Once you have installed the experience, your environment will be populated with these tables and queries:

- Forms table
- Forms Sections table
- Form Fields table
- Get Form Metadata query
- Get Form Sections Metadata query
- Get Form Fields Metadata

<figure><img src="../../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

**Forms Table:** This table houses all the existing forms in your environment. Adding an entry to this table, will add a form to the complete list of forms. The table is used to link form fields and form sections to one specific form, and it connects to the table which contains all the form responses/data.

**Form Sections Table:** Use this table to create separation between parts of the form. For instance, when creating a quiz based on compliance, you can use this table to create a section for the technological questions, one for the policy based questions, and one for any possible quiz feedback. A new form section can be created by adding a new entry to this table. The form sections need to be connected to the form created in the Forms table.

**Form Fields Table:** Use this table to create all the possible data entry fields associated with the form sections within the form. To create a new form field, simply add an entry to the table and connect it to the form section. In the example of the quiz, add an entry for every question and link it to the appropriate form section.

**Get Form MetaData Query:** This query will return the metadata of your form, using the form ID present in the Forms table. The results will show such data as whether a form field is mandatory or not, the choices present for each question, the column names and types present in the table containing your form data, the form fields and sections, the JSON data, and more.

**Get Form Sections Query:** This query uses the form ID, present in the Forms table, to return all the form sections present within the form.

For any users who want to access forms/want forms to execute properly, they need to have access to all the following tables and queries. These include the three tables and two queries shown above, as well as the Tables, Table Columns, and Domains tables within that user's specific environment.
