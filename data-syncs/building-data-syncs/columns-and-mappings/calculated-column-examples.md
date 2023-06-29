# Calculated Column Examples

## Examples

_Examples 1 and 2_ are shown first within the experience as well as their XML equivalent.

_Example 3_ demonstrates the use of Javascript in Calculated Columns.

### Example 1: XML

The value of this column for each record is whatever the value is of the lob parameter.&#x20;

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

XML equivalent of the above example_:_

```xml
<Parameters>
<Schema>
  <CalculatedColumn name="lob" formula="@lob" dataType="Text" maxLength="100" 
      isMandatory="false" description="" trimWhitespace="true"/>
  <CalculatedColumn name="name" formula="CONCAT(firstname, lastname)" 
      dataType="Text" maxLength="100" isMandatory="false" 
      description="" trimWhitespace="true"/>
 </Schema>
```

{% hint style="warning" %}
The CONCAT function supports more than 2 parameters, and any literal values must be enclosed in single quotes (e.g. 'abc')
{% endhint %}

### Example 2: XML

The values of two columns are concatenating together.&#x20;

<figure><img src="../../../.gitbook/assets/image (267).png" alt=""><figcaption></figcaption></figure>

XML equivalent of the above example:

```xml
<Parameters>
<Schema>
  <CalculatedColumn name="lob" formula="@lob" dataType="Text" maxLength="100" 
      isMandatory="false" description="" trimWhitespace="true"/>
  <CalculatedColumn name="name" formula="CONCAT(firstname, lastname)" 
      dataType="Text" maxLength="100" isMandatory="false" 
      description="" trimWhitespace="true"/>
 </Schema>
```

{% hint style="warning" %}
The CONCAT function supports more than 2 parameters, and any literal values must be enclosed in single quotes (e.g. 'abc')
{% endhint %}

### Example 3: JavaScript

This example splits a \[Name] column with the format "Lastname, Firstname" into two columns: \[First Name] and \[Last Name].

```javascript
       <Schema>
            <Column name="Name" dataType="Text"/>
            <CalculatedColumn name="First Name" ordinal="3" dataType="Text">
                <Script>
            function firstName(Name) {
                return Name.substr(Name.indexOf(', ')+2);
            }
                        
            function calc(currentRecord) {
                if (currentRecord['Name'])
                    return firstName(currentRecord['Name']);
                    return '';
            }            
            </Script>
            </CalculatedColumn>
            <CalculatedColumn name="Last Name" dataType="Text">
                <Script>
                    function lastName(Name) {
                        return Name.substr(0,Name.indexOf(', '));
                    }
    
    
                    function calc(currentRecord) {
                        if (currentRecord['Name'])
                        return lastName(currentRecord['Name']);                            
                        return '';
                    }   
                    </Script>
            </CalculatedColumn>
        </Schema>
```

## Attributes <a href="#id-less-than-column-greater-than-attributes" id="id-less-than-column-greater-than-attributes"></a>

**`name`**

The user defined name for each calculated column. This is used in [\<ColumnMapping>](https://cinchy.atlassian.net/wiki/spaces/KB/pages/196575278) when you want to indicate the name of the _sourceColumn_.

**`formula`**

CQL expression used to define formula. Supported functions:

| Function                                               | Details                                                                                                                                         |
| ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| CONCAT(colA, colB, 'literal value1', 'literal value2') | Concatenates multiple columns, parameters or literal values together. Supports two or more parameters.                                          |
| row\_number()                                          | This is the numeric row number of files (Excel, delimited, fixed width). Currently not supported in conjunction with other formulas/parameters. |
| isnull(colA,'alt value')                               | If the first column is null, use the second value (can be a literal or another column).                                                         |
| hash('SHA256',colA)                                    | Hashes the column using the algorithm specified.                                                                                                |

{% hint style="warning" %}
We recommend you salt your value before you hash it.
{% endhint %}

**`dataType`**

The data type of each column could be Text, Date, Number or Bool.

**`maxLength`**

The max length of data in the column.

**`isMandatory`**

Boolean value determining if the field is a mandatory column to create a row entry.

**`validateData`**

Boolean value determining whether or not to validate the data before inserting. Valid data means to fit all the constraints of the column (dataType, maxLength, isMandatory, inputFormat). If the data is not valid and validateData is true, then the entry will not be synced into the table. Also, Execution Errors Table is updated with appropriate Error Type (Invalid Format Exception, Max Length Violation, Mandatory Rule Violation, Input Format Exception)

**`description`**

Description of the column.

**`trimWhitespace`**

Boolean value determining whether or not to trim white space.
