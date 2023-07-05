# Configure AWS IAM for Connections

## Overview

In Cinchy v5.6, you are now able to run the Connections pod under a service account that uses an AWS IAM (Identity and Access Management) role, which is an IAM identity that you can create to have specific permissions and access to your AWS resources. To set up AWS IAM role authentication, please review the procedure below.

## AWS IAM role authentication

1. To check that you have an OpenID Connect set up with the cluster (the default for deployments made using the Cinchy automation process), run the below command within a terminal:

```bash
aws eks describe-cluster --name <CLUSTER_NAME> --query "cluster.identity.oidc.issuer"
```

2. The output should appear like the below. Make sure to note this down for later use.

```bash
https://oidc.eks.<REGION>.amazonaws.com/id/<OIDC_ID>
```

3. Log in to your AWS account and [create an IAM Role policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) through the AWS UI. Ensure that it has S3 access.
4. Run the below command in a terminal to create a service account with the role created in step 3. **If your cluster has a special character like an underscore, skip to the next section**.

{% code overflow="wrap" %}

```bash
eksctl create iamserviceaccount --name my-service-account --namespace default --cluster my-cluster --role-name "my-role" --attach-policy-arn arn:aws:iam::111122223333:policy/my-policy --approve
```

{% endcode %}

### Cluster names with special characters

If your cluster name has a special character, like an underscore, you will need to create and apply the YAML. Follow section 1 up until step 4, and then follow the below procedure.

1. In an IDE (Visual Studio, VsCode), create a new file titled `my-service-account.yaml` in your working directory. It should contain the below content.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: default
```

2. In a terminal, run the below command:

```bash
kubectl apply -f my-service-account.yaml -n <namespace>
```

3. In an IDE (Visual Studio, VsCode), create a new file titled `trust-relationship.json` in your working directory. It should contain the below content.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::$account_id:oidc-provider/$oidc_provider"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "$oidc_provider:aud": "sts.amazonaws.com",
          "$oidc_provider:sub": "system:serviceaccount:$namespace:$service_account"
        }
      }
    }
  ]
}
```

For example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::204393242335:oidc-provider/oidc.eks.ca-central-1.amazonaws.com/id/8A024CF24ADD3925BEFA224C4BDD005B"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "oidc.eks.ca-central-1.amazonaws.com/id/8A024CF24ADD3925BEFA224C4BDD005B:aud": "sts.amazonaws.com",
          "oidc.eks.ca-central-1.amazonaws.com/id/8A024CF24ADD3925BEFA224C4BDD005B:sub": "system:serviceaccount:devops-aurora-1:connections-serviceaccount-s3"
        }
      }
    }
  ]
}
```

1. Execute the following command to create the role, referencing the above .json file:

{% code overflow="wrap" %}

```bash
aws iam create-role --role-name my-role --assume-role-policy-document file://trust-relationship.json --description "my-role-description"
```

{% endcode %}

For example:

{% code overflow="wrap" %}

```
aws iam create-role --role-name connections-role-test --assume-role-policy-document file://trust-relationship.json --description "testing sa role for pod"
```

{% endcode %}

5. Execute the following command to attach the IAM policy to your role:

{% code overflow="wrap" %}

```bash
aws iam attach-role-policy --role-name my-role --policy-arn=arn:aws:iam::$account_id:policy/my-policy
```

{% endcode %}

For example:

{% code overflow="wrap" %}

```bash
aws iam attach-role-policy --role-name connections-role-test --policy-arn=arn:aws:iam::aws:policy/AmazonS3FullAccess
```

{% endcode %}

6. Execute the following command to annotate your service account with the Amazon Resource Name (ARN) of the IAM role that you want the service account to assume:

{% code overflow="wrap" %}

```bash
kubectl annotate serviceaccount -n $namespace $service_account eks.amazonaws.com/role-arn=arn:aws:iam::$account_id:role/my-role
```

{% endcode %}

For example:

{% code overflow="wrap" %}

```bash
kubectl annotate serviceaccount -n devops-aurora-1 connections-serviceaccount-s3 eks.amazonaws.com/role-arn=arn:aws:iam::204393242335:role/connections-role-test
```

{% endcode %}

7. Confirm that the role and service account are correctly configured by verifying the output of the following commands:

```bash
aws iam get-role --role-name my-role --query Role.AssumeRolePolicyDocument
aws iam get-role --role-name connections-role-test --query Role.AssumeRolePolicyDocument
```

### Authorize for Data Syncs

To ensure that the Connections pod's role has the correct permissions, the role specified by the user in AWS must have its **Trusted Relationships** configured as such:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "<role-arn-created-above>",
        "Service": "s3.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Confirmation

To confirm that the Connections app is using the service account:

1. Navigate to the **cinchy.kubernetes** repository > `connections/kustomization.yaml` file
2. Execute the following:

```yaml
- op: replace
      path: /spec/template/spec/serviceAccountName
      value: connections-serviceaccount-s3
```

3. From a terminal, run the below command:

```bash
kubectl describe deployment connections-app -n <namespace>
```

4. The output should look like the following:

<figure><img src="../../../.gitbook/assets/image (324).png" alt=""><figcaption></figcaption></figure>
