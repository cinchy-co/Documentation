# CinchyDXD command reference

This section lists all available commands and their arguments for CinchyDXD.

## Cinchy DXD functions

| Action  | Description                                                               |
|---------|---------------------------------------------------------------------------|
| export  | Exports a Data Experience from a Cinchy source environment.                |
| install | Installs a Data Experience in a Cinchy target environment.                 |
| keygen  | Generates an encryption key file.                                         |
| help    | Displays this help text. To display help for an action, pass only that action. |
| version | Displays version information.                                             |


## Common arguments

| Option   | Description                                                  | Requirement |
| -------- | ------------------------------------------------------------ | ----------- |
| `-s`     | Full path to the Cinchy server without the protocol.         | Required    |
| `-u`     | User ID for accessing Cinchy.                                | Required    |
| `-p`     | Clear text or encrypted password for the specified user.     | Required    |
| `-t`     | Path to directory for storing temporary DXD files.           | Required    |

## CinchyDXD Export

**Command**: `.\CinchyDXD.ps1 export`

### Arguments

| Option   | Description                                              | Requirement |
| -------- | -------------------------------------------------------- | ----------- |
| `-g`     | The Data Experience Definition GUID.                     | Required    |
| `-o`     | Path to directory for Data Experience release export.    | Required    |
| `-k`     | Path to the encryption key file for metadata encryption. | Optional    |
| `-c`     | Flag to keep the temp (work) directory when complete.    | Optional    |
| `-start` | The Export Step to start from.                           | Optional    |
| `-end`   | The Export Step to end on.                               | Optional    |

<!-- vale off -->
## CinchyDXD Install

**Command**: `.\CinchyDXD.ps1 install`

| Option   | Description                                                  | Requirement |
| -------- | ------------------------------------------------------------ | ----------- |
| `-x`     | Path to directory containing the DX to be installed.         | Required    |
| `-v`     | Data Experience Definition Version.                           | Required    |
| `-d`     | Path to directory for storing temporary Connection files.     | Optional    |
| `-e`     | Prompt to continue on Connection and Model Loader errors.     | Optional    |
| `-m`     | Do not stop on Model Loader errors.                           | Optional    |
| `-z`     | Display Connections output on completion.                     | Optional    |
| `-k`     | Path to the encryption key file for metadata decryption.      | Optional    |
| `-skip`  | List of Tables and/or Table.Column(s) to skip during sync.    | Optional    |
| `-c`     | Flag to keep the temp (work) directory when complete.         | Optional    |
| `-start` | The Install Step to start from.                               | Optional    |
| `-end`   | The Install Step to end on.                                   | Optional    |
| `-y`     | Force install if release already exists in the target.         | Optional    |

<!-- vale on -->