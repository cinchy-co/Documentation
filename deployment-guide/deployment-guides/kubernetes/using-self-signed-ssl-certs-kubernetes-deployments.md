---
description: >-
  This page details the optional steps that you can take to use self-signed SSL
  Certificates in a Kubernetes Deployment of Cinchy.
---

# Use self-signed SSL certificates (Kubernetes Deployments)

{% hint style="danger" %}
Follow this process only **after** running the **devops.automations** script during your initial deployment and **each additional time** you run the script (such as updating your Cinchy platform), as it wipes out all custom configurations you set up to use a self-signed certificate.
{% endhint %}

1.  Execute the following commands in any folder to generate the self-signed certificate:

```bash
openssl genrsa -des3 -out rootCA.key 4096
```

```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt
```

```bash
openssl genrsa -out mydomain.com.key 2048
```

```bash
openssl req -new -sha256 -key mydomain.com.key -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=mydomain.com " -out mydomain.com.csr
```

2. Create a YAML file located at **cinchy.kubernetes/platform\_components/base/self-signed-ssl-root-ca.yaml.**

3. Add the following to the YAML file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: self-signed-ca-pemstore
data:
  rootCA.crt: |
    <rootCA.crt>
```

4. Add the self signed root CA cert file to the _**cinchy.kubernetes/environment\_kustomizations/cinchy\_nonprod/base**_ folder.

5. Add the yaml code snippet to the _**cinchy.kubernetes/environment\_kustomizations/cinchy\_nonprod/base/kustomization.yaml**_ file, changing the below files key value as per your **root ca cert file name**:

```yaml
configMapGenerator:
- name: self-signed-ca-pemstore
  behavior: replace
  files:
  - rootCA.crt
```

6. Add the following line to the  _**cinchy.kubernetes/platform\_components/base/kustomization.yaml**_ file

```yaml
- self-signed-ssl-root-ca.yaml
```

7. Add the below Deployment patchesJson6902 to each of your _**cinchy.kubernetes/environment\_kustomizations/cinchy\_nonprod/ENV\_NAME/PLATFORM\_COMPONENT\_NAME/kustomization.yaml**_ files, except `base`.

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

1. Once the changes are deployed, verify the root CA cert is available on the pod under _/etc/ssl/certs_ with below command. Make sure to input your own `POD_NAME` and `NAMESPACE`:

```bash
 kubectl exec -it POD_NAME -n NAMESPACE -- openssl x509 -in /etc/ssl/certs/rootCA.crt -text
```

{% hint style="info" %}
For further reference material, [see the linked article on self-signed certificates in Kubernetes](https://paraspatidar.medium.com/add-self-signed-or-ca-root-certificate-in-kubernetes-pod-ca-root-certificate-store-cb7863cb3f87#b760).
{% endhint %}
