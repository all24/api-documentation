---
title: all24 API Reference

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
$ curl https://api.all24.com/v1/tracking \
  -u api_key:

```

> Make sure to replace `api_key` with your API key.<br> curl uses the -u flag to pass basic auth credentials (adding a colon after your API key prevents cURL from asking for a password).


Authentication to the API is performed via HTTP [Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). Provide your API key as the basic auth username value. You do not need to provide a password.

all24 uses API keys to allow access to the API. You can register a new API key by contacting your Account Manager.

<aside class="notice">
You must replace <code>api_key</code> with your personal API key.
<br>
All API requests must be made over <a href="https://en.wikipedia.org/wiki/HTTPS" target="_blank">HTTPS</a>. Calls made over plain HTTP will fail. API requests without authentication will also fail.
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
 409 - Conflict | The request conflicts with another request.
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

Retrieves all the locations with timestamp that this order went through.

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
Location|{ "location_name": "","service_area": "", "postcode": "" }| Location object
Event Location|{ "location_name": "","service_area": "", "postcode": "" }| Event Location object

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

Retrieves all the locations with timestamp that this order went through.

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
Location|{ "location_name": "","service_area": "", "postcode": "" }| Location object
Event Location|{ "location_name": "","service_area": "", "postcode": "" }| Event Location object

# Shipping

## Delivery Order

```shell
#Request:
$ curl https://api.all24.com/v1/shipping
  -u api_key: \
  -d hs_meta[schema-type]=hs-delivery-order, \
  -d hs_meta[schema-version]=1.0, \
  -d hs_delivery_order[origin_order_date]="2015-08-18T11:12:00+07:00", \
  -d hs_delivery_order[shipper]="HSLZTH", \
  -d hs_delivery_order[shipper_order_reference]="FRETEST003", \
  -d hs_delivery_order[shipment_value]="1234.00", \
  -d hs_delivery_order[remarks]="", \
  -d hs_delivery_order[service_requirements]={ \
  -d hs_delivery_order[service_contract_reference]="HSLZTHND", \
  -d hs_delivery_order[preferred_delivery_group]="", \
  -d hs_delivery_order[is_pay_on_pickup]="no", \
  -d hs_delivery_order[is_cash_on_delivery]="yes", \
  -d hs_delivery_order[expected_completion]="2015-08-19T11:12:00+07:00", \
  -d hs_delivery_order[extended]="", \
  -d hs_delivery_order[goods][quantity]="", \
  -d hs_delivery_order[goods][items][description]="Product", \
  -d hs_delivery_order[goods][items][quantity]="1", \
  -d hs_delivery_order[goods][items][value]="420.00", \
  -d hs_delivery_order[goods][items][length]="1", \
  -d hs_delivery_order[goods][items][width]="1", \
  -d hs_delivery_order[goods][items][height]="1", \
  -d hs_delivery_order[goods][items][weight]="1" \
  -d hs_delivery_order[goods][remarks_on_goods]="Remarks on goods" \
  -d hs_delivery_order[payment_requirements][pop_amount]="", \
  -d hs_delivery_order[payment_requirements][cod_amount]="420.00", \
  -d hs_delivery_order[payment_requirements][charge_items][charge_code]="total", \
  -d hs_delivery_order[payment_requirements][charge_items][display_text]="Total", \
  -d hs_delivery_order[payment_requirements][charge_items][display_sequence]="0", \
  -d hs_delivery_order[payment_requirements][charge_items][amount]="420.00", \
  -d hs_delivery_order[sender_location][location_ref]="abc123", \
  -d hs_delivery_order[sender_location][zone]="Chatuchak", \
  -d hs_delivery_order[sender_location][company]="Fashion F200", \
  -d hs_delivery_order[sender_location][building]="J.J. Mall", \
  -d hs_delivery_order[sender_location][room]="F 39-40", \
  -d hs_delivery_order[sender_location][floor]="1", \
  -d hs_delivery_order[sender_location][addressline1]="588 Kampaeng Phet 2 Rd", \
  -d hs_delivery_order[sender_location][addressline2]="", \
  -d hs_delivery_order[sender_location][addressline3]="", \
  -d hs_delivery_order[sender_location][addressline4]="", \
  -d hs_delivery_order[sender_location][city]="Chatuchak", \
  -d hs_delivery_order[sender_location][province]="Bangkok", \
  -d hs_delivery_order[sender_location][postcode]="10900", \
  -d hs_delivery_order[sender_location][country]="", \
  -d hs_delivery_order[sender_location][notes_for_driver]="Directions for driver",
  -d hs_delivery_order[sender_location][name]="F200 staff", \
  -d hs_delivery_order[sender_location][phone]="089-4985098", \
  -d hs_delivery_order[sender_location][email]="", \
  -d hs_delivery_order[sender_location][remarks]="remarks on F200", \
  -d hs_delivery_order[recipient_location][location_ref]="w0szv4da", \
  -d hs_delivery_order[recipient_location][zone]="Sathorn", \
  -d hs_delivery_order[recipient_location][company]="", \
  -d hs_delivery_order[recipient_location][building]="The Met Condo", \
  -d hs_delivery_order[recipient_location][room]="123/322", \
  -d hs_delivery_order[recipient_location][floor]="", \
  -d hs_delivery_order[recipient_location][addressline1]="South Sathorn road", \
  -d hs_delivery_order[recipient_location][addressline2]="", \
  -d hs_delivery_order[recipient_location][addressline3]="", \
  -d hs_delivery_order[recipient_location][addressline4]="", \
  -d hs_delivery_order[recipient_location][city]="", \
  -d hs_delivery_order[recipient_location][province]="Bangkok", \
  -d hs_delivery_order[recipient_location][postcode]="10110", \
  -d hs_delivery_order[recipient_location][country]="", \
  -d hs_delivery_order[recipient_location][notes_for_driver]="past sathorn soi 5", \
  -d hs_delivery_order[recipient_location][name]="Giorgio F", \
  -d hs_delivery_order[recipient_location][phone]="0990912768", \
  -d hs_delivery_order[recipient_location][email]="", \
  -d hs_delivery_order[recipient_location][remarks]="Giorgio F is from italy", \
  -d hs_delivery_order[return_location][location_ref]="", \
  -d hs_delivery_order[return_location][zone]="", \
  -d hs_delivery_order[return_location][company]="", \
  -d hs_delivery_order[return_location][building]="", \
  -d hs_delivery_order[return_location][room]="", \
  -d hs_delivery_order[return_location][floor]="", \
  -d hs_delivery_order[return_location][addressline1]="", \
  -d hs_delivery_order[return_location][addressline2]="", \
  -d hs_delivery_order[return_location][addressline3]="", \
  -d hs_delivery_order[return_location][addressline4]="", \
  -d hs_delivery_order[return_location][city]="", \
  -d hs_delivery_order[return_location][province]="", \
  -d hs_delivery_order[return_location][postcode]="", \
  -d hs_delivery_order[return_location][country]="", \
  -d hs_delivery_order[return_location][notes_for_driver]="", \
  -d hs_delivery_order[return_location][name]="", \
  -d hs_delivery_order[return_location][phone]="", \
  -d hs_delivery_order[return_location][email]="", \
  -d hs_delivery_order[return_location][remarks]=""
```

Delivery order instruction to all24.
This typically comes from ecommerce platform (Sales Order) or a warehouse management system (Delivery Order).

### HTTP Request

`POST https://api.all24.com/v1/tracking/shipping?hs_meta[schema-type]&...`

### Query Parameters

Parameter|Type|Specification
---------|----|-------------
hs_meta|HSMeta|mandatory
hs_meta[schema-type]|string|mandatory
hs_meta[schema-version]|string|mandatory
hs_delivery_order|HSDeliveryOrder|mandatory
hs_delivery_order[origin_order_date]|string|mandatory
hs_delivery_order[shipper]|string|mandatory
hs_delivery_order[shipper_order_reference]|string|mandatory
hs_delivery_order[shipment_value]|string|optional
hs_delivery_order[remarks]|string|optional
hs_delivery_order[service_requirements]|ServiceRequirements|mandatory
hs_delivery_order[service_requirements][service_contract_reference]|string|mandatory
hs_delivery_order[service_requirements][service_contract_reference]|string|mandatory
hs_delivery_order[service_requirements][preferred_delivery_group]|string|optional
hs_delivery_order[service_requirements][is_pay_on_pickup]|string|optional
hs_delivery_order[service_requirements][is_cash_on_delivery]|string|optional
hs_delivery_order[service_requirements][expected_completion]|string|optional
hs_delivery_order[service_requirements][extended]|Extended Service Req.|optional
hs_delivery_order[goods]|Goods|optional
hs_delivery_order[goods][quantity]|string|optional
hs_delivery_order[goods][items]|Item|optional
hs_delivery_order[goods][items][description]|string|optional
hs_delivery_order[goods][items][quantity]|string|optional
hs_delivery_order[goods][items][value]|string|optional
hs_delivery_order[goods][items][length]|string|optional
hs_delivery_order[goods][items][width]|string|optional
hs_delivery_order[goods][items][height]|string|optional
hs_delivery_order[goods][items][weight]|string|optional
hs_delivery_order[goods][remarks_on_goods]|string|optional
hs_delivery_order[payment_requirements]|PaymentRequirements|Conditional
hs_delivery_order[payment_requirements][pop_amount]|string|Conditional
hs_delivery_order[payment_requirements][cod_amount]|string|Conditional
hs_delivery_order[payment_requirements][charge_items][charge_code]|string|optional
hs_delivery_order[payment_requirements][charge_items][display_text]|string|optional
hs_delivery_order[payment_requirements][charge_items][display_sequence]|string|optional
hs_delivery_order[payment_requirements][charge_items][amount]|string|optional
hs_delivery_order[sender_location]|Location|mandantory
hs_delivery_order[sender_location][location_ref]|string|optional
hs_delivery_order[sender_location][zone]|string|optional
hs_delivery_order[sender_location][company]|string|optional
hs_delivery_order[sender_location][building]|string|optional
hs_delivery_order[sender_location][room]|string|optional
hs_delivery_order[sender_location][floor]|string|optional
hs_delivery_order[sender_location][addressline1]|string|mandantory
hs_delivery_order[sender_location][addressline2]|string|optional
hs_delivery_order[sender_location][addressline3]|string|optional
hs_delivery_order[sender_location][addressline4]|string|optional
hs_delivery_order[sender_location][city]|string|optional
hs_delivery_order[sender_location][province]|string|optional
hs_delivery_order[sender_location][postcode]|string|mandantory
hs_delivery_order[sender_location][country]|string|optional
hs_delivery_order[sender_location][notes_for_driver]|string|optional
hs_delivery_order[sender_location][name]|string|mandantory
hs_delivery_order[sender_location][phone]|string|mandantory
hs_delivery_order[sender_location][email]|string|optional
hs_delivery_order[sender_location][remarks]|string|optional
hs_delivery_order[recipient_location]|Location|mandantory
hs_delivery_order[recipient_location][location_ref]|string|optional
hs_delivery_order[recipient_location][zone]|string|optional
hs_delivery_order[recipient_location][company]|string|optional
hs_delivery_order[recipient_location][building]|string|optional
hs_delivery_order[recipient_location][room]|string|optional
hs_delivery_order[recipient_location][floor]|string|optional
hs_delivery_order[recipient_location][addressline1]|string|mandantory
hs_delivery_order[recipient_location][addressline2]|string|optional
hs_delivery_order[recipient_location][addressline3]|string|optional
hs_delivery_order[recipient_location][addressline4]|string|optional
hs_delivery_order[recipient_location][city]|string|optional
hs_delivery_order[recipient_location][province]|string|optional
hs_delivery_order[recipient_location][postcode]|string|mandantory
hs_delivery_order[recipient_location][country]|string|optional
hs_delivery_order[recipient_location][notes_for_driver]|string|optional
hs_delivery_order[recipient_location][name]|string|mandantory
hs_delivery_order[recipient_location][phone]|string|mandantory
hs_delivery_order[recipient_location][email]|string|optional
hs_delivery_order[recipient_location][remarks]|string|optional
hs_delivery_order[return_location]|Location|optional
hs_delivery_order[return_location][location_ref]|string|optional
hs_delivery_order[return_location][zone]|string|optional
hs_delivery_order[return_location][company]|string|optional
hs_delivery_order[return_location][building]|string|optional
hs_delivery_order[return_location][room]|string|optional
hs_delivery_order[return_location][floor]|string|optional
hs_delivery_order[return_location][addressline1]|string|mandantory
hs_delivery_order[return_location][addressline2]|string|optional
hs_delivery_order[return_location][addressline3]|string|optional
hs_delivery_order[return_location][addressline4]|string|optional
hs_delivery_order[return_location][city]|string|optional
hs_delivery_order[return_location][province]|string|optional
hs_delivery_order[return_location][postcode]|string|mandantory
hs_delivery_order[return_location][country]|string|optional
hs_delivery_order[return_location][notes_for_driver]|string|optional
hs_delivery_order[return_location][name]|string|mandantory
hs_delivery_order[return_location][phone]|string|mandantory
hs_delivery_order[return_location][email]|string|optional
hs_delivery_order[return_location][remarks]|string|optional

### Response

Attribute|Type
---------|----
response_code|string
error_codes|Array
hs_order_ref|string


### Response Type

Type|Value|Specification
----|-----|-------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1100|Invalid Customer ref.
error_codes|1300|Unkown Customer ref.
error_codes|1150|Invalid Order ref id.
error_codes|1200|Service Code missing
error_codes|2000|Failed to Create Order
error_codes|2100|Duplicate Customer Order
