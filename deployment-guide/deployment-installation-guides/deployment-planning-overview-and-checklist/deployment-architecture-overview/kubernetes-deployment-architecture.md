---
description: >-
  This page details the deployment architecture of Cinchy v5 when running on
  Kubernetes.
---

# Kubernetes Deployment Architecture

## Table of Contents

| Table of Contents                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [#1.-infrastructure-configuration-on-cluster](kubernetes-deployment-architecture.md#1.-infrastructure-configuration-on-cluster "mention")                       |
| [#2.-aws-infrastructure-configuration-outside-cluster](kubernetes-deployment-architecture.md#2.-aws-infrastructure-configuration-outside-cluster "mention")     |
| [#3.-azure-infrastructure-configuration-outside-cluster](kubernetes-deployment-architecture.md#3.-azure-infrastructure-configuration-outside-cluster "mention") |
| [#4.-cluster-level-component-overview](kubernetes-deployment-architecture.md#4.-cluster-level-component-overview "mention")                                     |
| [#5.-instance-component-overview](kubernetes-deployment-architecture.md#5.-instance-component-overview "mention")                                               |
| [#6.-gitops](kubernetes-deployment-architecture.md#6.-gitops "mention")                                                                                         |

## 1. Infrastructure Configuration (On Cluster)

The below diagram shows a high level overview of a **possible** Infrastructure diagram with components on the cluster, however your specific configuration may vary _(Image 1)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![Image 1:Infrastructure Configuration (On Cluster)](<../../../../.gitbook/assets/Technical Deep Dive - Tech Stack (1).png>)

## 2. AWS Infrastructure Configuration (Outside Cluster)

When deploying Cinchy version 5 on Kubernetes, you may deploy via Amazon Web Services (AWS). The below diagram shows a high level overview of a **possible** AWS Infrastructure with components outside the cluster, however your specific configuration may vary _(Image 2)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

### 2.1 Infrastructure Component Overview

![Image 2: AWS Infrastructure Configuration (Outside Cluster)](<../../../../.gitbook/assets/DNB Cloud Architecture - AWS.png>)

## 3. Azure Infrastructure Configuration (Outside Cluster)

When deploying Cinchy version 5 on Kubernetes, you may deploy via Microsoft Azure. The below diagram shows a high level overview of **possible** Azure Infrastructure with components outside the cluster, however your specific configuration may vary _(Image 3)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

### 3.1 Infrastructure Component Overview&#x20;

![Image 3: Azure Infrastructure Configuration (Outside Cluster)](<../../../../.gitbook/assets/DNB Cloud Architecture - Azure (2).png>)

## 4. Cluster Level Component Overview

The following highlighted area provides a high-level overview of cluster level components used when deploying Cinchy on Kubernetes, as well as what versions they are running.

These are created once per cluster. Clients may choose to run these components outside of the cluster or replace with their own comparable components. This diagram shows them in the cluster _(Image 4)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![Image 4: Cluster Level Component Overview](<../../../../.gitbook/assets/Cluster components Technical Deep Dive - Tech Stack copy 2.png>)

#### Cluster Level Components

These are created once per cluster. Clients may choose to run these components outside of the cluster or replace with their own comparable components.

* **Service Mesh -** [**Istio**](https://istio.io/)**:** All inbound traffic to your Cinchy instance is routed and handled through this component, keeping it secure and managed.
* **Monitoring/Alerting -** [**Prometheus**](https://prometheus.io/) **&** [**Grafana:**](https://prometheus.io/docs/visualization/grafana/) Prometheus consumes metrics from the running components in your environment, which can then be visualized into user friendly graphs and dashboards by Grafana. Prometheus can also connect to third party services to provide alerting capabilities. Both Prometheus and Grafana use persistent storage.
* **Logging -** [**Opensearch**](https://opensearch.org/) **and** [**Fluentbit**](https://fluentbit.io/)**:** All logs are captured and indexed by Opensearch in a single, easily accessible location. These logs can be queried, searched, and filtered, and Correlation IDs mean that they can also be traced across various components. These logging components take advantage of persistent storage.
* **Caching -** [**Redis**](https://redis.io/)**:** Redis is currently being used to facilitate a distributed lock using RedLock, which guarantees lock synchronizations across Cinchy instances. It is also a storage location for the execution output when running batch data syncs.
* **Event Processing -** [**Kafka**](https://kafka.apache.org/)**:** This is designed to act as the middleware that allows for messaging between components through a queuing mechanism. Kafka features persistent storage.&#x20;

### 4.1 Cluster Configuration

There are a few things to consider about your cluster configuration before you deploy Cinchy on Kubernetes:

* How many clusters will you need?
* Will you be sharing from an existing cluster?
* Will you be running multiple environments on a single cluster?

## 5. Instance Component Overview

Each instance of Cinchy has the following components, which are used to either provide an experience to users/applications or connect data in/out of Cinchy. Since multiple Cinchy instances can be deployed per cluster, these components will repeat for each environment.

The following highlighted area provides a high-level overview of instance level components used when running Cinchy on Kubernetes _(Image 5)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![Image 5: Instance Component Overview](<../../../../.gitbook/assets/Instance components Technical Deep Dive - Tech Stack copy.png>)

* **Meta Experiences:** Cinchy offers pre-packaged experiences that you can import into your Cinchy environment and use on your data network. This includes experiences like [Meta-Forms](https://cinchy.gitbook.io/cinchy-meta-forms/) and [Meta-Reports](https://cinchy.gitbook.io/cinchy-meta-reports/).
* **Connections:** The Cinchy Connections experience is used to help easily create data syncs in/out of the platform. It features persistent storage.
* **Data Browser:** Cinchy’s Dataware platform features a Universal Data Browser that allows users to view, change, analyze, and otherwise interact with all data on the network. The Data Browser even enables non-technical business users to manage and update data, build models, and set controls, all through an easy and intuitive UI.
* **Identity Provider:** An Identity Provider (IdP) creates and manages user credentials and associated identity attributes. IdPs authentication services are used in Cinchy to authenticate end-users.
* **Event Listener:** The Event Listener is used to picks up events from connected sources during a data sync. Review [Cinchy's Data Sync documentation](https://cli.docs.cinchy.com/) for further information on the Event Listener. The Event Listener uses persistent storage.
* **Event Stream Worker:** The Event Stream Worker is used to process data picked up by the Event Listener during data syncs. Review [Cinchy's Data Sync documentation ](https://cli.docs.cinchy.com/)for further information on the Event Stream Worker. The Event Worker uses persistent storage.
* **Maintenance (Batch Jobs):** Cinchy [performs maintenance tasks](https://cinchy.gitbook.io/cinchy-v5.0.0/deployment-guide/deployment-installation-guide/maintenance#maintenance) through the CLI. This currently includes the data erasure and data compression deletions.

## 6. GitOps&#x20;

[**ArgoCD**](https://argo-cd.readthedocs.io/en/stable/) is a declarative, GitOps continuous delivery tool for Kubernetes that simplifies the application deployment and lifecycle management. ArgoCD is highly recommended for deploying Cinchy, however you may also choose to use another tool.

Once you configurations are set, ArgoCD automates the deployment of the desired application states into your specified target environments. Implemented as a Kubernetes controller, it continuously monitors running applications and compares the current, live state against the desired target state (as specified in your repositories).
