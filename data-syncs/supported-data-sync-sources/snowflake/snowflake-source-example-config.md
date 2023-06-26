# Snowflake Source Example Config

## 1. XML Example

This example XML uses the following values:

<table><thead><tr><th width="343.2339286910899">Value</th><th width="189">Description</th><th>Example</th></tr></thead><tbody><tr><td>connectionString</td><td>The connections string for your source</td><td>"87E4lvPf83gLK8eKapH6Y0YqIFSNbFlq62uN9487"</td></tr><tr><td>Object</td><td>The type of source object</td><td>"Table"</td></tr><tr><td>Table</td><td>The name of your source object (in this case a table)</td><td>"Employees"</td></tr><tr><td>Column Name</td><td>The name(s) of your source column(s)</td><td>"name"</td></tr><tr><td>dataType</td><td>The data type of your source column</td><td>"Text"</td></tr><tr><td>isMandatory</td><td>Whether the column is mandatory or not</td><td>"false"</td></tr><tr><td>validateData</td><td>Whether the column data needs to be validated or not</td><td>"false"</td></tr></tbody></table>

### 1.1 Blank XML Example

```xml
    <SnowflakeDataSource 
connectionString=“” object=“” table=“”>
        <Schema>
            <Column name=“” dataType=“” isMandatory=“” validateData=“”/>
        </Schema>
    </SnowflakeDataSource>
```

### 1.2 Populated XML Example

```xml
    <SnowflakeDataSource 
connectionString="87E4lvPf83gLK8eKapH6Y0YqIFSNbFlq62uN9487" object="table" table="Employees">
        <Schema>
            <Column name="name" dataType="Text" isMandatory="false" validateData="false"/>
        </Schema>
    </SnowflakeDataSource>
```
