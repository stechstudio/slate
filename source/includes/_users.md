# Users

## Get All Users

```shell
curl "https://api.qiplans.com/v1/users" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->get("/users");
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "type": "users",
      "id": 1,
      "attributes": {
        "first_name": "Ed",
        "last_name": "Carter",
        "name": "Ed Carter",
        "username": "edcarter",
        "email": "edcarter@example.com",
        "title": "",
        "phone": "",
        "mobile": "",
        "fax": "",
        "confirmed": true,
        "verified": true,
        "member": false,
        "credit_balance": 0,
        "company_id": 1,
        "last_login": "Mon, 01 Feb 15 16:01:29 +0000",
        "last_login_approx": "5 months ago"
      },
      "links": {
        "self": "https://api.qiplans.com/v1/users/1"
      }
    },
    {
      "type": "users",
      "id": 2,
      "attributes": {
        "first_name": "John",
        "last_name": "Doe",
        "name": "John Doe",
        "username": "john.doe",
        "email": "jdoe@example.com",
        "title": "Estimator",
        "phone": "",
        "mobile": "",
        "fax": "",
        "confirmed": true,
        "verified": true,
        "member": false,
        "credit_balance": 0,
        "company_id": 2,
        "last_login": "Mon, 06 Apr 15 20:22:03 +0000",
        "last_login_approx": "3 months ago"
      },
      "links": {
        "self": "https://api.qiplans.com/v1/users/2"
      }
    }
    <snip>
  ],
  "links": {
    "self": "https://api.qiplans.com/v1/users"
  },
  "meta": {
    "pagination": {
      "total": 150,
      "count": 100,
      "per_page": 100,
      "current_page": 1,
      "total_pages": 2,
      "links": {
        "next": "https://api.qiplans.com/v1/users?page=2"
      }
    }
  }
}
```

This endpoint retrieves all users paginated.

### HTTP Request

`GET /users`

### Query Parameters

Parameter   | Type   | Default  | Details
---------   | ----   | -------  | -------
sort        | string | id       | Specify the sorting. Can list one or more fields (comma separated). Each field can be prefixed with a minus "-" sign to indicate reverse order. So `sort=last_name,-id` would order users by last_name, newest first.
quickSearch | string |          | Perform a quick search on first_name, last_name, username, email, and user name. The search is case insensitive and will return users where any of these fields *contain* the value specified.
page        | number | 1        | Specify the page of results to fetch.

## Advanced User Search

```shell
curl "https://api.qiplans.com/v1/users/searches" \
  -X POST \
  -d '[ ["first_name","=","Fred"], ["last_login",">","2015-08-01"] ]' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->post("/users/searches", [
  'json' => [
    ["first_name","=","Fred"],
    ["last_login",">","2015-08-01"]
  ]
]);
```

> This will return a list of users that match the criteria. The structure will be exactly the same as what you would get from 'Get All Users' above.

This endpoint provides powerful, precise searching of users.

When you submit a search request, you will be redirected to the search result. Make sure your HTTP client follows redirects. The search results will be coming from the 'Get All Users' endpoint, with a `search` query param added. You will then be able to paginate through your search results just like you would for a normal call to 'Get All Users' above.

### HTTP Request

`POST /users/searches`

### JSON Body

The request body must be a JSON array, with each search criterion specified as a sub-array with exactly three elements. The first element is the field name, the second is the comparison operator, and the third is the value.

For example, to find users that have a last name of Smith, you would submit this request:

`[ ["last_name","=","Smith"] ]`

To specify multiple criteria, simply create multiple sub-arrays:

`[ ["last_name","=","Smith"], ["email","like","%gmail%"] ]`

Note the percent symbols around 'gmail' above. This will find users where the email *contains* 'gmail' anywhere. If you wanted instead to find user that *ends exactly* with 'gmail.com' just use the first percent symbol:

`[ ["last_name","=","Smith"], ["email","like","%gmail.com"] ]`

Furthermore you could request users that have logged in since a particular date:

`[ ["last_name","=","Smith"], ["email","like","%gmail.com"], ["last_login",">","2015-08-01"] ]`

### Searchable Fields

These are the fields where searching is permitted. Note the field type, make sure to only use comparison operators that support the field type.

Field     | Type  | Details
--------- | ----- | -------
id        | number
first_name | string
last_name | string
username  | string
email     | string
phone     | string
fax       | string
mobile    | string
confirmed | boolean
verified  | boolean
last_login | date
created   | date
company_id | number

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

## Get a Specific User

```shell
curl "https://api.qiplans.com/v1/users/1" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->get("/users/1");
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "type": "users",
    "id": 1,
    "attributes": {
      "first_name": "Ed",
      "last_name": "Carter",
      "name": "Ed Carter",
      "username": "edcarter",
      "email": "edcarter@example.com",
      "title": "",
      "phone": "",
      "mobile": "",
      "fax": "",
      "confirmed": true,
      "verified": true,
      "member": false,
      "credit_balance": 0,
      "company_id": 1,
      "last_login": "Mon, 01 Feb 15 16:01:29 +0000",
      "last_login_approx": "5 months ago",
      "notes": "Notes about this user"
    },
    "company": {
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
          "fax": "630-447-0045",
          "blasts_access": true,
          "tax_exempt": false,
          "address_formatted": "3601 Sweeten Creek Road\nSte 4\nArden, NC 28704",
          "notes": "Notes about this company"
        },
        "links": {
          "self": "https://api.qiplans.com/v1/companies/1"
        }
      }
    },
    "links": {
      "self": "https://api.qiplans.com/v1/users/1"
    }
  }
}
```

This endpoint retrieves a specific user.

### HTTP Request

`GET /users/<ID>`

### URL Parameters

Parameter | Type    | Details
--------- | ------  | -------
ID        | number  | The ID of the user to retrieve

### Query Parameters

Parameter | Type   | Details
--------- | ----   | -------
include   | string | Set to `company` to fetch the related company as well

## Create a User

```shell
curl "https://api.qiplans.com/v1/users" \
  -X POST \
  -d '{ "first_name" : "Gerald", "last_name" : "Gustaf", "email" : "gg@example.com", "phone" : "111-222-3333", "company_id" : 1 }' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"

# OR

curl "https://api.qiplans.com/v1/users" \
  -X POST \
  -d '{ "first_name" : "Gerald", "last_name" : "Gustaf", "email" : "gg@example.com", "phone" : "111-222-3333", "company" : { "name" : "New Company Name", "phone" : "555-555-5555" } }' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->post("/users", [
  'json' => [
    "first_name" => "Gerald",
    "last_name" => "Gustaf",
    "email" => "gg@example.com",
    "phone" => "111-222-3333",
    "company_id" => 1
  ]
]);

// OR

$response = $client->post("/users", [
  'json' => [
    "first_name" => "Gerald",
    "last_name" => "Gustaf",
    "email" => "gg@example.com",
    "phone" => "111-222-3333",
    "company" => [
      "name" => "New Company Name",
      "phone" => "555-555-5555"
    ]
  ]
]);
```

> The above command returns the new user JSON structure. See above under "Get a Specific User" for an example.


This endpoint creates a new user.

The body of the POST request must include a properly formatted JSON document.

### HTTP Request

`POST /users`

### JSON Body Parameters

Parameter   | Type    | Required  | Details
---------   | ----    | --------- | -----------
first_name  | string  | **Yes**
last_name   | string  | **Yes**
username    | string  | No        | If not specified, the email address will be used as the username
password    | string  | No        | Requires minimum 8 characters, uppercase and lowercase letters, and at least one number. If not specified, a random password will be generated.
email       | string  | **Yes**
phone       | string  | No
title       | string  | No
member      | boolean | No        | Only applicable if your planroom is setup as a membership planroom
verified    | boolean | No
confirmed   | boolean | No        | Will be set to `true` by default. If `false` the user will have to confirm their email address before logging in.
notes       | string  | No
company_id  | number  | **Yes**   | Must reference an existing company. You must provide this ID *or* a nested `company` object.
company     | object  | **Yes**   | Valid nested JSON object for a new company. See [Create a Company](#create-a-company) for details. You must provide this company object *or* an existing `company_id`.

## Update a User

```shell
curl "//api.qiplans.com/v1/users/1" \
  -X PATCH \
  -d '{ "first_name" : "Jerry" }' \
  -H "Content-Type: application/json" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->patch("/users/1", [
  'json' => ["first_name" => "Jerry"]
]);
```

> The above command returns the updated user JSON structure. See above under "Get a Specific User" for an example.


This endpoint updates a specific user.

The body of the request must include a properly formatted JSON document.

### HTTP Request

`PATCH /users/<ID>`

### URL Parameters

Parameter | Type    | Details
--------- | ----    | -------
ID        | number  | The ID of the user to update

### JSON Body Parameters

See parameters under [Create a User](#create-a-user)

## Delete a User

```shell
curl "https://api.qiplans.com/v1/users/1" \
  -X DELETE \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->delete("/users/1");
```

> The above command returns a '200 OK' HTTP response if successful, with an empty body.


This endpoint deletes a specific user.

### HTTP Request

`DELETE /users/<ID>`

### URL Parameters

Parameter | Type    | Details
--------- | ----    | -------
ID        | number  | The ID of the user to delete
