# Post Sync Scripts

## Overview

Post sync scripts are written in CQL and can be added to the end of a sync to allow you to do more with your data, such as inserted retrieved values into a Cinchy table.

<figure><img src="../../../.gitbook/assets/image (278).png" alt=""><figcaption></figcaption></figure>

## Example

This example takes you through a batch data sync using the Cinchy Table **\[Product].\[Names Test].** This scenario uses the following API as the destination:  [https://cinchy.net/API/Product/LatestCinchyReleaseDate](https://cinchy.net/API/Product/LatestCinchyReleaseDate)

When you run the batch job, it will check for updates in the source table. These updates trigger the REST API to fetch our defined value, which we can use in a post-sync script. In this example, the script will insert this value into a second table, **\[Product].\[Response Table].**

The following steps will walk you through how to use this functionality.

1. Set up your data sync to a REST API. You can review the **Source** _(Image 1),_ **Destination** _(Image 2)_, and **Sync Behaviour** _(Image 3)_ below.

<figure><img src="https://lh5.googleusercontent.com/_eb2qTk-AHKJKOyTDs_x2INkb411fJsUr4SnJXZn-Lw-g-MmKYvDVS-8Ipbe365ylWIEKGfzIsryr4A50j7VNgyqdWDER4oc0b0plB3NXsTW4IpyWIjlvwZBJs24Y9zqeCK7hkQXWrTj2d5ZMmd5nuAnEulIjtQ-sE4cWSsWLZO0NcissuusyQs4On9FJQ" alt=""><figcaption><p>Image 1: Post Sync Scripts</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (195).png" alt=""><figcaption><p>Image 2: Post Sync Scripts</p></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/Wbc5RRQRbhcuhULXlKvqBUrSiRw2HcDIimdA6-0WgC5_6LzW94wqRDOMLFDVCMNXOv7FlL8wGdvhmCe0-Ky6Xoz-S_5OlOLavKr421mVU6m6IOj852-ceSdR5Alth6IbeJZUhhaFAttoQTb191Gj9MeeZi0CNrsHJMMABqfHBz1Cbraiez2-A8DX6yTpsQ" alt=""><figcaption><p>Image 3: Post Sync Scripts</p></figcaption></figure>

1. Under the **Post Sync tab**, input the CQL that takes the defined variable from the REST API and insert it into the **\[Product].\[Response Test]** table _(Image 4)._

<figure><img src="https://lh4.googleusercontent.com/tgyxXID-3g8RcMVWygxSPMVMScmjqmdJGu0hra3q5SSuRugND3fEFcI4TIs63oQHmjyXBZl8uXGkChgXDJOaJYRRiWGS19Gej0K_bZtviLBO-OafojrzWmwl8AA8TOQ8WbEkdvmfqB12Z8MEG_vGDeBHts3QvWzr3KbU5z5X8mAArpumWvjWRkOsSyiKWg" alt=""><figcaption><p>Image 4: Post Sync Scripts</p></figcaption></figure>

3. Add in your Permissions and click **Save.**

4.  After you configure the sync, run the job. It will check for new data in the Name column on the **\[Product].\[Names]** table _(Image 5)_. When found, it will trigger the REST API to GET the “value” variable. The post sync script will take that value and INSERT it into the **\[Product].\[Response Test]** table _(Image 6)._

<figure><img src="https://lh4.googleusercontent.com/IBkC9bHB-O1iuPKmBiHIzim_SV_xgPkp79PH_R9LS6yUkLTbFEchp5jMLlu0-KzPu7-Cbd4SrcvR31XPJCLG8LHpvxgoIdu23IWArqX2EvQ3g7FnoF6feEp_V06X-WSAoZ08JXvG4ud0mZleVqiwwoCL5Xqp0fCSXAGGzBL-z3WcLDkOTubhx129kOP5Ww" alt=""><figcaption><p>Image 5: Post Sync Scripts</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption><p>Imaage 6: Post Sync Scripts</p></figcaption></figure>
