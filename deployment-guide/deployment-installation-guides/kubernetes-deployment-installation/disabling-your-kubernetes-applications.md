# Disable your Kubernetes applications

## Disable your applications

There might be times when you want to temporarily disable your Kubernetes pods to perform maintenance or upgrades. You can do so through the following steps:

1. Access your ArgoCD.
2. Navigate to the application directory for the `namespace` you wish to disable, in this case **development-cinchy** _(Image 1)._ You should see your cluster component applications.

![Image 1: Applications](<../../../.gitbook/assets/image (31).png>)

3. Select the main application (i.e. **development-cinchy**) _(Image 2)._

![Image 3: Navigate to your main app](<../../../.gitbook/assets/image (641).png>)

4. Navigate to **Summary > Sync Policy > Automated**, then select **Disable Auto-Sync > OK** _(Image 3)._

![Image 4: Select "Disable Auto-Sync"](<../../../.gitbook/assets/image (585).png>)

5. For each of the cluster applications that you wish to disable, select the **"..." > Delete** _(Image 5)._

![Image 5: Delete your applications](<../../../.gitbook/assets/image (285).png>)

6. Your apps should all appear as **"out of sync"** _(Image 6)._

![Image 6: Your apps should all appear out of sync](<../../../.gitbook/assets/image (316).png>)

## Re-enable your applications

1. To re-enable your applications, return to the application directory for your disabled `namespace` _(Image 7)._

![Image 7: Navigate to your app directory](<../../../.gitbook/assets/image (649).png>)

2. Select the main application (i.e. **development-cinchy**) _(Image 8)._

![Image 8: Navigate to your main app](<../../../.gitbook/assets/image (22) (1).png>)

3\. Navigate to **Summary > Sync Policy**, then select **Enable Auto-Sync > OK** _(Image 9)._

![Image 9: Enable your Auto Sync](<../../../.gitbook/assets/image (732).png>)
