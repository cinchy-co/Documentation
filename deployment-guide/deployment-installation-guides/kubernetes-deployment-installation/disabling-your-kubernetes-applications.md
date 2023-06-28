# Disabling your Kubernetes Applications

## 1. Disabling your Applications

There may be times when you want to temporarily disable your Kubernetes pods in order to perform maintenance or certain upgrades. You can do so through the following steps:

1. Access your ArgoCD
2. Navigate to the application directory for the namespace you wish to disable, in this case **development-cinchy** _(Image 1)._ You should see your cluster component applications.

![Image 1: Applications](../../../.gitbook/assets/image.png)

3\. Click on the main application (i.e. **development-cinchy**) _(Image 2)._

![Image 3: Navigate to your main app](<../../../.gitbook/assets/image (379).png>)

4\. Navigate to **Summary > Sync Policy > Automated**. Click on **Disable Auto-Sync > OK** _(Image 3)._

![Image 4: Click on "Disable Auto-Sync"](<../../../.gitbook/assets/image (224).png>)

5\. For each of the cluster applications that you wish to disable, click on the **"..." > Delete** _(Image 5)._

![Image 5: Delete your applications](<../../../.gitbook/assets/image (393).png>)

6\. Your apps should all appear as **"out of sync"** _(Image 6)._

![Image 6: Your apps should all appear out of sync](<../../../.gitbook/assets/image (329).png>)

## 2. Re-enabling your Applications

1. To re-enable your applications, return to the application directory for your disabled namespace _(Image 7)._

![Image 7: Navigate to your app directory](<../../../.gitbook/assets/image (382).png>)

2\. Click on the main application (i.e. **development-cinchy**) _(Image 8)._

![Image 8: Navigate to your main app](<../../../.gitbook/assets/image (186).png>)

3\. Navigate to **Summary > Sync Policy**. Click on **Enable Auto-Sync > OK** _(Image 9)._

![Image 9: Enable your Auto Sync](<../../../.gitbook/assets/image (180).png>)
