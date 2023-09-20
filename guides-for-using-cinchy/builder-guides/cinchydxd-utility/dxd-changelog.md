## CinchyDXD changelog

### Version 2.0.0

- NEW: Cinchy DXD now synchronizes into a repository folder, allowing for syncing with your local git repository
- NEW: Cinchy DXD artifacts are now stored as individual files in a fixed structure, suitable for Git diff activities
- NEW: Cinchy DXD no longer adds PowerShell scripts to the install package folder, instead, Cinchy DXD must be used for all operations
- NEW: Provided support for forwarding Sync GUIDs from an origin environments, if they are populated

### Version 1.7.0

- NEW: Data Experience Definitions and CinchyDXD now facilitate the syncing of:
  - `Secrets` - Note that `External Secrets Manager` are implicitly synced when referenced in `Secrets`
  - `Listener Configurations`
  - `Webhooks`
  - `Views` - `Views` are no longer synced by default for `Tables`, and must be explicitly referenced in the `Data Experience Definition`
- NEW: `Pre-Install Scripts` and `Post-Install Scripts` are now specified within the `Data Experience Definition`
  - Pre|Post-Install scripts are saved automatically with the `Export` command.
  - Pre|Post-Install Scripts are run automatically with the `Install` command.
  - Pre|Post-Install Scripts will be executed in alphabetical order
  - The command line options `-ps` and `-pr` have been removed from CinchyDXD as a result of this new enhancement
- NEW: Cinchy DXD automatically skips syncing metadata not in the definition, and when the target doesn't already contain metadata from a previous version

### Version 1.6.2

- FIXED: Corrected an issue with the model validation logic to properly check for link display columns that are more than two table hops away.

### Version 1.6.1

- NEW: `Groups` now sync in two stages to account for parent/child references
- NEW: Added pre-export error messaging for common `Data Experience Definition` configuration issues
- FIXED: Optimization of linkColumn references in all syncs to batter support syncing references to Domains, Tables, and Columns with & in their names
- FIXED: Optimizations of linkColumns throughout to use GUID and Sync GUID references where applicable, improving sync accuracy and reducing sync errors
- FIXED: Data Experience Reference Data that have Binary data type columns would fail to sync. Binary columns are now excluded from Reference Data configurations

### Version 1.6.0

- NEW: Introducing the `-skip` parameter for `./CinchyDXD.ps1 install`. This parameter allows you to specify a list, separated by commas, of:
  - `Table` names to skip syncing
  - `Table.Column` names to skip syncing
  - Example: `-skip "System Colours, Groups.User Groups, Groups.Owner Groups"`
    - This will completely skip the synchronization of the `System Colours` table and exclude the `Groups.User Groups` and `Groups.Owner Groups` columns when syncing the `Groups` table.
    - If a `Table` is specified that's the same as a `Table.Column`, the sync for that `Table` won't be executed, and the `Table.Column` directive will be ignored.
- NEW: During the export process, DXD will now notify you if there are tables that need to be opened and saved before exporting the DXD. This requirement may arise if a Display Value of a Linked Column has been renamed in the Linked Table.
  - Currently, this check only works with TSQL.
- NEW: During the installation process, if you specify the -y flag (force install), DXD will remove any existing [Cinchy].[Models] record for this DXD, if it already exists.
- NEW: Encryption (optional). Cinchy DXD now supports encrypting the contents of exported Data Experience Definitions. To utilize this feature:
  - Use the `./CinchyDXD.ps1 keygen` command to generate an encryption key file.
  - For `./CinchyDXD.ps1 export|install`, use the -k property to specify the location of the encryption key file.
  - The encryption key used for exporting a DXD package must be the same as the encryption key used during the installation process. To prevent any issues, ensure not to misplace the key file and store it separately in a secure location from the exported DXD package.
- NEW: Support for optionally executing pre-install CQL scripts.
- FIXED: Fixed an issue where Views modified in the Install destination outside of DXD could result in duplicate column entries. A check and fix have been implemented to address this.
- FIXED: Changed the loading order of System Colours to load before Domains, as Domains depend on System Colours.

### Version 1.5.1

- CinchyDXD now requires Cinchy installed on SQL Server 2017 or PostgreSQL 14.3

### Version 1.5.0

- Cinchy DXD no longer requires Cinchy Connections CLI
- Note the following switch updates:
  -h    REMOVED     http/https is now auto-detected
  -sso  REMOVED     SSO/IDP is now auto-detected
  -c    REMOVED     Connections CLI is no longer used
  -t    REMOVED     No longer used
  -e    REPURPOSED  For Install - Pause on Errors
  -z    REPURPOSED  For Install - Connections Output to log and console
  -m    NEW         For Install - Ignore Model Errors on Install
- Install - Support for Pause on Errors
  - Default behaviour is to pause on ModelLoader errors. Use the -m switch to override
  - When the optional -e switch is used, DXD will pause on Connections sync errors
  - When paused, options to Retry, Ignore, Ignore All, and Exit are available
  - Before selecting Retry, correct Model or Connnection Sync errors first
- Install - When -d is provided, DXD will output Connections Sync Error files to this location

### Version 1.4.0

- Support for Cinchy v5
- Support for `Cinchy.Connections.CLI` v5
- Now initializes Views data in source and target environments
- Now removes DXDF CLIs after deployment is complete
- Fixed an issue where passwords containing $ would not properly encrypt
- Cross-platform support added (Windows, MacOS, Linux) with PowerShell 7+
- Windows support for PowerShell 5.1 or 7+

### Version 1.3.4

- Added encrypt command
  - For encrypting passwords that will be used with CinchyDXD
  - Usage: `.\CinchyDXD.ps1 encrypt -t "TextToBeEncrypted"`

### Version 1.3.3

- Corrected issue with Reference data

### Version 1.3.2

- This version must be used for deployments to Cinchy v4.20.x and above
- **IMPORTANT PATCH INSTRUCTIONS FOR OLDER DX PACKAGES**
- To install a DXD in 4.20.x that was previously exported from a version 4.19.x or earlier, you must do the following:
  - replace the following files in your exported data experience with the corresponding files from this release
  - CinchyDXD\CinchyDXD.ps1             -> replaces   -> ExportedDX\CinchyDXD.ps1
  - CinchyDXD\scripts\ps\_templates.ps1 -> replaces   -> ExportedDX\scripts\_templates.ps1
  - CinchyDXD\scripts\ps\push.ps1       -> new file   -> ExportedDX\scripts\push.ps1
  - CinchyDxd\scripts\clipatch          -> new folder -> ExportedDX\clipatch
- Note that the patch instructions above will by default insert or update any in scope Data Sync Configurations with [Admin Groups] = 'All Users'

### Version 1.3.1

- Version differences during an install are now displayed as a warning only and won't prevent as install
- Corrected an issue with character encoding in the model file generation

### Version 1.3.0

- Added support for Post Install Scripts
  - Specify a path with the install command. All CQL scripts in this folder will be executed after the install
- Added support for Data Experience Reference Data Target Filter
  - This will add a CinchyTableTarget filter to the Reference Data CLI configuration
- Fixed an issue that returned an error when no tables were defined in the DXD
- Fixed an issue that prevented syncing when Domain, Table and Column names contained &

### Version 1.2.2

- Compatibility update with Model Loader changes in Cinchy v4.16.0

### Version 1.2.1

- Updated REFDATA Data Sync Configurations to set Linked column source dataTypes to Text

### Version 1.2.0

- Added native ModelLoader support for:
  - Dropping Columns
  - Updating Column Information
  - Renaming Tables and Creating/Updating Domain Assignments
- Cinchy DXD will no longer allow an install when the source and target Cinchy versions aren't the same
- Fixed an issue where Domains or Tables with & would prevent REFDATA from loading

### Version 1.1.4

- Fixed an issue that was causing Reference Data to load out of order

### Version 1.1.3

- Fixed an issue that was creating duplicate rows in Table Access Control

### Version 1.1.2

- Fixed an issue introduced in 1.1.1 where REFDATA CLIs weren't assigning the correct dataType to Date columns

### Version 1.1.1

- Fixed an issue where DXD was attempting to update authorized users in Applets, Saved Queries, Views, and Table Access Control

### Version 1.1.0

- DXDs now delete deprecated configuration data during a target install. Previously, configuration data was inserted or updated only
- Force Install.sql is now more comprehensive and removes all traces of data tied to a previous install. Be careful using this feature
- Fixed a couple of bugs in syncing of Formatting Rules, Table Access Controls, and Views
- Views sync issue resolved with the Cinchy 4.6.0 dependency

### Version 1.0.0

- First official release.

### Known issues in version 1.0.0

If using `CinchyCLI` version 4 and above, `[Cinchy].[Views]`, `[Cinchy].[View Columns]`, and `[Cinchy].[View Columns Link Graph]` won't synchronize unless the `Mandatory` attribute of the `[Json]` column in the `[Cinchy].[Views]` table is set to `False`. This issue isn't present in `CinchyCLI` version 3.7.0 and earlier.

### Version 0.9.1

Pre-release sent for beta testing.

### Known issues in version 0.9.1

This version is unsupported, please use version 1.0.0 or later of CinchyDXD.
