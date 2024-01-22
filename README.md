# Couponimbo API Documentation

This docs provide with more information how to use Couponimbo API.

# REST API

The REST API to the example app is described below.

# CAMPAIGNS

## Endpoints
### EndPoints
* [GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns](#user-content-Get-list-of-`Campaign`)
* [POST /dev/couponimbo/api/v1/store/{subdomain}/campaigns](#user-content-Create-a-new-`Campaign`)
* [GET /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}](#user-content-Get-a-specific-`Campaign`)
* [PUT /dev/couponimbo/api/v1/store/{subdomain}/campaigns/{campaignId}/active](#user-content-Put-an-active-or-deactive-`campaign`)

## Fields Details
Name | Type | Defualt | Required | Description | Example
:--|:--|:---|---:|---:|---:
name                        | string                |       | Yes | name of campaign | `black friday campaign`
type                        | string literal        |       | Yes | discount type, precentage or nis only. | `'%' | '₪'`
amount                      | number                |       | Yes | discount amount. | `5`
additionalDiscountAllowed   | boolean               | false | No  | defualt value is `false`, if `true` will alllow additional discount. | `true`
minimumCartPrice            | number                |   0   | No  | defualt value is `0`, otherwise, discount will apply only if cart price is greater or equal to minimum cart price | `350`  

maxUsage                    | number                |   1   | No  | the maximum usage per `Coupon`. | `5`
bonusDiscount               | number                |   0   | No  | add bonus discount in nis per each `Coupon` in campaign. | `20`
startDate                   | date Time (ISO-8601)  |  null | No  | start date of campaign. | `2024-01-21T13:00:13.180Z`
endDate                     | date Time (ISO-8601)  |  null | No  | end date of campaign.   | `2024-01-23T13:00:13.180Z`
groupType                   | string literal        |  All  | No  | discount by group type, All = products, Tag = products, Category = products | `'All' | 'Tag' | 'Category'`
groupId                     | number                |   0   | No  | the group id, Required in case of `groupType` is 'Tag' or 'Category' | `123123`
numberOfCoupons             | number                |   1   | No  | the number of coupon that generate from the system, in case of `couponPrefix`, it means you choose the code of the coupon, the number of coupon in this campaign will be `1` | `999`

couponPrefix                | string                |  ''   | No  | a coupon name that you will detremine, if value is not empty the `numberOfCoupons` will be `1`, otherwise the system will generate generic custom coupon codes | `testcoupon2024`

charactersType              | number                |   0   | No  | a coupon characters type, will generate coupon code with letter & numbers or numbers only, 0 = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789', 1 = '0123456789' | `0` | `1`

imported                    | boolean               |       | No  | in case of creating `customCodes` coupons, send `true` | `true`
customCodes                 | Array<string>         |       | No  | if imported is `true`, Required to send array of string with code coupons, must be unique's code, if there is a code that already exists, the campaign will not create.

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
