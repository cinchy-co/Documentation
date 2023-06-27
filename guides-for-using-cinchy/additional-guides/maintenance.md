---
description: This page contains an overview of Cinchy's maintenance parameters.
---

# Maintenance

## Prerequisites

* Requires CLI v4.7+

## Maintenance

Cinchy performs maintenance tasks through the CLI. This currently includes the data erasure and data compression deletions.

```
Cinchy.Maintenance.CLI.exe maintenance -s "cinchyBaseURL" -u username -p "encryptedPassword" -t 180
```

| Parameter | Value                                                                                                                                                                             |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -s        | Server, i.e. Cinchy Base URL (ex. cinchy.com/Cinchy/)                                                                                                                             |
| -u        | Username, this will need to be an account that is part of the Cinchy Administrators group                                                                                         |
| -p        | Encrypted password (you can encrypt your password by using `Cinchy.CLI.exe encrypt -t "plaintextpassword"`                                                                        |
| -t        | Set a maintenance time window in minutes. Maintenance tasks will stop executing after the allotted time frame. This allows you to run this during an allotted maintenance window. |
| -h        | This flag must be added if you are accessing Cinchy over **https.**                                                                                                               |
