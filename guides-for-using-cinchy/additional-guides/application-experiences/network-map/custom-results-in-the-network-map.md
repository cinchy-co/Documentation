# Custom Results in the Network Map

## **Custom Results in the Network Map**

**Focus the results of your Network Map to show only the data that you really want to see** with our new URL parameters.

{% hint style="info" %}
This feature was added in version 5.20 of the Cinchy platform.
{% endhint %}

You can now add **Target Node, Depth Level, and Max Depth Level** Parameters, if you choose.

Example: \<base url>/apps/datanetworkvisualizer?**targetNode=\&maxDepth=\&depthLevel=**

* **Target Node:** Using the Target Node parameter defines which of your nodes will be the central node from which all connections branch from.
  * Target Node uses the **TableID number,** which you can find in the URL of any table.
  * Example: \<base url>/apps/datanetworkvisualizer?**targetNode=8** will show **TableID 8** as the central node
* **Max Depths:** This parameter defines how many levels of network hierarchy you want to display.
  * Example: \<base url>/apps/datanetworkvisualizer?**maxDepth=2** will only show you two levels of connections.
* **Depth Level:** Depth Level is a UI parameter that will highlight/focus on a certain depth of connections.
  * Example: \<base url>/apps/datanetworkvisualizer?**DepthLevel=1** will highlight all first level network connections, while the rest will appear muted.

The below example visualizer uses the following URL: \<base url>/apps/datanetworkvisualizer?**targetNode=8\&maxDepth=2\&depthLevel=1**

* It shows **Table ID 8** ("Groups") as the central node.
* It only displays the **Max Depth of 2** connections from the central node.
* It highlights the nodes that have a **Depth Level of 1** from the central node.

<figure><img src="../../../../.gitbook/assets/image (550).png" alt=""><figcaption></figcaption></figure>
