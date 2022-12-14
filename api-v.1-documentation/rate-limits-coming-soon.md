# Rate Limits

## Introduction

To prevent system abuse and ensure API stability, we require partners to operate within certain rate limits. Once a rate limit has been exceeded, no more requests are handled until the limit expires.&#x20;

The expected limit is 20 ARI total per minute total and broken down into 2 endpoints

**10 Restrictions & Price Requests** per minute per property

**10 Availability Requests** per minute per property

****

**Best practices to avoid rate limit:**

* Verify that your requests include the x-api-key in the headers
* Include a queuing system and batch changes
* Perform exponential backoff when a rate limit is exceeded.

{{APPLICATION_NAME}} we can handle up to 10mb per json call, so make sure you send all your data efficiently. A full sync would be 2 API calls, it should be very easy to keep each property to be a few api calls per minute for availability or rates.



### Best Practices

**1: Avoid Hitting Rate Limits**

There are several ways to avoid hitting the rate limits, the most used are queues and CRON jobs to batch updates. You can for example batch all changes and combine into 1 api call each 6 seconds

**2: Throttle Your Requests**

Within your queue system, there should be a throttling system in place to space out the number of requests. Throttling allows for more control over the number of requests that can be processed reducing the chances to hit the limit.

**3: Exponential Back-off**

Exponential backoff is an algorithm that increases the time for retrying requests based on the number of failed requests you receive due to a rate limit. If you hit any error you should pause updates for the property for 1 minute and try again, this also has benefits to recover from unexpected network or server errors also.
