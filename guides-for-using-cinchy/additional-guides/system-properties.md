---
description: >-
  This page details the vales and functions of the Cinchy System Properties
  table.
---

# System properties

System Properties is a table within Cinchy for managing system properties, such as default time zones, system lockout durations, password expiration, password properties, password attempts allowed etc.

## Setup

The Default of the Systems Properties table is set up as follows:

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


{% hint style="info" %}
This table is case sensitive.
{% endhint %}

## Configure the table values

The System Properties requirements can be changed by an admin user by editing the 'Value' columns where applicable:

### Default time zone

Users can set their own time zones in their user profile. If a user doesn't set one up, the system default time zone will take effect. If this property doesn't exist or is invalid, the default time zone will default to UTC.

### Minimum password length

The minimum password length is 8 characters. The length will always default to 8 if an invalid value is provided, or if you attempt to set it to less than 8. This number can be changed (made higher than 8) in the **Value** column to require users to have longer passwords.

### Password requires symbols

This property specifies whether symbols are required in a user's password. The 'Value' 0 means symbols aren't required and 1 means they're required.

### Password requires numbers

This property specifies whether numbers are required in a user's password. The 'Value' 0 means numbers aren't required and 1 means they're required.

{% hint style="info" %}
For a new password policy to take effect, you can set all user's **Password Expiration Timestamp** to yesterday. They will need to change their password the next time they attempt to log in.
{% endhint %}

### Password expiration (days)

This property specifies how many days until a password expires. This defaults to 90 but can be set to be shorter or longer by changing the number in the 'Value' column.

### Password attempts allowed

This property specifies how many bad password attempts a user can make before they're locked out of the system. The default is 3 but this can be set to be more or less attempts by changing the number in the 'Value' column.

### System lockout duration (minutes)

This property specifies how long a user is locked out of the system once they've run out of bad password attempts. The default is 15 minutes but this can be set to be shorter or longer by changing the number in the 'Value' column.

{% hint style="info" %}
Note that an administrator can also go into the 'Users' table to manually unlock a user by clearing the Locked Timestamp.
{% endhint %}

### Maintenance enabled

This property, defaulted to 0, shows this warning when a data owner is setting up Data Erasure or Data Compression on a table _(Image 2)_. It's the administrator's responsibility to set up a scheduled maintenance job for performing compression and erasure, and then to change the property to 1 so that the warning no longer appears.

![Image 2: Data Compression](<../../.gitbook/assets/image (216).png>)

## Forbidden passwords

Cinchy has a table called Forbidden Passwords. This table comes with a pre-populated list of passwords from [https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt](https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt)

You can add more blocked passwords to this list as well, and users won't be able to set their password to it (this can be used to add your company's name, or to import other blocked password lists). The check against the list is case insensitive.

Like other password policies, this check occurs when your password changes, so to enforce this you will need to set all passwords to be expired.
