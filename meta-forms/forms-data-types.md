# Forms Data Types

## 1. Overview

There are various data types you can select from when setting up your Form Fields. This page will guide you through the different options available to use.

Not specifying a data format type will default to a text input, date field, or non-editable text based on your source column.

<figure><img src="../.gitbook/assets/image (388).png" alt=""><figcaption></figcaption></figure>

## 2. Data Types

This section breaks down the various data format types available to you when creating a Form.

### 2.1 CQL

Using the CQL data type allows you to use Cinchy Query Language expressions in your Form value.

You can review the basics of CQL here.

### 2.2 Javascript

Selecting Javascript as your data type will allow the form to render inputed Javascript text in your form.

### 2.3 JSON

Selecting JSON as your data type means that the Form will expect the value of this cell to be in JSON format.

### 2.4 XML

Selecting XML as your data type will allow the form to render inputed XML text in your form.

### 2.5 ImageUrl

Using the ImageUrl data format type allows you to embed images into your Form.

There are four different ImageUrl data types:

* **ImageUrl**
* **ImageUrl (small)**
* **ImageUrl (medium)**
* **ImageUrl (large)**



The below image shows what an example ImageUrl Form Field may look like:

<figure><img src="../.gitbook/assets/image (396).png" alt=""><figcaption></figcaption></figure>

To enable an ImageUrl data format type on your form:

* The table column you are referencing should have a valid image URL as its value OR The values of the table column you are referencing should be valid image URLs
* You must set up this column as a Form Field with the Data Type **ImageUrl/ImageUrl (small)/ImageUrl (medium)/ImageUrl (large).**
* The image will render under its associate Form Section in you Form

### 2.6 Rich Text

[Please see here ](meta-forms-builders-guides/rich-text-editing-in-forms.md)for details on Rich Text data formatting.

### 2.7 iFrames

Enable dynamic content in your Forms with iFrames — a new Data Format Type used to render content within the display of a record. Instead of taking the value of your table column and outputting it in a form field (text input, date field, non-editable text), we’re outputting an iFrame and setting its source attribute to be the value you have in your column

An inline frame (iframe) is a HTML element that loads another HTML page within the document. It essentially puts another webpage within the parent page. They are commonly used for advertisements, embedded videos, web analytics and interactive content.&#x20;

The below example shows an interactive AI chat box embedded into the Form.

<figure><img src="../.gitbook/assets/image (320).png" alt=""><figcaption></figcaption></figure>

To enable iFrames in your form:

1. The table column you are referencing should have a valid URL as its value OR The values of the table column you are referencing should be valid URLs
2. You must set up this column as a Form Field with the Data Type **iFrame/iFrame Sandboxed/iFrame Sandboxed Strict.**
3. The iFrame will render under its associate Form Section in you Form

There are three different iFrame data types, each with a unique level of allowable content.

* **iFrame:** Allows all content
* **iFrame Sandboxed:** Allows forms, scripts, modals, pop-ups, and same origin.
* **iFrame Sandboxed Strict:** Allows forms, scripts, modals, and popups.

{% hint style="success" %}
Tip: You can review the above [allowable attribute descriptions here.](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)
{% endhint %}

### 2.8 Link URL

[Please see here](broken-reference) for details on the Link URL data type.
