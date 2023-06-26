---
description: >-
  This page details the process for upgrading your AKS instance once you've
  deployed Cinchy v5 on Kuubernetes.
---

# Upgrading AKS (Azure Kubernetes Service)

## 1. Overview

**AKS (Azure Kubernetes Service)** is a managed K8 service used when deploying Cinchy v5 over Azure. Once you have deployed your v5 instance of Cinchy, you may want to upgrade your AKS to a newer version. You can do so with the below guide.

## 2. Prerequisites

Before proceeding for the AKS version upgrade, ensure that you have enough IP address space, as this process will need to start new nodes and pods.

You will need **more than 1024 IP addresses in total** when you have single Cinchy instance/environment.&#x20;

If you have limited IP address space, you can upgrade the master node and then the worker node pools **one by one**.

{% hint style="warning" %}
Before proceeding, make sure to check with Cinchy support which versions of  Kubernetes are supported by this process.
{% endhint %}

## Upgrading AKS

1. Navigate to your ArgoCD Dashboard.
2. Find and click on your **Istio app > disable auto-sync.**
3. Find and click on your **Istio-ingress app > disable auto-sync.**
4. Open a terminal and run the following command to get the currently available Kubernetes versions for AKS, inputting your **AKS cluster name** and **resource group** where specified:

```bash
az aks get-upgrades --resource-group <myresourcegroup> --name <myaksclustername> --output table
```

5\. Within your **cinchy.devops.automations folder/repo,** navigate to the **deployment-azure.json file** and change the values for `kubernetes_version` and `orchestrator_version` to the **currently available Kubernetes version** found in step 4.

6\. Within the **cinchy.devops.automations folder/repo**, run the following command to push your new values:

```bash
dotnet Cinchy.DevOps.Automations.dll deployment-azure.json
```

7\. Within your **cinchy.terraform/azure/aks\_cluster/** directory, run `bash create.sh` and accept the change.&#x20;

8\. AKS has three node pools, one node pool per availability zone. Terraform will now start the upgrade of the master node and the zone1 node pool.

9\. Verify which nodes that the **istio-ingressgateway and istiod pods** are running on with below command.

```bash
kubectl get pods -n istio-system -o wide
```

10\. In the output you will see **aks-zone1/2/3-xxxxx** under the **Nodes** column.

If one or both pods are running on **aks-zone1-xxxxx** then you need to scale up replicas 2 by following steps 12-14. **If not, skip to step 15.**

11\. Use the following command to start replica on zone2 or zone3 nodes; once you scale down it will terminate from zone1 upgrading nodes.

```bash
kubectl scale --replicas=2 deployment.apps/istio-ingressgateway -n istio-system
kubectl scale --replicas=2 deployment.apps/istiod -n istio-system
```

12\. Verify that new pods are running on other node pool by running the following command:

```bash
kubectl get pods -n istio-system -o wide
```

13\. Run the following command to scale it back. This will remove the pod from cordoned node:

```bash
kubectl scale --replicas=1 deployment.apps/istio-ingressgateway -n istio-system
kubectl scale --replicas=1 deployment.apps/istiod -n istio-system
kubectl get pods -n istio-system -o wide
```

14\. Once Maser node and zone1 upgrade is done, you will see a message such as:

{% code overflow="wrap" %}
```
module.aks.azurerm_kubernetes_cluster.main: Modifications complete after 9m23s [id=/subscriptions/e5d6912-eeaa-461c-8f6-30e7c6d945a/resourceGroups/<vnet>/providers/Microsoft.ContainerService/managedClusters/<myaksclustername>]
```
{% endcode %}

15\. The Terraform script will continue with the zone2 and zone3 node pool upgrade. You will see messages like the below when thee zone2 and zone3 node pool upgrade is in process:

{% code overflow="wrap" %}
```
module.aks-node-pool.azurerm_kubernetes_cluster_node_pool.node_pool["zone2"]: Modifying... [id=/subscriptions/ed16912-eeaa-461c-8f36-30e76de945a/resourceGroups/<vnet>/providers/Microsoft.ContainerService/managedClusters/<myaksclustername>/agentPools/zone2]`
```
{% endcode %}

{% code overflow="wrap" %}
```
`module.aks-node-pool.azurerm_kubernetes_cluster_node_pool.node_pool["zone3"]: Modifying... [id=/subscriptions/e5d1612-eeaa-461c-8f36-30e7c6e945a/resourceGroups/<vnet>/providers/Microsoft.ContainerService/managedClusters/<myaksclustername>/agentPools/zone3]
```
{% endcode %}

16\. Repeat steps 11-13. This will make sure that **istio-ingressgateway and istiod** are running on zone1 nodes.

17\. Return to your ArgoCD dashboard and **re-enable the auto sync** for Istio and Istio-ingress apps auto sync on argocd dashboard.
