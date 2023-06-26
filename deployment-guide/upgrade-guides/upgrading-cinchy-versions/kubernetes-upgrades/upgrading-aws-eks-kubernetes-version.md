---
description: >-
  This page will guide you through how to update your AWS EKS Kubernetes version
  for your Cinchy v5 platform.
---

# Upgrading AWS EKS Kubernetes Version

## 1. Overview

This page will guide you through how to update your AWS EKS Kubernetes version for your Cinchy v5 platform.

## 2. Prerequisites

* Update your Cinchy platform to the [latest version.](../../)
* Confirm the latest Cinchy supported version of EKS. You can find the version number in the  **cinchy.devops.automations\aws-deployment.json** as _"cluster\_version": "1.xx"_.

## 3. Considerations

* You must upgrade your EKS sequentially. For example, if you are on EKS cluster version 1.22 and wish to upgrade to 1.24, you must upgrade from 1.22 > 1.23 > 1.24.

## 4. Upgrade Instructions

1. Navigate to your **cinchy.devops.automations\aws-deployment.json** file.
2. Change the **cluster\_version** key value to the EKS version you wish to upgrade to. _(Example: "1.24")_
3. Open a shell/terminal and navigate to the **cinchy.devops.automations directory location.**
4. Execute the following command:

```
dotnet Cinchy.DevOps.Automations.dll "deployment.json"
```

5. Commit changes for all the repositories **(cinchy.argocd, cinchy.kubernetes, cinchy.terraform and cinchy.devops.automation).**
6. Open a new shell/terminal and navigate to the **cinchy.terraform\aws\eks\_cluster\CLUSTER\_NAME directory location.**
7. Execute the following command:

```
bash create.sh
```

7. Verify the changes are as expected and then accept.

This process will first upgrade the managed master node version and then the worker nodes. During the upgrade process, existing pods get migrated to new worker nodes and all pods will get migrated to new upgraded worker nodes automatically.&#x20;

The below two commands can be used to verify that all pods are being migrated to new worker nodes.

* To show both old and new nodes:

```
kubectl get nodes #
```

* To show all the pods on the new worker nodes and the old worker nodes.

```
kubectl get pods --all-namespaces -o wide #
```

## 5. Reinstalling the Metrics Server

**For EKS version 1.24, the metrics server goes into a crashed loop status. Reinstalling the metrics server will fix this, should you encounter this during your upgrade.**

1. In a code editor, open the **cinchy.terraform\aws\eks\_cluster\CLUSTER\_NAME\new-vpc.tf or existing-VPC.tf file.**
2. Find the **enable\_metrics\_server key** and mark its value to **false.**
3. Open a new shell/terminal and navigate to the **cinchy.terraform\aws\eks\_cluster\CLUSTER\_NAME file.**
4. Run the below command to remove metrics server.

```
terraform apply
```

5. Revert the **enable\_metrics\_server key** value from step 1 to **true.**
6. Run the below command within the same shell/terminal as step 3 to deploy metrics server with

```
 terraform apply
```
