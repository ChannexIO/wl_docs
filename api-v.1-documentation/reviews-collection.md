---
description: API to work with Reviews
---

# Reviews Collection

At {{APPLICATION_NAME}} you are able to work with OTA Reviews. It is unified API to work with reviews from Airbnb and Booking.com.

This API has 2 parts - Reviews and Scores.

## Enable reviews on the Property

Go here to applications: [https://{{domain}}/applications](https://{{domain}}/applications)

Add the `Messages & Reviews` app to the property, then the API and UI for chat will be available.

## Data Structures

### Review

```json
{
  "content": "Guest review content",
  "guest_name": "Guest Name",
  "id": "a82d22ce-20c8-45c3-b49b-8e080a13560a",
  "inserted_at": "2022-06-06T04:27:42.553115",
  "is_hidden": false,
  "is_replied": false,
  "ota": "AirBNB",
  "ota_reservation_id": "HMSZMHHF2X",
  "overall_score": 8.0,
  "received_at": "2022-06-06T04:27:39.366000",
  "reply": null,
  "scores": [
    {
      "category": "accuracy",
      "score": 8.0
    },
    {
      "category": "checkin",
      "score": 8.0
    },
    {
      "category": "communication",
      "score": 8.0
    },
    {
      "category": "cleanliness",
      "score": 6.0
    },
    {
      "category": "value",
      "score": 10.0
    },
    {
      "category": "location",
      "score": 10.0
    }
  ],
  "updated_at": "2022-06-06T04:27:42.553115"
}
```

`content` Text field with guest Review text.

`guest_name` String with guest Name. Can be empty.

`is_hidden` Boolean status marker specific for Airbnb reviews. If `true`, that mean review is created but will be visible when Property Owner provide they feedback to Guest.

`is_replied` Boolean status marker which represent has Review reply from Property Owner or not.

`ota` String. Name of OTA associated with Review.

`ota_reservation_id` String which contain code of Reservation associated with Review.

`overall_score` Float value which represent Overall score of Review. Max is 10.

`reply` String with reply Property Owner to Guest Review

`scores` List with detailed information about scores provided by Guest.

### Score

```json
{
  "id": "9e9ac4cd-5856-4469-9bce-dbce8f5a435f",
  "count": 1121,
  "overall_score": 9.15,
  "scores": {
    "accuracy": {
      "count": 18,
      "score": 9.78
    },
    "checkin": {
      "count": 18,
      "score": 9.88
    },
    "clean": {
      "count": 1113,
      "score": 9.55
    },
    "comfort": {
      "count": 1098,
      "score": 9.45
    },
    "communication": {
      "count": 18,
      "score": 9.88
    },
    "facilities": {
      "count": 1099,
      "score": 9.15
    },
    "location": {
      "count": 1114,
      "score": 9.41
    },
    "staff": {
      "count": 1097,
      "score": 9.67
    },
    "value": {
      "count": 1118,
      "score": 9.21
    }
  },
  "inserted_at": "2022-06-01T09:43:20.106161",
  "updated_at": "2022-06-05T23:01:07.935270"
}
```

`count` Integer value to represent count of scores for Property.

`overall_score` Float value to represent overall score. Max is 10.

`scores` Map with score categories where each key is associated with object with `count` and `score` values. Key represent score category.

### OTA Score

```json
{
  "id": "383a4180-1df5-40f3-a086-de41509da8d5",
  "channel_id": "305757c7-15c2-4517-8414-7ee6fb69cfc4",
  "count": 1104,
  "ota": "BookingCom",
  "overall_score": 9.14,
  "scores": {
    "clean": {
      "count": 1096,
      "score": 9.55
    },
    "comfort": {
      "count": 1099,
      "score": 9.45
    },
    "facilities": {
      "count": 1100,
      "score": 9.15
    },
    "location": {
      "count": 1097,
      "score": 9.4
    },
    "staff": {
      "count": 1098,
      "score": 9.67
    },
    "value": {
      "count": 1101,
      "score": 9.21
    }
  }
}
```

`channel_id` UUID Reference to Channel entity associated with OTA Score.

`count` Integer value to represent count of Reviews from guests.

`ota` String to represent type of OTA.

`overall_score` Float to represent overall score. Max is 10.

`scores` Map with score categories where each key is associated with object with `count` and `score` values. Key represent score category.

## Score categories mappings

### Review Categories <a href="#review-categories" id="review-categories"></a>

| **Booking.com** | **Airbnb**    | **{{APPLICATION_NAME}}**   |
| --------------- | ------------- | ------------- |
| clean           | cleanliness   | clean         |
| facilities      |               | facilities    |
| location        | location      | location      |
| services        |               | services      |
| staff           |               | staff         |
| value           | value         | value         |
|                 | accuracy      | accuracy      |
|                 | communication | communication |
|                 | checkin       | checkin       |
| comfort         |               | comfort       |

## Reviews

Simple API to read and reply to reviews.

### Get Reviews List

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/reviews
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "content": "Guest review test",
        "guest_name": "Guest Name",
        "id": "5d9aa0d9-a888-46b5-bde8-13cc7a15161c",
        "inserted_at": "2022-06-06T05:00:41.020781",
        "is_hidden": false,
        "is_replied": false,
        "ota": "BookingCom",
        "ota_reservation_id": "2328423042",
        "overall_score": 10.0,
        "received_at": "2022-06-06T05:45:23.000000",
        "reply": null,
        "scores": [
          {
            "category": "clean",
            "score": 10
          },
          {
            "category": "comfort",
            "score": 7.5
          },
          {
            "category": "facilities",
            "score": 10
          },
          {
            "category": "location",
            "score": 10
          },
          {
            "category": "staff",
            "score": 10
          },
          {
            "category": "value",
            "score": 7.5
          }
        ],
        "updated_at": "2022-06-06T05:00:41.020781"
      },
      "id": "5d9aa0d9-a888-46b5-bde8-13cc7a15161c",
      "relationships": {
        "booking": {
          "data": {
            "id": "203f359b-08d6-4e5c-b64c-1aa67cfb775d",
            "type": "booking"
          }
        },
        "channel": {
          "data": {
            "id": "9d571186-b4d0-4792-84b7-af04ab1e28e1",
            "type": "channel"
          }
        },
        "property": {
          "data": {
            "id": "2b4832de-ad00-489b-8acc-b5051ea86d94",
            "type": "property"
          }
        }
      },
      "type": "review"
    }
  ],
  "meta": {
      "limit": 10,
      "order_by": "received_at",
      "order_direction": "desc",
      "page": 1,
      "total": 1
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

**Forbidden Error**

Status Code: `403 Forbidden`

```javascript
{
  "errors": {
    "code": "forbidden",
    "title": "Forbidden"
  }
}
```

This error happened, when Property is not have installed Messages Application.

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Reviews` list in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Booking with provided ID is not present at system.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

### **Get Review by ID**

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/reivews/:review_id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "content": "Guest review test",
      "guest_name": "Guest Name",
      "id": "5d9aa0d9-a888-46b5-bde8-13cc7a15161c",
      "inserted_at": "2022-06-06T05:00:41.020781",
      "is_hidden": false,
      "is_replied": false,
      "ota": "BookingCom",
      "ota_reservation_id": "2328423042",
      "overall_score": 10.0,
      "received_at": "2022-06-06T05:45:23.000000",
      "reply": null,
      "scores": [
        {
          "category": "clean",
          "score": 10
        },
        {
          "category": "comfort",
          "score": 7.5
        },
        {
          "category": "facilities",
          "score": 10
        },
        {
          "category": "location",
          "score": 10
        },
        {
          "category": "staff",
          "score": 10
        },
        {
          "category": "value",
          "score": 7.5
        }
      ],
      "updated_at": "2022-06-06T05:00:41.020781"
    },
    "id": "5d9aa0d9-a888-46b5-bde8-13cc7a15161c",
    "relationships": {
      "booking": {
        "data": {
          "id": "203f359b-08d6-4e5c-b64c-1aa67cfb775d",
          "type": "booking"
        }
      },
      "channel": {
        "data": {
          "id": "9d571186-b4d0-4792-84b7-af04ab1e28e1",
          "type": "channel"
        }
      },
      "property": {
        "data": {
          "id": "2b4832de-ad00-489b-8acc-b5051ea86d94",
          "title": "PROPERTY TITLE",
          "type": "property"
        }
      }
    },
    "type": "review"
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

**Forbidden Error**

Status Code: `403 Forbidden`

```javascript
{
  "errors": {
    "code": "forbidden",
    "title": "Forbidden"
  }
}
```

This error happened, when Property is not have installed Messages Application.

**Not Found Error**

Status Code: `404 Not Found`

```javascript
{
  "errors": {
    "code": "not_found",
    "title": "Not Found"
  }
}
```

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Review` list in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Booking with provided ID is not present at system.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

### Reply to Review

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/reviews/:review_id/reply
```

Payload:

```json
{
  "reply": {
    "reply": "Reply to guest review"
  }
}
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

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
    "code": "not_found",
    "title": "Resouce Not Found"
  }
}
```

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Review` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Message Thread.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Message Thread with provided ID is not present at system.

### Send Guest Review

Method specific only to Airbnb reviews.

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/reviews/:review_id/guest_review
```

Payload:

```json
{
  "review": {
    "scores": [
      {
        "category": "respect_house_rules",
        "rating": 5
      },
      {
        "category": "communication",
        "rating": 5
      },
      {
        "category": "cleanliness",
        "rating": 5
      }
    ],
    "private_review": "private feedback",
    "public_review": "public feedback",
    "is_reviewee_recommended": true
  }
}
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

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
    "code": "not_found",
    "title": "Resouce Not Found"
  }
}
```

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Review` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Message Thread.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Message Thread with provided ID is not present at system.

## Scores

API to read Score per Property

### Get Property Score

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/scores/:property_id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "count": 1122,
      "id": "9e9ac4cd-5a51-4469-9bce-dbce8f5a435f",
      "inserted_at": "2022-06-01T09:43:20.106161",
      "overall_score": 9.15,
      "scores": {
        "accuracy": {
          "count": 18,
          "score": 9.78
        },
        "checkin": {
          "count": 18,
          "score": 9.88
        },
        "clean": {
          "count": 1114,
          "score": 9.55
        },
        "comfort": {
          "count": 1099,
          "score": 9.45
        },
        "communication": {
          "count": 18,
          "score": 9.88
        },
        "facilities": {
          "count": 1100,
          "score": 9.15
        },
        "location": {
          "count": 1115,
          "score": 9.41
        },
        "staff": {
          "count": 1098,
          "score": 9.67
        },
        "value": {
          "count": 1119,
          "score": 9.22
        }
      },
      "updated_at": "2022-06-06T05:01:14.622988"
    },
    "id": "9e9ac4cd-5c16-4469-9bce-dbce8f5a435f",
    "relationships": {
      "property": {
        "data": {
          "id": "57a92389-1cd1-4773-9f0d-47e31d22609f",
          "title": "Hotel",
          "type": "property"
        }
      }
    },
    "type": "score"
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
    "code": "not_found",
    "title": "Resouce Not Found"
  }
}
```

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Score` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Message Thread.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Message Thread with provided ID is not present at system.Close Message Thread

### Get Detailed Property Scores

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/scores/:property_id/detailed
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "count": 1122,
      "id": "9e9ac4cd-c11c-4469-9bce-dbce8f5a435f",
      "inserted_at": "2022-06-01T09:43:20.106161",
      "overall_score": 9.15,
      "scores": {
        "accuracy": {
          "count": 18,
          "score": 9.78
        },
        "checkin": {
          "count": 18,
          "score": 9.88
        },
        "clean": {
          "count": 1114,
          "score": 9.55
        },
        "comfort": {
          "count": 1099,
          "score": 9.45
        },
        "communication": {
          "count": 18,
          "score": 9.88
        },
        "facilities": {
          "count": 1100,
          "score": 9.15
        },
        "location": {
          "count": 1115,
          "score": 9.41
        },
        "staff": {
          "count": 1098,
          "score": 9.67
        },
        "value": {
          "count": 1119,
          "score": 9.22
        }
      },
      "updated_at": "2022-06-06T05:01:14.622988"
    },
    "id": "9e9ac4cd-c11c-4469-9bce-dbce8f5a435f",
    "relationships": {
      "ota_scores": [
        {
          "data": {
            "attributes": {
              "channel_id": "305757c7-bbca-4517-8414-7ee6fb69cfc4",
              "count": 1104,
              "id": "383a4180-1cc1-40f3-a086-de41509da8d5",
              "ota": "BookingCom",
              "overall_score": 9.14,
              "scores": {
                "clean": {
                  "count": 1096,
                  "score": 9.55
                },
                "comfort": {
                  "count": 1099,
                  "score": 9.45
                },
                "facilities": {
                  "count": 1100,
                  "score": 9.15
                },
                "location": {
                  "count": 1097,
                  "score": 9.4
                },
                "staff": {
                  "count": 1098,
                  "score": 9.67
                },
                "value": {
                  "count": 1101,
                  "score": 9.21
                }
              }
            },
            "id": "383a4180-1cc1-40f3-a086-de41509da8d5",
            "type": "ota_score"
          }
        },
        {
          "data": {
            "attributes": {
              "channel_id": "bd90735a-bfd1-4dc7-b302-57bb1fe52909",
              "count": 18,
              "id": "1be8fb58-1cc1-4ee2-b94f-7b3e2bf6c9c0",
              "ota": "AirBNB",
              "overall_score": 9.88,
              "scores": {
                "accuracy": {
                  "count": 18,
                  "score": 9.78
                },
                "checkin": {
                  "count": 18,
                  "score": 9.88
                },
                "clean": {
                  "count": 18,
                  "score": 9.78
                },
                "communication": {
                  "count": 18,
                  "score": 9.88
                },
                "location": {
                  "count": 18,
                  "score": 9.78
                },
                "value": {
                  "count": 18,
                  "score": 9.66
                }
              }
            },
            "id": "1be8fb58-1cc1-4ee2-b94f-7b3e2bf6c9c0",
            "type": "ota_score"
          }
        }
      ],
      "property": {
        "data": {
          "id": "57a92389-1cc1-4773-9f0d-47e31d22609f",
          "title": "Hotel",
          "type": "property"
        }
      }
    },
    "type": "score"
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
    "code": "not_found",
    "title": "Resouce Not Found"
  }
}
```

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Score` and `OTA Scores` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Message Thread.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Message Thread with provided ID is not present at system.
