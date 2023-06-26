# Updating the Kubernetes Image Registry

## Overview

The Kubernetes project runs a community-owned image registry called **registry.k8s.io** in which to host its container images. On April 3rd, 2023, the registry **k8s.gcr.io** was deprecated and no further images for Kubernetes and related subprojects are being pushed to this location.

**Instead, there is a new registry: **<mark style="color:green;">**registry.k8s.io.**</mark>

**New Cinchy Deployments:** this change will be automatically reflected in your installation.

**For Current Cinchy Deployments:** please follow the instructions outlined in this upgrade guide to ensure your components are pointed to the correct image repo.

You can review the full details on this change here: https://kubernetes.io/blog/2023/02/06/k8s-gcr-io-freeze-announcement/

## Update Instructions

1. In a shell/terminal, run the below command to get a list of all the pods that are pointing to the old registry: **k8s.gcr.io**. **These will need to be updated to point to the new image registry.**

```
kubectl get pods --all-namespaces -o jsonpath="{.items[*].spec.containers[*].image}" |\
tr -s '[[:space:]]' '\n' |\
sort |\
uniq -c
grep "k8s.gcr.io"
```

2. Once you find which pods are using old image registry, you need to update its deployment/daemonset/stateful set with new registry.k8s.io registry. Find the right resource type for your pods with below command:

```
kubectl get deployment,daemonset,statefulset --all-namespaces
```

3. Once you have your pod name and resource type, run the below command to open the manifest file in an editor:

{% hint style="danger" %}
The below command uses the **autoscaler** as an example. You will want to propagate the command with your correct **pod and resource type.**
{% endhint %}

```
kubectl edit deployment cluster-autoscaler -n kube-system
```

4. In the editor, find any instances of **k8s.gcr.io** and replace it with **registry.k8s.io.**
5. Save and close the file.
6. Repeat steps 3-5 for the rest of your pods until they are all pointing to the correct registry.
