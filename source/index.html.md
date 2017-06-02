---
title: API Reference

language_tabs:
  - shell: curl

toc_footers:

search: true
---

# Introduction

The all24 API is organized around [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. [JSON](http://www.json.org/) is returned by all API responses, including errors.

# Authentication

```plaintext
Example request:
```
```shell
$ curl https://api.all24.com/vi/tracking \
  -u api_key:

```

> Make sure to replace `api_key` with your API key.<br> curl uses the -u flag to pass basic auth credentials (adding a colon after your API key prevents cURL from asking for a password).


Authentication to the API is performed via HTTP [Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). Provide your API key as the basic auth username value. You do not need to provide a password.

all24 uses API keys to allow access to the API. You can register a new API key by mailing us at <a href="mailto:api@all24.com">api@all24.com</a>.

<aside class="notice">
You must replace <code>api_key</code> with your personal API key.
<br>
All API requests must be made over [HTTPS](https://en.wikipedia.org/wiki/HTTPS). Calls made over plain HTTP will fail. API requests without authentication will also fail.
</aside>

# Errors

all24 uses conventional HTTP response codes to indicate the success or failure of an API request. In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, etc.), and codes in the 5xx range indicate an error with all24's servers.

 HTTP status code | Description
 -----------------|-------
 200 - OK | Everything worked as expected.
 400 - Bad Request | The request was unacceptable, often due to missing a required parameter.
 401 - Unauthorized | No valid API key provided.
 402 - Request Faile | The parameters were valid but the request failed.
 404 - Not Found | The requested resource doesn't exist.
 409 - Conflict | The request conflicts with another request (perhaps due to using the same idempotent key).
 429 - Too Many Requests | Too many requests hit the API too quickly. We recommend an exponential backoff of your requests.
 500, 502, 503, 504 - Server Errors | Something went wrong on all24's end.

# Order Tracking

## all24 Tracking Number

```shell
#Request:
$ curl https://api.all24.com/v1/tracking/orders
  -u api_key: \
  -d all24_tracking_number=HSBY37H7
  
```

```json
#Response:
{
  "HSBY37H7": {
    "origin_order_date": "2015-08-18T11:12:00+07:00",
    "order_ref_id": "123abc1",
    "hs_order_ref": "HSBY37H7",
    "expected_completion": "2015-08-19T23:59:00+07:00",
    "sender_location": {
      "location_name": "Fashion F200",
      "addressline1": "F 39-40, J.J. Mall, 1, 588 Kampaeng Phet 2 Rd",
      "service_area": "Chatuchak",
      "postcode": "10900"
    },
    "recipient_location": {
      "location_name": "",
      "addressline1": "123/322, The Met Condo, South Sathorn road",
      "service_area": "",
      "postcode": ""
    },
    "history": [
    {
      "event_time": "2015-08-18 12:02:00",
      "event_name": "Delivery complete",
      "event_code": "500000",
      "location_ref": "w0szv4da",
      "location": {
        "location_name": "",
        "service_area": "",
        "postcode": ""
      }
    },
    {
      "event_time": "2015-08-18 12:02:00",
      "event_name": "Goods arrived",
      "event_code": "300000",
      "location_ref": "w0szv4da",
      "location": {
        "location_name": "",
        "service_area": "",
        "postcode": ""
      }
    },
    {
      "event_time": "2015-08-18 12:01:46",
      "event_name": "Picked up",
      "event_code": "200000",
      "location_ref": "",
      "location": {
        "location_name": "On vehicle",
        "service_area": "Bangkok",
        "postcode": ""
      }
    },
    {
      "event_time": "2015-08-18 12:01:30",
      "event_name": "Goods received",
      "event_code": "100000",
      "location_ref": "",
      "location": {
        "location_name": "Fastrak Service Limited",
        "service_area": "Bangkok",
        "postcode": "10110"
      }
    },
    {
      "event_time": "2015-08-18 12:00:59",
      "event_name": "Order received",
      "event_code": "050000",
      "location_ref": "abc123",
      "location": {
        "location_name": "Fashion F200",
        "service_area": "Chatuchak",
        "postcode": "10900"
      }
    }
    ]
  },
  "response_code": 200,
  "error_codes": []
}
```

Retrieves the all the location with timestamp that this order went through.

### HTTP Request

`GET https://api.all24.com/v1/tracking/orders?all24_tracking_number=HSBY37H7`

### Query Parameters

Parameter | Description
--------- | -----------
all_24_tracking_number | all24 tracking number

### Response

Property | Type
---------|-----
origin_order_date|string
order_ref_id|string
hs_order_ref|string
expected_completion|string
sender_location|Location
location_name|string
addressline1|string
service_area|string
postcode|string
recipient_location|Location
location_name|string
addressline1|string
service_area|string
postcode|string
history|Array
event_time|string
event_name|string
event_code|string
location_ref|string
location|Event Location
location_name|string
service_area|string
postcode|string
response_code|string
error_codes|Array
hs_order_ref|string

### Response Type

Type | Value | Specification
-----|-------|--------------
event_code|500000|Order received
event_code|100000|Arrived at location
event_code|300000|Delivered
event_code|401100|Order cancelled (by customer)
event_code|401101|Order cancelled (by carrier)
event_code|200000|Picked up
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1350|Order Not found

## Third Party Tracking Number

```shell
#Request:
$ curl https://api.all24.com/v1/tracking/orders
  -u api_key: \
  -d customer_id=12345 \
  -d tracking_number=HSBY37H7
```

```json
#Response:
{
  "HSBY37H7": {
    "origin_order_date": "2015-08-18T11:12:00+07:00",
    "order_ref_id": "123abc1",
    "hs_order_ref": "HSBY37H7",
    "expected_completion": "2015-08-19T23:59:00+07:00",
    "sender_location": {
      "location_name": "Fashion F200",
      "addressline1": "F 39-40, J.J. Mall, 1, 588 Kampaeng Phet 2 Rd",
      "service_area": "Chatuchak",
      "postcode": "10900"
    },
    "recipient_location": {
      "location_name": "",
      "addressline1": "123/322, The Met Condo, South Sathorn road",
      "service_area": "",
      "postcode": ""
    },
    "history": [
    {
      "event_time": "2015-08-18 12:02:00",
      "event_name": "Delivery complete",
      "event_code": "500000",
      "location_ref": "w0szv4da",
      "location": {
        "location_name": "",
        "service_area": "",
        "postcode": ""
      }
    },
    {
      "event_time": "2015-08-18 12:02:00",
      "event_name": "Goods arrived",
      "event_code": "300000",
      "location_ref": "w0szv4da",
      "location": {
        "location_name": "",
        "service_area": "",
        "postcode": ""
      }
    },
    {
      "event_time": "2015-08-18 12:01:46",
      "event_name": "Picked up",
      "event_code": "200000",
      "location_ref": "",
      "location": {
        "location_name": "On vehicle",
        "service_area": "Bangkok",
        "postcode": ""
      }
    },
    {
      "event_time": "2015-08-18 12:01:30",
      "event_name": "Goods received",
      "event_code": "100000",
      "location_ref": "",
      "location": {
        "location_name": "Fastrak Service Limited",
        "service_area": "Bangkok",
        "postcode": "10110"
      }
    },
    {
      "event_time": "2015-08-18 12:00:59",
      "event_name": "Order received",
      "event_code": "050000",
      "location_ref": "abc123",
      "location": {
        "location_name": "Fashion F200",
        "service_area": "Chatuchak",
        "postcode": "10900"
      }
    }
    ]
  },
  "response_code": 200,
  "error_codes": []
}
```

Retrieves the all the location with timestamp that this order went through.

### HTTP Request

`GET https://api.all24.com/v1/tracking/orders?tracking_number=HSBY37H7&customer_id=12345`

### Query Parameters

Parameter | Description
--------- | -----------
tracking_number | This is the tracking number of the third party
customer_id | This is the customer of the order tracked (typically the seller)

### Response

Property | Type
---------|-----
origin_order_date|string
order_ref_id|string
hs_order_ref|string
expected_completion|string
sender_location|Location
location_name|string
addressline1|string
service_area|string
postcode|string
recipient_location|Location
location_name|string
addressline1|string
service_area|string
postcode|string
history|Array
event_time|string
event_name|string
event_code|string
location_ref|string
location|Event Location
location_name|string
service_area|string
postcode|string
response_code|string
error_codes|Array
hs_order_ref|string

### Response Type

Type | Value | Specification
-----|-------|--------------
event_code|500000|Order received
event_code|100000|Arrived at location
event_code|300000|Delivered
event_code|401100|Order cancelled (by customer)
event_code|401101|Order cancelled (by carrier)
event_code|200000|Picked up
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1350|Order Not found

# Order Tracking
