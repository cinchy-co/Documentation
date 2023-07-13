---
description: This page provides some examples of Cinchy Query Language in action.
---

# CQL Examples

This section breaks down some example queries for you to get a feel for the power of Cinchy Query Language (CQL) and how to use it. These simple examples provide a point of reference for you to create more complex queries.

For more information on saved queries, see our [Builder Guide](https://cinchy.gitbook.io/cinchy-v5.0.0/guides-for-using-cinchy/builder-guides/saved-queries).

## Create a list of specific users <a href="#1.1-creating-a-list-of-specific-users" id="1.1-creating-a-list-of-specific-users"></a>

This query is useful for pulling a list of specific user information from a table. This query will return a list with **Names** (which will be returned under the **'Full Name'** label), **Emails**, and **Companies** of the users listed in the **\[Contacts].\[People]** table. This example only includes entries where the **\[Name]** isn't null, and where the **\[Tags].\[Name]** column is set to _'Analyst'_

### Syntax

```sql
SELECT [Column1] as 'Alias',[Column2], [Column3]
FROM [Domain].[Table]
WHERE [Deleted] IS NULL
AND [Column1] IS NOT NULL
AND [Column4].[Value1]= 'Value2'
ORDER BY [Column1]
```

### Example

```sql
SELECT [Name] as 'Full Name',[Email], [Company]
FROM [Contacts].[People]
WHERE [Deleted] IS NULL
AND [Name] IS NOT NULL
AND [Tags].[Name]= 'Analyst'
ORDER BY [Name]
```

## Find all tables in a specific domain <a href="#1.2-finding-all-tables-in-a-specific-domain" id="1.2-finding-all-tables-in-a-specific-domain"></a>

In this example we want to find all **System Tables** in our instance. System Tables come pre-packaged with Cinchy and live under the **'Cinchy'** domain. You can query for a list of them using the below CQL, where the query pulls the **'Name'** column from the **\[Tables] table**, in the **\[Cinchy] Domain**. There's also a filter to only return results where the table **\[Domain]** is tagged as _'Cinchy**'**._

### Syntax

```sql
SELECT [Column1]
FROM [Domain].[Table]
WHERE [Deleted] IS NULL
AND [Domain] = 'Value1'
```

### Example

```sql
SELECT [Name]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NULL
AND [Domain] = 'Cinchy'
```

## Find system columns attached to a table <a href="#1.3-finding-system-columns-attached-to-a-table" id="1.3-finding-system-columns-attached-to-a-table"></a>

In this example, we want to generate a list of the system columns attached to the **\[Policies]** table in the **\[Compliance]** domain. We do so by putting the **\*** symbol in the **SELECT** statement. There's also an optional **ORDER BY** clause, to order results based on the input values.

### Syntax

```sql
SELECT *
FROM [Domain].[Table]
WHERE [Deleted] IS NULL
ORDER BY [Column1], [Column2], [Column3], [Column4]
```

### Example

```sql
SELECT *
FROM [Compliance].[Policies]
WHERE [Deleted] IS NULL
ORDER BY [Cinchy Id], [Title], [Version], [Draft Version]
```

## Create a custom View for a logged-in user <a href="#1.4-creating-a-view-that-is-tailored-to-the-user-who-is-logged-in" id="1.4-creating-a-view-that-is-tailored-to-the-user-who-is-logged-in"></a>

This example has a table with a view for **My Open Tasks**. This view uses **currentuserID=()**, which only shows the tasks assigned to the user currently logged in.

### Example

```sql
[Assignee].[Person].[Cinchy User Account].[Cinchy Id] = currentuserID()
```

## Finding the owner of a table <a href="#1.5-finding-the-owner-of-a-table" id="1.5-finding-the-owner-of-a-table"></a>

The below query finds the creator of any specific table, based on its `tablecinchyid`. The query pulls the **Table Name** and the **Created By** user information from the **\[Cinchy].\[Tables]** table. By using **\[Cinchy Id] = @tablecinchyid**, a search box will appear when you run this query, where you can insert the **Table ID** number of the table you are curious about.

### Example

```sql
SELECT [Name], [Created By]
FROM [Cinchy].[Tables]
WHERE [Deleted] is null and [Cinchy Id] = @tablecinchyid
```

## Find all tables created by a specific user <a href="#1.6-finding-all-tables-created-by-a-specific-user" id="1.6-finding-all-tables-created-by-a-specific-user"></a>

The example below uses a query to return a list of all tables created by a specific user. It pulls the **Full Name of the table** and the **name of the creator** from the **\[Cinchy].\[Tables]** table. By using **\[Created By].\[Name] = @author**, running the query will generate a search box where you can write in the full name of the user you are searching for information on.

### Example

```sql
SELECT [Full Name], [Created By].[Name]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NULL AND [Created By].[Name] = @author
```

## If/Else statements <a href="#1.7-using-if-else-statements" id="1.7-using-if-else-statements"></a>

You can use **If/Else** statements in CQL to specify conditions. The example below uses a query to vote for user awards, and uses **IF** statements to look for the **currentuserid()** to determine the results of the query.

### Example

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

The second condition checks whether the current user is the same as the user who created the nomination. In this case, the query will return a message stating _"Hey! No voting for your own nomination!"_

```sql
ELSE
IF( @CREATEDBY_USER = currentuserid())
Begin
SELECT 'Hey! No voting for your own nomination!'
 End
```

If neither of the two **IF** statements apply, the final **ELSE** triggers to capture the vote (using the INSERT INTO statement), and a message is returned stating _"Your vote has been recorded. Great work!"_

```sql
 ELSE
Begin
   		INSERT INTO [Employee Success].[BASICs In Action Votes] ([Nomination], [Vote]) VALUES (ResolveLink(@NOMINATION_ID, 'Cinchy Id'), @VOTE)
		SELECT 'Your vote has been recorded. Great work!'
End
```

## Find all Deleted Tables <a href="#1.8-finding-all-deleted-tables" id="1.8-finding-all-deleted-tables"></a>

This query returns the **Name, Domain, and timestamp** of deletion for all tables within **\[Cinchy].\[Tables]** where `[Deleted] IS NOT NULL`. We've added an option **WHERE** clause to ignore any data from the **'Sandbox'** Domain.

### Example

```sql
SELECT [Name], [Domain], [Deleted]
FROM [Cinchy].[Tables]
WHERE [Deleted] IS NOT NULL
	AND [Domain] != 'Sandbox'
```
