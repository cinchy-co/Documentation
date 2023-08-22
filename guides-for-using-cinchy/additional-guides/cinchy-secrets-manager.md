---
description: This page outlines the Cinchy Secrets Manager, added to the platform in v5.7.
---

# Cinchy secrets manager

## Overview

The Cinchy platform provides a built-in solution for securely storing secrets known as the Cinchy Secrets Table. Built with adherence to Cinchy’s Universal Access Controls, this table functions as a key vault similar to services like Azure Key Vault or AWS Secrets Manager. It allows you to store sensitive data that's accessible only to specific users or user groups with authorized access.

You can refer to secrets stored within this table in the Connections UI and use them wherever Cinchy supports variables. Some common use cases include:

- Including them in a connection string.
- Using them in REST Headers, URLs, or the request body.
- Configuring the Listener via the Listener Configuration table.

Cinchy has also introduced a new [API endpoint](../../api-guide/api-overview/) for retrieving your stored secrets.

## Creating a secret

To create a secret in Cinchy:

1. Navigate to the **\[Cinchy].\[Secrets]** table on your platform (see Image 1).
2. Provide the following details for your secret:

| Field         | Description                                                                                           | Example                                                                |
| ------------- | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Secret Source | The location where the secret is stored. This field supports only 'Cinchy' as a source.               | Cinchy                                                                 |
| Domain        | The domain name of the location where the secret is stored.                                           | QA                                                                     |
| Name          | The identifier for your secret.                                                                       | Password                                                               |
| Secret Value  | The actual secret content.                                                                            | YourSecretValueHere                                                    |
| Description   | A brief explanation of the secret's purpose.                                                          | This secret contains the password for logging into the QA environment. |
| Read Groups   | A list of User Groups with read access to the secret. These groups can access the secret via the API. | GroupA, GroupB                                                         |
| Write Groups  | A list of User Groups with write access to configure the secret.                                      | GroupC, GroupD                                                         |

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Image 1: Cinchy Secrets Table</p></figcaption></figure>


## Call a secret via API

Cinchy has a new [API endpoint](../../api-guide/api-overview/) designed for retrieving secrets. By utilizing the endpoint provided below, you can specify the `<base-url>`, `<secret-name>`, and `<domain-name>` to retrieve the desired secret.

This endpoint functions seamlessly with Cinchy’s [Personal Access Token](../user-guides/user-preferences/personal-access-tokens.md) capability, along with Access Tokens obtained from your Identity Provider (IDP).

**Blank Example:**

```http
<base-url>/api/v1.0/secrets-manager/secret?secretName=<secret-name>&domain=<domain-name>
```

**Populated Example:**

The example below uses `ExampleSecret` as a secretName and `Sandbox` as the domain:

```
Cinchy.net/api/v1.0/secrets-manager/secret?secretName=<ExampleSecret>&domain=<Sandbox>
```

The API response will be in the following format:

```
{
    "secretValue": "password123"
}
```

## Use a secret as a connections variable

You can use secrets stored in the Cinchy Secrets table as variables for your data syncs, wherever you use a regular variable. For instance, you can incorporate them within a connection string, an access key ID, or within a REST Source or Destination in the Header.

To use a Secret within Connections:

1. In the Connections UI, navigate to **Info > Variables**.
2. Under the **Variables** section, select **Secret**.
3. Enter the name of your variable.
4. Under the **Value** dropdown, select the secret you want to assign from the **Secrets** table.

## Use a secret in real-time syncs

You can also use your Cinchy Secrets when configuring your Listener for real-time syncs.

To use a secret in real-time syncs:

1. When configuring your sync, navigate to the **Info Tab > Variables**.
2. Under the **Variables** section, choose **Secret**.
3. Input the name of your variable.
4. Under the **Value** dropdown, choose the secret you intend to assign from the **Secrets** table.
5. Go to the **Source** tab.
6. Within the **Listener** section, input the secret variables as values for the relevant property in your **Topic** or **Connection Attribute** fields.

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

For example, in the JSON code below, the Connection Attribute property `connectionString` is replaced with the `@connectionString` variable defined in the data sync.

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