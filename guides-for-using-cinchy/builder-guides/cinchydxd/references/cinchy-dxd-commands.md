# CinchyDXD CLI reference

This section lists all available commands and their arguments for CinchyDXD.

## CinchyDXD 2.0 DXD functions

| Action  | Description                                                                    |
| ------- | ------------------------------------------------------------------------------ |
| export  | Exports a Data Experience from a Cinchy source environment.                    |
| install | Installs a Data Experience in a Cinchy target environment.                     |
| keygen  | Generates an encryption key file.                                              |
| help    | Displays this help text. To display help for an action, pass only that action. |
| version | Displays version information.                                                  |

## Common arguments

| Option | Description                                              | Requirement |
| ------ | -------------------------------------------------------- | ----------- |
| `-s`   | Full path to the Cinchy server without the protocol.     | Required    |
| `-u`   | User ID for accessing Cinchy.                            | Required    |
| `-p`   | Clear text or encrypted password for the specified user. | Required    |
| `-t`   | Path to directory for storing temporary DXD files.       | Required    |

## Export

**Command**: `.\CinchyDXD.ps1 export`

| Option   | Description                                              | Requirement |
| -------- | -------------------------------------------------------- | ----------- |
| `-g`     | The Data Experience Definition GUID.                     | Required    |
| `-o`     | Path to directory for Data Experience release export.    | Required    |
| `-k`     | Path to the encryption key file for metadata encryption. | Optional    |
| `-c`     | Flag to keep the temp (work) directory when complete.    | Optional    |
| `-start` | The Export Step to start from.                           | Optional    |
| `-end`   | The Export Step to end on.                               | Optional    |

<!-- vale off -->
## Install

**Command**: `.\CinchyDXD.ps1 install`

| Option   | Description                                                | Requirement |
| -------- | ---------------------------------------------------------- | ----------- |
| `-x`     | Path to directory containing the DX to be installed.       | Required    |
| `-v`     | Data Experience Definition Version.                        | Required    |
| `-d`     | Path to directory for storing temporary Connection files.  | Optional    |
| `-e`     | Prompt to continue on Connection and Model Loader errors.  | Optional    |
| `-m`     | Do not stop on Model Loader errors.                        | Optional    |
| `-z`     | Display Connections output on completion.                  | Optional    |
| `-k`     | Path to the encryption key file for metadata decryption.   | Optional    |
| `-skip`  | List of Tables and/or Table.Column(s) to skip during sync. | Optional    |
| `-c`     | Flag to keep the temp (work) directory when complete.      | Optional    |
| `-start` | The Install Step to start from.                            | Optional    |
| `-end`   | The Install Step to end on.                                | Optional    |
| `-y`     | Force install if release already exists in the target.     | Optional    |

## CinchyDXD 1.7 commands

### Actions

| Action    | Description                                                      |
|-----------|------------------------------------------------------------------|
| `export`  | Exports a Data Experience from a Cinchy source environment.      |
| `install` | Installs a Data Experience in a Cinchy target environment.       |
| `keygen`  | Generates an encryption key file.                                |
| `help`    | Displays this help text. To display help for an action, pass only that action. |
| `version` | Displays version information.                                    |

### Common arguments

| Parameter             | Description                                               | Requirement |
|-----------------------|-----------------------------------------------------------|-------------|
| `-s (Cinchy Server)`  | Full path to the Cinchy server without the protocol.      | Required    |

### Export arguments

| Parameter                   | Description                                                       | Requirement |
|-----------------------------|-------------------------------------------------------------------|-------------|
| `-u (Username)`             | User ID for accessing Cinchy. Required when not using a Personal Access Token. | Optional    |
| `-p (Password)`             | Clear text password or Personal Access Token. If using a token, do not provide `-u`. | Required    |
| `-g (DXD Guid)`             | The Data Experience Definition Guid.                              | Required    |
| `-v (DXD Version)`          | The Data Experience Definition Version.                           | Required    |
| `-o (DXD Output Directory)` | Path where CinchyDXD will create the Data Experience release export. | Required    |
| `-k (Encryption Key File)`  | Path to the encryption key file for encrypting metadata.          | Optional    |
| `-w (Write Binary Files to Cinchy)` | Flag to save release files into Cinchy.                    | Optional    |
| `-x (CinchyDXD Temp Directory)` | Flag to prevent cleanup of the output temp directory.          | Optional    |
| `-start (Start Step)`       | Export Step to start from.                                        | Optional    |
| `-end (End Step)`           | Export Step to end on.                                            | Optional    |

### Install arguments

| Parameter                      | Description                                                       | Requirement |
|--------------------------------|-------------------------------------------------------------------|-------------|
| `-u (Username)`                | User ID for accessing Cinchy.                                     | Required    |
| `-p (Password)`                | Clear text password of the specified user.                        | Required    |
| `-d (Connections Temp Directory)` | Path for storing temporary files to support sync.             | Optional    |
| `-e (Pause On Errors)`         | Prompt to continue on Connection and Model Loader errors.         | Optional    |
| `-m (Ignore Model Errors)`     | Do not stop on ModelLoader errors.                                | Optional    |
| `-z (Display Sync Output)`     | Displays the Connections output on completion.                     | Optional    |
| `-k (Encryption Key File)`     | Path to the encryption key file for decrypting metadata.          | Optional    |
| `-skip (Skip Sync)`            | Comma delimited list of Tables and/or Table.Column(s) to skip.    | Optional    |
| `-start (Start Step)`          | Install Step to start from.                                       | Optional    |
| `-end (End Step)`              | Install Step to end on.                                           | Optional    |
| `-y (Force Install)`           | Force install if release already exists in target.                 | Optional    |

### Keygen arguments

| Parameter                     | Description                                                       | Requirement |
|-------------------------------|-------------------------------------------------------------------|-------------|
| `-o (DXD Output Directory)`   | Path where the encryption key file will be created. Used for encrypting and decrypting metadata during DXD Export and Import. | Required    |


## CinchyDXD 1.6 commands

### Actions

| Action    | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| `export`  | Exports a Data Experience from a Cinchy source environment.                    |
| `install` | Installs a Data Experience in a Cinchy target environment.                     |
| `keygen`  | Generates an encryption key file.                                              |
| `help`    | Displays this help text. To display help for an action, pass only that action. |
| `version` | Displays version information.                                                  |

### Common arguments

| Parameter            | Description                                          | Requirement |
| -------------------- | ---------------------------------------------------- | ----------- |
| `-s (Cinchy Server)` | Full path to the Cinchy server without the protocol. | Required    |

### Export arguments

| Parameter                           | Description                                                                          | Requirement |
| ----------------------------------- | ------------------------------------------------------------------------------------ | ----------- |
| `-u (Username)`                     | User ID for accessing Cinchy. Required when not using a Personal Access Token.       | Optional    |
| `-p (Password)`                     | Clear text password or Personal Access Token. If using a token, do not provide `-u`. | Required    |
| `-g (DXD Guid)`                     | The Data Experience Definition Guid.                                                 | Required    |
| `-v (DXD Version)`                  | The Data Experience Definition Version.                                              | Required    |
| `-o (DXD Output Directory)`         | Path where CinchyDXD will create the Data Experience release export.                 | Required    |
| `-k (Encryption Key File)`          | Path to the encryption key file for encrypting metadata.                             | Optional    |
| `-w (Write Binary Files to Cinchy)` | Flag to save release files into Cinchy.                                              | Optional    |
| `-x (CinchyDXD Temp Directory)`     | Flag to prevent cleanup of the output temp directory.                                | Optional    |
| `-start (Start Step)`               | Export Step to start from.                                                           | Optional    |
| `-end (End Step)`                   | Export Step to end on.                                                               | Optional    |

### Install arguments

| Parameter                         | Description                                                    | Requirement |
| --------------------------------- | -------------------------------------------------------------- | ----------- |
| `-u (Username)`                   | User ID for accessing Cinchy.                                  | Required    |
| `-p (Password)`                   | Clear text password of the specified user.                     | Required    |
| `-d (Connections Temp Directory)` | Path for storing temporary files to support sync.              | Optional    |
| `-e (Pause On Errors)`            | Prompt to continue on Connection and Model Loader errors.      | Optional    |
| `-m (Ignore Model Errors)`        | Do not stop on ModelLoader errors.                             | Optional    |
| `-z (Display Sync Output)`        | Displays the Connections output on completion.                 | Optional    |
| `-k (Encryption Key File)`        | Path to the encryption key file for decrypting metadata.       | Optional    |
| `-ps (Post Install Scripts)`      | Path to directory with CQL scripts to execute post-install.    | Optional    |
| `-pr (Pre Install Scripts)`       | Path to directory with CQL scripts to execute pre-install.     | Optional    |
| `-skip (Skip Sync)`               | Comma delimited list of Tables and/or Table.Column(s) to skip. | Optional    |
| `-start (Start Step)`             | Install Step to start from.                                    | Optional    |
| `-end (End Step)`                 | Install Step to end on.                                        | Optional    |
| `-y (Force Install)`              | Force install if release already exists in target.             | Optional    |

### Keygen arguments

| Parameter                   | Description                                                                                                                   | Requirement |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `-o (DXD Output Directory)` | Path where the encryption key file will be created. Used for encrypting and decrypting metadata during DXD Export and Import. | Required    |

## CinchyDXD 1.5 commands

### Actions

| Action    | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| `export`  | Exports a Data Experience from a Cinchy source environment.                    |
| `install` | Installs a Data Experience in a Cinchy target environment.                     |
| `help`    | Displays this help text. To display help for an action, pass only that action. |
| `version` | Displays version information.                                                  |

### Common arguments

| Parameter            | Description                                             | Requirement |
| -------------------- | ------------------------------------------------------- | ----------- |
| `-s (Cinchy Server)` | Full path to the Cinchy server without the protocol.    | Required    |
| `-u (Username)`      | User ID for accessing Cinchy.                           | Required    |
| `-p (Password)`      | Clear text or encrypted password of the specified user. | Required    |

### Export

| Parameter                           | Description                                                          | Requirement |
| ----------------------------------- | -------------------------------------------------------------------- | ----------- |
| `-g (DXD Guid)`                     | The Data Experience Definition Guid.                                 | Required    |
| `-v (DXD Version)`                  | The Data Experience Definition Version.                              | Required    |
| `-o (DXD Output Directory)`         | Path where CinchyDXD will create the Data Experience release export. | Required    |
| `-w (Write Binary Files to Cinchy)` | Flag to save release files into Cinchy.                              | Optional    |
| `-x (CinchyDXD Temp Directory)`     | Flag to prevent cleanup of the output temp directory.                | Optional    |
| `-start (Start Step)`               | Export Step to start from.                                           | Optional    |
| `-end (End Step)`                   | Export Step to end on.                                               | Optional    |

### Install

| Parameter                         | Description                                                 | Requirement |
| --------------------------------- | ----------------------------------------------------------- | ----------- |
| `-d (Connections Temp Directory)` | Path for storing temporary files to support sync.           | Optional    |
| `-e (Pause On Errors)`            | Prompt to continue on Connection and Model Loader errors.   | Optional    |
| `-m (Ignore Model Errors)`        | Do not stop on ModelLoader errors.                          | Optional    |
| `-z (Display Sync Output)`        | Displays the Connections output on completion.              | Optional    |
| `-ps (Post Install Scripts)`      | Path to directory with CQL scripts to execute post-install. | Optional    |
| `-start (Start Step)`             | Install Step to start from.                                 | Optional    |
| `-end (End Step)`                 | Install Step to end on.                                     | Optional    |
| `-y (Force Install)`              | Force install if release already exists in target.          | Optional    |

## CinchyDXD 1.4 commands

### Actions

| Action    | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| `export`  | Exports a Data Experience from a Cinchy source environment.                    |
| `install` | Installs a Data Experience in a Cinchy target environment.                     |
| `encrypt` | Generate an encrypted string.                                                  |
| `help`    | Displays this help text. To display help for an action, pass only that action. |
| `version` | Displays version information.                                                  |

### Common arguments

### Common

| Parameter                           | Description                                                            | Requirement |
| ----------------------------------- | ---------------------------------------------------------------------- | ----------- |
| `export`                            | Exports a Data Experience from a Cinchy source environment.            | Required    |
| `-h (https)`                        | Flag for https connections to Cinchy.                                  | Optional    |
| `-s (Cinchy Server)`                | Full path to the Cinchy server without the protocol.                   | Required    |
| `-sso (CinchySSO Server)`           | Full path to the CinchySSO server without the protocol.                | Optional    |
| `-u (Username)`                     | User ID for accessing Cinchy.                                          | Required    |
| `-p (Password)`                     | Clear text or encrypted password of the specified user.                | Required    |
| `-c (CinchyCLI Directory)`          | Path to the Cinchy CLI.                                                | Required    |
| `-d (CinchyCLI Temp Directory)`     | Path for storing temporary files.                                      | Required    |
| `-e (CinchyCLI EXE)`                | Flag to use `Cinchy.CLI.exe`. `Cinchy.CLI.dll` is the default.         | Optional    |
| `-g (DXD Guid)`                     | Data Experience Definition Guid.                                       | Required    |
| `-v (DXD Version)`                  | Data Experience Definition Version.                                    | Required    |
| `-o (DXD Output Directory)`         | Path where `CinchyDXD` will create the Data Experience release export. | Required    |
| `-w (Write Binary Files to Cinchy)` | Flag to save release files into Cinchy.                                | Optional    |
| `-x (CinchyDXD Temp Directory)`     | Flag to prevent cleanup of the output temp directory.                  | Optional    |
| `-start (Start Step)`               | Export Step to start from.                                             | Optional    |
| `-end (End Step)`                   | Export Step to end on.                                                 | Optional    |

### Export

| Parameter                           | Description                                                          | Requirement |
| ----------------------------------- | -------------------------------------------------------------------- | ----------- |
| `-g (DXD Guid)`                     | The Data Experience Definition Guid.                                 | Required    |
| `-v (DXD Version)`                  | The Data Experience Definition Version.                              | Required    |
| `-o (DXD Output Directory)`         | Path where CinchyDXD will create the Data Experience release export. | Required    |
| `-w (Write Binary Files to Cinchy)` | Flag to save release files into Cinchy.                              | Optional    |
| `-x (CinchyDXD Temp Directory)`     | Flag to prevent cleanup of the output temp directory.                | Optional    |
| `-start (Start Step)`               | Export Step to start from.                                           | Optional    |
| `-end (End Step)`                   | Export Step to end on.                                               | Optional    |

### Install

| Parameter                    | Description                                                 | Requirement |
| ---------------------------- | ----------------------------------------------------------- | ----------- |
| `-ps (Post Install Scripts)` | Path to directory with CQL scripts to execute post-install. | Optional    |
| `-start (Start Step)`        | Install Step to start from.                                 | Optional    |
| `-end (End Step)`            | Install Step to end on.                                     | Optional    |
| `-y (Force Install)`         | Force install if release already exists in target.          | Optional    |
| `-z (Log syncdata)`          | Write CLI syncdata commands to the log.                     | Optional    |

<!-- vale on -->

