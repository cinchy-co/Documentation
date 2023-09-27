# Variables

## Overview

Variables are dynamic values you can include in sync jobs. Use the "@" prefix to reference these variables in other parts of your sync configuration. During job execution, you might be prompted to input these values.

Execution variables can be either provided at runtime or computed using a formula. Optionally, you can pass the `name` attribute value as a command line option using `param-values`.

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Image 1: Connections UI</p></figcaption></figure>

## Supported Formulas

You can choose to just use plain text in the Name field of the Variable or you can use a calculated formula.

The following formulas are currently supported by Connections.

{% hint style="info" %}
Cinchy supports .NET regular expressions. For more information, please see the [Microsoft .NET Expressions page](https://learn.microsoft.com/en-us/dotnet/standard/base-types/regular-expressions).
{% endhint %}


### Supported formulas

| Formula | Parameters | Description | Use Case |
| ------- | ---------- | ----------- | -------- |
| `FILENAME(<some-path>, <some-regex>)` | 1. File path<br>2. Regex with match group | Applies regex to file name only, excluding the directory. | To extract specific data from a file name. |
| `FILEPATH(<some-path>, <some-regex>)` | 1. File path<br>2. Regex with match group | Applies regex to the full file path, including the directory. | To extract specific data from a full file path. |
| `GUID()` | None | Generates a random GUID. | To create a unique identifier during sync. |
| `GETSECRETVALUE(domain, secretname)` | Domain, Secret name | Retrieves a secret from the Cinchy Secrets Table. | To insert sensitive information like a connection string. |
| `ENV(<place-environment-variable-here>)` | Environment variable | Uses an available environment variable. | To insert an environment variable value. |


{% hint style="danger" %}
Avoid using the ENV formula for credentials.
{% endhint %}

## Examples

Below are the three Variable examples shown in the Connections experience, followed by the relevant XML:

### Example 1

A name attribute reference an execution variable _(Image 2)._ You can use this when pulling in a local file for an Excel sync and specify the path to your file on your machine.

<figure><img src="../../../.gitbook/assets/image (364).png" alt=""><figcaption><p>Image 2: Example 1</p></figcaption></figure>

### Example 2

The FILEPATH function takes in two variables: 

1. A reference to the first variable, such as a file path.
2. A regular expression that includes a match group _(Image 3)_. The first match group's value is assigned to the variable. FILEPATH function executes regex against the full file path (including the directory structure). For the full formula, see the XML example below.

<figure><img src="../../../.gitbook/assets/image (684).png" alt=""><figcaption><p>Image 3: Example 2</p></figcaption></figure>

### Example 3

The FILENAME function takes in two variables:

1. A reference to the first variable, such as a file path. 
2. A regular expression that includes a match group _(Image 4)_. The first match group's value is what gets assigned to the variable. FILENAME function applies the regex only to the name of the file (excluding the directory structure).

<figure><img src="../../../.gitbook/assets/image (696).png" alt=""><figcaption><p>Image 4: Example 3</p></figcaption></figure>

### Example 4 

The ENV formula uses an environment variable available in the connections/worker pods as the value of the variable _(Image 5)_. An example use case for this would be a situation where the URLs used in a REST API sync is different across environments -- instead of manually updating the syncs with the various URLs, you can use this formula to automatically calculated it from your pod configuration files.

<figure><img src="../../../.gitbook/assets/image (676).png" alt=""><figcaption><p>Image 5: Example 4</p></figcaption></figure>

#### XML examples

_Example 5:_ The **GETSECRETVALUE** formula _(Image 6)_ is input as a variable for a REST Source and is used to call a secret from the [Cinchy Secrets Table.](../../../guides-for-using-cinchy/additional-guides/cinchy-secrets-manager.md)

<div data-full-width="true">

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Image 6: Example 5</p></figcaption></figure>

</div>

This secret can then be used in the REST Header _(Image 7)._

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Image 7: Example 5</p></figcaption></figure>

#### XML examples

{% hint style="info" %}
While in the UI the term is **variables**, the paired XML configuration uses the term **parameters**.
{% endhint %}

```markup
<Parameters>
	<!-- Example 1 -->
	<Parameter name="file_path" />
	<!-- Example 2 -->
	<Parameter name="lob" 
		formula="FILEPATH('@file_path', '^.*[\\/](.{3})[\\/].*xls')" /> 
	<!-- Example 3 -->
	<Parameter name="business_date" formula="FILENAME('@file_path', 
		'^.*_Transaction_Detail_Report_(\d{8})\.xls$|^TRDTRX-(\d{8})-\d{4}[ap]m.xls$')" />
       <!-- Example 4 -->
         <Parameter name="URL" formula="ENV(&apos;API_URL&apos;)"/>
</Parameters>
```

## Appendix A

This section details how to configure your platform to use Environment Variables.

### Configure environment variables in IIS

To create or change environment variables on Windows:

1. On the Windows taskbar, right-click the **Windows Icon** **>** **System.**
2. In the **Settings** window, click **Related Settings > Advanced System Settings > Environment Variables** _(Image 8)._

<figure><img src="../../../.gitbook/assets/image (406).png" alt=""><figcaption><p>Image 8: Environment Variables</p></figcaption></figure>

3. Under **System Variables**, click **New** to create your new environment variable (_Image 9)._

<figure><img src="../../../.gitbook/assets/image (488).png" alt=""><figcaption><p>Image 9: New Environment Variable</p></figcaption></figure>

### Configure environment variables in Kubernetes

To configure an environment variable in Kubernetes, do the following:

#### Add the variable to your cluster

1. Navigate to your `cinchy.kubernetes\environment\_kustomizations\_template\instance\_template\connections\kustomization.yaml` file.
2. Under **patchesJson6902 > patch**, add your environment variable as shown in the code snippet below. Input your own namespace and variable name where indicated.

```yaml
patchesJson6902:
- target:
  [...]
  patch: |-
    - op: replace
      path: /spec/template/spec/containers/0/env/1/name
      value: <<your namespace>>-YOUR_VARIABLE
```

#### Add the variable to your pod

1. Navigate to your **platform\_components/connections/connections-app.yaml** file.
2. Under **Spec > Template > Spec > Containers > ENV**, add in your environment variable. This addition depends on what value you are using as an environment variable. The below code snippet shows a basic example:

```yaml
        env:
          - name: ENV_VARIABLE_NAME
            value: ENV_VARIABLE_VALUE
```

1. Make the same change to the **platform\_components/worker/worker-app.yaml** file.
2. Push and merge your changes.
