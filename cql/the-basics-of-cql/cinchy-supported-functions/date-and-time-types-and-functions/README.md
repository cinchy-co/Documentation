# Date and Time Types and Functions

## Overview

Perform operations on a date and time input values and return string, numeric, or date and time values.

The date and time date type and functions covered in this section are:

* [​Date and Time Data Types​](./#date-and-time-data-types)
* Date and Time Function Categories
  * [​Return System Date and Time Values ​](return-system-date-and-time-values.md)
  * ​[Return Date and Time Parts​](return-date-and-time-parts.md)
  * [​Return Date and Time Values from their Parts​](return-date-and-time-values-from-their-parts.md)
  * ​[Return Date and Time Difference Values​](return-date-and-time-difference-values.md)
  * ​[Modify Date and Time Values ​](modify-date-and-time-values.md)
  * [​Validate Date and Time Values​](validate-date-and-time-values.md)

### Date and Time Data Types <a href="#date-and-time-data-types" id="date-and-time-data-types"></a>

| Data type      | Format                                      | Range                                                                    | Accuracy        | Storage size (bytes) | User-defined fractional second precision | Time zone offset |
| -------------- | ------------------------------------------- | ------------------------------------------------------------------------ | --------------- | -------------------- | ---------------------------------------- | ---------------- |
| time           | hh:mm:ss\[.nnnnnnn]                         | 00:00:00.0000000 through 23:59:59.9999999                                | 100 nanoseconds | 3 to 5               | Yes                                      | No               |
| date           | YYYY-MM-DD                                  | 0001-01-01 through 9999-12-31                                            | 1 day           | 3                    | No                                       | No               |
| smalldatetime  | YYYY-MM-DD hh:mm:ss                         | 1900-01-01 through 2079-06-06                                            | 1 minute        | 4                    | No                                       | No               |
| datetime       | YYYY-MM-DD hh:mm:ss\[.nnn]                  | 1753-01-01 through 9999-12-31                                            | 0.00333 second  | 8                    | No                                       | No               |
| datetime2      | YYYY-MM-DD hh:mm:ss\[.nnnnnnn]              | 0001-01-01 00:00:00.0000000 through 9999-12-31 23:59:59.9999999          | 100 nanoseconds | 6 to 8               | Yes                                      | No               |
| datetimeoffset | YYYY-MM-DD hh:mm:ss\[.nnnnnnn] \[+\|-]hh:mm | 0001-01-01 00:00:00.0000000 through 9999-12-31 23:59:59.9999999 (in UTC) | 100 nanoseconds | 8 to 10              | Yes                                      | ​                |

​
