---
description: This page outlines the Cinchy Secrets Manager, added to the platform in v5.7.
---

# Cinchy Secrets Manager

## Overview

The Cinchy platform comes with an out-of-the-box way to store secrets: the Cinchy Secrets Table. Adhering to Cinchy’s Universal Access Controls, you can use this table as a key vault (such as Azure Key Vault or AWS Secrets Manager) to store sensitive data only accessible to the users or user groups that you give access to.

You can reference secrets stored in this table in the Connections UI and use them in places where Cinchy supports variables. Some examples are:

* As part of a connection string
* Within a REST Header, URL, or Body
* In the Listener Configuration

Cinchy has also implemented a new [API endpoint](../../api-guide/api-overview/) for the retrieval of your secrets.

## Configuring a secret

To create a secret in Cinchy:

1. Navigate to the **\[Cinchy].\[Secrets]** table on your platform (Image 1).
2. Input the following values for your secret:

| Value         | Description                                                                                                                    | Example                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| Secret Source | Where your secret is housed. Currently this field only supports 'Cinchy' as a source.                                          | Cinchy                                                                         |
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

You can use secrets stored in the Cinchy Secrets table as a variable for your data syncs anywhere a regular variable can be used. For example, as part of a connection string, access key ID, or within a REST Source or Destination in the Header, URL, or Body**.**

To use a Secret within Connections:

1. In the Connections UI, navigate to the **Info Tab > Variables.**
2. Under the **Variables** section, select **Secret.**
3. Use your defined Variable anywhere a regular variable can be used, such as **within a REST Header** _(Image 3)._

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Image 3: Use your Variable in a REST Header</p></figcaption></figure>

## Use a Secret in real-time syncs

You are also able to use your Cinchy Secrets when configuring your Listener for real-time syncs.

To use a secret in the Listener:

1. When configuring your sync, navigate to the **Info Tab > Variables.**
2. Under the **Variables** section, select **Secret.**
3. Under **Listener** section, use your secrets in the **Topic** or **Connection Attributes** field of your sync. For example:

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
