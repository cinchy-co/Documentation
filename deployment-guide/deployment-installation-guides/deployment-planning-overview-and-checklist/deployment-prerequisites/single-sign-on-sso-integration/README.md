---
description: >-
  This page walks through the integration of an Identity Provider with Cinchy
  via SAML Authentication
---

# Single sign-on (SSO) integration

## Overview

Cinchy supports integration with any **Identity Provider that issues SAML tokens** (such as Active Directory Federation Services) for authenticating users.

It follows an **SP Initiated SSO pattern** where the SP will Redirect to the IdP and the IdP must submit the SAML Response via an HTTP Post to the SP Assertion Consumer Service.

Below is a diagram outlining the flow when a non-authenticated user attempt to access a Cinchy resource _(Image 1)._

![Image 1: Non-authenticated user access attempt ](<../../../../../.gitbook/assets/image (48).png>)



## Configure SAML authentication - IIS deployments

You must register Cinchy with the Identity Provider. As part of that process you'll supply the Assertion Consumer Service URL, choose a client identifier for the Cinchy application, and generate a metadata XML file.

The Assertion Consumer Service URL for Cinchy is **the base URL of the CinchySSO application followed by "{AcsURLModule}/Acs"**

`https:///\<CinchySSO URL>/Saml2/Acs`

`https://myCinchyServer/Saml2/Acs`

To enable SAML authentication within Cinchy, do the following:

1. You can find the necessary metadata XML from the applicable identity provider. Place the metadata file in the deployment directory of the CinchySSO web application.

{% hint style="info" %}
If you are using **Azure AD** for this process, you can find your metadata XML by [following these steps.](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/troubleshoot-saml-based-sso#cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application)

If you are using **GSuite** for this process, you can find your metadata XML by [following steps 1-6 here.](https://support.google.com/a/answer/6087519?hl=en)

If you are using **ADFS** for this process, you can find your metadata XML at the following link, inputting your own information for **\<your.AD.server>**: _`https://`**`<your.AD.server>`**`/FederationMetadata/2007-06/FederationMetadata.xml`_

If you are using **Okta** for this process, you can find your metadata XML by [following these steps.](https://support.okta.com/help/s/article/How-do-we-download-the-IDP-XML-metadata-file-from-a-SAML-Template-App?language=en\_US)

If you are using **Auth0** for this process, you can find your metadata XML by [following these steps.](https://auth0.com/docs/authenticate/protocols/saml/saml-sso-integrations/configure-auth0-saml-identity-provider#configure-saml-sso-on-the-service-provider)

If you are using **PingIdentity** for this process, you can find your metadata XML by [following these steps.](https://docs.pingidentity.com/bundle/pingfederate-93/page/bdx1564002975039.html)
{% endhint %}

2. Update the values of the below app settings in the CinchySSO appsettings.json file.

* **SAMLClientEntityId** - The client identifier chosen when registering with the Identity Provider
* **SAMLIDPEntityId** - The entityID from the Identity Provider metadata XML
* **SAMLMetadataXmlPath** - The full path to the metadata XML file
* **AcsURLModule -** This parameter is needs to be configured per your SAML ACS URL. For example, if your ACS URL looks like this **"https:///\<CinchySSO URL>/Saml2/Acs",** then the value of this parameter should be **"/Saml2"**

{% hint style="info" %}
When configuring the Identity Provider, the only required claim is a user name identifier. If you plan to enable automatic user creation, then additional claims must be added to the configuration, **see section 4 below for more details.**
{% endhint %}

Once you enable SSO, the next time a user arrives at the Cinchy login screen they will see an additional button for **"Single Sign-On".**

## Configure SAML authentication - Kubernetes deployments

1. Retrieve your **metadata.xml** file from your identity provider.

{% hint style="info" %}
If you are using **Azure AD** for this process, you can find your metadata XML by [following these steps.](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/troubleshoot-saml-based-sso#cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application)

If you are using **GSuite** for this process, you can find your metadata XML by [following steps 1-6 here.](https://support.google.com/a/answer/6087519?hl=en)

If you are using **ADFS** for this process, you can find your metadata XML at the following link, inputting your own information for **\<your.AD.server>**: _`https://`**`<your.AD.server>`**`/FederationMetadata/2007-06/FederationMetadata.xml`_

If you are using **Okta** for this process, you can find your metadata XML by [following these steps.](https://support.okta.com/help/s/article/How-do-we-download-the-IDP-XML-metadata-file-from-a-SAML-Template-App?language=en\_US)

If you are using **Auth0** for this process, you can find your metadata XML by [following these steps.](https://auth0.com/docs/authenticate/protocols/saml/saml-sso-integrations/configure-auth0-saml-identity-provider#configure-saml-sso-on-the-service-provider)

If you are using **PingIdentity** for this process, you can find your metadata XML by [following these steps.](https://docs.pingidentity.com/bundle/pingfederate-93/page/bdx1564002975039.html)
{% endhint %}

2. Navigate to your **cinchy.kubernetes\environment\_kustomizations\_template\instance\_template\idp\kustomization.yaml** file.

3. Add your **metadata.xml patch** into your secrets where specified below as <\<metadata.xml>>

```yaml
- target:
    version: v1
    kind: Secret
    name: idp-secret-appsettings
  patch: |-
    - op: replace
      path: /data/appsettings.json
      value: <<idp_appsettings_json>>
    - op: add
      path: /data/metadata.xml
      value: <<metadata.xml>>
```

4. Navigate to your **devops.automation > deployment.json** in your Cinchy instance.

5. Add the following fields into the .json and update them below **using the metadata.xml.**

```json
"sso": {
    "SAMLClientEntityId": "cinchy-dev", // Cinchy instance name
    "SAMLMetadataXmlPath": "/usr/share/appsettings/metadata.xml",
    // All below values get from metadata.xml and fill them here.
    "SAMLIDPEntityId": "",
    "SAMLSSOServiceURL": "",
    "AcsURLModule": "",
    "FirstNameExternalClaimName": "",
    "LastNameExternalClaimName": "",
    "EmailExternalClaimName": "",
    "MemberOfExternalClaimName": "",
    "metadata.xml": "BASE64_ENCODED_METADATA.XML"
}
```

6. Navigate to your **kubernetes\environment\_kustomizations\_template\instance\_template\_encoded\_vars\idp\_appsettings\_json.**

7.  Update the below code with your proper **AppSettings and ExternalIdentityClaimSection** details.

```json
{
    "ConfigSettings": {
        "AppSettings": {
            "SAMLClientEntityId": "<<SAMLClientEntityId>>",
            "SAMLIDPEntityId": "<<SAMLIDPEntityId>>",
            "SAMLMetadataXmlPath": "<<SAMLMetadataXmlPath>>",
            "SAMLSSOServiceURL": "<<SAMLSSOServiceURL>>",
            "AcsURLModule": "<<AcsURLModule>>",
        },
        "ExternalIdentityClaimSection": {
            "FirstName": {
                "ExternalClaimName": "<<FirstNameExternalClaimName>>"
            },
            "LastName": {
                "ExternalClaimName": "<<LastNameExternalClaimName>>"
            },
            "Email": {
                "ExternalClaimName": "<<EmailExternalClaimName>>"
            },
            "MemberOf": {
                "ExternalClaimName": "<<MemberOfExternalClaimName>>"
            }
        }
    }
}
```

8. **Run DevOps automation script** which will populate the updated outputs into the **cinchy.kubernetes** repository.

9. Commit your changes and push to your source control system.

10. Navigate to your ArgoCD dashboard and refresh the **idp-app** to pick up your changes. It will also delete your currently running pods to pick up the latest secrets.

11. Once the pods are healthy, you can verify the changes by looking for the **SSO Tab on your Cinchy login page.**

## User management

Before a user is able to login through the SSO flow, the user must be set up in Cinchy with the appropriate authentication configuration.

Users in Cinchy are maintained within the Users table in the Cinchy domain. Each user in the system is configured with 1 of 3 Authentication Methods:

* **Cinchy User Account** - These are users that are created and managed directly in the Cinchy application. They log into Cinchy by entering their username and password on the login screen.
* **Non Interactive** - These accounts are intended for application use.
* **Single Sign-On** - These users authenticate through the SSO Identity Provider (configured using the steps above). They log into Cinchy by clicking the "Login with Single Sign-On" link on the login screen.

### Define a new SSO User

Create a new record within the Users table with the Authentication Method set to **Single Sign-On**.

{% hint style="info" %}
The password field in the Users table is mandatory. For SSO users, the value entered is ignored. You can input **n/a**.
{% endhint %}

### Convert an existing user to SSO User

Change the Authentication Method of the existing user to **Single Sign-On**.

### Login with SSO

When a user is configured for SSO, they can select **Login with Single Sign-On** on the login page, which directs logins through the Identity Provider's authentication flow.

{% hint style="warning" %}
If a user successfully authenticates with the Identity Provider but hasn't been set up in the Users table, then they will see the following error message - " **You are not a registered user in Cinchy. Please contact your Cinchy administrator."** To avoid the manual step to add new users, you can consider enabling automatic user creation.
{% endhint %}

## Automatic user creation - IIS deployments

On SSO enabled Cinchy instances, users that don't exist in the Cinchy Users table won't be able to login, regardless if they're authenticated by the Identity Provider.

If you enable Automatic User Creation, the Identity Provider authorizes the user and automatically create a user entry in the Cinchy Users table if one doesn't already exist. **This means that any SSO authenticated user is guaranteed to be able to access the platform.**

If AD Groups are configured within Cinchy, then the authenticated user is also automatically be added to any Cinchy mapped AD Groups where they're a member. See [AD Group Integration](ad-group-integration.md) for additional information on how to define AD Groups in Cinchy.

See below for details on how to enable Automatic User Creation.

{% hint style="info" %}
Users that are automatically added won't be allowed to create or modify tables and queries. To provision this access, Can Design Tables and Can Design Queries must be checked on the User record in the Cinchy Users table.
{% endhint %}

### Prerequisites for automatic user creation

The Identity Provider configuration must include the following additions to the base configuration in the SAML token response:

* First Name
* Last Name
* Email

To enable automatic group assignment for newly created users, then you must also include an attribute that captures the groups that this user is a member of. For example, the **memberOf** field in AD. This is applicable if you plan on using AD Groups.

### Configuration setup

To enable automatic user creation, you require the following changes. For IIS Deployments this will be done to the appsettings.json file in the CinchySSO web application.

1. Add **ExternalClaimName** attribute values under **"ExternalIdentityClaimSection"** in **appsettings.json** file. Don't add the value for **MemberOf** if you don't want to enable automatic group assignment .
2. The **ExternalClaimName** value must be updated to create a mapping between the attribute name in the SAML response and the required field. For example, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** is the name in the SAML response for the FirstName field.

{% code title="ExternalIdentityClaimSection" %}
```javascript
"ExternalIdentityClaimSection": {
			"FirstName": {
				"ExternalClaimName": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
			},
			"LastName": {
				"ExternalClaimName": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
			},
			"Email": {
				"ExternalClaimName": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
			},
			"MemberOf": {
				"ExternalClaimName": "http://schemas.microsoft.com/ws/2008/06/identity/claims/role"
			}
		}
```
{% endcode %}

## 6. Further Reading

* [Configuring ADFS](configuring-adfs.md)
* [AD Group Integration](ad-group-integration.md)
