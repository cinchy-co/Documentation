---
description: >-
  When you create a column within Cinchy, you can choose to create a link
  column. A link column allows you to establish inherent relationships with
  other tables.‌
---

# Linking Data

## Overview

Linking is done by the Cinchy ID, which is unique. When you create a link column, you select a column to link to. This is simply a decision on which field to show from the linked record. You should pick a unique field to link on to avoid confusion if possible.

Once a record is created, its Cinchy ID never changes. This means that modifying the row of data in the linked table won't change the relationship in your table to that row. This also means that if you didn't use a unique column, even though the UI looks the same, you are actually linking to different rows.‌

## Choose a linked column <a href="#choosing-linked-column" id="choosing-linked-column"></a>

‌In general, you should only use unique columns as the linked column. This needs to be balanced with readability in other tables.

For example, **Full Name** might not be unique to every employee, but it's more readable and understandable than Employee ID. In other cases, it makes sense to link via an ID and add a display column to show relevant information.‌

### Allow Linking <a href="#allow-linking" id="allow-linking"></a>

‌To help other builders follow best practices of only linking to unique (or close to unique, such as Full Name) columns, you should un-check the Allow Linking checkbox for non-unique columns so they won't be able to use it for linking.‌

### Allow Display in Linked View <a href="#allow-display-in-linked-view" id="allow-display-in-linked-view"></a>

‌If this option is unchecked, it prevents users from showing this column in another table.

For example, if you have an ID card # within an employees table, you may not want to display it to the rest of the company because it simply would not be relevant when they're linking to employees and want to see additional information (such as department, title, location). Arguably, a lot of these columns are also taken care of by access controls (since most people won't have access to view that column).‌

Deselecting this box should be done sparingly, as it doesn't impact the security of your data, only how convenient it's to see it.‌

### Display Columns <a href="#display-columns" id="display-columns"></a>

‌When you select a record to link to on the Manage Data screen, it can be useful to see additional information about the records to ensure that it's the record you want to link to _(Image 1)_. You can add additional display columns in the advanced options for link columns _(Image 2)._

<figure><img src="https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LIP3Xr2BuWD7FDjJFmZ%2F-LY8T-aeWVPm-D31gGaP%2F-LY8VJHpIHkrOhNGPZ7z%2Fimage.png?alt=media&#x26;token=5990a742-3bbf-482e-ad16-9229ff7c57f0" alt=""><figcaption><p>Image 1</p></figcaption></figure>

<figure><img src="https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LIP3Xr2BuWD7FDjJFmZ%2F-LY8T-aeWVPm-D31gGaP%2F-LY8Vc9MqmBtFEOtchwb%2Fimage.png?alt=media&#x26;token=dc00e333-1e31-494e-969d-ed1db6830335" alt=""><figcaption><p>Image 2</p></figcaption></figure>

When you type in the cell, all displayed columns will be searched through, not just the Linked Column _(Image 3)_. (Green doesn't have a B in it, but #00B050 does so the Green record shows up)

‌

<figure><img src="https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LIP3Xr2BuWD7FDjJFmZ%2F-LY8T-aeWVPm-D31gGaP%2F-LY8WZ2RWIbThd9DaIvz%2Fimage.png?alt=media&#x26;token=7b36c042-d94a-4b85-ad11-39c522751f19" alt=""><figcaption><p>Image 3</p></figcaption></figure>

### Link Filter <a href="#link-filter" id="link-filter"></a>

‌The link filter filters out records from the drop down list. This is useful for reducing the options to only the typical use case. Commonly used for filtering the drop down to a list of active users or other resources, while not preventing someone from entering missing records with inactive resources.‌

This is only a display filter; it doesn't prevent other values from being entered as long as they're valid records in the linked table.‌

## Relationships <a href="#relationships" id="relationships"></a>

‌You can define 1 to 1, 1 to many, and many to many relationships.‌

### 1:1 Relationship <a href="#1-1-relationship" id="1-1-relationship"></a>

‌Generally it's rare to link 1:1 relationships since they should usually be in the same table. For example, you would not have a separate table for Employee Phone Number and Employee Address, they would simply be two columns within the Employees table. However there are cases nonetheless where it makes sense, for example, a Keycard tracking table where each keycard has 1 assigned employee.‌

To enforce a 1:1 relationship within Cinchy, you set the unique constraint and leave it as single-select when creating a link column.‌

### 1:Many Relationship <a href="#1-many-relationship" id="1-many-relationship"></a>

‌A common relationship to have is a one to many relationship. For example, one customer can have multiple invoices.‌

To enforce a 1:many relationship within Cinchy, you want to create a link column in the table on the “many” side of the relationship (in the above example, in the invoices table) and leave the link column as single select.‌

### Many:Many Relationship <a href="#many-many-relationship" id="many-many-relationship"></a>

‌You can also have a many to many relationship. For example, you can have multiple customers, and multiple products. Each customer can order multiple products, and each product can be ordered by multiple customers. Another example is books and authors. An author can write multiple books, but a book can also have multiple authors. You can express many to many relationships in two ways.‌

For the use case of multiple customers and multiple products, you can use orders as an intermediary table to create indirect relationships between customers and products. Each order has one customer, and each order has multiple products in it. You can derive the relationship between customers and products through the orders table.‌

To create a many:many relationship through a different entity, you want to create a table for orders. Within orders, you want to create a single-select link to customers and a multi-select link to products.‌

For the use case of books and authors, it makes sense to create a multi-select link column in the Books table where multiple authors can be selected.‌

To create a multi-select link column in Cinchy, you select the **Multi-Select** option when you create a new link column.

