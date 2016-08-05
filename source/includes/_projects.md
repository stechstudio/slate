# Projects

## Get All Projects

```shell
curl "https://api.qiplans.com/v1/projects" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->get("/projects");
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "type": "projects",
      "id": 1,
      "attributes": {
        "name": "Millington High School",
        "slug": "millington-high-school",
        "created": "Sat, 07 Apr 12 20:43:39 +0000",
        "created_approx": "4 years ago",
        "access": "private",
        "key": "mykey",
        "locked": true,
        "description": "Brief project description",
        "location": "Chicago, IL",
        "company_id": 1,
        "company_name": "ReproConnect",
        "contact_name": "Joseph",
        "contact_phone": "",
        "prebid": "Wed, 17 Aug 16 05:00:00 +0000",
        "prebid_ddt": "Wed, Aug 17, 2016 5:00 AM",
        "prebid_approx": "1 week from now",
        "bid": "Thu, 25 Aug 16 05:00:00 +0000",
        "bid_ddt": "Thu, Aug 25, 2016 5:00 AM",
        "bid_approx": "2 weeks from now",
        "bid_status": "Accepting Bids",
        "notes": "Detailed project notes in textile format"
      },
      "links": {
        "self": "https://api.qiplans.com/v1/projects/1"
      }
    },
    <snip>
  ],
  "links": {
    "self": "https://api.qiplans.com/v1/projects"
  },
  "meta": {
    "pagination": {
      "total": 150,
      "count": 100,
      "per_page": 100,
      "current_page": 1,
      "total_pages": 2,
      "links": {
        "next": "https://api.qiplans.com/v1/projects?page=2"
      }
    }
  }
}
```

This endpoint retrieves all projects paginated.

### HTTP Request

`GET /projects`

### Query Parameters

Parameter   | Type   | Default  | Details
---------   | ----   | -------  | -------
sort        | string | id       | Specify the sorting. Can list one or more fields (comma separated). Each field can be prefixed with a minus "-" sign to indicate reverse order. So `sort=name,-id` would order companies by name, newest first.
quickSearch | string |          | Perform a quick search on project name, description, or company name. The search is case insensitive and will return projects where any of these fields *contain* the value specified.
page        | number | 1        | Specify the page of results to fetch.

## Advanced Project Search

```shell
curl "https://api.qiplans.com/v1/projects/searches" \
  -X POST \
  -d '[ ["name","like","Millington%"], ["created",">","2015-01-01"] ]' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->post("/projects/searches", [
  'json' => [
    ["name","like","Millington%"],
    ["created",">","2015-01-01"]
  ]
]);
```

> This will return a list of projects that match the criteria. The structure will be exactly the same as what you would get from 'Get All Projects' above.

This endpoint provides powerful, precise searching of projects.

When you submit a search request, you will be redirected to the search result. Make sure your HTTP client follows redirects. The search results will be coming from the 'Get All Projects' endpoint, with a `search` query param added. You will then be able to paginate through your search results just like you would for a normal call to 'Get All Projects' above.

### HTTP Request

`POST /projects/searches`

### JSON Body

The request body must be a JSON array, with each search criterion specified as a sub-array with exactly three elements. The first element is the field name, the second is the comparison operator, and the third is the value.

For example, to find projects that belong to a particular company name, you would submit this request:

`[ ["company","=","ReproConnect"] ]`

To specify multiple criteria, simply create multiple sub-arrays:

`[ ["company","=","%Connect%"], ["bid",">","2016-01-01"] ]`

Note the percent symbols around 'ABC' above. This will find companies where the company name *contains* 'Connect' anywhere. If you wanted instead to find companies that *start* with 'Connect' simply omit the first percent symbol:

`[ ["company","=","Connect%"] ]`

Furthermore you could request projects that were created within a certain date range. You could use the `>` or `<` operators here:

`[ ["created",">","2015-01-01"], ["created","<","2015-08-01"] ]`

### Searchable Fields

These are the fields where searching is permitted. Note the field type, make sure to only use comparison operators that support the field type.

Field         | Type    | Details
---------     | -----   | -----------
id            | number
name          | string
description   | string
contact_name  | string
company       | string
bid           | date
prebid        | date
created       | date

### Comparison Operators

Operator  |  Types          | Details
--------  | -------         | -------
=         | all             | Requires a complete, case sensitive match
<         | numbers, dates  | Less than
\>        | numbers, dates  | Greater than
<=        | numbers, dates  | Less than or equal to
\>=       | numbers, dates  | Greater than or equal to
!=        | all             | Not equal to
like      | strings         | Partial match, use in conjunction with the percent symbol
not like  | strings         | Partial match, use in conjunction with the percent symbol
ilike     | strings         | Partial match, case insensitive. Use in conjunction with the percent symbol

## Get a Specific Project

```shell
curl "https://api.qiplans.com/v1/projects/1" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->get("/projects/1");
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "type": "projects",
    "id": 1,
    "attributes": {
      "name": "Millington High School",
      "slug": "millington-high-school",
      "created": "Sat, 07 Apr 12 20:43:39 +0000",
      "created_approx": "4 years ago",
      "access": "private",
      "key": "mykey",
      "locked": true,
      "description": "Brief project description",
      "location": "Chicago, IL",
      "company_id": 1,
      "company_name": "ReproConnect",
      "contact_name": "Joseph",
      "contact_phone": "",
      "prebid": "Wed, 17 Aug 16 05:00:00 +0000",
      "prebid_ddt": "Wed, Aug 17, 2016 5:00 AM",
      "prebid_approx": "1 week from now",
      "bid": "Thu, 25 Aug 16 05:00:00 +0000",
      "bid_ddt": "Thu, Aug 25, 2016 5:00 AM",
      "bid_approx": "2 weeks from now",
      "bid_status": "Accepting Bids",
      "notes": "Detailed project notes in textile format",
      <snip additional project attributes>
    }
  },
  "links": {
    "self": "https://api.qiplans.com/v1/projects/1"
  }
}
```

This endpoint retrieves a specific project.

### HTTP Request

`GET /projects/<ID>`

### URL Parameters

Parameter | Type    | Details
--------- | ----    | -------
ID        | number  | The ID of the project to retrieve

### Query Parameters

Parameter | Type   | Details
--------- | ----   | -------
include   | string | Set to `company` to fetch all the details for the owning company
