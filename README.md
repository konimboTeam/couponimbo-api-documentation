# Couponimbo API Documentation

This docs provide with more information how to use Couponimbo API.

# REST API

The REST API to the example app is described below.

* [CAMPAIGNS](#user-content-campaigns)
* [COUPONS](#user-content-coupons)

## Authorization - API Key
The authorization header will be automatically generated when you send the request. Learn more about [API Key](https://learning.postman.com/docs/sending-requests/authorization/authorization-types/#api-key)
 authorization.

![Alt text](/screenshots/auth-apikey.png)

# CAMPAIGNS

## Endpoints
* [GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns](#user-content-get-list-of-campaign)
* [POST /dev/couponimbo/api/v1/store/{subdomain}/campaigns](#user-content-create-a-new-campaign)
* [GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}](#user-content-cet-a-specific-campaign)
* [PUT /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}/active](#user-content-put-an-active-or-deactive-campaign)

## Get list of `Campaign`

### Request

`GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns`

    curl -i -H 'Accept: application/json' http://localhost:3000/dev/couponimbo/api/v1/store/mortest/campaigns

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:30 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 2

    [
        {
            "id":"Cwhx",
            "name":"dev - api",
            "active":true,
            "amount":5,
            "type":"%",
            "numberOfCoupons":999,
            "maxUsage":1,
            "additionalDiscountAllowed":false,
            "bonusDiscount":0,
            "groupId":0,
            "groupType":"All",
            "minimumCartPrice":0,
            "startDate":null,
            "endDate":null,
            "totalUsage":0,
            "testCoupon":"eAWOt4Pr-Cwhx",
            "createdAt":"2024-01-21T13:00:13.180Z"
        }
    ]

## Create a new `Campaign`

### Table Fields
Name | Type | Defualt | Required | Description | Example
:--|:--:|:--:|:--:|:---|:---:
name                        | string                |       | Yes | name of campaign | `black friday campaign`
type                        | string literal        |       | Yes | discount type, precentage or nis only. | `%` or `₪`
amount                      | number                |       | Yes | discount amount. | `5`
additionalDiscountAllowed   | boolean               | false | No  | defualt value is `false`.<br>if `true` will alllow additional discount. | `true`
minimumCartPrice            | number                |   0   | No  | defualt value is `0`.<br>otherwise, discount will apply only if cart price<br>is greater or equal to minimum cart price | `350` 
maxUsage                    | number                |   1   | No  | the maximum usage per `Coupon`. | `5`
bonusDiscount               | number                |   0   | No  | add bonus discount in nis<br>for all coupons in campaign. | `20`
startDate                   | date Time (ISO-8601)  |  null | No  | start date of campaign. | `2024-01-21T13:00:13.180Z`
endDate                     | date Time (ISO-8601)  |  null | No  | end date of campaign.   | `2024-01-23T13:00:13.180Z`
groupType                   | string literal        |  All  | No  | discount by group type.<br>All = products<br>Tag = products<br>Category = products | `All` or `Tag` or `Category`
groupId                     | number                |   0   | No  | the group id<br>Required<br>in case of `groupType` is 'Tag' or 'Category' | `123123`
numberOfCoupons             | number                |   1   | No  | the number of coupon that generate from the system<br>in case of `couponPrefix`<br>it means you choose the code of the coupon<br>the number of coupon in this campaign will be `1` | `999`
couponPrefix                | string                |  ''   | No  | a coupon name that you will detremine<br>if value is `not` empty<br> `numberOfCoupons` will be `1`<br>otherwise, the system will generate<br>generics custom coupon codes | `testcoupon2024`
charactersType              | number                |   0   | No  | a coupon characters type<br>will generate coupon code<br>with letters & numbers or numbers only<br>0 = letters & numbers<br>1 = numbers only | `0` | `0` or `1`
imported                    | boolean               |       | No  | in case of creating `customCodes` coupons, send `true` | `true`
customCodes                 | Array<string>         |       | No  | if imported is `true`<br>Required to send array of string<br>represent the codes coupon<br>must be unique's code<br>with minimum 4 digits<br>if there is a code that already exists<br>the campaign will not create. | `["1111","2222","3332"]`

### Request

`POST /dev/couponimbo/api/v1/store/{subdomain}/campaigns`

    curl -i -H 'Accept: application/json' -d 'name=black friday campaign&type=%&amount=5&numberOfCoupons=999' http://localhost:3000//dev/couponimbo/api/v1/store/mortest/campaigns

### Response

    HTTP/1.1 201 Created
    Date: Thu, 24 Feb 2011 12:36:30 GMT
    Status: 201 Created
    Connection: close
    Content-Type: application/json
    Location: /thing/1
    Content-Length: 36

    {
        "id":"Cwhx"
    }

## Get a specific `Campaign`

### Request

`GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}`

    curl -i -H 'Accept: application/json' http://localhost:3000/dev/couponimbo/api/v1/store/mortest/campaigns/Cwhx

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:30 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 36

        {
            "id":"Cwhx",
            "name":"dev - api",
            "active":true,
            "amount":5,
            "type":"%",
            "numberOfCoupons":999,
            "maxUsage":1,
            "additionalDiscountAllowed":false,
            "bonusDiscount":0,
            "groupId":0,
            "groupType":"All",
            "minimumCartPrice":0,
            "startDate":null,
            "endDate":null,
            "totalUsage":0,
            "testCoupon":"eAWOt4Pr-Cwhx",
            "createdAt":"2024-01-21T13:00:13.180Z"
        }

## Put an active or deactive `campaign`

### Request

`PUT /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}/active`

    curl -i -H 'Accept: application/json' -d 'active=false' http://localhost:3000/dev/couponimbo/api/v1/store/mortest/campaigns/l_nG/active

### Response

    HTTP/1.1 404 Not Found
    Date: Thu, 24 Feb 2011 12:36:30 GMT
    Status: 404 Not Found
    Connection: close
    Content-Type: application/json
    Content-Length: 35

    {
        "id": "Cwhx",
        "active": false
    }

# COUPONS

## Endpoints
* [GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}/coupons](#user-content-get-list-of-coupons)

## Get list of `Coupons`

### Request

`GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}/coupons`

    curl -i -H 'Accept: application/json' http://localhost:3000/dev/couponimbo/api/v1/store/mortest/campaigns/RUgi/coupons

### Response

    HTTP/1.1 200 OK
    Date: Thu, 24 Feb 2011 12:36:30 GMT
    Status: 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 2

{
    "status": true,
    "data": [
        {
            "Code": "testcoupontag",
            "UsesLeft": 1
        }
    ]
}
