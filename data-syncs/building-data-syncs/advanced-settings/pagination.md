# Pagination

## Pagination

When syncing a Data Source, you may have the option to add in extra configuration sections, such as an Pagination, under the "Add a Section" drop down tab in the Connection Experience _(Image 1)._

<figure><img src="../../../.gitbook/assets/image (765).png" alt=""><figcaption><p><em>Image 1: Adding a Pagination Block</em></p></figcaption></figure>

There are two types of pagination available _(Image 2):_

- **Cursor:** The cursor is a key parameter in this type of pagination. You receive a variable named `Cursor` along with the response. It's a pointer that points at a particular item that you must send with a request. The server then uses the cursor to seek the other set of items. Cinchy recommends using cursor-based pagination when dealing with a real-time data set.
- **Offset:** Offset-based pagination is for parameters with a specific limit (the number of results) and offset (the number of records you need to skip). Cinchy recommends using offset-based pagination for static data.

<figure><img src="../../../.gitbook/assets/image (402).png" alt=""><figcaption><p>Image 2: Selecting your type of pagination</p></figcaption></figure>

### Cursor pagination

To set up cursor pagination, fill in the following parameters _(Image 3)_:

- **Type:** Select _Cursor_
- **Next Page URL JSON Path:** This is the JSON Path within the response to the URL for the next page
- **Cursor Key:** This is the key used in the query string to specify the cursor value. This is only required if the cursor returned isn't a fully qualified URL.

<figure><img src="../../../.gitbook/assets/image (430).png" alt=""><figcaption><p>Image 3: Cursor Pagination</p></figcaption></figure>

### Offset pagination

To set up offset pagination, fill in the following parameters _(Image 4)_:

- **Type:** Select _Offset_
- **Limit Key:** The key used in the query string to specify the limit
- **Limit:** The desired page size
- **Offset By:** The offset type that the API uses for pagination. This will be either **Record Number** or **Page Number.**
- **Offset Key:** The key used in the query string to specify the offset
- **Initial Offset:** The starting offset

<figure><img src="../../../.gitbook/assets/image (427).png" alt=""><figcaption><p>Image 4: Offset Pagination</p></figcaption></figure>

{% hint style="warning" %}
**A pagination block is mandatory**, even if the API doesn't return results from multiple pages. You can use the following as the placeholder:

`<Pagination type="OFFSET" limitField="" offsetField="" limit="0" initialOffset="0" />`
{% endhint %}

```markup
<RestApiDataSource
    method="GET or POST"
    format="JSON"
    rootPath="$.result"
    endpoint="https://data.com/api/v1/records/@recordId/details?apiKey=@apiKey">
	<!-- optional -->
  <RequestHeaders>
		<Header name="" isEncrypted="true or false">value goes here</Header>
	</RequestHeaders>
    <!-- optional -->
	<Body>body goes here, only relevant for POST operations</Body>
	<!-- one of the following pagination blocks is required. If there's no pagination you still need a placeholder -->
	<Pagination type="OFFSET"
	    limitField="name of the field in the querystring to use when passing the limit"
	    offsetField="name of the field in the querystring to use when passing the offset"
	    limit="20"
	    initialOffset="0">
	<Pagination type="CURSOR"
	    pathInResponse="JSON path to the field in the response containing the next cursor value"
	    cursorPropertyName="name of the field in the querystring to use when passing the cursor">
  <Schema>
			<Column name="$.id" dataType="Text" />
			<Column name="$.variants.field1" dataType="Text" />
            <!-- you need to use the "item" placeholder if you're looking for fields in an array -->
			<Column name="$.colors.item[1]" dataType="Text" />
      <Column name="$.colors.item[2]" dataType="Text" />
	</Schema>
</RestApiDataSource>
```
