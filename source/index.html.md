---
title: Zaui IO API

language_tabs: # must be one of https://git.io/vQNgJ
  - XML Examples
  - JSs

toc_footers:
  - <a href='https://api.zaui.io/signup'>Sign Up for a Developer Key</a>
  - <a href='#'>See our Web API Documentation</a>
  - <a href='#'>See our Webhook and Remote Target Documentation</a>

search: true
---

# Zaui Software IO API

## Introduction

Zaui.io is an XML API based messaging platform designed for resellers, developers or anyone wanting access to real-time inventory from any suppliers within the Zaui supplier market place.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# API Throttle

To ensure continuous quality of service, API usage can be subjected to throttling. The throttle will be applied once an API client reaches a certain threshold.
Zaui Software reserves the right to throttle any and all API clients to ensure quality of service for all Zaui Software customers.

Those clients who do encounter a throttle threshold will get met with a HTTP 503 response code and the error XML node populated.

We encourage all API developers to anticipate this error and take appropriate measures like:<br>
• Using a cached value from a previous call
<br>• Passing on a message to the end user that is subjected to this behavior (if any)
<br>• Implement exponential back-off in your logic (see below) <br>Before starting, anyone organization wishing to use this API, must have been provided authentication credentials.

# Authentication Credentials

All API calls are authenticated. To obtain your login credentials, please visit www.zaui.io, use our automated reseller sign up to obtain your login credentials. Credentials will be your reseller, or developer, ID and your authentication token, and destination supplier ID.

Credentials for the API can be obtained by signing up, for free, on our website: <https://api.zaui.io/signup>

# Test Supplier System

During your development and testing phases, and prior to connecting to live suppliers, you are welcome to use the test supplier ID for all your API calls: Test supplier ID = 200

This supplier has various tours and activities available for you to integrate your test environment with.

Should you need to see a specific activity type in this system, which is not present, please contact our support staff at support@zaui.com

# Submitting to the API
> All API access is over HTTPS and accessed from the <https://api.zaui.io/v1/> domain.

```xml
$ curl -ki https://api.zaui.io/v1/
HTTP/1.1 200 OK
Date: Wed, 07 Jan 2015 00:12:51 GMT Server: Apache
X-Powered-By: PHP/5.4.35
Vary: Accept-Encoding Content-Length: 411
Content-Type: text/xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<version xmlns="http://www.zaui.io/api" xmlns:atom="http://www.w3.org/2005/Atom" id="v1" status="CURRENT" >
  <media-types>
    <media-type base="application/xml" type="application/xml;version=v1"/>
  </media-types>
  <atom:link rel="self" href="https://api.zaui.io/v1"/>
  <atom:link rel="describedby" href="http://www.zaui.io/api/v1"/>
</version>
```

All API request to the Zaui.io platform must be sent using an HTTPS POST using the following URI:

All API access is over HTTPS and accessed from the https://api.zaui.io/v1/ domain.

Each response consists of an object with the following properties:<br>
**• HTTP response status code:** The status code of the HTTP response. This does match the header value of the response, unless suppress_response_codes=true was sent (see below). For a list of suppress_response_codes=true valid response codes, see the appendix at the close of this document. <br>**• XML data payload:** The result data will always return XML

### Suppressing HTTP Response codes

Certain technologies either don’t handle or can’t handle HTTP response codes. More specifically, they
don’t handle any non 200 HTTP status codes. For this result our API provides provisioning for this. In
this case, your implementation can send through the suppress_response_codes with the value true .
This will always request a status code of 200 in the response header. The original, and true, status code
is then available within the response body.

### Implementing Exponential Back-off

Exponential back-off is the process of a client periodically retrying a failed request over an increasing amount of time. *It is a standard error handling strategy for network applications.* Besides being “required”, using exponential back-off increases the efficiency of bandwidth usage, reduces the number of requests required to get successful response, and maximizes the throughput of requests in concurrent environments.
The flow of implementing a simple exponential back-off is as follows:

1.  Make a request to the API
2.  Receive the response, check for error that **has a retry-able error code (such as 503)**
3.  Wait 1s + random_number_milliseconds seconds
4.  Retry request
5.  Receive the response, check for error that **has a retry-able error code (such as 503)**
6.  Wait 2s + random_number_milliseconds seconds
7.  Retry request
8.  Receive the response, check for error that **has a retry-able error code (such as 503)**
9.  Wait   seconds
10. Retry request
11. Receive the response, check for error that **has a retry-able error code (such as 503)**
12. Wait 8s + random_number_milliseconds seconds
13. Retry request
14. Receive the response, check for error that **has a retry-able error code (such as 503)**
15. Wait 16s + random_number_milliseconds seconds
16. Retry request
17. If you still get an error, stop and log the error

**Note:** _random_number_milliseconds MUST be redefined after each “Wait”_

In the above flow, random_number_milliseconds is a random number of milliseconds less than or equal to 1000. This is necessary to avoid certain lock errors in some concurrent implementations.

**Note:** _the wait is always (2^n) + random_number_milliseconds, where n is a mono-tonically increasing integer initially defined as 0. N is incremented by 1 for each iteration (each request)_

The algorithm is set to terminate when n == 5. This ceiling is in place to prevent clients from retrying infinitely, and results in a total delay of 32 seconds before a deemed “unrecoverable error”.

# API Function Definitions

## Required Authentication Variables

While we have outlined each of the request and response API calls, its important to know that you must
submit through your *ResellerID*, destination *SupplierId* and *APIKey* with every API request to our systems.

## Activity List Requests

> Example Activity List Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ActivityListRequest  xmlns="https://api.zaui.io/api/01">
   <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
   <ResellerId>2005</ResellerId>
   <SupplierId>200</SupplierId>
   <ExternalReference>10051374722994001</ExternalReference>
   <Timestamp>2015-01-10T13:30:54.616+10:00</Timestamp>
</ActivityListRequest>
```

The **ActivityListRequest** enables pulling a list of available activities and options from the specified supplier. The activity list will include descriptive fields, options, and the specific activity value, to be used on subsequent API calls where a single activity is requested by your implementation.

It is the responsibility of any integration to store the unique activity codes for further API calls, where its required.

| XML Node                       | Parent Node         | Description                                                                                                                                           | Optionality |
| ------------------------------ | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| ActivityListRequest            |                     | Root XML node| Mandatory   |
| APIKey                         | ActivityListRequest | Your unique API KEY| Mandatory   |
| ResellerID                     | ActivityListRequest | Your unique reseller ID| Mandatory   |
| SupplierID                     | ActivityListRequest | Your unique reseller ID| Mandatory   |
| ExternalReference              | ActivityListRequest | String representing a unique transaction ID. Used to identify your original request.| Optional    |
| TimeStamp                      | ActivityListRequest | Time of creation request YYYY-MM-DDTHH:MM:SS.SSSZ(in utc time) or YYYY-MM-DDTHH:MM:SS.SSS[+/-]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) |             |
| 2013-04-28T13:10:12.123+10:00 | Mandatory           |                                                                                                                                                       |             |

**Note:** _Your implementation must store the returned activity codes for future calls, where required. The ActivityListRequest should be called periodically, since the supplier can update the available activities on the Zaui IO channels anytime, potentially making your stored activity codes stale._

**Note:** _Depending on the number of allowed activities that the supplier your communicating with has provided for you, the data size on this call back can be large. As a result, it’s not uncommon to see this callback having a large data set. This call can often take up to 30 seconds to complete._

## Activity List Response

> Example Activity List Response

```xml
<?xml version="1.0" encoding="UTF-8"?>

<ActivityListResponse  xmlns="https://api.zaui.io/api/01" >
 <APIKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</APIKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722994001</ExternalReference>
 <Timestamp>2015-12-10T13:30:54.616+10:00</Timestamp>
 <RequestStatus>
         <Status>SUCCESS</Status>
 </RequestStatus>
 <Tour>
         <SupplierProductCode>ACT_1765</SupplierProductCode>
<SupplierProductName>Zip line Adventure</SupplierProductName>
<TourDepartureTime>09:00:00</TourDepartureTime>
<TourDuration>PT1D</TourDuration>
<CountryCode>USA</CountryCode>
<DestinationCode>WA</DestinationCode>
<DestinationName>Washington</DestinationName>
<TourDescription>Zip line Adventure that will blow your mind</TourDescription>
<TourOption>
        <SupplierOptionCode>ACT_1765</SupplierOptionCode>
        <SupplierOptionName>Zip line</SupplierOptionName>
        <TourDepartureTime>09:00:00</TourDepartureTime>
        <TourDuration>PT1D</TourDuration>
      </TourOption>
<PickupLocation>
        <SupplierPickupCode>202</SupplierPickupCode>
        <SupplierPickupName>Hilton</SupplierPickupName>
</PickupLocation>
<DropoffLocation>
        <SupplierPickupCode>309</SupplierPickupCode>
        <SupplierPickupName>Airport – Terminal 2</SupplierPickupName>
</DropoffLocation>
</Tour>
<Tour>
<SupplierProductCode>ACT_1765</SupplierProductCode>
<SupplierProductName>Zip line Adventure</SupplierProductName>
<TourDepartureTime>09:30:00</TourDepartureTime>
<TourDuration>PT1D</TourDuration>
<CountryCode>USA</CountryCode>
<DestinationCode>WA</DestinationCode>
<DestinationName>Washington</DestinationName>
<TourDescription>Zip line Adventure that will blow your mind</TourDescription>
<TourOption>
       <SupplierOptionCode>ACT_1765</SupplierOptionCode>
       <SupplierOptionName>Zip line</SupplierOptionName>
       <TourDepartureTime>09:30:00</TourDepartureTime>
       <TourDuration>PT1D</TourDuration>
</TourOption>
<PickupLocation>
         <SupplierPickupCode>202</SupplierPickupCode>
         <SupplierPickupName>Hilton</SupplierPickupName>
</PickupLocation>
<DropoffLocation>
         <SupplierPickupCode>309</SupplierPickupCode>
         <SupplierPickupName>Airport – Terminal 2</SupplierPickupName>
</DropoffLocation>
</Tour>
</TourListResponse >

```

The ActivityListResponse will contain the details for all the activities that the supplier has made available for you to pull across through the Zaui IO API.

| XML Node             | Parent Node          | Description|
| -------------------- | -------------------- | -----------|
| ActivityListResponse |                      | Root XML node|
| APIKey               | ActivityListResponse | Your unique API KEY|
| ResellerId           | ActivityListResponse | Your unique reseller ID|
| SupplierId           | ActivityListResponse | String representing the unique supplier ID within the Zaui Marketplace|
| ExternalReference    | ActivityListResponse | String representing a unique transaction ID. Used to identify your original request.|
| TimeStamp            | ActivityListResponse | Time of creation of request|
|                      |                      | YYYY-MM-DDTHH:MM:SS.SSSZ(in utc time) or YYYY-MM-DDTHH:MM:SS.SSS[+/-]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04-28T13:10:12.123+10:00 |
| RequestStatus        | ActivityListResponse | Request status root XML node, which holds details about the status|
| Status               | RequestStatus        | Status of the request. **SUCCESS** for a successful transaction, or **ERROR** for unsuccessful transaction. Error XML node will be provided|
| Error                | RequestStatus        | Error root XML node |
| ErrorCode            | Error                | The error code|
| ErrorMessage         | Error                | The error code|
| ErrorDetails         | Error                | String for the error message|
| Activity             | ActivityListResponse | Root activity XML node|
| SupplierProductCode  | Activity             | Suppliers unique product code. This will be used across multiple API calls and should be stored as part of your implementation.|
| SupplierProductName  | Activity             | String that contains the supplier activity name|
| TourDescription      | Activity             | Description of the activity as provided by the supplier. |
| TourDepartureTime    | Activity             | Time of the activity. Values will be in the format of HH:MM:SS |
| TourDuration         | Activity             | Duration of the activity. Format will be **PnYnMnDTnHnMnS**. Example PT300s (5 mins)|
| Tour Option          | Activity             | Root XML node that holds details for options on the activity.|
| SupplierOptionCode   | TourOption           | Duration of the activity option. Duration format **PnYnMnDTnHnMnS** |
| PickupLocation       | Activity             ||
| SupplierPickupCode   | PickupLocation       | Supplier pickup ID code|
| SupplierPickupName   | PickupLocation       | Supplier pickup location name as a string|
| DropoffLocation      | Activity             ||
| SupplierDropoffCode  | DropoffLocation      | Supplier dropoff ID code|
| SupplierDropoffName  | DropoffLocation      | Supplier dropoff location name as a string|


## Check Availability RequestStatus

> Example Single Date Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CheckAvailabilityRequest  xmlns="https://api.zaui.io/api/01" >
  <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
  <ResellerId>2005</ResellerId>
  <SupplierId>200</SupplierId>
  <ExternalReference>10051374722992616</ExternalReference>
  <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <StartDate>2015-10-31</StartDate>
 <EndDate></EndDate>
 <SupplierProductCode>ACT_1765</SupplierProductCode>
<TourOptions>
         <SupplierOptionCode>09:00:00</SupplierOptionCode>
         <SupplierOptionName>Zip line 9:00 am</SupplierOptionName>
         <TourDepartureTime>09:00:00</TourDepartureTime>
         <TourDuration>PT1H</TourDuration>
 </TourOptions>
 <TravellerMix>
         <Senior>0</Senior>
         <Adult>1</Adult>
         <Student>0</Student>
         <Child>1</Child>
         <Infant>0</Infant>
         <Total>2</Total>
 </TravellerMix>
</CheckAvailabilityRequest>
```

> Example Date Range Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CheckAvailabilityRequest  xmlns="https://api.zaui.io/api/01" >
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
<StartDate>2015-10-30</StartDate>
<EndDate>2015-10-31</EndDate>
<SupplierProductCode>ACT_1765</SupplierProductCode>
<TourOptions>
       <SupplierOptionCode>09:00:00</SupplierOptionCode>
</TourOptions>
<TravellerMix>
        <Senior>0</Senior>
        <Adult>1</Adult>
        <Student>0</Student>
        <Child>1</Child>
        <Infant>0</Infant>
        <Total>2</Total>
</TravellerMix>
</CheckAvailabilityRequest>
```

The availability request determines if the requested activity has availability for the specific number of passengers in real-time.

This API call provisions for requesting availability by specifying a specific date, or providing a date range. In either case, the API allows providing tour options optionally.


| XML Node | Parent Node | Description | Optionality |
| -------- | ----------- | ----------- | ----------- |
| CheckAvailabilityRequest|| Root XML node| Mandatory|
| APIKey| CheckAvailabilityRequest | Your unique API KEY | Mandatory |
| ResellerID | CheckAvailabilityRequest || Mandatory |
| SupplierID | CheckAvailabilityRequest | String representing the unique supplier ID within the Zaui Marketplace| Mandatory |
| ExternalReference | CheckAvailabilityRequest | String representing a unique transaction ID. Used to identify your original request. | Optional |
| TimeStamp | CheckAvailabilityRequest | Time of creation of request YYYY-MM-DDTHH:MM:SS.SSSZ(in utc time) or YYYY-MM-DDTHH:MM:SS.SSS[+/-]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04-28T13:10:12.123+10:00 | Mandatory |
| StartDate | CheckAvailabilityRequest | Date for which to check. Date format: YYYY-MM-DD | Mandatory |
| EndDate | CheckAvailabilityRequest | Date for which to end checking. Date format: YYYY-MM-DD | Optional |
| SupplierProductCode | CheckAvailabilityRequest | The unique product code for the supplier | Mandatory |
| TourOptions | CheckAvailabilityRequest | Tour option root XML node | Optional |
| SupplierOptionCode | TourOptions | String representing the unique supplier option code | Optional |
| SupplierOptionName | TourOptions | String representing the supplier option name | Optional |
| TourDepartureTime | TourOptions | Time of the activity departure and must be in the format of HH:MM:SS | Optional |
| TourDuration | TourOptions | Option duration, and must be in the format: **PnYnMnDTnHnMnS**. | Optional |
| TravellerMix | CheckAvailabilityRequest | Root XML element that contains the details of the passenger types to query for inventory | Optional |
| PickupLocation | CheckAvailabilityRequest | Root node for the pickup location| |
| SupplierPickupCode | PickupLocation | The pickup location ID, which is returned as part of the ActivityListRequest | Optional, but required for all transportation based supplier products |
| DropoffLocation | CheckAvailabilityRequest | Root node for the dropoff location | |
| SupplierPickupCode | PickupLocation | The pickup location ID, which is returned as part of the ActivytListRequest | Optional, but required for all transportation based supplier product |
| Senior | TravellerMix | | Optional |
| Adult | TravellerMix | | Optional|
| Student | TravellerMix | | Optional |
| Children | TravellerMix | | Optional |
| Infant | TravellerMix | | Optional |
| Total | TravellerMix | | Optional |


**Note:** _Requests for availability can be requested no further than 120 days out. Availability for historical dates are not permitted. Transportation based supplier products may require that you send through the proper pickup and dropoff supplier location codes._

## Check Availability Response

> Example Single Date Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CheckAvailabilityResponse  xmlns="https://api.zaui.io/api/01">
<ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
  <ResellerId>2005</ResellerId>
  <SupplierId>20</SupplierId>
  <ExternalReference>10051374722992616</ExternalReference>
  <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
  <RequestStatus>
    <Status>SUCCESS</Status>
  </RequestStatus>
  <SupplierProductCode>ACT_1765</SupplierProductCode>
  <TourAvailability>
  <Date>2015-10-31</Date>
  <TourOptions>
    <SupplierOptionCode>09:00:00</SupplierOptionCode>
    <SupplierOptionName>Zip line 9:00 am</SupplierOptionName>
    <TourDepartureTime>09:00:00</TourDepartureTime>
    <TourDuration>PT1H</TourDuration>
  </TourOptions>
  <AvailabilityStatus>
    <Status>AVAILABLE</Status>
  </AvailabilityStatus>
  </TourAvailability>
    <TravellerMixAvailability>
      <Senior>TRUE</Senior>
      <Adult>TRUE</Adult>
      <Student>TRUE</Student>
      <Child>TRUE</Child>
      <Infant>FALSE</Infant>
    </TravellerMixAvailability>
</CheckAvailabilityResponse>
```

>Date Range Example response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CheckAvailabilityResponse  xmlns="https://api.zaui.io/api/01">
  <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
  <ResellerId>2005</ResellerId>
  <SupplierId>20</SupplierId>
  <ExternalReference>10051374722992616</ExternalReference>
<Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
  <RequestStatus>
    <Status>SUCCESS</Status>
  </RequestStatus>
  <SupplierProductCode>ACT_1765</SupplierProductCode>
  <TourAvailability>
    <Date>2015-10-30</Date>
    <TourOptions>
<SupplierOptionCode>09:00:00</SupplierOptionCode> <SupplierOptionName>Zip line at 9:00 am</SupplierOptionName> <TourDepartureTime>09:00:00</TourDepartureTime> <TourDuration></TourDuration>
    </TourOptions>
    <AvailabilityStatus>
      <Status>AVAILABLE</Status>
    </AvailabilityStatus>
  </TourAvailability>
  <TourAvailability>
    <Date>2015-10-31</Date>
    <TourOptions>
<SupplierOptionCode>ACT_1765</SupplierOptionCode> <SupplierOptionName>Zip Line at 9:00 am</SupplierOptionName> <TourDepartureTime>09:00:00</TourDepartureTime> <TourDuration></TourDuration>
    </TourOptions>
    <AvailabilityStatus>
      <Status>UNAVAILABLE</Status>
      <UnavailabilityReason>SOLD_OUT</UnavailabilityReason>
    </AvailabilityStatus>
 </TourAvailability>
</CheckAvailabilityResponse>
```

| XML Node | Parent Node | Description|
| -------- | ----------- | ---------- |
| CheckAvailabilityResponse || Root XML Node|
| APIKey | CheckAvailabilityResponse | Your unique API KEY |
| ResellerId | CheckAvailabilityResponse | |
| SupplierID | CheckAvailabilityResponse | String representing the unique supplier ID within the Zaui Marketplace|
| External Reference | CheckAvailabilityResponse | String representing a unique transaction ID. Used to identify your original request. |
| TimeStamp | CheckAvailabilityResponse | Time of creation of request: <br> YYYY-MM-DDTHH:MM:SS.SSSZ(in utc time) or YYYY-MM-DDTHH:MM:SS.SSS[+/-]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04-28T13:10:12.123+10:00 |
| RequestStatus | CheckAvailabilityResponse | Request status root XML element |
| Status | RequestStatus | Status value for the request. Values are:<br> **SUCCESS** <br>**ERROR** |
| Error | Request Status | Root node for the error details on a non successful request. |
| ErrorCode | Error | String with error code. |
| ErrorMessage | Error | String with the error message |
| ErrorDetails | Error | String with additional details, and recommendation on error |
| SupplierProductCode | CheckAvailabilityResponse | Unique supplier product code |
| TourAvailability | CheckAvailabilityResponse | Root node with the tour availability |
| Date | TourAvailability | Date for which the tour is available, in the format YYYY-MM-DD |
| TourOptions | TourAvailability | Tour options root node |
| SupplierOptionCode | TourOptions | String containing the unique supplier option code |
| SupplierOptionName | TourOptions | String containing the supplier option name |
| TourDepartureTime | TourOptions | Time of the tour departure, in the format HH:MM:SS |
| TourDuration | TourOptions | Duration for the tour option, in the format **PnYnMnDTnHnMnS**. Example: PT300s (5 mins). |
| AvailabilityStatus | CheckAvailabilityResponse | Root element for availability status |
| Status | Availability Status | Status of the availability request. Values are: <br> AVAILABLE - when available for the given Date and TravellerMix. <br> UNAVAILABLE - when not available for the given Date and TravellerMix. |
| UnavailabilityReason | AvailabilityStatus | Reason why the requested activity is NOT available. <br> Valid values: <br> SOLD_OUT <br> BLOCKED_OUT - when the product (Tour)/ product option (Tour Option) has been blocked out (not taking place on this date). <br> INACTIVE - when the product (Tour)/ product option (Tour Option) is no longer active. Recommend a ActivityListRequest to refresh the supplier product codes <br> PAST_CUTOFF_DATE - when the booking cut-off date has been reach for this product (Tour)/ product option (Tour Option). <br> TRAVELLER_MISMATCH - when the required combination of travellers for this product (Tour)/ product option (Tour Option) is not met. |
| TravellerMixAvailability | CheckAvailabilityResponse | Guest mix availability root element |
| Senior | TravellerMixAvailability| TRUE or FALSE |
|Adult | TravellerMixAvailability | TRUE or FALSE |
| Student | TravellerMixAvailability | TRUE or FALSE |
| Child | TravellerMixAvailability | TRUE or FALSE|
| Infant | TravellerMixAvailability | TRUE or FALSE|

## Batch Check Availability Request

> Example Single Day Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<BatchCheckAvailabilityRequest  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <StartDate>2015-10-1</StartDate>
 <SupplierProductCode>ACT_1765</SupplierProductCode>
</BatchCheckAvailabilityRequest>
```

> Example Multi Date Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<BatchCheckAvailabilityRequest  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
<StartDate>2015-10-1</StartDate>
<EndDate>2015-10-31</EndDate>
<SupplierProductCode>ACT_1765</SupplierProductCode>
</BatchCheckAvailabilityRequest>
```

The batch availability request allows you to determine availability in large scale. This API call is designed to enable fetching all activity and activity options for a single date or date range. When a single activity is provided, then the API will return the activity availability for the date or date range requested. When a activity is not provided, then the API will return all activities and options for the date or date range requested.

| XML Node | Parent Node | Description | Optionality |
| -------- | ----------- | ------------------------ | ----------- |
| BatchCheckAvailabilityRequest || Root XML node | Mandatory |
| ApiKey | BatchCheckAvailabilityRequest | Your unique API KEY | Mandatory |
| ResellerId | BatchCheckAvailabilityRequest | | Mandatory |
| SupplierId | BatchCheckAvailabilityRequest | String representing the unique supplier ID within the Zaui Marketplace | Mandatory |
| ExternalReference | BatchCheckAvailabilityRequest | String representing a unique transaction ID. Used to identify your original request. | Optional |
| TimeStamp | BatchCheckAvailabilityRequest | Time of creation of request YYYY-MM-DDTHH:MM:SS.SS SZ(in utc time) or YYYY-MM-DDTHH:MM:SS.SS S[+/-]HH:MM Example: <br> 2013-04- 28T13:10:12.123Z (utc time) <br> 2013-04- 28T13:10:12.123+10:00 | Mandatory |
| StartDate | BatchCheckAvailabilityRequest | Date for which to check. Date format: YYYY-MM-DD | Mandatory |
| EndDate | BatchCheckAvailabilityRequest | Date for which to end checking. Date format: YYYY-MM-DD | Optional |
| SupplierProductCode | BatchCheckAvailabilityRequest | The unique product code for the supplier | Mandatory |


**Note:** _Requests for batch availability can be requested no further than 120 days out. Batch Availability for historical dates are not permitted.
Dates ranges between the StartDate and EndDate cannot be more then 120 days._

## Batch Check Availability Response

> Example Single Date Response

```xml
<?xml version="1.0" encoding="UTF-8"?>

<BatchCheckAvailabilityResponse  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <StartDate>2015-10-1</StartDate>
 <SupplierProductCode>ACT_1765</SupplierProductCode>
 <RequestStatus>
   <Status>SUCCESS</Status>
</RequestStatus>
<BatchTourAvailability>
  <Date>2015-10-1</Date>
  <TourOptions>
          <SupplierOptionCode>09:00:00</SupplierOptionCode>
          <SupplierOptionName>Zip Line 9:00 am</SupplierOptionName>
          <TourDepartureTime>09:00:00</TourDepartureTime>
          <TourDuration>PT1Hn</TourDuration>
   </TourOptions>
  <AvailabilityStatus>
         <Status>UNAVAILABLE</Status>
         <UnavailabilityReason>SOLD_OUT</UnavailabilityReason>
  </AvailabilityStatus>
</BatchTourAvailability>
<BatchTourAvailability>
   <Date>2015-10-1</Date>
   <TourOptions>
           <SupplierOptionCode>10:00:00</SupplierOptionCode>
           <SupplierOptionName>Zip Line 10:00 am</SupplierOptionName>
           <TourDepartureTime>10:00:00</TourDepartureTime>
           <TourDuration>PT1Hn</TourDuration>
    </TourOptions>
   <AvailabilityStatus>
           <Status>AVAILABLE</Status>
   </AvailabilityStatus>
</BatchTourAvailability>
</BatchCheckAvailabilityResponse>

```

> Example Date Range Response

```xml
<?xml version="1.0" encoding="UTF-8"?>

<BatchCheckAvailabilityResponse  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <StartDate>2015-10-1</StartDate>
 <SupplierProductCode>ACT_1765</SupplierProductCode>
 <RequestStatus>
         <Status>SUCCESS</Status>
 </RequestStatus>
 <BatchTourAvailability>
         <Date>2015-10-30</Date>
<TourOptions>
                <SupplierOptionCode>09:00:00</SupplierOptionCode>
                <SupplierOptionName>Zip line 9:00 am</SupplierOptionName>
                <TourDepartureTime>09:00:00</TourDepartureTime>
                <TourDuration>PT1Hn</TourDuration>
                </TourOptions>
        <AvailabilityStatus>
               <Status>UNAVAILABLE</Status>
               <UnavailabilityReason>SOLD_OUT</UnavailabilityReason>
        </AvailabilityStatus>
</BatchTourAvailability>
<BatchTourAvailability>
        <Date>2015-10-31</Date>
        <TourOptions>
                <SupplierOptionCode>09:00:00</SupplierOptionCode>
                <SupplierOptionName>Zip Line 9:00 am</SupplierOptionName>
                <TourDepartureTime>09:00:00</TourDepartureTime>
                <TourDuration>PT1Hn</TourDuration>
                </TourOptions>
        <AvailabilityStatus>
               <Status>UNAVAILABLE</Status>
               <UnavailabilityReason>SOLD_OUT</UnavailabilityReason>
        </AvailabilityStatus>
</BatchTourAvailability>
</BatchCheckAvailabilityResponse>
```

| XML Node | Parent Node | Description |
| -------- | ----------- | ----------- |
| BatchCheckAvailabilityResponse | | Root XML node |
| ApiKey | BatchCheckAvailabilityResponse | Your unique API KEY |
| ResellerId | BatchCheckAvailabilityResponse | |
| SupplierId | BatchCheckAvailabilityResponse | | String representing the unique supplier ID within the Zaui Marketplace |
| ExternalReference | BatchCheckAvailabilityResponse | String representing a unique transaction ID. Used to identify your original request. |
| TimeStamp | BatchCheckAvailabilityResponse | Time of creation of request YYYY-MM- DD HH:MM:SS.SSSZ(in utc time) or YYYY-MM-DD HH:MM:SS.SSS[+/- ]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04- 28T13:10:12.123+10:00 |
| RequestStatus | BatchCheckAvailabilityResponse | Request status root XML element |
| Status | RequestStatus | Status value for the request. Values are: <br> **SUCCESS** <br> **ERROR** |
| Error | RequestStatus | Root node for the error details on a non- successful request |
| ErrorCode | Error | String with the error code |
| ErrorMessage | Error | String with the error message |
| ErrorDetails | Error | String with additional details, and recommendation on error |
| BatchTourAvailability | BatchCheckAvailabilityResponse | Availability root node. |
| Date | BatchTourAvailability | Date for which the availability is supplied in the format YYYY- MM-DD |
| SupplierProductCode | BatchTourAvailability | Unique supplier product code |
| TourOptions | BatchTourAvailability | Tour options root node |
| SupplierOptionCode | TourOptions | String containing the unique supplier option code |
| TourDepartureTime | TourOptions | Time of the tour departure, in the format HH:MM:SS |
| TourDuration | TourOptions | Duration for the tour option, in the format of **PnYnMnDTnHnMnS**. Example: PT300S (5 minutes).|
| AvailabilityStatus | BatchTourAvailability | Root element for availability status |
| Status | AvailabilityStatus | Status of the availability request. Values are: <br> **AVAILABLE** - when available for the given Date and TravellerMix. <br> **UNAVAILABLE** - when not available for the given Date and TravellerMix. |
| UnavailabilityReason | AvailabilityStatus | Reason why the requested activity is NOT available. Valid values: <br> **SOLD_OUT** <br> **BLOCKED_OUT** - when the product (Tour)/ product option (Tour Option) has been blocked out (not taking place on this date). <br> **INACTIVE** - when the product (Tour)/ product option (Tour Option) is no longer active. Recommend a ActivityListRequest to refresh the supplier product codes <br> **PAST_CUTOFF_DATE** - when the booking cut-off date has been reach for this product (Tour)/ product option (Tour Option). <br> **TRAVELLER_MISMATCH** - when the required combination of travellers for this product (Tour)/ product option (Tour Option) is not met. |
| TravellerMixAvailability | BatchTourAvailability | Guest mix availability root element |
| Senior | TravellerMixAvailability | TRUE or FALSE |
| Adult | TravellerMixAvailability| TRUE or FALSE |
| Student | TravellerMixAvailability | TRUE or FALSE|
| Child | TravellerMixAvailability | TRUE or FALSE|
| Infant | TravellerMixAvailability | TRUE or FALSE |

## Booking Create Response

> Example Booking Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<BookingCreateResponse  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
<Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <RequestStatus>
    <Status>SUCCESS</Status>
 </RequestStatus>
 <BookingReference>999999999</BookingReference>
 <TransactionStatus>
    <Status>CONFIRMED</Status>
 </TransactionStatus>
 <SupplierConfirmationNumber>1635213</SupplierConfirmationNumber>
</BookingCreateResponse>
```

| XML Node | Parent Node | Description |
| -------- | ----------- | ----------- |
| BookingCreateResponse | | Root node |
| ApiKey | BookingCreateResponse | Your unique API KEY |
| ResellerId | BookingCreateResponse | |
| SupplierId | BookingCreateResponse | |
| ExternalReference | BookingCreateResponse | String representing a unique transaction ID. Used to identify your original request. |
| TimeStamp | BookingCreateResponse | Time of creation of request YYYY-MM- DD HH:MM:SS.SSSZ(in utc time) or YYYY-MM-DD HH:MM:SS.SSS[+/- ]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04- 28T13:10:12.123+10:00 |
| RequestStatus | BookingCreateResponse | Request status root XML element |
| Status | RequestStatus | Status value for the request. Values are: <br> **SUCCESS** <br> **ERROR** |
| Error | RequestStatus | Root node for the error details on a non- successful request |
| ErrorCode | Error | String with the error code |
| ErrorMessage | Error | String with the error message |
| ErrorDetails | Error | String with additional details, and recommendation on error |
| BookingReference | BookingCreateResponse | String containing the unique booking reference from your system |
| TransactionStatus | BookingCreateResponse | Root node that holds data on the transaction |
| Status | TransactionStatus | Status of the transition. Options are: <br> **CONFIRMED** <br> **REJECTED** |
| RejectionReasonDetails | TransactionStatus | Extended details on the transaction rejection |
| RejectionReason | TransactionStatus | Reason transaction was rejected. Options are: <br> **NOT_OPERATING** - Tour is not operating on the date for which the booking was made. <br> **OTHER** - Any other reason. Details must be provided in Rejection- ReasonDetails. |
| SupplierConfirmationNumber | BookingCreateResponse | String holding the unique supplier confirmation number. This number must be used on subsequent booking API calls, such as amend, cancel. |
| SupplierConfirmationPromotionCode| BookingCreateResponse | Auto generated promotion code, that provides an online redemption mechanism with the Suppliers system |

## Booking Update Request

> Example Update Request

```xml
<?xml version="1.0" encoding="UTF-8"?>

<BookingUpdateRequest  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
  <BookingReference>999999999</BookingReference>
  <TravelDate>2015-12-10</TravelDate>
  <SupplierProductCode>ACT_1765</SupplierProductCode>
  <TourOptions>
    <SupplierOptionCode>09:00:00</SupplierOptionCode>
    <SupplierOptionName>Zip line 9:00 am</SupplierOptionName>
    <TourDepartureTime>09:00:00</TourDepartureTime>
    <TourDuration>PT1H </TourDuration>
  </TourOptions>
  <Inclusions>
    <Inclusion>Bottle of Champagne</Inclusion>
<Inclusion>Hotel Pickup</Inclusion>
</Inclusions>
<Traveller>
  <TravellerIdentifier>1</TravellerIdentifier>
  <TravellerTitle>Mr.</TravellerTitle>
  <GivenName>Phil</GivenName>
  <Surname>Dover</Surname>
  <AgeBand>ADULT</AgeBand>
  <LeadTraveller>true</LeadTraveller>
</Traveller>
<TravellerMix>
  <Senior>0</Senior>
    <Adult>1</Adult>
   <Child>0</Child>
   <Student>0</Student>
   <Infant>0</Infant>
   <Total>1</Total>
 </TravellerMix>
<RequiredInfo>
 <Question>
   <QuestionText>Passport No.</QuestionText>
   <QuestionText>L99999</QuestionText>
 </Question>
</RequiredInfo>
<SpecialRequirement>Vegetarian Meal</SpecialRequirement>
<PickupPoint>Hilton</PickupPoint>
<SupplierNote>Change to number of travellers. Customer reimbursed.</SupplierNote>
<AdditionalRemarks>
         <Remark>Additional charges for large luggage may apply. To be advised at pickup.</Remark>
 </AdditionalRemarks>
 <ContactDetail>
    <ContactType>MOBILE</ContactType>
    <ContactName>Phil Dover</ContactName>
    <ContactValue>US+1 999999999</ContactValue>
 </ContactDetail>
<PickupLocation>
         <SupplierPickupCode>209</SupplierPickupCode>
 </PickupLocation>
 <DropoffLocation>
         <SupplierDropoffCode>219</SupplierDropoffCode>
 </DropoffLocation>
 <SupplierConfirmationNumber>125467</SupplierConfirmationNumber>
</BookingUpdateRequest>
```

The booking amendment request allows you to submit amendments to existing bookings created through your implementation in the supplier system. To request a booking to be changed, your request must submit through a valid booking reference number, and that booking must have been originally created through your implementation. Each booking amendment is for a single booking only.

| XML Node | Parent Node | Description | Optionality |
| -------- | ----------- | ----------- | ----------- |
| BookingUpdateRequest | | Root XML node | Mandatory |
| ApiKey | BookingUpdateRequest | Your unique API KEY | Mandatory |
| ResellerId | BookingUpdateRequest| | Mandatory |
| SupplierId | BookingUpdateRequest | String representing the unique supplier ID within the Zaui Marketplace | Mandatory |
| ExternalReference | BookingUpdateRequest | String representing a unique transaction ID. Used to identify your original request. | Optional |
| TimeStamp | BookingUpdateRequest | Time of creation of request YYYY-MM-DD HH:MM:SS.SSSZ(in utc time) or YYYY-MM-DD HH:MM:SS.SSS[+/- ]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04- 28T13:10:12.123+10:00 | Mandatory |
| BookingReference | BookingUpdateRequest | Unique booking reference | Mandatory |
| TravelDate | BookingUpdateRequest | Date of travel for the itinerary item. Date must be in the format YYYY-MM-DD | Mandatory |
| SupplierProductCode | BookingUpdateRequest | The unique product code for the supplier | Mandatory |
| TourOptions | BookingUpdateRequest | Root tour options XML node | Optional |
| SupplierOptionCode | TourOptions | Unique option code | Optional |
| SupplierOptionName | TourOptions| | Optional |
| TourDepartureTime | TourOptions | Time of activity departure. Must be in the format of HH:MM:SS | Optional |
| TourDuration | TourOptions | Duration of activity option. Must be in the format of **PnYnMnDTnHnMnS**. | Optional |
| Traveller | BookingUpdateRequest | Root XML element describing the customer details | Mandatory |
| travellerIdentifier | Traveller | Unique integer per traveller for the booking | Mandatory |
| GivenName | Traveller | Guest first name | Mandatory |
| Surname | Traveller | Guest last name | Mandatory |
| AgeBand | Traveller | Age band for the traveller. Valid values are: <br> • Senior <br> • Adult <br> • Student <br> • Child <br> • Infant | Mandatory |
| TravellerMix | BookingUpdateRequest | Root XML element describing the number of travellers | Optional |
| Senior | TravellerMix | | Optional |
| Adult | TravellerMix | | Optional |
| Student| TravellerMix| | Optional |
| Child | TravellerMix | | Optional |
| Infant | TravellerMix | | Optional |
| Total | TravellerMix | | Optional |
| RequiredInfo | BookingUpdateRequest | Root XML node that will hold additional booking questions/answers for the booking | Optional |
| Question | RequiredInfo | String holding the question | Optional |
| QuestionText | RequiredInfo | String holding the question text | Optional |
| QuestionAnswer | RequiredInfo | Answer text supplied | Optional |
| SpecialRequirement | BookingUpdateRequest | String wit any special requirements from the customer | Optional |
| PickupPoint | BookingUpdateRequest | String value with the pickup point supplied | Optional |
| SupplierNote | BookingUpdateRequest | String value with any supplier notes passed from your implementation | Optional |
| AdditionalRemarks | BookingUpdateRequest| Root XML node with additional remarks | Optional |
| Remark | AdditionalRemarks | String with the remark | Optional |
| ContactDetail | BookingUpdateRequest | Root XML node holding customer contact details | Optional |
| ContactType | ContactDetail | Type of contact. Options are: <br> • MOBILE <br> • EMAIL <br> • ALTERNATE <br> • NOT_CONTACTABLE | Optional |
| ContactName | ContactDetail | String with name of contact | Optional |
| ContactValue | ContactDetail | String with the contact information (name, email, phone..etc) | Optional |
| PickupLocation | BookingRequest | Root node for the pickup location | |
| SupplierPickupCode | PickupLocation | The pickup location ID, which is returned as part of the ActivityListRequest | Optional, but required for all transportation based supplier product |
| DropoffLocation | BookingRequest | Root node for the dropoff location | |
| SupplierDropoffCode | DropoffLocation | The dropoff location ID | Optional but required for all transportation based supplier product |
| SupplierConfirmationNumber | BookingUpdateRequest | String that holds the unique supplier confirmation number. The SupplierConfirmationNumber is used in all subsequent request pertaining to he booking, and must be stored in your implementation. | Mandatory |

## Booking Update Response


> Example Booking Update Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<BookingUpdateResponse  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <RequestStatus>
    <Status>SUCCESS</Status>
 </RequestStatus>
 <BookingReference>999999999</BookingReference>
 <TransactionStatus>
    <Status>CONFIRMED</Status>
 </TransactionStatus>
 <SupplierConfirmationNumber>1235458</SupplierConfirmationNumber>
</BookingUpdateResponse>
```

| XML Node | Parent Node | Description |
| -------- | ----------- | ----------- |
| BookingUpdateResponse | | Root XML node |
| ApiKey | BookingUpdateResponse | Your unique API KEY |
| ResellerId | BookingUpdateResponse | |
| SupplierId | BookingUpdateResponse | |
| ExternalReference | BookingUpdateResponse | String representing a unique transaction ID. Used to identify your original request. |
| TimeStamp | BookingUpdateResponse | Time of creation of request YYYY-MM- DD HH:MM:SS.SSSZ(in utc time) or YYYY-MM-DD HH:MM:SS.SSS[+/- ]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04- 28T13:10:12.123+10:00 |
| RequestStatus | BookingUpdateResponse | Request status root XML element |
| Status | RequestStatus | Status value for the request. Values are: <br> **SUCCESS**<br> **ERROR** |
| Error | RequestStatus | Root node for the error details on a non-successful request |
| ErrorCode | Error | String with the error code |
| ErrorMessage | Error | String with the error message |
| ErrorDetails | Error | String with additional details, and recommendation on error |
| BookingReference | BookingUpdateResponse | String containing the unique booking reference from your system |
| TransactionStatus | BookingUpdateResponse | Root node that holds data on the transaction |
| Status | TransactionStatus | Status of the transition. Options are: <br>**CONFIRMED**<br>**REJECTED** |
| RejectionReasonDetails | TransactionStatus | Extended details on the transaction rejection |
| RejectionReason | TransactionStatus | Reason transaction was rejected. Options are: <br> **NOT_OPERATING** – Activity not offered on requested date <br> **OTHER** - Any other reason. Details must be provided in RejectionReasonDetails. |
| SupplierConfirmationNumber | BookingUpdateResponse | String value of the unique supplier confirmation number. This supplier confirmation number is used in all subsequent booking requests to reference the supplier booking number. <br> **NOTE:** the supplier confirmation number must be the number of the original booking request. |

## Booking Cancel Request

> Example Booking Cancel Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<BookingUpdateResponse  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <RequestStatus>
    <Status>SUCCESS</Status>
 </RequestStatus>
 <BookingReference>999999999</BookingReference>
 <TransactionStatus>
    <Status>CONFIRMED</Status>
 </TransactionStatus>
 <SupplierConfirmationNumber>1235458</SupplierConfirmationNumber>
</BookingUpdateResponse>
```

The booking cancellation request allows your implementation to cancel previously made bookings, successfully created through your implementation, directly into the supplier system. The API allows cancellation of the booking.

| XML Node | Parent Node | Description | Optionality |
| -------- | ----------- | ----------- | ----------- |
| BookingCancelRequest | | Root XML node | Mandatory |
| ApiKey | BookingCancelRequest | Your unique API KEY | Mandatory |
| ResellerId | BookingCancelRequest | | Mandatory |
| SupplierId | BookingCancelRequest | String representing the unique supplier ID within the Zaui Marketplace | Mandatory |
| ExternalReference | BookingCancelRequest | String representing a unique transaction ID. Used to identify your original request. | Optional |
| TimeStamp | BookingCancelRequest | Time of creation of request YYYY-MM- DD HH:MM:SS.SSSZ(in utc time) or YYYY-MM-DD HH:MM:SS.SSS[+/- ]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04- 28T13:10:12.123+10:00 | Mandatory |
| BookingReference | BookingCancelRequest | Unique booking reference | Mandatory |
| SupplierConfimationNumber | BookingCancelRequest | Supplier confirmation number | Mandatory |
| CancelDate | BookingCancelRequest | Date the cancellation was made. Must be in the format YYYY-MM- DD
| Mandatory |
| Author | BookingCancelRequest | String holding the name of person whom made the cancellation | Mandatory |
| Reason | BookingCancelRequest | String holding the reason for cancellation | Mandatory |
| SupplierNote | BookingCancelRequest | String holding any additional notes about the cancellation | Mandatory |

## Booking Cancel Response

> Example Booking Cancel Response

```xml
<?xml version="1.0" encoding="UTF-8"?>

<BookingCancelResponse  xmlns="https://api.zaui.io/api/01">
 <ApiKey>cdqu60CykKeca1Qc000VXwgchV000L2fNOOf0bv9gPp</ApiKey>
 <ResellerId>2005</ResellerId>
 <SupplierId>200</SupplierId>
 <ExternalReference>10051374722992616</ExternalReference>
 <Timestamp>2015-07-25T13:29:52.616+10:00</Timestamp>
 <RequestStatus>
   <Status>SUCCESS</Status>
 </RequestStatus>
 <BookingReference>999999999</BookingReference>
 <SupplierConfirmationNumber>1236578</SupplierConfirmationNumber>
<SupplierCancellationNumber>CANCEL78910</SupplierCancellationNumber>
 <TransactionStatus>
   <Status>CONFIRMED</Status>
 </TransactionStatus>
</BookingCancelResponse>
```
| XML Node | Parent Node | Description |
| -------- | ----------- | ----------- |
| BookingCancelResponse | | Root node |
| ApiKey | BookingCancelResponse | Your unique API KEY |
| ResellerId | BookingCancelResponse | |
| SupplierId | BookingCancelResponse | |
| ExternalReference | BookingCancelResponse | String representing a unique transaction ID. Used to identify your original request. |
| TimeStamp | BookingCancelResponse | Time of creation of request YYYY-MM- DD HH:MM:SS.SSSZ(in utc time) or YYYY-MM-DD HH:MM:SS.SSS[+/- ]HH:MM Example: 2013-04-28T13:10:12.123Z (utc time) 2013-04- 28T13:10:12.123+10:00 |
| RequestStatus | BookingCancelResponse | Request status root XML element |
| Status | RequestStatus | Status value for the request. Values are: <br> **SUCCESS**<br>**ERROR** |
| Error | RequestStatus | Root node for the error details on a non-successful request |
| ErrorCode | Error | String with the error code |
| ErrorMessage | Error | String with the error message |
| ErrorDetails | Error | String with additional details, and recommendation on error |
| BookingReference | BookingCancelResponse | String containing the unique booking reference from your system. |
| SupplierConfirmationNumber | BookingCancelResponse| String holding the original unique supplier confirmation number.  |
| SupplierCancellationNumber | BookingCancelResponse | String holding the unique cancellation number |
| TransactionStatus | BookingCancelResponse | Root node that holds data on the transaction |
| Status | TransactionStatus | Status of the transition. Options are:<br>**CONFIRMED**<br> **REJECTED** |
| RejectionReasonDetails | TransactionStatus | Extended details on the transaction rejection |
| RejectionReason | TransactionStatus | Reason transaction was rejected. Options are: <br> **PAST_CANCEL_DATE** - Allowed cancellation date is in the past. <br> **PAST_TOUR_DATE** - Tour date is in the past.<br> **TOUR_REDEEMED** - Tour has al- ready been redeemed.<br> **OTHER** - Any other reason. Details must be provided in RejectionReasonDetails. |

# HTTP Status Codes

| Code | Reason | Description | Recommended Action |
| ---- | ------ | ----------- | ------------------ |
| 200 | Status OK | Request executed without error | None |
| 400 | Bad Request | Invalid Request | Check for malformed XML |
| 401 | Unauthorized | Authentication for the request has failed | Check the required authentication requirements. |
| 404 | Not Found | Requested API routine is not an implemented method | Check the naming of the inbound request |
| 503 | Unavailable | Throttle rate limit has been exceeded | Retry using exponential backoff. You need to slow down the rate at which you are sending requests. |

# Legal

## LEGAL STATEMENT

This guide, in addition, to the software described within, is under the copyright owned by Zaui Software Limited and subject to license. This guide and any included software, whole or in part, cannot be published, downloaded, or reproduced, transmitted, transferred or combined with any other material or be used for any other purpose without written permission from Zaui Software Limited.


## NOTICE OF NON-LIABILITY

Every effort has been made to ensure the accuracy of information published in this guide. However, Zaui Software cannot accept any responsibility for any errors, inaccuracies, or omission that may or may not be published in this guide.

Zaui Software is providing the information in this guide to you “AS-IS” with all faults. Zaui software makes no warranties of any kind (whether express, implied or statutory) with respect to the information contained herein. Zaui Software assumes no liability for damages (whether direct or indirect), caused by errors or omissions, or resulting from the use of this document or the information contained in this document or resulting from the application or use of the product or service described herein. Zaui Software Limited reserves the right to make changes to any information herein within prior notice or consent.
