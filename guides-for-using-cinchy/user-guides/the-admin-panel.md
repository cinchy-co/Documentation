---
description: This page details information about the Admin Panel on Cinchy.
---

# The Admin Panel

## 1. Admin Panel

You can view the admin panel of your Cinchy instance by [using the /admin/index endpoint.](../../api-guide/api-overview/#2.1-admin-index) This is only reachable if you are logged in as a user with admin access. The admin panel includes the following sections:

### 1.1 Cinchy Healthcheck_:_

The Cinchy Healthcheck shows information about your system such as current version, IP Address, the system time, and your database status _(Image 1)._

![Image 1: The Cinchy Healthcheck](<../../.gitbook/assets/image (130).png>)

### 1.2 Cinchy Log Files_:_

This section shows a list of viewable log files from your system, as well as their size, creation time, and last modified time _(Image 2)._

{% hint style="warning" %}
Log files in the Admin Panel are only visible on Cinchy deployments on IIS, or on a version prior to v5. In those cases, you will need to [navigate to Opensearch (or comparable component).](../../deployment-guide/deployment-installation-guides/kubernetes-deployment-installation/#5.3.1-accessing-opensearch)
{% endhint %}

![Image 2: Cinchy Log Files](<../../.gitbook/assets/image (742).png>)

### 1.3 Upload a Logo

Review the [documentation here ](overview-of-the-data-browser.md#5.-the-logo)for information on uploading a logo.
