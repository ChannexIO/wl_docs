---
description: Documentation for {{APPLICATION_NAME}} HTTP JSON-based API version 1.0
APPLICATION_DOMAIN: app.io
APPLICATION_NAME: app_name
STAGING_DOMAIN: staging.app.io
---

# API Reference

## API Reference

The [{{APPLICATION_NAME}}]({{APPLICATION_DOMAIN}}) API is organised around REST. Our API has a predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. We support cross-origin resource sharing, allowing you to interact securely with our API from a client-side web application. JSON is returned by all API responses, including errors.

API support `GET`, `POST`, `PUT` and `DELETE` queries.

Each response is valid JSON object and **MUST** contain at least one key: `error`, `meta` or `data`.

If response has success status, it **MUST** contain `data` or `meta` key at response object.

`data` object **CAN** be an Object or Array of Objects.

Each `data` object contain `type` and `attributes` keys with response object definition.

```javascript
{
  "meta": {
    "message": "Human readability message"
  },
  "data": {
    "type": "session",
    "attributes": {
      "token": "Bearer Access Token",
      "system_role": "admin"
    }
  }
}
```

Each `POST` or `PUT` query **MUST** contain a valid JSON Object and use `type` of passed object as key for data.

```javascript
{
  "user": {
    "email": "test@test.com"
  }
}
```

Where `user` is `type` of passed entity.

## Authentication

{{APPLICATION_NAME}} supports API Key access.

### API Key Access

Authentication method, where previously generated API Key is used to sign requests:

```
GET https://{{STAGING_DOMAIN}}/api/v1/properties/ HTTP/1.1
Host: {{STAGING_DOMAIN}}
Content-Type: application/json
user-api-key: uU08XiMgk8a7CrY4xUjAReUIuTrn83R123adaVb8Tf/qMcVTEgriuJhXWs/1Q1P
```

Please, read this [article](api-key-access.md) to get more information.

### JSON Web Token

Early, {{APPLICATION_NAME}} provide support for JSON Web Token auth.

Please, keep in mind, this method is deprecated and can be broken in future by adding 2 Factor Auth or other restrictions.

JSON Web Token will be used only for {{APPLICATION_NAME}} UI side and can be limited and restricted in future. Please, migrate to API Key based access logic as soon as possible.

## Errors

[{{APPLICATION_NAME}}]({{APPLICATION_DOMAIN}}) uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, validation errors, etc.). Codes in the 5xx range indicate an error with [{{APPLICATION_NAME}}]({{APPLICATION_DOMAIN}}) servers.

Each error response **MUST** include `errors` Object with error details.

```javascript
{
  "errors": {
    "code": "validation_error",
    "title": "Validation Error",
    "details": {
      "is_active": [
        "can't be blank"
      ]
    }
  }
}
```

Errors Object **MUST** include `code` and `title` fields, other fields is optional.

## Status Codes

`200 OK`\
Success Response

`400 Bad Request`\
The request was unacceptable, often due to missing a required parameter.

`401 Unauthorized`\
No valid Bearer Token provided.

`403 Forbidden`\
Access forbidden. User not have rights to call this action.

`404 Not Found`\
The requested resource doesn't exist.

`422 Unprocessable Entity`\
Validation Error.

## Pagination

The most List API endpoints at {{APPLICATION_NAME}} by default returns only first 10 elements. To get more elements you should implement Pagination traversing logic. To work with pagination, use next arguments:

```
GET https://{{STAGING_DOMAIN}}/api/v1/{resource}?pagination[page]={X}&pagination[limit]={Y}
```

Where `resource` is requested endpoint, `X` - number of requested page, `Y` - count of elements at response.

Please, keep in mind, `page` counted from 1. Max `limit` value is 100.

To control how much elements associated with current account, you can use `meta` section from response:

```javascript
...
"meta": {
  "limit": 10,
  "page": 1,
  "total": 4
}
...
```

## Order

The most List API Endpoints at {{APPLICATION_NAME}} support order arguments to get elements ordered by requested argument. Order field and direction should be provided as GET argument at API call:

```
GET https://{{STAGING_DOMAIN}}/api/v1/{resource}?order[{field}]={direction}
```

Where `field` is field name for sort, `direction` is one of possible values (`asc` or `desc`).

Most endpoints by default sort entities by `title` field at ascending direction.

## Filtering data arguments

The most API endpoints at [{{APPLICATION_NAME}}]({{APPLICATION_DOMAIN}}) supports filtering data arguments. Our filtering API provide operations to comparison and inclusion checks.

### Basic Concept

Filtering arguments are passed as regular `GET` arguments in the query string under the `filter` prefix. Each field should be wrapped into square brackets: `filter[field]`. To pass list of possible values, use comma symbol: `filter[field]=value1,value2`.

By default symbol `=` mean comparison operator is _equal_ if single value passed or is _includes_ if list of values passed. But you can use other operators, like greater then or less then by passing it as second argument for filter: `filter[field][gte]=value` or `filter[field][lte]=value`. You can use more then one comparison operator for one field, to build conditions like DATE greater then 2019-01-01 and less then 2019-02-01.

### Supported comparison operators

* gt (greater than)
* gte (greater than or equal)
* lt (less than)
* lte (less than or equal)
* eq (equal to) default operation if you pass value after `=` symbol
* not (not equal to)

### Examples

#### Basic Comparison

Field equal provided value.

```
API_ENDPOINT/?filter[property_id]=PROPERTY_ID
```

#### Multiple values

Field should be equal to at least one values from provided list.&#x20;

```
API_ENDPOINT/?filter[property_id]=PROPERTY_ID1,PROPERTY_ID2
```

#### Multiple fields

Pass several filter arguments.

```
API_ENDPOINT/?filter[property_id]=PROEPRTY_ID&filter[room_type_id]=ROOM_TYPE_ID
```

#### Comparison operations

Use greater then and less then comparison operations

```
API_ENDPOINT/?filter[date][gte]=DATE_FROM&filter[date][lte]=DATE_TO
```

## API Sandbox Server

To provide early access to our API and provide ability make some tests we are prepare sandbox server what you can use for your experiments. You can sign up yourself and use your email and password as the sign in keys.

```
https://{{STAGING_DOMAIN}}
```
