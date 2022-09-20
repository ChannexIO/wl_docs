---
description: API to work with Chat
---

# Messages Collection

At {{APPLICATION_NAME}} you are able to work with `Channel Messages`, it is an unified API to work with chat messages from Booking.com and Airbnb (only these 2 channels are supported currently).

The messages API has 2 parts - Booking Messages and Message Thread.

### NOTE
If you would like a quick start you can use the iframe feature to insert our chat interface into your PMS.

## Enable chat on the Property

Go here to applications: [https://{{domain}}/applications](https://{{domain}}/applications)

Add the chat app to the property, then the API and UI for chat will be available. Also chat will be available for that property on the {{APPLICATION_NAME}} mobile app.

## Data Structures

### Message

```javascript
{
  "message": "Client Message",
  "attachments": [],
  "sender": "guest",
  "inserted_at": "2021-07-28T04:25:15.000000",
  "updated_at": "2021-07-28T04:25:15.000000"
}
```

`message` Text field with Guest message. Can be empty, if `attachments` is present.

`attachments` List of links to Attachments associated with message.

`sender` Enum field to represent message direction. Can be `guest` or `property`.

`inserted_at` Timestamp, when message was received.

`updated_at` Timestamp, when message was updated.

### Message Thread

```javascript
{
  "title": "Maldonado Roxy",
  "is_closed": false,
  "provider": "BookingCom",
  "message_count": 2,
  "last_message": {
    "attachments": [],
    "inserted_at": "2021-07-27T09:43:05.864520",
    "message": "Thanks for your message.",
    "sender": "property"
  },
  "last_message_received_at": "2021-07-27T09:43:05.864520",
  "inserted_at": "2021-07-27T09:32:14.281622",
  "updated_at": "2021-07-27T09:43:05.868034"
}
```

`title` String field with Message Thread title, usually equal to the Customer name.

`is_closed` Boolean marker to show if the thread is open or not.

`provider` Message provider. String. (This will be "BookingCom" (Booking.com) or Airbnb) Later there will be more providers.

`message_count` Integer field to represent count of messages inside Message Thread.

`last_message` - Message entity.

`last_message_received_at` Timestamp to represent when last message was received.

`inserted_at` Timestamp, when Message Thread was received.

`updated_at` Timestamp, when Message Thread was updated.

## Booking Messages

Simple API to send and read messages at the Booking level.

### Get Messages per Booking

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/bookings/:booking_id/messages
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "message": "Message",
        "attachments": [],
        "sender": "guest",
        "inserted_at": "2021-07-28T04:25:15.000000",
        "updated_at": "2021-07-28T04:25:22.613482"
      },
      "id": "5848b518-07a4-4c8c-a998-2784d638ba30",
      "relationships": {
        "message_thread": {
          "data": {
            "id": "4e160f0b-016a-424a-a30d-a9f0a8b1cbaa",
            "type": "message_thread"
          }
        }
      },
      "type": "message"
    }
  ],
  "meta": {
    "limit": 10,
    "order_by": "inserted_at",
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

**Not Supported**

Status Code: `422 Unprocessable Entity`

```javascript
{
  "errors": {
    "code": "not_supported",
    "title": "Method not supported"
  }
}
```

This error happened, when Property connected to Messages Application, but original Booking OTA is not support Message API.

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Messages` list in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Booking with provided ID is not present at system.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

**Not Supported Error**\
****Method can return a Not Supported Error result with `422 Unprocessable Entity` HTTP Code if Booking with provided ID associated with OTA, which not support Messages API.

### Send Message to Booking

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/bookings/:booking_id/messages
```

Payload:

```javascript
{
  "message": {
    "message": "MESSAGE CONTENT"
  }
}
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "attachments": [],
      "inserted_at": "2021-07-30T09:53:46.563215",
      "message": "MESSAGE CONTENT",
      "sender": "property",
      "updated_at": "2021-07-30T09:53:46.563215"
    },
    "id": "34849183-7c36-4ac4-9103-cfaeecbc2cc8",
    "relationships": {
      "message_thread": {
        "data": {
          "id": "4e160f0b-016a-424a-a30d-a9f0a8b1cbaa",
          "type": "message_thread"
        }
      },
      "user": {
        "data": {
          "id": "c9080091-6b9f-4868-a2ca-691036d29ed0",
          "type": "user"
        }
      }
    },
    "type": "message"
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

**Not Supported**

Status Code: `422 Unprocessable Entity`

```javascript
{
  "errors": {
    "code": "not_supported",
    "title": "Method not supported"
  }
}
```

This error happened, when Property connected to Messages Application, but original Booking OTA is not support Message API.

#### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Message` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Booking with provided ID is not present at system.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

**Not Supported Error**\
****Method can return a Not Supported Error result with `422 Unprocessable Entity` HTTP Code if Booking with provided ID associated with OTA, which not support Messages API.

## Message Threads

API methods to work with Message Threads.

### Get Message Threads

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/message_threads
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "title": "Maldonado Roxy",
        "is_closed": false,
        "provider": "BookingCom",
        "message_count": 2,
        "last_message": {
          "attachments": [],
          "inserted_at": "2021-07-27T09:43:05.864520",
          "message": "Thanks for your message.",
          "sender": "property"
        },
        "last_message_received_at": "2021-07-27T09:43:05.864520",
        "inserted_at": "2021-07-27T09:32:14.281622",
        "updated_at": "2021-07-27T09:43:05.868034"
      },
      "id": "20d1b08c-190e-4068-a77d-4a909a21835d",
      "relationships": {
        "booking": {
          "data": {
            "id": "4d8240fd-d709-454b-a866-08bca2a5a909",
            "type": "booking"
          }
        },
        "channel": {
          "data": {
            "id": "351f145d-cb8c-4df7-9f12-2f6854991b91",
            "type": "channel"
          }
        },
        "property": {
          "data": {
            "id": "71d34923-a8be-4682-9625-e4a2f080df92",
            "type": "property"
          }
        }
      },
      "type": "message_thread"
    }
  ],
  "meta": {
    "limit": 10,
    "order_by": "inserted_at",
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Message Threads` list in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

### Get Message Thread by ID

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/message_threads/:id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "title": "Maldonado Roxy",
      "is_closed": false,
      "provider": "BookingCom",
      "message_count": 2,
      "last_message": {
        "attachments": [],
        "inserted_at": "2021-07-27T09:43:05.864520",
        "message": "Thanks for your message.",
        "sender": "property"
      },
      "last_message_received_at": "2021-07-27T09:43:05.864520",
      "inserted_at": "2021-07-27T09:32:14.281622",
      "updated_at": "2021-07-27T09:43:05.868034"
    },
    "id": "20d1b08c-190e-4068-a77d-4a909a21835d",
    "relationships": {
      "booking": {
        "data": {
          "id": "4d8240fd-d709-454b-a866-08bca2a5a909",
          "type": "booking"
        }
      },
      "channel": {
        "data": {
          "id": "351f145d-cb8c-4df7-9f12-2f6854991b91",
          "type": "channel"
        }
      },
      "property": {
        "data": {
          "id": "71d34923-a8be-4682-9625-e4a2f080df92",
          "type": "property"
        }
      }
    },
    "type": "message_thread"
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Message Thread` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

### Get Message for Message Thread

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/message_threads/:id/messages
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "attachments": [],
        "inserted_at": "2021-07-30T09:53:46.563215",
        "message": "MESSAGE CONTENT",
        "sender": "property",
        "updated_at": "2021-07-30T09:53:46.563215"
      },
      "id": "34849183-7c36-4ac4-9103-cfaeecbc2cc8",
      "relationships": {
        "message_thread": {
          "data": {
            "id": "20d1b08c-190e-4068-a77d-4a909a21835d",
            "type": "message_thread"
          }
        },
        "user": {
          "data": {
            "id": "c9080091-6b9f-4868-a2ca-691036d29ed0",
            "type": "user"
          }
        }
      },
      "type": "message"
    },
    {
      "attributes": {
        "attachments": [],
        "inserted_at": "2021-07-28T04:25:15.000000",
        "message": "Special request text 1",
        "sender": "guest",
        "updated_at": "2021-07-28T04:25:22.613482"
      },
      "id": "5848b518-07a4-4c8c-a998-2784d638ba30",
      "relationships": {
        "message_thread": {
          "data": {
            "id": "20d1b08c-190e-4068-a77d-4a909a21835d",
            "type": "message_thread"
          }
        }
      },
      "type": "message"
    }
  ],
  "meta": {
    "limit": 10,
    "order_by": "inserted_at",
    "order_direction": "desc",
    "page": 1,
    "total": 2
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Messages` associated with `Message Thread` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Booking.

**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if Property, associated with requested Booking, is not connected to Messages Application.

### Send Message to Message Thread

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/message_threads/:id/messages
```

Payload:

```javascript
{
  "message": {
    "message": "MESSAGE CONTENT"
  }
}
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "attachments": [],
      "inserted_at": "2021-07-30T09:53:46.563215",
      "message": "MESSAGE CONTENT",
      "sender": "property",
      "updated_at": "2021-07-30T09:53:46.563215"
    },
    "id": "34849183-7c36-4ac4-9103-cfaeecbc2cc8",
    "relationships": {
      "message_thread": {
        "data": {
          "id": "4e160f0b-016a-424a-a30d-a9f0a8b1cbaa",
          "type": "message_thread"
        }
      },
      "user": {
        "data": {
          "id": "c9080091-6b9f-4868-a2ca-691036d29ed0",
          "type": "user"
        }
      }
    },
    "type": "message"
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Message` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Message Thread.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Message Thread with provided ID is not present at system.

### Close Message Thread

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/message_threads/:id/close
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "title": "Maldonado Roxy",
      "is_closed": true,
      "provider": "BookingCom",
      "message_count": 2,
      "last_message": {
        "attachments": [],
        "inserted_at": "2021-07-27T09:43:05.864520",
        "message": "Thanks for your message.",
        "sender": "property"
      },
      "last_message_received_at": "2021-07-27T09:43:05.864520",
      "inserted_at": "2021-07-27T09:32:14.281622",
      "updated_at": "2021-07-27T09:43:05.868034"
    },
    "id": "20d1b08c-190e-4068-a77d-4a909a21835d",
    "relationships": {
      "booking": {
        "data": {
          "id": "4d8240fd-d709-454b-a866-08bca2a5a909",
          "type": "booking"
        }
      },
      "channel": {
        "data": {
          "id": "351f145d-cb8c-4df7-9f12-2f6854991b91",
          "type": "channel"
        }
      },
      "property": {
        "data": {
          "id": "71d34923-a8be-4682-9625-e4a2f080df92",
          "type": "property"
        }
      }
    },
    "type": "message_thread"
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a `Message Thread` in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Message Thread.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Message Thread with provided ID is not present at system.
