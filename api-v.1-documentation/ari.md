---
description: API methods to work with Availability and Rates information
---

# Availability and Rates

Availability and Restriction information is the core data of {{APPLICATION_NAME}}. This information represents how many rooms your property has to sell and what restrictions you have applied to each rate plan.

NOTE: At {{APPLICATION_NAME}} availability information is represented at two levels: for Rate Plan and for Room Type. At Rate Plan we  provide availability for specific Rate Plan, this value is derived from the Room Type availability. At Room Type we provide the real availability without any changes from modifiers.

## Get Availability Or Restrictions Per Rate Plan

Get Availability and Restrictions data from Rate Plans.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/restrictions?filter[property_id]=716305c4-561a-4561-a187-7f5b8aeb5920&filter[date][gte]=2019-02-01&filter[date][lte]=2019-02-10&filter[restrictions]=rate
```

Query requires two get arguments:

**date**\
Specific date or Date range what you would like to load. Date should be provided as ISO 8601 format `YYYY-MM-DD`.\
Query to get values for specific date:\
`filter[date]=YYYY-MM-DD`\
To get values for Date Range:\
`filter[date][gte]=YYYY-MM-DD&filter[date][lte]=YYYY-MM-DD`

**restrictions**\
List of comma separated restrictions what you would like to load. Supported values:\
\- availability\
\- rate\
\- min\_stay\_arrival\
\- min\_stay\_through\
\- min\_stay\
\- closed\_to\_arrival\
\- closed\_to\_departure\
\- stop\_sell\
\- max\_stay\
\- availability\_offset\
\- max\_sell\
\- max\_availability
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2": {
      "2019-02-01": {
        "rate": "200.00"
      },
      "2019-02-02": {
        "rate": "200.00"
      },
      "2019-02-03": {
        "rate": "200.00"
      },
      "2019-02-04": {
        "rate": "200.00"
      },
      "2019-02-05": {
        "rate": "200.00"
      },
      "2019-02-06": {
        "rate": "200.00"
      },
      "2019-02-07": {
        "rate": "200.00"
      },
      "2019-02-08": {
        "rate": "200.00"
      },
      "2019-02-09": {
        "rate": "200.00"
      },
      "2019-02-10": {
        "rate": "200.00"
      }
    }
  }
}

```
{% endtab %}

{% tab title="Error Response" %}
**Bad Request Error Response**

Status Code: `400 Bad Request`

```javascript
{
  "errors": {
    "code": "bad_request",
    "title": "Bad Request",
    "details": [
      "restrictions is required"
    ]
  }
}
```

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

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Restriction object in the answer.

**Bad Request Error**\
****Method can return a Bad Request Error result with `400 Bad Request` HTTP Code if user pass wrong arguments.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

### Restriction Object

Restriction Object is valid answer for Get Restrictions method. This object contain information about availability and restrictions. Each key at this object is equal to Rate Plan id. Each Rate Plan ID represented as Object with dates as keys. Each date represented as object where keys is restrictions and values is restriction values for specific date.

```
{
  [RATE_PLAN_ID]: {
    [DATE_YYYY-MM-DD]: {
      [RESTRICTION]: [VALUE]
    }
  }
}
```

## Get Availability Per Room Type

Get the Availability per Room Type

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/availability?filter[date][gte]=2019-02-01&filter[date][lte]=2019-02-10
```

Query requires one get arguments:

**date**\
Specific date or Date range what you would like to load. Date should be provided as ISO 8601 format `YYYY-MM-DD`.\
Query to get values for specific date:\
`filter[date]=YYYY-MM-DD`\
To get values for Date Range:\
`filter[date][gte]=YYYY-MM-DD&filter[date][lte]=YYYY-MM-DD`
{% endtab %}

{% tab title="Success Response" %}
Success Response Example

Status Code: 200 OK

```javascript
{
  "data": {
    "994d1375-dbbd-4072-8724-b2ab32ce781b": {
      "2019-02-01": 20,
      "2019-02-02": 20,
      "2019-02-03": 20,
      "2019-02-04": 20,
      "2019-02-05": 20,
      "2019-02-06": 20,
      "2019-02-07": 20,
      "2019-02-08": 20,
      "2019-02-09": 20,
      "2019-02-10": 20
    }
  }
}
```
{% endtab %}

{% tab title="Error Response" %}
**Bad Request Error Response**

Status Code: `400 Bad Request`

```javascript
{
  "errors": {
    "code": "bad_request",
    "title": "Bad Request",
    "details": [
      "date is required"
    ]
  }
}
```

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

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation was successful. Will contain an Availability object in the answer.

**Bad Request Error**\
****Method can return a Bad Request Error result with `400 Bad Request` HTTP Code if the user passes wrong arguments.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

### Availability Object

Availability Object is valid answer for Get Availability method. This object contains information about availability per Room Type. Each key at this object is equal to Room Type ID. Each Room Type ID represented as Object with dates as keys. Each date has a value equal to current Availability.

```
{
  [ROOM_TYPE_ID]: {
    [DATE_YYYY-MM-DD]: [AVAILABILITY]
  }
}
```

## Update Rate & Restrictions

Update Rate & Restrictions for specific Rate Plans and Dates.

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/restrictions
```

Query body (JSON):

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date": "2019-02-20",
    "rate": 30000
  }]
}
```

Date Range update query body (JSON):

This method allow update multiple dates from single message.

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date_from": "2019-02-20",
    "date_to": "2019-02-28",
    "rate": 30000
  }]
}
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "id": "eb31d631-4fcc-478a-80c3-bf7a2acf0699",
      "type": "task"
    }
  ],
  "meta": {
    "message": "Success",
    "warnings": []
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

Validation Error Response

Status Code: `200 OK`

```javascript
{
  "data": [],
  "meta": {
    "message": "Success",
    "warnings": [
      {
        "availability_offset": null,
        "closed_to_arrival": true,
        "closed_to_departure": null,
        "date": "2020-12-16",
        "date_from": null,
        "date_to": null,
        "max_availability": null,
        "max_sell": null,
        "max_stay": null,
        "min_stay_arrival": null,
        "min_stay_through": null,
        "property_id": "5648db98-e082-49e8-a428-2fd3250b47dd",
        "rate": null,
        "rate_plan_id": "87c012d2-26d4-4e97-8ccb-108da06f379e",
        "stop_sell": null,
        "warning": {
          "availability_offset": [
            "Should be a non null value or not existed field"
          ],
          "closed_to_departure": [
            "Should be a non null value or not existed field"
          ],
          "max_stay": [
            "Should be a non null value or not existed field"
          ],
          "min_stay_arrival": [
            "Should be a non null value or not existed field"
          ],
          "min_stay_through": [
            "Should be a non null value or not existed field"
          ],
          "rate": [
            "Should be a non null value or not existed field"
          ],
          "stop_sell": [
            "Should be a non null value or not existed field"
          ]
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

### Fields

Query object should contain values key with list of change objects.\
Each change object should have the next structure:

**property\_id `[required]`**\
String with valid UUID of Property object.

**rate\_plan\_id `[required]`**\
String with valid UUID of Rate Plan object.

**date `[required]`**\
String with date in ISO 8601 format by mask `YYYY-MM-DD`.\
Past dates are not allowed.

**date\_from `[required]` if date is not present**\
****String with date in ISO 8601 format by mask `YYYY-MM-DD`.\
Start of applicable date range.\
Past dates are not allowed.

**date\_to `[required]` if date is not present**\
****String with date in ISO 8601 format by mask `YYYY-MM-DD`.\
End of applicable date range.\
Past dates are not allowed.

**days** **`[optional]`**\
List of days which should be affected by update. Allow names of days at 2 symbol format (`mo, tu, we, th, fr, sa, su`).\
To update each Monday in specific date range, you can send next request:

```javascript
{
  "date_from": "2020-01-01",
  "date_to": "2020-12-31",
  "days": ["mo"],
  "rate": 10000
  ...
}
```

**rate `[optional]`**\
String or Positive Integer value.\
**The value should be greater than 0.**\
****Our API allows 2 ways to pass the rate value:\
Decimal value converted into String ("200.00"), or Integer value with a minimum fraction size of currency (20000 for 200.00 USD).\
Both of these ways allows you to work with Rates and prevents any problems with floating point operations.

**min\_stay\_arrival `[optional]`**\
Positive Integer value.

**min\_stay\_through `[optional]`**\
Positive Integer value.

**min\_stay `[optional]`**\
Positive Integer value. Applicable only if `property.settings.min_stay_type` not equal to `both` value. It is virtual option and provided value will be automatically translated into correct `min_stay_*` value based at `property.settings.min_stay_type` value.

**max\_stay `[optional]`**\
Non-negative Integer value.

**closed\_to\_arrival `[optional]`**\
Boolean value.\
Also, our API allow pass `0` or `1` as Boolean representation.

**closed\_to\_departure `[optional]`**\
Boolean value.\
Also, our API allow pass `0` or `1` as Boolean representation.

**stop\_sell `[optional]`**\
Boolean value.\
Also, our API allow pass `0` or `1` as Boolean representation.

**max\_sell `[optional]`**\
Null or non-negative integer value.

**max\_availability `[optional]`**\
Null or non-negative integer value.

**availability\_offset `[optional]`**\
Non-negative integer value.

### Notes

At least one restriction should be present on the request.

**Last Win logic**\
****All provided updates is processed in FIFO logic, as result, you can use overrides to minimize message size:

```javascript
{
  "values": [
    // Setup rate for whole year to 300.00
    {
      "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
      "date_from": "2020-01-01",
      "date_to": "2020-12-31",
      "rate": 30000
    },
    // Override Saturday and Sunday rate to 350.00 for whole year
    {
      "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
      "date_from": "2020-01-01",
      "date_to": "2020-12-31",
      "days": ["sa", "su"]
      "rate": 35000
    }
  ]
}
```

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if the operation is successful. Will contain a meta object with message in the answer.

**Validation errors**\
****If your request contain wrong data, you will receive warning messages at answer. Please, keep in mind, this response will be marked at Success and will have header `200 OK`. It is happened, because one message can be rejected, but another will be successfully produced.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.&#x20;

### Query Examples

#### Update multiple dates

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date_from": "2019-02-20",
    "date_to": "2019-03-20",
    "rate": 30000
  }]
}
```

#### Update multiple restrictions

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date_from": "2019-02-20",
    "date_to": "2019-03-20",
    "rate": 30000,
    "min_stay_through": 1,
    "closed_to_arrival": true,
    "closed_to_departure": true
  }]
}
```

#### Update multiple rate plans

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "rate_plan_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date_from": "2019-02-20",
    "date_to": "2019-03-20",
    "rate": 30000
  },{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "rate_plan_id": "a07e712e-cb34-4ec9-b085-63e59a88c249",
    "date_from": "2019-02-20",
    "date_to": "2019-03-20",
    "rate": 30000
  }]
}
```

**Multi Occupancy Rate Plan update**

```javascript
{
  "values": [
    {
      "date_from": "2021-03-19",
      "date_to": "2021-03-29",
      "property_id": "fc9f35d6-c810-4305-8eb7-8c04bcffe778",
      "rate_plan_id": "c9c80104-af3b-47b2-8a2f-ba7e544638f3",
      "rates": [
        {
          "occupancy": 1,
          "rate": 12100
        },
        {
          "occupancy": 2,
          "rate": 13200
        }
      ]
    }
  ]
}
```

### Warning Notifications

If a request contains wrong values, we will reject this value from the update and provide a warning message in the response.&#x20;

Keep in mind, if the request contains a few messages, our application process all correct information and ignores the problem ones.

Example of warning message:

```javascript
{
  "data": [],
  "meta": {
    "message": "Success",
    "warnings": [
      {
        "availability_offset": -2,
        "booked": -1,
        "closed_to_arrival": 3,
        "closed_to_departure": 4,
        "date_from": "2020-10-03",
        "date_to": "2020-10-05",
        "max_availability": -2,
        "max_sell": -2,
        "max_stay": -2,
        "min_stay_arrival": -2,
        "min_stay_through": -1,
        "property_id": "954ee839-2598-431b-8c35-8aa68f7b127d",
        "rate": "-2",
        "rate_plan_id": "0db682fe-e86b-43c5-8f0c-d055737f8dd9",
        "stop_sell": 123,
        "warning": {
          "availability_offset": [
            "must be greater than or equal to 0"
          ],
          "closed_to_arrival": [
            "is invalid"
          ],
          "closed_to_departure": [
            "is invalid"
          ],
          "max_availability": [
            "must be greater than or equal to 0"
          ],
          "max_sell": [
            "must be greater than or equal to 0"
          ],
          "max_stay": [
            "must be greater than or equal to 0"
          ],
          "min_stay_arrival": [
            "must be greater than or equal to 1"
          ],
          "min_stay_through": [
            "must be greater than or equal to 1"
          ],
          "rate": [
            "must be greater than 0"
          ],
          "stop_sell": [
            "is invalid"
          ]
        }
      }
    ]
  }
}
```

## Update Availability

Update Availability for a specific Room Type and Date.

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/availability
```

Query body (JSON):

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "room_type_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date": "2019-02-20",
    "availability": 2
  }]
}
```

Date range update query body (JSON):

This method allow you to update several dates with one message.

```javascript
{
  "values": [{
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "room_type_id": "bab451e7-9ab1-4cc4-aa16-107bf7bbabb2",
    "date_from": "2019-02-20",
    "date_to": "2019-02-25",
    "availability": 2
  }]
}
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
{% endtab %}
{% endtabs %}

You can insert multiple dates and ranges into one API call

```javascript
{
  "values": [
    {
      "availability": 10,
      "date": "2021-04-09",
      "property_id": "57a92389-4878-4773-9f0d-47e31d22609f",
      "room_type_id": "f477e6a0-8e9d-4d6f-b506-fb394504d2bc"
    },
    {
      "availability": 11,
      "date": "2021-04-10",
      "property_id": "57a92389-4878-4773-9f0d-47e31d22609f",
      "room_type_id": "f477e6a0-8e9d-4d6f-b506-fb394504d2bc"
    }
  ]
}
```

### Fields

Query object should contain values key with a list of change objects.\
Each change object should have the next structure:

**property\_id `[required]`**\
String with valid UUID of Property object.

**room\_type\_id `[required]`**\
String with valid UUID of Room Type object.

**date `[required] if date_from is not present`**\
String with date in ISO 8601 format by mask `YYYY-MM-DD`.

**date\_from `[required]` if date is not present**\
****String with date in ISO 8601 format by mask `YYYY-MM-DD`.\
Start of applicable date range.

**date\_to `[required]` if date is not present**\
****String with date in ISO 8601 format by mask `YYYY-MM-DD`.\
End of applicable date range.

**availability `[required]`**\
Non-negative Integer value.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if the operation is successful. Will contain a meta object with message in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if a wrong Bearer Token was provided.&#x20;

## Get Availability Or Restrictions Changes

Get Availability or / and Restrictions changes for Rate Plans.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/restrictions/changes?timestamp=2019-05-06T10:00:00&filter[property_id]=716305c4-561a-4561-a187-7f5b8aeb5920&filter[date][gte]=2019-02-01&filter[date][lte]=2019-02-10&filter[restrictions]=rate
```

Query require three get arguments:

**timestamp**\
****Last check timestamp. Can be a valid ISO 8601 date time string or UNIX timestamp. {{APPLICATION_NAME}} will return changes happened after that date.

**property\_id**\
Property ID what changes are you like to get.

**restrictions**\
List of comma separated restrictions what you would like to load. Supported values:\
\- availability\
\- rate\
\- min\_stay\_arrival\
\- min\_stay\_through\
\- min\_stay\
\- closed\_to\_arrival\
\- closed\_to\_departure\
\- stop\_sell\
\- max\_stay\
\- availability\_offset\
\- max\_sell\
\- max\_availability

As optional argument, you can pass date filter to fetch changes for specific date or date range.
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "445835fb-7956-42ac-9efc-3e6f331f0808",
      "rate": 17600,
      "date": "2019-05-07"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "445835fb-7956-42ac-9efc-3e6f331f0808",
      "rate": 17600,
      "date": "2019-05-08"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "5824ae4b-0cdd-441b-a01f-647177b48120",
      "rate": 16600,
      "date": "2019-05-07"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "5824ae4b-0cdd-441b-a01f-647177b48120",
      "rate": 16600,
      "date": "2019-05-08"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "96a44e07-2158-43e4-8baa-8f6f56922ba8",
      "rate": 19600,
      "date": "2019-05-07"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "96a44e07-2158-43e4-8baa-8f6f56922ba8",
      "rate": 19600,
      "date": "2019-05-08"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "f0a84d15-c36f-487c-9c8d-a7f907224a24",
      "rate": 17600,
      "date": "2019-05-07"
    },
    {
      "stop_sell": true,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "f0a84d15-c36f-487c-9c8d-a7f907224a24",
      "rate": 17600,
      "date": "2019-05-08"
    },
    {
      "stop_sell": false,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "445835fb-7956-42ac-9efc-3e6f331f0808",
      "rate": 20000,
      "date": "2020-09-17"
    },
    {
      "stop_sell": false,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "5824ae4b-0cdd-441b-a01f-647177b48120",
      "rate": 19000,
      "date": "2020-09-17"
    },
    {
      "stop_sell": false,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "96a44e07-2158-43e4-8baa-8f6f56922ba8",
      "rate": 22000,
      "date": "2020-09-17"
    },
    {
      "stop_sell": false,
      "room_type_id": "9ab6293a-a96f-4926-9f8a-06bc244aaa7a",
      "rate_plan_id": "f0a84d15-c36f-487c-9c8d-a7f907224a24",
      "rate": 20000,
      "date": "2020-09-17"
    }
  ]
}
```
{% endtab %}

{% tab title="Error Response" %}
**Bad Request Error Response**

Status Code: `400 Bad Request`

```javascript
{
  "errors": {
    "code": "bad_request",
    "title": "Bad Request",
    "details": [
      "restrictions is required"
    ]
  }
}
```

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

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if the operation is successful. Will contain a Restriction object in the answer.

**Bad Request Error**\
****Method can return a Bad Request Error result with `400 Bad Request` HTTP Code if the user passed wrong arguments.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if the wrong Bearer Token was provided.
