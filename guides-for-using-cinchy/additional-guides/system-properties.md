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

Users can set their own time zones in their user profile. The default time zone values are entered manually and must correspond with the one of the values in the Default Time Zone value list located below. For changes to take effect, you must either clear the application cache or restart the instance.

{% hint style="info" %}
If you enter an incorrect value in the **Value** column, then it will default to Eastern Standard Time (EST)
{% endhint %}

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

## Forbidden Passwords Table

You can also use a table called Forbidden Passwords to define passwords that won't be accepted by the platform. This table comes with a pre-populated list of passwords from [https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt](https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt)

You can add more blocked passwords to this list as well, and users won't be able to set their password to it (this can be used to add your company's name, or to import other blocked password lists). The check against the list is case insensitive.

Like other password policies, this check occurs when your password changes, so to enforce this you will need to set all passwords to be expired.

## References

### Default Time Zone value list

| Time Zone                       | Time Difference (GMT) |
| ------------------------------- | --------------------- |
| Dateline Standard Time          | -12:00:00             |
| UTC-11                          | -11:00:00             |
| Aleutian Standard Time          | -10:00:00             |
| Hawaiian Standard Time          | -10:00:00             |
| Marquesas Standard Time         | -09:30:00             |
| Alaskan Standard Time           | -09:00:00             |
| UTC-09                          | -09:00:00             |
| Pacific Standard Time (Mexico)  | -08:00:00             |
| UTC-08                          | -08:00:00             |
| Pacific Standard Time           | -08:00:00             |
| US Mountain Standard Time       | -07:00:00             |
| Mountain Standard Time (Mexico) | -07:00:00             |
| Mountain Standard Time          | -07:00:00             |
| Yukon Standard Time             | -07:00:00             |
| Central America Standard Time   | -06:00:00             |
| Central Standard Time           | -06:00:00             |
| Easter Island Standard Time     | -06:00:00             |
| Central Standard Time (Mexico)  | -06:00:00             |
| Canada Central Standard Time    | -06:00:00             |
| SA Pacific Standard Time        | -05:00:00             |
| Eastern Standard Time (Mexico)  | -05:00:00             |
| Eastern Standard Time           | -05:00:00             |
| Haiti Standard Time             | -05:00:00             |
| Cuba Standard Time              | -05:00:00             |
| US Eastern Standard Time        | -05:00:00             |
| Turks and Caicos Standard Time  | -05:00:00             |
| Paraguay Standard Time          | -04:00:00             |
| Atlantic Standard Time          | -04:00:00             |
| Venezuela Standard Time         | -04:00:00             |
| Central Brazilian Standard Time | -04:00:00             |
| SA Western Standard Time        | -04:00:00             |
| Pacific SA Standard Time        | -04:00:00             |
| Newfoundland Standard Time      | -03:30:00             |
| Tocantins Standard Time         | -03:00:00             |
| E. South America Standard Time  | -03:00:00             |
| SA Eastern Standard Time        | -03:00:00             |
| Argentina Standard Time         | -03:00:00             |
| Montevideo Standard Time        | -03:00:00             |
| Magallanes Standard Time        | -03:00:00             |
| Saint Pierre Standard Time      | -03:00:00             |
| Bahia Standard Time             | -03:00:00             |
| UTC-02                          | -02:00:00             |
| Greenland Standard Time         | -02:00:00             |
| Mid-Atlantic Standard Time      | -02:00:00             |
| Azores Standard Time            | -01:00:00             |
| Cabo Verde Standard Time        | -01:00:00             |
| Coordinated Universal Time      | 00:00:00              |
| GMT Standard Time               | 00:00:00              |
| Greenwich Standard Time         | 00:00:00              |
| Sao Tome Standard Time          | 00:00:00              |
| Morocco Standard Time           | 00:00:00              |
| W. Europe Standard Time         | 01:00:00              |
| Central Europe Standard Time    | 01:00:00              |
| Romance Standard Time           | 01:00:00              |
| Central European Standard Time  | 01:00:00              |
| W. Central Africa Standard Time | 01:00:00              |
| GTB Standard Time               | 02:00:00              |
| Middle East Standard Time       | 02:00:00              |
| Egypt Standard Time             | 02:00:00              |
| E. Europe Standard Time         | 02:00:00              |
| Syria Standard Time             | 02:00:00              |
| West Bank Gaza Standard Time    | 02:00:00              |
| South Africa Standard Time      | 02:00:00              |
| FLE Standard Time               | 02:00:00              |
| Jerusalem Standard Time         | 02:00:00              |
| South Sudan Standard Time       | 02:00:00              |
| Russia TZ 1 Standard Time       | 02:00:00              |
| Sudan Standard Time             | 02:00:00              |
| Libya Standard Time             | 02:00:00              |
| Namibia Standard Time           | 02:00:00              |
| Jordan Standard Time            | 03:00:00              |
| Arabic Standard Time            | 03:00:00              |
| Turkey Standard Time            | 03:00:00              |
| Arab Standard Time              | 03:00:00              |
| Belarus Standard Time           | 03:00:00              |
| Russia TZ 2 Standard Time       | 03:00:00              |
| E. Africa Standard Time         | 03:00:00              |
| Volgograd Standard Time         | 03:00:00              |
| Iran Standard Time              | 03:30:00              |
| Arabian Standard Time           | 04:00:00              |
| Astrakhan Standard Time         | 04:00:00              |
| Azerbaijan Standard Time        | 04:00:00              |
| Russia TZ 3 Standard Time       | 04:00:00              |
| Mauritius Standard Time         | 04:00:00              |
| Saratov Standard Time           | 04:00:00              |
| Georgian Standard Time          | 04:00:00              |
| Caucasus Standard Time          | 04:00:00              |
| Afghanistan Standard Time       | 04:30:00              |
| West Asia Standard Time         | 05:00:00              |
| Russia TZ 4 Standard Time       | 05:00:00              |
| Pakistan Standard Time          | 05:00:00              |
| Qyzylorda Standard Time         | 05:00:00              |
| India Standard Time             | 05:30:00              |
| Sri Lanka Standard Time         | 05:30:00              |
| Nepal Standard Time             | 05:45:00              |
| Central Asia Standard Time      | 06:00:00              |
| Bangladesh Standard Time        | 06:00:00              |
| Omsk Standard Time              | 06:00:00              |
| Myanmar Standard Time           | 06:30:00              |
| SE Asia Standard Time           | 07:00:00              |
| Altai Standard Time             | 07:00:00              |
| W. Mongolia Standard Time       | 07:00:00              |
| Russia TZ 6 Standard Time       | 07:00:00              |
| Novosibirsk Standard Time       | 07:00:00              |
| Tomsk Standard Time             | 07:00:00              |
| China Standard Time             | 08:00:00              |
| Russia TZ 7 Standard Time       | 08:00:00              |
| Malay Peninsula Standard Time   | 08:00:00              |
| W. Australia Standard Time      | 08:00:00              |
| Taipei Standard Time            | 08:00:00              |
| Ulaanbaatar Standard Time       | 08:00:00              |
| Aus Central W. Standard Time    | 08:45:00              |
| Transbaikal Standard Time       | 09:00:00              |
| Tokyo Standard Time             | 09:00:00              |
| North Korea Standard Time       | 09:00:00              |
| Korea Standard Time             | 09:00:00              |
| Russia TZ 8 Standard Time       | 09:00:00              |
| Cen. Australia Standard Time    | 09:30:00              |
| AUS Central Standard Time       | 09:30:00              |
| E. Australia Standard Time      | 10:00:00              |
| AUS Eastern Standard Time       | 10:00:00              |
| West Pacific Standard Time      | 10:00:00              |
| Tasmania Standard Time          | 10:00:00              |
| Russia TZ 9 Standard Time       | 10:00:00              |
| Lord Howe Standard Time         | 10:30:00              |
| Bougainville Standard Time      | 11:00:00              |
| Russia TZ 10 Standard Time      | 11:00:00              |
| Magadan Standard Time           | 11:00:00              |
| Norfolk Standard Time           | 11:00:00              |
| Sakhalin Standard Time          | 11:00:00              |
| Central Pacific Standard Time   | 11:00:00              |
| Russia TZ 11 Standard Time      | 12:00:00              |
| New Zealand Standard Time       | 12:00:00              |
| UTC+12                          | 12:00:00              |
| Fiji Standard Time              | 12:00:00              |
| Kamchatka Standard Time         | 12:00:00              |
| Chatham Islands Standard Time   | 12:45:00              |
| UTC+13                          | 13:00:00              |
| Tonga Standard Time             | 13:00:00              |
| Samoa Standard Time             | 13:00:00              |
| Line Islands Standard Time      | 14:00:00              |
