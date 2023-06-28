# Parameters

## 1. Overview

Runtime Parameters are values that can be dynamically inserted when the sync job is run. The parameters you define here can be referenced in fields in other parts of your sync config (using the @ prefix) and when the job is run you can be prompted for their values.

The execution parameters are either passed in at the time of execution or calculated through a formula. The value of the name attribute is passed in as command line option, param-values. (Optional, if the path to the source file to load is specified in the path attribute of the source element or calculated column formula do not reference execution parameters)

Below is an element that holds an array of `<Parameter>` elements, shown in the Connections experience _(Image 1)_ and the equivalent XML.&#x20;

<figure><img src="../../../.gitbook/assets/image (433).png" alt=""><figcaption><p>Image 1: Connections UI</p></figcaption></figure>

```markup
<Parameters>
    <!-- Array of <Parameter> -->
    ...
</Parameters>
```

## 2. Supported Formulas

You can choose to just use plain text in the Name field of the Parameter, or you can use a calculated formula.

The following formulas are currently supported by Connections.

* **FILENAME(\<some-path>, \<some-regex>):** The FILENAME formula takes in two parameters. The first is a reference to the first parameter **(i.e. a file path)**, and the second is a regular expression that includes a match group. The first match group's value is what gets assigned to the parameter. The FILENAME function applies the regex only to the name of the file (**excluding** the directory structure).
* **FILEPATH(\<some-path>, \<some-regex>)**: Similar to FILENAME, the FILEPATH formula takes in two parameters. The first is a reference to the first parameter **(i.e. a file path)**, and the second is a regular expression that includes a match group. The first match group's value is what gets assigned to the parameter. The FILEPATH function executes the regex against the full file path (**including** the directory structure).
* **GUID()**: The GUID formula uses a random GUID for that parameter's value. It's used when you want to generate a unique identifier to be used during the context the sync and can be useful, for example, as a way to track if changes were made from a particular sync that ran.
* **ENV(\<place-environment-variable-here>):** The ENV formula uses an environment variable available in the connections/worker pods as the value of the parameter. An example use case for this would be a situation where the URLs used in a REST API sync is different across environments -- instead of manually updating the syncs with the various URLs, you can use this formula to automatically calculated it from your pod configuration files. To use the ENV() formula you will need to do some back-end configuration; please review Appendix A for more details.

{% hint style="danger" %}
We do not recommend using the ENV formula for credentials.
{% endhint %}

## 3. Examples

Below are the three Parameter examples shown in the Connections experience, followed by the XML equivalent of all of the examples:

_Example 1:_ A name attribute reference an execution parameter _(Image 2)._

<figure><img src="../../../.gitbook/assets/image (217).png" alt=""><figcaption><p>Image 2: Example 1</p></figcaption></figure>

_Example 2:_ The FILEPATH function takes in two parameters, the first is a reference to the first parameter (i.e. a file path), and the second is a regular expression that includes a match group _(Image 3)_. The first match group's value is what gets assigned to the parameter. FILEPATH function executes regex against the full file path (including the directory structure) \[full formula in XML at end of page].

<figure><img src="../../../.gitbook/assets/image (67).png" alt=""><figcaption><p>Image 3: Example 2</p></figcaption></figure>

_Example 3:_ The FILENAME function takes in two parameters, the first is a reference to the first parameter (i.e. a file path), and the second is a regular expression that includes a match group _(Image 4)_. The first match group's value is what gets assigned to the parameter. FILENAME function applies the regex only to the name of the file (excluding the directory structure).

<figure><img src="../../../.gitbook/assets/image (386).png" alt=""><figcaption><p>Image 4: Example 3</p></figcaption></figure>

_Example 4:_ The ENV formula uses an environment variable available in the connections/worker pods as the value of the parameter _(Image 5)_. An example use case for this would be a situation where the URLs used in a REST API sync is different across environments -- instead of manually updating the syncs with the various URLs, you can use this formula to automatically calculated it from your pod configuration files.

<figure><img src="../../../.gitbook/assets/image (422).png" alt=""><figcaption><p>Image 5: Example 4</p></figcaption></figure>

#### Example XMLs

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

This section details how to correctly configure your platform to use Environment Variables.

### Configuring Environment Variables in IIS

To create or modify environment variables on Windows:

1. On the Windows taskbar, right-click the **Windows Icon** **>** **System.**
2. In the **Settings** window, click **Related Settings > Advanced System Settings > Environment Variables** _(Image 6)._

<figure><img src="../../../.gitbook/assets/image (417).png" alt=""><figcaption><p>Image 6: Environment Variables</p></figcaption></figure>

3. Under **System Variables**, click **New** to create your new environment variable (_Image 7)._

<figure><img src="../../../.gitbook/assets/image (501).png" alt=""><figcaption><p>Image 7: New Environment Variable</p></figcaption></figure>

### Configuring Environment Variables in Kubernetes

To configure an environment variable in Kubernetes, you will first add the variable to your cluster:

1. Navigate to your **cinchy.kubernetes\environment\_kustomizations\_template\instance\_template\connections\kustomization.yaml file.**
2. Under **patchesJson6902 > patch**, add in your environment variable as shown in the code snippet below, inputting your own namespace and variable name where indicated.

```yaml
patchesJson6902:
- target:
  [...]
  patch: |-
    - op: replace
      path: /spec/template/spec/containers/0/env/1/name
      value: <<your namespace>>-YOUR_VARIABLE
```

You must then add the variable to your pod:

1. Navigate to your **platform\_components/connections/connections-app.yaml** file.
2. Under **Spec > Template > Spec > Containers > Env**, add in your environment variable. How this is added can depend on what value you are using as an environment variable. The below code snippet shows a basic example:

```yaml
        env:
          - name: ENV_VARIABLE_NAME
            value: ENV_VARIABLE_VALUE
```

3. Make the _same code change_ to the **platform\_components/worker/worker-app.yaml** file as well.
4. Ensure that you push and merge your changes.
