---
description: This page provides an overview for the deployment architecture of Cinchy v5.
---

# Deployment Architecture Overview

## Table of Contents

| Table of Contents                                                                                    |
| ---------------------------------------------------------------------------------------------------- |
| [#1.-kubernetes-vs-iis](./#1.-kubernetes-vs-iis "mention")                                           |
| [#2.-choosing-a-database](./#2.-choosing-a-database "mention")                                       |
| [#3.-sizing-considerations-and-requirements](./#3.-sizing-considerations-and-requirements "mention") |

## 1. Kubernetes vs IIS

When choosing to deploy Cinchy version 5, you must decide whether to deploy via Kubernetes or on a VM (IIS).

[**Kubernetes**](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is an open-source system that manages and automates the full lifecycle of container-based applications. You now have the ability to deploy Cinchy v5 on Kubernetes, and with it comes a myriad of features that help to simplify your deployment and enhance your scaling. Kubernetes can maximize your container capacity and easily scale up/down with your current operations.&#x20;

* **Grafana, Opensearch, Opensearch Dashboard:** Working together, these three applications provide a visual logging dashboard for all of the information coming in from your database pods. This streamlines your search for information by putting the control into your hands and compiling your logs in one easy to access place — you can now easily write a query against all of your logs, in all of your environments. You will have access to a default configuration out of the box, but you can also customize your dashboards as well.&#x20;
* P**rometheus:** With the Kubernetes addition, you now have access to Prometheus for your metrics dashboard. Prometheus records real-time metrics in a time series database used for event monitoring and alerting. You can then create custom dashboards to display your data in an easy to use visual that makes reporting on your metrics easy, and even set up push alerts based on your custom needs.

You also have the option to run Cinchy on [**Microsoft IIS**](https://www.iis.net/overview)**,** which was the traditional deployment method prior to Cinchy v5. Internet Information Services (IIS) for Windows Server is a flexible, secure and manageable Web server for hosting anything on the Web.

**We recommend** using Kubernetes to deploy Cinchy v5, because of the robust features that you can leverage, such as improved logging and metrics. Using Kubernetes allows for a greater ability to scale your Cinchy instances as well as the ability to lower your costs by using PostgreSQL.

## 2. Choosing a Database

Before deploying Cinchy v5, you must select which database you want to use.&#x20;

The following list outlines which databases we support for Kubernetes Deployments.

For IIS Deployments [please review the architecture requirements here.](iis-deployment-architecture.md)

### 2.1 Microsoft SQL Server

#### 2.1.1 [MS SQL Server](https://www.microsoft.com/en-ca/sql-server/sql-server-2019?SilentAuth=1\&f=255\&MSPPError=-2147208139)

* Microsoft SQL Server is a relational database management system. As a database server, it is a software product with the primary function of storing and retrieving data as requested by other software applications—which may run either on the same computer or on another computer across a network.
* [Resource limits](https://docs.microsoft.com/en-us/sql/sql-server/maximum-capacity-specifications-for-sql-server?view=sql-server-ver16)

#### 2.1.2  [Azure SQL Database](https://azure.microsoft.com/en-ca/products/azure-sql/database/)

* Microsoft Azure SQL Database is a managed cloud database provided as part of Microsoft Azure. It runs on a cloud computing platform, and access to it is provided as a service. Managed database services take care of scalability, backup, and high availability of the database.
* [Differences between Azure SQL and Azure Managed SQL](https://docs.microsoft.com/en-us/azure/azure-sql/database/features-comparison?view=azuresql#features-of-sql-database-and-sql-managed-instance)
* [Resource Limits](https://docs.microsoft.com/en-us/azure/azure-sql/database/resource-limits-logical-server?view=azuresql)

#### 2.1.3 [Azure Managed SQL Instance](https://azure.microsoft.com/en-ca/products/azure-sql/managed-instance/)

* SQL Managed Instance is a managed, always up-to-date SQL instance in the cloud that combines broad SQL Server engine compatibility with the benefits of a fully managed PaaS.
* [Differences between Azure SQL and Azure Managed SQL](https://docs.microsoft.com/en-us/azure/azure-sql/database/features-comparison?view=azuresql#features-of-sql-database-and-sql-managed-instance)
* [Resource Limits](https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/resource-limits?view=azuresql)

### 2.2 Amazon Aurora

[Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP\_AuroraOverview.html)

* Amazon Aurora (Aurora) is a fully managed relational database engine that's compatible with MySQL and PostgreSQL. It combines the speed and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. Aurora is part of the managed database service Amazon Relational Database Service (Amazon RDS). Amazon RDS is a web service that makes it easier to set up, operate, and scale a relational database in the cloud.
* [Resource Limits](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP\_Limits.html)

### 2.3 PostgreSQL

#### 2.3.1 [PostgreSQL](https://www.postgresql.org/)

* PostgreSQL is a free and open-source relational database management system emphasizing extensibility and SQL compliance. PostgreSQL comes with features aimed to help developers build applications, administrators to protect data integrity and build fault-tolerant environments, and help you manage your data no matter how big or small the dataset.

#### 2.3.2 [AWS RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/)

* Amazon RDS makes it easy to set up, operate, and scale PostgreSQL deployments in the cloud. With Amazon RDS, you can deploy scalable PostgreSQL deployments with cost-efficient and resizable hardware capacity. Amazon RDS manages complex and time-consuming administrative tasks such as PostgreSQL software installation and upgrades; storage management; replication for high availability and read throughput; and backups for disaster recovery. Amazon RDS for PostgreSQL gives you access to the capabilities of the familiar PostgreSQL database engine.
* [Resource Limits](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP\_PostgreSQL.html#PostgreSQL.Concepts.General.Limits)

#### 2.3.3 [Azure PostgreSQL Database](https://azure.microsoft.com/en-us/services/postgresql/#overview)

* This is a fully managed and intelligent Azure Database for PostgreSQL. Enjoy high availability with a service-level agreement (SLA) up to 99.99 percent, AI-powered performance optimization, and advanced security. A fully managed database that automates maintenance, patching, and updates. Provision in minutes and independently scale compute or storage in seconds.
* [Resource Limits](https://docs.microsoft.com/en-us/azure/postgresql/single-server/concepts-limits)

## 3. Sizing Considerations and Requirements

Prior to deploying Cinchy version 5, you need to define your sizing requirements.&#x20;

### 3.1 Sizing

#### 3.1.1 Kubernetes Sizing

Cluster sizing recommendations vary and are dependant on a myriad of deployment factors. We have provided the following very general sizing recommendations, but encourage you to explore more personalized options.

**CPU:** 8 Cores

**Memory:** 32GB Ram

**Number of Servers:** 3

**AWS:** m5.2xlarge

**Azure:** D8 v3

#### 3.1.2 IIS Sizing

For sizing recommendations and prerequisites concerning an IIS deployment, [please review the documentation found here.](../deployment-prerequisites/#2.-iis-deployment-prerequisites)

### 3.2 Application Storage Requirements

If you are choosing to deploy Cinchy v5 on IIS, then you need to ensure that your VM disks have enough application storage to run your clusters.

### 3.3 Object Storage Requirements

Cinchy supports both [**Amazon S3**](https://aws.amazon.com/s3/) and [**Azure Blob Storage.**](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)

If you are using Terraform for your Kubernetes deployment, you will need to set up a new S3 compatible bucket to manually to store your state file. You will also need a bucket for Connections, to store error files created during data syncs.

You will create your **two S3 compatible buckets** using either Amazon or Azure. Ensure that you use the following convention when naming your buckets so that the automation script runs correctly: **\<org>-\<component>-\<cluster>.** These bucket names will be referenced in your configuration files when you [deploy Cinchy on Kubernetes.](kubernetes-deployment-architecture.md)

**Example Terraform Bucket:** cinchy-terraform-state

**Example Connection Bucket:** cinchy-connections-cinchy-nonprod

There is no sizing definitions, as S3 provides unlimited scalability and it charges only for what you use/how much you store on it.

* [How to set up an Amazon S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
* [How to set up Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)
