---
description: >-
  This page details the vales and functions of the Cinchy System Properties
  table.
---

# System Properties

System Properties is a table within Cinchy for managing system properties, such as default time zones, system lockout durations, password expirations, password properties, password attempts allowed etc.

## Table of Contents

| Table of Contents                                                                                  |
| -------------------------------------------------------------------------------------------------- |
| [#1.-set-up](system-properties.md#1.-set-up "mention")                                             |
| [#2.-configuring-the-table-values](system-properties.md#2.-configuring-the-table-values "mention") |
| [#3.-forbidden-passwords](system-properties.md#3.-forbidden-passwords "mention")                   |

## 1. Set up

The Default of the Systems Properties table is set up as follows _(Image 1)_:

| Property ID | Name                              | Value (Default)       |
| ----------- | --------------------------------- | --------------------- |
| 2           | Default Time Zone                 | Eastern Standard Time |
| 12          | Password Attempts Allowed         | 3                     |
| 13          | System Lockout Duration (minutes) | 15                    |
| 8           | Minimum Password Length           | 8                     |
| 9           | Password Requires Symbols         | 1                     |
| 10          | Password Requires Numbers         | 1                     |
| 11          | Password Expiration (Days)        | 90                    |
| 15          | Maintenance Enabled               | 0                     |

![Image 1: Default Set Up](<../../.gitbook/assets/image (574).png>)

{% hint style="info" %}
Please note that this table is case sensitive.&#x20;
{% endhint %}

## 2. Configuring the Table Values

The System Properties requirements can be changed by an admin user simply by editing the 'Value' columns where applicable:&#x20;

### Default Time Zone

Users can set their own time zones in their user profile. If a user does not set one up, the system default time zone will take effect. If this property does not exist or is invalid, the default time zone will default to UTC.

### Minimum Password Length

The minimum password length is 8 characters. The length will always default to 8 if an invalid value is provided, or if you attempt to set it to less than 8. This number can be changed (i.e. made higher than 8) in the 'Value' column to require users to have longer passwords.

### Password Requires Symbols

This property specifies whether symbols are required in a user's password. The 'Value' 0 means symbols are not required and 1 means they are required.

### Password Requires Numbers

This property specifies whether numbers are required in a user's password. The 'Value' 0 means numbers are not required and 1 means they are required.

{% hint style="info" %}
For a new password policy to take effect, you can set all user's **Password Expiration Timestamp** to yesterday. They will need to change their password the next time they attempt to log in.
{% endhint %}

### Password Expiration (Days)

This property specifies how many days until a password expires. This defaults to 90 but can be set to be shorter or longer by changing the number in the 'Value' column.

### Password Attempts Allowed

This property specifies how many bad password attempts a user can make before they are locked out of the system. The default is 3 but this can be set to be more or less attempts by changing the number in the 'Value' column.&#x20;

### System Lockout Duration (minutes)

This property specifies how long a user is locked out of the system once they've run out of bad password attempts. The default is 15 minutes but this can be set to be shorter or longer by changing the number in the 'Value' column.

{% hint style="info" %}
Note that an administrator can also go into the 'Users' table to manually unlock a user by clearing the Locked Timestamp.
{% endhint %}

### Maintenance Enabled

This is a property, defaulted to 0, that is simply responsible for showing this warning when a data owner is setting up [Data Erasure](broken-reference) or [Data Compression](broken-reference) on a table _(Image 2)_. It is the administrator's responsibility to set up a scheduled maintenance job for performing compression and erasure, and then to change the property to 1 so that the warning no longer appears.

![Image 2: Data Compression](<../../.gitbook/assets/image (402).png>)

## 3. Forbidden Passwords

There is a Cinchy table called Forbidden Passwords. This table comes with a prepopulated list of passwords from [https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt](https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt)

You can add more blocked passwords to this list as well, and users will not be able to set their password to it (this can be used to add your company's name, or to import other blocked password lists). The check against the list is case insensitive.

Like other password policies, this check occurs when your password changes, so to enforce this you will need to set all passwords to be expired.
