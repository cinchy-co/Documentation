# Creating a Dynamic Meta-Form Example (Using Form Designer)

## 1. Overview

This page outlines how to create a form using the form designer, a simple alternative for making your forms.

## 2. Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Before creating a form, you must have the [CLI package set up in your environment](https://cinchy.gitbook.io/cinchy-meta-forms/meta-forms-overview/dynamic-forms-example-using-form-designer)​
* You must also have [installed and configured the Meta-Forms experience​](../meta-forms-deployment-installation-guide/)

## 3. Creating your Form <a href="#1.-creating-your-form" id="1.-creating-your-form"></a>

The form designer allows users to create new Forms using the Forms experience itself and is an intuitive and easy process.

In this example, a **Contacts Form (Example Test)** form will be created to pull information from the **Contacts** table.

1. Navigate to the **Form Designer** experience from the homepage (Image 1).

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption><p>Image 1: Navigate to the Form Designer experience</p></figcaption></figure>

2. Open up the experience and it should look like this (Image 2):

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption><p>Image 2: The Forms Designer Experience</p></figcaption></figure>

3. To create a new form, click the Create New Record button on the left-hand navigation pane (Image 3):

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2Fw3vrXn2vV7LgRC7NN4fG%2Fimage.png?alt=media&#x26;token=47ac2eba-0a33-4c12-a84d-558f3492b9d2" alt=""><figcaption><p>Image 3: Create New Record button</p></figcaption></figure>

4. In the **Form Definitions** section, fill in the following information and click **Save** once done (Image 4):

* **Table:** This will be the table that you want to push/pull information to/from. In this example, we are pulling information from the **Contacts.People** table.
* **Name:** The name of your form. In this example we have called it **Contacts Form (Example Test)**
* **Display Name:** Enter a display name for your form. In this example we have called it **Contacts Form (Example Test)**
* **Lookup Label:** This option will designate how you pull information from your assigned table. We have designated the **Contacts.People.Name** column from the Contacts table, in order to pull information based on a contact name.
* **Is Accordion:** Select this checkbox if you want the form to open in accordion format or not.
* **Brand:** Select a branding colour for this form.

<figure><img src="../../.gitbook/assets/image (354).png" alt=""><figcaption><p>Image 4: Filling out the Forms Definition</p></figcaption></figure>

5. Once your Form Definitions are set, the **Form Sections** tab will open (Image 5).

<figure><img src="../../.gitbook/assets/image (398).png" alt=""><figcaption><p>Image 5: Form Sections</p></figcaption></figure>

6. Click on the **plus sign** to add a Section Definition to your form (Image 6).

<figure><img src="../../.gitbook/assets/image (358).png" alt=""><figcaption><p>Image 6: Section Definition</p></figcaption></figure>

7. You can now add your Form Sections. In this example we are adding three sections: Profile, Contact Info, and Company.

Fill in the following information and select **OK** when done (Image 7):

* **Name:** Add in your section name. In this example we are using **Profile, Contact Info, and Company,**
* **Sequence:** Select the sequence that this section will show up on the form. 1 means it will show up first.
* **Columns in Row:** Select how many columns you'd like to appear in the row. In this example we have chosen **1** for our **Profile** section, and **2** for our **Company** and **Contact Info** sections.
* **Auto-Expand:** Check this box if you want to the section to auto-expand.

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FYpsvnX1Ce6HUW3pF71t0%2Fimage.png?alt=media&#x26;token=5ef4bc44-05ad-4c0f-a689-4901953e90d5" alt=""><figcaption><p>Image 7: Filled in Form Sections</p></figcaption></figure>

8\. Click **Save.**

9\. Within the **Actions** column of each of your sections, click on **Design** to navigate to the **Form Section Designer.**

11\. Navigate to the **Section Fields tab** (Image 8).

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FYU1jpNaoa3Pl3toyxqos%2Fimage.png?alt=media&#x26;token=30d27ac1-dceb-44d5-be5c-6fbd5b53f01d" alt=""><figcaption><p>Image 8: The Section Fields tab</p></figcaption></figure>

12\. Click on the **+** in the **Fields** tab to add a new **Section Field.** In this example, we are adding two fields to the **Contact Info** section of our form (since we designated it to have 2 columns per row in step 7).

13\. Fill out the following sections and click OK when finished (Image 9):

* **Table Column:** The column you want to pull from/push to in this field. In this example we have used Contacts.People.Email and Contacts.People.Work Phone.
* **Name:** Select a name for your section. In this example we have used **Email** and **Work Phone.**
* **Sequence Override:** You can choose to override the sequence in which this section appears on the form, with the section listed as **1** appearing first.
* **View Only:** Check this box if you want this section to be viewable only.

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FOE7a6oZw4NS7xGiO4x0P%2Fimage.png?alt=media&#x26;token=b36b161c-9f79-4c8c-a016-a50bef43d836" alt=""><figcaption><p>Image 9: Your Form Sections</p></figcaption></figure>

14\. Click **Save.**

15\. Navigate back to your **Forms Table**, click **Open,** and it will appear with your sections and fields (Image 10):

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FgXTTynXBv19C6Krngnm7%2Fimage.png?alt=media&#x26;token=e3aca4e6-8c2a-4414-bdaf-323092f71754" alt=""><figcaption><p>Image 10: Your completed form​</p></figcaption></figure>

## 4. Adding Your Form to the Homepage <a href="#2.-adding-your-form-to-the-homepage" id="2.-adding-your-form-to-the-homepage"></a>

1. To add your form as an applet that users can quickly bookmark and access, navigate to the **Cinchy Applets** table.
2. In a new row, fill out the following information:

* **Domain:** The domain that your form sits in.
* **Name:** The name of your form/applet. In this example we use **"Contacts Form (Example Test)".**
* **Icon:** Select an icon for the widget.
* **Icon Colour**
* **Description:** Add a description for your applet.
* **Target Window:** Select if you want you form to open in a new window or not.
* **Application URL:** Add in the URL to your form here.
* **Users and Groups:** Add in which users/groups can access your form in these columns.

​​

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2F2WNsqJMUN6cLiewDwBuG%2Fimage.png?alt=media&#x26;token=ed7a8b13-982e-4529-ad45-6447ed968a81" alt=""><figcaption></figcaption></figure>
