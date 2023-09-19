# CinchyDXD

## Overview

CinchyDXD is a utility that will export a Data Experience Definition from a source Cinchy environment and create a complete release package that can:

- Deploy to an upper environment with the same CinchyDXD utility.
- Committed to source control to reflect a snapshot of a specific point in the release version.

## Prerequisites

CinchyDXD 1.5.0 and later requires Cinchy v4.21.0+ with the Connections UI installed.

For a full list of requirements, see the [Dependencies](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/README.md#dependencies) section below.
## Download CinchyDXD

{% hint style="info" %}
You only need to download CinchyDXD on the source environment; it's not needed on the destination.
{% endhint %}

To download the Utility:

1. Login to Cinchy.
2. Navigate to the **Releases Table**.
3. Select the **Experience Deployment Utility View**.
4. Locate and download the utility **(Cinchy DXD vX.X.X.zip)**.
5. Unzip the utility and place the folder at any location on a computer that also has CinchyCLI installed.
6. Create a new folder in the same directory that will hold all DX exports generated (CinchyDXD\*Output).


## Using CinchyDXD

1. Unzip CinchyDXD and place the folder at any location.
2. From a PowerShell console, navigate to the CinchyDXD directory.
3. For first time users, execute this command:
    - `Get-ChildItem -Recurse * | Unblock-File`
4. For a list of actions and parameters execute one of the following at the PowerShell command prompt:
    - `.\CinchyDXD version`
    - `.\CinchyDXD help`
    - `.\CinchyDXD export`
    - `.\CinchyDXD install`
    - `.\CinchyDXD keygen`

## Dependencies

### Cinchy

| Cinchy Platform Version | Cinchy DXD Version |
| ----------------------- | ------------------ |
| v5.7.0 or later         | v1.7.0 or later    |
| v5.2.0 - v5.6.3         | v1.5.1 - v1.6.1    |
| v4.21.0 - v5.1.5 **     | v1.4.0 - v1.5.0    |
| v4.20.0 - v4.20.2       | v1.3.2 - v1.3.4    |
| v4.17.0 - v4.19.2       | v1.3.0 - v1.3.1    |
| v4.13.2 - v4.16.0       | v1.1.0 - v1.2.2    |
| v4.13.0 - v4.13.1       | Not supported      |
| v4.6.0  - v4.12.0       | v1.1.0 - v1.2.0    |
| v4.3.0  - v4.5.1        | v1.0.0             |


{% hint style="info" %}
- When deploying from Cinchy v4 to Cinchy v5, use CinchyDXD v1.5.0 to generate the source package.
- For using CinchyDXD 1.5.0+ with Cinchy v4.21.0+ environments, ensure the Cinchy Connections Applet Sync GUID is set to 98055dec-e314-421a-a9da-456ae8308c60 in the [Cinchy].[Applets] table.
{% endhint %}


### Cinchy CLI and Connections CLI

| CLI Type and Version           | Cinchy DXD Version |
| ------------------------------ | ------------------ |
| Not required                   | v1.5.0 or later    |
| Connections CLIv5.0.0 or later | v1.4.0             |
| CinchyCLI v3.7.0 or later      | v1.0.0 - v1.4.0    |


### PowerShell

| PowerShell Version | Cinchy DXD Version |
| ------------------ | ------------------ |
| 7 or later         | v1.4.0 or later    |
| 5.0.1 or later     | v1.0.0 - v1.3.4    |


### SQL Server 

| SQL Server Version | Cinchy DXD Version |
| ------------------ | ------------------ |
| 2017 or later      | v1.4.0 or later    |
| 2012 - 2016        | v1.0.0 - v1.3.4    |

### PostgreSQL

| PostgreSQL Version | Cinchy DXD Version |
| ------------------ | ------------------ |
| 14.3 or later      | v1.5.0 or later    |

## Version history

See the [Version history](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/version-history-dxd.md) for more details.

## Next steps



