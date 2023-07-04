---
description: >-
  Data Control Entitlements allow you to set up permissions for who can view,
  edit, or approve data within a table. Note that this was formerly called
  "Design Controls"
---

# Data Entitlements

## Overview

Data Entitlements define who has access to do what on your Cinchy platform. These access controls are universally set at a cellular level, meaning that you can configure user access in the way that best supports your use case.

You can set entitlements such that specific users can view, edit, delete, insert, or all of the above for whichever data you want them to access.

**Cinchy supports user-based, role-based, and attribute-based access controls.**

### 1.1 User-Based Controls

User-based controls are entitlements given to specific users. **This is done via the Users column.**

Defining access based on a user means that even if the user changes their role, team, group, etc., they will still maintain their data entitlements.

### 1.2 Role-Based Controls

Role-based controls are entitlements given to set(s) or users based on their role in your environment. For example, you are able to define that only the Product team has access to insert records into a Product Roadmap table. Instead of configuring the entitlements user by user, which takes time and can lead to incorrect data when/if employees shift teams, you can configure it such that any user within the Product team automatically maintains the same level of control across the board.

In Cinchy, this is done via the **Groups column.**

### 1.3 Attribute-Based Controls

Attribute-based controls are entitlements given to a user(s) based on a defined set of "tags". This can include attributes such as their team, their role, their security clearance, their location, etc.

Defining entitlements based on attributes allows you to drill even deeper into the specificity of which users can do what on your tables.

In Cinchy, you can set up an infinite number of attributes based on your specific use case(s). This is done via [**Row Filters.**](data-entitlements.md#examples-for-row-filter)

For example, if you have an Employee table that contains salary information visible only to certain people, you can configure a Row Filter such that the logged in user MUST have at least one of the following attributes to be able to see it:

* The user to whom the salary belongs
* Their manager
* All VPs
* The CEO

You are able to add as many attributes into your Row Filter as needed. For example you could only allow a user with the following set of tags to view a row: _Located in Toronto, on the Marketing Team, and with a Security Clearance level of 2._

## 2. Changing Entitlements

1. When viewing a table, click on **Data Controls > Entitlements** from the left navigation menu _(Image 1)._

![Image 1: Step 1, Entitlements](<../../../../.gitbook/assets/image (257).png>)

2\. Currently both the table creator and anyone in the `Cinchy Administrators` group has access to perform any action on any objects. You can give granular entitlements at a Group or a  User level, for both viewing and editing access _(Image 2)._

![Image 2: Step 2, An example of Entitlements](<../../../../.gitbook/assets/image (545).png>)

3\. In the above scenario, John Smith is part of the **Developers group**. He is able to view all columns via the entitlement to the **Developers group**, and he is able to edit both the First Name and Last Name column through different entitlements.

## 3. Table Level Entitlements

There are certain entitlements in the **Data Controls menu** that apply to the entire table.

### 3.1 Marketplace

Approving this entitlement enables users to see and serarch for the table in the Marketplace/Homepage.

### 3.2 Bulk Export

Approving this entitlement enables users to export data from the table via the Manage Data screen _(Image 3)._

![Image 3: Step 2.2 Bulk Export](<../../../../.gitbook/assets/image (374).png>)

### 3.3 Direct Query

Approving this entitlement enables users to query the data from the table directly in the Query Builder _(Image 4)._

![Image 4: Step 2.3 Direct Queries](<../../../../.gitbook/assets/image (355).png>)

### 3.4 Design Table

Approving this entitlement enables users to alter the structure of the table.

{% hint style="warning" %}
This is a builder/administrative function and should not generally be granted to end users.
{% endhint %}

### 3.5 Design Controls

Approving this entitlement enables users to change the permissions on a table.

{% hint style="warning" %}
This is a builder/administrative function and should not generally be granted to end users.
{% endhint %}

## 4. Column Level Entitlements

There are certain entitlements in the **Data Controls menu** that apply only to columns.

### 4.1 View All Columns

Approving this entitlement enables users to view all columns within the table.

{% hint style="warning" %}
Note that this applies to any new columns that are added to the table after providing this permission as well.
{% endhint %}

### 4.2 View Specific Columns

This is a drop down where you can select the specific columns you want to grant view access to for users.

### 4.3 Edit All Columns

Approving this entitlement enables users to edit all columns within the table.

{% hint style="warning" %}
Note that this applies to any new columns that are added to the table after providing this permission as well.
{% endhint %}

{% hint style="info" %}
Giving a user edit permission will also give them view permission.
{% endhint %}

### 4.4 Edit Specific Columns

This is a drop down where you can select the specific columns you want to grant edit access to for users.

{% hint style="info" %}
Giving a user edit permission will also give them view permission.
{% endhint %}

### 4.5 Approve All Columns

Approving this entitlement enables users to approve all columns within the table. This also allows users to approve Create and Delete requests.

{% hint style="warning" %}
Note that this applies to any new columns that are added to the table after providing this permission as well.
{% endhint %}

Approve permissions only apply when Change Approvals are enabled.

{% hint style="info" %}
Giving a user approve permission will also give them view permission.
{% endhint %}

### 4.6 Approve Specific Columns

This is a drop down where you can select the specific columns you want to grant approve access to for users.

Approve permissions only apply when Change Approvals are enabled.

{% hint style="info" %}
Giving a user approve permission will also give them view permission.
{% endhint %}

### 4.7 Link Columns

Link columns require both permission to the column within this table, as well as the column in the link column itself.

## 5. Row Level Entitlements

These are entitlements that apply to specific rows. Used in conjunction with Column Level entitlements this allows for granular cell level entitlements.

### 5.1 Insert Row <a href="#insert-row" id="insert-row"></a>

Approving this entitlement enables users to create new rows in the table.

### 5.2 Delete Row <a href="#delete-row" id="delete-row"></a>

Approving this entitlement enables users to delete rows in the table.

### 5.3 Viewable & Editable Row Filter <a href="#viewable-and-editable-row-filter" id="viewable-and-editable-row-filter"></a>

This is a CQL fragment that applies a filter to which rows will be viewable or editable. Think of the column entitlements and the fragment as a SQL statement applied to the table.`SELECT {Edit Selected Columns} WHERE {Editable Row Filter}`

### 5.4 Examples for Row Filter <a href="#examples-for-row-filter" id="examples-for-row-filter"></a>

Most of these examples will be with the editable row filter so it is easy to see the underlying data for comparison. However this can be done for viewable row data as well.

#### Sample Data <a href="#sample-data" id="sample-data"></a>

_(Image 5)_

![Image 5: Sample Data](<../../../../.gitbook/assets/image (164).png>)

#### Simple Example

With the following entitlements _(Image 6):_

* Edit Specific Columns: Age
* Editable Row Filter: \[Age] > 30

![Image 6: Simple Example](<../../../../.gitbook/assets/image (14) (1).png>)

#### Example with Viewable Data

_(Image 7)_

* View Specific Columns: First Name, Last Name
* Viewable Row Filter: `[End Date] IS NULL OR [End Date] > GetDate()`

![Image 7: Example with Viewable Data](<../../../../.gitbook/assets/image (123).png>)

#### Layer On Another Entitlement

_(Image 8)_

* View Specific Columns: All
* Edit Specific Columns: First Name, Last Name, Age
* Viewable Row Filter: \[First Name] = 'John'
* Editable Row Filter: \[First Name] = 'John'

![Image 8: Layer on Another Entitlement](<../../../../.gitbook/assets/image (608).png>)

#### Example for Current User

_(Image 9)_

![Image 9: Example for current user](<../../../../.gitbook/assets/image (622).png>)

**For the All Users group:**

_(Image 10)_

* View All Columns: Check
* Edit Selected Columns: First Name, Last Name
* Editable Row Filter: `[User Account].[Cinchy Id] = CurrentUserId()`

![Image 10: For the All Users Group](<../../../../.gitbook/assets/image (763).png>)

To allow a user to edit certain fields of their own data, you will need an association from a user to the `[Cinchy].[Users]` table. You can then use the following function to allow edit for that user, where `[...]` is the chain of link columns to get to the Users table.

`[...].[Cinchy Id] = CurrentUserId()`
