---
description: This page outlines the Cinchy GraphQL (beta) capabilities
---

# GraphQL (Beta)

{% hint style="info" %}
GraphQL was first introduced in Cinchy v5.1 as a read-only beta.

Write operations were introduced in v5.2
{% endhint %}

{% hint style="warning" %}
GraphQL functionalities in Cinchy are **currently in beta.**
{% endhint %}

## Introduction

[GraphQL is a query language for APIs ](https://graphql.org/)and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives you the power to ask for exactly what you need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

In a Cinchy context, the primary audience for the GraphQL betta **are developers writing apps on top of Cinchy.**It's a simple, efficient, new way to retrieve and manage data and build apps on via API.

GraphQL was introduced to Cinchy as a supplement to the[ /executecql endpoint](../../api-guide/api-overview/#2.5-api-executecql) and the [REST API ](../../api-guide/api-overview/)functions. With GraphQL, not only are your app building capabilities streamlined and more powerful, but there is no switching between CQL and code; all code changes can be done within the GraphQL user interface. It also adheres to your defined access controls, included anonymous level access, ensuring that your data remains secure.

<figure><img src="../../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>

## Use GraphQL

You can access GraphQL at the following URL: _\<baseurl>/graphqlplayground._

{% hint style="info" %}
**Note that you need at least Cinchy v5.1 for read-only, and v5.2 for write access.**
{% endhint %}

{% hint style="warning" %}
If there is an error in the responses window when logging in, log out and try again. This error sometimes occurs during a timeout.
{% endhint %}

GraphQL has a robust set of documentation, videos, and training resources that will help you realize its full capabilities. Here are some to get started:

- [Introduction to GraphQL](https://graphql.org/learn/)
- [Best Practices](https://graphql.org/learn/best-practices/)
- [GraphQL Frequently Asked Questions](https://graphql.org/faq/)
- [GraphQL Training Courses](https://graphql.org/community/users/#training-courses)

### Notes

- Using GraphQL on Cinchy means you still need to adhere to the Cinchy data structure. Just like with CQL, you have to adhere to the **\[Domain].\[Table]** structure when creating your queries. [See this example here](graphql-beta.md#example-1) for a use case.
- When writing your query, you can bring up an auto complete menu of fields by hitting the **ctrl+space** keys when your mouse in inside a **{**. The fields brought up will be related to the specific level you are on.
- You can use the **# symbol** for in-line commenting.

### Errors

The following section contains common errors and their solutions.

#### Server can't be reached

- **Problem:** If your Cinchy instance times out and prompts you to re-enter your credentials/SSO authentication, you might get the above error when trying to hit the GraphQL endpoint again.

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

- **Solution:** Log out and log back in to Cinchy. Hit the GraphQL endpoint again and refresh to remove the error.

## Query examples

The following are some query examples to help you get a feel for using GraphQL in a Cinchy context.

### Example 1: Return data from a single source

This example returns data from a single source, the **Videos** table in the **CinchyTV** domain. It specifically requests all data in the **Title** column of the table.

As a reminder, queries must adhere to the data structure in Cinchy. **You must first hit the domain (in this example: Cinchy TV) and then the table (in this example: Videos)**

#### Example query

```graphql
{
  cinchytv {
    videos {
      title
    }
  }
}
```

#### Example response

<figure><img src="../../.gitbook/assets/image (338).png" alt=""><figcaption></figcaption></figure>

### Example 2: Return data from multiple sources

This example returns data from two sources, the **Applets** table and the **Users** table, both in the same **Cinchy** domain.

It requests the following information:

- The **Name, Description, Application URL, Version, and Domain Name** of all the **applets** on the environment
- The **Email Address** of every user on the environment.

#### Example query

```graphql
{
  cinchy {
    applets {
      name
      description
      applicationUrl
      version
      domain {
        name
      }
    }
    users {
      emailAddress
    }
  }
}
```

#### Example results

<figure><img src="../../.gitbook/assets/image (357).png" alt=""><figcaption></figcaption></figure>

### Example 3: Use a data filter

This example uses a filter to return a specific subset of data. This example returns all results from the **Videos** table in the **CinchyTV** domain that contains the search term **"Apps"** in the **Title** field/column.

{% hint style="success" %}
Other filters you can use include: Equals, Not Contains, Starts With, Not Starts With, Ends With, Not Ends With, Empty, Not Empty, Is True, Is False, Before, After
{% endhint %}

The query will give us the **Title and Description** of all matching data rows.

#### Example query

```graphql
{
  cinchytv {
    videos(filter: { contains: { field: "title", value: "Apps" } }) {
      title
      description
    }
  }
}
```

#### Example Results

<figure><img src="../../.gitbook/assets/image (534).png" alt=""><figcaption></figcaption></figure>
