# Webhook Collection

You can create **Webhooks** to be notified about any changes of the property's ARI or about booking and some OTA specific webhooks.\
**Webhooks** are Push-notifications. When any changes happens, we send a POST request with JSON payload to the provided endpoint.



**Webhook UI**

We have an UI available to view, add and edit webhooks also so you can lessen your development efforts if you wish to manually set up instead of via API.

Link: [https://{{domain}}/webhooks](https://{{domain}}/webhooks)



## List of webhook events available

* ari&#x20;

This will send you any ARI changes like changed availability or prices, useful if you allow users to change ARI in the {{APPLICATION_NAME}} interface or mobile app and also to integrate any RM system

* booking

If you wish to get all booking changes then this is the one, you will get notification for any booking revision new, modified and cancelled.

* booking\_unmapped\_room

This will let you know if any bookings were created which were not mapped

* booking\_unmapped\_rate

Similar to unmapped room but this means room is mapped but rate is not

* message

If you use the messaging API this is required to push messages to you in real time

* sync\_error

You can see any sync errors on your dashboard

* reservation\_request

Airbnb specific, You can see a reservation request and can accept or deny

* alteration\_request

Airbnb specific, You can see a alteration request and can accept or deny



## Webhooks List

Retrieve a list of Webhooks associated with users Properties.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/webhooks
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "id": "a9a88747-767e-458c-99e0-9212fcfb0b39",
      "type": "webhook",
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
### Error Response
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


### Pagination

By default, this method returns the first 10 elements. To get more details, you should use [Pagination](./api-v.1-documentation/api-reference#pagination) arguments.\
Information about count of entities and current pagination position contained at `meta` section at response object.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Webhook objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

## Get Webhook by ID

Retrieve specific webhook associated with User by ID.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/webhooks/:id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "id": "a9a88747-767e-458c-99e0-9212fcfb0b39",
    "type": "webhook",
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
### Error Response
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


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Webhook object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Webhook.

**Not Found Error**\
Method can return a Not Found Error result with `404 Not Found` HTTP Code if Webhook with provided ID is not exists.

## Create Webhook

Create a new Webhook.

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/webhooks
```

Query body (JSON):

```javascript
{
  "webhook": {
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

### Success Response
**Success Response Example**

Status Code: `201 Created`

```javascript
{
  "data": {
    "type": "webhook",
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
### Error Response
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


### Fields

**property\_id `[required]`**

String with valid UUID of Property object what you would like to associate with created Webhook.

**callback\_url `[required]`**

Valid URL address.\
Note: This URL will be called via POST request when trigger event is happened.

**event\_mask `[required]`**

Non-empty string with event mask.\
Take a look a list of supported webhook events at [List of webhook events](webhook-collection.md#list-of-webhook-events-available) section.\
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
Note: This field represent active status of Webhook. Only Webhooks with `is_active` field equal to `true` value can receive notifications.\
Receive `false` as default value.

**send\_data `[optional]`**

Boolean value.\
Note: This field is a flag to send payload data in push callback. If value is `false` we are call callback url without any information about changes.\
Receive `false` as default value.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Webhook object in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Update Webhook

Update a Webhook.

### Request
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/webhooks/:id
```

Query body (JSON):

```javascript
{
  "webhook": {
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

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "type": "webhook",
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
### Error Response
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


### Fields

This method use same fields as Create Webhook method.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Webhook object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Webhook with provided ID is not present at system.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Remove Webhook

Remove a Webhook.

### Request
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/webhooks/:id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "meta": {
    "message": "Success"
  }
}
```
### Error Response
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


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Meta object with message in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Webhook with provided ID is not present at system.



## Test Webhook

Test a Webhook by sending test query to your endpoint.

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/webhooks/test
```

Query body (JSON):

```javascript
{
  "webhook": {
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

### Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "status_code": 200,
  "body": "{\n  \"success\": true\n}"
}
```


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code with body and status code of request results to your endpoint.



## Payloads

### No Data Version <a href="#no-data-version" id="no-data-version"></a>

The message is provided to the provided webhook endpoint, depending on the webhook settings (`send_data`). If `send_data` is `true`, {{APPLICATION_NAME}} will push the message payload, if `false` message payload will be removed and target endpoint will only receive `event`, `user_id` and `property_id` fields.

```json
{
  "event": "booking_new",
  "property_id": "90958ec0-9214-4796-873e-4add0d834670",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

#### ARI (Availability, Rates & Restrictions) <a href="#ari-availability-rates-and-restrictions" id="ari-availability-rates-and-restrictions"></a>

Triggered when any changes have happened at the property State or inventory table. We provide information about changed prices, restrictions and availability.

Note: We have included the user ID of who generated the changes, this can be useful if you would like to ignore changes made by your own app.

```json
{
  "event": "ari",
  "payload": [
    {
      "availability": 5,
      "booked": 7,
      "date": "2021-12-02",
      "rate_plan_id": "04c607e4-644d-45ea-ab1a-da920ee36e50",
      "room_type_id": "27e24739-239c-4619-9c30-0dc390f5d7ac",
      "stop_sell": false
    }
  ],
  "property_id": "90958ec0-9713-1196-873e-4add0d834670",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

#### Booking <a href="#booking" id="booking"></a>

Triggered when {{APPLICATION_NAME}} receives a booking revision (New, Cancelled or Modified).

```json
{
  "event": "booking",
  "payload": {
    "booking_id": "e10de9d1-3e2c-431c-b88c-ffca9ed5db5d",
    "property_id": "90958ec0-9713-1196-873e-4add0d834670",
    "revision_id": "80b3b60c-5e24-35c5-ad1b-da67cd704093"
  },
  "property_id": "90958ec0-9713-1396-873e-4add0d834670",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

This event was originally designed to trigger a Pull booking revision operation from the PMS. When this event arrives, we expect the PMS will call `api/v1/booking_revisions/:id`, to pull the new revision and ack it.

Alternatively the PMS can call the Feed endpoint also to get list of all unack bookings.

#### Booking Unmapped Room <a href="#booking-unmapped-room" id="booking-unmapped-room"></a>

Triggered when {{APPLICATION_NAME}} receives a booking revision but can’t map it with existing Room Types. This can happen if the channel is not mapped correctly or if the OTA provides ID which has no mapping.

This event is designed to notify PMS about potential problems at mapping and usually used to trigger notification to support team at PMS side to investigate problems with mapping.

Please, keep in mind, to prevent any potential problems with overbookings, Mapping Issues should be solved in short time-frame and should have high priority.

```json
{
  "event": "booking_unmapped_room",
  "payload": {
    "booking_id": "4995a8d5-552b-4d6c-acc5-cc8ca45bd32a",
    "booking_revision_id": "8a5c7299-611e-4b57-a703-7e146b538750"
  },
  "property_id": "a88cdfa3-2e25-1bcc-ab18-2d4f899ca49b",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

#### Booking Unmapped Rate <a href="#booking-unmapped-rate" id="booking-unmapped-rate"></a>

Triggered when {{APPLICATION_NAME}} receives a booking revision but can’t map it with existing Rate Plans.

This trigger will not come if revision has not mapped Room Type error. Booking Unmapped Room event will mute Booking Unmapped Rate, because when we have no mapped Room it also means rate is not mapped.

This event is designed to notify PMS about potential problems at mapping, but in that case, we can map Room (and correctly process Availability changes), but can’t map to Rate Plan.

```json
{
  "event": "booking_unmapped_rate",
  "payload": {
    "booking_id": "4995a8d5-552b-4d6c-acc5-cc8ca45bd32a",
    "booking_revision_id": "8a5c7299-611e-4b57-a703-7e146b538750"
  },
  "property_id": "a88cdfa3-2e25-3bcc-ab18-2d4f899ca49b",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

#### Message <a href="#message" id="message"></a>

Triggered when new chat message from a guest is registered at {{APPLICATION_NAME}}.

```json
{
  "event": "message",
  "payload": {
    "booking_id": "b4671a17-4440-4da4-9c72-f6afa2d61908",
    "have_attachment": false,
    "attachments": [],
    "id": "9d7e25a0-7602-4454-b3c0-a42afad0fde0"
    "message": "Message content",
    "message_thread_id": "8a632561-ebd3-4dd6-b669-06f560b4510d",
    "ota_message_id": "9f157e80-5275-11ec-a8dd-7d6269b894d7"
  },
  "property_id": "12da6232-a2db-41ac-ba1c-31c934b91c19",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

#### Sync Error <a href="#sync-error" id="sync-error"></a>

Triggered when sync error has happened at a connected channel.

Originally designed to notify PMS about potential problems at existed connections.

```json
{
  "event": "sync_error",
  "payload": {
    "channel": "Agoda"
    "channel_event_id": "e7883431-3df6-412f-8c07-0d5e1d983e0f"
    "channel_id": "d691462b-45d1-4655-9076-072210f2ceca"
    "channel_name": "Agoda"
    "error_type": "general_error"
    "property_name": "Hotel Name"
  },
  "property_id": "d69a591e-9be3-4822-cc95-7374ed13a673",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

#### Reservation Request <a href="#reservation-request" id="reservation-request"></a>

Triggered when {{APPLICATION_NAME}} receives a Reservation Request from Airbnb.

Contains information about requested reservation.

```json
{
  "event": "reservation_request",
  "payload": {
    "bms": BOOKING_MESSAGE,
    "resolved": false
  },
  "property_id": "4b70ec18-9ec1-4f77-8408-628b6477e824",
  "user_id": null,
  "timestamp": "2021-12-24T00:00:00.0000Z"
}
```

Where BOOKING\_MESSAGE is structure equal to regular Booking Revision structure.

## Webhook message sequence <a href="#webhook-message-sequence" id="webhook-message-sequence"></a>

Before you start any integration with webhooks, you should to know -

Sequence of incoming webhook calls can be different from sequence of events which trigger that calls. Webhooks may come out of order.

**Explanation**

At property A we have 2 state changes event:\
Change availability for Room Type A from 0 to 1.\
Change availability for Room Type A from 1 to 0.

This events triggers 2 webhook calls with `ari` type. But when {{APPLICATION_NAME}} sends the first webhook we might catch some network issue at middle level and message failed with a timeout error. This is a temporary error and webhook will be rescheduled. Second webhook had no issues and was sent successfully and would arrive first.

PMS receives webhook #2 with Availability 0.\
First webhook is queued for 2nd attempt to be sent to the target endpoint and this time all went well and we deliver that webhook.

PMS receive webhook #1 with Availability 1.

As a result, if PMS will interpret incoming results, this can cause problems. Instead use information from payload, we suggest to use webhooks as a trigger to execute logic to pull ARI info from {{APPLICATION_NAME}}.

So, in case with Availability changes, we suggest instead using data from payload, trigger a pull request to get values for changed dates.

\
