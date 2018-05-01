---
title: Zaui IO API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - activityrequests

search: true
---

# Introduction

Zaui.io is an XML API based messaging platform designed for resellers, developers or anyone wanting access to real-time inventory from any suppliers within the Zaui supplier market place.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# API Throttle
To ensure continuous quality of service, API usage can be subjected to throttling. The throttle will be applied once an API client reaches a certain threshold.
Zaui Software reserves the right to throttle any and all API clients to ensure quality of service for all Zaui Software customers.
Those clients who do encounter a throttle threshold will get met with a HTTP 503 response code and the error XML node populated.
We encourage all API developers to anticipate this error and take appropriate measures like:
• using a cached value from a previous call, or
• passing on a message to the end user that is subjected to this behavior (if any)
• implement exponential back-off in your logic (see below) Before starting, anyone organization wishing to use this API, must have been provided authentication credentials.

# Authentication Credentials

All API calls are authenticated. To obtain your login credentials, please visit www.zaui.io, use our automated reseller sign up to obtain your login credentials. Credentials will be your reseller, or developer, ID and your authentication token, and destination supplier ID.

Credentials for the API can be obtained by signing up, for free, on our website: https://api.zaui.io/signup

# Test Supplier System

During your development and testing phases, and prior to connecting to live suppliers, you are welcome
to use the test supplier ID for all your API calls: Test supplier ID = 200
This supplier has various tours and activities available for you to integrate your test environment with.
Should you need to see a specific activity type in this system, which is not present, please contact our
support staff at support@zaui.com


# Submitting to the API

All API request to the Zaui.io platform must be sent using an HTTPS POST
using the following URI:

> All API access is over HTTPS and accessed from the https://api.zaui.io/v1/ domain.

```XML
$ curl -ki https://api.zaui.io/v1/
HTTP/1.1 200 OK
Date: Wed, 07 Jan 2015 00:12:51 GMT Server: Apache
X-Powered-By: PHP/5.4.35
Vary: Accept-Encoding Content-Length: 411
Content-Type: text/xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<version xmlns="http://www.zaui.io/api" xmlns:atom="http://www.w3.org/2005/Atom" id="v1" statu
s="CURRENT" >
   <media-types>
      <media-type base="application/xml" type="application/xml;version=v1"/>
   </media-types>
   <atom:link rel="self" href="https://api.zaui.io/v1"/>
   <atom:link rel="describedby" href="http://www.zaui.io/api/v1"/>
</version>
```


Each response consists of an object with the following properties:
• HTTP response status code: The status code of the HTTP response. This does match the header
value of the response, unless suppress_response_codes=true was sent (see below). For a list of
    suppress_response_codes=true

valid response codes, see the appendix at the close of this document. • XML data payload: The result data will always return XML

### Suppressing HTTP Response codes
Certain technologies either don’t handle or can’t handle HTTP response codes. More specifically, they
don’t handle any non 200 HTTP status codes. For this result our API provides provisioning for this. In
this case, your implementation can send through the suppress_response_codes with the value true .
This will always request a status code of 200 in the response header. The original, and true, status code
is then available within the response body.

### Implementing Exponential Back-off
Exponential back-off is the process of a client periodically retrying a failed request over an increasing amount of time. It is a standard error handling strategy for network applications. Besides being “re- quired”, using exponential back-off increases the efficiency of bandwidth usage, reduces the number of requests required to get successful response, and maximizes the throughput of requests in concurrent environments.
The flow of implementing a simple exponential back-off is as follows:
1. Make a request to the API
2. Receive the response, check for error that has a retry-able error code (such as 503)
3. Wait 1s + random_number_milliseconds seconds
4. Retry request
5. Receive the response, check for error that has a retry-able error code (such as 503)
6. Wait 2s + random_number_milliseconds seconds
7. Retry request
8. Receive the response, check for error that has a retry-able error code (such as 503)
9. Wait   seconds
10. Retry request
11. Receive the response, check for error that has a retry-able error code (such as 503)
12. Wait 8s + random_number_milliseconds seconds
13. Retry request
14. Receive the response, check for error that has a retry-able error code (such as 503)
15. Wait 16s + random_number_milliseconds seconds
16. Retry request
17. If you still get an error, stop and log the error

*Note: random_number_milliseonds MUST be redefined after each “Wait”*

In the above flow,
random_number_milliseconds
is a random number of milliseconds less than or equal to 1000. This is necessary to avoid certain lock errors in some concurrent implementations.

*Note: the wait is always (2^n) + random_number_milliseconds, where n is a mono-tonically increasing integer initially defined as 0. N is incremented by 1 for each iteration (each request)*

# API Function Definitions

## Required Authentication Variables
While we have outlined each of the request and response API calls, its important to know that you must
submit through your ResellerID, destination SupplierId and APIkey with every API request to our systems.

## Activity List Requests
The ActivityListRequest enables pulling a list of available activities and options from the specified supplier. The activity list will include descriptive fields, options, and the specific activity value, to be used on subsequent API calls where a single activity is requested by your implementation.

It is the responsibility of any integration to store the unique activity codes for further API calls, where its required.


*Note: your implementation must store the returned activity codes for future calls,
where required. The ActivityListRequest should be called periodically, since the
supplier can update the available activities on the Zaui IO channels anytime, poten-
tially making your stored activity codes stale.*

*Note: Depending on the number of allowed activities that the supplier your com- municating with has provided for you, the data size on this call back can be large.
As a result, it’s not uncommon to see this callback having a large data set. This call can often take up to 30 seconds to complete.*

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

# Legal Statements

This guide, in addition, to the software described within, is under the copyright owned by Zaui Software Limited and subject to license. This guide and any included software, whole or in part, cannot be published, downloaded, or reproduced, transmitted, transferred or combined with any other material or be used for any other purpose without written permission from Zaui Software Limited.

Every effort has been made to ensure the accuracy of information published in this guide. However, Zaui Software cannot accept any responsibility for any errors, inaccuracies, or omission that may or may not be published in this guide.
Zaui Software is providing the information in this guide to you “AS-IS” with all faults. Zaui software makes no warranties of any kind (whether express, implied or statutory) with respect to the information contained herein. Zaui Software assumes no liability for damages (whether direct or indirect), caused by errors or omissions, or resulting from the use of this document or the information contained in this document or resulting from the application or use of the product or service described herein. Zaui Software Limited reserves the right to make changes to any information herein within prior notice or consent.
