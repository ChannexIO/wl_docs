---
description: API methods to work with Group Users
---

# Group Users Collection

**Group User** is an association between a Group and a User, represents who can manage a group and all properties under the group with which role and access rights.

## Group Users List

Retrieve list of Group Users associated with a Group.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/group_users?filter[group_id]=GROUP_ID
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
      "type": "group_user"
      "attributes": {
        "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
        "overrides": null,
        "group_id": "52397a6e-c330-44f4-a293-47042d3a3607",
        "role": "owner",
        "user_id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184"
      },
      "relationships": {
        "group": {
          "data": {
            "id": "52397a6e-c330-44f4-a293-47042d3a3607",
            "type": "group"
          }
        },
        "user": {
          "data": {
            "id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184",
            "type": "user",
            "email": "user@test.com",
            "name": "User"
          }
        }
      }
    }
  ]
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

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Group User objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.&#x20;

## Invite User to Group

Create a new Group User. By inviting a User into a Group, you are automatically granting access for this user to all properties in the Group.

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/group_users
```

Query body (JSON):

```javascript
{
  "invite": {
    "group_id": "52397a6e-c330-44f4-a293-47042d3a3607",
    "user_email": "other_user@test.com",
    "role": "user",
    "overrides": {}
  }
}
```

### Success Response
**Success Response Example**

Status Code: `201 Created`

```javascript
{
  "data": {
    "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
    "type": "group_user"
    "attributes": {
      "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
      "overrides": null,
      "group_id": "52397a6e-c330-44f4-a293-47042d3a3607",
      "role": "owner",
      "user_id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184"
    },
    "relationships": {
      "group": {
        "data": {
          "id": "52397a6e-c330-44f4-a293-47042d3a3607",
          "type": "group"
        }
      },
      "user": {
        "data": {
          "id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184",
          "type": "user",
          "email": "user@test.com",
          "name": "User"
        }
      }
    }
  }
}
```

### Error Response
**Bad Request Error Response**

Status Code: `400 Bad Request`

```javascript
{
  "errors": {
    "code": "bad_request",
    "title": "Bad Request",
    "details": "User already invited"
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

**Forbidden Error Response**

Status Code: `403 Forbidden`

```javascript
{
  "errors": {
    "code": "forbidden",
    "title": "Forbidden"
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
      "user_email": [
        "can't be blank"
      ]
    }
  }
}
```

### Fields

**group\_id `[required]`**

String with valid UUID for Group object what you would use as target for invitation.

**user\_email `[required]`**

String with a valid email address of invited user.\
Note: If user is not registered at our system, we are create they account automatically and send email with instructions to on-board into {{APPLICATION_NAME}}.

**role `[required]`**

String with a valid role name.\
Right now you can use 2 roles - `owner` and `user`.

**overrides `[optional]`**

JSON Object with access policies overrides.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Group User object in the answer.\
\
**Bad Request Error**\
Method can return a Bad Request Error result with `400 Bad Request` HTTP Code if provided user already invited.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.\
\
**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if current user not have permissions to invite user into provided property.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Get Group User by ID

Retrieve a Group User by ID.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/group_users/:id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
    "type": "group_user"
    "attributes": {
      "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
      "overrides": null,
      "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
      "role": "owner",
      "user_id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184"
    },
    "relationships": {
      "group": {
        "data": {
          "id": "52397a6e-c330-44f4-a293-47042d3a3607",
          "type": "group"
        }
      },
      "user": {
        "data": {
          "id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184",
          "type": "user",
          "email": "user@test.com",
          "name": "User"
        }
      }
    }
  }
}
```

### Error Response
**Validation Error Response**

Status Code: `401 Unauthorized`

```javascript
{
  "errors": {
    "code": "unauthorized",
    "title": "Unauthorized"
  }
}
```

**Forbidden Error Response**

Status Code: `403 Forbidden`

```javascript
{
  "errors": {
    "code": "forbidden",
    "title": "Forbidden"
  }
}
```

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Group User object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided. \
\
**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if current user not have permissions to call this action.

## Update Group User

Update the Group User information.

### Request
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/group_users/:id
```

Query body (JSON):

```javascript
{
  "group_user": {
    "role": "user",
    "overrides": null
  }
}
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
    "type": "group_user"
    "attributes": {
      "id": "776533f2-c10e-49d8-bddc-14b3e27c2a00",
      "overrides": null,
      "group_id": "52397a6e-c330-44f4-a293-47042d3a3607",
      "role": "owner",
      "user_id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184"
    },
    "relationships": {
      "group": {
        "data": {
          "id": "52397a6e-c330-44f4-a293-47042d3a3607",
          "type": "group"
        }
      },
      "user": {
        "data": {
          "id": "c9cfa184-5095-4ef2-bbe2-e723ffb49184",
          "type": "user",
          "email": "user@test.com",
          "name": "User"
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

**Forbidden Error Response**

Status Code: `403 Forbidden`

```javascript
{
  "errors": {
    "code": "forbidden",
    "title": "Forbidden"
  }
}
```

**Resource Not Found Error Response**

Status Code: `404 Not Found`

```javascript
{
  "errors": {
    "code": "resource_not_found",
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
      "role": [
        "can't be blank"
      ]
    }
  }
}
```

### Fields

Through this method you can update only two fields - role and overrides. Please see Invite User to Group for more detailed information.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Group User object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.\
\
**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if current user not have permissions to call this action.\
\
**Resource Not Found Error**\
Method can return a Resource Not Found Error result with `404 Not Found` HTTP Code if requested Group User is not defined.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Withdraw Group User Access

Revoke Group User access to a specific Group.

### Request
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/group_users/:id
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
**Bad Request Error Response**

Status Code: `400 Bad Request`

```javascript
{
  "errors": {
    "code": "bad_request",
    "details": "User can not withdraw themself",
    "title": "Bad Request"
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

**Forbidden Error Response**

Status Code: `403 Forbidden`

```javascript
{
  "errors": {
    "code": "forbidden",
    "title": "Forbidden"
  }
}
```

**Resource Not Found Error Response**

Status Code: `404 Not Found`

```javascript
{
  "errors": {
    "code": "resource_not_found",
    "title": "Resource Not Found"
  }
}
```

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful.\
\
**Bad Request Error**\
Method can return a Bad Request Error result with `400 Bad Request` HTTP Code if user will try to remove them self.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.\
\
**Forbidden Error**\
Method can return a Forbidden Error result with `403 Forbidden` HTTP Code if current user not have permissions to call this action.\
\
**Resource Not Found Error**\
Method can return a Resource Not Found Error result with `404 Not Found` HTTP Code if requested Group User is not defined.
