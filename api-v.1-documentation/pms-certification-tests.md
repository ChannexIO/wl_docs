---
description: >-
  To complete your integration and go into our production server we would like
  you to complete a self certification test and we can check the results
---

# PMS Certification Tests

## Initial steps <a href="#initial-steps" id="initial-steps"></a>

Please use your staging account credentials to start certification scenarios.&#x20;

{% hint style="info" %}
Please let us know the email and password for your staging account in the document
{% endhint %}

### Follow Best Practices <a href="#follow-best-practices" id="follow-best-practices"></a>

We have prepared a list of best practices for integration with [{{APPLICATION_NAME}}]({{APPLICATION_DOMAIN}}) Please, read this document and follow provided scenarios at your integration.

[{{APPLICATION_NAME}} Best Practices Guide.](../guides/best-practices-guide.md)

{% hint style="success" %}
If your app doesn't support something please just mention you do not support it and move to the next test.

Example: You don't support stop sell so ignore the test for stop sell.
{% endhint %}

### Setup Mapping <a href="#setup-mapping" id="setup-mapping"></a>

For test scenario please prepare a new property for testing. This account contains:

* Property Name “Test Property - (Provider Name)”
* Test Currency: USD
* Create two Room Types
  * Twin Room - 2 Occupancy
  * Double Room - 2 Occupancy
* Create four Rate Plans combinations
  * Twin Room
    * Best Available Rate - Default rate 100
    * Bed & Breakfast Rate - Default rate 120
  * Double Room
    * Best Available Rate - Default rate 100
    * Bed & Breakfast Rate - Default Rate 120

Please, use our API to fetch ID’s for provided entities.

* Property API - [./api-v.1-documentation/hotels-collection#properties-list](./api-v.1-documentation/hotels-collection#properties-list)
* Room Type API - [./api-v.1-documentation/room-types-collection#room-types-list](./api-v.1-documentation/room-types-collection#room-types-list)
* Rate Plans API - [./api-v.1-documentation/rate-plans-collection#rate-plans-list](./api-v.1-documentation/rate-plans-collection#rate-plans-list)

Setup mapping between your system and [{{APPLICATION_NAME}}]({{APPLICATION_DOMAIN}})

{% hint style="info" %}
If possible can you show screenshot of your mapping screen to the test property.
{% endhint %}

## Prepare your tests <a href="#prepare-your-tests" id="prepare-your-tests"></a>

Please, prepare a document with the result of your tests using the following template:

> **Test Case Number**
>
> List of called API endpoints\
> UTC Timestamps for each call in test case\
> `id` value from response payload for each call in test case

If some test case is not applicable for your integration, please let us know in the document.

{% hint style="info" %}
You will receive a task ID in a successful response from {{APPLICATION_NAME}}.
{% endhint %}

## Execute test scenarios <a href="#execute-test-scenarios" id="execute-test-scenarios"></a>

### 1. Full Data Update (Full Sync) <a href="#1.-full-data-update-full-sync" id="1.-full-data-update-full-sync"></a>

We require a “Full Sync” this would simulate what happens when a Hotel goes “Live” with your integration on our Production Environment.&#x20;

{% hint style="info" %}
Full sync means you should send 500 days of Availability, rates and restrictions for all rooms and rates on the property.

We expect the full sync to be 2 API calls:

1 x 500 days for Availability (All Rooms)

1 x 500 days Rates & restrictions (All Rates)
{% endhint %}

To make sure this is correctly sent in order to certify, the data on the “Test Property” should be similar to that of a Live Hotel with different inventory/rate/restriction values for multiple days of the year. If you are unsure of how to set this up, please let us know so we can advise further.

{% hint style="info" %}
We don't want to see a full sync with all rooms with 1 availability and 100 USD as example. Better the availability and prices are different like a real hotel.
{% endhint %}

Once you have sent the “Full Sync”, please attach the returned id(s) generated by our side.

The ID you can find at the response from our side:`1 2 3 4 5 6 7 8 9 10 11` `{ "data": [ { "id": "03854d5e-5234-43e9-b673-803e91bfe640", "type": "task" } ], "meta": { "message": "Success" } }`

### 2. Single Date Update for Single Rate <a href="#2.-single-date-update-for-single-rate" id="2.-single-date-update-for-single-rate"></a>

Generate next updates for single rate:

| **Room Type** | **Rate Plan**       | **Date**         | **Value** |
| ------------- | ------------------- | ---------------- | --------- |
| Twin Room     | Best Available Rate | 22 November 2022 | 333$      |

### 3. Single Date Update for Multiple Rates <a href="#3.-single-date-update-for-multiple-rates" id="3.-single-date-update-for-multiple-rates"></a>

Generate next updates for multiple rates:

| **Room Type** | **Rate Plan**       | **Date**         | **Value** |
| ------------- | ------------------- | ---------------- | --------- |
| Twin Room     | Best Available Rate | 21 November 2022 | 333$      |
| Double Room   | Best Available Rate | 25 November 2022 | 444$      |
| Double Room   | Bed & Breakfast     | 29 November 2022 | 456.23$   |

{% hint style="info" %}
This should be 1 API call with multiple updates inside
{% endhint %}

### 4. Multiple Date Update for Multiple Rates <a href="#4.-multiple-date-update-for-multiple-rates" id="4.-multiple-date-update-for-multiple-rates"></a>

Generate next updates for multiple rates:

| **Room Type** | **Rate Plan**       | **Date**                             | **Value** |
| ------------- | ------------------- | ------------------------------------ | --------- |
| Twin Room     | Best Available Rate | 01 November 2022 to 10 November 2022 | 241$      |
| Double Room   | Best Available Rate | 10 November 2022 to 16 November 2022 | 312.66$   |
| Double Room   | Bed & Breakfast     | 01 November 2022 to 20 November 2022 | 111$      |

{% hint style="info" %}
This should be 1 API call with multiple details inside
{% endhint %}

### 5. Min Stay Update <a href="#5.-min-stay-update" id="5.-min-stay-update"></a>

Generate next updates for multiple rates:

| **Room Type** | **Rate Plan**       | **Date**         | **Min Stay Value** |
| ------------- | ------------------- | ---------------- | ------------------ |
| Twin Room     | Best Available Rate | 23 November 2022 | 3                  |
| Double Room   | Best Available Rate | 25 November 2022 | 2                  |
| Double Room   | Bed & Breakfast     | 15 November 2022 | 5                  |

{% hint style="info" %}
This should be 1 API call
{% endhint %}

### 6. Stop Sell Update <a href="#6.-stop-sell-update" id="6.-stop-sell-update"></a>

Generate the next updates to enable StopSell for multiple rates:

| **Room Type** | **Rate Plan**       | **Date**         | **Stop Sell** |
| ------------- | ------------------- | ---------------- | ------------- |
| Twin Room     | Best Available Rate | 14 November 2022 | true          |
| Double Room   | Best Available Rate | 16 November 2022 | true          |
| Double Room   | Bed & Breakfast     | 20 November 2022 | true          |

{% hint style="info" %}
This should be 1 API call
{% endhint %}

### 7. Multiple Restrictions Update <a href="#7.-multiple-restrictions-update" id="7.-multiple-restrictions-update"></a>

Generate next updates for multiple rates:

| **Room Type** | **Rate Plan**       | **Date**                             | **Restrictions**                                                                              |
| ------------- | ------------------- | ------------------------------------ | --------------------------------------------------------------------------------------------- |
| Twin Room     | Best Available Rate | 01 November 2022 to 10 November 2022 | <p>closed_to_arrival: true,<br>closed_to_departure: false,<br>max_stay: 4,<br>min_stay: 1</p> |
| Twin Room     | Bed & Breakfast     | 12 November 2022 to 16 November 2022 | <p>closed_to_arrival: false,<br>closed_to_departure: true,<br>min_stay: 6</p>                 |
| Double Room   | Best Available Rate | 10 November 2022 to 16 November 2022 | <p>closed_to_arrival: true,<br>min_stay: 2</p>                                                |
| Double Room   | Bed & Breakfast     | 01 November 2022 to 20 November 2022 | min\_stay: 10                                                                                 |

{% hint style="info" %}
This should be 1 API call
{% endhint %}

### 8. Half-year Update <a href="#8.-half-year-update" id="8.-half-year-update"></a>

Generate next updates for half-year period:

| **Room Type** | **Rate Plan**       | **Date**                        | **Restrictions**                                                                             |
| ------------- | ------------------- | ------------------------------- | -------------------------------------------------------------------------------------------- |
| Twin Room     | Best Available Rate | 01 December 2022 to 01 May 2023 | <p>rate: 432$<br>closed_to_arrival: false,<br>closed_to_departure: false,<br>min_stay: 2</p> |
| Double Room   | Best Available Rate | 01 December 2022 to 01 May 2023 | <p>rate: 342$<br>min_stay: 3</p>                                                             |

{% hint style="info" %}
This should be 1 API call
{% endhint %}

### 9. Single Date Availability Update <a href="#9.-single-date-availability-update" id="9.-single-date-availability-update"></a>

Generate next updates for availability:

| **Room Type** | **Date**         | **Value** |
| ------------- | ---------------- | --------- |
| Twin Room     | 21 November 2022 | 7         |
| Double Room   | 25 November 2022 | 0         |

{% hint style="info" %}
This should be 1 API call
{% endhint %}

### 10. Multiple Date Availability Update <a href="#10.-multiple-date-availability-update" id="10.-multiple-date-availability-update"></a>

Generate next updates for availability:

| **Room Type** | **Date**                            | **Value** |
| ------------- | ----------------------------------- | --------- |
| Twin Room     | 10 November 2022 - 16 November 2022 | 3         |
| Double Room   | 17 November 2022 - 24 November 2022 | 4         |

{% hint style="info" %}
This should be 1 API call
{% endhint %}

### 11. Booking receiving <a href="#11.-booking-receiving" id="11.-booking-receiving"></a>

Using our Booking API ([./api-v.1-documentation/bookings-collection#create-booking](./api-v.1-documentation/bookings-collection#create-booking)) create booking at {{APPLICATION_NAME}} and receive it at your Property Management system.

For both you need to make a “Booking Engine” channel active on a property.

1. Get booking by using the feed API: [./api-v.1-documentation/bookings-collection#booking-revisions-feed](./api-v.1-documentation/bookings-collection#booking-revisions-feed)
2. Make sure the booking is "acknowledged" by using the ack API: [./api-v.1-documentation/bookings-collection#acknowledge-booking-revision-receiving](./api-v.1-documentation/bookings-collection#acknowledge-booking-revision-receiving)
3. Provide us an image about the created booking of how it looks in your system.

### 12. Use API Key Method

Can you confirm that you use our API Key method: [./api-v.1-documentation/api-key-access](./api-v.1-documentation/api-key-access)

Sign in with email/pass will be discontinued and will be protected by 2-factor authentication.

### 13. Rate Limits

Please look at proposed rate limits and make sure you have a queue or limiter to not spam our API endpoints. Let us know you can work with these limits

[./api-v.1-documentation/rate-limits-coming-soon](./api-v.1-documentation/rate-limits-coming-soon)

Can you stay in rate limits?

### 14. Update Logic

We will not accept any logic that just sends full sync on a timer basis

Example: Each 5 mins you send full update of availability for all rooms for 2 years.

We require partners to only send changes to availability and prices.

Full sync is allowed once every 24h if required but please schedule this on off peak hours and try to give some seconds between each property updates if you have a lot of properties to full sync.

Do you agree to only send updated changes to {{APPLICATION_NAME}}?

### 13. Extra Notes

* Do you support both Min Stay Through and Arrival? If only one please specify which
* Do you not support the following restrictions? Stop Sell, CTA, CTD etc. Let us know if you don't support any
* Do you support multiple room types and multiple rate plans per room type?
* Do you need credit card details with bookings?
* Are you PCI Certified or use a PCI service like {{APPLICATION_NAME}} PCI or PCI Booking/Tokenex?

## Collect results <a href="#collect-results" id="collect-results"></a>

When you finish your tests, collect all information together in a word or excel doc and provide it to us via email: [{{SUPPORT_EMAIL}}](mailto:{{SUPPORT_EMAIL}})

In some cases we may require you to update your integration to be more efficient, if all is ok the certification should be successful and we will get back to you with the next steps for production server.