# Full Text Search Functions

## Overview

Full Text Searching can provide robust search capabilities on columns that have a Full Text Index. You are able to use the predicates **CONTAINS** and **FREETEXT** for this purpose.

To perform a Full Text Search, [each column in your query will need a Full Text Index.](https://platform.docs.cinchy.com/guides-for-using-cinchy/builder-guides/creating-tables/indexing-and-partitioning#2.-full-text-indexing)

{% hint style="warning" %}
Full Text Searching is currently **only available** for those using SQL Server 2016 or up.
{% endhint %}

## CONTAINS

CONTAINS is a predicate used in the WHERE clause of a CQL SELECT statement to perform full-text search on full-text indexed columns containing character-based data types.

CONTAINS can search for:

* A word or phrase.
* The prefix of a word or phrase.
* A word near another word.
* A word inflectionally generated from another (for example, the word drive is the inflectional stem of drives, drove, driving, and driven).

#### General Syntax

Using this general syntax without any modifiers, your results will return the specific rows that match the exact word or phrase specified between the single quotes in line 2.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = ''  
SELECT [column]   
FROM [domain].[table]
WHERE CONTAINS([Title], @SearchWord)
```

#### Example Syntax

In the following example, we want to return all rows from the **Cinchy Wiki Documentation** table (in the **Product** domain) that match the exact word **overview** in their **Title** column.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'overview'  
SELECT [Title]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([Title], @SearchWord)
```

#### Example Results

<figure><img src="../../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

## CONTAINS Modifiers

There are many modifiers you can add to your CONTAINS full text search query to receive more specific results.

### Prefix Term

Using a prefix term modifier will return results with your specified prefix. For example, the prefix **'over'** could return overview, overture, overruled, etc.

To use a prefix term, wrap your term in single, then double quotes and put an asterix at the end: _'"example\*"'_

#### General Syntax

```sql
DECLARE @SearchWord VARCHAR(30)
SET @SearchWord = '"*"'
SELECT [column]   
FROM [domain].[table]
WHERE CONTAINS([column], '"*"')
```

#### Example Syntax

In this example, we have modified our search so that we receive all results where the any word in the **Title** column contains the prefix **'over'.**

```sql
DECLARE @SearchWord VARCHAR(30)
SET @SearchWord = '"over*"'
SELECT [Title]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([Title], '"over*"')
```

#### Example results

<figure><img src="../../../.gitbook/assets/image (520).png" alt=""><figcaption></figcaption></figure>

### Generation Term

A generation term modifier searches for all the different tenses and conjugations of a verb or both the singular and plural forms of a noun (an **inflectional** search) or for synonymous forms of a specific word (a **thesaurus** search).

{% hint style="warning" %}
To return the synonymous forms, you must have a Thesaurus file configured. [Find more information here.](https://docs.microsoft.com/en-us/sql/relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search?view=sql-server-ver16)
{% endhint %}

#### General Syntax (Inflectional)

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = ''  
SELECT [Column]   
FROM [domain].[table]
WHERE CONTAINS([column], 'FORMSOF(INFLECTIONAL, "")') 
```

#### Example Syntax (Inflectional)

In this example, our query will return all results with different tense and conjugations of our search word, **'gave'.**

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'gave'  
SELECT [Summary]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([Summary], 'FORMSOF(INFLECTIONAL, "gave")') 
```

#### Example Results (Inflectional)

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

#### General Syntax (Thesaurus)

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = ''  
SELECT [Column]   
FROM [domain].[table]
WHERE CONTAINS([column], 'FORMSOF(THESAURUS, "")') 
```

#### Example Syntax (Thesaurus)

In this example, we want to return all results where the data in the Summary column matches the meaning of our search term.

_I.E. "install" might return results with "deploy", "configure", "set", etc._

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'install'  
SELECT [Summary]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([Summary], 'FORMSOF(THESAURUS, "install")') 
```

### Proximity Term

A _proximity term_ will return words or phrases that are near to each other. You can also specify the maximum number of non-search terms that separate the first and last search terms.

{% hint style="info" %}
**Note** that proximity terms in Cinchy don't adhere to the specified order written in the query. You will receive results of both "term 1+term 2" as well as "term 2+term 1".
{% endhint %}

#### General Syntax

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'NEAR((term1, term2), #)'
SELECT [column]   
FROM [domain].[table]
WHERE CONTAINS([column], 'NEAR((term1, term2), #)')
```

#### Example Syntax

This example returns all results where the terms **"first" and "page"** appear within **two** words of each other.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'NEAR((first, page), 2)'
SELECT [summary]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([summary], 'NEAR((first, page), 2)')
```

#### Example Results

<figure><img src="../../../.gitbook/assets/image (526).png" alt=""><figcaption></figcaption></figure>

### Boolean Operators

With CONTAINS, you can use **AND, OR, and AND NOT** to specify your results.

#### General Syntax

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'term operator term'  
SELECT [column]   
FROM [domain].[table]
WHERE CONTAINS([column], @SearchWord)
```

#### Example 1 (AND) Syntax

In this example, we want search results from the Summary column of the Cinchy Wiki Documentation page that contain both the word 'user' **and** the word 'page'.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'user AND page'  
SELECT [summary]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([summary], @SearchWord)
```

#### Example 1 (AND) Result

#### Example 2 (OR) Syntax

In this example, we want search results from the Summary column of the Cinchy Wiki Documentation page that contain either the word 'user' **or** the word 'page'.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'user OR page'  
SELECT [summary]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([summary], @SearchWord)
```

#### Example 2 (OR) Results

<figure><img src="../../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

#### Example 3 (AND NOT) Syntax

In this example, we want search results from the Summary column of the Cinchy Wiki Documentation page that contain the word 'user' **and not** the word 'page'.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'user AND NOT page'  
SELECT [summary]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([summary], @SearchWord)
```

#### Example 3 (AND NOT) Results

<figure><img src="../../../.gitbook/assets/image (516).png" alt=""><figcaption></figcaption></figure>

### NEAR

You can use this modifier to return results with terms that appear near each other (i.e. within the same data cell).

#### General Syntax

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'term1 NEAR term2'  
SELECT [column]   
FROM [domain].[table]
WHERE CONTAINS([title], @SearchWord)
```

#### Example Syntax

In this example, we want to return all results where the data in the **Title** column has the term **"data"** appearing near the term **"CinchyDXD".**

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord = 'Data NEAR CinchyDXD'  
SELECT [title]   
FROM [Product].[Cinchy Wiki Documentation]
WHERE CONTAINS([title], @SearchWord)
```

#### Example Results

<figure><img src="../../../.gitbook/assets/image (743).png" alt=""><figcaption></figcaption></figure>

## FREETEXT

The FREETEXT command provides the ability to search for a matched term based on the meaning of the terms as opposed to the exact character string.

At a high level, this commands finds matches based on separating the string into individual words, determining inflectional versions of the word and using a thesaurus to expand or replace the term to improve the search.

The difference between FREETEXT and CONTAINS is that it searches for the values that match the **meaning of a phrase** and not just exact words. It is therefore a better option if you are **searching phrases**, in lieu of individual words.

{% hint style="warning" %}
To use FREETEXT, you must have a Thesaurus file configured. [Find more information here.](https://docs.microsoft.com/en-us/sql/relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search?view=sql-server-ver16)
{% endhint %}

#### General Syntax

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord =''  
SELECT [column]
FROM [domain].[table]
WHERE FREETEXT([summary], @SearchWord);
```

#### Example Syntax

In this example, we want to return all results where the data in the Summary column matches the meaning of our search phrase.

I.E. "installation guide" might return results with "deployment instructions", "set up guide", etc.

```sql
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='installation guide'  
SELECT [summary]
FROM [Product].[Cinchy Wiki Documentation]
WHERE FREETEXT([summary], @SearchWord);
```
