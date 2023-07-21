---
description: This page is your first stop when considering a deployment of Cinchy v5.
---

# Deployment Planning Overview and Checklist

## 1. Overview

There are various things to consider before deploying Cinchy v5.

The pages in this **Deployment Planning** section will guide you through the considerations you should think about and the prerequisites that you should implement before deploying version 5 of the Cinchy platform.

The pages in this section include:

* [Deployment Architecture Overview](deployment-architecture-overview/): This page explores your two high-level options for deploying Cinchy, on Kubernetes or on VM, and why we recommend a Kubernetes deployment. It also walks you through the important decision of selecting a database to run your deployment on, as well as some sizing considerations.
  * [Kubernetes Deployment Architecture](deployment-architecture-overview/): This page provides Infrastructure (for both Azure and AWS), Cluster, and Platform component overviews for Kubernetes deployments. It also guides you through considerations concerning your cluster configuration.
  * [IIS Deployment Architecture](deployment-architecture-overview/iis-deployment-architecture.md): This page provides Infrastructure and Platform component overviews for IIS (VM) deployments.
* [Deployment Prerequisites](deployment-prerequisites/): This page details important prerequisites for deploying Cinchy v5.

## 2. Deployment Planning Checklist

We have provided the following checklist for you to use when planning for your Cinchy v5 deployment. Each item is linked to the appropriate documentation page to provide more insight and clarity.

* [ ] Will you run on [Kubernetes or a VM](deployment-architecture-overview/readme.md#1-kubernetes-vs-iis)?

#### WIndows IIS vs Kubernetes

[**Kubernetes**](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) is an open-source system that manages and automates the full lifecycle of container-based applications. You now have the ability to deploy Cinchy v5 on Kubernetes, and with it comes a myriad of features that help to simplify your deployment and enhance your scaling. Kubernetes can maximize your container capacity and easily scale up/down with your current operations.&#x20;

You also have the option to run Cinchy on [**Microsoft IIS**](https://www.iis.net/overview)**,** which was the traditional deployment method prior to Cinchy v5. Internet Information Services (IIS) for Windows Server is a flexible, secure and manageable Web server for hosting anything on the Web.

**We recommend** using Kubernetes to deploy Cinchy v5, because of the robust features that you can leverage, such as improved logging and metrics. Using Kubernetes allows for a greater ability to scale your Cinchy instances as well as the ability to lower your costs by using PostgreSQL.

The main differences between a Kubernetes based deployment and an IIS deployment are:

| Kubernetes                                                                                                                              | Windows IIS                                                                                                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:green;">The ability to elastically scale with your business needs.</mark>                                            | <mark style="color:orange;">Limits certain components to running single instances.</mark>                                                                                             |
| <mark style="color:green;">Better performance due to Kafka/Redis components.</mark>                                                     | <mark style="color:orange;">Kafka/Redis not available as part of the deployment.</mark>                                                                                               |
| <mark style="color:green;">Ability to run the Maintenance CLI as a cron job.</mark>                                                     | <mark style="color:orange;">Maintenance CLI must be orchestrated using a scheduler.</mark>                                                                                            |
| <mark style="color:green;">Detailed logging and metrics capabilities available via Prometheus/Grafana and Opensearch components.</mark> | <mark style="color:orange;">Prometheus/Grafana and Opensearch not available as part of the deployment.</mark>                                                                         |
| <mark style="color:green;">Point to point communication not required to maintain multiple instance caching.</mark>                      | <mark style="color:orange;">Point to point communications (HTTP requests on the server IPs) is required to maintain caching in the event that multiple instances are stood up.</mark> |
| <mark style="color:green;">The use of container images allow for simpler version upgrades.</mark>                                       | <mark style="color:orange;">Does not use container images for upgrades.</mark>                                                                                                        |
| <mark style="color:green;">Ability to use PostgreSQL database to lower infrastructure costs.</mark>                                     | <mark style="color:orange;">Locked into a TSQL database.</mark>                                                                                                                       |
| <mark style="color:orange;">You need to manually manage your cluster(s).</mark>                                                         | <mark style="color:green;">You do not need to manually manage your cluster(s).</mark>                                                                                                 |

**If you will be running on Kubernetes**, please review the following checklist:

- [ ] [Which database](deployment-architecture-overview/README.md#2-choosing-a-database) will you use?
- [ ] Define your [sizing.](deployment-architecture-overview/README.md#3-sizing-considerations-and-requirements)
- [ ] Define your [cluster configuration](deployment-architecture-overview/kubernetes-deployment-architecture.md#41-cluster-configuration).
- [ ] How many [clusters will you need](deployment-architecture-overview/kubernetes-deployment-architecture.md#41-cluster-configuration)?
  - [ ] Will you be sharing from an existing cluster?
  - [ ] Will you be running multiple environments on a single cluster?
  - [ ] Will you be using Cinchy's recommended cluster components or your own?
- [ ] Define your [object storage requirements.](deployment-architecture-overview/README.md#33-object-storage-requirements)
  - [ ] Create an [S3 compatible bucket](deployment-architecture-overview/README.md#33-object-storage-requirements).
- [ ] Create your [SSL Certs](deployment-prerequisites/README.md#ssl-certs) (With the option to use [Self-Signed](../kubernetes-deployment-installation/dusing-self-signed-ssl-certs-kubernetes-deployments.md)).
- [ ] Define your [Secrets Management,](deployment-prerequisites/README.md#14-secrets-management) if desired.
- [ ] Define whether you will use Cinchy's [Docker Images ](deployment-prerequisites/README.md#16-docker-images)or your own.
  - [ ] If using Cinchy’s, [pull the images.](deployment-prerequisites/README.md#161-accessing-cinchys-docker-images)

{% hint style="info" %}
Starting in Cinchy v5.4, you will have the option between Alpine or Debian based image tags for the listener, worker, and connections. **Using Debian tags will allow a Kubernetes deployment to be able to connect to a DB2 data source, and that option should be selected if you plan on leveraging a DB2 data sync.**

* When either installing or upgrading your platform, you can use the following Docker image tags for the **listener, worker, and connections:**
  * **"5.x.x" - Alpine**
  * **"5.x.x-debian" - Debian**
{% endhint %}

* [ ] Access the [deployment repositories](deployment-prerequisites/readme.md#deployment-prerequisites) and copy them into your own repo (Github or similar).

**If you will be running on IIS,** please review the following checklist:

* [ ] Ensure that you have an instance of [SQL Server 201](deployment-prerequisites/readme.md#deployment-prerequisites)7+
* [ ] Ensure that you have [a Windows Server 2012+ machine with IIS 7.5+ installed](deployment-prerequisites/readme.md#general-requirements)
* [ ] [Install .net core Hosting bundle Version 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
  * Specifically, install: ASP.NET Core/.NET Core Runtime & Hosting Bundle
* [ ] Ensure that you review the minimum [web server hardware recommendations](deployment-prerequisites/readme.md#system-requirements)
* [ ] Ensure that you review the minimum [database server hardware recommendations](deployment-prerequisites/readme.md/#system-requirements)
* [ ] Define your [application storage requirements.](deployment-architecture-overview/readme.md#32-application-storage-requirements)
* [ ] Ensure you have access to the[ release binary.](deployment-prerequisites/readme.md#access-the-artifacts)
