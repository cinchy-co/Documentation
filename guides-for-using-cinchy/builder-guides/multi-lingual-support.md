---
description: This page outlines multi-lingual support information.
---

# Multi-Lingual Support

## Table of Contents <a href="#translate-api" id="translate-api"></a>

| Table of Contents                                                      |
| ---------------------------------------------------------------------- |
| [#translate-api-1](multi-lingual-support.md#translate-api-1 "mention") |
| [#system-tables](multi-lingual-support.md#system-tables "mention")     |

## 1. Translate API <a href="#translate-api" id="translate-api"></a>

{% swagger baseUrl="<Cinchy-URL>" path="/API/Translate" method="post" summary="Translation API" %}
{% swagger-description %}
Pass in a list of literal GUIDs, along with a language and region. If translations are found in that language, they will be returned.
{% endswagger-description %}

{% swagger-parameter in="body" name="debug" type="boolean" %}
Defaults to false if not specified. Debug true will explain why that string was returned as the translation.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="region" type="string" %}
Subtag from the Regions table. User's preferences will be used if not specified.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="guids" type="array" %}
Array of strings. Guids from the Literals table.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="language" type="string" %}
Subtag from the Languages table. User's preferences will be used if not specified.
{% endswagger-parameter %}

{% swagger-response status="200" description="This shows a response with debug = true. Without debug = true, only the translation would return within the object." %}
```javascript
{
  "data": {
    "button.create": {
      "translation": "Create",
      "language": "en",
      "region": "US",
      "defaultText": false
    },
    "button.cancel": {
      "translation": "Cancel",
      "language": null,
      "region": null,
      "defaultText": true
    },
    "button.favorite": {
      "translation": "Favourite",
      "language": "en",
      "region": "CA",
      "defaultText": false
    },
    "button.delete": {
      "translation": "button.delete",
      "language": null,
      "region": null,
      "defaultText": false
    }
  }
}

```
{% endswagger-response %}
{% endswagger %}

### Logic <a href="#logic" id="logic"></a>

* If the translation exists in the language and region specified, it will be returned.
* If the translation exists in the language but not the specified region, it will still be translated and returned.
* If the GUID exists but it is not available in the specified language, the default text in the Literals table will return.
* If the GUID does not exist or you do not have permission to it, it will return the GUID back as the translation.

## 2. System Tables <a href="#system-tables" id="system-tables"></a>

There are 3 tables in Cinchy to provide language support.&#x20;

1. \[Cinchy].\[Literal Groups]
2. \[Cinchy].\[Literals]
3. \[Cinchy].\[Literal Translations].

### 2.1 Literal Groups <a href="#literal-groups" id="literal-groups"></a>

This table can optionally be used to group the translations. The default Cinchy strings belong to the Cinchy literal group. We recommend you create one literal group per applet or UI so you can retrieve the full list of GUIDs required for that page/applet easily.

### 2.2 Literals <a href="#literals" id="literals"></a>

This table defines all the strings that you want to translate.

#### Default Text <a href="#default-text" id="default-text"></a>

String that displays if no translation is found for the language specified.

#### GUID <a href="#guid" id="guid"></a>

GUID used to refer to the literal. A UUID will be generated by default, but can be overrode using the **Guid Override** field to something more human-readable.

#### Literal Group <a href="#literal-group" id="literal-group"></a>

As mentioned above, this can be used to group your strings so they can be easily retrieved. Note that this is a multi-select so you can use a single literal for multiple applets (including using the default Cinchy literals and translations for custom applets).

### 2.3 Literal Translations <a href="#literal-translations" id="literal-translations"></a>

This is the table where the translations are stored.

#### Translated Text <a href="#translated-text" id="translated-text"></a>

This is the translated string that is returned.

#### Literal <a href="#literal" id="literal"></a>

This is the literal the translation is for.

#### Language and Region <a href="#language-and-region" id="language-and-region"></a>

A language must be specified for a translation. Region can also be optionally specified for region specific words (ex. color vs colour).​