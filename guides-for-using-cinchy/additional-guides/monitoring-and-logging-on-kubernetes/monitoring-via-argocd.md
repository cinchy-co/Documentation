---
description: This page serves as a general guide to using ArgoCD for monitoring purposes
---

# Monitoring via ArgoCD

## ArgoCD Overview

ArgoCD is implemented as a Kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in your Git repository).

You can use ArgoCD's dashboard _(Image 1)_ to visually monitor your namespaces and pods, and to quickly visualize deployment issues. It can easily show you what your cluster or pods are doing, and if they're healthy.

![Image 1: An example ArgoCD dashboard](<../../../.gitbook/assets/image (127).png>)

## Get started with ArgoCD

ArgoCD has a robust set of documentation that can help you to get started with the application. We recommend the following two pages:

* [A General Overview](https://argo-cd.readthedocs.io/en/stable/)
* [ArgoCD Metrics](https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/)

## ArgoCD dashboard

Your ArgoCD dashboard has a lot of important information about how your Cinchy instance is behaving.

### Application tiles view

The application tiles view is the default view when logging into ArgoCD. You can also access it through the grid widget in the upper right hand corner of the screen.

![](<../../../.gitbook/assets/image (546).png>)

These application tiles each represent a pod in one of your Cinchy environments. By default, you should have an application tile for each of the following pods per namespace _(Image 2)_:

* Base
* Connections
* Event Listener
* IDP
* Maintenance CLI
* Meta Forms
* Web
* Worker

Each application tile shows you some important information about the pod, including its health and sync status. You can use the **Sync, Refresh, and Delete** buttons to manage your pods.

You can also click the **star button** on any application title to favourite the associated pod.

![Image 2: A pod in ArgoCD](<../../../.gitbook/assets/image (542).png>)

### Pie chart view

The pie chart view _(Image 3)_ shows an easy visualization of the health of your applications. Access this view by clicking on the pie chart widget located in the upper right hand corner of the screen.

![Image 3: Pie chart view](<../../../.gitbook/assets/image (231).png>)

## Filtering

You can filter your view so that it only shows certain data. You can find the various filter options from the Filter column on the left hand side of any of the data views.

* **Favorites Only:** Filter by only favorite tiles _(Image 4)._

![Image 4: Filter by Favorites](<../../../.gitbook/assets/image (420).png>)

* **Sync Status:** You can easily view what's out of sync using this filter _(Image 5)._

![Image 5: Filter by Sync Status](<../../../.gitbook/assets/image (206).png>)

* **Health Status:** Check on the health of your applications by filtering using these parameters _(Image 6)._

![Image 6: Filter by Health Status](<../../../.gitbook/assets/image (96).png>)

* **Labels:** Use these labels to filter by specific instances/environments _(Image 7)._

![Image 7: Filtering via label](<../../../.gitbook/assets/image (238).png>)

* **Projects:** If you choose to group your applications into projects, you can filter them using this tile _(Image 8)._

![Image 8: Filtering via project](<../../../.gitbook/assets/image (573).png>)

* **Clusters:** If you have more than one cluster, you can filter for it using the Clusters tile _(Image 9)._

![Image 9: Filtering by cluster](<../../../.gitbook/assets/image (172).png>)

* **Namespace:** Lastly, you can filter by Namespace. The below example shows a filter based on the dev-aurora-1 namespace _(Image 10)._

![Image 10: Namespace view](<../../../.gitbook/assets/image (594).png>)

## Detailed view

To bring up a more detailed view of your applications, click on the application tile. This view will show you all components, their health and their sync status _(Image 11)._ You can use the top navigational buttons perform actions such as syncing, rolling back, or refreshing.

This view can be useful for load testing, since you can see each individual pod spinning up and down.

![Image 11: Detailed view](<../../../.gitbook/assets/image (513).png>)

### Health and sync status

View the status of your apps by looking at the health and sync status along the top of the page _(Image 12)._

![Image 12: Health and Sync Status](<../../../.gitbook/assets/image (218).png>)

### Summary information

Clicking on any individual pod or component tile in this view will bring up its information, including a **Summary**, a list of **Events**, your **Manifest**, and **Parameters** _(Image 13)._

{% hint style="warning" %}
You can also use this screen to edit or delete applications.
{% endhint %}

![Image 13: Individual information](<../../../.gitbook/assets/image (254).png>)

### Sync policies

From the detailed tile summary you can also set your sync policies, such as automation, resource pruning, and self healing _(Image 14)._

![Image 14: Sync Policy](<../../../.gitbook/assets/image (11).png>)

### Logs

You can click on the **Logs** tab in your detailed summary page to view the applicable logs for your selected pod _(Image 15)._ You can filter, follow, snooze, copy, or download any logs.

![Image 15: Logs](<../../../.gitbook/assets/image (106).png>)
