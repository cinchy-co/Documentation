---
description: >-
  This page details the deployment architecture of Cinchy v5 when running on
  Kubernetes.
---

# Kubernetes deployment architecture


## Infrastructure configuration (on cluster)

The diagram below shows a high level overview of a **possible** Infrastructure diagram with components on the cluster, but your specific configuration may vary _(Image 1)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![Image 1:Infrastructure Configuration (On Cluster)](<../../../../.gitbook/assets/Technical Deep Dive - Tech Stack (1).png>)

## AWS infrastructure configuration (outside cluster)

When deploying Cinchy version 5 on Kubernetes, you may deploy via Amazon Web Services (AWS). The diagram below shows a high level overview of a **possible** AWS Infrastructure with components outside the cluster, but your specific configuration may vary _(Image 2)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

### Infrastructure component overview

![Image 2: AWS Infrastructure Configuration (Outside Cluster)](<../../../../.gitbook/assets/DNB Cloud Architecture - AWS.png>)

## Azure infrastructure configuration (outside cluster)

When deploying Cinchy v5 on Kubernetes, you may deploy via Microsoft Azure. The diagram below shows a high level overview of **possible** Azure Infrastructure with components outside the cluster, but your specific configuration may vary _(Image 3)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

### Infrastructure component overview

![Image 3: Azure Infrastructure Configuration (Outside Cluster)](<../../../../.gitbook/assets/DNB Cloud Architecture - Azure (2).png>)

## Cluster level component overview

The following highlighted area provides a high-level overview of cluster level components used when deploying Cinchy on Kubernetes and what versions they're running.

These are created once per cluster. Clients may choose to run these components outside of the cluster or replace with their own comparable components. This diagram shows them in the cluster _(Image 4)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![Image 4: Cluster Level Component Overview](<../../../../.gitbook/assets/Cluster components Technical Deep Dive - Tech Stack copy 2.png>)

#### Cluster level components

These are created once per cluster. Clients may choose to run these components outside of the cluster or replace with their own comparable components.

* **Service Mesh -** [**Istio**](https://istio.io/)**:** Istio handles and routes all inbound traffic to your Cinchy instance, keeping it secure and managed.
* **Monitoring/Alerting -** [**Prometheus**](https://prometheus.io/) **&** [**Grafana:**](https://prometheus.io/docs/visualization/grafana/) Prometheus consumes metrics from the running components in your environment, which you can visualize into user friendly graphs and dashboards by Grafana. Prometheus can also connect to third party services to provide alerting capabilities. Both Prometheus and Grafana use persistent storage.
* **Logging -** [**OpenSearch**](https://opensearch.org/) **and** [**Fluent Bit**](https://fluentbit.io/)**:** OpenSearch captures and indexes all logs in a single, accessible location. These logs can be queried, searched, and filtered, and Correlation IDs mean that they can also be traced across various components. These logging components take advantage of persistent storage.
* **Caching -** [**Redis**](https://redis.io/)**:** Redis facilitates a distributed lock using RedLock, which guarantees lock synchronizations across Cinchy instances. It's also a storage location for the execution output when running batch data syncs.
* **Event Processing -** [**Kafka**](https://kafka.apache.org/)**:** This acts as the middleware for messaging between components through a queuing mechanism. Kafka features persistent storage.

### Cluster configuration

Before you deploy Cinchy on Kubernetes, consider the following about your cluster configuration :

* How many clusters will you need?
* Will you be sharing from an existing cluster?
* Will you be running multiple environments on a single cluster?

## Instance component overview

Each Cinchy instance uses the following components to either provide an experience to users/applications or connect data in/out of Cinchy. You can deploy multiple Cinchy instances per cluster, so these components will repeat for each environment.

The following highlighted area provides a high-level overview of instance level components used when running Cinchy on Kubernetes _(Image 5)._

{% hint style="success" %}
Tip: Click on an image to enlarge it.
{% endhint %}

![Image 5: Instance Component Overview](<../../../../.gitbook/assets/Instance components Technical Deep Dive - Tech Stack copy.png>)

* **Meta Experiences:** Cinchy offers pre-packaged experiences that you can import into your Cinchy environment and use on your data network. This includes experiences like [Meta-Forms](https://cinchy.gitbook.io/cinchy-meta-forms/).
* **Connections:** Use the Cinchy Connections experience to create data syncs in/out of the platform. It features persistent storage.
* **Data Browser:** Cinchy’s data collaboration platform features a Universal Data Browser that allows users to view, change, analyze, and otherwise interact with all data on the network. The Data Browser even enables non-technical business users to manage and update data, build models, and set controls, all through an easy and intuitive UI.
* **Identity Provider:** An Identity Provider (IdP) creates and manages user credentials and associated identity attributes. Cinchy uses IdPs authentication services to authenticate end-users.
* **Event Listener:** The Event Listener picks up events from connected sources during a data sync. Review the [Data Sync page ](https://cli.docs.cinchy.com/) for further information on the Event Listener. The Event Listener uses persistent storage.
* **Event Stream Worker:** The Event Stream Worker processes data picked up by the Event Listener during data syncs. Review the [ Data Sync page ](https://cli.docs.cinchy.com/)for further information on the Event Stream Worker. The Event Worker uses persistent storage.
* **Maintenance (Batch Jobs):** Cinchy [performs maintenance tasks](https://cinchy.gitbook.io/cinchy-v5.0.0/deployment-guide/deployment-guide/maintenance#maintenance) through the CLI. This includes the data erasure and data compression deletions.

## GitOps

[**ArgoCD**](https://argo-cd.readthedocs.io/en/stable/) is a declarative, GitOps continuous delivery tool for Kubernetes that simplifies the application deployment and lifecycle management. ArgoCD is highly recommended for deploying Cinchy, but you can also use another tool.

Once you set up the configurations, ArgoCD automates the deployment of the desired application states into your specified target environments. Implemented as a Kubernetes controller, it continuously monitors running applications and compares the current, live state against the desired target state (as specified in your repositories).
