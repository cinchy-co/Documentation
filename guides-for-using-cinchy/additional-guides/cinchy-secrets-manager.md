---
description: This page outlines the Cinchy Secrets Manager, added to the platform in v5.7.
---

# Cinchy Secrets Manager

## Overview

The Cinchy platform comes with an out-of-the-box way to store secrets — **the Cinchy Secrets Table.** Adhering to Cinchy’s Universal Access Controls, you can utilize this table as a key vault (such as Azure Key Vault or AWS Secrets Manager) to store sensitive data only accesible to the users or user groups that you give access to.

You can use secrets stored in this table when configuring data syncs:

* As part of a connection string;
* Within a REST Header, URL, or Body;
* In the Listener Configuration;

Additionally, we have implemented a new [API endpoint](broken-reference) for the retrieval of your secrets.

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

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Image 1: Cinchy Secrets Table</p></figcaption></figure>

## Calling a secret via API

We have implemented a new [API endpoint](broken-reference) for the retrieval of your secrets. Using the below endpoint, fill in your \<base-url>, \<secret-name>, and the \<domain-name> to retrieve the referenced secret.

This endpoint works with Cinchy’s [Personal Access Token](../user-guides/user-preferences/personal-access-tokens.md) capability, as well as Access Tokens retrieved from your IDP.

**Blank Example:**

```
<base-url>/api/v1.0/secrets-manager/secret?secretName=<secret-name>&domain=<domain-name>
```

**Populated Example:**&#x20;

```
Cinchy.net/api/v1.0/secrets-manager/secret?secretName=<ExampleSecret>&domain=<Sandbox>
```

The API will return an object in the below format:

```
{
    "secretValue": "password123"
}
```

## Using a secret as a Connections variable

You can use secrets stored in the Cinchy Secrets table as a variable for your data syncs -- as part of a connection string or within a REST Source or Destination (in the Header, URL, or Body).

To use a Secret within Connections:

1. When configuring your sync, navigate to the **Info Tab > Variables.**
2. Use the **GETSECRET formula**, [described here](../../data-syncs/building-data-syncs/advanced-settings/variables.md#2.-supported-formulas), to configure your secret _(Image 2)._

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Image 2: Configure your Variable</p></figcaption></figure>

</div>

3. Use your defined Variable within **any connection string** or **within a REST URL, Body, or Header** _(Image 3)._

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Image 3: Use your Variable in a REST Header</p></figcaption></figure>

## Using a Secret in the Listener Config

You are also able to call and use your Cinchy Secrets when configuring your Listener for real-time syncs.

To call a secret in the LIstener:

1. When configuring your sync, navigate to the **Info Tab > Variables.**
2. Use the **GETSECRET formula**, [described here](../../data-syncs/building-data-syncs/advanced-settings/variables.md#2.-supported-formulas), to configure your secret _(Image 4)._

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Image 4: Configure your Variable</p></figcaption></figure>

</div>

3. Navigate the the **\[Cinchy].\[Listener Config]** table in your Cinchy platform.
4. Navigate to the row pertaining to the data sync you want to configure.
5. Using the below as a guide, update the **"Parameters" column** _(Image 5)._

{% code overflow="wrap" %}
```json
// Update the below values, where "name" and "formula" are equal to the values set in step 2.
{
    "parameters": [
        {
            "name": "mySecret"
            "formula": "GETSECRET('domain','name')"
        }
    ]
}
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Image 5: Listener Config Parameters Column</p></figcaption></figure>

6. You are able to use your secret in the **Topic or Connection Attributes column** of your sync _(Image 6)_, for example:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Image 6: Listener Config Connection Attributes</p></figcaption></figure>
