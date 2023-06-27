---
description: >-
  This page details the installation instructions for deploying Cinchy v5 on
  Kubernetes
---

# Kubernetes Deployment Installation

## Table of Contents

| Table of Contents                                                            |
| ---------------------------------------------------------------------------- |
| [#1.-introduction](./#1.-introduction "mention")                             |
| [#2.-deployment-prerequisites](./#2.-deployment-prerequisites "mention")     |
| [#3.-initial-configuration](./#3.-initial-configuration "mention")           |
| [#4.-terraform-deployment](./#4.-terraform-deployment "mention")             |
| [#5.-update-the-deployment.json](./#5.-update-the-deployment.json "mention") |
| [#6.-connect-with-kubectl](./#6.-connect-with-kubectl "mention")             |
| [#7.-deploy-and-access-argocd](./#7.-deploy-and-access-argocd "mention")     |
| [#8.-deploy-cluster-components](./#8.-deploy-cluster-components "mention")   |
| [#9.-deploy-cinchy-components](./#9.-deploy-cinchy-components "mention")     |
| [#10. Troubleshooting](./#10.-troubleshooting)                               |

## 1. Introduction

This page details the instructions for deployment of Cinchy v5 on Kubernetes. We recommend, and have documented below, that this is done via **Terraform** and **ArgoCD.** This setup involves a utility to centralize and streamline your configurations.

The Terraform scripts and instructions provided enable deployment on Azure and AWS cloud environments.

## 2. Deployment Prerequisites

The following lists are required prerequisites for installing Cinchy v5 on Kubernetes.&#x20;

Note that some prerequisites will depend on whether you are deploying on Azure or on AWS.

### 2.1 All Platforms

These prerequisites apply whether you are installing on Azure or on AWS.

* The following four Git repositories must be created. Any source control platform supporting Git may be used, e.g. [**Gitlab**](https://about.gitlab.com/)**,** [**Azure DevOps**](https://docs.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)**,** [**Github**](https://github.com/)
  * **cinchy.terraform:** This repo contains all Terraform configurations.
  * **cinchy.argocd:** This repo contains all ArgoCD configurations.
  * **cinchy.kubernetes:** This repo contains cluster and application component deployment manifests.
  * **cinchy.devops.automations:** This repo contains the single configuration file and binary utility that maintains the contents of the above three repositories.
* Download the artifacts for the four Git repositories. [See here for information on accessing these.](../deployment-planning-overview-and-checklist/deployment-prerequisites/#1.6-access-to-cinchy-artifacts) Check the contents of each of the directories into the respective repository.
* You must have a service account with read/write permissions to the git repos created above.
* The following tools should be installed on the machine where the deployment will run:
  * [Terraform](https://www.terraform.io/)
    * For an introductory guide on Terraform + AWS,[ see here](https://learn.hashicorp.com/collections/terraform/aws-get-started?utm\_source=terraform\_io\_download)
    * For an introductory guide on Terraform + Azure, [see here](https://learn.hashicorp.com/collections/terraform/azure-get-started?utm\_source=terraform\_io\_download)
  * [Kubectl](https://kubernetes.io/docs/tasks/tools/) (v1.23.0+)
  * [.NET Core 3.1.x](https://dotnet.microsoft.com/en-us/download/dotnet/3.1)
  * [Bash](https://www.gnu.org/software/bash/) ([Git Bash](https://www.atlassian.com/git/tutorials/git-bash) may be used on Windows)
* If you are using Cinchy's docker images, [pull them.](../deployment-planning-overview-and-checklist/deployment-prerequisites/#1.6-docker-images)

{% hint style="info" %}
Starting in Cinchy v5.4, you will have the option between Alpine or Debian based image tags for the listener, worker, and connections. **Using Debian tags will allow a Kubernetes deployment to be able to connect to a DB2 data source, and that option should be selected if you plan on leveraging a DB2 data sync.**

* When either installing or upgrading your platform, you can use the following Docker image tags for the **listener, worker, and connections:**
  * **"5.x.x" - Alpine**
  * **"5.x.x-debian" - Debian**
{% endhint %}

* You will need a single domain for accessing ArgoCD, Grafana, Opensearch Dashboard, and any deployed Cinchy instances. There are two routing options for accessing these applications - path based or subdomains. See below for an example with multiple cinchy instances:

<table><thead><tr><th width="153.43148487423616">Application</th><th width="150">Path Based Routing</th><th>Subdomain Based Routing</th></tr></thead><tbody><tr><td>Cinchy 1 (Dev)</td><td>domain.com/dev</td><td>dev.mydomain.com</td></tr><tr><td>Cinchy 2 (QA)</td><td>domain.com/qa</td><td>qa.mydomain.com</td></tr><tr><td>Cinchy 3 (UAT)</td><td>domain.com/uat</td><td>uat.mydomain.com</td></tr><tr><td>ArgoCD</td><td>domain.com/argocd</td><td>cluster.mydomain.com/argocd</td></tr><tr><td>Grafana</td><td>domain.com/grafana</td><td>cluster.mydomain.com/grafana</td></tr><tr><td>Opensearch</td><td>domain.com/dashboard</td><td>cluster.mydomain.com/dashboard</td></tr></tbody></table>

* You will need an **SSL certificate** for the cluster. This should be a wildcard certificate if you will use subdomain based routing. [You can also use Self-Signed SSL.](using-self-signed-ssl-certs-kubernetes-deployments.md)

### 2.2 Azure Deployment

The following prerequisites are required if you are deployment Cinchy v5 on Azure.

* Terraform Backend Requirements:
  * A resource group that will contain the Azure Blob Storage with the terraform state.
  * A storage account and container (Azure Blob Storage) for persisting terraform state.
* The [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) should be installed on the machine where the deployment will be run. It must be set to the correct profile/login
* The deployment template has the option of either leveraging an existing resource group or creating a new one:
  * If an **existing resource group** is preferred, the prerequisite requires the following be provisioned in advance of the deployment:
    * The resource group.
    * A VNet within the resource group.
    * A single subnet. It's important that the address range be sufficient for all executing processes within the cluster, e.g. a CIDR ending with /22 to provide a range of 1024 IPs.
  * If a **new resource group** is preferred, all resources will be automatically provisioned.
* The quota limit of the **Total Regional vCPUs** and the **Standard DSv3 Family vCPUs** (or equivalent) must provide sufficient availability for the required number of vCPUs (minimum of 24).
* An AAD user account to connect to Azure, which has the necessary privileges to create resources in any existing resource groups and the ability to create a resource group (if required).

### 2.3 AWS Deployment

The following prerequisites are required if you are deployment Cinchy v5 on AWS.

* Terraform Backend Requirements:
  * [An S3 bucket](https://platform.docs.cinchy.com/deployment-guide/deployment-installation-guides/deployment-planning-overview-and-checklist/deployment-architecture-overview#3.3-object-storage-requirements) that will contain the terraform state.
* The [AWS CLI](https://aws.amazon.com/cli/) should be installed on the machine where the deployment will be run. It must be set to the correct profile/login
* The template has the option of either leveraging an existing VPC or creating a new one:
  * If an **existing VPC** is preferred, the prerequisite requires the following be provisioned in advance of the deployment:
    * The VPC. It's important that the address range be sufficient for all executing processes within the cluster, e.g. a CIDR ending with /21 to provide a range of 2048 IPs.
    * 3 Subnets (one per AZ). It's important that the address range be sufficient for all executing processes within the cluster, e.g. a CIDR ending with /23 to provide a range of 512 IPs.
    * If the subnets are **private**, a NAT Gateway is required to enable node group registration with the EKS cluster.
  * If a **new VPC** is preferred, all resources will be automatically provisioned.
* The limit of the **Running On-Demand All Standard** vCPUs must provide sufficient availability for the required number of vCPUs (minimum of 24).
* An **IAM user account** to connect to AWS which has the necessary privileges to create resources in any existing VPC and the ability to create a VPC (if required).
* The SSL certificate must be [imported into AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html), or a new certificate can be requested via [AWS Certificate Manager.](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html)
  * If your are importing it, you will need the PEM-encoded certificate body and private key. You can find this, you can get the PEM file from your chosen domain provider (GoDaddy, Google, etc.) [Read more on this here.](https://aws.amazon.com/blogs/security/how-to-import-pfx-formatted-certificates-into-aws-certificate-manager-using-openssl/)

{% hint style="success" %}
**Tips for Success:**

* Ensure that your **region** is configured the same across your SSL Certificate, your Terraform bucket, and your deployment.json in the next step of this guide.
{% endhint %}

## 3. Initial Configuration

The following steps detail the instructions for setting up the initial configurations.

### 3.1 Configure the Deployment.json

1. Navigate to your **cinchy.devops.automations repository** where you will see an **aws.json and azure.json.**&#x20;
2. Depending on the cloud platform that you are deploying to, select the appropriate file and copy it into a new file named **deployment.json (or \<cluster name>.json)** within the same directory.&#x20;
3. This file will contain the configuration for the infrastructure resources and the Cinchy instances to deploy. Each property within the configuration file has comments in-line describing its purpose along with instructions on how to populate it.&#x20;
4. Follow the guidance within the file to configure the properties.

5\. Commit and push your changes.

{% hint style="success" %}
**Tips for Success:**

* You can return to this step at any point in the deployment process if you need to update your configurations. Simply rerun through the guide sequentially after making any changes.
*   The **deployment.json** will ask for your **repo username and password**, however ArgoCD may encounter errors when retrieving your credentials in certain situations (ex: if using Github).\
    \
    You can verify if your credentials have been picked up or not by navigating to the **ArgoCD Settings** once you have deployed Argo in step 7 of this guide.\
    \
    To avoid errors, we recommend using a [Personal Access Token instead.](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

    [Find more information here.](https://argo-cd.readthedocs.io/en/release-1.8/user-guide/private-repositories/)
{% endhint %}

### 3.2 Execute cinchy.devops.automations

This utility updates the configurations in the cinchy.terraform, cinchy.argocd, and cinchy.kubernetes repositories.

1. From a shell/terminal, navigate to the **cinchy.devops.automations directory** location and execute the following command:

```
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

2\. If the file created in [**"Configuring the Deployment.json" step 2**](./#3.1-configure-deployment.json)  has a name other than "deployment.json", the reference in the command will will need to be replaced with the correct name of the file.

3\. The console output should terminate with a **"Completed successfully"**.

## 4. Terraform Deployment

The following steps detail how to deploy Terraform.

#### Cinchy.terraform Repo Structure - AWS

If deploying on AWS: Within the Terraform > AWS directory, a new folder named **eks\_cluster** is created. Nested within that is a subdirectory with the same name as the newly created cluster.&#x20;

To perform terraform operations, **the cluster directory must be the working directory during execution. This applies to everything within step 4 of this guide.**

#### Cinchy.terraform Repo Structure - Azure

If deploying on Azure: Within the Terraform > Azure directory, a new folder named **aks\_cluster** is created. Nested within that is a subdirectory with the same name as the newly created cluster.&#x20;

To perform terraform operations, **the cluster directory must be the working directory during execution.**

### 4.1 Cloud Provider Authentication

1. Launch a shell/terminal with the working directory set to the cluster directory within the cinchy.terraform repo.

2\. **If you are using AWS,** run the following commands to authenticate the session:

```
export AWS_DEFAULT_REGION=REGION
export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=YOUR_ACCESS_KEY
```

3\. **If using Azure,** run the following command and follow the on screen instructions to authenticate the session:

```
az login
```

### 4.2 Deploy the Infrastructure

1. Execute the following command to create the cluster:

```
bash create.sh
```

2\. Type **yes** when prompted to apply the terraform changes.

The resource creation process can take approx. 15-20 minutes. At the end of the execution there will be a section with the following header

**======= Output Variables =======**

If deploying on AWS, this section will contain 2 values: **Aurora RDS Server Host** and **Aurora RDS Password**

If deploying on Azure, this section will contain a single value: **Azure SQL Database Password**

These variable values are required to update the connection string within the deployment.json file (or equivalent) in the cinchy.devops.automations repo.

### 4.3 Retrieve the SSH Keys

The following section breaks down how to retrieve your SSH keys for both AWS and Azure deployments.

{% hint style="success" %}
SSH keys **should be saved** for future reference in the event that a connection needs to be established directly to a worker node in the Kubernetes cluster.
{% endhint %}

#### 4.3.1 AWS SSH Keys

1. The SSH key to connect to the Kubernetes nodes is maintained within the terraform state and can be retrieved by executing the following command:

```
terraform output -raw private_key
```

#### 4.3.2 Azure SSH Keys

1. The SSH key is output to the directory containing the cluster terraform configurations.

## 5. Update the Deployment.json

The following section pertains to updating the Deployment.json file.

### 5.1 Update the Database Connection String

1. Navigate to the the **deployment.json (**[**created in step 3.1**](./#3.1-configure-the-deployment.json)**) > cinchy\_instance\_configs section.**&#x20;
2. Each object within represents an instance that will be deployed on the cluster. Each instance configuration contains a **database\_connection\_string property**. This has placeholders for the **host name and password** that must be updated using output variables from the previous section.

{% hint style="warning" %}
**Note** that in the case of an Azure deployment, the host name is not available as part of the terraform output and instead must be sourced from the Azure Portal.
{% endhint %}

### 5.2 Create the IAM User for S3 (AWS Only)

The terraform script will create an S3 bucket for the cluster that must be accessible to the Cinchy application components.&#x20;

To access this programmatically, an IAM user that has read/write permissions to the new S3 bucket is required. **This can be an existing user.**

The Access Key and Secret Access Key for the IAM user must be specified under the **object\_storage** section of the **deployment.json**

### 5.3 Update Blob Storage Connection Details (Azure Only)

1. Within the **deployment.json**, the **azure\_blob\_storage\_conn\_str** must be set.&#x20;
2. The in-line comments outline the commands required to source this value from the Azure CLI.

### 5.3.1 Enabling Azure Key Vault Secrets

If you have the **key\_vault\_secrets\_provider\_enabled=true** value in the **azure.json** then the below secrets files would have been created during the execution of step 3.2:

<figure><img src="../../../.gitbook/assets/image (566).png" alt=""><figcaption></figcaption></figure>

You will need to add the following secrets to your azure key vault:

* **worker-secret-appsettings-\<cinchy\_instance\_name>**
* **web-secret-appsettings-\<cinchy\_instance\_name>**
* **maintenance-cli-secret-appsettings-\<cinchy\_instance\_name>**
* **idp-secret-appsettings-\<cinchy\_instance\_name>**
* **forms-secret-config-\<cinchy\_instance\_name>**
* **event-listener-secret-appsettings-\<cinchy\_instance\_name>**
* **connections-secret-config-\<cinchy\_instance\_name>**
* **connections-secret-appsettings-\<cinchy\_instance\_name>**

To create your new secrets:

1. Navigate to your key vault in the **Azure portal.**
2. Open your **Key Vault Settings** and select **Secrets.**
3. Select **Generate/Import.**
4. On the **Create a Secret** screen, choose the following values:
   1. Upload options: **Manual.**
   2. Name: Choose the secret name from the **above list.** They will all follow the format of: **\<app>-secret-appsettings-\<cinchy\_instance\_name>** or **\<app>-secret-config-\<cinchy\_instance\_name>**
   3. Value: The value for the secret will be the content of each app JSON located in the **cinchy.kubernetes\environment\_kustomizations\nonprod\<cinchy\_instance\_name>\secrets** folder, and as shown in above image.
   4. Content type: **JSON**
5. Leave the other values to their defaults.&#x20;
6. Select **Create.**

Once you receive the message that the first secret has been successfully created, you may proceed to create the other secrets. There are a total of **8 secrets to create** as shown in the above list of secret names.

### 5.4 Execute cinchy.devops.automations

This utility updates the configurations in the cinchy.terraform, cinchy.argocd, and cinchy.kubernetes repositories.

1. From a shell/terminal, navigate to the **cinchy.devops.automations** directory and execute the following command:

```
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

2\. If the file [created in section 3](./#3.-initial-configuration) has a name other than "deployment.json", the reference in the command will will need to be replaced with the correct name of the file.

3\. The console output should terminate with a "Completed successfully".&#x20;

4\. The updates must be committed to Git before proceeding to the next step.

## 6. Connect with kubectl

### 6.1 Update the Kubeconfig

#### 6.1.1 AWS

1. From a shell/terminal run the following command, replacing **\<region>** and **\<cluster\_name>** with the accurate values for those placeholders:

```
aws eks update-kubeconfig --region <region> --name <cluster_name>
```

#### 6.1.2 Azure

1. From a shell/terminal run the following commands, replacing **\<subscription\_id**_**>**, **<**_**deployment**_**\_**_**resource\_group>,** and **\<cluster\_name>** with the accurate values for those placeholders.&#x20;

{% hint style="info" %}
These commands with the values pre-populated can also be found from the Connect panel of the AKS Cluster in the Azure Portal.
{% endhint %}

```
az account set --subscription <subscription_id>
az aks get-credentials --admin --resource-group <deployment_resource_group> --name <cluster_name>
```

### 6.2 Verify the Connection

1. Verify that the connection has been established and the context is the correct cluster by running the following command:

```
kubectl config get-contexts
```

## 7. Deploy and Access ArgoCD

In this step, we will deploy and access ArgoCD.

### 7.1 Deploy ArgoCD

1. Launch a shell/terminal with the working directory set to **the root of the cinchy.argocd** repository.
2. Execute the following command to deploy ArgoCD:

```
bash deploy_argocd.sh
```

3\. Monitor the pods within the argocd namespace by running the following command every 30 seconds until they all move into a healthy state:

<pre><code><strong>kubectl get pods -n argocd
</strong></code></pre>

### 7.2 Access ArgoCD

1. Launch a new shell/terminal with the working directory set to the root of the cinchy.argocd repository.
2. Execute the following command to access ArgoCD:

<pre><code><strong>bash access_argocd.sh
</strong></code></pre>

This script creates a port forward using kubectl to enable ArgoCD to be accessed at **http://localhost:9090**

The credentials for ArgoCD's portal are output at the start of the access\_argocd's script execution in Base64. [The Base64 value must be decoded](https://www.base64decode.org/) to get the login credentials to use for the **http://localhost:9090** endpoint.

## 8. Deploy Cluster Components

In this step, you will deploy your cluster components.

### 8.1 Deploy ArgoCD Applications

1. Launch a shell/terminal with the working directory set to the root of the **cinchy.argocd repository.**
2. Execute the following command to deploy the cluster components using ArgoCD:

```
bash deploy_cluster_components.sh
```

3\. Navigate to ArgoCD at **http://localhost:9090** and login. Wait until all components are healthy (this may take a few minutes).

{% hint style="success" %}
**Tips for Success:**

* If your pods are degraded or failed to sync, refresh of resync your components. You can also delete pods and ArgoCD will automatically spin them back up for you.
* Check that ArgoCD is pulling from your git repo by navigating to your Settings
* If your components are failing upon attempting to pull an image, refer to your deployment.json to check that each component is set to the correct version number.
{% endhint %}

### 8.2 Update the DNS

1. Execute the following command to get the External IP used by the istio ingress gateway.&#x20;

```
kubectl get svc -n istio-system
```

2\. DNS entries must be created using the External IP for any subdomains / primary domains that will be used, including Opensearch, Grafana, and ArgoCD.

### 8.3 Accessing Opensearch

The default path to access Opensearch, unless you have configured it otherwise in your deployment.json, is **\<baseurl>/dashboard**

{% hint style="info" %}
The default credentials for accessing Opensearch are **admin/admin. We recommend that you change these credentials the first time you log in to Opensearch.**

**To change the default credentials for Cinchy v5.4+**, [follow the documentation here.](https://platform.docs.cinchy.com/guides-for-using-cinchy/additional-guides/monitoring-and-logging-on-kubernetes/opensearch-dashboards#3.-updating-your-opensearch-password)

To change the default credentials and/or add new users for all **other deployments**, follow [this documentation](https://opensearch.org/docs/1.2/security-plugin/access-control/users-roles/#create-users) or navigate to Settings > Internal Roles in Opensearch.”
{% endhint %}

### 8.4 Accessing Grafana

The default path to access Grafana, unless you have configured it otherwise in your deployment.json, is  **\<baseurl>/grafana**

{% hint style="info" %}
The default username is **admin**. The default password for accessing Grafana can be found by doing a search of "adminPassword" within the following path: **cinchy.kubernetes/cluster\_components/metrics/kube-prometheus-stack/values.yaml**

**We recommend that you change these credentials the first time you access Grafana. You can do so through the admin profile once logged in.**
{% endhint %}

## 9. Deploy Cinchy Components

In this step, you will deploy your Cinchy components.

### 9.1 Deploy ArgoCD Application

1. Launch a shell/terminal with the working directory set to the root of the **cinchy.argocd repository.**
2. Execute the following command to deploy the Cinchy application components using ArgoCD:

```
bash deploy_cinchy_components.sh
```

3\. Navigate to ArgoCD at **http://localhost:9090** and login. Wait until all components are healthy (may take a few minutes)

4\. You will be able to access ArgoCD through the **URL that you configured in your deployment.json**, as long as you created a DNS entry for it in step [8.2.](./#8.2-update-the-dns)

{% hint style="info" %}
You have now finished the deployment steps required for Cinchy. Navigate to your configured domain URL to verify that you can login using the default username (**admin**) and password (**cinchy**).
{% endhint %}

## 10. Troubleshooting

* If ArgoCD Application Sync is stuck waiting for PreSync jobs to complete, you can run the below command to restart the application controller.

```
kubectl rollout restart sts argocd-application-controller -n argocd
```
