# Companies

## Get All Companies

```shell
curl "https://api.qiplans.com/v1/companies" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->get("/companies");
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "type": "companies",
      "id": 1,
      "attributes": {
        "name": "ReproConnect",
        "address_1": "3601 Sweeten Creek Road",
        "address_2": "Ste 4",
        "city": "Arden",
        "state": "NC",
        "zip": "28704",
        "phone": "630-938-7601"
      },
      "links": {
        "self": "https://api.qiplans.com/v1/companies/1"
      }
    },
    {
      "type": "companies",
      "id": 2,
      "attributes": {
        "name": "ABC Construction",
        "address_1": "123 S Main St",
        "address_2": "",
        "city": "Chicago",
        "state": "IL",
        "zip": "12345",
        "phone": ""
      },
      "links": {
        "self": "https://api.qiplans.com/v1/companies/2"
      }
    }
    <snip>
  ],
  "meta": {
    "pagination": {
      "total": 150,
      "count": 100,
      "per_page": 100,
      "current_page": 1,
      "total_pages": 2,
      "links": {
        "next": "https://api.qiplans.com/v1/companies?page=2"
      }
    }
  }
}
```
> This is a simplified list of attributes, the API will return more than what you see here.

This endpoint retrieves all companies paginated.

### HTTP Request

`GET /companies`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
sort | id | Specify the sorting. Can list one or more fields (comma separated). Each field can be prefixed with a minus "-" sign to indicate reverse order. So `sort=state,-id` would order companies by state, newest first.
quickSearch |  | Perform a quick search on company name, address_1, city, state, or phone. The search is case insensitive and will return companies where any of these fields *contain* the value specified.
page | 1 | Specify the page of results to fetch.

## Advanced Company Search

```shell
curl "https://api.qiplans.com/v1/companies/searches" \
  -X POST \
  -d '[ ["name","like","ABC%"], ["created",">","2015-01-01"] ]' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->post("/companies/searches", [
  'json' => [
    ["name","like","ABC%"],
    ["created",">","2015-01-01"]
  ]
]);
```

> This will return a list of companies that match the criteria. The structure will be exactly the same as what you would get from 'Get All Companies' above.

This endpoint provides powerful, precise searching of companies.

When you submit a search request, you will be redirected to the search result. Make sure your HTTP client follows redirects. The search results will be coming from the 'Get All Companies' endpoint, with a `search` query param added. You will then be able to paginate through your search results just like you would for a normal call to 'Get All Companies' above.

### HTTP Request

`POST /companies/searches`

### JSON Body

The request body must be a JSON array, with each search criterion specified as a sub-array with exactly three elements. The first element is the field name, the second is the comparison operator, and the third is the value.

For example, to find companies that are in the state of NC, you would submit this request:

`[ ["state","=","NC"] ]`

To specify multiple criteria, simply create multiple sub-arrays:

`[ ["state","=","NC"], ["name","like","%ABC%"] ]`

Note the percent symbols around 'ABC' above. This will find companies where the company name *contains* 'ABC' anywhere. If you wanted instead to find companies that *start* with 'ABC' simply omit the first percent symbol:

`[ ["state","=","NC"], ["name","like","ABC%"] ]`

Furthermore you could request companies that were created within a certain date range. You could use the `>` or `<` operators here:

`[ ["state","=","NC"], ["name","like","ABC%"], ["created",">","2015-01-01"], ["created","<","2015-01-71"] ]`

### Searchable Fields

These are the fields where searching is permitted. Note the field type, make sure to only use comparison operators that support the field type.

Field | Type | Details
--------- | ----------- | -----------
id | number
name | string
address_1 | string
address_2 | string
city | string
state | string | Will be compared against the uppercase, two-letter abbreviation
zip | string
phone | string
fax | string
tax_exempt | boolean
blasts_access | boolean
created | date

### Comparison Operators

Operator | Types | Details
--------- | ----------- | -----------
= | all | Requires a complete, case sensitive match
< | numbers, dates | Less than
\> | numbers, dates | Greater than
<= | numbers, dates | Less than or equal to
\>= | numbers, dates | Greater than or equal to
!= | all | Not equal to
like | strings | Partial match, use in conjunction with the percent symbol
not like | strings | Partial match, use in conjunction with the percent symbol
ilike | strings | Partial match, case insensitive. Use in conjunction with the percent symbol

## Get a Specific Company

```shell
curl "https://api.qiplans.com/v1/companies/1" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->get("/companies/1");
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "type": "companies",
    "id": 1,
    "attributes": {
      "name": "ReproConnect",
      "address_1": "3601 Sweeten Creek Road",
      "address_2": "Ste 4",
      "city": "Arden",
      "state": "NC",
      "zip": "28704",
      "phone": "630-938-7601",
      "notes": "Notes about this company"
    }
  }
}
```
> This is a simplified list of attributes, the API will return more than what you see here.

This endpoint retrieves a specific company.

### HTTP Request

`GET /companies/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the company to retrieve

### Query Parameters

Parameter | Description
--------- | -----------
include | Set to `users` to fetch all the users for this company as well

## Create a Company

```shell
curl "https://api.qiplans.com/v1/companies" \
  -X POST \
  -d '{ "name" : "Company Name", "phone" : "111-222-3333" }' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->post("/companies", [
  'json' => [ "name" => "Company Name", "phone" => "111-222-3333" ]
]);
```

> The above command returns the new company JSON structure. See above under "Get a Specific Company" for an example.


This endpoint creates a new company.

The body of the POST request must include a properly formatted JSON document.

### HTTP Request

`POST /companies`

### JSON Body Parameters

Parameter | Required | Comments
--------- | ----------- | -----------
name | **Yes**
address_1 | No
address_2 | No
city | No
state | No | Must be a two-letter abbreviation
zip | No
phone | No
fax | No
notes | No
blasts_access | No | Allows access to the private "dashboard" area where verified users can manage contacts and send blasts
tax_exempt | No


## Update a Company

```shell
curl "https://api.qiplans.com/v1/companies/1" \
  -X PATCH \
  -d '{ "name" : "New Company Name" }' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"  
```

```php
<?php
$response = $client->patch("/companies/1", [
  'json' => ["name" => "New Company Name"]
]);
```

> The above command returns the updated company JSON structure. See above under "Get a Specific Company" for an example.


This endpoint updated a specific company.

The body of the request must include a properly formatted JSON document.

### HTTP Request

`PATCH /companies/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the company to update

### JSON Body Parameters

See parameters under [Create a Company](#create-a-company)

## Delete a Company

```shell
curl "https://api.qiplans.com/v1/companies/1" \
  -X DELETE \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->delete("/companies/1");
```

> The above command returns a '200 OK' HTTP response if successful, with an empty body.


This endpoint deletes a specific company.

### HTTP Request

`DELETE /companies/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the company to delete
