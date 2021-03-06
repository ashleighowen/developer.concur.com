---
title: Budget Header
layout: reference
---

{% include prerelease.html %}

## Menu

* [Getting Started](./getting-started.html)
* [Fiscal Year](/api-reference/budget/fiscal-year.html)
* [Budget Category](/api-reference/budget/budget-category.html)
* [Budget Item](/api-reference/budget/budget-header.html)
* [Budget Tracking Field](/api-reference/budget/budget-tracking.html)
* [Budget Adjustments](/api-reference/budget/budget-adjustments.html)

# Budget Item

This resource is used to retrieve and update information about a budget that spans a single fiscal year.  Each Budget has multiple details that correspond to Fiscal Periods--months, quarters, or a single period for a yearly budget.

* [Overview](#overview)
* [GET all Budget Items](#getall)
* [GET a Budget Item](#get)
* [POST a Budget Item](#post)
* [DELETE a Budget Item](#delete)
* [Schema](#schema)
  * [Budget Item Header List](#budgetItemHeaderList)
  * [Budget Item Header](#budgetItemHeader)
  * [Budget Item Detail](#budgetItemDetail)
  * [Budget Amounts](#budgetAmounts)
  * [BudgetPerson](#budgetPerson)
  * [Budget Category](#budgetCategory)
  * [Expense Type](#expenseType)
  * [Budget Tracking Value](#budgetTrackingValue)
  * [Budget Item Balance](#budgetItemBalance)
  * [Fiscal Year](#fiscalYear)
  * [Fiscal Period](#fiscalPeriod)
  * [Budget Item Response](#budgetItemResponse)
  * [Error Response](#errorResponse)
  * [Error Message](#errorMessage)

## Version

4.0  

## <a name="getall"></a>GET all Budget Items

Retrieve all budget items in groups of up to 50 items.  Due to response size and performance considerations, this endpoint does not return budgetItemDetails.  Use the [get request below](#get) to retrieve all fo the details for a single budget.

### Scopes

Name | Description
---|---
`budgetitem.write`|Create/update/delete access to budget data
`budgetitem.read`|Read access to budget data

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)

#### Parameters

Name | Type | Format | Description
-----|------|--------|------------			
adminView	|	`boolean`	|	`query`	|	If true, returns all budgets for this entity, if false, returns only the budgets owned by the current user.  Defaults to false.
offset	|	`integer`	|	`query`	|	The start of the page offset.  Defaults to zero.

##### URI Template

```shell
GET /budget/v4/budgetItemHeader
```

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) Successful call, response is in body.
* [400 Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1) The request was determined to be invalid by the server.
* [403 Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3) The user does not have the necessary permissions to perform the request.
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) Error message in response body
* [504 Gateway Timeout](https://tools.ietf.org/html/rfc7231#section-6.6.5) Error message in response body

#### Headers

* `concur-correlationid` (Optional) is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)


#### Payload

[Paged Budget Item Header List](#budgetItemHeaderList)

### Example

#### Request

```http
GET https://us.api.concursolutions.com/budget/v4/budgetItemHeader
Authorization: Bearer: {YOUR ACCESS TOKEN}
Accept: application/json
```

#### Response
```http
HTTP/1.1 200 OK
Cache-Control: max-age=604800
Content-Type: application/json
Date: Wed, 06 Jul 2020 17:33:03 GMT
Etag: "359670651"
Expires: Wed, 13 Jul 2020 17:33:03 GMT
Last-Modified: Fri, 09 Aug 2020 23:54:35 GMT
Content-Length: 1270
concur-correlationid: dd6cee88-b725-4c06-9ee9-0ca4ae4f16b2
```

```json
{
  "totalRows":122,"offset":0,"limit":50,
  "budgetItemHeaders":[
    {
        "name":"Marketing-US-Jean Normandy",
        "isTest":false,
        "budgetItemStatusType":"OPEN",
        "description":"Marketing-US",
        "id":"72eee673-3d81-49c2-966a-b63c7a9302e6",
        "costObjects":[
          {"code":"6",
          "value":"2",
          "listKey":"1334",
          "operator":"EQUAL"}
        ],
        "periodType":"YEARLY",
        "active":true,
        "owned":false,
        "budgetAmounts":{
          "pendingAmount":1178.37697091,
          "unExpensedAmount":2310.73578092,
          "spendAmount":35.78378912,
          "availableAmount":8785.83923997,
          "unExpensedSettings":null,
          "threshold":"UNDER",
          "consumedPercent":12
        },
        "currencyCode":"EUR",
        "annualBudget":10000.00000000,
        "budgetCategory":{
          "name":"airfare",
          "description":null,
          "statusType":"OPEN",
          "id":"27451c2d-9121-44bd-b4b0-f2119d2071c7"
        },
        "owner":{
          "externalUserCUUID":"8002250890004822936",
          "employeeUuid":"210fe25f-e326-495c-847a-de333173f616",
          "id":"f779261d-77ce-4123-b739-d842ef6f104d",
          "name":"Jean Normandy",
    	  "email":"jean.normandy@xyz.com"
         },
        "budgetApprovers":[
	      {
            "externalUserCUUID":"8002250890004822936",
            "employeeUuid":"210fe25f-e326-495c-847a-de333173f616",
            "id":"f779261d-77ce-4123-b739-d842ef6f104d",
            "name":"Jean Normandy",
        	"email":"jean.normandy@xyz.com"
          }
        ],
        "budgetManagers":[
	      {
            "externalUserCUUID":"1563846384638464842",
            "employeeUuid":"13a13839-68d6-4ee8-90e9-58604278aa8f",
            "id":"e2bae688-e000-464a-8728-e1362c94f172",
            "name":"Walter Gupta",
        	"email":"walter.gupta@xyz.com"
          }
        ],
        "budgetViewers":[
          {
            "externalUserCUUID":"5005380230004873464",
            "employeeUuid":"eb6082b0-3a9a-4e79-a350-e6e067f34969",
            "id":"7ce7dfe0-6168-4b93-bb35-386bf023acc6",
            "name":"Dan Lee",
	        "email":"dan.lee@xyz.com"
          }
        ],
        "fiscalYear":{
          "name":"2017",
          "startDate":"2017-01-01",
          "endDate":"2017-12-31",
          "status":"OPEN",
          "id":"a4f9d57f-14ac-4f03-b5aa-4256e5cff790",
          "lastModified":"2017-03-26 20:53:19",
          "currentYear":false
        }
      },
    {"Additional budget item headers removed for brevity":"Additional budget items headers removed for brevity"}
  ],
  "href":"https://us.api.concursolutions.com/budget/v1.1/budgetItemHeader/?offset=0",
  "next":{
    "href":"http://budget-service-rqa3-budget.us-west-2.nonprod.cnqr.delivery/budget/v1.1/budgetItemHeader/?adminView=true&offset=50"
  },
  "previous":null
}
```

## <a name="get"></a>GET a Budget Item

Retreive the details of a single budget item.

### Scopes

Name | Description
---|---
`budgetitem.write`|Create/update/delete access to budget data
`budgetitem.read`|Read access to budget data

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)

#### Parameters

Name | Type | Format | Description
-----|------|--------|------------			
id	|	`string`	|	`path`	|	The budget item header's key field (sync guid).


##### URI Template

```shell
GET  /budget/v4/budgetItemHeader/{id}
```
### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) Successful call, response is in body.
* [400 Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1) The request was determined to be invalid by the server.
* [403 Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3) The user does not have the necessary permissions to perform the request.
* [404 Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) The resource could not be found or does not exist
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) Error message in response body
* [504 Gateway Timeout](https://tools.ietf.org/html/rfc7231#section-6.6.5) Error message in response body

#### Headers

* `concur-correlationid` (Optional) is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)


#### Payload

[Budget Item Header](#budgetItemHeader)

### Example

#### Request

```http
GET https://us.api.concursolutions.com/budget/v4/budgetItemHeader/{id}
Authorization: Bearer: {YOUR ACCESS TOKEN}
```

#### Response

```http
HTTP/1.1 200 OK
Cache-Control: max-age=604800
Content-Type: application/json
Date: Wed, 06 Jul 2020 17:33:03 GMT
Etag: "359670651"
Expires: Wed, 13 Jul 2020 17:33:03 GMT
Last-Modified: Fri, 09 Aug 2020 23:54:35 GMT
Content-Length: 1270
concur-correlationid: 918cfb55-06a1-47da-8ef1-774a45427af9
```

```json
  {
    "name":"Marketing-US-Jean Normandy",
    "isTest":false,
    "budgetItemStatusType":"OPEN",
    "description":"Marketing-US",
    "id":"72eee673-3d81-49c2-966a-b63c7a9302e6",
    "costObjects":[
      {"code":"6",
      "value":"2",
      "listKey":"1334",
      "operator":"EQUAL"}
    ],
    "periodType":"QUARTERLY",
    "active":true,
    "owned":false,
    "budgetAmounts":{
      "pendingAmount":6870.48165307,
      "unExpensedAmount":102126.89000000,
      "spendAmount":764.86966050,
      "availableAmount":2364.64868643,
      "unExpensedSettings":null,
      "threshold":"UNDER",
      "consumedPercent":76
    },
    "currencyCode":"EUR",
    "annualBudget":10000.00000000,
    "budgetCategory":null,
    "owner":{
      "externalUserCUUID":"8002250890004822936",
      "employeeUuid":"210fe25f-e326-495c-847a-de333173f616",
      "id":"f779261d-77ce-4123-b739-d842ef6f104d",
      "name":"Jean Normandy"
     },
    "budgetApprovers":[
       {
        "externalUserCUUID":"8002250890004822936",
        "employeeUuid":"210fe25f-e326-495c-847a-de333173f616",
        "id":"f779261d-77ce-4123-b739-d842ef6f104d",
        "name":"Jean Normandy",
        "email":"jean.normandy@xyz.com"
       }
    ],
    "budgetManagers":[
      {
        "externalUserCUUID":"1563846384638464842",
        "employeeUuid":"13a13839-68d6-4ee8-90e9-58604278aa8f",
        "id":"e2bae688-e000-464a-8728-e1362c94f172",
        "name":"Walter Gupta",
        "email":"walter.gupta@xyz.com"
      }
    ],
    "budgetViewers":[
      {
        "externalUserCUUID":"5005380230004873464",
        "employeeUuid":"eb6082b0-3a9a-4e79-a350-e6e067f34969",
        "id":"7ce7dfe0-6168-4b93-bb35-386bf023acc6",
        "name":"Dan Lee",
	"email":"dan.lee@xyz.com"
      }
    ],
    "budgetItemDetails":[
      {
        "currencyCode":"USD",
        "amount":2500.00000000,
        "id":"4c165d40-804f-4aaa-b900-a46538537f6a",
        "budgetItemDetailStatusType":"OPEN",
        "budgetAmounts":{
          "pendingAmount":6870.48165307,
          "unExpensedAmount":102126.89000000,
          "spendAmount":764.86966050,
          "availableAmount":-5135.35131357,
          "unExpensedSettings":null,
          "consumedPercent":305,
          "threshold":"OVER"
        },
        "fiscalPeriod":{
          "name":"2017 - Q1",
          "fiscalPeriodStatus":"OPEN",
          "id":"b9659f8a-4e74-4531-9e23-1222ab1507f2",
          "periodType":"QUARTERLY",
          "startDate":"2017-01-01",
          "endDate":"2017-03-31",
          "spendDate":null,
          "fiscalYearId":"bcb41c95-2d53-4a1a-830f-7c6b01fa79da",
          "currentPeriod":false
        },
        "budgetItemBalances":[
          {
            "featureTypeCode":"PURCHASE_REQUEST",
            "featureTypeSubCode":"NONE",
            "workflowState":"SUBMITTED",
            "amount":6870.48165307,
            "id":"11cb732e-cbc4-41cb-82be-162d632d5499"
          },
          {
            "featureTypeCode":"EXPENSE",
            "featureTypeSubCode":"NONE",
            "workflowState":"PAID",
            "amount":764.86966050,
            "id":"0f09cc65-b879-4969-a8a1-9dd52c96486d"
          },
          {
            "featureTypeCode":"EXPENSE",
            "featureTypeSubCode":"ERECEIPTS",
            "workflowState":"UNSUBMITTED",
            "amount":102126.89000000,
            "id":"27c49c8a-c24d-42eb-b089-84268350ae03"
          }
        ]
      },
      {
        "currencyCode":"EUR",
        "amount":2500.00000000,
        "id":"0a2dc181-389e-4c85-bb57-e4f1a11ace4e",
        "budgetItemDetailStatusType":"OPEN",
        "budgetAmounts":{
          "pendingAmount":0,
          "unExpensedAmount":0,
          "spendAmount":0,
          "availableAmount":2500.00000000,
          "unExpensedSettings":null,
          "consumedPercent":0,
          "threshold":"UNDER"
        },
        "fiscalPeriod":{
          "name":"2017 - Q2",
          "fiscalPeriodStatus":"OPEN",
          "id":"590d4e22-40be-43cc-ac1b-01b0d0263e19",
          "periodType":"QUARTERLY",
          "startDate":"2017-04-01",
          "endDate":"2017-06-30",
          "spendDate":null,
          "fiscalYearId":"bcb41c95-2d53-4a1a-830f-7c6b01fa79da",
          "currentPeriod":true
        },
        "budgetItemBalances":[]
      },
      {
        "Additional budget item details removed for brevity": "Additional budget item details removed for brevity"
      }
    ],
    "fiscalYear":{
      "name":"2017",
      "startDate":"2017-01-01",
      "endDate":"2017-12-31",
      "status":"OPEN",
      "id":"a4f9d57f-14ac-4f03-b5aa-4256e5cff790",
      "lastModified":"2017-03-26 20:53:19",
      "currentYear":false
    }
  }
```

## <a name="post"></a>POST a Budget Item

Save a new budget or update an existing budget

### Scopes

Name | Description
---|---
`budgetitem.write`|Create/update/delete access to budget data

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)

#### Parameters

N/A

##### URI Template

```http
POST  /budget/v4/budgetItemHeader
```

#### Payload

[Budget Item Header](#budgetItemHeader)

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) Successful call, response is in body.
* [400 Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1) The request was determined to be invalid by the server. Possibly a validation failed on the data that was sent in the payload. The response will have a list of validation errors in the body. See below for an example 400 response.
* [403 Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3) The user does not have the necessary permissions to perform the request.
* [404 Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) The resource could not be found or does not exist
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) Error message in response body
* [504 Gateway Timeout](https://tools.ietf.org/html/rfc7231#section-6.6.5) Error message in response body

#### Headers

* `concur-correlationid` (Optional) is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)

#### Payload

[Budget Item Response](#budgetItemResponse) or [Error Response](#errorResponse)

### Example

#### Request
```http
GET https://us.api.concursolutions.com/budget/v4/budgetItemHeader
Authorization: Bearer: {YOUR ACCESS TOKEN}
```

```json
  {
    "name":"Marketing-US-Jean Normandy",
    "isTest":false,
    "budgetItemStatusType":"OPEN",
    "description":"Marketing-US",
    "id":"72eee673-3d81-49c2-966a-b63c7a9302e6",
    "costObjects":[
      {"code":"6",
      "value":"2",
      "listKey":"1334",
      "operator":"EQUAL"}
    ],
    "periodType":"QUARTERLY",
    "currencyCode":"EUR",
    "budgetCategory":{
      "id":"27451c2d-9121-44bd-b4b0-f2119d2071c7"
    },
    "owner":{
      "externalUserCUUID":"8002250890004822936",
      "employeeUuid":"210fe25f-e326-495c-847a-de333173f616",
      "email":"jean.normandy@xyz.com"
     },
    "budgetApprovers":[
      {
        "externalUserCUUID":"5005380230004873464",
        "employeeUuid":"eb6082b0-3a9a-4e79-a350-e6e067f34969",
	    "email":"dan.lee@xyz.com"
      }
    ],
    "budgetManagers":[
      {
        "externalUserCUUID":"1563846384638464842",
        "employeeUuid":"13a13839-68d6-4ee8-90e9-58604278aa8f",
        "email":"walter.gupta@xyz.com"
      }
    ],
    "budgetViewers":[
      {
        "externalUserCUUID":"5005380230004873464",
        "employeeUuid":"eb6082b0-3a9a-4e79-a350-e6e067f34969",
        "email":"dan.lee@xyz.com"
      }
    ],
    "budgetItemDetails":[
      {
        "currencyCode":"EUR",
        "amount":2500.00000000,
        "id":"4c165d40-804f-4aaa-b900-a46538537f6a",
        "budgetItemDetailStatusType":"OPEN",
        "fiscalPeriod":{
          "id":"b9659f8a-4e74-4531-9e23-1222ab1507f2"
        }
      },
      {
        "currencyCode":"EUR",
        "amount":2500.00000000,
        "id":"0a2dc181-389e-4c85-bb57-e4f1a11ace4e",
        "budgetItemDetailStatusType":"OPEN",
        "fiscalPeriod":{
          "id":"590d4e22-40be-43cc-ac1b-01b0d0263e19"
        }
      },
      {
        "currencyCode":"EUR",
        "amount":2500.00000000,
        "id":"35d7dc8a-5ec8-4d5f-ba7c-d9304f7afee3",
        "budgetItemDetailStatusType":"OPEN",
        "fiscalPeriod":{
          "id":"09cd5be1-a21d-47f2-b6b5-8d9019709327"
        }
      },
      {
        "currencyCode":"EUR",
        "amount":2500.00000000,
        "id":"4ec30f7c-e7fa-4832-9134-85bed9a85b9c",
        "budgetItemDetailStatusType":"OPEN",
        "fiscalPeriod":{
          "id":"c3beec03-a096-4a33-b7af-b49127742702"
        }
      }
    ],
    "fiscalYear":{
      "id":"a4f9d57f-14ac-4f03-b5aa-4256e5cff790"
    }
  }
```

#### Response

##### Success Response

```http
HTTP/1.1 200 OK
Cache-Control: max-age=604800
Content-Type: application/json
Date: Wed, 06 Jul 2020 17:33:03 GMT
Etag: "359670651"
Expires: Wed, 13 Jul 2020 17:33:03 GMT
Last-Modified: Fri, 09 Aug 2020 23:54:35 GMT
Content-Length: 97
concur-correlationid: 809a0898-e523-4114-950d-bd22705a3b25
```

```json
  {
    "success": true,
    "budgetItemHeaderId": "72eee673-3d81-49c2-966a-b63c7a9302e6"
  }
```

##### Failure Response

```shell
HTTP/1.1 400 Bad Request
Cache-Control: no-store
Connection: close
Content-Length: 459
Content-Type: application/json;charset=utf-8
Date: Fri, 21 Sep 2018 15:27:05 GMT
Expires: Thu, 20 Sep 2018 15:27:05 GMT
Pragma: no-cache
concur-correlationid: cea62849-02e5-4a7f-a576-68280c84bd02
```


```json
{
  "status" : false,
  "errorMessageList" :
  [
    {"errorType" : "ERROR", "errorCode" : "BUDGET.BUDGET_ITEM_NAME_REQUIRED", "errorMessage" : "Budget item name is required"},
    {"errorType" : "ERROR", "errorCode" : "BUDGET.BUDGET_ITEM_NAME_ERROR", "errorMessage" : "Budget item name should be more than 1 characters"},
    {"errorType" : "ERROR", "errorCode" : "BUDGET.BUDGET_ITEM_OWNER_REQUIRED", "errorMessage" : "Budget item owner is required"}
  ]
}
```

## <a name="delete"></a>DELETE a Budget Item Header

Delete a budget item.

### Scopes

Name | Description
---|---
`budgetitem.write`|Create/update/delete access to budget data

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)

#### <a name="parameters"></a>Parameters

Name | Type | Format | Description
-----|------|--------|------------
id	|	`string`	|	`path`	|	The budget item header's key field (sync guid).

##### URI Template

```http
DELETE  /budget/v4/budgetItemHeader/{id}
```

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) Successful call, response is in body.
* [400 Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1) The request was determined to be invalid by the server.
* [403 Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3) The user does not have the necessary permissions to perform the request.
* [404 Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4) The resource could not be found or does not exist
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1) Error message in response body
* [504 Gateway Timeout](https://tools.ietf.org/html/rfc7231#section-6.6.5) Error message in response body

#### Headers

* `concur-correlationid` (Optional) is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)

#### Payload

[Budget Item Response](#budgetItemResponse) or [Error Response](#errorResponse)

### Example

#### Request
```http
DELETE https://us.api.concursolutions.com/budget/v4/budgetItemHeader/{id}
Authorization: Bearer: {YOUR ACCESS TOKEN}
```

### Response

```shell
HTTP/1.1 200 OK
Cache-Control: no-store
Connection: keep-alive
Content-Length: 97
Content-Type: application/json;charset=utf-8
Date: Fri, 21 Sep 2018 15:24:27 GMT
Expires: Thu, 20 Sep 2018 15:24:27 GMT
Pragma: no-cache
Vary: Origin
concur-correlationid: 86a0d9fe-9e98-43c3-89d8-a2917dd844cb
```

```json
  {
    "success": true,
    "budgetItemHeaderId": "72eee673-3d81-49c2-966a-b63c7a9302e6"
  }

```

## <a name="schema"></a>Schema

### <a name="budgetItemHeaderList"></a>PagedBudgetItemHeaderList

Name | Type | Format | Description
-----|------|--------|------------
`totalRows`	|	`integer`	|	-	|	The total number of rows available
`offset`	|	`integer`	|	-	|	The starting row for this page of results (zero-based)
`limit`	|	`integer`	|	-	|	The number of results returned per page.  (maximum 50)
`budgetItemHeaders`	|	`Array[BudgetItemHeader]`	|	-	|	List of budget items
`href`	|	`string`	|	-	|	The href for this request
`previous`	|	`integer`	|	-	|	The href for the previous page of results (null if this is the first page of results)
`next`	|	`integer`	|	-	|	The href for the next page of results (null if no results remaining)


### <a name="budgetItemHeader"></a>BudgetItemHeader

Name | Type | Format | Description
-----|------|--------|------------
`active`	|	`boolean`	|	-	|	Indicates if this budget should be displayed on user screens **READ ONLY**
`annualBudget`	|	`decimal`	|	-	|	The total budget amount and accumulated balances for this budget header. **READ ONLY**
`budgetAmounts`	|	`Array[BudgetAmounts]`	|	-	|	The accumulated budget amounts for this budget.  **READ ONLY**
`budgetManagers`	|	`Array[BudgetPerson]`	|	-	|	Manager Hierarchy only.
`budgetApprovers`	|	`Array[BudgetPerson]`	|	-	|	The workflow approvers for this budget.
`budgetCategory`	|	`BudgetCategory`	|	-	|	The budget category for this budget item.  If a budget category is present, spending items must match one of the expense types in the budget category in order to match this budget.
`budgetItemDetails`	|	`Array[BudgetItemDetail]`	|	-	|	**Required** Specify the budget information for each fiscal period in the fiscal year.
`budgetItemStatusType`	|	`string`	|	-	|	The status of this budget item. Valid values are 'OPEN', 'CLOSED', and 'REMOVED' (Closed means no spending may be attached to this budget.)
`budgetViewers`	|	`Array[BudgetPerson]`	|	-	|	The users who can view this budget
`costObjects`	|	`Array[CostObjectValue]`	|	-	|	The cost object list for matching spending items.
`currencyCode`	|	`string`	|	-	|	The 3-letter ISO 4217 currency code for the expense report currency. Examples: USD - US dollars; BRL - Brazilian real; CAD - Canadian dollar; CHF - Swiss franc; EUR - Euro; GBO - Pound sterling; HKD - Hong Kong dollar; INR - Indian rupee; MXN - Mexican peso; NOK - Norwegian krone; SEK - Swedish krona. This is the currencycode of the budget amount. Spending Items are converted using yesterday's closing value.
`description`	|	`string`	|	-	|	**Required** The user-friendly name for this budget. This description is displayed to end users on desktop and mobile.
`fiscalYear`	|	`FiscalYear`	|	-	|	**Required** The fiscal year for this budget.  Only the sync_guid is technically required for creating/updating a budget.
`isTest`	|	`boolean`	|	-	|	The test flag for the budget item.  If true, this budget will only match spending submitted by test users.
`name`	|	`string`	|	-	|	**Required** The admin-facing name for this budget.
`owned`	|	`string`	|	-	|	A flag indicating if the current user is the owner of this budget.  **READ ONLY**
`owner`	|	`BudgetPerson`	|	-	|	**Required** The user who is ultimately responsible for this budget.  He/she may approve spending for the budget.
`periodType`	|	`string`	|	-	|	The type of period within the fiscal year that this budget's details use. **READ ONLY**
`id`	|	`string`	|	-	|	The key for this object.


### <a name="budgetItemDetail"></a>BudgetItemDetail

Name | Type | Format | Description
-----|------|--------|------------
`amount`	|	`decimal`	|	-	|	**Required** The budget currency amount allocated to this fiscal period.  May be zero.
`budgetAmounts`	|	`Array[BudgetAmounts]`	|	-	|	The accumulated budget numbers for this budget.  **READ ONLY**
`budgetItemBalances`	|	`Array[BudgetItemBalance]`	|	-	|	Shows the break-out of budget spending by product and workflow state.  **READ ONLY**
`budgetItemDetailStatusType`	|	`string`	|	-	|	The status of this budget item. Valid values are 'OPEN', 'CLOSED', and 'REMOVED' (Closed means no spending may be attached to this budget.)
`currencyCode`	|	`string`	|	-	|	The 3-letter ISO 4217 currency code for the expense report currency. Examples: USD - US dollars; BRL - Brazilian real; CAD - Canadian dollar; CHF - Swiss franc; EUR - Euro; GBO - Pound sterling; HKD - Hong Kong dollar; INR - Indian rupee; MXN - Mexican peso; NOK - Norwegian krone; SEK - Swedish krona.
`fiscalPeriod`	|	`FiscalPeriod`	|	-	|	**Required** The fiscal period for this budget amount.  Only the sync_guid is technically required for creating/updating a budget.
`id`	|	`string`	|	-	|	The key for this object.


### <a name="budgetAmounts"></a>BudgetAmounts

Name | Type | Format | Description
-----|------|--------|------------
`availableAmount`	|	`decimal`	|	-	|	The available amount accumulated against this budget. Uses budget entity setting to determine which amounts are included to calculate available amount. **READ ONLY**
`consumedPercent`	|	`decimal`	|	-	|	The percentage of the budget that is considered used. Uses budget entity setting to determine which amounts to include. **READ ONLY**
`pendingAmount`	|	`decimal`	|	-	|	The pending amount accumulated against this budget. **READ ONLY**
`spendAmount`	|	`decimal`	|	-	|	The spent amount accumulated against this budget. **READ ONLY**
`threshold`	|	`string`	|	-	|	The indicator of whether this budget is under the alert limit (UNDER), equal to or over the alert limit, but under the control limit (ALERT), or equal to or over the control limit (OVER).  **READ ONLY**
`unexpensedAmount`  |   `decimal`   |    -  |   The amount of unexpensed items like travel bookings, quick expenses, or e-receipts **READ ONLY**
`unexpensedSettings`    |   `string`    |   -   |   An indicator for whether this company has an special setting for unexpensed items.  Example values: SHOW_UNSUBMITTED_EXPENSES_AS_PENDING, SHOW_UNSUBMITTED_EXPENSES_BALANCE, and DO_NOT_SHOW_UNSUBMITTED_EXPENSES **READ ONLY**

### <a name="budgetPerson"></a>BudgetPerson

Provide externalUserCUUID or email of the user for looking up the person.

Name | Type | Format | Description
-----|------|--------|------------
`externalUserCUUID`	|	`string`	|	-	|	The unique identifier for this user. This must match the CUUID from Concur's profile service.
`name`	|	`string`	|	-	|	The user's name.  Provided for convenience.  **READ ONLY**
`id`	|	`string`	|	-	|	The budget service's key for this object.
`email`	|	`string`	|	-	|	The email address of the person to lookup.


### <a name="budgetCategory"></a>BudgetCategory

Name | Type | Format | Description
-----|------|--------|------------
`description`	|	`string`	|	-	|	Not used.
`name`	|	`string`	|	-	|	 The admin-facing name for this category.
`statusType`	|	`string`	|	-	|	The status of this budget category. Valid values are 'OPEN' and 'REMOVED'
`id`	|	`string`	|	-	|	The budget service's key for this object.


### <a name="expenseType"></a>ExpenseType

Name | Type | Format | Description
-----|------|--------|------------
`featureTypeCode`	|	`string`	|	-	|	**Required** The type of feature that this expense type applies to, Purchase Request, Payment Request (Invoice), Expense or Travel Authorization (Possible values: 'REQUEST', 'TRAVEL', 'EXPENSE', 'PAYMENT_REQUEST', 'PURCHASE_REQUEST')
`expenseTypeCode`	|	`string`	|	-	|	**Required** The alphanumeric code that describes an expense type.  Ex: TRAVEL, AC_CATER Any string may be used, but only expense type codes returned by GET /budgetCategory/expenseType will behave properly in the Concur UI.  
`name`	|	`string`	|	-	|	The name for this expense type if it maps to an expense type set up in Concur. **READ ONLY**
`id`	|	`string`	|	-	|	The budget service's key for this object.


### <a name="budgetTrackingValue"></a>CostObjectValue

Name | Type | Format | Description
-----|------|--------|------------
`code`	|	`string`	|	-	|	**Required** The code for the cost object field.
`value`	|	`string`	|	-	|	The value for the cost object field. Blank or null mean that we
`listKey`	|	`string`	|	-	|	When setting up the budget, specify the listKey that maps to the value of this list in the concur list service.


### <a name="budgetItemBalance"></a>BudgetItemBalance

Name | Type | Format | Description
-----|------|--------|------------
`amount`	|	`decimal`	|	-	|	The balance amount. **READ ONLY**
`featureTypeCode`	|	`string`	|	-	|	The product type for this balance. Valid values are 'REQUEST', 'TRAVEL', 'EXPENSE', 'PAYMENT_REQUEST'  **READ ONLY**
`workflowState`	|	`string`	|	-	|	Valid values are 'UNSUBMITTED', 'UNSUBMITTED_HELD', 'SUBMITTED', 'APPROVED', 'PROCESSED', 'PAID' **READ ONLY**
`id`	|	`string`	|	-	|	The budget service's key for this object.


### <a name="fiscalYear"></a>FiscalYear

Name | Type | Format | Description
-----|------|--------|------------
`currentYear`	|	`boolean`	|	-	|	Is this the current fiscal year based on the current time?  **READ ONLY**
`startDate`	|	`date`	|	-	|	**Required** The start date for this fiscal year. The distance between start date and end date may not be more than two years. Format: YYYY-MM-DD
`endDate`	|	`date`	|	-	|	**Required** The end date for this fiscal year. The distance between start date and end date may not be more than two years.  Format: YYYY-MM-DD
`name`	|	`datetime`	|	-	|	**Required** The name of this fiscal year. Must be unique for this entity.
`status`	|	`string`	|	-	|	**Required** The status of this fiscal year. Valid values are 'OPEN', 'CLOSED' and 'REMOVED'
`id`	|	`string`	|	-	|	The budget service's key for this object.
`lastModified`  |   `datetime`  |   -   |   The UTC date and time when this object was last changed.  **READ ONLY**

### <a name="fiscalPeriod"></a>FiscalPeriod

Name | Type | Format | Description
-----|------|--------|------------
`currentPeriod`	|	`boolean`	|	-	|	Is this the current fiscal period based on the current time?  **READ ONLY**
`startDate`	|	`date`	|	-	|	**Required** The start date for this fiscal period. Must be within the parent fiscal year. Format: YYYY-MM-DD
`endDate`	|	`date`	|	-	|	**Required** The end date for this fiscal year. Must be within the parent fiscal year. Format: YYYY-MM-DD
`name`	|	`string`	|	-	|	**Required** The name of this fiscal period. Must be unique for this entity.
`fiscalPeriodStatus`	|	`string`	|	-	|	**Required** The status of this fiscal period. Valid values are 'OPEN', 'CLOSED' and 'REMOVED'
`periodType`  | `string`    |   -   |   **Required** The type of fiscal period.  Valid values are 'MONTHLY', 'QUARTERLY', 'YEARLY', 'CUSTOM'
`fiscalYearId`	|	`string`	|	-	|	The key of the parent fiscal year for this fiscal period.
`id`	|	`string`	|	-	|	The budget service's key for this object.
`spendDate` |   `date`  |   -   |   If the current date is after this fiscal period's start date, this field shows the current date.  **READ ONLY**

### <a name="budgetItemResponse"></a>BudgetItemResponse

Name | Type | Format | Description
-----|------|--------|------------
`success`	|	`boolean`	|	-	|
`budgetItemHeaderId`  |   `guid`    | -   |   The key of the created/updated/removed budget item header

### <a name="errorResponse"></a>Error Response

Name | Type | Format | Description
---|---|---|---
`status`|`boolean`|-|False if there was an error
`errorMessageList`|`Array[ErrorMessages]`|-|List of all errors detected

### <a name="errorMessage"></a>Error Message

Name | Type | Format | Description
---|---|---|---
`errorType`|`String`|-|WARNING or ERROR
`errorCode`|`String`|-|Text code for this error
`errorMessage`|`String`|-|Plain language error message
