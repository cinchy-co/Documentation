# Creating a Dynamic Meta-Form (Using Tables)

## 1. Overview

In this example, we will create a form that pulls specific information from a customer contacts table that a user is looking for.

## 2. Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Before creating a form, you must have the [CLI package set up in your environment](https://cinchy.gitbook.io/cinchy-meta-forms/meta-forms-overview/dynamic-forms-example)​
* You must also have[ installed and configured the Meta-Forms experience​](../meta-forms-deployment-installation-guide/)

## 3. Creating your Form <a href="#1.-creating-your-form" id="1.-creating-your-form"></a>

In this example, we are using a form to pull information about contacts from specific table, to display in the forms experience.You can also use a form to push data to a connected table. _See: Step 5, Creating New Records._

1. Navigate to the **Forms** table by searching for "Forms" on the homepage _(Image 1)._

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FvZ0sUk00NL1WvoqYY7ZS%2Fimage.png?alt=media&#x26;token=c0c06b50-0353-4e3e-9f1a-ed2cf47ec082" alt=""><figcaption><p>Image 1: Navigate to the Forms table</p></figcaption></figure>

2. Fill out the following sections of a new row _(Image 2):_

* **Table:** For a pull request, this will be whichever table you want to be receiving information from. In this example, we have used the **Contacts** table.
* **Name:** This will be the name of your form. In this example, we have used **Contacts Form (Example Test)**.
* **Display Name:** You may enter an alternative display name if you wish
* **Brand:** Select a branding colour. In this example we have selected **light green.**
* **Is Accordion:** Select whether you want your form to appear in accordion format or not.
* **Lookup Label:** This option will designate how you pull information from your assigned table. We have designated the **Contacts.People.Name** column from the Contacts table, in order to pull information based on a contact name.
* **Owner Groups:** Select which groups have ownership of the form. In this example we have used the **Product Success** group.
* **User Groups:** Select which user groups have access to the form. In this example we have used the **Product Success** group.
* **Bookmarked Users:** If you want this form to appear in your **"My Forms"** view on the Forms table, enter it here.

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2Fc3alCAX0QpNDMIPY822j%2Fimage.png?alt=media&#x26;token=f2f6037b-72ca-4ae8-968a-de2a03540052" alt=""><figcaption><p>Image 2: Filling out the form table</p></figcaption></figure>

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2F6yA8qSGBoKm0SxIpSAnz%2Fimage.png?alt=media&#x26;token=53f0d64a-4e92-4489-8442-a1f612f6bc79" alt=""><figcaption><p>Image 2: Continued</p></figcaption></figure>

## 4. Setting Up Your Form Sections <a href="#2.-setting-up-your-form-sections" id="2.-setting-up-your-form-sections"></a>

The next step is to set up your various form sections. These act as the headers of the form, and help organize your information.In this example we will create section to pull information from the **Profile, Contact Info,** and **Company** sections of the **Contacts** table.

1. Navigate to the **Form Sections** table by searching for Form Sections on the homepage _(Image 3)._

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FKwJ3lr1ZuiRUg3JYRx9z%2Fimage.png?alt=media&#x26;token=affbc789-0450-48b1-a6ce-97ad35a51a7d" alt=""><figcaption><p>Image 3: Navigate to the Form Sections table</p></figcaption></figure>

2\. You will fill out a new row for each form section you want, filling out the following _(Image 4):_

* **Form:** From the drop down, select the name of the form that you just created. In this example, that would be **Contacts Form (Example Test).**
* **Sequence:** This section can be used to dictate the order in which your sections appear on your form, with 1 appearing first.
* **Name:** Enter the name of your field. In this example, we are entering P**rofile** for the first row, **Contact Info** for the second, and **Company** for the third form section row.
* **Columns in Row:** Enter in how many columns you want to appear for this section. In this example we have chosen **1** column for the Profile section, and **2** each for Contact Info and Company.
* **Auto Expand:** Check this box if you want your sections to auto expand when someone opens the form.

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FrRnerlaIcciuYNGyzNGR%2Fimage.png?alt=media&#x26;token=b4906d1c-d34b-4f78-a753-473a91fcbae1" alt=""><figcaption><p>Image 4: Filling out the Form Sections Table</p></figcaption></figure>

## 5. Setting Up Your Form Fields <a href="#3.-setting-up-your-form-fields" id="3.-setting-up-your-form-fields"></a>

The final step of form creation is to populate the **Form Fields** section.

1. Navigate to the Form Fields table by searching for Form Fields on the homepage _(Image 5)._

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FvYWeORjom5WmVxW2fH1w%2Fimage.png?alt=media&#x26;token=d7e82b30-5a5d-45b7-a10e-b0a1a8ba199b" alt=""><figcaption><p>Image 5: Form Fields table</p></figcaption></figure>

​​2. Populate the following sections _(Image 6)_:

* **Form Section:** Populate this column with the fields that you created in step 2 of "Form Sections". For example, here we will populate five columns, **Profile, Contact Info, and Company**. Note that we populate Contact Info and Company twice in two separate columns since we inserted "2" as the value for our **"Columns in Row"** variable in step 2.
* **Sequence Override:** You may input an override to the sequence order here if you desire.
* **Tale Column:** This is where we link to the column that is storing the information we seek. In this example, we link to the **Customer Contacts** table, and specifically to the section for **Name, Email, Work Phone, Company, and Website.**

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2FAmDzQiDlQbIY0o9JjN7Q%2Fimage.png?alt=media&#x26;token=b7f724c3-c5b7-4a28-9b6c-5458e8201630" alt=""><figcaption><p>Image 6: Populate your form fields</p></figcaption></figure>

## 6. Test Your Form <a href="#4.-test-your-form" id="4.-test-your-form"></a>

1. You can open your form by clicking on your **Form Name > Open.**
2. To populate our example form, select a record from **"Select Existing Record".** The information will automatically populate in your form fields _(Image 7)_.

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-Meab1e-ktEn2Oom7FZi%2Fuploads%2F6PoI3aI4UeiOTsSDqJEh%2Fimage.png?alt=media&#x26;token=cdb9f566-ed7c-44b2-ba23-4e2f4a4934cc" alt=""><figcaption><p>Image 7: Testing your form</p></figcaption></figure>

## 7. Creating New Records <a href="#5.-creating-new-records" id="5.-creating-new-records"></a>

Your form can both pull information from an existing table and push information to that same table.

1. On the left-hand navigation pane, click on **Create New Record** _(Image 8)_. The information that you populate and save into the form here will automatically be reflected on the connected table.

<figure><img src="https://762429502-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Meab1e-ktEn2Oom7FZi%2F-MekLlGaeTKFEGZFvngX%2F-MekSiJBdq7xIi9QdVBJ%2Fform%20sections.png?alt=media&#x26;token=234fccd6-9a4c-47e9-8d4a-965cbbfc87ea" alt=""><figcaption><p>Image 8: Creating a new record</p></figcaption></figure>

## 8. Adding Your Form to the Homepage <a href="#6.-adding-your-form-to-the-homepage" id="6.-adding-your-form-to-the-homepage"></a>

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

<figure><img src="../../.gitbook/assets/image (266).png" alt=""><figcaption></figcaption></figure>
