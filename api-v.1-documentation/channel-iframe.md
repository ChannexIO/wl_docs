---
description: >-
  This is the API to show an iframe for the {{APPLICATION_NAME}} mapping screen in your
  application. The user will be able to create channels and map by themselves.
---

# Channel IFrame

## Generate a One-Time access token

To generate a One-Time access token you should call the next API Method:

```
POST {{server}}/api/v1/auth/one_time_token
```

{% hint style="info" %}
Please, don’t forget to add your Authorisation header.
{% endhint %}

You should get the following response:

```
{
"data": {
"token": "94feab9f-60e6-411b-d854-8f12004d8bc8"
},
"meta": {
"message": "You are successfully received one-time token! Use it for exchange to JWT"
}
}
```

At data.token you should receive a One-Time access token to authorise your user in {{APPLICATION_NAME}} without providing credentials.&#x20;

After the first usage, token will be removed. The token will live for 15 minutes. Once iframe loaded it will not have an expiry time.

The user will be authenticated under the same user, who requested the Access Token!

## Generate the Iframe Code

The next step is to generate the iframe to show to your user

```
<iframe
src="{{server}}/auth/exchange?oauth_session_key={{ONE_TIME_ACCESS_TOKEN}}&app_mo
de=headless&redirect_to=/channels&property_id={{PROPERTY_ID}}"
>
</iframe>
```

\
Where {{server}} is the address of the {{APPLICATION_NAME}} server, {{ONE\_TIME\_ACCESS\_TOKEN}} is the token received at the previous step, {{PROPERTY\_ID}} is the ID of the Property in {{APPLICATION_NAME}} which will be associated with the created channels.

### Filter available channels

To allow user connect only specific channels, you can pass additional argument `channels` inside URL:

```
{{server}}/auth/exchange?oauth_session_key={{ONE_TIME_ACCESS_TOKEN}}&app_mo
de=headless&redirect_to=/channels&property_id={{PROPERTY_ID}}&channels=BDC,ABB
```

Available list of channels:

| Code | Channel              |
| ---- | -------------------- |
| ABB  | Airbnb               |
| ACO  | Abode Connect        |
| ADO  | Advertising Online   |
| AGO  | Agoda                |
| BDC  | Booking.com          |
| CTZ  | CultBooking          |
| DDC  | Despegar             |
| EXP  | Expedia              |
| GDS  | OpenGDS              |
| GHA  | Google Hotel         |
| GIT  | Go4IT                |
| HBD  | Hotelbeds            |
| HG   | HyperGuest           |
| HIC  | Hey Iceland          |
| HWL  | Hostelworld          |
| LO   | LocalOTA             |
| MMB  | MemberButton         |
| OC   | Open Channel         |
| OSA  | Instant Booking Page |
| RC   | Room Cloud           |
| RSR  | Reserva              |
| TSQ  | The Square           |
| VB   | VerticalBooking      |
| WBK  | Wubook               |
| WBR  | Webrooms             |

## Notes

The provided iframe UI is limited and only allows the user to create / edit / remove channels with the provided Property ID.

There is no access policy yet for example “Read-Only” mode. Let us know your ideas or requirements if that is needed.

## Other Pages from {{APPLICATION_NAME}}

This API will allow you to iframe any page from {{APPLICATION_NAME}}, you will need to edit the redirect. All pages will generally work with property ID option.

Example to get messages page:

```
{{server}}/auth/exchange?oauth_session_key={{ONE_TIME_ACCESS_TOKEN}}&app_mo
de=headless&redirect_to=/messages&property_id={{PROPERTY_ID}}
```

