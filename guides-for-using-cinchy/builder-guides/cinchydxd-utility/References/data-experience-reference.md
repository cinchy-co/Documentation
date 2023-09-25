## Data Experience Reference Table

The list below is a reference to all columns in the Data Experience Reference Table.

| Column                     | Definition                                                                                                                                                                  |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name                       | The name you give to your Reference Data Table. Doesn't have to  match the actual table name.                                                                               |
| Ordinal                    | Specifies load order based on data dependencies.                                                                                                                            |
| Filter                     | WHERE clause for row conditions. For example, for parent-child hierarchies, two rows are needed—one for parent data and one for child data, each with its own WHERE clause. |
| Target Filter |  Defines conditions that impact records in the target table, such as INSERT, UPDATE, DELETE, or IGNORE. When using both **Source Filter** and **Target Filter**, it's crucial that they align to ensure accurate data operations. |
| New Records                | Action for new records: INSERT, UPDATE, DELETE, IGNORE.                                                                                                                     |
| Change Records             | Action for updated records: INSERT, UPDATE, DELETE, IGNORE.                                                                                                                 |
| Dropped Records            | Action for removed records: INSERT, UPDATE, DELETE, IGNORE.                                                                                                                 |
| Table                      | Source table for data export.                                                                                                                                               |
| Sync Key                   | Unique key from the target table.                                                                                                                                           |
| Expiration Timestamp Field | Required if **Dropped Records** is set to `Expire`.                                                                                                                         |