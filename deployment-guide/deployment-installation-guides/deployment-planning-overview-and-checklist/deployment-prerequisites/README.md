---
description: This page details various prerequisites for deploying Cinchy v5.
---

# Deployment prerequisites

## General Kubernetes deployment prerequisites

Before deploying Cinchy v5 on Kubernetes, you must follow the steps listed below.

### Download your tools

Install the following tools on the machine where the deployment will run:

- [Terraform](https://www.terraform.io/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/) (v1.23.0+)
- [.NET Core 6.0.x](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
- [Bash](https://www.gnu.org/software/bash/) (You can also use [Git Bash](https://www.atlassian.com/git/tutorials/git-bash) on Windows)

### Create your domains

All your Cinchy environments will need a domain for each of the following:

- ArgoCD
- OpenSearch
- Grafana

Do this through your specific domain registrar. For example, GoDaddy or Google Domains.

### SSL certs

You must have valid SSL Certs ready when you deploy Cinchy v5. **Cinchy recommends** using a wildcard certificate if ArgoCD will be exposed via a subdomain. Without the wildcard certificate, you must create a port forward using `kubectl` on demand to access ArgoCD's portal.

You also have the option to use Self-Signed Certs in Kubernetes deployments. Find more information [here.](../../kubernetes-deployment-installation/using-self-signed-ssl-certs-kubernetes-deployments.md)

### Secrets management

Although optional, **Cinchy strongly recommends** secret management for storing and accessing secrets that you use in the deployment process. Cinchy currently supports:

- [Amazon Secrets Manager](https://aws.amazon.com/secrets-manager/)
- [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/basic-concepts)

### Single sign-on

If you would like to set up single sign-on for use in your Cinchy v5 environments, [please review the SSO integration page](single-sign-on-sso-integration/).

### Docker images

You can use Cinchy Docker images or your own. If you would like to use Cinchy images, please follow the section below to access them.

#### Access Cinchy Docker images

You will pull Docker images from Cinchy's AWS Elastic Container Registry (ECR).

To gain access to Cinchy's Docker images, you need login credentials to the ECR. Contact [Cinchy Support](../../../../getting-help.md) for access.

{% hint style="info" %}
Starting in Cinchy v5.4, you will have the option between Alpine or Debian based image tags for the listener, worker, and connections. **Using Debian tags will allow a Kubernetes deployment to be able to connect to a DB2 data source. Use this option if you plan on leveraging a DB2 data sync.**

- When installing or upgrading your platform, you can use the following Docker image tags for the **listener, worker, and connections:**
  - **"5.x.x" - Alpine**
  - **"5.x.x-debian" - Debian**
{% endhint %}

### Create your repositories

You must create the following four Git repositories. You can use any source control platform that supports Git, such as [**Gitlab**](https://about.gitlab.com/)**,** [**Azure DevOps**](https://docs.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)**,** or [**GitHub**](https://github.com/).

- **cinchy.terraform:** Contains all Terraform configurations.
- **cinchy.argocd:** Contains all ArgoCD configurations.
- **cinchy.kubernetes:** Contains cluster and application component deployment manifests.
- **cinchy.devops.automations:** Contains the single configuration file and binary utility that maintains the contents of the above three repositories.

{% hint style="warning" %}
You must have a service account with read/write permissions to the git repositories created above.
{% endhint %}

### Access to Cinchy artifacts

You will need to access and download the Cinchy artifacts before deployment.

To access the Kubernetes artifacts:

1. Access the [**Cinchy Releases**](https://cinchy.net/Cinchy/Tables/1477) table. Please contact [Cinchy Support](../../../../getting-help.md) if you don't have the access credentials necessary.
2. Navigate to the release you wish to deploy.
3. Download the .zip file(s) listed under the **Kubernetes Artifacts** column.
4. Check the contents of each of the directories into their [respective repository.](./#undefined)

{% hint style="warning" %}
Please contact Cinchy Support if you are encountering issues accessing the table or the artifacts.
{% endhint %}

## Kubernetes Azure requirements

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
- A VNet within the resource group.
- A single subnet. It's important that the address range be enough for all executing processes within the cluster, such as a CIDR ending with /22 to provide a range of 1024 IPs.

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

- Use an existing VPC.
- Create a new one.

#### Existing VPC

- If you prefer an **existing VPC**, you must provision the following before the deployment:
  - The VPC. It's important that the address range be enough for all executing processes within the cluster, such as a CIDR ending with /21 to provide a range of 2048 IPs.
  - 3 Subnets (one per AZ). It's important that the address range be enough for all executing processes within the cluster, such as a CIDR ending with /23 to provide a range of 512 IPs.
  - If the subnets are **private**, a NAT Gateway is required to enable node group registration with the EKS cluster.

#### New VPC

- If you prefer a **new VPC**, all resources will be automatically provisioned.
- The limit of the **Running On-Demand All Standard** vCPUs must offer enough availability for the required number of vCPUs (minimum of 24).
- An **IAM user account** to connect to AWS which has the necessary privileges to create resources in any existing VPC and the ability to create a VPC (if required).
- You must import the SSL certificate into AWS Certificate Manager (or a new certificate can be requested via AWS Certificate Manager).
- You must import the SSL certificate [into AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html), or a new certificate can be requested via [AWS Certificate Manager.](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html)
- If you are importing it, you will need the PEM-encoded certificate body and private key. You can find this, you can get the PEM file from your chosen domain provider (GoDaddy, Google, etc.) [Read more on this here.](https://aws.amazon.com/blogs/security/how-to-import-pfx-formatted-certificates-into-aws-certificate-manager-using-openssl/)

## IIS deployment prerequisites

Before deploying Cinchy v5 on IIS, you require the following:

### Access the artifacts

You need to access and download the Cinchy binary before deployment:

- Access the [**Cinchy Releases**](https://cinchy.net/Cinchy/Tables/1477) table. Please contact [Cinchy Support](../../../../getting-help.md) if you don't have the access credentials necessary.
- Navigate to the release you wish to deploy
- Download the files listed under the **Component Artifacts** column. This should include zip files for:
  - Cinchy Platform
  - [Cinchy Connections](https://cli.docs.cinchy.com/connections-installation-guide)
  - [Cinchy Event Listener](https://cli.docs.cinchy.com/installation-guide)
  - [Cinchy Maintenance CLI and CLI ](../../../../data-syncs/installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)(optional)
  - [Cinchy Meta-Forms](https://cinchy.gitbook.io/cinchy-meta-forms/) (optional)

{% hint style="warning" %}
Please contact [Cinchy Support](../../../../getting-help.md) if you are encountering issues accessing the table or the artifacts.
{% endhint %}

### General requirements

1. An instance of SQL Server 2017+
2. A Windows Server 2012+ machine with IIS 7.5+ installed
3. [Install .net core Hosting bundle Version 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
   - Specifically, install: ASP.NET Core/.NET Core Runtime & Hosting Bundle

{% hint style="info" %}
Cinchy Platform 5.4+ uses .NET Core 6.0.

4.18.0+ used .NET Core 3.1 and earlier versions used .NET Core 2.1
{% endhint %}

### System requirements

### Minimum web server hardware recommendations

- 2 × 2 GHz Processor
- 8 GB RAM
- 4 GB Hard Disk storage available

### Minimum database server hardware recommendations

- 4 × 2 GHz Processor
- 12 GB RAM
- Hard disk storage dependent upon use case. Note that Cinchy maintains historical versions of data and performs soft deletes which will add to the storage requirements.

### Clustering

Clustering considerations are applicable to both the Web and Database tiers in the Cinchy deployment architecture.

The web tier can be clustered by introducing a load balancer and scaling web server instances horizontally. Each node within Cinchy uses an in-memory cache of metadata information, and expiration of cached elements is triggered upon data changes that would impact that metadata. Data changes processed by one node wouldn't be known to other nodes without establishing connectivity between them. The nodes must be able to communicate over either HTTP or HTTPS through an IP based binding on the IIS server that allows the broadcast of cache expiration messages. The port used for this communication is different from the standard port that's used by the application when a domain name is involved. Often for customers this means that a firewall port must be opened on these servers.

The database tier relies on standard MS SQL Server failover clustering capabilities.

### Scaling Cconsiderations

The web application oversees all interactions with Cinchy be it through the UI or connectivity from an application. It interprets/routes incoming requests, handles serialization/deserialization of data, data validation, enforcement of access controls, and the query engine to transform Cinchy queries into the physical representation for the database. The memory footprint for the application is low, as caching is limited to metadata, but CPU use grows with request volume and complexity(For example, insert/update operations are more complex than select operations). As the user population grows or request volume increases, there may be a need to add nodes.

The database tier relies on a persistence platform that scales vertically. As the user population grows and request volume increases, the system may require additional CPU / Memory. Cinchy recommends you start off in an environment that allows flexibility (such as a VM) until you can profile the real-world load and establish a configuration. On the storage side, Cinchy maintains historical versions of records when changes are made and performs soft deletes of data which will add to the storage requirements. The volume of updates occurring to records should be considered when estimating the storage size.

### Backups

Outside of log files there is no other data generated & stored on the web servers by the application, which means backups are centered around the database. Since the underlying persistence platform is a MS SQL Server, this relies on standard procedures for this platform.
