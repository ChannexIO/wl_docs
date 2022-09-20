---
description: API methods to work with Room Types
---

# Room Types Collection

**Room Type** is an entity to represent accommodation inventory at your properties. Your villa, room or bed at a dormitory can be represented as a Room Type.

{% hint style="info" %}
If your property uses rooms instead of the traditional room type then just create a room type for each room. If you have vacation rentals you need to make a room type for the property since creating a property alone is not enough.
{% endhint %}

## Room Types List

Retrieve a list of Room Types associated with user Properties.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/room_types
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "type": "room_type",
      "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
      "attributes": {
        "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
        "title": "Standard Room",
        "occ_adults": 3,
        "occ_children": 0,
        "occ_infants": 0,
        "default_occupancy": 2,
        "count_of_rooms": 20,
        "room_kind": "room",
        "capacity": null,
        "content": {
          "description": "Some Room Type Description Text",
          "photos": [
            {
              "author": "Author Name",
              "description": "Room View",
              "id": "198a19d4-42c0-48d8-a55c-c7836b2c1f7e",
              "kind": "photo",
              "position": 0,
              "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
              "room_type_id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
              "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
            }
          ]
        }
      },
      "relationships": {
        "facilities": {
          "data": []
        },
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

### Filter

{% hint style="info" %}
You can use a filter to retrieve Room Types for a specific property:&#x20;

`GET https://{{STAGING_DOMAIN}}/api/v1/room_types?filter[property_id]=PROPERTY_ID`
{% endhint %}

###

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Room Type objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.&#x20;

## Room Type Options

Method to get a list of all room types associated with current account without additional details and pagination limits.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/room_types/options
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "default_occupancy": 2,
        "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
        "property_id": "2941337f-50d6-4189-ae13-f76efaf9c515",
        "title": "Standard Room"
      },
      "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
      "type": "room_type"
    }
  ]
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

## Get Room Type by ID

Retrieve specific Room Types by ID.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/room_types/:id
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "type": "room_type",
    "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
    "attributes": {
      "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
      "title": "Standard Room",
      "occ_adults": 3,
      "occ_children": 0,
      "occ_infants": 0,
      "default_occupancy": 2,
      "count_of_rooms": 20,
      "room_kind": "room",
      "capacity": null,
      "content": {
        "description": "Some Room Type Description Text",
        "photos": [
          {
            "author": "Author Name",
            "description": "Room View",
            "id": "198a19d4-42c0-48d8-a55c-c7836b2c1f7e",
            "kind": "photo",
            "position": 0,
            "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
            "room_type_id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
            "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
          }
        ]
      }
    },
    "relationships": {
      "facilities": {
        "data": []
      },
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

**Not Found Error**

Status Code: `404 Not Found`

```javascript
{
  "errors": {
    "code": "not_found",
    "title": "Resouce Not Found"
  }
}
```
{% endtab %}
{% endtabs %}

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Room Type object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Room Type.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Room Type with provided ID is not present at system.

## Create Room Type

Create a new Room Type.

{% hint style="info" %}
Availability of all rooms created will be 0, to set availability you will need to use the [Availability and Rates API](ari.md#update-availability)
{% endhint %}

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/room_types
```

Query body (JSON):

```javascript
{
  "room_type": {
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "title": "Standard Room",
    "count_of_rooms": 20,
    "occ_adults": 3,
    "occ_children": 0,
    "occ_infants": 0,
    "default_occupancy": 2,
    "facilities": [],
    "room_kind": "room",
    "capacity": null,
    "content": {
      "description": "Some Room Type Description Text",
      "photos": [
        {
          "author": "Author Name",
          "description": "Room View",
          "kind": "photo",
          "position": 0,
          "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
        }
      ]
    }
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
    "type": "room_type",
    "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
    "attributes": {
      "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
      "title": "Standard Room",
      "occ_adults": 3,
      "occ_children": 0,
      "occ_infants": 0,
      "default_occupancy": 2,
      "count_of_rooms": 20,
      "room_kind": "room",
      "capacity": null,
      "content": {
        "description": "Some Room Type Description Text",
        "photos": [
          {
            "author": "Author Name",
            "description": "Room View",
            "id": "198a19d4-42c0-48d8-a55c-c7836b2c1f7e",
            "kind": "photo",
            "position": 0,
            "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
            "room_type_id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
            "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
          }
        ]
      }
    },
    "relationships": {
      "facilities": {
        "data": []
      },
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

**Validation Error Response**

Status Code: `422 Unprocessable Entity`

```javascript
{
  "errors": {
    "code": "validation_error",
    "title": "Validation Error",
    "details": {
      "title": [
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

String with a valid UUID of the Property to associate with the created Room Type.

**title `[required]`**

Any non-empty string with maximum length of 255 symbols.\
Note: The Room Type will be represented in the system under that title.

**count\_of\_rooms `[required]`**

Any positive integer number.\
Note: This has no effect and is for information only

**occ\_adults `[required]`**

Any positive integer number.\
Note: How many Adult bed spaces have in this Room Type.

**occ\_children `[required]`**

Any positive integer number.\
Note: How many Child only bed spaces in this Room Type. Children can sleep in adult beds also. If no Child only beds then set this to 0.

**occ\_infants `[required]`**

Any positive integer number.\
Note: How many Infants cots available in this Room Type.&#x20;

**default\_occupancy `[required]`**

Any positive integer number lower or equal to `occ_adults`.\
Note: How many guests can stay in the room by default (without extra spaces). Keep in mind, this field can not be greater than `occ_adults` value. Typically this value is set equal to amount of adults.\


**facilities `[optional]`**

List of [Room Type Facility](./api-v.1-documentation/facilities-collection#room-type-facilities-list) IDs associated with Property.

**room\_kind `[optional]`**

String. Type of Room. Enumerable. Possible values: `room`, `dorm`.

**capacity `[optional]`**

Integer. Count of beds at one physical room. Applicable only for Room Type with kind equal to `dorm`.

**content `[optional]`**

Object with content information for property. Content object can contain:\
`description` - optional text field with Property description. By default Description will be equal to `null`.\
`photos` - optional list of photos associated with Property. Each photo is object with next fields:\
`url` - photo URL\
`position` - integer value to represent photo position at list, Photo with position equal to 0 is used as Cover Photo for Room Type\
`description` - Photo text description\
`author` - Name of photo Author\
`kind` - one of three possible values: photo, ad (advertising), menu (restaurant menu photo).\
More information about Photo API is [here](photos-collection.md).

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Room Type object in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Update Room Type

Update a Room Type.

{% tabs %}
{% tab title="Request" %}
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/room_types/:id
```

Query body (JSON):

```javascript
{
  "room_type": {
    "title": "Standard Room",
    "count_of_rooms": 20,
    "occ_adults": 3,
    "occ_children": 0,
    "occ_infants": 0,
    "default_occupancy": 2,
    "facilities": [],
    "content": {
      "description": "Some Room Type Description Text",
      "photos": [
        {
          "author": "Author Name",
          "description": "Room View",
          "kind": "photo",
          "position": 0,
          "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
        }
      ]
    }
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
    "type": "room_type",
    "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
    "attributes": {
      "id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
      "title": "Standard Room",
      "occ_adults": 3,
      "occ_children": 0,
      "occ_infants": 0,
      "default_occupancy": 2,
      "count_of_rooms": 20,
      "room_kind": "room",
      "capacity": null,
      "content": {
        "description": "Some Room Type Description Text",
        "photos": [
          {
            "author": "Author Name",
            "description": "Room View",
            "id": "198a19d4-42c0-48d8-a55c-c7836b2c1f7e",
            "kind": "photo",
            "position": 0,
            "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
            "room_type_id": "994d1375-dbbd-4072-8724-b2ab32ce781b",
            "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
          }
        ]
      }
    },
    "relationships": {
      "facilities": {
        "data": []
      },
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
      "title": [
        "can't be blank"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Fields

This method use same fields as Create Room Type method.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Room Type object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Room Type with provided ID is not present at system.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Remove Room Type

Remove a Room Type.

{% tabs %}
{% tab title="Request" %}
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/room_types/:id
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

### Flags

Because system is not allow to remove RoomType associated with any channel, we expose additional feature flag - force. To remove RoomType and unmap it from Channel you can use next request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/room_types/:id?force=true
```

Please, be careful with that method, once Room Type was removed we can't restore it and any channel information.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Meta object with message in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Room Type with provided ID is not present at system.
