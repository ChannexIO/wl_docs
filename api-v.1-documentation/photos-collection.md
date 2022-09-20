---
description: API Methods to work with Photos
---

# Photos Collection

**Photo** is entity to represent photo associated with Property and Room Type (optional).

Right now, Create Photo API doesn't support upload files, but we have this feature at our Roadmap. Please, use our application UI to upload photos, this way provide better User Experience for you and allow you use all features associated with [Photo transformations](photos-collection.md#photo-transformations).

## Photos List

Retrieve list of Photos associated with user Properties.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/photos
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
        "author": null,
        "description": null,
        "id": "ae75d43e-21c5-46b6-a593-5046791f7841",
        "kind": "ad",
        "position": 0,
        "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
        "room_type_id": null,
        "url": "https://img.{{APPLICATION_DOMAIN}}/312fa6cb-8151-409b-b612-773e271a76df/"
      },
      "id": "ae75d43e-21c5-46b6-a593-5046791f7841",
      "type": "photo"
    }
  ],
  "meta": {}
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a list of Photo objects in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.&#x20;

## Get Photo by ID

Retrieve specific Photo by ID.

{% tabs %}
{% tab title="Request" %}
Request:

```
GET https://{{STAGING_DOMAIN}}/api/v1/photos/:id
```
{% endtab %}

{% tab title="Success Response" %}
**Success Response Example**

Status Code: `200 OK`

```javascript
{
  "data": {
    "attributes": {
      "author": null,
      "description": null,
      "id": "ae75d43e-21c5-46b6-a593-5046791f7841",
      "kind": "ad",
      "position": 0,
      "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
      "room_type_id": null,
      "url": "https://img.{{APPLICATION_DOMAIN}}/312fa6cb-8151-409b-b612-773e271a76df/"
    },
    "id": "ae75d43e-21c5-46b6-a593-5046791f7841",
    "type": "photo"
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
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Photo object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided or User not have access to requested Photo.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Photo with provided ID is not present at system.

## Create Photo

Create new Photo.

{% tabs %}
{% tab title="Request" %}
Request:

```
POST https://{{STAGING_DOMAIN}}/api/v1/photos
```

Query body (JSON):

```javascript
{
  "photo": {
    "author": "Author Name",
    "description": "Room View",
    "kind": "photo",
    "position": 0,
    "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/",
    "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
    "room_type_id": null
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
    "attributes": {
      "author": "Author Name",
      "description": "Room View",
      "id": "656d8cab-beaa-45a3-8ddb-44684816edba",
      "kind": "photo",
      "position": 0,
      "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
      "room_type_id": null,
      "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
    },
    "id": "656d8cab-beaa-45a3-8ddb-44684816edba",
    "type": "photo"
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
      "url": [
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

String with valid UUID of Property object what you would like to associate with created Photo.

**url `[required]`**

Valid URL address of Photo image.

**room\_type\_id `[optional]`**

String with valid UUID of Room Type object what you would like to associate with created Photo.\
If `room_type_id` is `null`, Photo will be associated only with Property.

**kind `[optional]`**

One of three possible values: `photo`, `ad` (advertising), `menu` (restaurant menu photo).\
By default value kind will be equal to `photo`.

**author `[optional]`**

Name of photo author.

**description `[optional]`**

Text with Photo description.

**position `[optional]`**

Any positive integer number.\
This field represent Photo position at list of Property or Room Type Photos. Photo with position equal to 0 is used as Cover Photo.\
Position should be uniq per `property_id` and `room_type_id` combination.

### Returns

**Success**\
Method can return a Success result with `201 Created` HTTP Code if operation is successful. Will contain a Photo object in the answer.

**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Update Photo

Update Photo.

{% tabs %}
{% tab title="Request" %}
Request:

```
PUT https://{{STAGING_DOMAIN}}/api/v1/photos/:id
```

Query body (JSON):

```javascript
{
  "photo": {
    "author": "Author Name",
    "description": "Room View",
    "kind": "photo",
    "position": 0,
    "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/",
    "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
    "room_type_id": null
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
    "attributes": {
      "author": "Author Name",
      "description": "Room View",
      "id": "656d8cab-beaa-45a3-8ddb-44684816edba",
      "kind": "photo",
      "position": 0,
      "property_id": "52397a6e-c330-44f4-a293-47042d3a3607",
      "room_type_id": null,
      "url": "https://img.{{APPLICATION_DOMAIN}}/af08bc1d-8074-476c-bdb7-cec931edaf6a/"
    },
    "id": "656d8cab-beaa-45a3-8ddb-44684816edba",
    "type": "photo"
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
      "url": [
        "can't be blank"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Fields

This method use same fields as Create Photo method.

### Returns

**Success**\
Method can return a Success result with `200 OK` HTTP Code if operation is successful. Will contain a Photo object in the answer.\
\
**Unauthorised Error**\
Method can return a Unauthorised Error result with `401 Unauthorized` HTTP Code if wrong Bearer Token provided.

**Not Found Error**\
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Photo with provided ID is not present at system.

**Validation Error**\
Method can return a Validation Error result with `422 Unprocessable Entity` HTTP Code if any validation rule is failed.

## Remove Photo

Remove Photo.

{% tabs %}
{% tab title="Request" %}
Request:

```
DELETE https://{{STAGING_DOMAIN}}/api/v1/photos/:id
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
****Method can return a Not Found Error result with `404 Not Found` HTTP Code if Photo with provided ID is not present at system.

## Batch operations

API methods to create or update [Property](hotels-collection.md) or [Room Type](room-types-collection.md) have implementation of Photo batch operations.\
To create Property with Photos, you can pass list of Photo arguments as value into `content.photos` key of affected object.

Batch operations support logic to create new Photo entity associated with parent object, update existed photos or drop it.

To update Photo at batch operation, you must provide photo with it ID.

To drop Photo at batch operation, you can pass additional optional key: `is_removed` with value equal to `true` at Photo object what are you like to remove.

## Photo Transformations

### Concept

| <p><a href="https://ucarecdn.com/5e39e315-c06c-4d81-9b4a-35fca661621c/-/preview/image.jpg"><img src="https://ucarecdn.com/5e39e315-c06c-4d81-9b4a-35fca661621c/-/scale_crop/220x220/center/image.jpg" alt=""></a><br>Original image</p> | <p><a href="https://ucarecdn.com/5e39e315-c06c-4d81-9b4a-35fca661621c/-/preview/-/sharp/15/image.jpg"><img src="https://ucarecdn.com/5e39e315-c06c-4d81-9b4a-35fca661621c/-/scale_crop/220x220/center/-/sharp/15/image.jpg" alt=""></a><br>Sharpen <code>-/sharp/15/</code></p> | <p><a href="https://ucarecdn.com/5e39e315-c06c-4d81-9b4a-35fca661621c/-/preview/-/enhance/100/image.jpg"><img src="https://ucarecdn.com/5e39e315-c06c-4d81-9b4a-35fca661621c/-/scale_crop/220x220/center/-/enhance/100/image.jpg" alt=""></a><br>Enhance <code>-/enhance/100/</code></p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Photo Transformations can help you enhance images, optimize them, implement responsive images or art direction. In any of the listed cases, you will either go with modifying original images or making several versions of an original. All that can be done on-the-fly by including the respective URL directives in CDN URLs related to photos. For example,

```
https://img.{{APPLICATION_DOMAIN}}/:uuid/-/resize/200x/
```

We support many input image formats, while the processed outputs will always be set to one of these three: `PNG`, `JPEG` or `WebP`.

### Resize Image Proportionally, `preview`

`-/preview/`\
`-/preview/:two_dimensions/`

Reduces an image proportionally for it to fit into the given dimensions in pixels. If dimensions are not specified, we use the default values of `2048x2048` pixels.

```
/:uuid/-/preview/
/:uuid/-/preview/300x500/
```

### Resize Image, `resize`

`/resize/:one_or_two_dimensions/`

Resizes an image to fit into the specified dimensions. With just a single linear dimension specified, preserves your original aspect ratio and resizes an image along one of its axes.

```
/:uuid/-/resize/200x200/
/:uuid/-/resize/200x/
/:uuid/-/resize/x200/
```

### Crop Image, `crop`

`-/crop/:two_dimensions/`\
`-/crop/:two_dimensions/:two_coords/`\
`-/crop/:two_dimensions/center/`

Crops an image using specified dimensions and offsets. If no offset values are passed into the operation, the top-left image corner is used by default.

| <p><a href="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/preview/"><img src="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/preview/440x440/" alt=""></a><br>Original image.</p> | <p><a href="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/crop/2000x1325/center/"><img src="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/crop/2000x1325/center/-/preview/440x440/-/quality/lightest/" alt=""></a><br><code>/crop/2000x1325/center/</code></p> | <p><a href="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/crop/640x424/2560,830/"><img src="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/crop/640x424/2560,830/-/preview/440x440/-/quality/lightest/" alt=""></a><br><code>/crop/960x636/2400,700/</code></p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

If given dimensions are greater than the ones of the original image, the image is rendered inside a color-filled box. The color of that box can be changed via the `setfill` operation.

### Downscale and Crop Image, `scale_crop`

`-/scale_crop/:two_dimensions/`\
`-/scale_crop/:two_dimensions/:type/`\
`-/scale_crop/:two_dimensions/:offsets/`

When `:type` is not specified.

Scales down an image until one of its dimensions gets equal to some of the specified ones; the rest is cropped. This proves useful when your want to fit as much of your image as possible into a box. Let us compare the two resizing methods:

| <p><a href="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/resize/1024x1024/"><img src="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/resize/1024x1024/-/preview/440x440/-/quality/lighter/" alt=""></a><br><code>/resize/1024x1024/</code><br>Requested size.<br>Distorted.</p> | <p><a href="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/preview/1024x1024/"><img src="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/preview/1024x1024/-/preview/440x440/-/quality/lighter/" alt=""></a><br><code>/preview/1024x1024/</code><br>Adjusted size.<br>No distortion.</p> | <p><a href="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/scale_crop/1024x1024/center/"><img src="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/scale_crop/1024x1024/center/-/preview/440x440/-/quality/lighter/" alt=""></a><br><code>/scale_crop/1024x1024/</code><br>Requested size.<br>No distortion.</p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

When `:type` is set to `smart`.

Switching the crop type to smart enables the content-aware mechanics: detecting faces and other visually important objects or areas in images.

Example:

| <p><a href="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/scale_crop/1024x1024/smart/"><img src="https://ucarecdn.com/22240276-2f06-41f8-9411-755c8ce926ed/-/scale_crop/1024x1024/smart/-/preview/440x440/-/quality/lighter/" alt=""></a><br><code>/scale_crop/1024x1024/smart/</code><br>Smart cropping.<br></p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Set `resize` Behavior, `stretch`

`-/stretch/:mode/`

Sets the `resize` behavior when a source image is smaller than the resulting dimensions. The following modes can apply:

* `on` — stretches an image up, the default option.
* `off` — forbids stretching an image along any dimension that exceeds image size along any of its axes.
* `fill` — does not stretch an image, the color-filled frame is rendered around instead, the default fill color is used.

| <p><a href="https://ucarecdn.com/52da3bfc-7cd8-4861-8b05-126fef7a6994/-/quality/best/-/preview/160x160/-/resize/220x/"><img src="https://ucarecdn.com/52da3bfc-7cd8-4861-8b05-126fef7a6994/-/quality/best/-/preview/160x160/-/resize/220x/" alt=""></a><br><code>/preview/160x160/-</code><br><code>/resize/220x/</code></p> | <p><a href="https://ucarecdn.com/52da3bfc-7cd8-4861-8b05-126fef7a6994/-/quality/best/-/preview/160x160/-/stretch/off/-/resize/240x160/"><img src="https://ucarecdn.com/52da3bfc-7cd8-4861-8b05-126fef7a6994/-/quality/best/-/preview/160x160/-/stretch/off/-/resize/220x/" alt=""></a><br><code>/preview/160x160/-</code><br><code>/stretch/off/-</code><br><code>/resize/220x/</code></p> | <p><a href="https://ucarecdn.com/52da3bfc-7cd8-4861-8b05-126fef7a6994/-/quality/best/-/preview/160x160/-/setfill/8d8578/-/stretch/fill/-/resize/220x/"><img src="https://ucarecdn.com/52da3bfc-7cd8-4861-8b05-126fef7a6994/-/quality/best/-/preview/160x160/-/setfill/8d8578/-/stretch/fill/-/resize/220x/" alt=""></a><br><code>/preview/160x160/-</code><br><code>/stretch/fill/-</code><br><code>/resize/220x/</code></p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Set Fill Color, `setfill`

`-/setfill/:color/`

Sets the fill color used with `crop`, `stretch` or when converting an alpha channel enabled image to JPEG. The operation uses hexadecimal notation to define colors.

| <p><a href="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/preview/440x380/-/crop/440x380/center/"><img src="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/quality/lightest/-/preview/440x380/-/crop/440x380/center/" alt=""></a><br><code>/preview/440x380/-</code><br><code>/crop/440x380/center/</code></p> | <p><a href="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/preview/440x380/-/setfill/ece3d2/-/crop/440x380/center/"><img src="https://ucarecdn.com/c4b32a69-f817-48be-b918-7eb6718f7aca/-/quality/lightest/-/preview/440x380/-/setfill/ece3d2/-/crop/440x380/center/" alt=""></a><br><code>/preview/440x380/-</code><br><code>/setfill/ece3d2/-</code><br><code>/crop/440x380/center/</code></p> | <p><a href="https://ucarecdn.com/b18b5179-b9f6-4fdc-9920-5539f938fc44/-/setfill/ece3d2/-/format/jpeg/"><img src="https://ucarecdn.com/b18b5179-b9f6-4fdc-9920-5539f938fc44/-/quality/better/-/setfill/ece3d2/-/format/jpeg/-/crop/220x190/center/" alt=""></a><br><code>/setfill/ece3d2/-</code><br><code>/format/jpeg/</code></p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Changing Image Format, `format`

`-/format/:format/`

Converts an image to one of the following formats:

* `jpeg` — JPEG is a lossy image format (good compression, good for photos). JPEG doesn’t support an alpha channel, hence you can use the `setfill` operation that sets a background color. All browsers support JPEG.
* `png` — PNG is a lossless format (good compression only for graphics) with alpha channel support. Supported by all browsers.
* `webp` — WebP is a modern image format that supports alpha channel and lossy compression. The format is good for all kinds of images but supported by a limited number of browsers.
* `auto` — the image format used for content delivery is set according to the presence of an alpha channel in your input and capabilities of a client.

Technically, the default behavior of `auto` is about always trying to deliver WebP images based on checking the `Accept` header.

| <p><a href="https://ucarecdn.com/6ad747ff-dda6-4508-8864-5fc83f7fe645/-/setfill/ece3d2/-/format/png/"><img src="https://ucarecdn.com/6ad747ff-dda6-4508-8864-5fc83f7fe645/-/setfill/ece3d2/-/format/png/" alt=""></a><br>400x301 <code>png</code> 116Kb<br>Transparent</p> | <p><a href="https://ucarecdn.com/6ad747ff-dda6-4508-8864-5fc83f7fe645/-/setfill/ece3d2/-/format/jpeg/"><img src="https://ucarecdn.com/6ad747ff-dda6-4508-8864-5fc83f7fe645/-/setfill/ece3d2/-/format/jpeg/" alt=""></a><br>400x301 <code>jpeg</code> 16Kb<br>Opaque</p> | <p><a href="https://ucarecdn.com/6ad747ff-dda6-4508-8864-5fc83f7fe645/-/setfill/ece3d2/-/format/webp/"><img src="https://ucarecdn.com/6ad747ff-dda6-4508-8864-5fc83f7fe645/-/setfill/ece3d2/-/format/auto/" alt=""></a><br>400x301 <code>webp</code> 15Kb<br>Transparent, size is equal<br>to the opaque one.</p> |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

Beside using `-/format/auto/`, there is another way to control which image format is delivered to a client. In HTML 5, you can force the browser to automatically choose a WebP image version over others. This is done by using `<picture>`. Simply, wrap your `<img>` element with `<picture>` and add `<source>` having its type set to `type="image/webp"`. Compatible browsers will then automatically load the WebP image version; others will take either JPEG or PNG.

```
<picture>
  <source srcset="//img.{{APPLICATION_DOMAIN}}/:uuid/:operations/-/format/webp/" type="image/webp"/>
  <img src="//img.{{APPLICATION_DOMAIN}}/:uuid/:operations/-/format/auto/"/>
</picture>
```

### Changing Image Quality, `quality`

`-/quality/:value/`

Sets the level of image quality that affects file sizes and hence loading speeds and volumes of generated traffic. `quality` works with JPEG and WebP images.

When your input and output are both JPEGs and no destructive operations are applied, your output image quality is limited to the initial input quality: when you upload a highly compressed image, you can use the `normal` setting or go even higher, but it will not affect neither your compression settings nor file size.

* `normal` — the default setting, suits most cases.
* `better` — can be used to render relatively small and detailed previews. ≈125% file size compared to `normal`.
* `best` — useful for hi-res images, when you want to get perfect quality without paying much attention to file sizes. ≈170% file size.
* `lighter` — useful when applied to relatively large images to save traffic without significant losses in quality. ≈80% file size.
* `lightest` — useful for retina resolutions, when you don’t have to worry about the quality of each pixel. ≈50% file size.

| <p><a href="https://ucarecdn.com/c5b7dd84-c0e2-48ff-babc-d23939f2c6b4/-/preview/220x220/-/quality/best/"><img src="https://ucarecdn.com/c5b7dd84-c0e2-48ff-babc-d23939f2c6b4/-/preview/220x220/-/quality/best/" alt=""></a><br>1x <code>best</code> 16Kb<br>Blurry on retina.</p> | <p><a href="https://ucarecdn.com/c5b7dd84-c0e2-48ff-babc-d23939f2c6b4/-/preview/330x330/-/quality/lighter/"><img src="https://ucarecdn.com/c5b7dd84-c0e2-48ff-babc-d23939f2c6b4/-/preview/330x330/-/quality/lighter/" alt=""></a><br>1.5x <code>lighter</code> 16Kb<br>Fits all screens.</p> | <p><a href="https://ucarecdn.com/c5b7dd84-c0e2-48ff-babc-d23939f2c6b4/-/preview/440x440/-/quality/lightest/"><img src="https://ucarecdn.com/c5b7dd84-c0e2-48ff-babc-d23939f2c6b4/-/preview/440x440/-/quality/lightest/" alt=""></a><br>2x <code>lightest</code> 16Kb<br>Perfect for retina.</p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Implementing Multi-Scan Images, `progressive`

`-/progressive/yes/`\
`-/progressive/no/`

Returns a progressive image. In progressive images, data are compressed in multiple passes of progressively higher detail. This is ideal for large images that will be displayed while downloading over a slow connection allowing a reasonable preview after receiving only a portion of the data. The operation does not affect non-JPEG images; does not force image formats to `JPEG`.

| <p><br>Baseline loading.</p> | <p><br>Progressive loading.</p> |
| ---------------------------- | ------------------------------- |
