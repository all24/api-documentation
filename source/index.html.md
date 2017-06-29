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

# Tracking

## Order Tracking

```shell
#Request:
$ curl https://api.all24.com/rest/v2/tracking/orders
  -u api_key: \
  -d shipper_order_ref=XYZ20170623000001
```

```json
#Response:
{
  "shipper_order_reference": "XYZ20170623000001",
  "hs_shipping_order_ref": "HSSA3DS5RE32F4E2E54R",
  "recipient_location": {
    "shipper_location_ref": "",
    "hs_location_ref": "cj3xro67v9991b5mi3y2ldade",
    "location_name": "",
    "addressline1": "Rama 4 Road",
    "city": "Klong Thoey",
    "province": "Bangkok",
    "postcode": "10110",
    "country": "Thailand"
  },
  "delivery_orders": [
    {
      "hs_delivery_order_ref": "HSR56HG5",
      "shipment_items": [
        "XYZI1234567890",
        "XYZI1234567891"
      ],
      "sender_location": {
        "shipper_location_ref": "",
        "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
        "location_name": "All 24 Crossdock",
        "addressline1": "some road, some soi",
        "city": "Klong Thoey",
        "province": "Bangkok",
        "postcode": "10110",
        "country": "Thailand"
      },
      "delivery_location": {
        "shipper_location_ref": "",
        "hs_location_ref": "cj3xro67v9991b5mi3y2ldade",
        "location_name": "",
        "addressline1": "Rama 4 Road",
        "city": "Klong Thoey",
        "province": "Bangkok",
        "postcode": "10110",
        "country": "Thailand"
      },
      "history": [
        {
          "event_time": "2015-08-18T12:02:01+07:00",
          "event_name": "Delivery complete",
          "event_code": "500000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v9991b5mi3y2ldade",
            "location_name": "",
            "addressline1": "Rama 4 Road",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-18T12:02:00+07:00",
          "event_name": "Goods arrived",
          "event_code": "300000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v9991b5mi3y2ldade",
            "location_name": "",
            "addressline1": "Rama 4 Road",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-18T12:01:48+07:00",
          "event_name": "Picked up",
          "event_code": "200000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v0001b5mi3y2ldwnj",
            "location_name": "On vehicle",
            "addressline1": "",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-18T12:01:47+07:00",
          "event_name": "Goods arrived",
          "event_code": "300000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v2221b5mi3y2ldabc",
            "location_name": "Fastrak Klong Thoey Last Mile Hub",
            "addressline1": "some road, some soi",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-18T12:01:46+07:00",
          "event_name": "Picked up",
          "event_code": "200000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v0001b5mi3y2ldwnj",
            "location_name": "On vehicle",
            "addressline1": "",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-18T12:00:59+07:00",
          "event_name": "Order received",
          "event_code": "050000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
            "location_name": "All 24 Crossdock",
            "addressline1": "some road, some soi",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        }
      ]
    },
    {
      "hs_delivery_order_ref": "HSR56FDS",
      "shipment_items": [
        "XYZI1234567890"
      ],
      "sender_location": {
        "shipper_location_ref": "XYZL87654321THSAMUT",
        "hs_location_ref": "cim1iyga90001iumi7j1mko93",
        "location_name": "Merchant Warehouse A",
        "addressline1": "M1 Some road",
        "city": "Bangpla",
        "province": "Samutprakan",
        "postcode": "10540",
        "country": "Thailand"
      },
      "delivery_location": {
        "shipper_location_ref": "",
        "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
        "location_name": "All 24 Crossdock",
        "addressline1": "some road, some soi",
        "city": "Klong Thoey",
        "province": "Bangkok",
        "postcode": "10110",
        "country": "Thailand"
      },
      "history": [
        {
          "event_time": "2015-08-17T12:02:00+07:00",
          "event_name": "Delivery complete",
          "event_code": "500000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
            "location_name": "All 24 Crossdock",
            "addressline1": "some road, some soi",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-17T12:02:00+07:00",
          "event_name": "Goods arrived",
          "event_code": "300000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
            "location_name": "All 24 Crossdock",
            "addressline1": "some road, some soi",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-17T12:01:46+07:00",
          "event_name": "Picked up",
          "event_code": "200000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v0001b5mi3y2ldwnj",
            "location_name": "On vehicle",
            "addressline1": "",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-17T12:00:59+07:00",
          "event_name": "Order received",
          "event_code": "050000",
          "location": {
            "shipper_location_ref": "XYZL87654321THSAMUT",
            "hs_location_ref": "cim1iyga90001iumi7j1mko93",
            "location_name": "Merchant Warehouse A",
            "addressline1": "M1 Some road",
            "city": "Bangpla",
            "province": "Samutprakan",
            "postcode": "10540",
            "country": "Thailand"
          }
        }
      ]
    },
    {
      "hs_delivery_order_ref": "HSR56FDS",
      "shipment_items": [
        "XYZI1234567891"
      ],
      "sender_location": {
        "shipper_location_ref": "XYZL1234568THSAMUT",
        "hs_location_ref": "cim1if4pu0001fjmiyl0bu90f",
        "location_name": "Merchant Warehouse B",
        "addressline1": "M9 Some road",
        "city": "Bangpla",
        "province": "Samutprakan",
        "postcode": "10540",
        "country": "Thailand"
      },
      "delivery_location": {
        "shipper_location_ref": "",
        "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
        "location_name": "All 24 Crossdock",
        "addressline1": "some road, some soi",
        "city": "Klong Thoey",
        "province": "Bangkok",
        "postcode": "10110",
        "country": "Thailand"
      },
      "history": [
        {
          "event_time": "2015-08-17T12:02:00+07:00",
          "event_name": "Delivery complete",
          "event_code": "500000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
            "location_name": "All 24 Crossdock",
            "addressline1": "some road, some soi",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-17T12:02:00+07:00",
          "event_name": "Goods arrived",
          "event_code": "300000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro33v0001b5mi3y2ldzzz",
            "location_name": "All 24 Crossdock",
            "addressline1": "some road, some soi",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-17T12:01:46+07:00",
          "event_name": "Picked up",
          "event_code": "200000",
          "location": {
            "shipper_location_ref": "",
            "hs_location_ref": "cj3xro67v0001b5mi3y2ldwnj",
            "location_name": "On vehicle",
            "addressline1": "",
            "city": "Klong Thoey",
            "province": "Bangkok",
            "postcode": "10110",
            "country": "Thailand"
          }
        },
        {
          "event_time": "2015-08-17T12:00:59+07:00",
          "event_name": "Order received",
          "event_code": "050000",
          "location": {
            "shipper_location_ref": "XYZL1234568THSAMUT",
            "hs_location_ref": "cim1if4pu0001fjmiyl0bu90f",
            "location_name": "Merchant Warehouse B",
            "addressline1": "M9 Some road",
            "city": "Bangpla",
            "province": "Samutprakan",
            "postcode": "10540",
            "country": "Thailand"
          }
        }
      ]
    }
  ],
  "response_code": 200,
  "error_codes": []
}
```

Retrieves all the locations with timestamp that this order went through.

### HTTP Request

`GET https://api.all24.com/rest/v2/tracking/orders?shipper_order_ref=XYZ20170623000001`

### Query Parameters

Parameter | Description
--------- | -----------
shipper_order_ref | This is shippers order reference
hs_shipping_order_ref | This is all24's order reference
shipment_item_ref | This is a reference to an item which has been shipped with in an order
* please use only one of the references above in a single request

### Response

Property | Type
---------|-----
shipper_order_reference | String
hs_shipping_order_ref | String
recipient_location | Location
recipient_location[shipper_location_ref] | String
recipient_location[hs_location_ref] | String
recipient_location[location_name] | String
recipient_location[addressline1] | String
recipient_location[city] | String
recipient_location[province] | String
recipient_location[postcode] | String
recipient_location[country] | String
delivery_orders | Array
hs_delivery_order_ref | String
shipment_item | Array
sender_location | Location
sender_location[shipper_location_ref] | String
sender_location[hs_location_ref] | String
sender_location[location_name] | String
sender_location[addressline1] | String
sender_location[city] | String
sender_location[province] | String
sender_location[postcode] | String
sender_location[country] | String
delivery_location | Location
delivery_location[shipper_location_ref] | String
delivery_location[hs_location_ref] | String
delivery_location[location_name] | String
delivery_location[addressline1] | String
delivery_location[city] | String
delivery_location[province] | String
delivery_location[postcode] | String
delivery_location[country] | String
history | Array
event_time | RFC3339
event_name | String
event_code | String
location | Location
location[shipper_location_ref] | String
location[hs_location_ref] | String
location[location_name] | String
location[addressline1] | String   
location[city] | String  
location[province] | String
location[postcode] | String
location[country] | String
response_code | String
error_codes | Array

### Event Code Type

Type | Value | Specification
-----|-------|--------------
event_code|050000|Order received
event_code|060000|Order live
event_code|200000|Picked up
event_code|300000|Goods arrived
event_code|410000|Pick up failed
event_code|420200|Delivery failed, Wrong recipient address
event_code|420201|Delivery failed, Recipient not home/office closed
event_code|420202|Delivery failed, Recipient could not be reached
event_code|420203|Delivery failed, Recipient refused Shipment / Pay / Sign
event_code|420204|Delivery failed, Rider reason
event_code|420300|Delivery failed, Recipient could not be contacted
event_code|420301|Delivery failed, Recipient postpone without appointment
event_code|420302|Delivery failed, Recipient postpone with appointment
event_code|420901|Delivery failed, Confirmation correction
event_code|420900|Delivery failed, Transport correction
event_code|401100|Order cancelled, Cancelled by shipper
event_code|401101|Order cancelled, Cancelled by carrier
event_code|401102|Order cancelled, Cancelled by recipient
event_code|401103|Order cancelled, Recipient could not be reach / all attempts failed
event_code|401104|Order cancelled, Out of delivery area
event_code|401105|Order cancelled, Parcel size exceeded
event_code|401106|Order cancelled, Parcel weight exceeded
event_code|401107|Order cancelled, Parcels COD amount exceeded
event_code|401108|Order cancelled, Parcel not received
event_code|401109|Order cancelled, Parcel lost
event_code|401110|Order cancelled, Packaging damaged
event_code|401300|Order cancelled, Recipient could not be contacted
event_code|401301|Order cancelled, Recipient postpone without appointment
event_code|401302|Order cancelled, Recipient postpone with appointment
event_code|401200|Order cancelled, Wrong recipient address
event_code|401201|Order cancelled, Recipient not home/office closed
event_code|401202|Order cancelled, Recipient could not be reached
event_code|401203|Order cancelled, Recipient refused Shipment / Pay / Sign

### Response Type

Type | Value | Specification
-----|-------|--------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1350|Order Not found

# Shipping

## Create Delivery Order

```shell
#Request:
$ curl https://api.all24.com/rest/v2/shipping/orders/new
  -u api_key: \
  -d hs_meta[schema-type]="hs-shipping-order \
  -d hs_meta[schema-version]=1.0 \
  -d hs_shipping_order[shipper]=HSXYZTH \
  -d hs_shipping_order[shipper_order_reference]=XYZ20170623000001 \
  -d hs_shipping_order[origin_order_date]=2015-08-18T11:12:00+07:00 \
  -d hs_shipping_order[remarks]=Some One is a first time customer \
  -d hs_shipping_order[recipient_location][location_ref]= \
  -d hs_shipping_order[recipient_location][zone]=THBKKKTY \
  -d hs_shipping_order[recipient_location][company]= \
  -d hs_shipping_order[recipient_location][building]=Some Tower \
  -d hs_shipping_order[recipient_location][room]=123/123 \
  -d hs_shipping_order[recipient_location][floor]=8th \
  -d hs_shipping_order[recipient_location][addressline1]=Rama 4 Road \
  -d hs_shipping_order[recipient_location][addressline2]= \
  -d hs_shipping_order[recipient_location][addressline3]= \
  -d hs_shipping_order[recipient_location][addressline4]= \
  -d hs_shipping_order[recipient_location][city]=Klong Thoey \
  -d hs_shipping_order[recipient_location][province]=Bangkok \
  -d hs_shipping_order[recipient_location][postcode]=10110 \
  -d hs_shipping_order[recipient_location][country]=Thailand \
  -d hs_shipping_order[recipient_location][notes_for_driver]= \
  -d hs_shipping_order[recipient_location][name]=Some One \
  -d hs_shipping_order[recipient_location][phone]=0987654321 \
  -d hs_shipping_order[recipient_location][email]= \
  -d hs_shipping_order[recipient_location][remarks]=
  -d hs_shipping_order[service_requirements][service_contract_reference]=HSXYZTHSTD \
  -d hs_shipping_order[service_requirements][is_cash_on_delivery]=yes \
  -d hs_shipping_order[service_requirements][expected_completion]=2015-08-23T23:59:59+07:00 \
  -d hs_shipping_order[payment_requirements][cod_amount]=1100.00 \
  -d hs_shipping_order[payment_requirements][charge_items][0][display_text]=Some other charges (global) \
  -d hs_shipping_order[payment_requirements][charge_items][0][display_sequence]=0 \
  -d hs_shipping_order[payment_requirements][charge_items][0][amount]=100.00
  -d hs_shipping_order[shipment_items][0][shipper_item_reference]=XYZI1234567890 \
  -d hs_shipping_order[shipment_items][0][goods_details][sku]=S12T-Gec-RS \
  -d hs_shipping_order[shipment_items][0][goods_details][quantity]=1 \
  -d hs_shipping_order[shipment_items][0][goods_details][description]=Summer 2012 \ Gecko Tee \ Small \ Red \
  -d hs_shipping_order[shipment_items][0][goods_details][declared_value]=1000.00 \
  -d hs_shipping_order[shipment_items][0][goods_details][length]= \
  -d hs_shipping_order[shipment_items][0][goods_details][width]= \
  -d hs_shipping_order[shipment_items][0][goods_details][height]= \
  -d hs_shipping_order[shipment_items][0][goods_details][weight]= \
  -d hs_shipping_order[shipment_items][0][goods_details][weight_unit]= \
  -d hs_shipping_order[shipment_items][0][goods_details][remarks_on_goods]= \
  -d hs_shipping_order[shipment_items][0][payment_requirements][charge_items][0][display_text]=Price (Item) \
  -d hs_shipping_order[shipment_items][0][payment_requirements][charge_items][0][display_sequence]=0 \
  -d hs_shipping_order[shipment_items][0][payment_requirements][charge_items][0][amount]=1000.00
  -d hs_shipping_order[shipment_items][0][payment_requirements][charge_items][1][display_text]=Tax (Item) \
  -d hs_shipping_order[shipment_items][0][payment_requirements][charge_items][1][display_sequence]=1 \
  -d hs_shipping_order[shipment_items][0][payment_requirements][charge_items][1][amount]=70.00
  -d hs_shipping_order[sender_location][location_ref]=XYZL1234567THSAMUT \
  -d hs_shipping_order[sender_location][zone]=THBKKBPLEE \
  -d hs_shipping_order[sender_location][company]=Some Warehouse Co. \ Ltd. \
  -d hs_shipping_order[sender_location][building]=Warehouse 3 \
  -d hs_shipping_order[sender_location][room]=Gate B \
  -d hs_shipping_order[sender_location][floor]= \
  -d hs_shipping_order[sender_location][addressline1]=M9 Some road \
  -d hs_shipping_order[sender_location][addressline2]= \
  -d hs_shipping_order[sender_location][addressline3]= \
  -d hs_shipping_order[sender_location][addressline4]= \
  -d hs_shipping_order[sender_location][city]=Bangpla \
  -d hs_shipping_order[sender_location][province]=Samutprakan \
  -d hs_shipping_order[sender_location][postcode]=10540 \
  -d hs_shipping_order[sender_location][country]=Thailand \
  -d hs_shipping_order[sender_location][notes_for_driver]= \
  -d hs_shipping_order[sender_location][name]=Warehouse outbound \
  -d hs_shipping_order[sender_location][phone]=0245657896 \
  -d hs_shipping_order[sender_location][email]= \
  -d hs_shipping_order[sender_location][remarks]= \
  -d hs_shipping_order[return_location][location_ref]=XYZL1234568THSAMUT \
  -d hs_shipping_order[return_location][zone]=THBKKBPLEE \
  -d hs_shipping_order[return_location][company]=Some Warehouse Co. \ Ltd. \
  -d hs_shipping_order[return_location][building]=Warehouse 4 \
  -d hs_shipping_order[return_location][room]=Gate B \
  -d hs_shipping_order[return_location][floor]= \
  -d hs_shipping_order[return_location][addressline1]=M9 Some road \
  -d hs_shipping_order[return_location][addressline2]= \
  -d hs_shipping_order[return_location][addressline3]= \
  -d hs_shipping_order[return_location][addressline4]= \
  -d hs_shipping_order[return_location][city]=Bangpla \
  -d hs_shipping_order[return_location][province]=Samutprakan \
  -d hs_shipping_order[return_location][postcode]=10540 \
  -d hs_shipping_order[return_location][country]=Thailand \
  -d hs_shipping_order[return_location][notes_for_driver]= \
  -d hs_shipping_order[return_location][name]=Warehouse inbound \
  -d hs_shipping_order[return_location][phone]=0245657897 \
  -d hs_shipping_order[return_location][email]= \
  -d hs_shipping_order[return_location][remarks]=
```

Delivery order instruction to all24.
This typically comes from ecommerce platform (Sales Order) or a warehouse management system (Delivery Order).

### HTTP Request

`POST https://api.all24.com/rest/v1/shipping/orders/new?hs_meta[schema-type]&...`

### Query Parameters

Parameter|Type|Specification
---------|----|-------------
hs_meta|HSMeta|Mandatory
hs_meta[schema-type]|String|Mandatory
hs_meta[schema-version]|String|
hs_shipping_order|HSShippingOrder|Mandatory
hs_shipping_order[shipper]|String|Mandatory
hs_shipping_order[shipper_order_reference]|String|Mandatory
hs_shipping_order[origin_order_date]|String|Mandatory
hs_shipping_order[remarks]|String|Optional
recipient_location|Location|Mandatory
hs_shipping_order[recipient_location][location_ref]|String|Optional
hs_shipping_order[recipient_location][zone]|String|Optional
hs_shipping_order[recipient_location][company]|String|Optional
hs_shipping_order[recipient_location][building]|String|Optional
hs_shipping_order[recipient_location][room]|String|Optional
hs_shipping_order[recipient_location][floor]|String|Optional
hs_shipping_order[recipient_location][addressline1]|String|Mandatory
hs_shipping_order[recipient_location][addressline2]|String|Optional
hs_shipping_order[recipient_location][addressline3]|String|Optional
hs_shipping_order[recipient_location][addressline4]|String|Optional
hs_shipping_order[recipient_location][city]|String|Mandatory
hs_shipping_order[recipient_location][province]|String|Mandatory
hs_shipping_order[recipient_location][postcode]|String|Mandatory
hs_shipping_order[recipient_location][country]|String|Mandatory
hs_shipping_order[recipient_location][notes_for_driver]|String|Optional
hs_shipping_order[recipient_location][name]|String|Mandatory
hs_shipping_order[recipient_location][phone]|String|Mandatory
hs_shipping_order[recipient_location][email]|String|Optional
hs_shipping_order[recipient_location][remarks]|String|Optional
service_requirements|ServiceRequirements|Mandatory
hs_shipping_order[service_requirements][service_contract_reference]|String|Mandatory
hs_shipping_order[service_requirements][is_cash_on_delivery]|String|Optional
hs_shipping_order[service_requirements][expected_completion]|String|Optional
payment_requirements|PaymentRequirements|Conditional
hs_shipping_order[payment_requirements][cod_amount]|String|Conditional
charge_items|Array|Optional
hs_shipping_order[payment_requirements][charge_items][display_text]|String|Mandatory
hs_shipping_order[payment_requirements][charge_items][display_sequence]|String|Mandatory
hs_shipping_order[payment_requirements][charge_items][amount]|String|Mandatory
shipment_items|Array|Mandatory
hs_shipping_order[shipment_items][shipper_item_reference]|String|Mandatory
goods_details|GoodsDetails|Optional
hs_shipping_order[shipment_items][goods_details][sku]|String|Mandatory
hs_shipping_order[shipment_items][goods_details][quantity]|String|Mandatory
hs_shipping_order[shipment_items][goods_details][description]|String|Optional
hs_shipping_order[shipment_items][goods_details][declared_value]|String|Mandatory
hs_shipping_order[shipment_items][goods_details][length]|String|Optional
hs_shipping_order[shipment_items][goods_details][width]|String|Optional
hs_shipping_order[shipment_items][goods_details][height]|String|Optional
hs_shipping_order[shipment_items][goods_details][weight]|String|Optional
hs_shipping_order[shipment_items][goods_details][weight_unit]|String|Optional
hs_shipping_order[shipment_items][goods_details][remarks_on_goods]|String|Optional
charge_items|Array|Optional
hs_shipping_order[shipment_items][payment_requirements][charge_items][display_text]|String|Mandatory
hs_shipping_order[shipment_items][payment_requirements][charge_items][display_sequence]|String|Mandatory
hs_shipping_order[shipment_items][payment_requirements][charge_items][amount]|String|Mandatory
sender_location|Location|Mandatory
hs_shipping_order[sender_location][location_ref]|String|Optional
hs_shipping_order[sender_location][zone]|String|Optional
hs_shipping_order[sender_location][company]|String|Optional
hs_shipping_order[sender_location][building]|String|Optional
hs_shipping_order[sender_location][room]|String|Optional
hs_shipping_order[sender_location][floor]|String|Optional
hs_shipping_order[sender_location][addressline1]|String|Mandatory
hs_shipping_order[sender_location][addressline2]|String|Optional
hs_shipping_order[sender_location][addressline3]|String|Optional
hs_shipping_order[sender_location][addressline4]|String|Optional
hs_shipping_order[sender_location][city]|String|Mandatory
hs_shipping_order[sender_location][province]|String|Mandatory
hs_shipping_order[sender_location][postcode]|String|Mandatory
hs_shipping_order[sender_location][country]|String|Mandatory
hs_shipping_order[sender_location][notes_for_driver]|String|Optional
hs_shipping_order[sender_location][name]|String|Mandatory
hs_shipping_order[sender_location][phone]|String|Mandatory
hs_shipping_order[sender_location][email]|String|Optional
hs_shipping_order[sender_location][remarks]|String|Optional
return_location|Location|Optional
hs_shipping_order[return_location][location_ref]|String|Optional
hs_shipping_order[return_location][zone]|String|Optional
hs_shipping_order[return_location][company]|String|Optional
hs_shipping_order[return_location][building]|String|Optional
hs_shipping_order[return_location][room]|String|Optional
hs_shipping_order[return_location][floor]|String|Optional
hs_shipping_order[return_location][addressline1]|String|Mandatory
hs_shipping_order[return_location][addressline2]|String|Optional
hs_shipping_order[return_location][addressline3]|String|Optional
hs_shipping_order[return_location][addressline4]|String|Optional
hs_shipping_order[return_location][city]|String|Mandatory
hs_shipping_order[return_location][province]|String|Mandatory
hs_shipping_order[return_location][postcode]|String|Mandatory
hs_shipping_order[return_location][country]|String|Mandatory
hs_shipping_order[return_location][notes_for_driver]|String|Optional
hs_shipping_order[return_location][name]|String|Mandatory
hs_shipping_order[return_location][phone]|String|Mandatory
hs_shipping_order[return_location][email]|String|Optional
hs_shipping_order[return_location][remarks]|String|Optional

### Response

Attribute|Type
---------|----
response_code|string
error_codes|Array
hs_shipping_order_ref|string

### Response Type
Type|Value|Specification
----|-----|-------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1010|Meta missing
error_codes|1011|Meta schema type missing
error_codes|1012|Meta schema version missing
error_codes|1013|Payload missing
error_codes|4300|Service Code missing
error_codes|4000|Failed to Create Order
error_codes|4100|Duplicate Order


## Cancel Shipment Item

```shell
#Request:
$ curl https://api.all24.com/rest/v2/shipping/orders/items/cancel
  -u api_key: \
  -d shipper_order_ref=XYZ20170623000001 \
  -d shipment_item_ref=XYZI1234567890
```

Cancels specific shipment items for a shipping order.

### HTTP Request

`POST https://api.all24.com/rest/v2/shipping/orders/items/cancel?shipper_order_ref=XYZ20170623000001&shipment_item_ref=XYZI1234567890


### Query Parameters

Parameter | Description
--------- | -----------
shipment_item_ref | This is a reference to an item which has been shipped with in an order
shipper_order_ref | This is shippers order reference
hs_shipping_order_ref | This is all24's order reference
* please use either shipper_order_ref or hs_shipping_order_ref to identify the shipping order

### Response

Attribute|Type
---------|----
response_code|string
error_codes|Array

### Response Type
Type|Value|Specification
----|-----|-------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|4200|Shipping order not found
error_codes|4250|Shipping item not found


# Fulfillment

## Create Purchase Order

```shell
#Request:
$ curl https://api.all24.com/rest/v1/warehouses/purchase-orders/new
  -u api_key: \
  -d hs_meta[schema-type]=hs-purchase-order \
  -d hs_meta[schema-version]=1.0 \
  -d hs_purchase_order[shipper]=HSXYZTH \
  -d hs_purchase_order[warehouse_location_ref]=cim1if4pu0001fjmiyl0bu90f \
  -d hs_purchase_order[arrival_due_date]=2015-08-18T23:59:59+07:00 \
  -d hs_purchase_order[line_items][0][sku]=S12T-Gec-RS \
  -d hs_purchase_order[line_items][0][quantity]=100 \
  -d hs_purchase_order[line_items][0][quantity_to_3pl]=50 \
  -d hs_purchase_order[line_items][0][cost]=100.00 \
  -d hs_purchase_order[po_number]=XYZ1234PO \
  -d hs_purchase_order[supplier_name]=Some Supplier Co., Ltd. \
```

Creates a Purchase Order for selected warehouse

### HTTP Request

`POST https://api.all24.com/rest/v1/warehouses/locations/purchase-orders/new?hs_meta[schema-type]&...`


### Query Parameters

Parameter|Type|Specification
---------|----|-------------
hs_meta|HSMeta|Mandatory
hs_meta[schema-type]|Mandatory
hs_meta[schema-version]|Mandatory
hs_purchase_order|HSPurchaseOrder|Mandatory
hs_purchase_order[shipper]|Mandatory
hs_purchase_order[warehouse_location_ref]|Mandatory
hs_purchase_order[arrival_due_date]|Optional
hs_purchase_order[line_items][sku]|Mandatory
hs_purchase_order[line_items][quantity]|Mandatory
hs_purchase_order[line_items][quantity_to_3pl]|Mandatory
hs_purchase_order[line_items][cost]|Optional
hs_purchase_order[po_number]|Mandatory
hs_purchase_order[supplier_name]|Mandatory


### Response

Attribute|Type
---------|----
response_code|string
error_codes|Array

### Response Type
Type|Value|Specification
----|-----|-------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1010|Meta missing
error_codes|1011|Meta schema type missing
error_codes|1012|Meta schema version missing
error_codes|1013|Payload missing

## Add Product

```shell
#Request:
$ curl https://api.all24.com/rest/v1/warehouse/locations/products/new
  -u api_key: \
  -d hs_meta[schema-type]=hs-purchase-order \
  -d hs_meta[schema-version]=1.0 \
  -d hs_product_details[shipper]=HSXYZTH \
  -d hs_product_details[warehouse_location_ref]=cim1if4pu0001fjmiyl0bu90f \
  -d hs_product_details[classification]=General \
  -d hs_product_details[sku]=S12T-Gec-RS \
  -d hs_product_details[length]= \
  -d hs_product_details[width]= \
  -d hs_product_details[height]= \
  -d hs_product_details[weight]= \
  -d hs_product_details[weight_unit]= \
  -d hs_product_details[supplier_info][0][cost]=100.00 \
  -d hs_product_details[supplier_info][0][cost][0][supplier_name]=Some Supplier \
  -d hs_product_details[supplier_info][0][cost][0][is_primary]=true
```

Add a new product for selected warehouse

### HTTP Request

`POST https://api.all24.com/rest/v1/warehouse/locations/products/new?hs_meta[schema-type]&...`


### Query Parameters

Parameter|Type|Specification
---------|----|-------------
hs_meta|HSMeta|Mandatory
hs_meta[schema-type]|String|Mandatory
hs_meta[schema-version]|String|Mandatory
hs_product_details|HSProductDetails|Mandatory
hs_product_details[shipper]|String|Mandatory
hs_product_details[warehouse_location_ref]|String|Mandatory
hs_product_details[classification]|String|Mandatory
hs_product_details[sku]|String|Mandatory
hs_product_details[length]|String|Optional
hs_product_details[width]|String|Optional
hs_product_details[height]|String|Optional
hs_product_details[weight]|String|Optional
hs_product_details[weight_unit]|String|Optional
hs_product_details[supplier_info]|Array|Mandatory
supplier_info[cost]|String|Optional
supplier_info[supplier_name]|String|Mandatory
supplier_info[is_primary]|String|Mandatory


### Response

Attribute|Type
---------|----
response_code|string
error_codes|Array

### Response Type
Type|Value|Specification
----|-----|-------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1010|Meta missing
error_codes|1011|Meta schema type missing
error_codes|1012|Meta schema version missing
error_codes|1013|Payload missing


## Get Inventory

```shell
#Request:
$ curl https://api.all24.com/rest/v1/warehouses/Locations/inventory
  -u api_key: \
  -d warehouse_location_ref=cim1if4pu0001fjmiyl0bu90f
  -d shipper=HSXYZTH
```

```json
#Response:
{
    "item_quantities": [
        {
            "quantity": "50",
            "sku": "S12T-Gec-RS"
        }
    ],
    "response_code": "200",
    "error_codes":[]
}
```

Retrieves all product quantities for selected warehouse location

### HTTP Request

`GET https://api.all24.com/rest/v1/warehouses/Locations/inventory?warehouse_location_ref=cim1if4pu0001fjmiyl0bu90f&shipper=HSXYZTH`

### Query Parameters

Parameter | Description
--------- | -----------
warehouse_location_ref | This is the warehouse location reference
shipper | This is a customer account reference

### Response

Property | Type
---------|-----
item_quantities|Array
quantity|String
sku|String
response_code|String
error_codes|Array

### Response Type

Type | Value | Specification
-----|-------|--------------
response_code|200|Request Ok
response_code|400|Request Failed
error_codes|1000|Missing Parameters
error_codes|1350|Order Not found
error_codes|1400|Unknown Location Reference
error_codes|1300|Unknown Shipper Reference
