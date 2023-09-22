# Cinchy functions

## Overview

The Cinchy functions covered in this section are:

- [​resolveLink()​](cinchy-functions.md#resolvelink)
- ​[currentUserID()​](cinchy-functions.md#currentuserid)
- ​[currentUsersGroups()​](cinchy-functions.md#currentusersgroups)
- ​[executeSavedQuery()​](cinchy-functions.md#executesavedquery)
- [​GetLastModifiedBy()​](cinchy-functions.md#getlastmodifiedby)
- ​[draft(\[Column Name\])​](cinchy-functions.md#draft-column-name)

## resolveLink() <a href="#resolvelink" id="resolvelink"></a>

Use the **resolveLink** function to insert or update values for link columns. Use it with values in the target table or the Cinchy Id.

{% hint style="danger" %}
Using **resolveLink** against non-unique data values may not return the same data response each time; in that scenario we recommend that you resolve using the CinchyID instead.
{% endhint %}

### Syntax

```sql
ResolveLink('value(s)','column in target table')

'CinchyId,0,CinchyId2,0'
```

#### Example

```sql
INSERT INTO [Cinchy].[Domain] (Name, Colour)
VALUES ('Test', ResolveLink('Purple','Name'))

INSERT INTO [Corporate].[Projects] (Name, Teams)
VALUES ('CQL Documentation', ResolveLink('Customer Success, Product, Development','Name'))


UPDATE [Product].[Backlog]
SET [Watchers] = '152,0,187,0,347,0'
WHERE [Summary] = 'You can use resolveLink to update link values in DML.'

```

## currentUserID() <a href="#currentuserid" id="currentuserid"></a>

The **currentUserID()** function returns the currently logged in user's Cinchy Id. This is useful for setting up views as well as permissions.

### Syntax

```sql
[Linked_Column].[Cinchy Id] = currentUserID()
```

#### Example

You can use the below example in a view filter so that only the currently logged in user's tasks will appear.

```sql
[Assignee].[Cinchy Id] = currentUserID()
```

## currentUsersGroups() <a href="#currentusersgroups" id="currentusersgroups"></a>

The **currentUsersGroups()** function returns a list of the Cinchy Ids of groups that the current user belongs to, including any parent groups. For example, if a user is in the `Cinchy Product` group and the `Cinchy Product` group is under `Cinchy Employees`, then both will be returned.

### Syntax

```sql
[Cinchy Id] IN (currentUsersGroups())
```

#### Example

```sql
SELECT [Name] FROM [Cinchy].[Groups]
WHERE [Cinchy Id] IN (currentUsersGroups())
```

## executeSavedQuery() <a href="#executesavedquery" id="executesavedquery"></a>

The **executeSavedQuery()** function returns a scalar or list of scalar values from the saved query specified as the parameters of the function. This function has two optional parameters: **CacheTimeout** and **RecordLimitForReturn.**

### Syntax

```sql
[Column] = executeSavedQuery('Domain','Saved Query Name')
```

#### Example (Single Value)

```sql
[Department] = executeSavedQuery('HR','Get Department')

-- Equivalent to below if you create a Saved Query in the HR domain called
-- Get Department with the following subquery.

[Department] = (SELECT [Team]
                FROM [HR].[Employees]
                WHERE [Cinchy User Account].[Cinchy Id] = currentUserID())
```

#### Example (Multi Value)

```sql
[Assignee] IN (executeSavedQuery('HR','Get My Direct Reports')
```

### Optional parameters

#### Timeout

To add a **CacheTimeout**, enter the number of seconds as a 3rd parameter to the function.

```sql
[Department] = executeSavedQuery('HR','Get Department',30)
```

#### Record Limit

The **RecordLimitForReturn** parameter limits the amount of records returned. 

For example, if the query returns 10 records, but you set the parameter to 5, then you will get the first five records back.

```sql
[Department] = executeSavedQuery('HR','Get Department',30, 5)
```

## GetLastModifiedBy() <a href="#getlastmodifiedby" id="getlastmodifiedby"></a>

The **GetLastModifiedBy(\[Column])** function will return the CinchyID of the user who last modified the specified column. It's currently only supported in SELECT statements.​

| Value                | Definiton                                                                                  |
| -------------------- | ------------------------------------------------------------------------------------------ |
| Function Name        | GetLastModifiedBy                                                                          |
| Function Description | This function will return the CinchyID of the user who last modified the specified column. |
| Function Type        | Scalar                                                                                     |
| Return Type          | Numeric. It returns the CinchyID                                                           |

#### Syntax

```sql
GetLastModifiedBy([Column])
```

#### Example

This example will **return the CinchyID** of the user who last modified the **Name** column in the **Employees** table.

```sql
SELECT getLastModifiedBy([Name])
FROM [HR].[Employees]
```

## draft(\[Column Name]) <a href="#draft-column-name" id="draft-column-name"></a>

This function queries for draft values on tables where Change Approval is enabled.

{% hint style="warning" %}
When querying for draft data, the query result type needs to be set to **"Query Results (including Draft Data)"**
{% endhint %}

#### Syntax

```sql
SELECT draft([Column])
FROM [domain].[table]
```

#### Example

In this example, we want to query all data in the Employees table, including the data that's pending a change request _(Image 1)._

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MBtHkNqYteSDPDzpqqZ%2Fuploads%2FEX4mHbdyTav7cvRbiokn%2FRecords%20with%20Pending%20Changes%20in%20the%20table.jpg?alt=media&#x26;token=f4ad0030-bf67-4cf8-aebf-8a368618a4be" alt=""><figcaption><p>Image 1: Querying a table with draft data</p></figcaption></figure>

To return results that include the draft changes in the **First Name** column, set your query results to **Include Draft Data**, and use the following syntax _(Image 2)_:

```sql
SELECT [Employee ID], draft([First Name]), [Full Name], [Salary], [Date Hired]
FROM [HR].[Employees]
```

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MBtHkNqYteSDPDzpqqZ%2Fuploads%2F6nG4R3USwbDd0pwb1mBS%2Fimage.png?alt=media&#x26;token=6f911eb0-a4b8-46bb-ad40-314f16bd11fd" alt=""><figcaption><p>Image 2: Query results that include draft data</p></figcaption></figure>
