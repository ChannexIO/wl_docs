# Taxes and Tax Sets

To represent Taxes associated with Property and Rate Plans at {{APPLICATION_NAME}} you can use Taxes and Tax Sets.

### **Taxes**

Entity which represent specific tax applicable to your Rate Plan or Property. Example: VAT 20%, City Tax 1 EUR per Guest per Night or any other.

### Tax Sets

Entity which represent group of Taxes applicable to your Rate Plan or Property.

Each property can have many Tax Sets and Taxes, but only one can be selected as Default Tax Set. Default Tax Set will be applied to each new Rate Plan automatically.

## Taxes List

Retrieve list of Taxes associated with user.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/taxes
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "applicable_after": null,
        "applicable_before": null,
        "currency": null,
        "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
        "is_inclusive": true,
        "logic": "percent",
        "max_nights": null,
        "rate": "10.00",
        "skip_nights": null,
        "title": "10% VAT",
        "type": "tax"
      },
      "relationships": {
        "property": {
          "data": {
            "type": "property",
            "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
          }
        }
      },
      "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
      "type": "tax"
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Tax objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

## Get Tax by ID

Retrieve specific Tax associated with User by ID.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/taxes/:id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "applicable_after": null,
      "applicable_before": null,
      "currency": null,
      "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
      "is_inclusive": true,
      "logic": "percent",
      "max_nights": null,
      "rate": "10.00",
      "skip_nights": null,
      "title": "10% VAT",
      "type": "tax"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    },
    "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
    "type": "tax"
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


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Tax object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Tax.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Tax with provided ID is not present at system.

## Create Tax

Create a new Tax.

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/taxes
```

Query body (JSON):

```javascript
{
  "tax": {
    "title": "VAT",
    "logic": "percent",
    "type": "tax",
    "rate": "20.00",
    "is_inclusive": true,
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920"
  }
}
```

### Success Response
**Success Response Example**

Status Code: `201 Created`

```javascript
{
  "data": {
    "attributes": {
      "applicable_after": null,
      "applicable_before": null,
      "currency": null,
      "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
      "is_inclusive": true,
      "logic": "percent",
      "max_nights": null,
      "rate": "20.00",
      "skip_nights": null,
      "title": "20% VAT",
      "type": "tax"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    },
    "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
    "type": "tax"
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
      "title": [
        "can't be blank"
      ]
    }
  }
}
```


### Fields

**title `[required]`**

Any non-empty string with maximum length of 255 symbols.\
Note: The Group will be represented in the system under that title.

**logic `[required]`**

One of possible values: `percent` , `per_room`, `per_room_per_night`, `per_person`, `per_person_per_night`, `per_night`, `per_booking`

**type `[required]`**

One of possible values: `tax`, `fee`, `city tax`.

**rate `[required]`**

String value with amount applicable to tax. If `logic` is `percent`, can be between 0 and 100 only. At other cases, represent fixed amount of tax.

**currency `[required]`\***

Required only of `logic` is not `percent`. Should a valid currency code. Represent Tax amount currency.

**is\_inclusive `[required]`**

Boolean value. Represent include tax into room price or should be added atop.

**property\_id `[required]`**

UUID. Relation to associated Property.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Tax object in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Update Tax

Update a Tax.

### Request
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/taxes/:id
```

Query body (JSON):

```javascript
{
  "tax": {
    "title": "New Tax Title"
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
      "applicable_after": null,
      "applicable_before": null,
      "currency": null,
      "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
      "is_inclusive": true,
      "logic": "percent",
      "max_nights": null,
      "rate": "20.00",
      "skip_nights": null,
      "title": "New Tax Title",
      "type": "tax"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    },
    "id": "6d30137e-74a1-41f6-aa96-2c0371e94dbf",
    "type": "tax"
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
      "title": [
        "can't be blank"
      ]
    }
  }
}
```


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Tax object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Tax with provided ID is not present at system.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Remove Tax

Remove a Tax.

### Request
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/taxes/:id
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
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Tax with provided ID is not present at system.

## Tax Sets List

Retrieve list of Tax Sets associated with user.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/tax_sets
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": [
    {
      "attributes": {
        "currency": "USD",
        "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
        "taxes": [
          {
            "applicable_after": null,
            "applicable_before": null,
            "currency": null,
            "id": "c1fb94b3-ce95-4233-be0e-2748b3728715",
            "is_inclusive": true,
            "logic": "percent",
            "max_nights": null,
            "rate": "10.00",
            "skip_nights": null,
            "taxes": [],
            "title": "10% IVA",
            "type": "tax"
          },
          {
            "applicable_after": null,
            "applicable_before": null,
            "currency": null,
            "id": "a3c4f3c8-841c-4492-b5ff-ce9ca92c1c83",
            "is_inclusive": false,
            "logic": "percent",
            "max_nights": null,
            "rate": "3.00",
            "skip_nights": null,
            "taxes": [],
            "title": "3% TBID",
            "type": "tax"
          }
        ],
        "title": "Tax Set Title"
      },
      "relationships": {
        "property": {
          "data": {
            "type": "property",
            "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
          }
        }
      },
      "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
      "type": "tax_set"
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Tax Set objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

## Get Tax Set by ID

Retrieve specific Tax Set associated with User by ID.

### Request
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/tax_sets/:id
```

### Success Response
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "currency": "USD",
      "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
      "taxes": [
        {
          "applicable_after": null,
          "applicable_before": null,
          "currency": null,
          "id": "c1fb94b3-ce95-4233-be0e-2748b3728715",
          "is_inclusive": true,
          "logic": "percent",
          "max_nights": null,
          "rate": "10.00",
          "skip_nights": null,
          "taxes": [],
          "title": "10% IVA",
          "type": "tax"
        },
        {
          "applicable_after": null,
          "applicable_before": null,
          "currency": null,
          "id": "a3c4f3c8-841c-4492-b5ff-ce9ca92c1c83",
          "is_inclusive": false,
          "logic": "percent",
          "max_nights": null,
          "rate": "3.00",
          "skip_nights": null,
          "taxes": [],
          "title": "3% TBID",
          "type": "tax"
        }
      ],
      "title": "Tax Set Title"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    },
    "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
    "type": "tax_set"
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


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Tax Set object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Tax Set.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Tax Set with provided ID is not present at system.

## Create Tax Set

Create a new Tax Set.

### Request
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/tax_sets
```

Query body (JSON):

```javascript
{
  "tax_set": {
    "title": "Tax Set Title",
    "property_id": "716305c4-561a-4561-a187-7f5b8aeb5920",
    "taxes": [
      {"id": "c1fb94b3-ce95-4233-be0e-2748b3728715"},
      {"id": "a3c4f3c8-841c-4492-b5ff-ce9ca92c1c83"}
    ],
    "currency": "USD"
  }
}
```

### Success Response
**Success Response Example**

Status Code: `201 Created`

```javascript
{
  "data": {
    "attributes": {
      "currency": "USD",
      "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
      "taxes": [
        {
          "applicable_after": null,
          "applicable_before": null,
          "currency": null,
          "id": "c1fb94b3-ce95-4233-be0e-2748b3728715",
          "is_inclusive": true,
          "logic": "percent",
          "max_nights": null,
          "rate": "10.00",
          "skip_nights": null,
          "taxes": [],
          "title": "10% IVA",
          "type": "tax"
        },
        {
          "applicable_after": null,
          "applicable_before": null,
          "currency": null,
          "id": "a3c4f3c8-841c-4492-b5ff-ce9ca92c1c83",
          "is_inclusive": false,
          "logic": "percent",
          "max_nights": null,
          "rate": "3.00",
          "skip_nights": null,
          "taxes": [],
          "title": "3% TBID",
          "type": "tax"
        }
      ],
      "title": "Tax Set Title"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    },
    "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
    "type": "tax_set"
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
      "title": [
        "can't be blank"
      ]
    }
  }
}
```


### Fields

**title `[required]`**

Any non-empty string with maximum length of 255 symbols.\
Note: The Tax Set will be represented in the system under that title.

**property\_id `[required]`**

UUID of Property which is associated with Tax Set.\
Note: If it is first `Tax Set` for `Property`, it will be automatically installed as `default_tax_set` for this property.

**currency `[required]`\***

String. Should a valid currency code. Represent Tax Set currency.

**taxes `[required]`**

List of object with associated taxes IDs. Can contain another taxes inside.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Tax Set object in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Update Tax Set

Update a Tax Set.

### Request
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/tax_set/:id
```

Query body (JSON):

```javascript
{
  "tax": {
    "title": "New Tax Set Title"
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
      "currency": "USD",
      "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
      "taxes": [
        {
          "applicable_after": null,
          "applicable_before": null,
          "currency": null,
          "id": "c1fb94b3-ce95-4233-be0e-2748b3728715",
          "is_inclusive": true,
          "logic": "percent",
          "max_nights": null,
          "rate": "10.00",
          "skip_nights": null,
          "taxes": [],
          "title": "10% IVA",
          "type": "tax"
        },
        {
          "applicable_after": null,
          "applicable_before": null,
          "currency": null,
          "id": "a3c4f3c8-841c-4492-b5ff-ce9ca92c1c83",
          "is_inclusive": false,
          "logic": "percent",
          "max_nights": null,
          "rate": "3.00",
          "skip_nights": null,
          "taxes": [],
          "title": "3% TBID",
          "type": "tax"
        }
      ],
      "title": "New Tax Set Title"
    },
    "relationships": {
      "property": {
        "data": {
          "type": "property",
          "id": "716305c4-561a-4561-a187-7f5b8aeb5920"
        }
      }
    },
    "id": "b70b756d-0b81-431d-a35c-f3dee28a00a7",
    "type": "tax_set"
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
      "title": [
        "can't be blank"
      ]
    }
  }
}
```


### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Tax Set object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Tax Set with provided ID is not present at system.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Remove Tax Set

Remove a Tax Set.

### Request
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/tax_sets/:id
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
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Tax Set with provided ID is not present at system.
