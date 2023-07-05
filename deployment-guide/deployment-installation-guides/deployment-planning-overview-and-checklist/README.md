---
description: This page is your first stop when considering a deployment of Cinchy v5.
---

# Deployment planning overview and checklist

## Overview

This section will guide you through the considerations and the prerequisites before deploying version 5 of the Cinchy platform.

The pages in this section include:

- [Deployment Architecture Overview](deployment-architecture-overview/): This page explores your two high-level options for deploying Cinchy, on Kubernetes or on VM, and why Cinchy recommends a Kubernetes deployment. It also walks you through selecting a database to run your deployment on and some sizing considerations.
  - [Kubernetes Deployment Architecture](deployment-architecture-overview/): This page provides Infrastructure (for both Azure and AWS), Cluster, and Platform component overviews for Kubernetes deployments. It also guides you through considerations about your cluster configuration.
  - [IIS Deployment Architecture](deployment-architecture-overview/iis-deployment-architecture.md): This page provides Infrastructure and Platform component overviews for IIS (VM) deployments.
- [Deployment Prerequisites](deployment-prerequisites/): This page details important prerequisites for deploying Cinchy v5.

## Deployment planning checklist

Use the following checklist when planning for your Cinchy v5 deployment. Each item links to the appropriate documentation page.

- [ ] Will you run on [Kubernetes or a VM](deployment-architecture-overview/#1.-kubernetes-vs-vms)?

The main differences between a Kubernetes based deployment and an IIS deployment are:

- Kubernetes offers the ability to elastically scale.
- IIS limits certain components to running single instances.
- As all caching is in memory in an IIS deployment, if multiple instances are online for redundancy there is point to point communication between them (HTTP requests on the server IPs) required to maintain the cache.
- Performance is better on Kubernetes because of Kafka/Redis
- Prometheus/Grafana and OpenSearch aren't available in an IIS deployment
- The Maintenance CLI runs as a CronJob in Kubernetes while this needs to be orchestrated using a scheduler for an IIS deployment.
- Upgrades are simpler with the container images on Kubernetes.

### Kubernetes checklist

**If you will be running on Kubernetes**, please review the following checklist:

- [ ] [Which database](deployment-architecture-overview/#2.-choosing-a-database) will you use?
- [ ] Define your [sizing.](deployment-architecture-overview/#3.-sizing-considerations-and-requirements)
- [ ] Define your [cluster configuration](deployment-architecture-overview/kubernetes-deployment-architecture.md#3.1-cluster-configuration).
- [ ] How many [clusters will you need](deployment-architecture-overview/kubernetes-deployment-architecture.md#3.1-cluster-configuration)?
  - [ ] Will you be sharing from an existing cluster?
  - [ ] Will you be running multiple environments on a single cluster?
  - [ ] Will you be using Cinchy's recommended cluster components or your own?
- [ ] Define your [object storage requirements.](deployment-architecture-overview/#3.4-object-storage-requirements)
  - [ ] Create an [S3 compatible bucket](deployment-architecture-overview/#3.4-object-storage-requirements).
- [ ] Create your [SSL Certs](deployment-prerequisites/#2.-ssl-certs) (With the option to use [Self-Signed](../kubernetes-deployment-installation/using-self-signed-ssl-certs-kubernetes-deployments.md)).
- [ ] Define your [Secrets Management,](deployment-prerequisites/#3.-secrets-management) if desired.
- [ ] Define whether you will use Cinchy's [Docker Images ](deployment-prerequisites/#5.-docker-images)or your own.
  - [ ] If using Cinchy’s, [pull the images.](deployment-prerequisites/#5.1-accessing-cinchys-docker-images)

{% hint style="info" %}
Starting in Cinchy v5.4, you will have the option between Alpine or Debian based image tags for the listener, worker, and connections. **Using Debian tags will allow a Kubernetes deployment to be able to connect to a DB2 data source, and that option should be selected if you plan on leveraging a DB2 data sync.**

- When installing or upgrading your platform, you can use the following Docker image tags for the **listener, worker, and connections:**

  - **"5.x.x" - Alpine**
  - **"5.x.x-debian" - Debian**
    {% endhint %}

- [ ] Access the [deployment repositories](deployment-prerequisites/#6.-access-to-cinchy-artifacts) and copy them into your own repository (Github or similar).

### IIS checklist

**If you will be running on IIS,** please review the following checklist:

- [ ] Ensure that you have an instance of [SQL Server 201](deployment-prerequisites/#2.1-general-requirements)7+
- [ ] Ensure that you have [a Windows Server 2012+ machine with IIS 7.5+ installed](deployment-prerequisites/#2.1-general-requirements)
- [ ] [Install .net core Hosting bundle Version 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
  - Specifically, install: ASP.NET Core/.NET Core Runtime & Hosting Bundle
- [ ] Ensure that you review the minimum [web server hardware recommendations](deployment-prerequisites/#2.2-system-requirements)
- [ ] Ensure that you review the minimum [database server hardware recommendations](deployment-prerequisites/#2.2-system-requirements)
- [ ] Define your [application storage requirements.](deployment-architecture-overview/#3.3-application-storage-requirements)
- [ ] Ensure you have access to the[ release binary.](deployment-prerequisites/#2.1-access-the-binary)
