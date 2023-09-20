# CinchyDXD

## Overview

CinchyDXD is a utility that exports a data experiences from a source Cinchy environment to a release package that can:

- Deploy to an upper environment with the same CinchyDXD utility.
- Committed to source control to reflect a snapshot of a specific point in the release version.

With CinchyDXD 2.0, you can also now work on a release package in a version controlled environment (such as Git) so multiple developers can work on a package.

## Prerequisites

CinchyDXD 1.5.0 and later requires Cinchy v4.21.0+ with the Connections UI installed.

For a full list of requirements, see the [Dependencies](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/dependencies.md) page for more information.
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
2. From a PowerShell Administrator console, navigate to the CinchyDXD directory.
3. For first time users, execute this command:
    - `Get-ChildItem -Recurse * | Unblock-File`
4. For a list of actions and parameters execute one of the following at the PowerShell command prompt:
    - `.\CinchyDXD version`
    - `.\CinchyDXD help`
    - `.\CinchyDXD export`
    - `.\CinchyDXD install`
    - `.\CinchyDXD keygen`

## Changelog

See the [Changelog](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/version-history-dxd.md) for more details.

## Next steps

- Review the [DXD workflow]()
- Try the [DXD tutorial: Currency exchange]()


