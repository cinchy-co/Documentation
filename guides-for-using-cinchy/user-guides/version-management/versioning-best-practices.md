---
description: This page outlines some best practices for versioning.
---

# Versioning Best Practices

## Overview

This page details a few best practices concerning **version history** in Cinchy. These recommendations are important because they can help:

* Minimize your database bloat/size.
* Make it easier to parse through version history when there aren't hundreds of redundant records.

## 2. Best Practices

* When doing any type of **update statement**, it is best practice to include an opposite **“where”** clause to avoid creating unnecessary history for unchanged values.
  * For example, if your update was “set name to Marc”, you would include a “where name does not already equal Marc”. Doing so prevents a redundant update in your version history.

```sql
UPDATE
	[Contacts].[Employees] 
SET [Name] = Marc
WHERE [Name] != Marc
```

* **When writing an update statement,** run it more than once. If it results in an update each time, return to your query and troubleshoot.
  * This is especially relevant anywhere the statement can be run repeatedly, such as in APIs or Post Sync Scripts.
* **In data syncs,** ensure that your data types are matched properly.&#x20;
  * For example, if the source is text and the target is data, even if the values are the same, it will update and create unnecessary version history.
* **When performing a data sync**, run it more than once. If it creates an update each time, return to your configuration and troubleshoot.
