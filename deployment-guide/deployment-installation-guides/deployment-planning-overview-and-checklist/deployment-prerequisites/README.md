---
description: This page details various prerequisites for deploying Cinchy v5.
---

# Deployment Prerequisites

## Table of Contents

| Table of Contents                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------- |
| [#1.-general-kubernetes-deployment-prerequisites](./#1.-general-kubernetes-deployment-prerequisites "mention")               |
| [#2.-kubernetes-deployments-azure-specific-requirements](./#2.-kubernetes-deployments-azure-specific-requirements "mention") |
| [#3.-kubernetes-deployments-aws-specific-requirements](./#3.-kubernetes-deployments-aws-specific-requirements "mention")     |
| [#4.-iis-deployment-prerequisites](./#4.-iis-deployment-prerequisites "mention")                                             |

## 1. General Kubernetes Deployment Prerequisites

The following is a list of steps that are required prior to deploying Cinchy v5 on Kubernetes.

### 1.1 Download your Tools

The following tools should be installed on the machine where the deployment will run:

* [Terraform](https://www.terraform.io/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/) (v1.23.0+)
* [.NET Core 6.0.x](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
* [Bash](https://www.gnu.org/software/bash/) ([Git Bash](https://www.atlassian.com/git/tutorials/git-bash) may be used on Windows)

### 1.2 Creating your Domains&#x20;

All of your Cinchy environments will need a domain for each of the following:

* ArgoCD&#x20;
* Opensearch
* Grafana

This is done through your specific domain registrar (for example: GoDaddy, Google Domains, etc.)

### 1.3 SSL Certs

You will need to have valid SSL Certs ready when you deploy Cinchy v5. This should be a wildcard certificate if ArgoCD will be exposed via a subdomain **(preferred)**. Without the wildcard certificate, accessing ArgoCD's portal requires a port forward to be created using kubectl on demand.

You also have the option to use Self-Signed Certs in Kubernetes deployments. Find more information [here.](../../kubernetes-deployment-installation/using-self-signed-ssl-certs-kubernetes-deployments.md)

### 1.4 Secrets Management

Secrets management is optional, however Cinchy recommends it for securely storing and accessing secrets that will be used in the deployment process. Cinchy currently supports:

* [Amazon Secrets Manager](https://aws.amazon.com/secrets-manager/)
* [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/basic-concepts)

### 1.5 Single Sign-On

This is optional, but if you would like to set up single sign-on for use in your Cinchy v5 environments, [please review the documentation here.](single-sign-on-sso-integration/)

### 1.6 Docker Images

You have the option to either use Cinchy's Docker images or your own. If you would like to use Cinchy's, please follow the section below to access them.

#### 1.6.1 Accessing Cinchy's Docker images

You will pull Docker images from Cinchy's AWS Elastic Container Registry (ECR).

To gain access to Cinchy's Docker images, you will need login credentials to the ECR. You can gain these by contacting Cinchy Support.

{% hint style="info" %}
Starting in Cinchy v5.4, you will have the option between Alpine or Debian based image tags for the listener, worker, and connections. **Using Debian tags will allow a Kubernetes deployment to be able to connect to a DB2 data source, and that option should be selected if you plan on leveraging a DB2 data sync.**

* When either installing or upgrading your platform, you can use the following Docker image tags for the **listener, worker, and connections:**
  * **"5.x.x" - Alpine**
  * **"5.x.x-debian" - Debian**
{% endhint %}

### 1.7 Create your Repositories

The following four Git repositories must be created. Any source control platform supporting Git may be used, e.g. [**Gitlab**](https://about.gitlab.com/)**,** [**Azure DevOps**](https://docs.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)**,** [**Github**](https://github.com/)

* **cinchy.terraform:** This repo contains all Terraform configurations.
* **cinchy.argocd:** This repo contains all ArgoCD configurations.
* **cinchy.kubernetes:** This repo contains cluster and application component deployment manifests.
* **cinchy.devops.automations:** This repo contains the single configuration file and binary utility that maintains the contents of the above three repositories.

{% hint style="warning" %}
You must have a service account with read/write permissions to the git repos created above.
{% endhint %}

### 1.8 Access to Cinchy Artifacts

You will need to access and download the Cinchy artifacts prior to deployment.

&#x20;To access the Kubernetes artifacts:

1. Access the [**Cinchy Releases**](https://cinchy.net/Cinchy/Tables/1477) table. Please contact [Cinchy Support](../../../../getting-help.md) if you do not have the access credentials necessary.
2. Navigate to the release you wish to deploy
3. Download the .zip file(s) listed under the **Kubernetes Artifacts** column
4. Check the contents of each of the directories into their [respective repository.](./#undefined)

{% hint style="warning" %}
Please contact Cinchy Support if you are encountering issues accessing the table or the artifacts.
{% endhint %}

## 2. Kubernetes Deployments (Azure Specific Requirements)

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

## 3. Kubernetes Deployments (AWS Specific Requirements)

The following prerequisites are required if you are deployment Cinchy v5 on AWS.

* Terraform Backend Requirements:
  * An S3 bucket that will contain the terraform state.
* The [AWS CLI](https://aws.amazon.com/cli/) should be installed on the machine where the deployment will be run. It must be set to the correct profile/login
* The template has the option of either leveraging an existing VPC or creating a new one:
  * If an **existing VPC** is preferred, the prerequisite requires the following be provisioned in advance of the deployment:
    * The VPC. It's important that the address range be sufficient for all executing processes within the cluster, e.g. a CIDR ending with /21 to provide a range of 2048 IPs.
    * 3 Subnets (one per AZ). It's important that the address range be sufficient for all executing processes within the cluster, e.g. a CIDR ending with /23 to provide a range of 512 IPs.
    * If the subnets are **private**, a NAT Gateway is required to enable node group registration with the EKS cluster.
  * If a **new VPC** is preferred, all resources will be automatically provisioned.
* The limit of the **Running On-Demand All Standard** vCPUs must provide sufficient availability for the required number of vCPUs (minimum of 24).
* An **IAM user account** to connect to AWS which has the necessary privileges to create resources in any existing VPC and the ability to create a VPC (if required).
* The SSL certificate must be imported into AWS Certificate Manager (or a new certificate can be requested via AWS Certificate Manager).

## 4. IIS Deployment Prerequisites

The following is a list of steps that are required prior to deploying Cinchy v5 on IIS

### 4.1 Access the Artifacts

You will need to access and download the Cinchy binary prior to deployment:

* Access the [**Cinchy Releases**](https://cinchy.net/Cinchy/Tables/1477) table.  Please contact [Cinchy Support](../../../../getting-help.md) if you do not have the access credentials necessary.
* Navigate to the release you wish to deploy
* Download the files listed under the **Component Artifacts** column. This should include zip files for:
  * Cinchy Platform
  * [Cinchy Connections](https://cli.docs.cinchy.com/connections-installation-guide)
  * [Cinchy Event Listener](https://cli.docs.cinchy.com/installation-guide)
  * [Cinchy Maintenance CLI and CLI ](../../../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)(optional)
  * [Cinchy Meta-Forms](https://cinchy.gitbook.io/cinchy-meta-forms/) (optional)

{% hint style="warning" %}
Please contact Cinchy Support if you are encountering issues accessing the table or the artifacts.
{% endhint %}

### 4.2 General Requirements

1. An instance of SQL Server 2017+
2. A Windows Server 2012+ machine with IIS 7.5+ installed
3. [Install .net core Hosting bundle Version 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
   * Specifically, install: ASP.NET Core/.NET Core Runtime & Hosting Bundle

{% hint style="info" %}
Cinchy Platform 5.4+ uses .NET Core 6.0.&#x20;

4.18.0+ used .NET Core 3.1 and previous versions used .NET Core 2.1
{% endhint %}

### 4.3 System Requirements

**Minimum Web Server Hardware Recommendations**

* 2 x 2 GHz Processor
* 8 GB RAM
* 4 GB Hard Disk storage available

**Minimum Database Server Hardware Recommendations**

* 4 x 2 GHz Processor
* 12 GB RAM
* Hard disk storage dependent upon use case. Note that Cinchy maintains historical versions of data and performs soft deletes which will add to the overall storage requirements.

### 4.4 Clustering

Clustering considerations are applicable to both the Web and Database tiers in the Cinchy deployment architecture.

The web tier can be clustered by introducing a load balancer and scaling web server instances horizontally. Each node within Cinchy uses an in-memory cache of metadata information, and expiration of cached elements is triggered upon data changes that would impact that metadata. Data changes processed by one node wouldn't immediately be known to other nodes without establishing connectivity between them. For this reason the nodes must be able to communicate over either http or https through an IP based binding on the IIS server that allows cache expiration messages to be broadcast. The port used for this communication is different from the standard port that is used by the application when a domain name is involved. Often for customers this means that a firewall port must be opened on these servers.

The database tier relies on standard MS SQL Server failover clustering capabilities.

### 4.5 Scaling Considerations

The web application is responsible for all interactions with Cinchy be it through the UI or connectivity from an application. It interprets/routes incoming requests, handles serialization/deserialization of data, data validation, enforcement of access controls, and the query engine to transform Cinchy queries into the physical representation for the database. The memory footprint for the application is fairly low as caching is limited to metadata, but the CPU utilization grows with request volume and complexity (e.g. insert / update operations are more complex than select operations). As the user population grows or request volume increases from batch processes / upstream system API calls there may be a need to add nodes.

The database tier relies on a persistence platform that scales vertically. As the user population grows and request volume increases from batch processes / upstream system API calls the system may require additional CPU / Memory. Starting off in an environment that allows flexibility (e.g. a VM) would be advised until the real world load can be profiled and a configuration established. On the storage side, Cinchy maintains historical versions of records when changes are made and performs soft deletes of data which will add to the storage requirements. The volume of updates occurring to records should be considered when estimating the storage size.

### 4.6 Backups

Outside of log files there is no other data generated & stored on the web servers by the application, which means backups are generally centered around the database. Since the underlying persistence platform is a MS SQL Server, this relies on standard procedures for this platform.
