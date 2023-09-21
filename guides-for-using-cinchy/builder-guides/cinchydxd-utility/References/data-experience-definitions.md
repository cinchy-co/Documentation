## Data Experience Definitions Table

The list below is a reference to all fields in the Data Experience Definitions Table.

| Column                   | Definition                                                                                            |
| ------------------------ | ----------------------------------------------------------------------------------------------------- |
| GUID                     | Calculated value. Required for PowerShell exports.                                                    |
| Name                     | Name of your Data Experience.                                                                         |
| Tables                   | Tables included in the Data Experience.                                                               |
| Views                    | Views from the data browser included.                                                                 |
| Integrated Clients       | Integrated clients like Tableau, PowerBI.                                                             |
| Data Sync Configurations | Data syncs included. CLI compatibility required.                                                      |
| Listener Configurations  | Listener Config rows referring to Data Syncs.                                                         |
| Reference Data           | Reference data setup in the Data Experience Reference Data table.                                     |
| Secrets                  | Secrets used in Data Syncs or Listener Configs.                                                       |
| Webhooks                 | Webhooks included.                                                                                    |
| User Defined Functions   | User Defined Functions to include. For example, phone or email validation.                            |
| Models                   | Custom models that override columns or tables. Leave blank if none.                                   |
| Groups                   | Groups included. Also moves table access controls.                                                    |
| System Colours           | System color for the data experience, if defined.                                                         |
| Saved Queries            | Queries included.                                                                                     |
| Applets                  | Applets included.                                                                                     |
| Pre-install Scripts      | Scripts to run before installation.                                                                   |
| Post-install Scripts     | Scripts to run after installation. For data rectification between environments.                       |
| Formatting Rules         | Formatting rules included.                                                                            |
| Literal Groups           | Literals like key values in multiple languages.                                                       |
| Builders                 | Builders with export permission.                                                                      |
| Builder Groups           | Builder groups with export permission. Group preferred over individual users for ease of maintenance. |
| Sync GUID                | Leave blank.                                                                                          |
