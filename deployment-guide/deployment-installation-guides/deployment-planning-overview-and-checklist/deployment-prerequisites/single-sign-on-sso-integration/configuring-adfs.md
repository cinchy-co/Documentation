---
description: >-
  The following outlines the configuration required in Active Directory
  Federation Services (ADFS) to enable Single Sign-On (SSO).
---

# Configure ADFS

## ADFS Configuration

1. On your ADFS Server, Open **AD FS Management**.

2. Right-click on **Relying Party Trusts** and select **Add Relying Party Trust** to launch the **Add Relying Party Trust Wizard** _(Image 1)._

![Image 1: Add Relying Party Trust Wizard](<../../../../../.gitbook/assets/image (586).png>)

3. In the ADFS Wizard, select **Claims Aware > Start > Select Data Source**.

4. Select **Enter Data About the Relying Part Manually > Next**.

5. Under **Specify Display Name**, enter a Display Name of your choice.

6. Under **Configure Certificates**, don't select any certificates.

7. Under **Configure URL**, select **Enable support for the SAML 2.0 SSO Web SSO protocol**.

8. Enter your **login URL** in the following format:

```
https://<cinchy-sso-URL>/Saml2/Acs
```

9. Under **Configure Identifiers**, choose an Identifier.

10. Select **Next** until the process finishes.

## Claim issuance policy

1. To begin configuring your claim issuance policy, **right-click** on the Relying Party Trust you just created (look for the Display Name) and select **Edit Claim Issuance Policy**.
2. Select **Add Rule >** **Claim Rule >** **Send LDAP Attributes as Claims**.
3. Add your **Claim Rule Name**
4. Under **Attribute Store**, choose **Active Directory**. Map the **LDAP attribute** to the following outgoing claim types:

<table data-header-hidden><thead><tr><th>LDAP Attribute</th><th width="249.33333333333331">Outgoing Claim Type</th><th>Comments</th></tr></thead><tbody><tr><td>LDAP Attribute</td><td>Outgoing Claim Type</td><td>Comments</td></tr><tr><td>User-Principal-Name</td><td>Name ID</td><td></td></tr><tr><td>SAM-Account-Name</td><td>sub</td><td><code>sub</code>will need to be typed manually, make sure it doesn't autocomplete to something else like subject.</td></tr><tr><td>Given-Name</td><td>Given Name</td><td>Necessary for Automatic User Creation</td></tr><tr><td>Surname</td><td>Surname</td><td>Necessary for Automatic User Creation</td></tr><tr><td>E-Mail-Address</td><td>E-Mail Address</td><td>Necessary for Automatic User Creation</td></tr><tr><td>Is-Member-Of-DL</td><td>Role</td><td>Necessary for Automatic User Creation</td></tr></tbody></table>

![Image 2: Add Transform Claim Rule Wizard](<../../../../../.gitbook/assets/image (495).png>)

4. Select **Finish**.

5. Select **Edit Rule.**

6. Select **View Rule Language** and copy out the Claim URLs for the claims defined. You need this information to configure your Cinchy **appsettings.json**. It will look something like this:

```
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

7. Select **OK** to save the rule.

8. Right-click on **Relying Party Trust** > **Properties**.

9. Go to the **Advanced** tab and set the secure hash algorithm to **SHA-256** _(Image 3)._

![Image 3: Set the secure hash algorithm to SHA-256](<../../../../../.gitbook/assets/image (583).png>)

## Cinchy Configuration

{% hint style="info" %}
Everything below is case sensitive and must match whatever is specified in your SAML IdP configuration.
{% endhint %}

1. Open `https://<your.AD.server>/FederationMetadata/2007-06/FederationMetadata.xml` in a browser and save the XML file in the cinchysso folder.
2. Open **IIS Manager** and create an HTTPS binding on the Cinchy site (if necessary).
3. Go to SSO site and bind HTTPS with it. Make sure to use the same port as the login URL above if specified.

### Cinchy appsettings.json

#### AppSettings Section

| Attribute                   | Value                                                                                                                                                                                                             |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CinchyLoginRedirectUri      | `https://<cinchy-sso-URL>/Account/LoginRedirect`                                                                                                                                                                  |
| CinchyPostLogoutRedirectUri | `https://<Cinchy-Web-URL>`                                                                                                                                                                                        |
| CertificatePath             | `<Path to cinchysso>\\cinchyidentitysrv.pfx`                                                                                                                                                                      |
| SAMLClientEntityId          | **Relying party identifier** from **Relying Party Trust** above                                                                                                                                                   |
| SAMLIDPEntityId             | <p><code>http://&#x3C;AD-Server>/adfs/services/trust</code></p><p>Your FederationMetadata.xml will have this near the beginning. Note that this is the entityID, not the ID.</p>                                  |
| SAMLMetadataXmlPath         | <p><code>&#x3C;Path to cinchysso>\\FederationMetadata.xml</code></p><p>This is the location where you placed the FederationMetadata.xml in step 1.</p>                                                            |
| SAMLSSOServiceURL           | <p>In Domain controller, in-service endpoints, look for type SAML 2, URL path: <code>https://&#x3C;AD-Server>/Saml2/Acs</code>  </p><p>Same as the login URL provided to the wizard in the ADFS Configuration</p> |
| AcsURLModule                | `/Saml2`                                                                                                                                                                                                          |
| MaxRequestHeadersTotalSize  | <p><strong>Integer</strong></p><p>Bytes to set the max request header to. If the default (likely 32kb) doesn't work, you may have to set this larger to accommodate a large number of groups.</p>                |
| MaxRequestBufferSize        | <p><strong>Integer</strong></p><p>This should be equal or larger than your header's total size above.</p>                                                                                                         |
| MaxRequestBodySize          | <p><strong>Integer</strong></p><p>If any of these values are `-1 `they will use the default. It's not necessary to change the body size.</p>                                                                       |

#### External Identity Claim Section

You will need the Rule Language URLs you copied out from the ADFS Configuration in the example above. Using that example, we would get the following (replace with your own URLs).

```js
{
  "AppSettings": {
    ...
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

### Web.config

Add the 3 following lines to your web.config within the **appSettings** section:

```markup
<appSettings>
  ...
  <add key="UseHttps" value="true" />
  <add key="StsAuthorityUri" value="https://<your.cinchy.url>" />
  <add key="StsRedirectUri" value="https://<your.cinchysso.url>/Account/LoginRedirect" />
  ...
</appSettings>
```
