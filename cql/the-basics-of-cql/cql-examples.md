---
description: This page provides some examples of Cinchy Query Language in action.
---

# CQL Examples

## &#x20;1. CQL Examples

The following section breaks down some example queries for you to get a feel for the power of CQL and how to use it. These are just simple examples, but will provide you with a jumping off point for creating your own, more complex queries.

For more information on saved queries, see our Builder Guide, [here.](https://cinchy.gitbook.io/cinchy-v5.0.0/guides-for-using-cinchy/builder-guides/saved-queries)

#### **1.1 Creating a list of specific users** <a href="#1.1-creating-a-list-of-specific-users" id="1.1-creating-a-list-of-specific-users"></a>

This query is useful for pulling a list of specific user information from a table. This query will return a list with **Names** (which will be returned under the **'Full Name'** label), **Emails**, and **Companies** of the users listed in the **\[Contacts].\[People]** table. We have stipulated to only include entries where the **\[Name]** is not null, and where the **\[Tags].\[Name]** column is set to _'Analyst'_

#### Syntax

```sql
SELECT [Column1] as 'Alias',[Column2], [Column3]
FROM [Domain].[Table]
WHERE [Deleted] IS NULL
AND [Column1] IS NOT NULL
AND [Column4].[Value1]= 'Value2'
ORDER BY [Column1]
```

#### Example

```sql
SELECT [Name] as 'Full Name',[Email], [Company]
FROM [Contacts].[People]
WHERE [Deleted] IS NULL
AND [Name] IS NOT NULL
AND [Tags].[Name]= 'Analyst'
ORDER BY [Name]
```

#### 1.2 Finding all tables in a specific domain <a href="#1.2-finding-all-tables-in-a-specific-domain" id="1.2-finding-all-tables-in-a-specific-domain"></a>

In this example we want to find all **System Tables** in our instance. System Tables come pre-packaged with Cinchy and live under the **'Cinchy'** domain. You can query for a list of them using the below CQL, where we are pulling the **'Name'** column from the **\[Tables] table**, in the **\[Cinchy] Domain**. We have added a filter to only return results where the table **\[Domain]** is tagged as _'Cinchy**'**._

#### Syntax

```sql
SELECT [Column1]
FROM [Domain].[Table]
WHERE [Deleted] IS NULL
AND [Domain] = 'Value1'
```

#### Example

```sql
SELECT [Name]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NULL
AND [Domain] = 'Cinchy'
```

#### 1.3 Finding system columns attached to a table <a href="#1.3-finding-system-columns-attached-to-a-table" id="1.3-finding-system-columns-attached-to-a-table"></a>

In this example, we want to generate a list of the system columns attached to the **\[Policies]** table in the **\[Compliance]** domain. We do so by putting the **\*** symbol in the **SELECT** statement. We have also added an option **ORDER BY** clause, to order our results based on the input values.

#### Syntax

```sql
SELECT * 
FROM [Domain].[Table] 
WHERE [Deleted] IS NULL
ORDER BY [Column1], [Column2], [Column3], [Column4]
```

#### Example

```sql
SELECT * 
FROM [Compliance].[Policies] 
WHERE [Deleted] IS NULL
ORDER BY [Cinchy Id], [Title], [Version], [Draft Version] 
```

#### 1.4 Creating a View that is tailored to the user who is logged in <a href="#1.4-creating-a-view-that-is-tailored-to-the-user-who-is-logged-in" id="1.4-creating-a-view-that-is-tailored-to-the-user-who-is-logged-in"></a>

In this example, we have a table with a view for _"My Open Tasks"._ We want this view to only show tasks that are assigned to the currently logged in user, so we would use **currentuserID=().**

```sql
[Assignee].[Person].[Cinchy User Account].[Cinchy Id] = currentuserID()
```

#### 1.5 Finding the owner of a table <a href="#1.5-finding-the-owner-of-a-table" id="1.5-finding-the-owner-of-a-table"></a>

The below query is used to find the creator of any specific table, based on its **tablecinchyid**. We are pulling the **Table Name** and the **Created By** user information from the **\[Cinchy].\[Tables]** table. By using **\[Cinchy Id] = @tablecinchyid**, a search box will appear when we run this query, where you can insert the **Table ID** number of the table you are curious about.

```sql
SELECT [Name], [Created By]
FROM [Cinchy].[Tables]
WHERE [Deleted] is null and [Cinchy Id] = @tablecinchyid
```

#### 1.6 Finding all tables created by a specific user <a href="#1.6-finding-all-tables-created-by-a-specific-user" id="1.6-finding-all-tables-created-by-a-specific-user"></a>

In the below example, we are using a query to return a list of all tables created by a specific user. We are pulling the **Full Name of the table** and the **name of the creator** from the **\[Cinchy].\[Tables]** table. By using **\[Created By].\[Name] = @author**, running the query will generate a search box where you can write in the full name of the user you are searching for information on.

```sql
SELECT [Full Name], [Created By].[Name]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NULL AND [Created By].[Name] = @author
```

#### 1.7 Using If/Else statements <a href="#1.7-using-if-else-statements" id="1.7-using-if-else-statements"></a>

**If/Else** statements can be used in CQL to specify conditions. In the below example, we are using a query to vote for user awards, and our **IF** statements are looking for the **currentuserid()** to determine the results of the query.

```sql
IF( @NOMINEE_USER  = currentuserid())
Begin
SELECT 'Hey! No voting for yourself!'
 End
ELSE
IF( @CREATEDBY_USER = currentuserid())
Begin
SELECT 'Hey! No voting for your own nomination!'
 End
 ELSE
Begin
   		INSERT INTO [Employee Success].[BASICs In Action Votes] ([Nomination], [Vote]) VALUES (ResolveLink(@NOMINATION_ID, 'Cinchy Id'), @VOTE)
		SELECT 'Your vote has been recorded. Great work!'
End
```

If the current user is the same as the nominated user, the query will return a message stating _"Hey! No voting for yourself!"_

```sql
IF( @NOMINEE_USER  = currentuserid())
Begin
SELECT 'Hey! No voting for yourself!'
 End
```

Our second condition checks whether the current user is the same as the user who created the nomination. In this case, the query will return a message stating _"Hey! No voting for your own nomination!"_

```sql
ELSE
IF( @CREATEDBY_USER = currentuserid())
Begin
SELECT 'Hey! No voting for your own nomination!'
 End
```

If neither of the two **IF** statements apply, the final **ELSE** is triggered to capture the vote (using the INSERT INTO statement), and a message is returned stating _"Your vote has been recorded. Great work!"_

```sql
 ELSE
Begin
   		INSERT INTO [Employee Success].[BASICs In Action Votes] ([Nomination], [Vote]) VALUES (ResolveLink(@NOMINATION_ID, 'Cinchy Id'), @VOTE)
		SELECT 'Your vote has been recorded. Great work!'
End
```

#### 1.8 Finding all Deleted Tables <a href="#1.8-finding-all-deleted-tables" id="1.8-finding-all-deleted-tables"></a>

This query returns the **Name, Domain, and timestamp** of deletion for all tables within **\[Cinchy].\[Tables]** where _\[Deleted] IS NOT NULL._ We have added an option **WHERE** clause to ignore any data from the **'Sandbox'** Domain.

```sql
SELECT [Name], [Domain], [Deleted]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NOT NULL 
	AND [Domain] != 'Sandbox'
```
