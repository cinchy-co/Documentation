---
description: >-
  This page details the installation instructions for deploying Cinchy v5 on
  Kubernetes
---

# Kubernetes deployment installation

## Introduction

This page details the instructions for deployment of Cinchy v5 on Kubernetes. We recommend, and have documented below, that this is done via **Terraform** and **ArgoCD.** This setup involves a utility to centralize and streamline your configurations.

The Terraform scripts and instructions provided enable deployment on Azure and AWS cloud environments.

## Deployment prerequisites

To install Cinchy v5 on Kubernetes, you need to follow the requirements below. Some requirements depend on whether you deploy on Azure or on AWS.

### All platforms

These prerequisites apply whether you are installing on Azure or on AWS.

- You must create the following four Git repositories. You can use any source control platform that supports Git, such as [GitLab](https://about.gitlab.com/)**,** [**Azure DevOps**](https://docs.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops), and [**GitHub**](https://github.com/).
  - **cinchy.terraform:**: Contains all Terraform configurations.
  - **cinchy.argocd:**: Contains all ArgoCD configurations.
  - **cinchy.kubernetes:**: Contains cluster and application component deployment manifests.
  - **cinchy.devops.automations:**: Contains the single configuration file and binary utility that maintains the contents of the above three repositories.
- Download the artifacts for the four Git repositories. [See here for information on accessing these.](../deployment-planning-overview-and-checklist/deployment-prerequisites/#1.6-access-to-cinchy-artifacts) Check the contents of each of the directories into the respective repository.
- You must have a service account with read/write permissions to the git repositories created above.
- Install the following tools on the deployment machine:
  - [Terraform](https://www.terraform.io/)
    - For an introduction to Terraform + AWS,[ see this Get started Guide.](https://learn.hashicorp.com/collections/terraform/aws-get-started?utm_source=terraform_io_download)
    - For an introduction to Terraform + Azure, [see this Get started Guide](https://learn.hashicorp.com/collections/terraform/azure-get-started?utm_source=terraform_io_download)
  - [kubectl](https://kubernetes.io/docs/tasks/tools/) (v1.23.0+)
  - [.NET Core 3.1.x](https://dotnet.microsoft.com/en-us/download/dotnet/3.1)
  - [Bash](https://www.gnu.org/software/bash/) ([Git Bash](https://www.atlassian.com/git/tutorials/git-bash) may be used on Windows)
- If you are using Cinchy docker images, [pull them.](../deployment-planning-overview-and-checklist/deployment-prerequisites/#1.6-docker-images)

{% hint style="info" %}
Starting in Cinchy v5.4, you will have the option between Alpine or Debian based image tags for the listener, worker, and connections. **Using Debian tags will allow a Kubernetes deployment to be able to connect to a DB2 data source, and that option should be selected if you plan on leveraging a DB2 data sync.**

- When either installing or upgrading your platform, you can use the following Docker image tags for the **listener, worker, and connections:**

  - **"5.x.x" - Alpine**
  - **"5.x.x-debian" - Debian**
    {% endhint %}

- You will need a single domain for accessing ArgoCD, Grafana, OpenSearch Dashboard, and any deployed Cinchy instances. You have two routing options for accessing these applications - path based or subdomains. See below for an example with multiple Cinchy instances:

| Application    | Path Based Routing   | Subdomain Based Routing        |
|----------------|----------------------|--------------------------------|
| Cinchy 1 (DEV) | domain.com/dev       | dev.mydomain.com               |
| Cinchy 2 (QA)  | domain.com/qa        | qa.mydomain.com                |
| Cinchy 3 (UAT) | domain.com/uat       | uat.mydomain.com               |
| ArgoCD         | domain.com/argocd    | cluster.mydomain.com/argocd    |
| Grafana        | domain.com/grafana   | cluster.mydomain.com/grafana   |
| OpenSearch     | domain.com/dashboard | cluster.mydomain.com/dashboard |


- You will need an **SSL certificate** for the cluster. This should be a wildcard certificate if you will use subdomain based routing. [You can also use Self-Signed SSL.](using-self-signed-ssl-certs-kubernetes-deployments.md)

## Azure requirements

If you are deploying Cinchy v5 on Azure, you require the following:

### Terraform requirements

- A resource group that will contain the Azure Blob Storage with the terraform state.
- A storage account and container (Azure Blob Storage) for persisting terraform state.
- Install the [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on the deployment machine. It must be set to the correct profile/login

The deployment template has two options available:

- Use an existing resource group.
- Creating a new one.

#### Existing resource group

If you prefer an **existing resource group**, you must provision the following before the deployment:

- The resource group.
- A virtual network (VNet) within the resource group.
- A single subnet. It's important that the range be enough for all executing processes within the cluster, such as a CIDR ending with /22 to provide a range of 1024 addresses.

#### New resource group

- If you prefer a **new resource group**, all resources will be automatically provisioned.
- The quota limit of the **Total Regional vCPUs** and the **Standard DSv3 Family vCPUs** (or equivalent) must offer enough availability for the required number of vCPUs (minimum of 24).
- An AAD user account to connect to Azure, which has the necessary privileges to create resources in any existing resource groups and the ability to create a resource group (if required).

## Kubernetes AWS requirements

If you are deploying Cinchy v5 on AWS, you require the following:

### Terraform requirements:

- [An S3 bucket](https://platform.docs.cinchy.com/deployment-guide/deployment-installation-guides/deployment-planning-overview-and-checklist/deployment-architecture-overview#3.3-object-storage-requirements) that will contain the terraform state.
- Install the [AWS CLI](https://aws.amazon.com/cli/) on the deployment machine. It must be set to the correct profile/login

The template has two options available:

- Use an existing VPC
- Create a new one.

#### Existing VPC

- If you prefer an **existing VPC**, you must provision the following before the deployment:
  - The VPC. It's important that the range be enough for all executing processes within the cluster, such as a CIDR ending with /21 to provide a range of 2048 IP addresses.
  - 3 Subnets (one per AZ). It's important that the range be enough for all executing processes within the cluster, such as a CIDR ending with /23 to provide a range of 512 IP addresses.
  - If the subnets are **private**, a NAT Gateway is required to enable node group registration with the EKS cluster.

#### New VPC

- If you prefer a **new VPC**, all resources will be automatically provisioned.
- The limit of the **Running On-Demand All Standard** vCPUs must offer enough availability for the required number of vCPUs (minimum of 24).
- An **IAM user account** to connect to AWS which has the necessary privileges to create resources in any existing VPC and the ability to create a VPC (if required).
- You must import the SSL certificate into AWS Certificate Manager (or a new certificate can be requested via AWS Certificate Manager).
- You must import the SSL certificate [into AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html), or a new certificate can be requested via [AWS Certificate Manager.](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html)
- If you are importing it, you will need the PEM-encoded certificate body and private key. You can find this, you can get the PEM file from your chosen domain provider (GoDaddy, Google, etc.) [Read more on this here.](https://aws.amazon.com/blogs/security/how-to-import-pfx-formatted-certificates-into-aws-certificate-manager-using-openssl/)

{% hint style="success" %}
**Tips for Success:**

- Ensure you have the same **region** configuration across your SSL Certificate, your Terraform bucket, and your **deployment.json** in the next step of this guide.
  {% endhint %}

## Initial configuration

The following steps detail the instructions for setting up the initial configurations.

### Configure the deployment.json file

1. Navigate to your **cinchy.devops.automations repository** where you will see an **aws.json and azure.json.**
2. Depending on platform that you are deploying to, select the appropriate file and copy it into a new file named **deployment.json (or \<cluster name>.json)** within the same directory.
3. This file will contain the configuration for the infrastructure resources and the Cinchy instances to deploy. Each property within the configuration file has comments in-line describing its purpose along with instructions on how to populate it.
4. Follow the guidance within the file to configure the properties.
5. Commit and push your changes.

{% hint style="success" %}
**Tips for Success:**

- You can return to this step at any point in the deployment process if you need to update your configurations. Simply rerun through the guide sequentially after making any changes.
- The **deployment.json** will ask for your **repository username and password**, but ArgoCD may have errors when retrieving your credentials in certain situations (ex: if using GitHub).
  \
   To verify if your credentials are working, navigate to the **ArgoCD Settings** after you have deployed Argo in this guide.
  \
   To avoid errors, Cinchy recommends using a [Personal Access Token instead.](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

      [Find more information here.](https://argo-cd.readthedocs.io/en/release-1.8/user-guide/private-repositories/)

  {% endhint %}

### Execute cinchy.devops.automations

This utility updates the configurations in the cinchy.terraform, cinchy.argocd, and cinchy.kubernetes repositories.

1. From a shell/terminal, navigate to the **cinchy.devops.automations directory** location and execute the following command:

```bash
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

2. If the file created in [**"Configuring the Deployment.json" step 2**](#configure-the-deploymentjson-file) has a name other than `deployment.json`, the reference in the command will will need to be replaced with the correct name of the file.

3. The console output should have the following message:

```bash
Completed successfully
```

## Terraform deployment

The following steps detail how to deploy Terraform.

#### Cinchy.terraform repository structure - AWS

If deploying on AWS: Within the Terraform > AWS directory, a new folder named `eks_cluster` is created. Nested within that's a subdirectory with the same name as the newly created cluster.

To perform terraform operations, **the cluster directory must be the working directory during execution. This applies to everything within step 4 of this guide.**

#### Cinchy.terraform repository structure - Azure

If deploying on Azure: Within the Terraform > Azure directory, a new folder named `aks_cluster` is created. Nested within that's a subdirectory with the same name as the newly created cluster.

To perform terraform operations, **the cluster directory must be the working directory during execution.**

### Cloud provider authentication

1. Launch a shell/terminal with the working directory set to the cluster directory within the **cinchy.terraform** repository.

2. **If you are using AWS,** run the following commands to authenticate the session:

```bash
export AWS_DEFAULT_REGION=REGION
export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=YOUR_ACCESS_KEY
```

3. For Azure, run the following command and follow the on screen instructions to authenticate the session:

```bash
az login
```

### Deploy the infrastructure

1. Execute the following command to create the cluster:

```bash
bash create.sh
```

2. Type **yes** when prompted to apply the terraform changes.

The resource creation process can take about 15 to 20 minutes. At the end of the execution there will be a section with the following header

#### Output variables

If deploying on AWS, this section will contain 2 values: **Aurora RDS Server Host** and **Aurora RDS Password**

If deploying on Azure, this section will contain a single value: **Azure SQL Database Password**

These variable values are required to update the connection string within the deployment.json file (or equivalent) in the **cinchy.devops.automations** repository.

### Retrieve the SSH keys

The following section breaks down how to retrieve your SSH keys for both AWS and Azure deployments.

{% hint style="success" %}
SSH keys **should be saved** for future reference if a connection needs to be established directly to a worker node in the Kubernetes cluster.
{% endhint %}

#### AWS SSH keys

1. The SSH key to connect to the Kubernetes nodes is maintained within the terraform state and can be retrieved by executing the following command:

```bash
terraform output -raw private_key
```

#### Azure SSH keys

1. The SSH key is output to the directory containing the cluster terraform configurations.

## Update the deployment.json

The following section pertains to updating the Deployment.json file.

### Update the database connection string

1. Navigate to the **deployment.json (**[**created in step 3.1**](#configure-the-deploymentjson-file)**) > cinchy_instance_configs section.**
2. Each object within represents an instance that will be deployed on the cluster. Each instance configuration has a `database_connection_string` property. This has placeholders for the **host name and password** that must be updated using output variables from the previous section.

{% hint style="warning" %}
For Azure deployments, the host name isn't available as part of the terraform output and instead must be sourced from the Azure Portal.
{% endhint %}

### Create the IAM user for S3 (AWS)

The terraform script will create an S3 bucket for the cluster that must be accessible to the Cinchy application components.

To access this programmatically, an IAM user that has read/write permissions to the new S3 bucket is required. **This can be an existing user.**

The Access Key and Secret Access Key for the IAM user must be specified under the `object_storage` section of the **deployment.json**

### Update blob storage connection details (Azure)

1. Within the **deployment.json**, the `azure_blob_storage_conn_str` must be set.
2. The in-line comments outline the commands required to source this value from the Azure CLI.

### Enable Azure Key Vault secrets

If you have the `key_vault_secrets_provider_enabled=true` value in the **azure.json** then the below secrets files would have been created during the execution of step 3.2:

<figure><img src="../../../.gitbook/assets/image (566).png" alt=""><figcaption></figcaption></figure>

You will need to add the following secrets to your Azure Key Vault:

- **worker-secret-appsettings-\<cinchy_instance_name>**
- **web-secret-appsettings-\<cinchy_instance_name>**
- **maintenance-cli-secret-appsettings-\<cinchy_instance_name>**
- **idp-secret-appsettings-\<cinchy_instance_name>**
- **forms-secret-config-\<cinchy_instance_name>**
- **event-listener-secret-appsettings-\<cinchy_instance_name>**
- **connections-secret-config-\<cinchy_instance_name>**
- **connections-secret-appsettings-\<cinchy_instance_name>**

To create your new secrets:

1. Navigate to your key vault in the **Azure portal.**
2. Open your **Key Vault Settings** and select **Secrets.**
3. Select **Generate/Import.**
4. On the **Create a Secret** screen, choose the following values:
   1. Upload options: **Manual.**
   2. Name: Choose the secret name from the **above list.** They will all follow the format of: **\<app>-secret-appsettings-\<cinchy_instance_name>** or **\<app>-secret-config-\<cinchy_instance_name>**
   3. Value: The value for the secret will be the content of each app JSON located in the **cinchy.kubernetes\environment_kustomizations\nonprod\<cinchy_instance_name>\secrets** folder, and as shown in above image.
   4. Content type: **JSON**
5. Leave the other values to their defaults.
6. Select **Create.**

Once you receive the message that the first secret has been successfully created, you may proceed to create the other secrets. You must **create a total of 8 secrets**, as shown in the above list of secret names.

### Execute cinchy.devops.automations

This utility updates the configurations in the cinchy.terraform, cinchy.argocd, and cinchy.kubernetes repositories.

1. From a shell/terminal, navigate to the **cinchy.devops.automations** directory and execute the following command:

```
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

2. If the file [created in section 3](./#3.-initial-configuration) has a name other than `deployment.json`, the reference in the command will will need to be replaced with the correct name of the file.

3. The console output should end with the following message:

```bash
Completed successfully
```

4. The updates must be committed to Git before proceeding to the next step.

## Connect with kubectl

### Update the Kubeconfig

#### AWS

1. From a shell/terminal run the following command, replacing **\<region>** and **\<cluster_name>** with the accurate values for those placeholders:

```
aws eks update-kubeconfig --region <region> --name <cluster_name>
```

#### Azure

1. From a shell/terminal run the following commands, replacing **\<subscription_id**_**>**, **<**_**deployment**_**\_**_**resource_group>,** and **\<cluster_name>** with the accurate values for those placeholders.

{% hint style="info" %}
These commands with the values pre-populated can also be found from the Connect panel of the AKS Cluster in the Azure Portal.
{% endhint %}

```
az account set --subscription <subscription_id>
az aks get-credentials --admin --resource-group <deployment_resource_group> --name <cluster_name>
```

### Verify the connection

1. Verify that the connection has been established and the context is the correct cluster by running the following command:

```
kubectl config get-contexts
```

## Deploy and access ArgoCD

In this step, you will deploy and access ArgoCD.

### Deploy ArgoCD

1. Launch a shell/terminal with the working directory set to **the root of the cinchy.argocd** repository.
2. Execute the following command to deploy ArgoCD:

```bash
bash deploy_argocd.sh
```

3. Monitor the pods within the ArgoCD `namespace` by running the following command every 30 seconds until they all move into a healthy state:

<pre><code><strong>kubectl get pods -n argocd
</strong></code></pre>

### Access ArgoCD

1. Launch a new shell/terminal with the working directory set to the root of the cinchy.argocd repository.
2. Execute the following command to access ArgoCD:

```bash
bash access_argocd.sh
```

This script creates a port forward using kubectl to enable ArgoCD to be accessed at **http://localhost:9090**

The credentials for ArgoCD's portal are output at the start of the `access_argocd` script execution in Base64. [The Base64 value must be decoded](https://www.base64decode.org/) to get the login credentials to use for the **http://localhost:9090** endpoint.

## Deploy cluster components

In this step, you will deploy your cluster components.

### Deploy ArgoCD applications

1. Launch a shell/terminal with the working directory set to the root of the **cinchy.argocd repository.**
2. Execute the following command to deploy the cluster components using ArgoCD:

```bash
bash deploy_cluster_components.sh
```

3. Navigate to ArgoCD at **http://localhost:9090** and login. Wait until all components are healthy (this may take a few minutes).

{% hint style="success" %}
**Tips for Success:**

- If your pods are degraded or failed to sync, refresh of resynchronize your components. You can also delete pods and ArgoCD will automatically spin them back up for you.
- Check that ArgoCD is pulling from your git repository by navigating to your Settings
- If your components are failing upon attempting to pull an image, refer to your deployment.json to check that each component is set to the correct version number.
  {% endhint %}

### Update the DNS

1. Execute the following command to get the External IP used by the Istio ingress gateway.

```
kubectl get svc -n istio-system
```

2. DNS entries must be created using the External IP for any subdomains / primary domains that will be used, including OpenSearch, Grafana, and ArgoCD.

### Access OpenSearch

The default path to access OpenSearch, unless you have configured it otherwise in your deployment.json, is **\<baseurl>/dashboard**

{% hint style="info" %}
The default credentials for accessing OpenSearch are **admin/admin. We recommend that you change these credentials the first time you log in to OpenSearch.**

**To change the default credentials for Cinchy v5.4+**, [follow the documentation here.](https://platform.docs.cinchy.com/guides-for-using-cinchy/additional-guides/monitoring-and-logging-on-kubernetes/opensearch-dashboards#3.-updating-your-opensearch-password)

To change the default credentials and/or add new users for all **other deployments**, follow [this documentation](https://opensearch.org/docs/1.2/security-plugin/access-control/users-roles/#create-users) or navigate to Settings > Internal Roles in OpenSearch.”
{% endhint %}

### Access Grafana

The default path to access Grafana, unless you have configured it otherwise in your deployment.json, is **\<baseurl>/grafana**

{% hint style="info" %}
The default username is **admin**. The default password for accessing Grafana can be found by doing a search of `adminPassword` within the following path: **cinchy.kubernetes/cluster_components/metrics/kube-prometheus-stack/values.yaml**

**We recommend that you change these credentials the first time you access Grafana. You can do so through the admin profile once logged in.**
{% endhint %}

## Deploy Cinchy components

In this step, you will deploy your Cinchy components.

### Deploy ArgoCD application

1. Launch a shell/terminal with the working directory set to the root of the **cinchy.argocd repository.**
2. Execute the following command to deploy the Cinchy application components using ArgoCD:

```
bash deploy_cinchy_components.sh
```

3. Navigate to ArgoCD at **http://localhost:9090** and login. Wait until all components are healthy (may take a few minutes)

4. You will be able to access ArgoCD through the **URL that you configured in your deployment.json**, as long as you created a DNS entry for it in step [8.2.](./#8.2-update-the-dns)

{% hint style="info" %}
You have now finished the deployment steps required for Cinchy. Navigate to your configured domain URL to verify that you can login using the default username (**admin**) and password (**cinchy**).
{% endhint %}

## Troubleshooting

- If ArgoCD Application Sync is stuck waiting for PreSync jobs to complete, you can run the below command to restart the application controller.

```bash
kubectl rollout restart sts argocd-application-controller -n argocd
```
