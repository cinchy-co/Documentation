---
description: This page describes Cinchy's rich text editor functionality.
---

# Rich Text Editing in Forms

## 1. Overview

{% hint style="success" %}
Rich Text Editing was introduced to the Cinchy platform in version 5.4.
{% endhint %}

Customize the appearance of your text in Forms with our new rich text editing capabilities. Enabling this on your text columns will give you access to formatting options for text previously unavailable in Forms such as:

* **Bold, Italic, Underlined text**
* **Checklists**
* **Headers**
* **Code Blocks**
* **etc.**

You have the option to output the data as HTML.

## 2. Enabling Rich Text Editing

You can enable rich text editing on your Form text columns through the Design Table tab of your form table _(Image 1)._

* **Enable text formatting:** Checking this enables the rich text editor in Forms.
* **Save text as HTML:** If you need the data saved from the rich text editor to be in HTML format, check this box.
* **Max Characters:** The rich text editor saves data in Cinchy as structured data (or HTML), which is much more verbose than your actual text. You will need to increase this number much higher than you expect (e.g. 10,000 characters).

<figure><img src="../../.gitbook/assets/image (247).png" alt=""><figcaption><p>Image 1: Enabling the Rich Text Editor</p></figcaption></figure>

## 3. Using the Rich Text Editor

Once you have enabled rich text editing on your text column for your form, you can utilize the capabilities by navigating to your form applet.

You will see all the formatting options available to you in the editor toolbar _(Image 2)._ A form field that does not have rich text enabled appears like the "Plaintext" section below.

<figure><img src="../../.gitbook/assets/Text Editor.png" alt=""><figcaption><p>Image 2: The editing toolbar</p></figcaption></figure>

{% hint style="info" %}
Note: If you have an understanding of HTML, you can edit the data directly in the table cell and it will automatically reflect in your form. Be mindful that if you write invalid HTML, you will potentially see unexpected results in the Rich Text Editor. You can always revert changes using the [collaboration log.](../../guides-for-using-cinchy/user-guides/data-management.md)
{% endhint %}

## 4. Examples

The below section outlines some examples of what you can do with the rich text editor.

### 4.1 Example A

The below example _(Image 3)_ shows using the following rich text capabilities:

* Bulleted Lists
* Bold
* Italics
* Underlined

<figure><img src="../../.gitbook/assets/Text Editor - Examples.png" alt=""><figcaption><p>Image 3: Example A Forms</p></figcaption></figure>

### 4.2 Example B

The below example _(Image 3)_ shows using the following rich text capabilities:

* Headings 1, 2, 3

<figure><img src="../../.gitbook/assets/Text Editor - Examples headings.png" alt=""><figcaption><p>Image 3: Example B Forms</p></figcaption></figure>

### 4.3 Example C

The below example _(Image 4)_ shows using the following rich text capabilities:

* Linking

<figure><img src="../../.gitbook/assets/Text Editor - Examples links.png" alt=""><figcaption><p>Image 4: Example C Forms</p></figcaption></figure>



## 5. Output

As you can see below, the text you enter into the Rich Text Editor is saved as structured data _(Image 5)_, and when the "Save text as HTML" option is checked in your column settings, it is saved as HTML _(Image 6)._

<figure><img src="../../.gitbook/assets/Text Editor - Output.png" alt=""><figcaption><p>Image 5: Structure data output</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Text Editor - Output HTML.png" alt=""><figcaption><p>Image 6: HTML output</p></figcaption></figure>
