# MongoDB Collection Source Example

## 1. Example Data

The following is an example of data we want to sync out of MongoDB.

```xml
test> db.Articles.find()
[
  {
    _id: ObjectId("63d812afd755fcdeed234402"),
    Name: 'Shirt',
    Price: 19.95,
    Details: { Color: 'Red', Size: 'Medium' },
    Stock: 12
  },
  {
    _id: ObjectId("63d8137bd755fcdeed234403"),
    Name: 'Shirt',
    Price: 9.95,
    Details: { Color: 'White', Size: 'Small' },
    Stock: 61
  }
]
```

## 2. XML Example

This example XML uses the following values:

<table><thead><tr><th width="190.2339286910899">Value</th><th width="189">Description</th><th>Example</th></tr></thead><tbody><tr><td>connectionString</td><td>The connections string for your source</td><td>"87E4lvPf83gLK8eKapH6Y0YqIFSNbFlq62uN9487"</td></tr><tr><td>Database</td><td>The name of your MongoDB database</td><td>"test"</td></tr><tr><td>Collection</td><td>The name of your <a href="https://www.mongodb.com/docs/manual/core/databases-and-collections/">MongoDB collection.</a></td><td>"Article"</td></tr><tr><td>Type</td><td>The method used to retrieve your data.</td><td>"find"</td></tr><tr><td>Query</td><td>A query for retrieving your data.</td><td><p><em>This example query returns data where the price is less than 10$.</em></p><pre class="language-xml"><code class="lang-xml">"{ "Price": { "$lt": 10 } }"
</code></pre></td></tr><tr><td>Projection</td><td>A projection for flattening your source document.</td><td><pre class="language-xml"><code class="lang-xml">"{
    &#x26;quot;Name&#x26;quot;: 1,
    &#x26;quot;Price&#x26;quot;: 1,
    &#x26;quot;Color&#x26;quot;: &#x26;quot;Details.Color&#x26;quot;,
    &#x26;quot;Size&#x26;quot;: &#x26;quot;Details.Size&#x26;quot;,
    &#x26;quot;Stock&#x26;quot;: 1,
    &#x26;quot;Details&#x26;quot;: 1
}"
</code></pre></td></tr><tr><td>Column Name</td><td>The name(s) of your source column(s)</td><td>"id"<br>"name"<br>"price"<br>"colour"<br>"size"<br>"stock"<br>"$" (<em>This is used to retrieve the full document.)</em><br>"Details" <em>(This is imported both as set of fields (flattened from the projection) and as a JSON.)</em></td></tr><tr><td>dataType</td><td>The data type of your source column</td><td>"Text"<br>"Text"<br>"Number"<br>"Text"<br>"Text"<br>"Number"<br>"Text"<br>"Text"</td></tr><tr><td>isMandatory</td><td>Whether the column is mandatory or not</td><td>"false"</td></tr><tr><td>validateData</td><td>Whether the column data needs to be validated or not</td><td>"false"</td></tr></tbody></table>

### 1.1 XML Example

```xml
<BatchDataSyncConfig name=""MongoDB Data Source Example"" version=""1.0.0""
    xmlns=""http://www.cinchy.co"">
    <MongoCollectionDataSource connectionString=""AI+FJVIMO1HP/CkZ5yphXeJ01wjH/4ilJ8xAIPPDyxvYq0oiYnVBQrzaq2Cp5942poeDdOp"" database=""test"" collection=""Articles"" type=""find"" query=""{ &quot;Price&quot;: { &quot;$lt&quot;: 10 } }"" projection=""{
    &quot;Name&quot;: 1,
    &quot;Price&quot;: 1,
    &quot;Color&quot;: &quot;Details.Color&quot;,
    &quot;Size&quot;: &quot;Details.Size&quot;,
    &quot;Stock&quot;: 1,
    &quot;Details&quot;: 1
}"">
        <Schema>
            <Column name=""_id"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Name"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Price"" dataType=""Number"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Color"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Size"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Stock"" dataType=""Number"" isMandatory=""false"" validateData=""false""/>
            <Column name=""$"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
            <Column name=""Details"" dataType=""Text"" trimWhitespace=""true"" isMandatory=""false"" validateData=""false""/>
        </Schema>
    </MongoCollectionDataSource>
```
