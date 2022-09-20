---
description: API methods to work with Subscriptions
---

# Subscriptions Collection

{% hint style="danger" %}
**DEPRECATED**

This API is deprecated. Please, use [Webhooks API](webhook-collection.md) instead.
{% endhint %}

You can create **Subscriptions** to be notified about any changes at Property ARI or about Booking changes.\
You can think about **Subscriptions** like Push-notifications. When any  changes happens at Property ARI or bookings, we send a webhook to the  provided endpoint and send the changed data.

## Subscription List

Retrieve a list of Subscriptions associated with users Properties.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/subscriptions
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "id": "a9a88747-767e-458c-99e0-9212fcfb0b39",
      "type": "subscription",
      "attributes": {
        "request_params": null,
        "headers": null,
        "is_active": true,
        "send_data": true,
        "id": "a9a88747-767e-458c-99e0-9212fcfb0b39",
        "event_mask": "*",
        "callback_url": "https://YOUR-WEBSITE.COM/api/push_message"
      },
      "relationships": {
        "property": {
          "data": {
            "type": "property",
            "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
          }
        }
      }
    }
  ],
  "meta": {
    "page": 1,
    "total": 1,
    "limit": 10
  }
}
```
{% endtab %}

{% tab title="Error Response" %}
**Unauthorised Error Response**

Status Code: `401 Unauthorized`

```javascript
{
  "errors": {
    "code": "unauthorized",
    "title": "Unauthorized"
  }
}
```
{% endtab %}
{% endtabs %}

### Pagination

By default, this method returns the first 10 elements. To get more details, you should use [Pagination](./api-v.1-documentation/api-reference#pagination) arguments.\
Information about count of entities and current pagination position contained at `meta` section at response object.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Subscription objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

## Get Subscription by ID

Retrieve specific subscription associated with User by ID.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/subscriptions/:id
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "id": "a9a88747-767e-458c-99e0-9212fcfb0b39",
    "type": "subscription",
    "attributes": {
      "request_params": null,
      "headers": null,
      "is_active": true,
      "send_data": true,
      "id": "a9a88747-767e-458c-99e0-9212fcfb0b39",
      "event_mask": "*",
      "callback_url": "https://YOUR-WEBSITE.COM/api/push_message"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Error Response" %}
**Unauthorised Error Response**

Status Code: `401 Unauthorized`

```javascript
{
  "errors": {
    "code": "unauthorized",
    "title": "Unauthorized"
  }
}
```

**Not Found Error Response**

Status Code: `404 Not Found`

```json
{
    "errors": {
        "code": "resource_not_found",
        "title": "Resource Not Found"
    }
}
```
{% endtab %}
{% endtabs %}

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Subscription object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Subscription.

**Not Found Error**\
Method can return a Not Found Error result with `404 Not Found` HTTP Code if Subscription with provided ID is not exists.

## Create Subscription

Create a new Subscription.

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/subscriptions
```

Query body (JSON):

```javascript
{
  "subscription": {
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "callback_url": "https://YOUR-WEBSITE.COM/api/push_message",
    "event_mask": "*",
    "request_params": {},
    "headers": {},
    "is_active": true,
    "send_data": true
  }
}
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `201 Created`

```javascript
{
  "data": {
    "type": "subscription",
    "id": "227d4b09-7eef-4c7e-bc72-44c0c40b5299",
    "attributes": {
      "request_params": {},
      "headers": {},
      "is_active": true,
      "send_data": true,
      "id": "227d4b09-7eef-4c7e-bc72-44c0c40b5299",
      "event_mask": "*",
      "callback_url": "https://website.com/api/push_message"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "52397a6e-c330-44f4-a293-47042d3a3607"
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Error Response" %}
**Unauthorised Error Response**

Status Code: `401 Unauthorized`

```javascript
{
  "errors": {
    "code": "unauthorized",
    "title": "Unauthorized"
  }
}
```

**Validation Error Response**

Status Code: `422 Unprocessable Entity`

```javascript
{
  "errors": {
    "code": "validation_error",
    "title": "Validation Error",
    "details": {
      "property_id": [
        "can't be blank"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Fields

**property\_id `[required]`**

String with valid UUID of Property object what you would like to associate with created Subscription.

**callback\_url `[required]`**

Valid URL address.\
Note: This URL will be called via POST request when trigger event is happened.

**event\_mask `[required]`**

Non-empty string with event mask.\
Note: Right now we have two events, what can trigger Subscription - `ari` and `booking`. You can specify different endpoints for different events using `event_mask` field or subscribe to any event by passing `*` as event\_mask.\
For `ari` event event mask support filtering by restriction, room type id and rate plan id. In that case, event mask should looks like: `event:restrictions:room_type_ids:rate_plan_ids` where restrictions, room\_type\_ids and rate\_plan\_ids can contain several comma separated values.\
Real example to listen rate changes at Rate Plan with ID equal to `96a44e07-2158-43e4-8baa-8f6f56922ba8`:\
`ari:rate:*:96a44e07-2158-43e4-8baa-8f6f56922ba8`

**request\_params `[optional]`**

JSON Object with specific GET arguments for query.\


**headers `[optional]`**

JSON Object with request headers.\
Note: If you would like use URL endpoint protected via authentication, you can define request headers at this field.\
Example:`{"Authorization": "Basic user:password"}`

**is\_active `[optional]`**

Boolean value.\
Note: This field represent active status of Subscription. Only Subscriptions with `is_active` field equal to `true` value can receive notifications.\
Receive `false` as default value.

**send\_data `[optional]`**

Boolean value.\
Note: This field is a flag to send payload data in push callback. If value is `false` we are call callback url without any information about changes.\
Receive `false` as default value.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Subscription object in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Update Subscription

Update a Subscription.

{% tabs %}
{% tab title="Request" %}
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/subscriptions/:id
```

Query body (JSON):

```javascript
{
  "subscription": {
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "callback_url": "https://YOUR-WEBSITE.COM/api/push_message",
    "event_mask": "ari",
    "request_params": {},
    "headers": {},
    "is_active": true,
    "send_data": true
  }
}
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "type": "subscription",
    "id": "227d4b09-7eef-4c7e-bc72-44c0c40b5299",
    "attributes": {
      "request_params": {},
      "headers": {},
      "is_active": true,
      "id": "227d4b09-7eef-4c7e-bc72-44c0c40b5299",
      "event_mask": "ari",
      "callback_url": "https://website.com/api/push_message"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "52397a6e-c330-44f4-a293-47042d3a3607"
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Error Response" %}
**Unauthorised Error Response**

Status Code: `401 Unauthorized`

```javascript
{
  "errors": {
    "code": "unauthorized",
    "title": "Unauthorized"
  }
}
```

**Not Found Error**

Status Code: `404 Not Found`

```javascript
{
  "errors": {
    "code": "resource_not_found"
    "title": "Resource Not Found"
  }
}
```

**Validation Error Response**

Status Code: `422 Unprocessable Entity`

```javascript
{
  "errors": {
    "code": "validation_error",
    "title": "Validation Error",
    "details": {
      "property_id": [
        "can't be blank"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Fields

This method use same fields as Create Subscription method.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Subscription object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Subscription with provided ID is not present at system.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Remove Subscription

Remove a Subscription.

{% tabs %}
{% tab title="Request" %}
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/subscriptions/:id
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "meta": {
    "message": "Success"
  }
}
```
{% endtab %}

{% tab title="Error Response" %}
**Unauthorised Error Response**

Status Code: `401 Unauthorized`

```javascript
{
  "errors": {
    "code": "unauthorized",
    "title": "Unauthorized"
  }
}
```

**Not Found Error**

Status Code: `404 Not Found`

```javascript
{
  "errors": {
    "code": "resource_not_found"
    "title": "Resource Not Found"
  }
}
```
{% endtab %}
{% endtabs %}

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Meta object with message in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Subscription with provided ID is not present at system.



## Test Subscription

Test a Subscription by sending test query to your endpoint.

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/subscriptions/test
```

Query body (JSON):

```javascript
{
  "subscription": {
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "callback_url": "https://YOUR-WEBSITE.COM/api/push_message",
    "event_mask": "*",
    "request_params": {},
    "headers": {},
    "is_active": true,
    "send_data": true
  }
}
```
{% endtab %}

{% tab title="Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "status_code": 200,
  "body": "{\n  \"success\": true\n}"
}
```
{% endtab %}
{% endtabs %}

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code with body and status code of request results to your endpoint.

## Subscription Message Examples

### ARI Changes without payload

{% tabs %}
{% tab title="Subscription Config" %}
```javascript
{
  "subscription": {
    "property_id": "PROPERTY_ID",
    "callback_url": "https://your-service.com/api/webhook",
    "event_mask": "*",
    "request_params": {},
    "headers": {},
    "is_active": true,
    "send_data": false
  }
}
```
{% endtab %}

{% tab title="Message Example" %}
```javascript
{
  "event": "ari",
  "property_id": "PROPERTY_ID",
  "user_id": "USER_ID"
}
```
{% endtab %}
{% endtabs %}

### ARI Changes with payload

{% tabs %}
{% tab title="Subscription Config" %}
```javascript
{
  "subscription": {
    "property_id": "PROPERTY_ID",
    "callback_url": "https://your-service.com/api/webhook",
    "event_mask": "*",
    "request_params": {},
    "headers": {},
    "is_active": true,
    "send_data": true
  }
}
```
{% endtab %}

{% tab title="Message Example" %}
```javascript
{
  "event": "ari",
  "payload": [
    {
      "date": "2020-12-19",
      "rate": 11100,
      "rate_plan_id": "14073af6-c13b-4ad9-90c4-6478fc243eda",
      "room_type_id": "98509ae6-8586-4fc1-85f9-ec5221120def"
    },
    {
      "date": "2020-12-19",
      "rate": 12100,
      "rate_plan_id": "7f2c0154-c096-4871-94da-86da34d6ae4b",
      "room_type_id": "98509ae6-8586-4fc1-85f9-ec5221120def"
    },
    {
      "date": "2020-12-19",
      "rate": 11100,
      "rate_plan_id": "aea8c0e0-ef7f-4e93-b5bf-1ce764078137",
      "room_type_id": "98509ae6-8586-4fc1-85f9-ec5221120def"
    },
    {
      "date": "2020-12-19",
      "rate": 11100,
      "rate_plan_id": "b1a8af13-c253-495e-a2f4-a89bd0fa053a",
      "room_type_id": "98509ae6-8586-4fc1-85f9-ec5221120def"
    },
    {
      "date": "2020-12-19",
      "rate": 11100,
      "rate_plan_id": "f7eed33e-dfb4-4ae4-b081-36e88010cf27",
      "room_type_id": "98509ae6-8586-4fc1-85f9-ec5221120def"
    }
  ],
  "property_id": "PROPERTY_ID",
  "user_id": "USER_ID"
}
```
{% endtab %}
{% endtabs %}

ARI Changes webhook will contain information about User (`user_id`), who trigger that changes. This information will help you to identify should you apply this changes at your side or not.

**Stop Sell and Manual Stop Sell**

At {{APPLICATION_NAME}} we have 2 levels of Stop Sell:

* automatic (`stop_sell`)
* manual (`stop_sell_manual`)

Automatic Stop Sell is calculated value and have `true` value if specific date not have available rooms, have 0 rate or have enabled manual Stop Sell.

Manual Stop Sell is represent values installed by User.

To prevent any potential problems, if you subscribe to ARI changes and apply it to your inventory state, please, ignore `stop_sell` values and use only `stop_sell_manual` which represent values installed by User.

### Booking Message

{% tabs %}
{% tab title="Subscription Config" %}
```javascript
{
  "subscription": {
    "property_id": "PROPERTY_ID",
    "callback_url": "https://your-service.com/api/webhook",
    "event_mask": "*",
    "request_params": {},
    "headers": {},
    "is_active": true,
    "send_data": false
  }
}
```
{% endtab %}

{% tab title="Message Example" %}
```javascript
{
  "event": "booking",
  "payload": {
    "booking_id": "0111ac60-23aa-4131-910d-1ed64c31a563",
    "property_id": "605f075a-74ad-4bb5-a3f0-7a306e2c04fe",
    "revision_id": "caaa8ea0-c7aa-47c0-b181-ae3134db53cf"
  },
  "property_id": "605f075a-74ad-4bb5-a3f0-7a306e2c04fe"
}
```
{% endtab %}
{% endtabs %}

Please, keep in mind, by security reasons, we are not provide Full Booking info if `send_data` is `true`.
