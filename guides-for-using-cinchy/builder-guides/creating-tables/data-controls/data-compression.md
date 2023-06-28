---
description: This page provides an overview of Data Compression.
---

# Data Compression

If you need to manage space within your database, you can set a data compression policy. Currently we allow you to permanently delete versions in the collaboration log. Be aware that the current version of compression is a LOSSY process (data will be permanently deleted). Take that into consideration when configuring a policy.

![](<../../../../.gitbook/assets/image (332).png>)

{% hint style="info" %}
We recommend you keep more versions rather than less versions. You can think of the above as keep any versions newer than 180 days and keeping the most recent 50 versions. So as long as a version satisfies one of the two keep conditions, we keep it. Using the example above:

* A version that’s from 205 days ago but is amongst the most recent 50 versions (e.g. version 22 of 60) will be kept, because it satisfies at least one condition of being in the most recent 50 versions.
* A version that’s from 163 days ago but is version 65 of 80 will be kept, because it satisfies at least one condition of being less than 180 days old.
* A version that’s from 185 days ago and is version 65 of 80 will be deleted because, it doesn’t satisfy either of the conditions.
{% endhint %}

The actual compression of data happens during the maintenance window. Please check with your system administrators to confirm when maintenance is scheduled.

### Change Approval Enabled Tables

The number of versions is based on the major version and not the minor version. This means for a record on version 35.63 with a keep most recent 10 versions, it will keep all versions 26.0 +, rather than all versions 35.44+.
