# CinchyDXD

## CinchyDXD introduction



CinchyDXD is a utility that helps developers manage changes to their solutions built on Cinchy in a controlled and scalable way by enabling source-driven development and team collaboration with governance.

With DXD, you can:

- Manage Cinchy objects related to a particular solution/data product (also known as a Data Experience) as code. This code can be exported from an existing Cinchy environment used for development.
- Leverage existing software development best practices around change management using source-control
- Deploy the Data Experience into other Cinchy environments.

## About CinchyDXD 2.0

CinchyDXD 2.0 creates packages for further development in a version controlled environment (such as Git) where multiple developers can work on the same data experience.

## Prerequisites

CinchyDXD 1.5.0 and later requires Cinchy v4.21.0+ with the Connections UI installed.

For a full list of requirements, see the [Dependencies](/guides-for-using-cinchy/builder-guides/cinchydxd/dependencies.md) page for more information.
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


## Encrypting files with CinchyDXD

1. Run `.\CinchyDXD.ps1 keygen -o "<TargetDirectory>"`, where `TargetDirectory` is your desired output path.
1. Find the `dxd.key` file created.

<<<<<<< HEAD:guides-for-using-cinchy/builder-guides/cinchydxd-utility/overview.md
=======
## Changelog

See the [Changelog](/guides-for-using-cinchy/builder-guides/cinchydxd/version-history-dxd.md) for more details.

>>>>>>> 54ee3fe (Folder cleanup):guides-for-using-cinchy/builder-guides/cinchydxd/overview.md
## Next steps

- Review the [DXD workflow](../cinchydxd/dxd-workflow.md)
