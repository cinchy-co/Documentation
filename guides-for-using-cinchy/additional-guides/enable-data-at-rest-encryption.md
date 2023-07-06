---
description: >-
  This page details how to enable data at rest encryption and a few other
  important features.
---

# Enable Data At Rest Encryption

## Table of Contents

| Table of Contents                                                                                           |
| ----------------------------------------------------------------------------------------------------------- |
| [#introduction](enable-data-at-rest-encryption.md#introduction "mention")                                   |
| [#create-master-key-in-database](enable-data-at-rest-encryption.md#create-master-key-in-database "mention") |
| [#backup-master-key](enable-data-at-rest-encryption.md#backup-master-key "mention")                         |
| [#restore-master-key](enable-data-at-rest-encryption.md#restore-master-key "mention")                       |

## Introduction

Cinchy 2.0 has added the feature to encrypt data at rest. This means that you can encrypt data in the database such that users with access to view data in the database will see ciphertext in those columns. However, all users with authorized access to the data via Cinchy will see the data as plain text.To use this feature, your database administrator will be need to create a database master key (see below for instructions).

## 1. Create Master Key in Database <a href="#create-master-key-in-database" id="create-master-key-in-database"></a>

1. The first step is to create a master key in the database. Do so by connecting directly whichever database your Cinchy instance is running on.
2. Run the below query to create your master key:

```
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
```

{% hint style="info" %}
The password should adhere to your organization's password policy.
{% endhint %}

3\. You can now encrypt data via the user interface _(Image 1)_:

![Image 1:  The Encryption option is now available for column types supported via UI.](<../../.gitbook/assets/image (33).png>)

## 2. Backup Master Key <a href="#backup-master-key" id="backup-master-key"></a>

After you have created your master key you can create a backup file of that key in case any data corruption occurs in future.&#x20;

You will need the password you used to create your master key to complete this operation.

1. Run the following command:

```
BACKUP MASTER KEY TO FILE = 'path_to_file'   
    ENCRYPTION BY PASSWORD = 'password' 
```

Further documentation on creating a backup master key [can be found here.](https://docs.microsoft.com/en-us/sql/t-sql/statements/backup-master-key-transact-sql?view=sql-server-2017)

## 3. Restore Master Key <a href="#restore-master-key" id="restore-master-key"></a>

In the use case where you require to restore your master key due to data corruption, you may use the following steps.

You will need the password you used to create you master key to complete this operation.

1. Run the following command:

```
RESTORE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password'  
    ENCRYPTION BY PASSWORD = 'password'  
    [ FORCE ] 
```

Further documentation on restoring the master key [can be found here.](https://docs.microsoft.com/en-us/sql/t-sql/statements/restore-master-key-transact-sql?view=sql-server-2017)
