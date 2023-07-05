---
description: >-
  This page details how to change your File Storage configuration in Cinchy v5
  to S3, Azure Blob, or Local.
---

# Change your file storage configuration

## Overview

In v5.2, Cinchy implemented the ability to free up database space by using **S3 compatible or Azure Blob Storage** for file storage. You can set this configuration in the **deployment.json** of a Kubernetes installation, or the **appsettings.json** of an IIS installation.

## Kubernetes file storage

1. If you are using a Kubernetes deployment, you will change your file storage config in the **deployment.json.**
2. **Navigate to** the object storage section, where you will see either S3 or Azure Blob storage, depending on your deployment structure.

#### Azure Example

<pre class="language-json"><code class="lang-json">        "object_storage": {
          // Cinchy requires a new Azure Blob Storage container for it's file storage. Select a unique name, the template convention follows <organization>cinchy<cluster name>
          // Storage Account Names can only consist of lowercase letters and numbers, and must be between 3 and 24 characters long
          "storage_account_name": "<organization>cinchynonprod",
          // Two storage containers are created, one for the Connections component and one for the Platform. The default naming convention is <cluster name>-<component>
          "connections_storage_container_name": "nonprod-connections",
          "platform_storage_container_name": "nonprod-platform",
<strong>          "connections_storage_type": "AzureBlobStorage",
</strong>          // The connection string to the Azure Blob Storage account, it can be retrieved by executing the following command after the terraform apply has completed
          // In the below command the <storage_account_name> and <deployment_resource_group> must be replaced with the values for those properties within this file
          // az storage account show-connection-string --name <storage_account_name> --resource-group <deployment_resource_group>
          "azure_blob_storage_conn_str": ""
        },
</code></pre>

#### AWS Example

<pre class="language-json"><code class="lang-json">        "object_storage": {
          // Cinchy requires a new S3 bucket for it's file storage. Select a unique name, the template convention follows <organization>-<cluster name>
          "cinchy_s3_bucket": "<organization>-cinchy-nonprod",
          // During the S3 bucket creation, a tag named "Environment" is added to the resource and populated with the following value
          "cinchy_s3_environment_tag": "cinchy-nonprod",
<strong>          "connections_storage_type": "S3",
</strong>          // IAM user credentials (access key and secret) for access to this bucket. Ensure that the usre has the necessary privileges defined in IAM
          "connections_s3_access_key": "",
          "connections_s3_secret_access_key": "",
          // Optional - only set this value if you are using a third party S3 compatible service
          "connections_s3_service_url": ""
        },
</code></pre>

3. To utilize **Blob Storage or S3**, update each line with your own parameters.

4. To utilize **Local storage**, leave each line blank with the exception of the **Connections\_Storage\_Type,** which should be set to Local:

```json
          "connections_storage_type": "Local",
```

5\. Run the deployment script by using the following command in the root directory of your **devops.automations repo:**

```bash
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

6. Commit and push your changes.

## IIS file storage

1. If you are using an IIS deployment, you will change your file storage config in the **Cinchy Web appsettings file.**

2. Locate the **StorageType** section of the file and set it to either **"Local", "AzureBlobStorage" or "S3".**

{% code overflow="wrap" %}
```json
  "AppSettings": {
   ...
    "StorageType": ""      // Set this to "Local" to store files within Cinchy's database. Set this to "AzureBlobStorage" to store files within Azure Blob Storage. Set this to "S3" to store files within S3.
  },
```
{% endcode %}

3. If you selected **"AzureBlobStorage",** fill out the following lines in the same file:

```json
  "AzureBlobStorageSettings": {
    "ConnectionString": "",    // ConnectionString used to connected to the Azure Blob Storage
    "Container": "",       // The container for Cinchy's file storage
    "BasePath": ""         // The base directory path of where to store files within the container (eg. cinchy/files)
  },
```

4. If your selected **"S3"**, fill out the following lines in the same file:

```json
  "S3Settings": {
    "AccessKey": "",       // Access Key for the IAM user. Ensure that the user has the necessary privileges defined in IAM
    "SecretAccessKey": "", // Secret for the IAM user. Ensure that the user has the necessary privileges defined in IAM
    "Region": "",          // Region of where the S3 bucket is located in
    "Bucket": "",          // S3 bucket for Cinchy's file storage
    "BasePath": "",            // The base directory path of where to store files within the bucket (eg. cinchy/files)
    "ServiceURL": ""       // (Optional) - only set this value if you are using a third party S3 compatible service
  }
```
