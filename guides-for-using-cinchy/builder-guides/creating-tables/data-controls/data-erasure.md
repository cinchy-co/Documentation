---
description: This page provides an overview on data erasure.
---

# Data Erasure

## Overview

Data erasure allows you to permanently delete data in Cinchy. As the data owner, you can set an erasure policy on your table if you need to delete data for compliance reasons _(Image 1)._

![Image 1: Data Erasure](<../../../../.gitbook/assets/image (155).png>)

The actual erasing of data happens during the maintenance window. Please check with your system administrators to confirm when maintenance is scheduled.

Once data is erased, any links pointing to erased data will look like this _(Image 2)_:

![Image 2: Data Erasure](<../../../../.gitbook/assets/image (626).png>)

## 2. Change Approval Enabled Tables

The time is counted based on the record's modified time stamp, not the deleted time stamp. This means for change approval records it is the time when the pending delete request was approved and moved to the Recycle Bin, not when the delete request was made.
