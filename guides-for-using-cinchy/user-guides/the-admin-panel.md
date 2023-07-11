---
description: This page details information about the Admin Panel on Cinchy.
---

# The Admin panel

You can view the admin panel of your Cinchy instance by [using the /admin/index endpoint.](../../api-guide/api-overview/#2.1-admin-index) This is only reachable if you are logged in as a user with admin access. The admin panel includes the following sections:

## Cinchy Healthcheck

The Cinchy Healthcheck shows information about your system such as current version, IP Address, the system time, and your database status _(Image 1)._

![Image 1: The Cinchy Healthcheck](<../../.gitbook/assets/image (587).png>)

## Cinchy log files

This section shows a list of viewable log files from your system, as well as their size, creation time, and last modified time _(Image 2)._

{% hint style="warning" %}
Log files in the Admin Panel are only visible on Cinchy deployments on IIS, or on a version earlier than v5. In those cases, you will need to [navigate to OpenSearch (or comparable component).](../../deployment-guide/deployment-installation-guides/kubernetes-deployment-installation/#5.3.1-accessing-opensearch)
{% endhint %}

![Image 2: Cinchy Log Files](<../../.gitbook/assets/image (95).png>)

## Upload a logo

Review the [ Data Browser page ](overview-of-the-data-browser.md#5.-the-logo)for information on uploading a logo.
