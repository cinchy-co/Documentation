---
description: This page details the role of an Admin of the Cinchy Platform
---

# Administrator Guide

## Administrators

The **“Admins”** of the Cinchy platform are users who belong to the **"Cinchy Administrators"** User Group.

## Admin types

Cinchy Admins fall into two categories:

- [Builders](builder-guides/) 
- [End-users](user-guides/).

{% hint style="warning" %}
Setting a user as an Admin doesn't supersede/change their role as an end-user vs builder.
{% endhint %}

### Builder Admins

A Builder Admin can:

* Modify **all** table data (including system tables), **all** schema, and **all** data controls.
  * This includes setting up and configuring users, assigning them to groups, and assigning which users have builder access.
* View **all** tables (including system tables) and queries in the platform.

### End-User Admins

An End-User Admin can:

* View **all** tables (including system tables) and queries in the platform.
* Modify data controls for tables.

## Manage Admins

To view and manage who has administrator access, you will use the [System table](builder-guides/creating-tables/system-tables.md) **Groups**.

{% hint style="info" %}
Note that you must have the correct entitlements set to view or access the **"Groups"** table. If you are part of the **"Administrators"** group already, then you can view all system tables by default.
{% endhint %}

### Builder Admin management

A builder is able to view which users are part of the **"Administrators"** group via the **"Groups"** system table; either in the data browser or by using a saved/ad-hoc query.

**If you are an admin,** you can also use the **"Groups"** table to add or remove users from the **"Administrators"** Group.

### End-User Admin management

An End-User Admin is able to view which users are part of the **"Administrators"** group via the **"Groups"** system table; either in the data browser or by using a saved query.


