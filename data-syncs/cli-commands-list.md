# CLI Commands List

## Overview

This page provides a list of the commands you can use in tandem with Cinchy's Command LIne Interface (CLI).

Prior to attempting to use any of the below commands, [ensure that you have installed the CLI.](installation-and-maintenance/installing-the-cli-and-the-maintenance-cli.md)

## Global Command List

<table data-header-hidden><thead><tr><th width="362">Command</th><th>Description</th></tr></thead><tbody><tr><td>Command</td><td>Description</td></tr><tr><td>syncdata</td><td>Update a target dataset (a Cinchy table, a database table) with data from an input source (a delimited file, Salesforce) using a defined key to match records<br><br><em>View the parameters for this command below.</em></td></tr><tr><td>exportdata</td><td><p>Exports the results of a saved query to a delimited file.</p><p></p><p><em>View the parameters for this command below.</em><br><br>Ex:</p><pre><code>./Cinchy.Connections.CLI.exe exportdata -h -s sandbox.cinchy.net/dev-aurora-2 -d Sandbox -n "Query With Over 1000" -u admin -p cinchy -o 'C:\Users\MyUser\Downloads\output.csv' -r
</code></pre></td></tr><tr><td>matchdata</td><td>Matches data across sources to create a master record in Cinchy</td></tr><tr><td>appdiff</td><td>Generates a report outlining the differences between the physical implementation of an application across Cinchy environments</td></tr><tr><td>encrypt</td><td>Generates an encrypted string for use in other areas of the CLI.<br><br><em>View the parameters for this command below.</em></td></tr><tr><td>--version</td><td>Outputs the current version of the CLI</td></tr></tbody></table>

## Parameters

<details>

<summary>SyncData Parameters</summary>

* **-h, -https:** Flag indicating connections to Cinchy should be over https.
* **-s, --server:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The full path to the Cinchy server without the protocol (such as cinchy.co/Cinchy).
* **-u, --userid:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The user id to login to Cinchy.
* **-p, --password:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The password of the specified user. This can be optionally encrypted using the CLI's encrypt command.
* **-f, --feed:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The name of the feed configuration as defined in Cinchy.
* **-d, --tempdirectory:** <mark style="color:red;">**Only applies to Cinchy v4.**</mark>\
  **Required**. The path to a directory that the CLI can use for storing temporary files to support the sync (such as error files).
* **-b, --batchsize:** (Default: 5000) The number of rows to sync per batch (within a partition) when executing inserts/updates.
* **-z, --retrievalbatchsize:** (Default: 5000) The max number of rows to retrieve in a single batch from Cinchy when downloading data.
* **-v, --param-values:** Job parameter values defined as one or more name value pairs delimited by a colon. For example,` -v name1:value1 name2:value2`.
* **--file:** Works exactly as `-v` but it's for parameters that are files.
* **--help:** Displays the help screen with the options.
* **-w, --writetofile**: Write the data from Cinchy to disk, required for large data sets exceeding 2GB.

</details>

<details>

<summary>ExportData Parameters</summary>

* **-h, -https:** Flag indicating connections to Cinchy should be over https.
* **-s, --server:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The full path to the Cinchy server without the protocol (e.g. cinchy.co/Cinchy).
* **-u, --userid:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The user id to login to Cinchy.
* **-p, --password:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The password of the specified user. This can be optionally encrypted using the CLI's encrypt command.
* **-d, --domain:** <mark style="color:orange;">**Required.**</mark> The domain where your saved query resides.
* **-n, --name: Required.** The name of your saved query.
* **-o, --outputpath.** <mark style="color:orange;">**Required.**</mark> The full path for the target output file.
* **-l, --delimiter.** The delimiter to used in the output. This can be either COMMA, PIPE, or TAB. This defaults to COMMA  if you do not set the parameter.
* **-i, --includeheaders.** A flag to include headers in the output file.
* **-e, --encoding.** The encoding to use for the output file, either ASCII, UTF8, or UTF16. This defaults to UTF16 if you do not set the parameter.
* **-t, --commandtimeout.** The wait time in seconds before terminating the query execution. This defaults to 30 seconds if you do not set the parameter.
* **-g, --progressbatchsize.** When downloading data, this is the number of MB after which progress should be logged. Setting this to 0 prevents any progress messages to the console. This defaults to 10 if you do not set the parameter.
* **-r, --overwriteoutput.** A flag indicating whether to overwrite the output file contents if it already exists.
* **-q, --donotescape.** Setting this flag prevents text values from being escaped using quotes. This should only be used if you are certain the delimiter will not appear in your data, otherwise output may be invalid.
* **-v, --param-values.** Query parameter values defined as one or more name value pairs delimited by a colon (`-v name1:value1 name2:value2`)
* **-a, --tls.** The TLS protocol version.

</details>

<details>

<summary>Encrypt Parameters</summary>

* **-h, -https:** Flag indicating connections to Cinchy should be over https.
* **-s, --server:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The full path to the Cinchy server without the protocol (e.g. cinchy.co/Cinchy).
* **-u, --userid:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The user id to login to Cinchy.
* **-p, --password:** <mark style="color:orange;">**Required**</mark><mark style="color:orange;">.</mark> The password of the specified user. This can be optionally encrypted using the CLI's encrypt command.
* **-t, --text.** <mark style="color:orange;">**Required.**</mark> The full text that you want to encrypt.
* **-a, --tls.** The TLS protocol version.

</details>
