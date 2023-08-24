---
description: This page outlines the Cinchy Secrets Manager, added to the platform in v5.7.
---

# Cinchy Secrets Manager

## Overview

The Cinchy platform comes with an out-of-the-box way to store secrets: the Cinchy Secrets Table. Adhering to Cinchy’s Universal Access Controls, you can use this table as a key vault (such as Azure Key Vault or AWS Secrets Manager) to store sensitive data only accessible to the users or user groups that you give access to.

You can reference secrets stored in this table in the Connections UI and use them in places where Cinchy supports variables. Some examples are:

* As part of a connection string
* Within a REST Header, URL, or Body
* In the Listener Configuration table.

Cinchy has also implemented a new [API endpoint](../../api-guide/api-overview/) for the retrieval of your secrets.

## Create a secret

To create a secret in Cinchy:

1. Navigate to the **\[Cinchy].\[Secrets]** table on your platform (Image 1).
2. Input the following values for your secret:

| Value         | Description                                                                                                                    | Example                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| Secret Source | Where your secret is housed. This field only supports 'Cinchy' as a source.                                          | Cinchy                                                                         |
| Domain        | The domain name of where your secret resides.                                                                                  | QA                                                                             |
| Name          | The name of your secret.                                                                                                       | Password                                                                       |
| Secret Value  | The value of your secret.                                                                                                      |                                                                                |
| Description   | A brief description of what your secret is.                                                                                    | This secret contains the password information to log in to the QA environment. |
| Read Groups   | A list of User Groups who have read access to your secret. User Groups in this column will be able to call the secret via API. |                                                                                |
| Write Groups  | A list of User Groups who have write access to configure your secret.                                                          |                                                                                |

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Image 1: Cinchy Secrets Table</p></figcaption></figure>

## Call a secret via API

Cinchy has implemented a new [API endpoint](../../api-guide/api-overview/) for the retrieval of your secrets. Using the below endpoint, fill in your `<base-url>`, `<secret-name>`, and the `<domain-name>`to retrieve the referenced secret.

This endpoint works with Cinchy’s [Personal Access Token](../user-guides/user-preferences/personal-access-tokens.md) capability, as well as Access Tokens retrieved from your IDP.

**Blank Example:**

```
<base-url>/api/v1.0/secrets-manager/secret?secretName=<secret-name>&domain=<domain-name>
```

**Populated Example:**

```
Cinchy.net/api/v1.0/secrets-manager/secret?secretName=<ExampleSecret>&domain=<Sandbox>
```

The API will return an object in the below format:

```
{
    "secretValue": "password123"
}
```



## Use a secret as a Connections variable

You can use secrets stored in the Cinchy Secrets table as a variable for your data syncs anywhere you use a regular variable. For example, as part of a connection string, access key ID, or within a REST Source or Destination in the Header.

To use a Secret within Connections:

1. In the Connections UI, navigate to **Info > Variables.**
1. Under the **Variables** section, select **Secret.**
1. Enter the name of your variable.
1. Under the **Value** dropdown, select the secret you want to assign from the **Secrets** table.

## Use a Secret in real-time syncs

You are also able to use your Cinchy Secrets when configuring your Listener for real-time syncs.

To use a secret in the Listener:

1. When configuring your sync, navigate to the **Info Tab > Variables.**
1. Under the **Variables** section, select **Secret.**
1. Enter the name of your variable.
1. Under the **Value** dropdown, select the secret you want to assign from the **Secrets** table.
1. Go to the **Source** tab.
1. Under the **Listener** section, enter the secret variables as values for the relevant property in your **Topic** or **Connection Attribute** fields. 

    For example:

    ```json
    // Lists variables for GrantType, ClientID, Username, Password 
    {
    "InstanceAuthUrl": "@Url",
    "ApiVersion": 41.0,
        "GrantType": "@GrantType",
        "ClientId": "@ClientId",
        "UserName": "@Username",
        "Password": "@Password"
    }
    ```

## Use a secret in the Listener Config table

You can also add a secret that's attached to a variable to the **Topic** or **Connection Attributes** in the Listener Config table. 

1. Open the **Listener Config** table.
1. Select the row that corresponds to your data sync.
1. Select the **Topic** or **Connection Attribute** cell you want to change.
1. Replace the value for a property with the variable assigned to a secret. 

For example, the JSON code below replaces a **Connection Attribute** property, `connectionString`, with the `@connectionString` variable defined in the data sync.

```json
{
	"connectionString": "@connectionString",
	"retryConfiguration": {
		"retryMaxAttempts": "2",
		"retryDelayStrategy": "Linear"
	}
}
```


## Listener Config parameters

The following table lists the applicable Topic and Connection Attributes you can use as parameters or secrets.

| Event Connector Type      | Topic                | Connection Attributes | Value as Parameter/Secrets |
| ------------------------- | -------------------- | --------------------- | -------------------------- |
| Cinchy CDC                | tableGuid            |                       | Yes                        |
|                           | filter               |                       | Yes                        |
|                           | messageKeyExpression |                       | Yes                        |
|                           | batchSize            |                       | No                         |
|                           |                      |                       |                            |
| Salesforce Push Topic     | Name                 |                       | Yes                        |
|                           | Id                   |                       | Yes                        |
|                           | Query                |                       | Yes                        |
|                           |                      | InstanceAuthUrl       | Yes                        |
|                           |                      | GrantType             | Yes                        |
|                           |                      | ClientId              | Yes                        |
|                           |                      | ClientSecret          | Yes                        |
|                           |                      | UserName              | Yes                        |
|                           |                      | Password              | Yes                        |
|                           |                      | ApiVersion            | No                         |
|                           |                      |                       |                            |
| MongoDB Event             | database             |                       | Yes                        |
|                           | collection           |                       | Yes                        |
|                           | pipelineStage        |                       | Yes                        |
|                           |                      | connectionString      | Yes                        |
|                           |                      |                       |                            |
| Data Polling              | FromClause           |                       | Yes                        |
|                           | CursorColumn         |                       | Yes                        |
|                           | FilterCondition      |                       | Yes                        |
|                           | CursorColumnDataType |                       | Yes                        |
|                           | Columns              |                       | Yes                        |
|                           | BatchSize            |                       | No                         |
|                           | Delay                |                       | No                         |
|                           |                      | databaseType          | Yes                        |
|                           |                      | connectionString      | Yes                        |
|                           |                      |                       |                            |
| Kafka Topic               | topicName            |                       | Yes                        |
|                           |                      | bootstrapServers      | Yes                        |
|                           |                      |                       |                            |
| Salesforce Platform Event | Name                 |                       | Yes                        |
|                           |                      | InstanceAuthUrl       | Yes                        |
|                           |                      | GrantType             | Yes                        |
|                           |                      | ClientId              | Yes                        |
|                           |                      | ClientSecret          | Yes                        |
|                           |                      | UserName              | Yes                        |
|                           |                      | Password              | Yes                        |
|                           |                      | ApiVersion            | No                         |
|                           |                      |                       |                            |
| Amazon SQS                | deleteMessages       |                       | No                         |
|                           |                      | awsRegion             | No                         |
|                           |                      | awsAccessKey          | Yes                        |
|                           |                      | awsSecret             | Yes                        |
|                           |                      | queueUrl              | Yes                        |