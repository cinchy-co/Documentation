---
description: >-
  This page details the optional steps that you can take to use self-signed SSL
  Certificates in a Kubernetes Deployment of Cinchy.
---

# Using Self-Signed SSL Certs (Kubernetes Deployments)

{% hint style="danger" %}
This process needs to be followed **after** running the devops.automations script during your initial deployment, as well as **each additional time** that you run the script (Ex: updating your Cinchy platform), since it will wipe out all of the custom configuration you set up to use a self-signed certificate.
{% endhint %}

1. Generate the self-signed certificate by executing the following commands in any folder:

```
openssl genrsa -des3 -out rootCA.key 4096
```

```
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt
```

```
openssl genrsa -out mydomain.com.key 2048
```

```
openssl req -new -sha256 -key mydomain.com.key -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=mydomain.com " -out mydomain.com.csr
```

2\. Create a yaml file located at **cinchy.kubernetes/platform\_components/base/self-signed-ssl-root-ca.yaml.**

3\. Add the following to the yaml file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: self-signed-ca-pemstore
data:
  rootCA.crt: |
    <rootCA.crt>
```

4\. Add the self signed root CA cert file to the _**cinchy.kubernetes/environment\_kustomizations/cinchy\_nonprod/base**_ folder.

5\. Add the yaml code snippet to the _**cinchy.kubernetes/environment\_kustomizations/cinchy\_nonprod/base/kustomization.yaml**_ file, changing the below files key value as per your **root ca cert file name**:

```yaml
configMapGenerator:
- name: self-signed-ca-pemstore
  behavior: replace
  files:
  - rootCA.crt
```

6\. Add the following line to the  _**cinchy.kubernetes/platform\_components/base/kustomization.yaml**_ file

```yaml
- self-signed-ssl-root-ca.yaml
```

7\. Add the below Deployment patchesJson6902 to each of your _**cinchy.kubernetes/environment\_kustomizations/cinchy\_nonprod/ENV\_NAME/PLATFORM\_COMPONENT\_NAME/kustomization.yaml**_ files, except "base".

* Ensure that the **rootCA.crt** file name is matched with ConfigMap data, configMapGenerator files, and the patch subpath.

```yaml
    - op: add
      path: /spec/template/spec/volumes/-
      value: 
        configMap:
          name: self-signed-ca-pemstore
        name: self-signed-ca-pemstore  
    - op: add
      path: /spec/template/spec/containers/0/volumeMounts/-
      value: 
        mountPath: /etc/ssl/certs/rootCA.crt
        name: self-signed-ca-pemstore
        subPath: rootCA.crt
```

8\. Once the changes are deployed, verify the root CA cert is available on the pod under _/etc/ssl/certs_ with below command, inputing your own POD\_NAME and NAMESPACE where noted:

```
 kubectl exec -it POD_NAME -n NAMESPACE -- openssl x509 -in /etc/ssl/certs/rootCA.crt -text
```

{% hint style="info" %}
For further reference material, [click here.](https://paraspatidar.medium.com/add-self-signed-or-ca-root-certificate-in-kubernetes-pod-ca-root-certificate-store-cb7863cb3f87#b760)
{% endhint %}
