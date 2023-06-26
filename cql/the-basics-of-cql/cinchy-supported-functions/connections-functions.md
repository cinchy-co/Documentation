# Connections Functions

## 1. Overview <a href="#overview" id="overview"></a>

The set of functions listed in this page are for use in [Cinchy's Connections Experience](https://cli.docs.cinchy.com/) when configuring data syncs.

* [​JSON\_ESCAPE​](connections-functions.md#json\_escape)
* [​URL\_ESCAPE​](connections-functions.md#url\_escape)
* [STRING\_ESCAPE()](connections-functions.md#string\_escape)

## JSON\_ESCAPE <a href="#json_escape" id="json_escape"></a>

This function is used in Connections to escape parameter values and be safe to use inside a JSON document without breaking it

This function can be used in a REST API connection anywhere that allows parameters to be, such as the URL endpoint, the Request Body, or a Post-Sync Script.

#### Syntax

```sql
JSON_ESCAPE(@Parameter)
```

#### Arguments

| Argument  | Description                                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------------------------- |
| Parameter | The parameter value that you want to escape in order to be safe to use inside a JSON document without breaking it |

#### Example 1

The following example shows how you would use JSON\_ESCAPE in your REST API URL _(Image 1)._

In this example we have an API and want to add a value (@Parameter) that contains double quotes -- this could break the JSON structure, so we need to wrap the parameter with JSON\_ESCAPE().

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption><p>Image 1: Example 1</p></figcaption></figure>

#### Example 2

The following example shows how you would use JSON\_ESCAPE in your REST API Request Body _(Image 2)._

In this example we have an API and want to add a value (@Parameter) that contains double quotes -- this could break the JSON structure, so we need to wrap the parameter with JSON\_ESCAPE().

<figure><img src="../../../.gitbook/assets/image (249).png" alt=""><figcaption><p>Image 2: Example 2</p></figcaption></figure>

## URL\_ESCAPE

This function is used in Connections to escape parameter values and be safe to use inside a URL without breaking it

**This function can be used in a REST API connection anywhere that allows parameters to be, such as the URL endpoint, the Request Body, or a Post-Sync Script.**

#### Syntax

```sql
 URL_ESCAPE(@Parameter)
```

#### Arguments

| Argument  | Description                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------- |
| Parameter | The parameter value that you want to escape in order to be safe to use inside a URL without breaking it |

#### Example 1

The following example shows how you would use URL\_ESCAPE in your REST API URL _(Image 3)._

In this example we have an API and want to add a value (@Parameter) that contains the "&" symbol to the URL field. To properly read the URL, we need to wrap the parameter with URL\_ESCAPE(), **which will escape the & to be %26.**

<figure><img src="../../../.gitbook/assets/image (405).png" alt=""><figcaption><p>Image 3: Example 1</p></figcaption></figure>

## STRING\_ESCAPE()

The STRING\_ESCAPE() function escapes single quotes in data sync parameters by adding two single quotes. It can be used to wrap around parameters or column references respectively. This can be useful when you use it in a post sync script's CQL.

#### Syntax

```sql
STRING_ESCAPE(@yourparameter)
```

Or

```sql
STRING_ESCAPE(@COLUMN('yourcolumn'))
```

#### Example

```
STRING_ESCAPE(This is my data sync's test)

will become

This is my data sync''s test
```
