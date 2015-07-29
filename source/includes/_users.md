# Users

## Get All Users

```shell
curl "https://api.qiplans.com/users"
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
        "company_id": 1
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
        "company_id": 2
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
        "next": "http://api.qiplans.com/users?page=2"
      }
    }
  }
}
```

This endpoint retrieves all users paginated.

### HTTP Request

`GET /users`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
sort | id | Specify the sorting. Can list one or more fields (comma separated). Each field can be prefixed with a minus "-" sign to indicate reverse order. So `sort=last_name,-id` would order users by last_name, newest first.
quickSearch |  | Perform a quick search on first_name, last_name, username, email, and user name.
page | 1 | Paginates results.

## Get a Specific User

```shell
curl "http://api.qiplans.com/users/1"
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
      "last_login": "Mon, 06 Apr 15 20:22:03 +0000",
      "last_login_approx": "3 months ago",
      "notes": ""
    }
  }
}
```
> This is a simplified list of attributes, the API will return more than what you see here.

This endpoint retrieves a specific user.

### HTTP Request

`GET /users/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the user to retrieve
include | Set to `company` to fetch the related company as well

## Create a User

```shell
curl "http://api.qiplans.com/users"
  -X POST
  -d '{ "first_name" : "Gerald", "last_name" : "Gustaf", "email" : "gg@example.com", "phone" : "111-222-3333", "company_id" : 1 }'
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
```

> The above command returns the new user JSON structure. See above under "Get a Specific User" for an example.


This endpoint created a new user.

The body of the POST request must include a properly formatted JSON document.

### HTTP Request

`POST /users`

### JSON Body Parameters

Parameter | Required | Comments
--------- | ----------- | -----------
first_name | **Yes**
last_name | **Yes**
username | No | If not specified, the email address will be used as the username
password | No | If not specified, a random password will be generated
email | **Yes**
phone | No
title | No
member | No | Only applicable if your planroom is setup as a membership planroom
verified | No
confirmed | No | Will be set to `true` by default
notes | No
company_id | **Yes** | Must reference an existing company

## Update a User

```shell
curl "http://api.qiplans.com/users/1"
  -X PATCH
  -d '{ "first_name" : "Jerry" }'
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$response = $client->patch("/users/1", [
  'json' => ["first_name" => "Jerry"]
]);
```

> The above command returns the updated user JSON structure. See above under "Get a Specific User" for an example.


This endpoint updated a specific user.

The body of the request must include a properly formatted JSON document.

### HTTP Request

`PATCH /users/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the user to update

### JSON Body Parameters

See parameters under [Create a User](#create-a-user)

## Delete a User

```shell
curl "http://api.qiplans.com/users/1"
  -X DELETE
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

Parameter | Description
--------- | -----------
ID | The ID of the user to delete
