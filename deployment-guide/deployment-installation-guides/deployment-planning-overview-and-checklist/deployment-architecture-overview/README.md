---
description: This page provides an overview for the deployment architecture of Cinchy v5.
---

# Deployment architecture overview

## Kubernetes vs IIS

When choosing to deploy Cinchy version 5, you must decide whether to deploy via Kubernetes or on a VM (IIS).

[**Kubernetes**](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is an open-source system that manages and automates the full lifecycle of container-based applications. You now have the ability to deploy Cinchy v5 on Kubernetes, which helps to simplify your deployment and enhance your scaling. Kubernetes can maximize your container capacity and scale up/down with your current operations.

- **Grafana, OpenSearch, OpenSearch Dashboard:** Working together, these three applications provide a visual logging dashboard for all the information coming in from your database pods. This streamlines your search for information by putting the control into your hands and compiling your logs in one easy to access place — you can now write a query against all your logs, in all your environments. You will have access to a default configuration out of the box, but you can also customize your dashboards as well.
- **Prometheus:** With the Kubernetes addition, you now have access to Prometheus for your metrics dashboard. Prometheus records real-time metrics in a time series database used for event monitoring and alerting. You can then create custom dashboards to display your data in an easy to use visual that makes reporting on your metrics easy, and even set up push alerts based on your custom needs.

You also have the option to run Cinchy on [**Microsoft IIS**](https://www.iis.net/overview)**,** which was the traditional deployment method before Cinchy v5. Internet Information Services (IIS) for Windows Server is a flexible, secure and manageable Web server for hosting anything on the Web.

{% hint style="success" %}
**We recommend** using Kubernetes to deploy Cinchy v5, because of the robust features that you can leverage, such as improved logging and metrics. Kubernetes helps scale your Cinchy instances and lower your costs by using PostgreSQL.
{% endhint %}

## Choose a database

Before deploying Cinchy v5, you must select which database you want to use.

The following list outlines the supported databases for Kubernetes Deployments.

For IIS Deployments [please review the architecture requirements here.](iis-deployment-architecture.md)

### Microsoft SQL Server

#### [MS SQL Server](https://www.microsoft.com/en-ca/sql-server/sql-server-2019?SilentAuth=1&f=255&MSPPError=-2147208139)

- Microsoft SQL Server is a relational database management system. As a database server, it's a software product with the primary function of storing and retrieving data as requested by other software applications—which may run either on the same computer or on another computer across a network.
- [Resource limits](https://docs.microsoft.com/en-us/sql/sql-server/maximum-capacity-specifications-for-sql-server?view=sql-server-ver16)

#### [Azure SQL Database](https://azure.microsoft.com/en-ca/products/azure-sql/database/)

- Microsoft Azure SQL Database is a managed cloud database provided as part of Microsoft Azure. It runs on a cloud computing platform, and access to it's provided as a service. Managed database services take care of scalability, backup, and high availability of the database.
- [Differences between Azure SQL and Azure Managed SQL](https://docs.microsoft.com/en-us/azure/azure-sql/database/features-comparison?view=azuresql#features-of-sql-database-and-sql-managed-instance)
- [Resource Limits](https://docs.microsoft.com/en-us/azure/azure-sql/database/resource-limits-logical-server?view=azuresql)

#### [Azure Managed SQL Instance](https://azure.microsoft.com/en-ca/products/azure-sql/managed-instance/)

- SQL Managed Instance is a managed, cloud-based, always up-to-date SQL instance that combines broad SQL Server engine compatibility with the benefits of a fully managed PaaS.
- [Differences between Azure SQL and Azure Managed SQL](https://docs.microsoft.com/en-us/azure/azure-sql/database/features-comparison?view=azuresql#features-of-sql-database-and-sql-managed-instance)
- [Resource Limits](https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/resource-limits?view=azuresql)

### Amazon Aurora

[Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

- Amazon Aurora (Aurora) is a fully managed relational database engine that's compatible with MySQL and PostgreSQL. It combines the speed and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. Aurora is part of the managed database service Amazon Relational Database Service (Amazon RDS). Amazon RDS is a cloud web service that makes it easier to set up, operate, and scale a relational database.
- [Resource Limits](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_Limits.html)

### PostgreSQL

#### [PostgreSQL](https://www.postgresql.org/)

- PostgreSQL is a free and open-source relational database management system emphasizing extensibility and SQL compliance. PostgreSQL comes with features aimed to help developers build applications, administrators to protect data integrity and build fault-tolerant environments, and help you manage your data no matter how big or small the dataset.

#### [AWS RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/)

- Amazon RDS makes it easy to set up, operate, and scale cloud-based PostgreSQL deployments. With Amazon RDS, you can deploy scalable PostgreSQL deployments with cost-efficient and resizable hardware capacity. Amazon RDS manages complex and time-consuming administrative tasks such as PostgreSQL software installation and upgrades; storage management; replication for high availability and read throughput; and backups for disaster recovery. Amazon RDS for PostgreSQL gives you access to the capabilities of the familiar PostgreSQL database engine.
- [Resource Limits](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html#PostgreSQL.Concepts.General.Limits)

#### [Azure PostgreSQL Database](https://azure.microsoft.com/en-us/services/postgresql/#overview)

- This is a fully managed and intelligent Azure Database for PostgreSQL. Enjoy high availability with a service-level agreement (SLA) up to 99.99 percent, AI-powered performance optimization, and advanced security. A fully managed database that automates maintenance, patching, and updates. Provision in minutes and independently scale compute or storage in seconds.
- [Resource Limits](https://docs.microsoft.com/en-us/azure/postgresql/single-server/concepts-limits)

## Sizing considerations and requirements

Before deploying Cinchy v5, you need to define your sizing requirements.

### Sizing

#### Kubernetes sizing

Cluster sizing recommendations vary and are dependant on a number of deployment factors. We've provided the following general sizing recommendations, but encourage you to explore more personalized options.

**CPU:** 8 Cores

**Memory:** 32GB Ram

**Number of Servers:** 3

**AWS:** m5.2xlarge

**Azure:** D8 v3

#### IIS sizing

For sizing recommendations and prerequisites about an IIS deployment, [please review the IIS deployment prerequisites.](../deployment-prerequisites/#2.-iis-deployment-prerequisites)

### Application storage requirements

If you are choosing to deploy Cinchy v5 on IIS, then you need to ensure that your VM disks have enough application storage to run your clusters.

### Object storage requirements

Cinchy supports both [**Amazon S3**](https://aws.amazon.com/s3/) and [**Azure Blob Storage.**](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)

If you are using Terraform for your Kubernetes deployment, you will need to set up a new S3 compatible bucket to manually to store your state file. You will also need a bucket for Connections, to store error files created during data syncs.

You will create your **two S3 compatible buckets** using either Amazon or Azure. Ensure that you use the following convention when naming your buckets so that the automation script runs correctly: **\<org>-\<component>-\<cluster>.** These bucket names will be referenced in your configuration files when you [deploy Cinchy on Kubernetes.](kubernetes-deployment-architecture.md)

**Example Terraform Bucket:** `cinchy-terraform-state`

**Example Connection Bucket:** `cinchy-connections-cinchy-nonprod`

S3 provides unlimited scalability and it charges only for what you use/how much you store on it, so there are no sizing definitions.

- [How to set up an Amazon S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
- [How to set up Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)
