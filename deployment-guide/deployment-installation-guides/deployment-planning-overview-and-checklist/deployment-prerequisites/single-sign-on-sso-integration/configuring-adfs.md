---

description: >-
  This document outlines the steps for configuring Active Directory Federation Services (ADFS) to facilitate Single Sign-On (SSO) with Cinchy.

---

# Configuring ADFS for Cinchy SSO

## Before you begin

Before you start the ADFS configuration, make sure you have the following pieces of information:

- **Cinchy SSO URL**: The URL for your Cinchy SSO instance.  
  Reference: `{your.cinchysso.url}`

- **Cinchy URL**: The URL of your main Cinchy instance.  
  Reference: `{your.cinchy.url}`

- **Cinchy SSO Installation Path**: The directory where the CinchySSO files are located.  
  Reference: `{Path/to/CinchySSO}`

- **ADFS server**: The URL of your ADFS server.
- Reference: `{your.ADFS.server}`

## Configuration Steps in ADFS

1. Navigate to **AD FS Management** on your ADFS server.

2. Right-click on **Relying Party Trusts** and choose **Add Relying Party Trust** to open the **Add Relying Party Trust Wizard**.  
   ![Image 1: Add Relying Party Trust Wizard](<../../../../../.gitbook/assets/image (586).png>)

3. In the wizard, select **Claims Aware > Start > Select Data Source**.

4. Select **Enter Data About the Relying Part Manually > Next**.

5. Fill in a **Display Name** under **Specify Display Name**.

6. Skip certificate configuration in **Configure Certificates**.

7. In **Configure URL**, select **Enable support for the SAML 2.0 SSO Web SSO protocol**.

8. Input your **login URL** as follows:

   ```
   https://{your.cinchysso.url}/Saml2/Acs
   ```

9. Under **Configure Identifiers**, add an Identifier and press **Next** to complete the setup.

## Setting Up Claim Issuance Policy

1. **Right-click** on the newly created Relying Party Trust (located by its Display Name) and select **Edit Claim Issuance Policy**.

2. Select **Add Rule > Claim Rule > Send LDAP Attributes as Claims**.

3. Input a **Claim Rule Name**.

4. In the **Attribute Store**, select **Active Directory**. Map the LDAP attributes to the corresponding outgoing claim types as shown in the table below:

| LDAP Attribute      | Outgoing Claim Type | Comments                                   |
| ------------------- | ------------------- | ------------------------------------------ |
| User-Principal-Name | Name ID             |                                            |
| SAM-Account-Name    | sub                 | Type `sub` manually to avoid auto complete |
| Given-Name          | Given Name          | Required for Auto User Creation            |
| Surname             | Surname             | Required for Auto User Creation            |
| E-Mail-Address      | E-Mail Address      | Required for Auto User Creation            |
| Is-Member-Of-DL     | Role                | Required for Auto User Creation            |

![Image 2: Add Transform Claim Rule Wizard](<../../../../../.gitbook/assets/image (495).png>)

5. Select **Finish**.

6. Select **Edit Rule** > **View Rule Language**. Copy the Claim URLs for later use in configuring your Cinchy `appsettings.json`. It should look like the following:
    ```json
    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
      => issue(store = "Active Directory",
              types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier",
                        "sub",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress",
                        "http://schemas.microsoft.com/ws/2008/06/identity/claims/role"),
              query = ";userPrincipalName,sAMAccountName,givenName,sn,mail,memberOf;{0}",
              param = c.Value);
    ```
    

7. Press **OK** to confirm and save.

8. Right-click on **Relying Party Trust > Properties**. Move to the **Advanced** tab and select **SHA-256** as the secure hash algorithm.  
   ![Image 3: Set the secure hash algorithm to SHA-256](<../../../../../.gitbook/assets/image (583).png>)

## Configuration for Cinchy

{% hint style="info" %}
Note: Please ensure that the configurations below are case-sensitive and align exactly with those in your SAML IdP setup.
{% endhint %}

### Initial setup

1. Retrieve and save the Federation Metadata XML file from the following location: `https://{your.ADFS.server}/FederationMetadata/2007-06/FederationMetadata.xml`.

2. If needed, use **IIS Manager** to establish an HTTPS connection for the Cinchy website.

3. Also establish an HTTPS connection for the SSO site. Make sure the port number aligns with the one specified in the login URL.

### Configuration for appsettings.json

#### App settings section

| Attribute                   | Value or Description                                                                                                                                                                                                |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CinchyLoginRedirectUri      | The URL where the user is redirected to log in: `https://{your.cinchysso.url}/Account/LoginRedirect`                                                                                                                |
| CinchyPostLogoutRedirectUri | The URL where the user is redirected after logout: `https://{your.cinchy.url}`                                                                                                                                      |
| CertificatePath             | The path to the Cinchy SSO certificate: `{Path/to/CinchySSO}\\cinchyidentitysrv.pfx`                                                                                                                                |
| SAMLClientEntityId          | The **Relying Party Identifier** from the **Relying Party Trust** you configured earlier                                                                                                                            |
| SAMLIDPEntityId             | The entity ID for the SAML Identity Provider, as seen near the beginning of your FederationMetadata.xml: `http://<AD-Server>/adfs/services/trust`                                                                   |
| SAMLMetadataXmlPath         | The location where you saved the FederationMetadata.xml file during step 1                                                                                                                                          |
| SAMLSSOServiceURL           | Located in the Domain controller under in-service endpoints, look for type SAML 2, URL path: `https://{your.AD.server}/Saml2/Acs`. This should be the same as the login URL you provided in the ADFS Configuration. |
| AcsURLModule                | `/Saml2`                                                                                                                                                                                                            |
| MaxRequestHeadersTotalSize  | The maximum header size in bytes. Adjust this value if the default size doesn't accommodate your needs.                                                                                                             |
| MaxRequestBufferSize        | Should be equal or larger than the total size of your headers.                                                                                                                                                      |
| MaxRequestBodySize          | The maximum request body size in bytes. If set to `-1`, the default value will be used. It's usually not necessary to modify this value.                                                                            |

#### External identity claim section

You will need to refer to the Rule Language URLs you copied from the ADFS Configuration. Replace the placeholders below with your own URLs:

```js
{
  "AppSettings": {
    // ... Other settings
  },
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
}
```

### Edit web.config

Insert the following lines within the **appSettings** section of your `web.config` file. Make sure to replace the 
`{your.cinchy.url}` and `{your.cinchysso.url}` with your Cinchy and Cinchy SSO values.

```xml
<appSettings>
  <!-- Other settings -->
  <add key="UseHttps" value="true" />
  <add key="StsAuthorityUri" value="https://{your.cinchy.url}" />
  <add key="StsRedirectUri" value="https://{your.cinchysso.url}/Account/LoginRedirect" />
  <!-- Other settings -->
</appSettings>
```
