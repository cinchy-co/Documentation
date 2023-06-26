---
description: This page serves as a general guide to using ArgoCD for monitoring purposes
---

# Monitoring via ArgoCD

## Table of Contents

| Table of Contents                                                                                    |
| ---------------------------------------------------------------------------------------------------- |
| [#1.-argocd-overview](monitoring-via-argocd.md#1.-argocd-overview "mention")                         |
| [#2.-getting-started-with-argocd](monitoring-via-argocd.md#2.-getting-started-with-argocd "mention") |
| [#3.-your-argocd-dashboard](monitoring-via-argocd.md#3.-your-argocd-dashboard "mention")             |
| [#4.-filtering](monitoring-via-argocd.md#4.-filtering "mention")                                     |
| [#5.-detailed-view](monitoring-via-argocd.md#5.-detailed-view "mention")                             |
|                                                                                                      |

## 1. ArgoCD Overview

ArgoCD is implemented as a kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in your Git repo).

You can use ArgoCD's dashboard _(Image 1)_ to visually monitor your namespaces and pods, and to quickly visualize deployment issues. It can easily show you what your cluster or pods are doing, and if they are healthy.

![Image 1: An example ArgoCD Dashboard](<../../../.gitbook/assets/image (514).png>)

## 2. Getting Started with ArgoCD

ArgoCD has a robust set of documentation that can help you to get started with the application. We recommend the following two pages:

* [A General Overview](https://argo-cd.readthedocs.io/en/stable/)
* [ArgoCD Metrics](https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/)

## 3. Your ArgoCD Dashboard

Your ArgoCD dashboard contains a lot of important information about how your Cinchy instance is behaving.

### 3.1 Application Tiles View

The application tiles view is the default view when logging into ArgoCD. You can also access it through the grid widget in the upper right hand corner of the screen.

![](<../../../.gitbook/assets/image (645).png>)

These application tiles each represent a pod in one of your Cinchy environments. By default, you should have an application tile for each of the following pods per namespace _(Image 2)_:

* Base
* Connections
* Event Listener
* IDP
* Maintenance CLI
* Meta Forms
* Web
* Worker

Each application tile shows you some important information about the pod, including its health and sync status. You can easily use the **Sync, Refresh, and Delete** buttons to manage your pods.

You can also click the **star button** on any application title to favourite the associated pod.

![Image 2: A pod in ArgoCD](<../../../.gitbook/assets/image (562).png>)

### 3.2 Pie Chart View

The pie chart view _(Image 3)_ shows an easy visualization of the health of your applications. Access this view by clicking on the pie chart widget located in the upper right hand corner of the screen.

![Image 3: Pie chart view](<../../../.gitbook/assets/image (293).png>)

## 4. Filtering

You can filter your view so that it only shows certain data. You can find the various filter options from the Filter column on the left hand side of any of the data views.

* **Favorites Only:** You can filter by only the tiles that you have favourited _(Image 4)._

![Image 4: Filter by Favorites](<../../../.gitbook/assets/image (114).png>)

* **Sync Status:** You can easily view what is out of sync using this filter _(Image 5)._

![Image 5: Filter by Sync Status](<../../../.gitbook/assets/image (504).png>)

* **Health Status:** Check on the health of your applications by filtering using these parameters _(Image 6)._

![Image 6: Filter by Health Status](<../../../.gitbook/assets/image (465).png>)

* **Labels:** Use these labels to filter by specific instances/environments _(Image 7)._

![Image 7: Filtering via label](<../../../.gitbook/assets/image (299).png>)

* **Projects:** If you choose to group your applications into projects, you can filter them using this tile _(Image 8)._

![Image 8: Filtering via project](<../../../.gitbook/assets/image (264).png>)

* **Clusters:** If you have more than one cluster, you can filter for it using the Clusters tile _(Image 9)._

![Image 9: Filtering by cluster](<../../../.gitbook/assets/image (548).png>)

* **Namespace:** Lastly, you can filter by Namespace. The below example shows a filter based on the dev-aurora-1 namespace _(Image 10)._

![Image 10: Namespace view](<../../../.gitbook/assets/image (379).png>)

## 5. Detailed View

To bring up a more detailed view of your applications, click on the application tile. This view will show you all of components, their health and their sync status _(Image 11)._ You can use the top navigational buttons perform actions such as syncing, rolling back, or refreshing.

This view can be particularly useful for load testing, snice you can see each individual pod spinning up and down.

![Image 11: Detailed view](<../../../.gitbook/assets/image (32).png>)

### 5.1 Health and Sync Status&#x20;

Quickly view the status of your apps by looking at the health and sync status along the top of the page _(Image 12)._

![Image 12: Health and Sync Status](<../../../.gitbook/assets/image (281).png>)

### 5.2 Summary Information&#x20;

Clicking on any individual pod or component tile in this view will bring up its information, including a **Summary**, a list of **Events**, your **Manifest**, and **Parameters** _(Image 13)._&#x20;

{% hint style="warning" %}
You can also use this screen to edit or delete applications.
{% endhint %}

![Image 13: Individual information](<../../../.gitbook/assets/image (412).png>)

### 5.3 Sync Policies

From the detailed tile summary you can also set your sync policies, such as automation, resource pruning, and self healing _(Image 14)._

![Image 14: Sync Policy](<../../../.gitbook/assets/image (481).png>)

### 5.4 Logs

You can click on the "Logs" tab in your detailed summary page to view the applicable logs for your selected pod _(Image 15)._ You can filter, follow, snooze, copy, or download any logs.

![Image 15: Logs](<../../../.gitbook/assets/image (583).png>)
