# Setting Up Experiences

## Integrated Client Setup

| Column Name                     | Description                                                                                              |
| ------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Client Id                       | A unique identifier for each client.                                                                     |
| Client Name                     | A friendly name for the client to help users maintaining this record.                                    |
| Grant Type                      | The OAuth 2.0 flow that will be used during authentication. "Implicit" should be selected for API calls. |
| Permitted Login Redirect URLs   | Add all URLs of an Applet separated by semicolon which can initiate login                                |
| Permitted Logout Redirect URLs  | Add all URLs of an Applet separated by semicolon which can be used as Post Logout URL                    |
| Permitted Scopes                | The list of permitted OAuth scopes, please check all available options.                                  |
| Access Token Lifetime (seconds) | The time after with the token expires. If left blank, the default is 3600 seconds.                       |
| Show Cinchy Login Screen        | Uncheck if you want to have SSO as default authentication and skip the Cinchy login screen               |
| Enabled                         | This checkbox is used to enable or disable a client                                                      |
| Guid                            | This is a calculated field that will auto-generate the client secret                                     |



## Applet Setup

| Column Name       | Value                                                                                                                                                                                                                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Domain            | Select a domain for the applet to belong to.                                                                                                                                                                                                                                                                                                      |
| Name              | This is the name that will display for the applet in My Network                                                                                                                                                                                                                                                                                   |
| Full Name         | This is a calculated field `Domain.Name`                                                                                                                                                                                                                                                                                                          |
| Icon              | Select a system icon for the applet, this will show in My Network.                                                                                                                                                                                                                                                                                |
| Icon Colour       | Select a system color for the icon above.                                                                                                                                                                                                                                                                                                         |
| Description       | Similar to table or query description. This field is viewable and searchable in My Network.                                                                                                                                                                                                                                                       |
| Target Window     | <p>When someone clicks the applet, this is the default way it will open.</p><p>Existing Window (Redirect) - this will redirect the user in the current window</p><p>Existing Window (Embedded) - this will open the applet embedded in Cinchy, the Cinchy header will be visible</p><p>New Window - the applet will open in a new window/tab.</p> |
| Application Url   | This is the URL where the applet resides.                                                                                                                                                                                                                                                                                                         |
| Users             | Users who can see this applet in the marketplace.                                                                                                                                                                                                                                                                                                 |
| Groups            | Groups who can see this applet in the marketplace.                                                                                                                                                                                                                                                                                                |
| Integrated Client | The integrated client for the applet.                                                                                                                                                                                                                                                                                                             |
| Guid              | This is a calculated field that is automatically generated for the applet.                                                                                                                                                                                                                                                                        |

