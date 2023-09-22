# CinchyDXD Release Notes

## Version 2.0.0

- **Git Sync**: Cinchy DXD can now sync into a repository folder, compatible with local git repositories.
- **Artifact Structure**: Individual files stored in a standardized format for easier Git diffing.
- **PowerShell Removal**: No longer includes PowerShell scripts in install packages. Use Cinchy DXD for all operations.
- **Sync GUID Support**: Allows forwarding of Sync GUIDs from origin environments.

## Version 1.7.0

- **Enhanced Data Sync**: Now supports syncing of Secrets, Listener Configurations, Webhooks, and Views.
    - **Note**: Views must be explicitly defined; Secrets automatically include External Secrets Manager.
- **Script Management**: Define Pre-Install and Post-Install Scripts in Data Experience Definition.
    - **Auto-run**: Scripts run automatically during Install.
    - **Order**: Scripts executed alphabetically.
    - **Deprecated**: `-ps` and `-pr` command line options removed.
- **Metadata Skip**: Automatically omits non-defined or obsolete metadata during sync.

## Version 1.6.2

- **Fixed**: Resolved validation logic issue for link display columns with more than two table hops.

## Version 1.6.1

- **Group Sync**: Now occurs in two stages to manage parent/child references.
- **Pre-Export Checks**: Error messages for common configuration issues.
- **Optimized Sync**: Improved accuracy and reduced errors in `linkColumn` references.
- **Binary Column Fix**: Binary columns now excluded from Reference Data configurations.

## Version 1.6.0

- **Skip Parameter**: Use `-skip` to exclude specific Tables or Columns during sync.
- **Export Notifications**: Alerts for tables needing pre-export action. TSQL only.
- **Force Install**: Use `-y` flag to overwrite existing Models records.
- **Optional Encryption**: New support for encrypted Data Experience Definitions.
    - **Key Generation**: Use `keygen` command to create an encryption key.
- **Pre-Install Scripts**: Optional CQL script execution.
- **Fixes**: Resolved issues with Views and System Colours loading order.


## Version 1.5.1

- **System Requirements**: Now requires SQL Server 2017 or PostgreSQL 14.3 for Cinchy installation.

## Version 1.5.0

- **CLI Requirements**: Removed the need for Cinchy Connections CLI.
- **Switch Updates**: Various command line switches removed or repurposed:
    - `-h`, `-sso`, `-c`, `-t` are removed.
    - `-e` and `-z` are repurposed for install errors and log outputs.
    - `-m` is new for ignoring model errors during install.
- **Error Handling**: Provides options to pause and handle errors during installation. Use `-m` to override default behavior.
- **Error Output**: Use `-d` to specify location for Connections Sync Error files.

## Version 1.4.0

- **Version Support**: Compatibility with Cinchy v5 and `Cinchy.Connections.CLI` v5.
- **View Initialization**: Initializes Views data in both source and target environments.
- **Deployment Cleanup**: Removes DXDF CLIs post-deployment.
- **Encryption Fix**: Resolves password encryption issue for passwords with `$`.
- **Cross-Platform**: Adds support for Windows, MacOS, Linux with PowerShell 7+.

## Version 1.3.4

- **Encrypt Command**: New command for encrypting passwords. Usage: `.\CinchyDXD.ps1 encrypt -t "TextToBeEncrypted"`.

## Version 1.3.3

- **Fix**: Resolves issue with Reference data.

## Version 1.3.2

- **Compatibility**: Must be used for deployments to Cinchy v4.20.x and above.
- **Patch Instructions**: Specific steps for updating older DX packages for compatibility.

## Version 1.3.1

- **Install Warnings**: Version differences during install are now warnings, not errors.
- **Encoding Fix**: Resolves character encoding issue in model file generation.

## Version 1.3.0

- **Post-Install Scripts**: New support for running CQL scripts post-install.
- **Reference Data Filter**: Adds a new filter option for Reference Data CLI configuration.
- **Table Sync Fix**: Resolves error when no tables are defined in the DXD.
- **Sync Fix**: Resolves sync issue for names containing `&`.


## Version 1.2.2

- **Compatibility**: Updated for Model Loader changes in Cinchy v4.16.0.

## Version 1.2.1

- **REFDATA Sync**: Linked column source `dataTypes` now set to `Text`.

## Version 1.2.0

- **ModelLoader Enhancements**: Supports column drops, updates, and table/domain renaming.
- **Version Matching**: Installation only allowed when source and target Cinchy versions match.
- **Special Character Fix**: Resolved REFDATA loading issue with Domains or Tables containing `&`.

## Version 1.1.4

- **Order Fix**: Resolved issue causing Reference Data to load out of order.

## Version 1.1.3

- **Duplicate Row Fix**: Fixed issue creating duplicate rows in Table Access Control.

## Version 1.1.2

- **DataType Fix**: Corrected REFDATA CLIs not setting correct dataType for Date columns.

## Version 1.1.1

- **Authorization Fix**: Resolved issue where DXD was updating unauthorized users in various elements.

## Version 1.1.0

- **Cleanup**: Deletes deprecated config data during target install.
- **Force Install**: More comprehensive SQL script for complete data removal.
- **Bug Fixes**: Resolved issues in Formatting Rules, Table Access Controls, and Views sync.
- **Dependency**: Views sync issue fixed with Cinchy 4.6.0.

## Version 1.0.0

- **Initial Release**: First official version.

### Known issues in version 1.0.0

- **CinchyCLI Compatibility**: Specific attribute setting needed for proper synchronization with CinchyCLI version 4 and above.

## Version 0.9.1

- **Beta Release**: Pre-release for beta testing.

### Known issues in version 0.9.1

- **Unsupported**: Use version 1.0.0 or later for supported functionality.

