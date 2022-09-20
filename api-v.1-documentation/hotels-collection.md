---
description: API methods to work with Properties
---

# Properties Collection

**Property** is a physical premises – hotels, motels, lodges, cabins, chalets, luxury apartments and other types of buildings. Usually each property has a unique address.

{% hint style="info" %}
Don't combine multiple properties into one, it's better they are all created separately with their own address and details
{% endhint %}

## Properties List

Retrieve list of properties associated with user.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/properties
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "type": "property",
      "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "attributes": {
        "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
        "title": "Demo Hotel",
        "is_active": true,
        "email": "hotel@test.com",
        "phone": "01267237037",
        "currency": "GBP",
        "country": "GB",
        "state": "Demo State",
        "city": "Demo Town",
        "address": "Demo Street",
        "zip_code": "SA23 2JH",
        "latitude": null,
        "longitude": null,
        "timezone": "Europe/London",
        "property_type": "hotel",
        "content": {
          "description": "Some Property Description Text",
          "photos": [{
            "author": "Author Name",
            "description": "Room View",
            "id": "4355439c-df23-4f12-bffd-26476e31dd4a",
            "kind": "photo",
            "position": 0,
            "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
            "room_type_id": null,
            "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
          }],
          "important_information": null
        },
        "logo_url": null,
        "acc_channels_count": 0
      },
      "relationships": {
        "groups": {
          "data": [
            {
              "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
              "type": "group",
              "attributes": {
                "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
                "title": "User Group"
              }
            }
          ]
        },
        "facilities": {
            "data": []
        }
      }
    }
  ],
  "meta": {
    "limit": 10,
    "page": 1,
    "total": 1
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

### Pagination and Filters

By default, this method return first 10 element. To get more details, you should use Pagination arguments.\
Information about count of entities and current pagination position contained at `meta` section at response object.

This endpoint accept filters for fields: `id`, `title`, `is_active`.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Property objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.&#x20;

## Property Options

Method to get list of all properties associated with current account without additional details and pagination limits.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/properties/options
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
        "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
        "title": "Demo Hotel",
        "currency": "GBP"
      },
      "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "type": "properties"
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

## Create Property

Create a new Property.

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/properties
```

Query body (JSON):

```javascript
{
  "property": {
    "title": "Demo Hotel",
    "currency": "GBP",
    "email": "hotel@test.com",
    "phone": "01267237037",
    "zip_code": "SA23 2JH",
    "country": "GB",
    "state": "Demo State",
    "city": "Demo Town",
    "address": "Demo Street",
    "longitude": "-0.2416781",
    "latitude": "51.5285582",
    "timezone": "Europe/London",
    "facilities": [],
    "property_type": "hotel",
    "group_id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
    "settings": {
      "allow_availability_autoupdate_on_confirmation": true,
      "allow_availability_autoupdate_on_modification": true,
      "allow_availability_autoupdate_on_cancellation": true,
      "min_stay_type": "both",
      "min_price": null,
      "max_price": null,
      "state_length": 500
    }
    "content": {
      "description": "Some Property Description Text",
      "photos": [{
        "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/",
        "position": 0,
        "author": "Author Name",
        "kind": "photo",
        "description": "Room View"
      }],
      "important_information": "Some important notes about property"
    },
    "logo_url": "https://hotel.domain/logo.png"
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
    "type": "property",
    "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "attributes": {
      "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "title": "Demo Hotel",
      "is_active": true,
      "email": "hotel@test.com",
      "phone": "01267237037",
      "currency": "GBP",
      "country": "GB",
      "state": "Demo State",
      "city": "Demo Town",
      "address": "Demo Street",
      "zip_code": "SA23 2JH",
      "longitude": "-0.2416781",
      "latitude": "51.5285582",
      "timezone": "Europe/London",
      "property_type": "hotel",
      "content": {
        "description": "Some Property Description Text",
        "photos": [{
          "author": "Author Name",
          "description": "Room View",
          "id": "4355439c-df23-4f12-bffd-26476e31dd4a",
          "kind": "photo",
          "position": 0,
          "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
          "room_type_id": null,
          "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
        }],
        "important_information": null
      },
      "logo_url": null,
      "acc_channels_count": 0,
      "settings": {
        "allow_availability_autoupdate_on_confirmation": true,
        "allow_availability_autoupdate_on_modification": true,
        "allow_availability_autoupdate_on_cancellation": true,
        "min_stay_type": "both",
        "max_price": null,
        "min_price": null,
        "state_length": 500
      }
    },
    "relationships": {
      "groups": {
        "data": [
          {
            "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
            "type": "group",
            "attributes": {
              "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
              "title": "User Group"
            }
          }
        ]
      },
      "facilities": {
          "data": []
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

**title `[required]`**

Any non-empty string with maximum length of 255 symbols.\
Note: The property will be represented in the system under that title.

**currency `[required]`**

3 symbols long string with Currency Alphabetic code based at [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html).\
Note: This currency will be used as default currency for nested Property entities and provided as default property currency to 3rd party services.

**email `[optional]`**

String with a valid email address.\
Note: This email address will be provided to 3rd party services as contact email address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**phone `[optional]`**

String with maximum length of 32 symbols. Can contain digits, spaces, brackets and special characters.\
Note: This phone will be provided to 3rd party services as contact email address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**zip\_code `[optional]`**

String with maximum length of 32 symbols.\
Note: This zip\_code will be provided to 3rd party services as part of contact address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**country `[optional]`**

2 symbols long string with Country Alpha-2 code based at [ISO-3166-1](https://www.iso.org/iso-3166-country-codes.html).\
Note: This country will be provided to 3rd party services as part of contact address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**state `[optional]`**

String with maximum length of 255 symbols.\
Note: This state will be provided to 3rd party services as part of contact address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**city `[optional]`**

String with maximum length of 255 symbols.\
Note: This city will be provided to 3rd party services as part of contact address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**address `[optional]`**

String with maximum length of 255 symbols.\
Note: This address will be provided to 3rd party services as part of contact address for that property.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**longitude `[optional]`**

Decimal number represented as String with maximum length of 10 symbols. Can have maximum 7 decimal chars.\
Minimum value: -180.\
Maximum value: +180.\
Note: This field is part of property coordinates.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**latitude `[optional]`**

Decimal number represented as String with maximum length of 9 symbols. Can have maximum 7 decimal chars.\
Minimum value: -90.\
Maximum value: +90.\
Note: This field is part of property coordinates.\
Field is optional at initial setup step, but required when you try connect first 3rd party service.

**timezone `[optional]`**

Timezone name from ISO 8601. All possible values you can find at Time zone database ([https://www.iana.org/time-zones](https://www.iana.org/time-zones)). More info about Time Zone database is here - [https://en.wikipedia.org/wiki/Tz\_database](https://en.wikipedia.org/wiki/Tz\_database).

**facilities `[optional]`**

List of [Property Facility](./api-v.1-documentation/facilities-collection#property-facilities-list) IDs associated with Property.

**property\_type `[optional]`**

One of possible values:

* apart\_hotel
* apartment
* boat
* camping
* capsule\_hotel
* chalet
* country\_house
* farm\_stay
* guest\_house
* holiday\_home
* holiday\_park
* homestay
* hostel
* hotel
* inn
* lodge
* motel
* resort
* riad
* ryokan
* tent
* villa

**group\_id `[optional]`**

String with valid UUID for Group object what you would like to use as Base Group for created Property.\
Field is optional, if it is not provided, system automatically assign Default User Group as Base Group for Property.

**settings** `[optional]`

Object with Property settings. Should contain next fields:

`allow_availability_autoupdate` - option to allow increase and decrease Availability when bookings is came into {{APPLICATION_NAME}}. **\[deprecated]**

`allow_availability_autoupdate_on_confirmation` - option to allow decrease Availability when new bookings is came into {{APPLICATION_NAME}}.

`allow_availability_autoupdate_on_modification` - option to allow increase and decrease Availability when booking modification is came into {{APPLICATION_NAME}}.

`allow_availability_autoupdate_on_cancellation` - option to allow decrease Availability when booking cancellation is came into {{APPLICATION_NAME}}.

`min_stay_type` - option to control simplified Min Stay restrictions. Can be useful for situation when your system support only one of Min Stay Types (Arrival or Through).\
If your system work only with Min Stay Arrival or only with Min Stay Through you can setup that setting into `arrival` or `through` mode, as result we will simplify ARI updates and allow provide min stay changes under `min_stay` key and automatically setup correct selection for Min Stay type at Channel mappings.

`min_price` - setup minimum price per property. When user try to setup price less than min\_price, system increase it up to minimum.

`max_price` - setup maximum price per property. When user try to setup price greater than max\_price, system decrease it up to maximum.

`state_length` - setup length of inventory table for Property. Min value is 100 days, max value is 730 days.

**content `[optional]`**

Object with content information for property. Content object can contain:\
`description` - optional text field with Property description. By default Description will be equal to `null`.\
`important_information` - optional text field with some important information about Property. Will be included into Booking confirmation emails.\
`photos` - optional list of photos associated with Property. Each photo is object with next fields:\
`url` - photo URL\
`position` - integer value to represent photo position at list, Photo with position equal to 0 is used as Cover Photo for property\
`description` - Photo text description\
`author` - Name of photo Author\
`kind` - one of three possible values: photo, ad (advertising), menu (restaurant menu photo).\
More information about Photo API is [here](photos-collection.md).

**logo\_url `[optional]`**

String. Valid URL to property logo. Logo will be copied into our media storage.

### Read only fields

`acc_cannels_count`

Integer. Count of connected channels. Aggregate.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Property object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Get Property by ID

Retrieve specific property associated with User by ID.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/properties/:id
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "type": "property",
    "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "attributes": {
      "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "title": "Demo Hotel",
      "is_active": true,
      "email": "hotel@test.com",
      "phone": "01267237037",
      "currency": "GBP",
      "country": "GB",
      "state": "Demo State",
      "city": "Demo Town",
      "address": "Demo Street",
      "zip_code": "SA23 2JH",
      "latitude": null,
      "longitude": null,
      "timezone": "Europe/London",
      "property_type": "hotel",
      "content": {
        "description": "Some Property Description Text",
        "photos": [{
          "author": "Author Name",
          "description": "Room View",
          "id": "4355439c-df23-4f12-bffd-26476e31dd4a",
          "kind": "photo",
          "position": 0,
          "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
          "room_type_id": null,
          "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
        }],
        "important_information": null
      },
      "settings": {
        "allow_availability_autoupdate_on_confirmation": true,
        "allow_availability_autoupdate_on_modification": true,
        "allow_availability_autoupdate_on_cancellation": true,
        "min_stay_type": "both",
        "max_price": null,
        "min_price": null
      },
      "logo_url": null,
      "acc_channels_count": 0
    },
    "relationships": {
      "groups": {
        "data": [
          {
            "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
            "type": "group",
            "attributes": {
              "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
              "title": "User Group"
            }
          }
        ]
      },
      "facilities": {
          "data": []
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
{% endtab %}
{% endtabs %}

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Property object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Property.

## Update Property

Update property information.

{% tabs %}
{% tab title="Request" %}
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/properties/:id
```

Query body (JSON):

```javascript
{
  "property": {
    "title": "Demo Hotel",
    "currency": "GBP",
    "email": "hotel@test.com",
    "phone": "01267237037",
    "zip_code": "SA23 2JH",
    "country": "GB",
    "state": "Demo State",
    "city": "Demo Town",
    "address": "Demo Street"
    "longitude": "-0.2416781",
    "latitude": "51.5285582",
    "timezone": "Europe/London",
    "facilities": [],
    "property_type": "hotel",
    "content": {
      "description": "Some Property Description Text",
      "photos": [{
        "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/",
        "position": 0,
        "author": "Author Name",
        "kind": "photo",
        "description": "Room View"
      }]
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
    "type": "property",
    "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "attributes": {
      "id": "716305c4-561a-4561-a187-7f5b8aeb5920",
      "title": "Demo Hotel",
      "is_active": true,
      "email": "hotel@test.com",
      "phone": "01267237037",
      "currency": "GBP",
      "country": "GB",
      "state": "Demo State",
      "city": "Demo Town",
      "address": "Demo Street",
      "zip_code": "SA23 2JH",
      "latitude": null,
      "longitude": null,
      "timezone": "Europe/London",
      "property_type": "hotel",
      "content": {
        "description": "Some Property Description Text",
        "photos": [{
          "author": "Author Name",
          "description": "Room View",
          "id": "4355439c-df23-4f12-bffd-26476e31dd4a",
          "kind": "photo",
          "position": 0,
          "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
          "room_type_id": null,
          "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
        }],
        "important_information": null
      },
      "settings": {
        "allow_availability_autoupdate_on_confirmation": true,
        "allow_availability_autoupdate_on_modification": true,
        "allow_availability_autoupdate_on_cancellation": true,
        "min_stay_type": "both",
        "max_price": null,
        "min_price": null,
        "state_length": 500
      },
      "logo_url": null,
      "acc_channels_count": 0
    },
    "relationships": {
      "groups": {
        "data": [
          {
            "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
            "type": "group",
            "attributes": {
              "id": "f5338935-7fe0-40eb-9d7e-4dbf7ecc52c7",
              "title": "User Group"
            }
          }
        ]
      },
      "facilities": {
          "data": []
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

This method uses the same fields as [Create Property](./api-v.1-documentation/hotels-collection#create-property) method.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Property object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Property.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.
