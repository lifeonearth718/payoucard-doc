- [Introduction](#Introduction)
- [Basic Information](#Basic-Information)
    - [Glossary](#Glossary)
    - [Request Frequency Limit](#Request-Frequency-Limit)
    - [API Server Address](#API-Server-Address)
    - [Connection Method](#Connection-Method)
    - [API Request Parameters and Template](#API-Request-Parameters-and-Template)
    - [API Response Parameters and Template](#API-Response-Parameters-and-Template)
    - [Callback Notification Callback Parameters and Template](#Callback-Notification-Callback-Parameters-and-Template)
    - [Callback Notification Response Parameters and Template](#Callback-Notification-Response-Parameters-and-Template)
    - [Interface Authentication and Encryption and Decryption](#Interface-Authentication-and-Encryption-and-Decryption)
        - [Create RSA Public and Private Keys](#Create-RSA-Public-and-Private-Keys)
        - [Signature](#Signature)
        - [Verify Signature](#Verify-Signature)
    - [Response Code](#Response-Code)
- [Dictionary](#Dictionary)
- [REST API](#REST-API)
    - [Public API](#Public-API)
        - [Upload file](#Upload-file)
    - [Bank card](#Bank-card)
        - [Card type list](#Card-type-list)
        - [Merchant account query](#Merchant-account-query)
        - [Merchant user registration](#Merchant-user-registration)
        - [Merchant user information update](#Merchant-user-information-update)
        - [Merchant user KYC attachment upload](#Merchant-user-KYC-attachment-upload)
        - [Submit KYC certification](#Submit-KYC-certification)
        - [Query KYC certification](#Query-KYC-certification)
        - [Virtual card opening application](#Virtual-card-opening-application)
        - [Virtual card opening progress query](#Virtual-card-opening-progress-query)
        - [Bank card activation](#Bank-card-activation)
        - [Bank card recharge estimated amount query](#Bank-card-recharge-estimated-amount-query)
        - [Bank card recharge](#Bank-card-recharge)
        - [Bank card recharge order query](#Bank-card-recharge-order-query)
        - [Bank card recharge record query](#Bank-card-recharge-record-query)
        - [Bank card balance query](#Bank-card-balance-query)
        - [Bank card password retrieval](#Bank-card-password-retrieval)
        - [Bank card transaction information query](#Bank-card-transaction-information-query)
        - [Bank card freeze](#Bank-card-freeze)
        - [Bank card unfreeze](#Bank-card-unfreeze)
    - [Global remittance](#Global-remittance)
        - [Query remittance bank and related configuration](#Query-remittance-bank-and-related-configuration)
        - [Query legal currency exchange rate](#Query-legal-currency-exchange-rate)
        - [Payment verification](#Payment-verification)
        - [Payment payee verification](#Payment-payee-verification)
        - [Payment](#Payment)
        - [User order list](#User-order-list)
        - [Submit order information or file](#Submit-order-information-or-file)
        - [Query order results](#Query-order-results)
    - [Callback notification](#Callback-notification)
        - [Set callback notification url](#Set-callback-notification-url)
        - [Callback notification type](#Callback-notification-type)
        - [Global Express Remittance Payment result callback notification](#Global-Express-Remittance-Payment-result-callback-notification)
        - [Global Express Remittance Adjustment callback notification](#Global-Express-Remittance-Adjustment-callback-notification)
        - [User KYC callback notification](#User-KYC-callback-notification)
        - [Bank card callback notification of card recharge result](#Bank-card-callback-notification-of-card-recharge-result)
        - [Bank card activation result callback notification](#Bank-card-activation-result-callback-notification)
        - [Bank card freeze thaw processing status callback notification](#Bank-card-freeze-thaw-processing-status-callback-notification)
        - [Bank card 3DS verification](#Bank-card-3DS-verification)
        - [Bank card-consumption bill event](#Bank-card-consumption-bill-event)

# Introduction
Welcome to the PayouCard developer documentation. An overview of the merchant docking application development interface.

REST API includes two business categories: bank card and global remittance

# Basic Information
## Glossary
merchant: merchant

merchantId: Merchant ID. It can be viewed in the merchant's backend system - "Merchant Basic Information" menu.

## Request Frequency Limit
The request frequency limit will be added to the specific interface, please check the specific API description.

## API Server Address

**test environment:**

Server address (IP whitelist restriction): https://api-merchant-test.logtec.me

Merchant backend system (IP whitelist restriction): https://merchant-test.logtec.me

**Production environment:**

Server address (IP whitelist restriction): https://api-merchant.payoucard.com

Merchant backend system: https://merchant.payoucard.com

## Connection Method
All APIs are called using POST method. content-type: application/json format

**If an API request form is different, it will be marked in the specific API interface**

## API Request Parameters and Template


| Parameters | Type | Whether it is required | Meaning |
|------------|--------|------------|:-----------|
| requestId | String | Y | Request flow id. 20 random characters |
| merchantId | String | Y | Merchant ID. PayouCard assigned to merchants |
| data | Object | Y | Business data |
| signature | String | Y | signature |

```json
{
    "requestId": "128293243536342523",
    "merchantId": "88888888",
    "data": null,
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## API Response Parameters and Template
| Parameters | Type | Whether it is required | Meaning |
|------------|--------|------------|:-----------|
| code | Integer | Y | response code |
| message | String | Y | message |
| success | Boolean | Y | Whether it was successful. true: success; false: failure. <br />True when code=0 |
| data | Object | Y | response body |
| requestId | String | Y | Request flow id. 20 random characters |
| merchantId | String | Y | Merchant ID. PayouCard assigned to merchants |
| signature | String | Y | signature |

```JSON
{
    "code": 0,
    "message": "",
    "success": true,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## Callback Notification Callback Parameters and Template
| Parameters | Type | Whether it is required | Meaning |
|------------|--------|------------|:-----------|
| data | Object | Y | response body |
| requestId | String | Y | Request flow id. 20 random characters |
| merchantId | String | Y | Merchant ID. PayouCard assigned to merchants |
| signature | String | Y | signature |
| notifyType | Integer | Y | notification type |

## Callback Notification Response Parameters and Template
| Parameters | Type | Whether it is required | Meaning |
|------------|--------|------------|:-----------|
| code | Integer | Y | response code |
| message | String | Y | message |
```json
{
    "code": 0,
    "message": ""
}
```

## Interface Authentication and Encryption and Decryption
### Create RSA Public and Private Keys
After becoming a merchant, the partner needs to log in to the merchant's backend system. Generate RSA public key and private key in the "Merchant Basic Information" menu, and PayouCard will also provide PayouCard's public key. Merchants are required to properly keep the public key, private key, and PayouCard public key. The parameters will be used in subsequent API requests.

### Signature
requestId: request flow id. 20 random characters

merchantId: PayouCard is assigned to the merchant. It can be viewed in the "Merchant Basic Information" menu of the merchant's backend system.

data: business data.

signature: The signature source string consists of all non-empty field contents except the signature field, sorted according to the ASCII code of the message field, and connected with the "&" symbol in the manner of "field name = field value". The RSA algorithm is used for signing by default.

* For example: Query exchange rate request data
```json
{
    "requestId": "PYC20240325164529237",
    "marchant": "88888888",
    "data":{
     "currency": "EUR",
     "targetCurrency": "SGD",
     "targetCountry": "SG"
    }
}
```
The signature source string signature is as follows. The signature source string uses the merchant's RSA private key signature to generate the signature.

```
data={"targetCurrency":"SGD","currency":"EUR","targetCountry":"SG"}&merchantId=88888888&requestId=PYC20240325164529237
```
* Remark
    * The signature algorithm of RSA is SHA256withRSA.
    * The maximum encrypted plaintext length of RSA is 117.
    * The maximum decryption ciphertext length of RSA is 128.
    * The field signature in the request data is generated by the merchant's RSA private key signature.
    * The field signature in the response data is generated by the PayouCard RSA private key signature.
    * For specific implementation, please refer to payoucard-demo.zip```com.payoucard.demo.service.GlobalTransferTestService.class#main() method```

### Verify Signature
signature: generated by PayouCard RSA private key signature

* For example: Query exchange rate response data

```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": {
     "targetCurrency": "SGD",
     "exchangeRate": "1.4567",
     "currency": "EUR"
    },
    "merchantId": "88888888",
    "signature": "SCHaOIDlAkyaad4VxBrd9ON27ZrSK0IpNCPbkjQEe8YR/2UaZUlYUViBRDmnkvhJCehxjBTwECCF9vr4qjG1epyTKYVy8fqcQdynzXyclT5MYs6N9uWu5AHQm/HQpvfBjRcK3y65TaqZ dNkToREQ/XPg/PsAFpPueSugjYJ0/C0=",
    "requestId": "PYC20240325191750279"
}
```

* It is composed of all non-empty field contents except the signature field (signature), sorted according to the ASCII code of the message field, and connected with the "&" symbol in the manner of "field name = field value". Signature verification through PayouCard RSA public key

## Response Code

| code | meaning |
|------------|--------|
| 0 | Success |
| -1 | Specific business error. See the message description for the reason |
| 500 | Server error. Please contact PayouCard staff |
| 50001 | The request parameter is missing. requestId, merchantId, signature are empty |
| 50002 | Merchant does not exist |
| 50003 | API function is not available. For example, if the Global Money Transfer function is turned off, the Global Money Transfer related interfaces will be unavailable |
| 50004 | Signature verification failed |
| 50005 | The merchant has not set a public key

# Dictionary

* Business dictionary

```/src/dictionary_biz.pdf```

* Public dictionary

* Currency dictionary

* Mobile area code dictionary

* Country dictionary

* City dictionary

```/src/dictionary_common.xlsx```

* java-sdk-demo

```/src/payoucard-demo.zip```

# REST API

## Public API

### Upload file
This interface is used to upload files and get file url

**HTTP request**

POST /order/merchant/file/upload

**Request content-type:** multipart/form-data

**Frequency limit:** 100/5s

**Request parameter:**

| Parameter | Type | Required or not | Meaning |
|------------|--------|------------|:-----------|
| file | file | Y | file |
| requestId | String | Y | request flow id |
| merchantId | String | Y | merchant id |
| signature | String | Y | signature |
| data | Object | N | business data |

**Request response:**

| Parameter | Type | Required or not | Meaning |
|------------|--------|------------|:-----------|
| data | Object | Y | file path |

**Response example:**
```json
{
    "code": 0,
    "message": "",
    "success": true,
    "data": "https://asfw2sfw.cloudfront.net/media/88888/test.png",
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## Bank card

### Card type list
This interface is used to obtain all card types

**HTTP request**

POST /card/merchant/config/list

**Request parameters:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":null,
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|-------------------------------------------|---------|----------|:---------------------|
| id | Integer | Y | Card type ID |
| type | Integer | Y | Type (0-Visa virtual card 1-Visa physical card 2-MasterCard virtual card 3-MasterCard physical card) |
| typeDesc | String | Y | Type description |
| cardName | String | Y | Card name |
| cardDesc | String | Y | Card description |
| kycRequire | Boolean | Y | Whether KYC certification is required |
| status | Integer | Y | Status (0-optional 1-available) |
| rechargeCurrencyInfoList | List | Y | Rechargeable currency list configuration |
| rechargeCurrencyInfoList[0].currency | String | Y | Recharge currency |
| rechargeCurrencyInfoList[0].rechargeMinQuota | BigDecimal | Y | Minimum recharge amount |
| rechargeCurrencyInfoList[0].rechargeMaxQuota | BigDecimal | Y | Maximum recharge amount |
| rechargeCurrencyInfoList[0].digital | Long | Y | Precision |
| needPhotoForActiveCard | Boolean | Y | Is it necessary to hold a passport and bank card photo when activating the card? |
| needPhotoForOperateCard | Boolean | Y | Is it necessary to provide a user signature photo when freezing, unfreezing, or retrieving a password? |

**Response example:**

```JSON
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":[{
        "id":1,
        "type": 3,
        "typeDesc":"Master-Physical Card",
        "cardName":"master Physical Card",
        "kycRequire":true,
        "status": 1,
        "rechargeCurrencyInfoList":[
            {
            "currency": "USDT",
            "rechargeMinQuota": 10,
            "rechargeMaxQuota": 100000,
            "digital": 2
            }
        ],
        "needPhotoForActiveCard" : false,
        "needPhotoForOperateCard" : false
    }],
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Merchant Account Query
This interface is used to query the merchant's account balance information

**HTTP request**

POST /user/merchant/account/query

**Request example:**

```JSON
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{},
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|-------------------------------------------|---------|----------|:---------------------|
| currency | String | Y | Currency |
| balance | BigDecimal | Y | Balance |

**Response example:**
```JSON
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":[{
        "currency": "EUR",
        "balance": 0
       },
       {
        "currency": "USDT",
        "balance": 9989936.2
    }],
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888", 
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa" 
} 
```

### Merchant User Registration
This interface is used for merchant user registration. The registered information will be used to submit the KYC application to the bank; <br>
1. The application for a virtual card requires a mobile phone number, mobile phone international area code, email, surname, first name and address <br>
2. All information for the application of a physical card needs to be provided, and special cards need to provide a handheld ID number <br>

Requirements for submitting KYC in the test environment: <br>
id==3 or id==9: mobile phone number, mobile phone area code, email, surname, first name and address <br>
id==1 or id == 10: All fields in the registration interface cannot be empty <br>
id==2: All fields in the registration interface are required, Upload a photo of yourself holding your ID card and add it through the extended field extField <br>

id == 1 The country of application must be a member of the European Union, otherwise it is not supported <br>
EEA countries refer to EEA countries in dictionary_common.xlsx <br>

**It is recommended to register all cards with a passport**

Production environment ID to be provided

**HTTP request**

POST /user/merchant/register

**Request parameters:**

| Parameter | Type | Required | Meaning |
|-------------|--------|----------|:--------------------------------------------------------------|
| uniqueId | String | Y | Unique ID of the partner user |
| areaCode | String | Y | International mobile area code. Example: +86 |
| mobile | String | Y | Mobile phone number |
| email | String | Y | Email address |
| lastName | String | Y | Last name (can only be in English, one English space is supported between words, and the length cannot exceed 64) |
| firstName | String | Y | First name (can only be in English, one English space is supported between words, and the length cannot exceed 64) |
| birthday | String | N | Birthday (yyyy-MM-dd) |
| nationality | String | N | Nationality. 2-digit code. See dictionary_common.xlsx (sheet. regin) |
| | | | |
| sex | String | N | Gender (1: male, 2: female) |
| country | String | N | Country code. 2-digit code. See dictionary_common.xlsx (sheet. regin) |
| town | String | N | City code. See dictionary_common.xlsx (sheet. city) |
| address | String | Y | Detailed address (English address, length cannot exceed 100) |
| postCode | String |N | Postal code |
| idType | String | N | ID type. See dictionary_biz.pdf (1.1. Idno Type). |
| idNo | String | N | ID number |
| idNoExpiryDate | String | N | ID expiration date (yyyy-MM-dd) |
| idPicture | String | N | ID photo (URL format). Please call the interface /order/merchant/file/upload to obtain the url |
| facePicture | String | N | Face photo (URL format). Please call the interface /order/merchant/file/upload to get the url |
| extField   | String | N        | Json format({"eeaFileProof":"http://baidu.com/photo.jpg","handheldPhoto":"http://baidu.com/handheldPhoto.jpg","signPhoto":"http://baidu.com/signPhoto.jpg"})	                                                            |

**Request example:**

```JSON
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId":"U898IPO8KH65445F",
        "mobile": "+8617800001111",
        "email": "123@qq.com",
        "lastName": "wang",
        "firstName": "william",
        "birthday": "2002-01-01",
        "nationality": "CN",
        "sex": 1,
        "countryCode": "CN",
        "town": "CN_11",
        "address": "bei jing shi hai dian qu 123 hao",
        "postCode": "100000", 
        "idnoType": "PASSPORT", 
        "idno": "ES2324523", 
        "idNoExpiryDate":"2050-11-01", 
        "idPicture": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png", 
        "facePicture": "https://waefdf23asdf.cloudfront. net/media/88888888/test.png",
        "extField": "{\"eeaFileProof\":\"http://baidu.com/photo.jpg\",\"handheldPhoto\":\"http://baidu.com/handheldPhoto.jpg\",\"signPhoto\":\"http://baidu.com/signPhoto.jpg\"}"
    }, 
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa" 
} 
``` 

**Response example:** 
```JSON

{ 
    "code": 0, 
    "message": "success", 
    "success": true, 
    "data":null, 
    "requestId": "PYC20240325164529237", 
    "merchantId": "88888888", 
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Merchant user information update
This interface is used to update the merchant's user information

**HTTP request**

POST /user/merchant/info/update

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|-------------|--------|----------|:--------------------------------------------------------------|
| uniqueId | String | Y | Unique ID of the partner user |
| areaCode | String | Y | International mobile area code. Example: +86 |
| mobile | String | Y | Mobile phone number |
| email | String | Y | Email address |
| lastName | String | Y | Surname |
| firstName | String | Y | First name |
| birthday | String | N | Birthday (yyyy-MM-dd) |
| nationality | String | N | Nationality. 2-digit code. See dictionary_common.xlsx (sheet. regin) |
| | | | |
| sex | String | N | Gender (1: male, 2: female) |
| country | String | N | Country code. 2-digit code. See dictionary_common.xlsx (sheet. regin) |
| town | String | N | Town code. See dictionary_common.xlsx (sheet. city) |
| address | String | Y | Full address |
| postCode | String |N | Postal code |
| idType | String | N | ID type. See dictionary_biz.pdf (1.1. Idno Type). |
| idNo | String | N | ID number |
| idNoExpiryDate | String | N | ID expiration date (yyyy-MM-dd) |
| idPicture | String | N | ID photo (URL format). Please call the interface /order/merchant/file/upload to obtain the url |
| facePicture | String | N | Face photo (URL format). Please call the interface /order/merchant/file/upload to get the url |
| extField   | String | N        | Json format({"eeaFileProof":"http://baidu.com/photo.jpg","handheldPhoto":"http://baidu.com/handheldPhoto.jpg","signPhoto":"http://baidu.com/signPhoto.jpg"})	                                                            |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId":"U898IPO8KH65445F",
        "mobile": "+8617800001111",
        "email": "123@qq.com",
        "lastName": "wang",
        "firstName": "william",
        "birthday": "2002-01-01",
        "nationality": "CN",
        "sex": 1,
        "countryCode": "CN",
        "town": "CN_11",
        "address": "bei jing shi hai dian qu 123 hao",
        "postCode": "100000", 
        "idnoType": "PASSPORT", 
        "idno": "ES2324523", 
        "idNoExpiryDate":"2050-11-01", 
        "idPicture": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png", 
        "facePicture": "https://waefdf23asdf.cloudfront. net/media/88888888/test.png",
        "extField": "{\"eeaFileProof\":\"http://baidu.com/photo.jpg\",\"handheldPhoto\":\"http://baidu.com/handheldPhoto.jpg\",\"signPhoto\":\"http://baidu.com/signPhoto.jpg\"}"
     }, 
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa" 
} 
``` 

**Response example:** 

```json
{ 
    "code": 0, 
    "message": "success", 
    "success": true, 
    "data":null, 
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Merchant user KYC attachment upload
This interface is used to fill in missing KYC attachments

**HTTP request**

POST /user/merchant/kyc/attachments

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| type | Integer | Y | Type (1-EEA document proof 2-Handheld photo)data |
| data | String | Y | Data |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237", 
    "merchantId": "88888888", 
    "data": {  
      "uniqueId":"U898IPO8KH65445F", 
      "type": 2, 
      "data": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png" 
    }, 
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa" 
} 
``` 

**Response example:** 

```json
{
    "code": 0, 
    "message": "success", 
    "success": true, 
    "data":null, 
    "requestId": "PYC20240325164529237", 
    "merchantId": "88888888", 
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
} 
```

### Submit KYC certification
This interface is used for KYC certification for subsequent bank card-related business operations

Virtual card KYC generally has results within ten minutes, and you can also wait for callback through the query interface;
Physical card KYC review has results within 24;

**HTTP request**

POST /user/merchant/submit/kyc/verification

**Request parameters:**

| Parameter | Type | Is it required | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| cardTypeId | Integer | Y | Card type (obtained through the card type list interface) |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardTypeId": 1
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Query KYC certification
This interface is used to query whether KYC certification is successful

**HTTP request**

POST /user/merchant/kyc/query

**Request parameters:**

| Parameters | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| cardTypeId | Integer | Y | Card type (obtained through the card type list interface) |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardTypeId": 1
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| cardTypeId | Integer | Y | Card type ID |
| status | Integer | Y | Status (1-under review 2-success 3-rejected) |
| statusDesc | String | Y | Status description |

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "cardTypeId":1,
        "status":2,
        "statusDesc": "success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa" 
} 
```

### Virtual card opening application
HTTP request

POST /card/merchant/virtual/card/apply

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Partner's unique ID |
| cardTypeId | Integer | Y | Card type (obtained through the card type interface) |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardTypeId": 1
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| orderNo | String | Y | Order number (used to check the card opening progress) |

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
      "orderNo":"203434343434343434"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Virtual card opening progress query
**HTTP request**

POST /card/merchant/virtual/card/status

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| orderNo | String | Y | Order number |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
      "orderNo":"240123173818711000"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| cardNo | String | Y | Card number |
| expire | String | Y | Expiration time |
| cvv | String | Y | cvv |
| status | Integer | Y | Status (0-processing 1-card opening successful) |

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "cardNo":"4390789023456",
        "expire":"08/26",
        "cvv": "901",
        "status":1
    },
    "requestId": "PYC20240325164529237", 
    "merchantId": "88888888", 
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa" 
} 
```

### Bank card activation
**HTTP request**

POST /card/merchant/activation

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |
| activePhoto | String | N | Photo of holding passport and bank card (URL format). Cannot exceed 2M, supports .png, .jpeg, .jpg formats. When the user performs kyc, the card type represented by the parameter cardTypeId is only required when needPhotoForActiveCard=true. See the parameter needPhotoForActiveCard in the interface /card/merchant/config/list. Please call the interface /order/merchant/file/upload to obtain the image path |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "activePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**
| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| status | Integer | Y | Processing status. 0: Processing; 1: Success; 2: Failure. When 0 is returned, the final processing status will be sent through the activation callback notification. |

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "status": 1,
        "statusStr": "Processing"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Bank card recharge estimated amount query
This interface is used to query the estimated amount of bank card recharge

**HTTP request**

POST /card/merchant/recharge/estimationCurrency

**Request parameters:**

| Parameter | Type | Required | Meaning |
|----------|------------|----------|:-----|
| cardTypeId | Integer | Y | Card type id |
| currency | String | Y | Recharge currency |
| amount | BigDecimal | Y | Recharge amount. Cannot be less than 1 |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "cardTypeId": 10,
        "currency": "USDT",
        "amount": 20
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|----------------|------------|------|:-----------------------|
| receivedCurrency | String | Y | Received currency |
| receivedAmount | BigDecimal | N | Received amount |
| exchangeRate | BigDecimal | Y | Exchange rate |

**Response example:**

code is 0, which means the recharge is successful
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": {
        "receivedAmount": 18.15,
        "receivedCurrency": "EUR",
        "exchangeRate": 0.90768152
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```
### Bank card recharge
This interface is used to query bank card recharge

**HTTP request**

POST /card/merchant/recharge

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|------------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |
| amount | BigDecimal | Y | Recharge amount |
| currency | String | N | Currency (default USDT). For supported currencies, please refer to the rechargeCurrencyInfoList collection in the interface/card/merchant/config/list |
| orderNo | String | Y | Merchant order number |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "currency": "USDT",
        "amount": 100,
        "orderNo": "2324a2dfga3435fg34353"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response parameters:**

| Parameter | Type | Required | Meaning |
|----------------|------------|------|:-----------------------|
| cardNo | String | Y | Card number |
| status | Integer | Y | Card recharge status. 1: Success; 2: Failure; 3: Processing |
| orderNo | String | Y | Merchant order number |
| currency | String | Y | Currency |
| rechargeAmount | BigDecimal | Y | Recharge amount |
| receivedAmount | BigDecimal | N | Received amount |
| fee | BigDecimal | Y | Handling fee (money other than recharge amount) |
| msg | String | N | Error message |

**Response example:**

code is 0, which means recharge is successful
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": {
        "cardNo": "12456782323",
        "status": 3,
        "orderNo": "2324a2dfga3435fg34353",
        "currency": "USDT",
        "rechargeAmount": 100,
        "receivedAmount": 100,
        "fee": 2,
        "msg": "success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Bank card recharge order query
This interface is used to query the bank card recharge order status

**HTTP request**

POST /card/merchant/recharge/order/query

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|------------|----------|:----|
| orderNo | String | Y | Order number |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
      "orderNo": "U898IPO8KH65445F"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| orderNo | String | Y | Order number |
| cardNo | String | Y | Card number |
| currency | String | Y | Currency |
| amount | BigDecimal | Y | Recharge amount |
| fee | BigDecimal | Y | Recharge fee |
| receivedAmount | BigDecimal | Y | Amount received |
| receivedCurrency | String | Y | Currency received |
| exchangeRate | String | Y | Exchange rate |
| status | Integer | Y | Recharge status (1-success 2-failure 3-processing) |
| time | String | Y | Recharge time |

**Response example:**

```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "orderNo": "1800420978108948480",
        "cardNo": "4611990405493037",
        "currency": "USDT",
        "amount": 100,
        "fee": 2,
        "time": "2024-07-05T14:53:06",
        "receivedAmount": 99.67, 
        "receivedCurrency": "USD", 
        "exchangeRate": "0.99796725", 
        "status": 1 
    }, 
    "requestId": "PYC20240325164529237", 
    "merchantId": "88888888", 
    "signature": "2sadfj23san finasdfnawesamdfasdfasdfwasfasdfa" 
} 
```
### Bank card recharge record query
This interface is used to query bank card recharge records

**HTTP request**

POST /card/merchant/recharge/query

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|------------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | N | Bank card number |
| currency | String | N | Currency (EUR/USDT) |
| beginTime | datetime | N | Start time (yyyy-MM-dd HH:mm:ss) |
| endTime | datetime | N | End time (yyyy-MM-dd HH:mm:ss) |
| page | Integer | Y | Page number |
| pageSize | Integer | Y | Page size |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "currency": "USDT",
        "beginTime": "2024-07-01 00:00:00",
        "endTime": "2024-07-07 00:00:00",
        "page": 1,
        "pageSize": 10
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```
**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| total | Long | Y | Total page number |
| current | Long | Y | Current page |
| records | List | Y | Data collection |
| records[0].orderNo | String | Y | Order number |
| records[0].cardNo | String | Y | Card number |
| records[0].currency | String | Y | Currency |
| records[0].amount | BigDecimal | Y | Recharge amount |
| records[0].fee | BigDecimal | Y | Recharge fee |
| records[0].receivedAmount | BigDecimal | Y | Received amount |
| records[0].receivedCurrency | String | Y | Received currency |
| records[0].exchangeRate | String | Y | Exchange rate |
| records[0].status | Integer | Y | Deposit status (1-success 2-failure 3-processing) |
| records[0].time | String | Y | Deposit time |

**Response example:**

```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "total": 1,
        "current": 1,
        "records":[{
            "orderNo": "1800420978108948480",
            "cardNo": "4611990405493037",
            "currency": "USDT",
            "amount": 100,
            "fee": 2,
            "time": "2024-07-05 14:53:06",
            "receivedAmount": 99.67,
            "receivedCurrency": "USD",
            "exchangeRate": "0.99796725",
            "status": 1
        }]
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Bank card balance query
This interface is used to query bank card balance

**HTTP request**

POST /card/merchant/balance/query

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| amount | String | Y | Balance |
| currency | String | Y | Currency |

**Response example:**

```json
{
    "code": 0,
    "message": "success", 
    "success": true, 
    "data":[{ 
        "amount": "1000.90", 
        "currency": "USDT" 
        },{
        "amount": "800", 
        "currency": "EUR" 
    }], 
    "requestId": "PYC20240325164529237", 
    "merchantId": "88888888", 
    "signature ": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa" 
} 
```

### Bank card password retrieval
This interface is used to send a request to retrieve password. After receiving the request, the bank will send a link to set it according to the registered mobile phone or email address

**HTTP request**

POST /card/merchant/payment/password/set

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |
| signaturePhoto | String | Y | User signature photo (URL format). It cannot be larger than 2M and supports the formats .png, .jpeg, and .jpg. It is only required when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /card/merchant/config/list. Please call the interface /order/merchant/file/upload to get the image path |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "signaturePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response example:**
code is 0, which means the setting is successful

```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Bank card transaction information query
This interface is used to query bank card transaction information

**HTTP request**

POST /card/merchant/trade/query

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |
| currency | String | N | Currency. Supports EUR/USDT, default EUR |
| beginDate | String | Y | Start date (yyyy-MM-dd) |
| endDate | String | Y | End date (yyyy-MM-dd). The maximum interval between the start and end time is 30 days |
| page | Long | Y | Page number |
| pageSize | Long | Y | Page size. Maximum 30 |

**Request example:**
```JSON
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "currency": "EUR",
        "beginDate": "2024-01-01",
        "endDate": "2024-01-01",
        "pageNum": 1,
        "pageSize": 30
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:----------------------------------------------------------------------------------------|
| total | Long | Y | Total page number |
| current | Long | Y | Current page |
| records | List | Y | Data collection |
| records[0].cardNo | String | Y | Card number |
| records[0].cardTypeId | Integer     | Y    | Card type id                                                                                              |
| records[0].currency | String | Y | Transaction Currency |
| records[0].amount | BigDecimal | Y | Transaction Amount |
| records[0].fee | BigDecimal | Y | Handling fee |
| records[0].txnAmount | BigDecimal | Y | ActualActual Payment Amount |
| records[0].currencyTxn | String | Y | Actual Payment Currency |
| records[0].businessDate | String | Y | Business date |
| records[0].tradeId | String | Y | Transaction serial number |
| records[0].tradeType | Integer | Y | Transaction type. 1-Pre-authorization; 2-Payment; 3-Recharge; 4-Withdrawal; 5-Transfer in; 6-Transfer out; 7-Settlement adjustment; 8-Balance inquiry; 9-Service fee; 10-Consumption; 11-Consumption failure; 12-Refund; 13-Revocation; 14-Others 15-Card binding verification transaction|
| records[0].dataType | String | Y | A-Auth S-Settle |
| records[0].tradeTypeStr | String | Y | Transaction type description |
| records[0].tradeStatus | String | Y | Transaction status. SUCCESS-successful; REVERSAL-reversed; REVERSED-reversed; REJECTED-revoked; CANCELLED-revoked; REFUND-returned |
| records[0].tradeStatusStr | String | Y | Transaction status description |
| records[0].originTransactionId | String | Y | Source transaction serial number |
| records[0].remark | String | Y | Remarks |
| records[0].reason | String | Y | Reasons for consumption failure |
| records[0].transactionTime | Long       | N    | Transaction millisecond timestamp                                                                                         |

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "total": 1,
        "current": 1,
        "records":[{
            "amount": 8000.0,
            "businessDate": 1710950400000,
            "cardNo": "5554748800001227",
            "cardTypeId": 1,
            "currency": "EUR",
            "fee": 1.0,
            "remark": "Purchase clothing",
            "reason": "",
            "tradeStatus": "SUCCESS",
            "tradeStatusStr": "Success",
            "tradeType": 5,
            "dataType": "S",
            "tradeTypeStr": "Transfer",
            "txnAmount": 0.0,
            "currencyTxn": "USD",
            "transactionTime": 1724139032647
        }]
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Bank card freeze
This interface is used to freeze bank cards

**HTTP request**

POST /card/merchant/freeze

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |
| signaturePhoto | String | N | User signature photo (URL format). Cannot be larger than 2M, supports formats .png, .jpeg, .jpg. Required only when the card type represented by the bank card is needPhotoForOperateCard=true. See the parameter needPhotoForOperateCard in the interface /card/merchant/config/list. Please call the interface /order/merchant/file/upload to obtain the image path |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "signaturePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| status | Integer | Y | Processing status. 0: Processing; 1: Success; 2: Failure. When 0 is returned, the final processing status will be sent through the callback notification. |
| statusStr | String | Y | Processing status description |

**Response example:**

code is 0, which means the freeze is successful

```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": {
        "status": 1,
        "statusStr": "Success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Bank card unfreeze
This interface is used for bank card unfreeze

**HTTP request**

POST /card/merchant/unfreeze

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Bank card number |
| signaturePhoto | String | N | User signature photo (URL format). Cannot be larger than 2M, supports .png, .jpeg, .jpg formats. Required only when the card type represented by the bank card needsPhotoForOperateCard=true. See the interface /card/merchant/config/list parameter needPhotoForOperateCard. Please call the interface /order/merchant/file/upload to obtain the image path |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "U898IPO8KH65445F",
        "cardNo": "12456782323",
        "signaturePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| status | Integer | Y | Processing status. 0: Processing; 1: Success; 2: Failure. When 0 is returned, the final processing status will be sent through the callback notification. |
| statusStr | String | Y | Processing status description |

**Response example:**

code 0 means successful unfreezing
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "status": 1,
        "statusStr": "Success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## Global remittance
### Query remittance bank and related configuration
This interface is used to query all countries that support Express Remittance and the parameters, handling fee rates and other data that need to be filled in and passed by each bank

**HTTP request**

POST /order/merchant/globalTransfer/queryBankConfig

**Request parameters:**

| Parameter | Type | Required | Meaning |
|----------|--------|----------|:-----|

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data": null,
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required | Meaning |
|----------|------------|----------|:-----------------------------------|
| country | String | Y | Country code for money transfer |
| countryName | String | Y | Country name for money transfer |
| currency | String | Y | Currency code for money transfer |
| currencyName | String | Y | Currency name for money transfer |
| bankList | List | Y | Bank list for money transfer |
| bankList[0].bankId | Long | Y | Bank id |
| bankList[0].bankName | String | Y | Bank name |
| bankList[0].paymentType | Long | Y | Transaction type. 2: SEPA; 15: Money transfer |
| bankList[0].feeRate | BigDecimal | Y | Fee rate. Example 2=2% |
| bankList[0].feeAmount | BigDecimal | Y | Fixed fee amount. Default EUR |
| bankList[0].payeeParams | List | Y | Payee information request parameters. Each bank requires different parameters. You need to pass the parameters that are returned. |
| payeeParams[0].benAccountNum | String | Y | Payee account number |
| payeeParams[0].benAccountName | String | Y | Payee account name |
| payeeParams[0].benCountryCode | String | N | Payee country of residence |
| payeeParams[0].benCityCode | String | N | Payee city of residence |
| payeeParams[0].benAddress | String | N | Payee address |
| payeeParams[0].benPostCode | String | N | Payee postal code |
| payeeParams[0].benBankCode | String | N | Payee bank code |
| payeeParams[0].benTransBankSwift | String | N | Payee transit bank |
| payeeParams[0].benLastName | String | N | Payee's last name |
| payeeParams[0].benFirstName | String | N | Payee's name |
| payeeParams[0].benNationalityCountry | String | N | Payee's nationality |
| payeeParams[0].benIdNoType | String | N | Payee's ID type |
| payeeParams[0].benIdNo | String | N | Payee's ID number |
| payeeParams[0].benIdExpirationDate | String | N | Payee's ID expiration date |
| payeeParams[0].benBirthday | String | N | Payee's date of birth |
| payeeParams[0].benMobileCode | String | N | Payee's mobile area code |
| payeeParams[0].benMobile | String | N | Payee's mobile number |
| payeeParams[0].benBankAccountType | String | N | Payee bank account type |

**Response example:**

```JSON
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":[{
        "country": "SG",
        "countryName": "Singapore",
        "currency": "SGD",
        "currencyName": "Singapore Dollar",
        "bankList":[{
            "bankId": 2671,
            "bankName": "PayNow",
            "feeRate": 2,
            "feeAmount": 0.5,
            "payeeParams":["benAccountNum","benAccountName","benFirstName","benLastName"]
            },
            {
            "bankId": 2670,
            "bankName": "Sing Investments & Finance Limited",
            "feeRate": 3,
            "feeAmount": 0.4,
            "payeeParams":["benAccountNum","benAccountName","benFirstName","benLastName"]
        }]
    }],
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

```text
Note when bankName=alipay
1. The amount of a single remittance cannot be greater than 50,000 RMB
2. The country and city of residence of the payee cannot be in China
```

### Query legal currency exchange rate
This interface is used to query the exchange rate between legal currencies. For reference only. The actual amount of money received is subject to the bank's processing.

**HTTP request**

POST /order/merchant/globalTransfer/getExchangeRate

**Request parameters:**

| Parameter | Type | Required | Meaning |
|----------|--------|----------|:-----|
| currency | String | Y | Transaction currency. Only EUR is supported |
| targetCurrency | String | Y | Target currency code. Interface /order/merchant/globalTransfer/queryBankConfig response parameter currency |
| targetCountry | String | Y | Target country code. Interface/order/merchant/globalTransfer/queryBankConfigResponse parametercountry |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "currency": "EUR",
        "targetCurrency": "SGD",
        "targetCountry": "SG"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| currency | String | Y | Transaction currency |
| targetCurrency | String | Y | Target currency code |
| exchangeRate | BigDecimal | Y | Exchange rate |

**Response example:**

```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data":{
        "currency": "EUR",
        "targetCurrency": "SGD",
        "exchangeRate": 1.4568
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Payment verification
This interface is used to verify the parameters related to global remittance payment.

**HTTP request**

POST /order/merchant/globalTransfer/paymentVerify

**Frequency limit:** 200/5s

**Request parameters:**

Same as payment interface parameters

**Response example:**

Success example
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

Failure example
```json
{
    "code": -1,
    "message": "The parameter bankId cannot be empty",
    "success": false,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Payment payee verification
This interface is used to verify the parameters of the global remittance payer.

**HTTP request**

POST /order/merchant/globalTransfer/payerVerify

**Frequency limit:** 200/5s

**Request parameters:**

| Parameter | Type | Required or not | Meaning                                                                                          |
|-------------------------------|------------|----------|:-------------------------------------------------------------------------------------------------|
| payerType | String | Y | Payer type. INDIVIDUAL: individual                                                               |
| payerLastName | String | Y | Payer's last name. Chinese characters [2 .. 50] characters are not supported                     |
| payerFirstName | String | Y | Payer's first name. Chinese characters [2 .. 50] characters are not supported                    |
| payerIdNo | String | Y | Payer's ID number. Chinese characters are not supported [6 .. 18 ] characters                    |
| payerIdNoType | String | Y | Payer ID type. See dictionary_biz.pdf (1.1. Idno Type)                                           |
| payerIdCountry | String | Y | Payer ID issuing country. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)     |
| payerBirthday | String | Y | Payer date of birth. yyyy-MM-dd                                                                  |
| payerNationalityCountry | String | Y | Payer nationality. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)            |
| payerMobile | String | Y | Payer mobile number. For example: +37012345678. [8 .. 15 ] characters                            |
| payerCountryCode | String | Y | Payer's country of residence. Country code (2 digits). See dictionary_common.xlsx (sheet. regin) |
| payerCityCode | String | Y | Payer's city code of residence. See dictionary_common.xlsx (sheet. city)                         |
| payerAddress | String | Y | Payer's address. Chinese characters are not supported. [10 .. 100 ] characters                   |
| payerPostCode | String | Y | Payer's postal code. [3 ..9 ] characters                                                         |
| payerOccupation | String | Y | Payer's occupation. Only English [3 .. 20] characters are supported                              |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "payerType": "INDIVIDUAL",
        "payerLastName": "z",
        "payerFirstName": "tom",
        "payerIdNo": "ES123456",
        "payerIdNoType": "PASSPORT",
        "payerIdCountry": "SG",
        "payerBirthday": "1990-10-10",
        "payerNationalityCountry": "SG",
        "payerMobile": "+65012345678",
        "payerCountryCode": "SG",
        "payerCityCode": "SG_1",
        "payerAddress": "21 Tampines Ave 1, Singapore 529757",
        "payerPostCode": "999002",
        "payerOccupation": "worker"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response example:**

Success example
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

Failure example
```json
{
    "code": -1,
    "message": "The parameter bankId cannot be empty",
    "success": false,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Payee Verification
This interface is used to verify the parameters of the global remittance payee.

**HTTP request**

POST /order/merchant/globalTransfer/payeeVerify

**Frequency limit:** 200/5s

**Request parameters:**

| Parameter | Type | Required or not | Meaning                                                                                                          |
|-----------------------------|--------|----------|:-----------------------------------------------------------------------------------------------------------------|
| bankId | Long | Y | Bank id                                                                                                          |
| benAccountNum | String | Y | Payee account number. Will be converted to uppercase. Chinese characters [2 .. 48 ] characters are not supported |
| benAccountName | String | Y | Payee account name. English name [1 .. 100 ] characters                                                          |
| benCountryCode | String | N | Payee country of residence. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)                   |
| benCityCode | String | N | Beneficiary's city code. See dictionary_common.xlsx (sheet. city)                                                |
| benAddress | String | N | Beneficiary's address. English address [10 .. 100] characters                                                    |
| benPostCode | String | N | Beneficiary's postal code. [3 .. 9] characters                                                                   |
| benBankCode | String | N | Beneficiary's bank code. [6 .. 12] characters                                                                    |
| benTransBankSwift | String | N | Beneficiary's transit bank. [8 .. 11] characters                                                                 |
| benLastName | String | N | Beneficiary's last name. Chinese characters are not supported [2 .. 60] characters                               |
| benFirstName | String | N | Beneficiary's first name. Chinese characters are not supported [2 .. 60 ] characters                             |
| benNationalityCountry | String | N | Nationality of the beneficiary. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)               |
| benIdNoType | String | N | Type of beneficiary ID. See dictionary_biz.pdf (1.1. Idno type)                                                  |
| benIdNo | String | N | ID number of the beneficiary. [6 ..18 ] characters                                                               |
| benIdExpirationDate | String | N | Validity date of the beneficiary ID. Cannot be less than the current time yyyy-MM-dd                             |
| benBirthday | String | N | Date of birth of the beneficiary. yyyy-MM-dd                                                                     |
| benMobileCode | String | N | Area code of the beneficiary mobile phone. Example: +86. [2 .. 5 ] characters                                    |
| benMobile | String | N | Beneficiary's mobile phone number. [5 .. 15 ] characters                                                         |
| benBankAccountType | String | N | Beneficiary's bank account type. See dictionary_biz.pdf (2.1. Bank account type)                                 | 

**Request example:** 
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "bankId": 2671,
        "benAccountNum": "123456",
        "benAccountName": "123456",
        "benCountryCode": "SG",
        "benCityCode": "SG_1",
        "benAddress": "21 Tampines Ave 1, Singapore 529757",
        "benPostCode": "999002",
        "benBankCode": "",
        "benTransBankSwift": "",
        "benLastName": "z",
        "benFirstName": "alex",
        "benNationalityCountry": "SG",
        "benIdNoType": "PASSPORT",
        "benIdNo": "13242424",
        "benIdExpirationDate": "2030-01-01",
        "benBirthday": "2001-01-01",
        "benMobileCode": "+67",
        "benMobile": "12345678",
        "benBankAccountType": "SAVINGS"
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response example:**

Success example
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

Failure example
```json
{
    "code": -1,
    "message": "The parameter bankId cannot be empty",
    "success": false,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Payment
This interface is used for global remittance payment. EUR to other fiat currencies

```text
Payer
1. Unique according to payerType and payerIdNo. If it exists, the payer information will be updated

Payee
1. Unique according to bankId and benAccountNum. If it exists, the payee information will be updated
```

**HTTP request**

POST /order/merchant/globalTransfer/payment

**Frequency limit:** 200/5s

**Request parameters:**

| Parameter | Type | Required or not | Meaning                                                                                                          |
|-------------------------------|------------|----------|:-----------------------------------------------------------------------------------------------------------------|
| bankId | Long | Y | Bank id                                                                                                          |
| uniqueId | String | Y | Unique ID of the partner user. [1 .. 50 ] characters                                                             |
| originOrderNo | String | Y | Merchant business order number. [12 .. 30 ] characters                                                           |
| amount | BigDecimal | Y | Amount of money transferred. Only supports EUR, minimum amount 100EUR, maximum 2 decimal places                  |
| postscript | String | Y | Postscript. Only supports English [5 .. 64 ] characters                                                          |
| relationship | String | N | Payee and payee relationship. Required when paymentType=15. See dictionary_biz.pdf (2.4. Relationship)           |
| sourceFunds | String | N | Source of funds. Required when paymentType=15. See dictionary_biz.pdf (2.5. SourceFunds)                         |
| payPurpose | String | N | Purpose of payment. See dictionary_biz.pdf (2.6. PayPurpose)                                                     |
| payer | Object | Y | Payer information object                                                                                         |
| payer.payerType | String | Y | Payer type. INDIVIDUAL: Individual                                                                               |
| payer.payerLastName | String | Y | Payer last name. Chinese characters are not supported [2 .. 50 ] characters                                      |
| payer.payerFirstName | String | Y | Payer first name. Chinese characters are not supported [2 .. 50 ] characters                                     |
| payer.payerIdNo | String | Y | Payer ID number. Chinese characters are not supported [6 .. 18 ] characters                                      |
| payer.payerIdNoType | String | Y | Payer ID type. See dictionary_biz.pdf (1.1. Idno Type)                                                           |
| payer.payerIdCountry | String | Y | Payer ID issuing country. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)                     |
| payer.payerBirthday | String | Y | Payer's date of birth. yyyy-MM-dd                                                                                |
| payer.payerNationalityCountry | String | Y | Payer's nationality. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)                          |
| payer.payerMobile | String | Y | Payer's mobile number. Example: +37012345678. [8 .. 15 ] characters                                              |
| payer.payerCountryCode | String | Y | Payer's country of residence. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)                 |
| payer.payerCityCode | String | Y | Payer's city code of residence. See dictionary_common.xlsx (sheet. city)                                         |
| payer.payerAddress | String | Y | Payer address. Chinese characters are not supported [10 .. 100 ] characters                                      |
| payer.payerPostCode | String | Y | Payer postal code. [3 .. 9 ] characters                                                                          |
| payer.payerOccupation | String | Y | Payer occupation. English only supported [3 .. 20 ] characters                                                   |
| payee | Object | Y | Payee information object                                                                                         |
| payee.benAccountNum | String | Y | Payee account number. Will be converted to uppercase. Chinese characters are not supported [2 .. 48 ] characters |
| payee.benAccountName | String | Y | Payee name. English name [1 .. 100 ] characters                                                                  |
| payee.benCountryCode | String | N | Payee's country of residence. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)                 |
| payee.benCityCode | String | N | Payee's city code. See dictionary_common.xlsx (sheet. city)                                                      |
| payee.benAddress | String | N | Payee address. English address [10 .. 100 ] characters                                                           |
| payee.benPostCode | String | N | Payee postal code. [3 .. 9 ] characters                                                                          |
| payee.benBankCode | String | N | Payee bank code. [6 .. 12 ] characters                                                                           |
| payee.benTransBankSwift | String | N | Payee transit bank. [8 .. 11 ] characters                                                                        |
| payee.benLastName | String | N | Payee surname. Chinese characters are not supported [2 .. 60 ] characters                                        |
| payee.benFirstName | String | N | Payee first name. Chinese characters are not supported [2 .. 60 ] characters                                     |
| payee.benNationalityCountry | String | N | Payee nationality. Country code (2 digits). See dictionary_common.xlsx (sheet. regin)                            |
| payee.benIdNoType | String | N | Payee ID type. See dictionary_biz.pdf (1.1. Idno type)                                                           |
| payee.benIdNo | String | N | Payee ID number. [6 .. 18 ] characters                                                                           |
| payee.benIdExpirationDate | String | N | Payee ID validity period. Cannot be less than the current time yyyy-MM-dd                                        |
| payee.benBirthday | String | N | Payee date of birth. yyyy-MM-dd                                                                                  |
| payee.benMobileCode | String | N | Payee mobile area code. Example: +86. [2 .. 5 ] characters                                                       |
| payee.benMobile | String | N | Payee mobile number. [5 .. 15 ] characters                                                                       |
| payee.benBankAccountType | String | N | Payee bank account type. See dictionary_biz.pdf (2.1. Bank account type)                                         |

**Request example：**

```json
{
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "data":
  {
    "bankId": 2671,
    "uniqueId": "23242",
    "originOrderNo": "12345678934355462",
    "amount": 20,
    "postscript": "test order",
    "relationship": "OTHER",
    "sourceFunds": "SAVINGS",
    "payPurpose": "International trade",
    "payer":
    {
      "payerType": "INDIVIDUAL",
      "payerLastName": "z",
      "payerFirstName": "tom",
      "payerIdNo": "ES123456",
      "payerIdNoType": "PASSPORT",
      "payerIdCountry": "SG",
      "payerBirthday": "1990-10-10",
      "payerNationalityCountry": "SG",
      "payerMobile": "+65012345678",
      "payerCountryCode": "SG",
      "payerCityCode": "SG_1",
      "payerAddress": "21 Tampines Ave 1, Singapore 529757",
      "payerPostCode": "999002",
      "payerOccupation": "worker"
    },
    "payee":
    {
      "benAccountNum": "123456",
      "benAccountName": "123456",
      "benCountryCode": "SG",
      "benCityCode": "SG_1",
      "benAddress": "21 Tampines Ave 1, Singapore 529757",
      "benPostCode": "999002",
      "benBankCode": "",
      "benTransBankSwift": "",
      "benLastName": "z",
      "benFirstName": "alex",
      "benNationalityCountry": "SG",
      "benIdNoType": "PASSPORT",
      "benIdNo": "13242424",
      "benIdExpirationDate": "2030-01-01",
      "benBirthday": "2001-01-01",
      "benMobileCode": "+67",
      "benMobile": "12345678",
      "benBankAccountType": "SAVINGS"
    }
  },
  "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```
**Request Response：**

| Parameter | Type | Required | Meaning |
|----------|--------|----------|:-----|
| orderNo | Long | Y | PayouCard order number |
| originOrderNo | String | Y | Merchant's original order number |
| status | String | Y | Payment status. B1: pending |

**Response example：**
```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "orderNo": 123456789655,
    "originOrderNo": "asdfawe2324sfa",
    "status": "B1"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### User order list
This interface is used to query the user's order list

**HTTP request**

POST /order/merchant/globalTransfer/queryOrderPage

**Frequency limit:** 200/5s

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| uniqueId | String | Y | Unique ID of the partner user. [1 .. 50 ] characters |
| originOrderNo | String | N | Merchant business order number |
| orderNo | Long | N | Payoucard order number |
| page | Long | Y | Current page. Default 1 |
| pageSize | Long | Y | Number per page. Default is 10, maximum is 30 |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
        "uniqueId": "23242",
        "originOrderNo": null,
        "orderNo": 12345678934355462,
        "page": 1,
        "pageSize": 10
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| total | Long | Y | Total quantity |
| current | Long | Y | Current page |
| size | Long | Y | Number per page |
| records | List | Y | Data |
| records[0].uniqueId | String | Y | Unique ID of the partner user |
| records[0].paymentType | String | Y | Transaction type. 2: SEPA; 15: Express |
| records[0].orderNo | Long | Y | Payoucard order number |
| records[0].originOrderNo | String | Y | Merchant order number |
| records[0].status | String | Y | Order status. B1: pending; B2: paying; B3: successful payment; B4: failed payment; B6: refund; B11: order transfer_pending submission; B12: order transfer_pending review; B13: order transfer_approved; B15: order transfer_rejected |
| records[0].paymentCurrency | String | Y | payment currency |
| records[0].paymentAmount | BigDecimal | Y | payment amount |
| records[0].paymentFee | BigDecimal | Y | total payment fee |
| records[0].paymentFeeRate | BigDecimal | Y | fee rate. 2=2% |
| records[0].paymentFeeRateAmount | BigDecimal | Y | Fee rate amount |
| records[0].paymentFixedFeeAmount | BigDecimal | Y | Fixed fee amount |
| records[0].receivedAccountNum | String | Y | Receiving account number |
| records[0].receivedAccountName | String | Y | Receiving account name |
| records[0].receivedCurrency | String | Y | Receiving currency |
| records[0].receivedAmount | BigDecimal | Y | Receiving amount |
| records[0].receivedCountry | String | Y | Receiving country 2-digit code |
| records[0].receivedCountryStr | String | Y | Receiving country English name |
| records[0].resultMsg | String | N | Order notes |
| records[0].transferOrderInfo | List | N | Transfer order information code |
| records[0].transferOrderFile | List | N | Transfer order file code |
| records[0].transferOrderDesc | String | N | Transfer order description |
| records[0].postscript | String | Y | Transaction postscript |
| records[0].payer | Object | Y | Payer information |
| records[0].payer.payerType | String | Y | Payer type. INDIVIDUAL: individual |
| records[0].payer.payerLastName | String | Y | Payer last name |
| records[0].payer.payerFirstName | String | Y | Payer name |
| records[0].payer.payerIdNo | String | Y | Payer ID number |
| records[0].payer.payerIdNoType | String | Y | Payer ID type. EUROPEAN_ID.ID card; PASSPORT.Passport |
| records[0].payer.payerIdCountry | String | Y | Payer's ID issuing country. 2-digit code |
| records[0].payer.payerBirthday | LocalDate | Y | Payer's date of birth yyyy-MM-dd |
| records[0].payer.payerNationalityCountry | String | Y | Payer's nationality. 2-digit code |
| records[0].payer.payerMobile | String | Y | Payer mobile phone number |
| records[0].payer.payerCountryCode | String | Y | Payer country code |
| records[0].payer.payerCityCode | String | Y | Payer city code |
| records[0].payer.payerAddress | String | Y | Payer address in English |
| records[0].payer.payerPostCode | String | Y | Payer postal code |
| records[0].payer.payerOccupation | String | Y | Payer occupation |
| records[0].payee | Object | Y | Payee information |
| records[0].payee.bankId | Long | Y | Beneficiary bank ID |
| records[0].payee.bankName | String | Y | Beneficiary bank |
| records[0].payee.benAccountNum | String | Y | Payee account number |
| records[0].payee.benAccountName | String | Y | Payee account name. English name |
| records[0].payee.benCountryCode | String | N | Payee country code |
| records[0].payee.benCityCode | String | N | Payee city code |
| records[0].payee.benAddress | String | N | address. English address |
| records[0].payee.benPostCode | String | N | Postal code |
| records[0].payee.benBankCode | String | N | Bank code |
| records[0].payee.benTransBankSwift | String | N | Transit bank |
| records[0].payee.benLastName | String | N | Payee last name |
| records[0].payee.benFirstName | String | N | Payee first name |
| records[0].payee.benNationalityCountry | String | N | Nationality code |
| records[0].payee.benIdNoType | String | N | ID type. EUROPEAN_ID.ID card; PASSPORT.Passport |
| records[0].payee.benIdNo | String | N | ID number |
| records[0].payee.benIdExpirationDate | LocalDate | N | ID validity period. yyyy-MM-dd |
| records[0].payee.benBirthday | LocalDate | N | Date of birth. yyyy-MM-dd |
| records[0].payee.benBankAccountType | String | N | Payee bank account type. See dictionary_biz.pdf (2.1. Bank account type) | 

**Response example:** 

```json
{
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "data":
  {
    "total": 1,
    "current": 1,
    "size": 10,
    "records": [
      {
        "uniqueId": "11029",
        "paymentType": 15,
        "orderNo": 12345678934355462,
        "originOrderNo": "1777240103854825473",
        "status": "B3",
        "paymentCurrency": "EUR",
        "paymentAmount": "1234",
        "paymentFee": "4.5",
        "paymentFeeRate": "2",
        "paymentFeeRateAmount": "3",
        "paymentFixedFeeAmount": "1.5",
        "receivedAccountNum": "vbnnm",
        "receivedAccountName": "bbmnnnz",
        "receivedCurrency": "JPY",
        "receivedAmount": "191243.59",
        "receivedCountry": "JP",
        "receivedCountryStr": "JAPAN",
        "resultMsg": "test",
        "transferOrderInfo": [
          "1",
          "2"
        ],
        "transferOrderFile": [
          "1",
          "2"
        ],
        "transferOrderDesc": "test",
        "postscript": "lhhnvbbn",
        "payer": {
          "payerType": "INDIVIDUAL",
          "payerLastName": "wqw",
          "payerFirstName": "wqwq",
          "payerIdNo": "213232312",
          "payerIdNoType": "PASSPORT",
          "payerIdCountry": "AL",
          "payerBirthday": "2024-02-05",
          "payerNationalityCountry": "AL",
          "payerMobile": "19800001111",
          "payerCountryCode": "AL",
          "payerCityCode": "AL_BR",
          "payerAddress": "eqwweqewq",
          "payerPostCode": "212323",
          "payerOccupation": "worker"
        },
        "payee": {
          "bankId": 5143,
          "bankName": "Mizuho Trust and Banking Co.,Ltd.",
          "benAccountNum": "vbnnm",
          "benAccountName": "bbmnnnz",
          "benCountryCode": null,
          "benCityCode": null,
          "benAddress": null,
          "benPostCode": null,
          "benBankCode": "gjbvvbn",
          "benTransBankSwift": null,
          "benLastName": "vbjnnb",
          "benFirstName": "gjgbjj",
          "benNationalityCountry": "AL",
          "benIdNoType": null,
          "benIdNo": null,
          "benIdExpirationDate": null,
          "benBirthday": null,
          "benBankAccountType": null
        }
      }
    ]
  },
  "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

### Submit order information or file
This interface is used to submit transfer information or files. When the order status is B11 or B15, the order enters the transfer mode, and the order-related information needs to be submitted for review before the transfer can continue. Please use it in conjunction with the transfer callback notification

ps: Please pass all the file information of the order for each request

**HTTP request**

POST /order/merchant/globalTransfer/submitTransferOrderInfo

**Request parameters:**

| Parameter | Type | Required | Meaning                                                                                  |
|----------|--------|----------|:-----------------------------------------------------------------------------------------|
| orderNo | Long | Y | PayouCard order number                                                                   |
| transferInfos | List | N | Order information collection                                                             |
| transferInfos[0].code | String | N | Order information code. See dictionary_biz.pdf (2.2. Transfer order info type)           |
| transferInfos[0].content | String | N | Order information content. Chinese characters are not supported. Maximum 200 characters. |
| transferFiles | List | N | Order file collection. See dictionary_biz.pdf (2.3. Transfer order file type)            |
| transferFiles[0].code | String | N | Order file code                                                                          |
| transferFiles[0].content | String | N | Order file url. Each file cannot exceed 2M, Only supports image formats .png, jpg, jpeg  |

**Request example:**

```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
    "orderNo": 1234556778945342,
    "transferInfos":[{
        "code": "1",
        "content": "tom"
    },
    {
        "code": "16",
        "content": "2002-10-10"
    }],
    "transferFiles":[{
        "code": "26",
        "content": "https://adfasfwe.cloudfront.net/media/88888888/123.png"
    }]
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|

**Response example:**
```json
{
    "code": 0,
    "message": "success",
    "success": true,
    "data": null,
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### Query order results
This interface is used to query order results

**HTTP request**

POST /order/merchant/globalTransfer/getOrderResult

**Frequency limit:**50/5s Dimension merchantId + orderNo

**Request parameters:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| orderNo | Long | Y | PayouCard order number |

**Request example:**
```json
{
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "data":{
      "orderNo": 1234556778945342
    },
    "signature": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**Request response:**

| Parameter | Type | Required or not | Meaning |
|----------|--------|----------|:-----|
| orderNo | Long | Y | PayouCard order number |
| status | String | Y | Order status. B1: pending; B2: paying; B3: successful payment; B4: payment failed; B6: refund; B11: transfer order_pending submission; B12: transfer order_pending review; B13: transfer order_approved review; B15: transfer order_rejected review |
| paymentCurrency | String | Y | Payment currency |
| paymentAmount | BigDecimal | Y | Payment amount |
| paymentFee | BigDecimal | Y | Payment handling fee |
| receivedAccountNum | String | Y | Receiving account |
| receivedAccountName | String | Y | Receiving account name |
| receivedCurrency | String | Y | Receiving currency |
| receivedAmount | BigDecimal | N | Receiving amount. Returned when status = B3, B4, B6 |
| resultMsg | String | N | Message |
| transferOrderInfo | List | N | Transfer order information code. Returned when status = B11, B15. See dictionary_biz.pdf (2.2. Transfer order info type) |
| transferOrderFile | List | N | Transfer order file code. Returned when status = B11, B15. See dictionary_biz.pdf（2.3. Transfer order file type）| 
| transferOrderDesc | String | N | Transfer order description|

**Response example：**

```JSON
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "orderNo": 12343434232324,
    "status": "B3",
    "paymentCurrency": "EUR",
    "paymentAmount": 10,
    "payAccountNum": "1234",
    "paymentFee": 1.5,
    "receivedAccountNum": "es324353535",
    "receivedAccountName": "tom z",
    "receivedCurrency": "SGD",
    "receivedAmount": 30.5,
    "resultMsg": "success",
    "transferOrderInfo":
    [
      "1",
      "3",
      "5"
    ],
    "transferOrderFile":
    [
      "21",
      "23",
      "24"
    ],
    "transferOrderDesc": ""
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## Callback notification
### Set callback notification url
This interface is used to set callback notification URL.

Please set the callback notification address in the merchant basic information in the merchant backend system. All callback notifications share this URL, and use notifyType to distinguish different notification contents

### Callback notification type

| notifyType | business | meaning |
|------------|------|------|
| 1 | Global Express | Payment success, payment failure, refund notification |
| 2 | Global Express | Order adjustment notification |
| 3 | Bank card | Callback notification after user registration review is completed |
| 4 | Bank card | Card recharge result callback notification |
| 5 | Bank card | Card activation result callback notification |
| 6 | Bank card | Card freeze, thaw processing status callback notification |
| 7 | Bank card | 3DS verification |
| 8 | Bank Card | Bank Card Consumption Statement |

### Global Express Remittance Payment result callback notification
This notification notifyType = 1

**Callback parameters:**

| Parameter | Type | Must be transmitted | Meaning |
|------|------|------|:------|
| orderNo | Long | Y | PayoCard order number |
| status | String | Y | Order status. B3: Successful payment; B4: Failed payment; B6: Refund; |
| paymentCurrency | String | Y | Payment currency |
| paymentAmount | BigDecimal | Y | Payment amount |
| paymentFee | BigDecimal | Y | Payment fee |
| receivedAccountNum | String | Y | Receiving account number |
| receivedAccountName | String | Y | Receiving account name |
| receivedCurrency | String | Y | Receiving currency |
| receivedAmount | BigDecimal | N | Receiving amount. Return when status = B3, B4, B6 |
| resultMsg | String | N | message |

**Callback example:**

```JSON
{
    "data":{
        "orderNo": 12343434232324,
        "status": "B3",
        "paymentCurrency": "EUR",
        "paymentAmount": 10,
        "payAccountNum": "1234",
        "paymentFee": 1.5,
        "receivedAccountNum": "es324353535",
        "receivedAccountName": "tom z",
        "receivedCurrency": "SGD",
        "receivedAmount": 30.5,
        "resultMsg": "success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 1
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | Information |

**Response example:**

```json
{
    "code": 0,
    "message": "success"
}
```

### Global Express Remittance Adjustment callback notification

This notification notifyType = 2

**Callback parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| orderNo | Long | Y | PayouCard order number |
| status | String | Y | Order status. B11: Transfer order_pending submission; B13: Transfer order_approved; B15: Transfer order_rejected |
| transferOrderInfo | List | N | Transfer order information. See dictionary_biz.pdf (2.2. Transfer order info type) |
| transferOrderFile | List | N | Transfer order file. See dictionary_biz.pdf (2.3. Transfer order file type) |
| transferOrderDesc | String | N | Transfer order information |

**Callback example:**

```json
{
  "data":
  {
    "orderNo": 12343434232324,
    "status": "B11",
    "transferOrderInfo":
    [
      "1",
      "3",
      "5"
    ],
    "transferOrderFile":
    [
      "21",
      "23",
      "24"
    ],
    "transferOrderDesc": ""
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
  "notifyType": 2
}
```
**Response parameters:**

| Parameter | Type | Required | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be repeated |
| message | String | N | information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```

### User KYC callback notification
This notification notifyType = 3

**Callback parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|------|
| uniqueId | String | Y | Unique ID of partner user |
| cardTypeId | Integer | Y | Card type |
| status | Integer | Y | Status |
| statusDesc | String | Y | Status description |

**Callback example:**
```json
{
    "data":{
        "uniqueId": "T6789O067890",
        "cardTypeId": 1,
        "status": 1,
        "statusDesc":"success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 3
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | Information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```

### Bank card callback notification of card recharge result
This notification notifyType = 4

**Response parameters:**

| Parameter | Type | Required | Meaning |
|------|------|------|:------|
| cardNo | String | Y | Card number |
| status | Integer | Y | Card recharge status. 1: Success; 2: Failure; |
| orderNo | String | Y | Merchant order number |
| currency | String | Y | Currency |
| rechargeAmount | BigDecimal | Y | Recharge amount |
| receivedAmount | BigDecimal | N | Received amount |
| fee | BigDecimal | Y | Handling fee (money other than recharge amount) |
| msg | String | N | Error message |

**Callback example:**

```json
{
    "data":{
        "cardNo": "12456782323",
        "status": 1,
        "orderNo": "2324a2dfga3435fg34353",
        "currency": "USDT",
        "rechargeAmount": 100,
        "receivedAmount": 100,
        "fee": 2,
        "msg": "success"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 4
}
```

**Response parameters:**

| Parameter | Type | Required | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated again |
| message | String | N | information |

**Response example:**
```json
{
"code": 0,
"message": "success"
}
```

### Bank card activation result callback notification
This notification notifyType = 5

**Callback parameters:**

| Parameter | Type | Is it required | Meaning |
|------|------|------|:------|
| cardNo | String | Y | Card number |
| status | Integer | Y | Status. 5: Activation successful; 12: Activation review failed |
| msg | String | N | Error message |

**Callback example:**
```JSON
{
    "data":{
        "cardNo": "12456782323",
        "status": 5,
        "msg": null
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 5
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```

### Bank card freeze thaw processing status callback notification
This notification notifyType = 6

**Callback parameters:**

| Parameter | Type | Is it required | Meaning |
|------|------|-----|:------|
| uniqueId | String | Y | Unique ID of the partner user |
| cardNo | String | Y | Card number |
| status | Integer | Y | Status. 1: Success; 2: Failure |
| requestType | Integer | Y | Type. 1: Freeze; 2: Unfreeze |

**Callback example:**

```json
{
    "data":{
        "uniqueId": 502323,
        "cardNo": "12456782323",
        "status": 1,
        "requestType": 1
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 5
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be repeated |
| message | String | N | information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```

### Bank card 3DS verification
This notification notifyType = 7

**Callback parameters:**

| Parameter | Type | Must be passed | Meaning |
|------|------|------|:------|
| cardNo | String | Y | Card number |
| otp | String | Y | Verification code |
| merchantName | String | Y | Merchant name |
| transactionAmount | String | Y | Transaction amount |
| transactionCurrency | String | Y | Transaction currency |

**Callback example:**

```json
{
    "data":{
        "cardNo": "4611990424818446",
        "otp": "890789",
        "merchantName": "test",
        "transactionAmount": "100",
        "transactionCurrency": "EUR"
    },
    "requestId": "PYC20240325164529237",
    "merchantId": "88888888",
    "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
    "notifyType": 5
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|------|
| code | Integer | Y | 0. After returning 0, callback notification will not be initiated repeatedly |
| message | String | N | Information |

**Response example:**
```json
{
    "code": 0,
    "message": "success"
}
```

### Bank card consumption bill event
This notification notifyType = 8

**Callback parameters:**
| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| cardNo | String | Y | Card number |
| currency | String | Y | Transaction currency |
| amount | BigDecimal | Y | Transaction amount |
| fee | BigDecimal | Y | Handling fee |
| txnAmount | BigDecimal | Y | Actual payment amount |
| currencyTxn | String | Y | Actual payment currency |
| businessDate | String | Y | Business date |
| tradeId | String | Y | Transaction serial number |
| tradeType | Integer | Y | Transaction type. 1-Pre-authorization; 2-Payment; 3-Recharge; 4-Withdrawal; 5-Transfer in; 6-Transfer out; 7-Settlement adjustment; 8-Balance inquiry; 9-Handling fee; 10-Consumption; 11-Consumption failure; 12-Refund; 13-Revocation; 14-Others 15-Card binding verification transaction |
| tradeTypeStr | String | Y | Transaction type description |
| tradeStatus | String | Y | Transaction status. SUCCESS-successful; REVERSAL-reversed; REVERSED-reversed; REJECTED-revoked; CANCELLED-revoked; REFUND-returned |
| tradeStatusStr | String | Y | Transaction status description |
| originTransactionId | String | Y | Source transaction serial number |
| remark | String | Y | Remark |
| transactionTime | Long | N | Transaction millisecond timestamp |

**Response example:**
```json
{
"code": 0,
"message": "success",
"success": true,
"data":{
    "amount": 8000.0,
    "businessDate": 1710950400000,
    "cardNo": "5554748800001227",
    "currency": "EUR",
    "fee": 1.0,
    "remark": "Purchase clothing",
    "tradeStatus": "SUCCESS",
    "tradeStatusStr": "Success",
    "tradeType": 5,
    "tradeTypeStr": "Transfer",
    "txnAmount": 1000,
    "currencyTxn": "USD",
    "transactionTime": 1724139032647
},
"requestId": "PYC20240325164529237",
"merchantId": "88888888",
"signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

**Response parameters:**

| Parameter | Type | Required or not | Meaning |
|------|------|------|:------|
| code | Integer | Y | 0. After returning 0, callback notification will not be repeated |
| message | String | N | information |

**Response example:**
```json
{
"code": 0,
"message": "success"
}
```





