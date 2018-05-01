# Activity List Response

<aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside>

Zaui API Activity List Response

|XML Node|Parent Node|Description|
|---|---|---|
|ActivityListResponse|   |Root XML node|
|ApiKey|ActivityListResponse|Your unique API KEY|
|ResellerId|ActivityListResponse|Your unique reseller ID|
|SupplierId|ActivityListResponse|String representing the unique supplier ID within the Zaui Marketplace|
|ExternalReference|ActivityListResponse|String representing a unique transaction ID. Used to identify your original request.|
|TimeStamp|ActivityListResponse|Time of creation of request|
|   |   |●yyyy-MM-ddTHH:mm:ss.SSSZ(in utc time) or ●yyyy-MM-ddTHH:mm:ss.SSS[+/-]hh:mm
Example:
2013-04-28T13:10:12.123Z (utc time) 2013-04-28T13:10:12.123+10:00|
|RequestStatus|ActivityListResponse|Request status root XML node, which holds details about the status|
|Status|RequestStatus|Status of the request.
●SUCCESS for a successful transaction, or
●ERROR for unsuccessful transaction. Error XML node will be provided|
|Error|RequestStatus|Error root XML node|
|ErrorCode|Error|The error code|
|ErrorMessage|Error|The error code|
|ErrorDetails|Error|String for the error message|
|Activity|ActivityListResponse|Root activity XML node|
|SupplierProductCode|Activity|Suppliers unique product code. This will be used across multiple API calls and should be stored as part of your implementation.|
|SupplierProductName|Activity|String that contains the supplier activity name|
|TourDescription|Activity|Description of the activity as provided by the supplier.|
|TourDepartureTime|Activity|Time of the activity. Values will be in the format of HH:MM:SS|
|TourDuration|Activity|Duration of the activity. Format will be *PnYnMnDTnHnMnS*. Example PT300s (5 mins)|
|Tour Option|Activity|Root XML node that holds details for options on the activity.|
|SupplierOptionCode|TourOption|Duration of the activity option. Duration format *PnYnMnDTnHnMnS*|
|PickupLocation|Activity|   |
|SupplierPickupCode|PickupLocation|Supplier pickup ID code|
|SupplierPickupName|PickupLocation|Supplier pickup location name as a string|
|DropoffLocation|Activity|   |
|SupplierDropoffCode|DropoffLocation|Supplier dropoff ID code|
|SupplierDropoffName|DropoffLocation|Suplier dropoff location name as a string   |
