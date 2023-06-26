---
description: >-
  This page contains information on how to leverage Active Directory groups
  within Cinchy.
---

# AD Group Integration

## Table of Contents

| Table of Contents                                                                                       |
| ------------------------------------------------------------------------------------------------------- |
| [#1.-group-management](ad-group-integration.md#1.-group-management "mention")                           |
| [#2.-group-membership-sync](ad-group-integration.md#2.-group-membership-sync "mention")                 |
| [#3.-high-number-of-groups-in-adfs](ad-group-integration.md#3.-high-number-of-groups-in-adfs "mention") |

## 1. Group Management

This section defines how to manage Groups.

### 1.1 Cinchy Groups

Cinchy Groups are containers that have **Users** and other **Groups** within them as members. They are used to provision access controls throughout the platform. Cinchy Groups enable **centralized administration for access controls**.

Groups are defined in the **"Groups"** table within the Cinchy domain. By default this table can only be managed by members of the Cinchy Administrators group. Each group has the following attributes:

| Attribute        | Definition                                                                                                                                                                                                                                                                                           |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Name**         | The Group Name. This must be unique across all groups within the system.                                                                                                                                                                                                                             |
| **Users**        | The Users which are members of the group                                                                                                                                                                                                                                                             |
| **User Groups**  | The Groups which are members of the group                                                                                                                                                                                                                                                            |
| **Owners**       | Users who are able to administer memberships to the group. By default, Owners are also members of the group and this do not need to also be added into the Users category.                                                                                                                           |
| **Owner Groups** | Groups whose members are able to administer the membership of the group. By default, members of Owner Groups are also members of the group itself, and thus do not need to also be added into the User or User Groups category.                                                                      |
| **Group Type**   | <p>This will be either <strong>"Cinchy Group"</strong> or <strong>"AD Group".</strong><br><strong>"Cinchy Group":</strong> The membership is maintained directly in Cinchy.<br><strong>"AD Group":</strong> A sync process will be leveraged to maintain the membership and overwrite the Users.</p> |

### 1.2 How to Define a New AD Group

1. To define a new AD Group, create a new record within the Groups Table with the same name as the AD Group (using the cn attribute).
2. Set the Group Type to **"AD Group".**

### 1.3 How to Convert an Existing Group to Sync with AD

1. To convert an existing group, update the **Name** attribute of the existing group record to match the AD Group (using the cn attribute).
2. Set the Group Type to **"AD Group".**

## 2. Group Membership Sync

AD Groups defined in Cinchy have their members synced from AD through a batch process that leverages the [Cinchy Command Line Interface (CLI). ](../../../../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)

### 2.1 Execution Flow

&#x20;The sync operation performs the following high-level steps:

1. Fetches all Cinchy registered AD Groups using a Saved Query.
2. Retrieves the usernames of all members for each AD Group. The default attribute for username that is retrieved is **"userPrincipalName"**, but configurable as part of the sync process.
3. For each AD Group, it loads the users that are both a member in AD and exist in the Cinchy Users table (matched on the Username) into the "Users" attribute of the Cinchy Groups table.

### 2.2 Dependencies

1. The Cinchy CLI Model must be installed in your instance of Cinchy. [See here](../../../../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md) for more details.
2. An instance of the Cinchy CLI must be available to execute the sync.
3. A task scheduler is required to perform the sync on a regular basis (e.g. Autosys).

### 2.3 Configuration Steps

#### 2.3.1 Create a Saved Query to retrieve AD Groups from Cinchy

1. Create a **new query** within Cinchy with the below CQL to fetch all AD Groups from the Groups table. The domain & name assigned to the query will be referenced in the subsequent step.

{% code title="SavedQuery" %}
```sql
SELECT [Name]
FROM [Cinchy].[Cinchy].[Groups]
WHERE [Group Type] = 'AD Group' 
AND [Deleted] IS NULL
```
{% endcode %}

#### 2.3.2 Create the Sync Config

1. Copy the below XML into a text editor of your choice and update the attributes listed in the table below the XML to align to your environment specific settings.&#x20;
2. Once complete, create an entry with the config in your **Data Sync Configurations** table (part of the Cinchy CLI model).

{% code title="ConfigXML" %}
```markup
<?xml version="1.0" encoding="utf-16" ?>
<BatchDataSyncConfig name="AD_GROUP_SYNC" version="1.0.0" xmlns="http://www.cinchy.co">
  <Parameters />
  <LDAPDataSource objectCategory="group" ldapserver="LDAP:\\activedirectoryserver.domain.com" username="encryptedUsername" password="encryptedPassword" >
    <Schema>
      <Column name="cn" ordinal="1" dataType="Text" maxLength="5000" isMandatory="false" validateData="false" trimWhitespace="true" description=""/>
      <Column name="member.userPrincipalName" ordinal="2" dataType="Text" maxLength="200" isMandatory="false" validateData="false" trimWhitespace="true" description=""/>
    </Schema>
    <Filter>
      lookup('Domain Name','Query Name')
    </Filter>
  </LDAPDataSource>
  <CinchyTableTarget model="" domain="Cinchy" table="Groups" suppressDuplicateErrors="false">
    <ColumnMappings>
      <ColumnMapping sourceColumn="cn" targetColumn="name" />
      <ColumnMapping sourceColumn="member.userPrincipalName" targetColumn="Users" linkColumn="Username" />
    </ColumnMappings>
    <SyncKey>
      <SyncKeyColumnReference name="name" />
    </SyncKey>
	<ChangedRecordBehaviour type="UPDATE" />
    <DroppedRecordBehaviour type="IGNORE" />
  </CinchyTableTarget>
</BatchDataSyncConfig>
```
{% endcode %}

| XML Tag                  | Attribute   | Content                                                                                                                                                                         |
| ------------------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LDAPDataSource           | ldapserver  | <p>The LDAP server url</p><p>(e.g. LDAP:\\activedirectoryserver.domain.com)</p>                                                                                                 |
| LDAPDataSource           | username    | <p>The encrypted username to authenticate with the AD server</p><p>(generated using the CLI's encrypt command - </p><p>dotnet Cinchy.CLI.dll encrypt -t "Domain/username").</p> |
| LDAPDataSource           | password    | <p>The encrypted password to authenticate with the AD server</p><p>(generated using the CLI's encrypt command -</p><p>dotnet Cinchy.CLI.dll encrypt -t "password").</p>         |
| LDAPDataSource -> Filter | Domain Name | The domain of the Saved Query that retrieves AD Groups                                                                                                                          |
| LDAPDataSource -> Filter | Query Name  | The name of the Saved Query that retrieves AD Groups                                                                                                                            |

{% hint style="info" %}
If the `userPrincipalName` attribute in Active Directory does not match what you expect to have as the Username in the Cinchy Users table (e.g. if the SAML token as part of your SSO integration returns a different ID), then you must replace`userPrincipalName`in the XML config with the expected attribute.

&#x20;The `userPrincipalName` appears twice in the XML, once in the LDAPDataSource Columns and once in the CinchyTableTarget ColumnMappings.
{% endhint %}

#### 2.3.3 Sync Execution & Scheduling

1. The below CLI command ([see here](../../../../../data-syncs/cli-commands-list.md) for additional information on the syncdata command) should be used to execute the sync.&#x20;
2. Update the command parameters (described in the table below) with your environment specific settings.&#x20;
3. Execution of this command can be scheduled at your desired frequency using your scheduler of choice.

```
dotnet Cinchy.CLI.dll syncdata -s cinchyAppServer -u username -p "encryptedPassword" -m "Model" -f "AD_GROUP_SYNC" -d "TempDirectory" 
```

{% hint style="info" %}
The user account credentials provided in above CLI syncdata command must have View/Edit access to Cinchy Groups table.
{% endhint %}

## Parameters

<details>

<summary>SyncData Parameters</summary>

* **-h, -https:** Flag indicating connections to Cinchy should be over https.
* **-s, --server:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The full path to the Cinchy server without the protocol (e.g. cinchy.co/Cinchy).
* **-u, --userid: **<mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The user id to login to Cinchy.
* **-p, --password:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The password of the specified user. This can be optionally encrypted using the CLI's encrypt command.
* **-f, --feed: **<mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The name of the feed configuration as defined in Cinchy.
* **-d, --tempdirectory:** <mark style="color:red;">**Only applies to Cinchy v4.**</mark>\
  **Required**. The path to a directory that the CLI can use for storing temporary files to support the sync (e.g. error files).
* **-b, --batchsize:** (Default: 5000) The number of rows to sync per batch (within a partition) when executing inserts/updates.
* **-z, --retrievalbatchsize:** (Default: 5000) The max number of rows to retrieve in a single batch from Cinchy when downloading data.
* **-v, --param-values:** Job parameter values defined as one or more name value pairs delimited by a colon (i.e. -v name1:value1 name2:value2).
* **--file:** Works exactly as -v but it is for parameters that are files.
* **--help:** Displays the help screen with the options.
* **-w, --writetofile**: Write the data from Cinchy to disk, required for large data sets exceeding 2GB.

</details>

## 3. High Number of Groups in ADFS

If you are syncing someone with a lot of ADFS groups, the server may reject the request for the header being too large. If you are able to login as a user with a few groups in ADFS but run into an error with users with a lot of ADFS groups (regardless of if those ADFS groups are in Cinchy), you will need to make the following changes:

### 3.1 Update the Server Max Request Header Size <a href="#server-max-request-header-size" id="server-max-request-header-size"></a>

1. Follow the instructions [**outlined in this document.**](https://docs.microsoft.com/en-US/troubleshoot/developer/webapps/iis/www-administration-management/http-bad-request-response-kerberos)

### 3.2 CinchySSO App Settings <a href="#cinchysso-app-settings" id="cinchysso-app-settings"></a>

1. In your **CinchySSO app settings**, you will also need to increase the max size of the request, as follows:

```
"AppSettings": {
  ...
  "MaxRequestHeadersTotalSize": {max size in bytes},
  "MaxRequestRequestBufferSize": {max size in bytes, use same as above},
  "MaxRequestBodySize": -1
}
```
