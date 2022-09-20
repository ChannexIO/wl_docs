---
description: 'API Methods to get real time rates, availability and content from {{APPLICATION_NAME}}.'
---

# {{APPLICATION_NAME}} Shopping API

These are API methods for META-like channel connections. If your application does not cache any information on your side about Properties, Availability and Restrictions, you can implement support for our Shopping API and use it to build your own Booking Engine or Meta Channel.

{% hint style="info" %}
This API is for real time shopping of {{APPLICATION_NAME}} API for all required details. Perfect for many applications as you don't need to cache anything on your side.
{% endhint %}

{% hint style="info" %}
**Coming Soon**

Get available properties method

Provider API Key to protect channel
{% endhint %}

{% hint style="danger" %}
**Experimental API**  
These API methods are experimental and can be changed based at client feedback.
{% endhint %}

## Properties List

Method to get a list of connected Properties.

```text
GET https://{{STAGING_DOMAIN}}/api/v1/meta/{{CHANNEL_NAME}}/property_list

{
  "data": [
    {
      "attributes": {
        "address": "220125, RU, Moscow, Moscow, Zubovskaya 5",
        "city": "Moscow",
        "country": "RU",
        "description": "Excellent Hotel and Old City Centre",
        "id": "c1595b90-75f3-4b7e-b3c4-cf6a70f0d81d",
        "lat": "11.001323",
        "lng": "33.023321",
        "photos": [],
        "state": "Moscow",
        "title": "Old City Centre",
        "zip_code": "220125"
      },
      "id": "c1595b90-75f3-4b7e-b3c4-cf6a70f0d81d",
      "type": "property"
    },
    {
      "attributes": {
        "address": "SW1W 0AD, GB, London, 10 Chester Square",
        "city": "London",
        "country": "GB",
        "description": "Quiet hotel at heart of British capital",
        "id": "37d1c98a-bb20-4ca4-a8ce-bc1ee78d9455",
        "lat": "51.4966440",
        "lng": "-0.1476140",
        "photos": [
          {
            "url": "https://img.{{APPLICATION_DOMAIN}}/ec4f261a-1e21-4070-922b-86e0c46abd26/",
            "description": null,
            "author": null
          }
        ],
        "state": null,
        "title": "Royal Hotel tbf",
        "zip_code": "SW1W 0AD"
      },
      "id": "37d1c98a-bb20-4ca4-a8ce-bc1ee78d9455",
      "type": "property"
    }
  ],
  "meta": {}
}
```

This endpoint supports filters by `city`, `lat`, `lng` and `country`.  
To apply filter pass it as query argument:  
`/property_list?filter[city]=Moscow`

{% hint style="info" %}
You can use our [Photo Transformations](./api-v.1-documentation/photos-collection#photo-transformations) to get photos fit to your design.
{% endhint %}

## Get Property Info

Method to get Property Info by ID

```text
GET https://{{STAGING_DOMAIN}}/api/v1/meta/{{CHANNEL_NAME}}/{{PROPERTY_ID}}/property_info

{
  "data": {
    "attributes": {
      "address": "SW1W 0AD, GB, London, 10 Chester Square",
      "city": "London",
      "country": "GB",
      "description": "Quiet hotel at heart of British capital",
      "id": "37d1c98a-bb20-4ca4-a8ce-bc1ee78d9455",
      "location": {
        "lat": "51.4966440",
        "lng": "-0.1476140",
      },
      "facilities": [
        "Swimming Pool", "GYM"
      ],
      "photos": [
        {
          "url": "https://img.{{APPLICATION_DOMAIN}}/ec4f261a-1e21-4070-922b-86e0c46abd26/",
          "description": null,
          "author": null
        }
      ],
      "state": null,
      "title": "Royal Hotel tbf",
      "zip_code": "SW1W 0AD"
    },
    "id": "37d1c98a-bb20-4ca4-a8ce-bc1ee78d9455",
    "type": "property_info"
  }
}
```

## Get Closed Dates

Method to get unavailable dates, dates closed to arrival and departure.

```text
GET https://{{STAGING_DOMAIN}}/api/v1/meta/{{CHANNEL_NAME}}/{{PROPERTY_ID}}/closed_dates

{
  "data": {
    "attributes": {
      "closed": [
        "2020-09-17"
      ],
      "closed_to_arrival": [
        "2020-09-19"
      ],
      "closed_to_departure": [
        "2020-09-15"
      ]
    },
    "type": "closed_dates_list"
  }
}
```

## Get Rooms List

Method to get Rooms and Rates list

```text
GET https://{{STAGING_DOMAIN}}/api/v1/meta/{{CHANNEL_NAME}}/{{PROPERTY_ID}}/rooms?checkin_date=YYYY-MM-DD&checkout_date=YYYY-MM-DD

{
  "data": [
    {
      "type": "room_with_rates",
      "id": "ROOM_ID",
      "attributes": {
        "id": "ROOM_ID",
        "title": "Room Title",
        "description": "Description",
        "bed_options": [
          {
            "title": "Olympic Queen",
            "count": 2,
            "size": "90x200 CM"
          }
        ],
        "facilities": ["facility 1", "facility 2"],
        "photos": [
          {
            "url": "PHOTO_URL",
            "title": "title",
            "author": "author"
          }
        ],
        "rate_plans": [
          {
            "id": "RATE_PLAN_ID",
            "title": "Title",
            "occupancy": {
              "adults": 1,
              "children": 0,
              "infants": 0
            },
            "cancellation_policy": "Cancellation policy",
            "meal_plan": "Bed & Breakfast",
            "price": "100.00",
            "taxes": [
              {
                "title": "Tax Title",
                "amount": "10.00",
                "inclusive": false,
                "rate": "10.00",
                "mode": "percent"
              }
            ]
          }
        ]
      }
    }
  ]
}
```

This method will return list of Rooms and Rate Plans. This method supports filter arguments:

* checkin\_date \(Date at ISO format YYYY-MM-DD\)
* checkout\_date \(Date at ISO format YYYY-MM-DD\)
* length\_of\_stay \(Integer\)

Checkout\_date is optional if you use length\_of\_stay and vice versa.

If the method is called without dates, it will return Rooms list without Rate Plans.

## Push Booking

Method to create Bookings

```text
POST https://secure-{{STAGING_DOMAIN}}/api/v1/meta/{{CHANNEL_CODE}}/{{PROPERTY_ID}}/push_booking

{
  "booking": {
    "status": "new",

    "reservation_id": "{{UNIQUE_ID_FROM_OTA}}",

    "arrival_date": "2019-05-09",
    "departure_date": "2019-05-10",
    "arrival_hour": "10:00",

    "currency": "GBP",
    
    "payment_collect": "property",
    "payment_type": "credit_card",

    "customer": {
      "name": "User",
      "surname": "Surname",
      "country": "EN",
      "city": "London",
      "address": "101 Finsbury Pavement",
      "zip": "ec2a 1rs",
      "mail": "support@test.com",
      "phone": "+44 444 4444 44 44",
      "language": "EB",
      "company": {
        "title": "Company Name",
        "number": "TAX NUMBER",
        "number_type": "VAT"
      },
      "meta": {}
    },

    "guarantee": {
      "expiration_date": "10/2020",
      "cvv": "123",
      "cardholder_name": "User",
      "card_type": "visa",
      "card_number": "4111111111111111",
      "meta": {
        "virtual_card_currency_code": "GBP",
        "virtual_card_current_balance": 10000,
        "virtual_card_decimal_places": 2,
        "virtual_card_effective_date": "2020-02-02",
        "virtual_card_expiration_date": "2021-02-02"
      }
    },

    "rooms": [
      {
        "index": 0,
        "room_type_code": "{{ROOM_TYPE_ID}}",
        "rate_plan_code": "{{RATE_PLAN_ID}}",
        "occupancy": {
          "adults": 1,
          "children": 0,
          "infants": 0
        }
      }
    ],

    "services": [
      {
        "type": "Fee",
        "total_price": "100.00",
        "price_per_unit": "100.00",
        "price_mode": "Per stay",
        "persons": 0,
        "nights": 0,
        "name": "Cancellation Fee",
        "room_index": null
      }
    ]
  }
}
```

Please, keep in mind, if you pass Credit Card Information, you MUST use the secure endpoint: `secure-{{STAGING_DOMAIN}}`

### Field description

**status `[required]`**  
String. Status of Booking, can be one of three values: `new`, `modified`, `cancelled`.

**reservation\_id `[optional]`**  
String. Booking unique ID. For messages with status `new` can be empty, in that case {{APPLICATION_NAME}} will generate unique UUID for booking.

**arrival\_date `[required]`**  
String. Arrival Date represented as string with date in ISO 8601 format by mask `YYYY-MM-DD`.

**departure\_date `[required]`**  
String. Departure Date represented as string with date in ISO 8601 format by mask `YYYY-MM-DD`.

**arrival\_hour `[optional]`**  
String. Arrival Time represented as string with time in `HH:MM` format at 24h.

**currency `[required]`**  
String. Booking currency code. 3 symbols long string with Currency Alphabetic code based at [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html).

**payment\_collect `[optional]`**  
String. Information about payment collect point. If payment collected via OTA, it should be `ota`, in other case it should be `property`. Default value is `property`.

**payment\_type `[optional]`**  
String. Information about how payment should be collected. Support `bank_transfer` or `credit_card`. Can be `null` if not specified. `bank_transfer` value suitable for OTA collect case.

### **Customer Fields**

Information about the Customer \(Who made the booking\)

**name `[optional]`**  
String with maximum length of 255 symbols. Name of Customer.

**surname `[required]`**  
String with maximum length of 255 symbols. Surname of Customer.

**country `[optional]`**  
String. 2 symbols long string with Country Alpha-2 code based at [ISO-3166-1](https://www.iso.org/iso-3166-country-codes.html).

**city `[optional]`**  
String with maximum length of 255 symbols. Customer City name.

**address `[optional]`**  
String with maximum length of 255 symbols. Customer Address.

**zip `[optional]`**  
String with maximum length of 32 symbols. Customer ZIP Code.

**mail `[optional]`**  
String with a valid email address. Customer Email address.

**phone `[optional]`**  
String with maximum length of 32 symbols. Can contain digits, spaces, brackets and special characters. Customer Phone number.

Please if possible pass in friendly format with country code like this example: +447749617211 

This can be simple for the property to contact the guest.

**language `[optional]`**  
String. 2 symbols long string with language locale code.

**company `[optional]`**  
Object with information about Customer Company \(if customer is Business\). Can contain next fields:

* **title `[optional]`** String with maximum length of 255 symbols.
* **number `[optional]`** String with maximum length of 255 symbols. Tax Number.
* **number\_type `[optional]`** String with maximum length of 255 symbols. Tax Name \(eg: VAT\)

**meta `[optional]`**  
Object without any specific structure where you can pass any additional information about Customer.

### Guarantee Fields

Information about the Credit Card.

{% hint style="warning" %}
If you'd like to pass information about the credit card you must use `secure-{{STAGING_DOMAIN}}` or `secure.{{APPLICATION_DOMAIN}}` \(for production environment\) endpoints. Otherwise the credit card will be masked without ability to restore the original card.
{% endhint %}

**expiration\_date `[required]`**  
String with Card Expiration date in `MM/YYYY` format.

**cvv `[required]`**  
String with 3 or 4 numbers. Service code for payment systems.

**cardholder\_name `[required]`**  
String. Cardholder name from Card front.

**card\_type `[required]`**  
String. Card type name.

**card\_number `[required]`**  
String. Card number.

**meta `[optional]`**  
Object. Can contain any additional information for booking recipient. Also, if you work with Virtual Credit Cards, you can use next fields:

* **virtual\_card\_currency\_code** Currency of virtual card
* **virtual\_card\_current\_balance** Current balance of virtual card as Integer value
* **virtual\_card\_decimal\_places** Info about decimal places at provided current\_balance field
* **virtual\_card\_effective\_date** Date when card will be available to charge
* **virtual\_card\_expiration\_date** Date when card is expired

### Booking Rooms

Booking rooms should be passed as Array of Objects. Each room object should contain information about `room_type_code`, `occupancy`, and days breakdown.

**index `[optional]`**  
Integer. Room Index to associate Services at Room level. Incremented value, start from 0.

**room\_type\_code `[required]`**  
String. Code of Room Type received at Get Rooms List operation.

**rate\_plan\_code `[required]`**  
String. Rate Plan Code received at Get Rooms List operation.

**occupancy `[required]`**  
Object with information about occupancy. Should contain next fields:

* **adults `[required]`** Integer. Count of adults \(persons older then 16 years old\)
* **children `[required]`** Integer. Count of children \(persons between 2 and 16 years old\)
* **infants `[required]`** Integer. Count of infants \(persons younger then 2 years old\)

### Booking Services \(Extras\)

Services is Array of Objects to represent additional service or fees sold with bookings. Also, this field can contain Cancellation Fee when booking is cancelled with payment.  
****Each object should contain next fields:

**type `[required]`**  
String. Type of Service. One of possible values: `Meal, Fee, Extra`

**total\_price `[required]`**  
String. Total Service price.

**price\_per\_unit `[required]`**  
String. Price per one unit of Service.

**price\_mode `[required]`**  
String. Service calculation price logic. One of possible values: `Per stay, Per night, Per person, Per person per night`

**persons `[required]`**  
Integer. Count of persons associated with Service.

**nights `[required]`**  
Integer. Count of nights associated with Service.

**name `[required]`**  
String. Name of service.

**room\_index `[optional]`**  
Integer. Index of room which is associated with Service. Keep in mind, Room should have `index`.

