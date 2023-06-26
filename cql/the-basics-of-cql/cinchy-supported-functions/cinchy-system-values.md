# Cinchy System Values

## 1. Overview

The Cinchy system values covered in this section:

* [@cinchy\_row\_id](cinchy-system-values.md#cinchy\_row\_id)

## @cinchy\_row\_id

The @cinchy\_row\_id function returns the cinchy ID of the last-inserted row.

#### Syntax

```sql
select @cinchy_row_id
```

#### Examples

```sql
INSERT INTO [Contacts].[People] ([First Name],[Last Name])
VALUES ('John','Smith')

select @cinchy_row_id
```
