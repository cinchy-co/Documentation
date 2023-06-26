---
description: This page details the role of an Admin of the Cinchy Platform
---

# Administrator Guide

## 1. What is an Administrator?

The **“Admins”** of the Cinchy platform are users who belong to the **"Cinchy Administrators"** User Group.

## 2. What Can Admins Do?

Cinchy Admins can either be [builders](builder-guides/) or non-builders (i.e. [end-users](user-guides/)).

{% hint style="warning" %}
Setting a user as an Admin does not supersede/change their role as an end-user vs builder.
{% endhint %}

### 2.1 A Builder Admin can:

* Modify **all** table data (including system tables), **all** schema, and **all** data controls
  * This includes setting up and configuring users, assigning them to groups, and assigning which users have builder access
* View **all** tables (including system tables) and queries in the platform

### 2.2 A Non-Builder Admin can:

* View **all** tables (including system tables) and queries in the platform
* Modify data controls for tables

## 3. How to View/Manage Admins

To view and manage who has administrator access, you will use the [system table](builder-guides/creating-tables/system-tables.md) **"Groups".**

{% hint style="info" %}
Note that you must have the correct entitlements set in order to view or access the **"Groups"** table. If you are part of the **"Administrators"** group already, then you can view all system tables by default.
{% endhint %}

### 3.1 As a Non-Builder

A non-builder is able to view which users are part of the **"Administrators"** group via the **"Groups"** system table; either in the data browser or by using a saved query.

### 3.2 As a Builder

A builder is able to view which users are part of the **"Administrators"** group via the **"Groups"** system table; either in the data browser or by using a saved/ad-hoc query.

**If you are an admin,** you can also use the **"Groups"** table to add or remove users from the **"Administrators"** Group.

## 4. How to Manage Builder Licenses

A Cinchy platform admin has the ability to manage and transfer their builder licenses between users through the following process:

1. As an Administrator, navigate to Users table _(Image 1)._

<figure><img src="../.gitbook/assets/image (508).png" alt=""><figcaption><p>Image 1: The Users Table</p></figcaption></figure>

2. Navigate to the row pertaining to the user that you want to manage.
3. To remove Builder access, uncheck the following columns _(Image 2)_:
   1. **Can Design Tables**
   2. **Can Design Queries**
4. To add Builder access, check the following columns _(Image 2)_:
   1. **Can Design Tables**
   2. **Can Design Queries**

<figure><img src="../.gitbook/assets/image (516).png" alt=""><figcaption><p>Image 2: Managing Access</p></figcaption></figure>

{% hint style="warning" %}
Ensure that you are not surpassing your Builder license limits when managing access.
{% endhint %}
