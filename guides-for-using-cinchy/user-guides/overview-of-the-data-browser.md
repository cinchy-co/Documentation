---
description: >-
  This page provides an overview of some of the important pieces of the data
  browser: the homepage, the login page, and the data network.
---

# Overview of the Data Browser

## Table of Contents​

|                                                                               |
| ----------------------------------------------------------------------------- |
| [#my-network](overview-of-the-data-browser.md#my-network "mention")           |
| [#searching](overview-of-the-data-browser.md#searching "mention")             |
| [#my-bookmarks](overview-of-the-data-browser.md#my-bookmarks "mention")       |
| [#my-data-network](overview-of-the-data-browser.md#my-data-network "mention") |
| [#5.-the-logo](overview-of-the-data-browser.md#5.-the-logo "mention")         |

{% hint style="info" %}
Cinchy officially supports Google Chrome and Mozilla Firefox browsers for accessing the data browser.
{% endhint %}

## 1. Homepage <a href="#my-network" id="my-network"></a>

Once you log in to Cinchy, you'll be on the Homepage _(Image 1)_. From here, you can navigate to a variety of tables, queries, and applets you have access to.

You can return to this page at any time by clicking the Cinchy logo in the top left corner.

![Image 1: The Cinchy Homepage](<../../.gitbook/assets/image (563).png>)

## 2. Searching <a href="#searching" id="searching"></a>

‌All objects you have access to in your Marketplace (including bookmarks) are searchable and can be filtered by typing the partial or full name of the object you are searching for in the search bar _(Image 2)._

![Image 2: The search bar](<../../.gitbook/assets/image (246).png>)

You can also search by object type by clicking on either Tables, Queries, or Experiences in the toolbar.

## 3. Bookmarks <a href="#my-bookmarks" id="my-bookmarks"></a>

**‌**You can bookmark your most often used objects and rearrange them to your liking within your bookmarks.

To bookmark an object, select the star. The star will be yellow when bookmarked, and grey when not _(Image 3)._

![Image 3: Bookmarking](<../../.gitbook/assets/image (611).png>)

The object will pop into your “Bookmark” section.To rearrange your bookmarks simply drag and drop the object into the desired order.

## 4. Network Map <a href="#my-data-network" id="my-data-network"></a>

You "Network Map" shows a visualization of all tables in Cinchy you have access to and how they are all connected _(Image 4)._

Each of the coloured circles represents an object in Cinchy. The lines between them demonstrate the links between them.

![Image 4: Cinchy Network Map](<../../.gitbook/assets/image (532).png>)

You are able to search and open tables from this view using the search bar on the left _(Image 5)._

![Image 5: The search function](<../../.gitbook/assets/image (663).png>)

You can see what the network looked like in the past by clicking and dragging the pink circle along the timeline at the bottom.

{% hint style="info" %}
You can learn more about the Network Map [here.](../additional-guides/application-experiences/network-map/)
{% endhint %}

### 4.1 Network Map Extra Parameters

In Cinchy v5.2 we added the ability to include new parameters on the URL path for your network visualizer to focus your node view. You can now add **Target Node, Depth Level, and Max Depth Level** Parameters, if you choose.

Example: \<base url>/apps/datanetworkvisualizer?**targetNode=\&maxDepth=\&depthLevel=**

* **Target Node:** Using the Target Node parameter defines which of your nodes will be the central node from which all connections branch from.
  * Target Node uses the **TableID number,** which you can find in the URL of any table.
  * Example: \<base url>/apps/datanetworkvisualizer?**targetNode=8** will show **TableID 8** as the central node
* **Max Depths:** This parameter defines how many levels of network hierarchy you want to display.
  * Example: \<base url>/apps/datanetworkvisualizer?**maxDepth=2** will only show you two levels of connections.
* **Depth Level:** Depth Level is a UI parameter that will highlight/focus on a certain depth of connections.
  * Example: \<base url>/apps/datanetworkvisualizer?**DepthLevel=1** will highlight all first level network connections, while the rest will appear muted.

The below example visualizer uses the following URL _(Image 6)_: \<base url>/apps/datanetworkvisualizer?**targetNode=8\&maxDepth=2\&depthLevel=1**

* It shows **Table ID 8** ("Groups") as the central node.
* It only displays the **Max Depth of 2** connections from the central node.
* It highlights the nodes that have a **Depth Level of 1** from the central node.

<figure><img src="../../.gitbook/assets/image (378).png" alt=""><figcaption><p>Image 6: A Network Map with Parameters</p></figcaption></figure>



## 5. The Logo

You can upload a custom logo to appear on your platform login screen and homepage. You will need to have adamin access to do so.

#### Examples without a logo uploaded (Images 7&8)

<img src="../../.gitbook/assets/image (32).png" alt="" data-size="original">![](<../../.gitbook/assets/image (252).png>)

#### Examples with a logo uploaded (Images 9&10)

<img src="../../.gitbook/assets/image (358).png" alt="" data-size="original">![](<../../.gitbook/assets/image (666).png>)

### 5.1 Uploading a Logo:

1. Navigate to **\<base url>/admin/index**
2. Scroll to the bottom of the admin panel and navigate to the **“Upload Logo”** button _(Image 11)._

![Image 11: Upload Logo Button](https://lh5.googleusercontent.com/MrswakvG\_xoGCJp2R0wY-YWMs-NhdBeaoBZG5-K37d1fHA0SqaNpLIUFFI1lAph6oAwpzyfpdY-8bObZLhwUt16gCZs5lZ0QzWlWv040lO4wfxkfo8uwJC6JzPLiJLdLloGZLKLt16Yy4lR5FA)

3\. Upload your logo

### 5.2 Removing a Logo

1. Once uploaded, your logo is stored in the **System Properties** table_._
2. Navigate to the table and find the row with **Name: Logo** _(Image 12)_

![Image 12: Find the Logo row in the System Properties table](<../../.gitbook/assets/image (553).png>)

3\. **Delete** the Logo row to remove the logo.
