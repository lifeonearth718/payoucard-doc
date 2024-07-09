- [简介](#简介)
- [基本信息](#基本信息)
    - [名词解释](#名词解释)
    - [请求频率限制](#请求频率限制)
    - [API服务器地址](#api服务器地址)
    - [连接方式](#连接方式)
    - [API请求参数及模板](#api请求参数及模板)
    - [API响应参数及模板](#api响应参数及模板)
    - [回调通知回调参数及模板](#回调通知回调参数及模板)
    - [回调通知响应参数及模板](#回调通知响应参数及模板)
    - [接口认证及加解密](#接口认证及加解密)
        - [创建RSA公私钥](#创建rsa公私钥)
        - [签名](#签名)
        - [验证签名](#验证签名)
    - [响应码](#响应码)
- [字典](#字典)
- [REST API](#rest-api)
    - [公共API](#公共api)
        - [上传文件](#上传文件)
    - [银行卡](#银行卡)
        - [卡片类型列表](#卡片类型列表)
        - [商户用户注册](#商户用户注册)
        - [商户用户信息更新](#商户用户信息更新)
        - [商户用户KYC附件上传](#商户用户kyc附件上传)
        - [提交KYC认证](#提交kyc认证)
        - [查询KYC认证](#查询kyc认证)
        - [虚拟卡开卡申请](#虚拟卡开卡申请)
        - [虚拟卡开卡进度查询](#虚拟卡开卡进度查询)
        - [银行卡激活](#银行卡激活)
        - [银行卡充值](#银行卡充值)
        - [银行卡充值订单查询](#银行卡充值订单查询)
        - [银行卡充值记录查询](#银行卡充值记录查询)
        - [银行卡余额查询](#银行卡余额查询)
        - [银行卡找回密码](#银行卡找回密码)
        - [银行卡交易信息查询](#银行卡交易信息查询)
        - [银行卡冻结](#银行卡冻结)
        - [银行卡解冻](#银行卡解冻)
    - [全球速汇](#全球速汇)
        - [查询速汇银行及相关配置](#查询速汇银行及相关配置)
        - [查询法币汇率](#查询法币汇率)
        - [代付校验](#代付校验)
        - [代付](#代付)
        - [用户订单列表](#用户订单列表)
        - [提交调单信息或文件](#提交调单信息或文件)
        - [查询订单结果](#查询订单结果)
    - [回调通知](#回调通知)
        - [设置回调通知url](#设置回调通知url)
        - [回调通知类型](#回调通知类型)
        - [全球速汇-支付结果回调通知](#全球速汇支付结果回调通知)
        - [全球速汇-调单回调通知](#全球速汇调单回调通知)
        - [用户KYC回调通知](#用户kyc回调通知)
        - [银行卡-卡片充值结果回调通知](#银行卡卡片充值结果回调通知)
        - [银行卡-卡片激活结果回调通知](#银行卡卡片激活结果回调通知)
        - [银行卡-卡片冻结、解冻处理状态回调通知](#银行卡卡片冻结、解冻处理状态回调通知)

# 简介
欢迎使用PayouCard开发者文档。概述了商户对接应用开发接口。

REST API包含两个业务类別：银行卡、全球速汇

# 基本信息
## 名词解释
merchant：商户

merchantId：商户ID。在商户后台系统-”商户基本信息” 菜单中可查看

## 请求频率限制
具体接口中会增加请求频率限制，请查看具体API描述。

## API服务器地址
**test环境：**

服务器地址（IP白名单限制）：https://api-merchant-test.logtec.me

商户后台系统（IP白名单限制）：https://merchant-test.logtec.me

**生产环境：**

服务器地址（IP白名单限制）：https://api-merchant.payoucard.com

商户后台系统：https://merchant.payoucard.com

## 连接方式
所有API统一用POST方式调用。content-type：application/json格式

**若某API请求形式不同，会在具体API接口中进行标注**

## API请求参数及模板


| 参数       | 类型   | 是否必传 | 含义                       |
|------------|:-------|:---------|----------------------------|
| requestId  | String | Y        | 请求流水id。20位随机字符    |
| merchantId | String | Y        | 商户ID。PayouCard为商户分配 |
| data       | Object | Y        | 业务数据                   |
| signature  | String | Y        | 签名                       |

```json
{
  "requestId": "128293243536342523",
  "merchantId": "88888888",
  "data": null,
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## API响应参数及模板
| 参数       | 类型    | 是否必传 | 含义                                            |
|------------|:--------|:---------|-------------------------------------------------|
| code       | Integer | Y        | 响应码                                          |
| message    | String  | Y        | 信息                                            |
| success    | Boolean | Y        | 是否成功。true：成功；false：失败。<br />当code=0时为true |
| data       | Object  | Y        | 响应体                                          |
| requestId  | String  | Y        | 请求流水id。20位随机字符                         |
| merchantId | String  | Y        | 商户ID。PayouCard为商户分配                      |
| signature  | String  | Y        | 签名                                            |

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

## 回调通知回调参数及模板
| 参数       | 类型    | 是否必传 | 含义                       |
|------------|:--------|:---------|----------------------------|
| data       | Object  | Y        | 响应体                     |
| requestId  | String  | Y        | 请求流水id。20位随机字符    |
| merchantId | String  | Y        | 商户ID。PayouCard为商户分配 |
| signature  | String  | Y        | 签名                       |
| notifyType | Integer | Y        | 通知类型                   |

## 回调通知响应参数及模板
| 参数       | 类型    | 是否必传 | 含义                                            |
|------------|:--------|:---------|-------------------------------------------------|
| code       | Integer | Y        | 响应码                                          |
| message    | String  | Y        | 信息                                            |
```json
{
  "code": 0,
  "message": ""
}
```

## 接口认证及加解密
### 创建RSA公私钥
合作方成为商户后需要登录商户后台系统。在 “商户基本信息” 菜单中生成RSA公钥、私钥，同时PayouCard也会提供PayouCard的公钥。请商户妥善保管公钥、私钥、PayouCard公钥。在之后的API请求中会用到所述参数。

### 签名
requestId：请求流水id。20位随机字符

merchantId：PayouCard为商户分配。在商户后台系统 “商户基本信息” 菜单中可查看。

data：业务数据。

signature：签名源字符串由除签名（signature）字段外的所有非空字段内容组成，按照消息字段的ASCII码排序，并以"&"符号按照"字段名=字段值"的方式连接。默认使用RSA算法进行签名。

* 例如：查询汇率请求数据
```json
{
  "requestId": "PYC20240325164529237",
  "marchant": "88888888",
  "data":
  {
    "currency": "EUR",
    "targetCurrency": "SGD",
    "targetCountry": "SG"
  }
}
```
签名源字符串签名如下。签名源字符串使用商户RSA私钥签名生成签名。

```
data={"targetCurrency":"SGD","currency":"EUR","targetCountry":"SG"}&merchantId=88888888&requestId=PYC20240325164529237
```
* 备注

    * RSA的签名算法是SHA256withRSA。
    * RSA 的最大加密明文长度为117。
    * RSA的最大解密密文长度为128。
    * 请求数据中的字段signature由商户 RSA私钥签名生成。
    * 响应数据中的字段signature由 PayouCard RSA 私钥签名生成。
    * 具体实现请参考payoucard-demo.zip```com.payoucard.demo.service.GlobalTransferTestService.class#main()方法```

### 验证签名
signature：由PayouCard RSA 私钥签名生成

* 例如：查询汇率响应数据

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
  "signature": "SCHaOIDlAkyaad4VxBrd9ON27ZrSK0IpNCPbkjQEe8YR/2UaZUlYUViBRDmnkvhJCehxjBTwECCF9vr4qjG1epyTKYVy8fqcQdynzXyclT5MYs6N9uWu5AHQm/HQpvfBjRcK3y65TaqZdNkToREQ/XPg/PsAFpPueSugjYJ0/C0=",
  "requestId": "PYC20240325191750279"
}
```

* 使用除签名字段（signature）外的所有非空字段内容组成，按照消息字段的ASCII码排序，并以“&”符号按照“字段名=字段值”的方式连接。通过PayouCard RSA公钥进行验签

## 响应码

| code  | 含义                                                            |
|-------|-----------------------------------------------------------------|
| 0     | 成功                                                            |
| -1    | 具体业务错误。原因见message描述                                  |
| 500   | 服务器出错。请联系PayouCard工作人员                              |
| 50001 | 请求参数缺失。requestId、merchantId、signature为空                 |
| 50002 | 商户不存在                                                      |
| 50003 | API功能不可用。例如关闭了全球速汇的功能，则全球速汇相关接口不可用 |
| 50004 | 验签失败                                                        |
| 50005 | 商户未设置公钥                                                  |



# 字典

* 业务字典

  ```/src/dictionary_biz.pdf```

* 公用字典

    * 货币字典
    * 手机区号字典
    * 国家字典
    * 城市字典

  ```/src/dictionary_common.xlsx```

* java-sdk-demo

  ```/src/payoucard-demo.zip```

# REST API
## 公共API
### 上传文件
此接口用于上传文件，获取文件url

**HTTP请求**

POST /order/merchant/file/upload

**请求content-type：** multipart/form-data

**限频：** 100/5s

**请求参数：**

| 参数       | 类型   | 是否必传| 含义       |
|------------|--------|------------|:-----------|
| file       | file   | Y          | 文件       |
| requestId  | String | Y          | 请求流水id |
| marchantId | String | Y          | 商户id     |
| marchantId | String | Y          | 商户id     |
| data       | Object | N          | 业务数据   |

**请求响应：**

| 参数       | 类型   | 是否必传| 含义       |
|------------|--------|------------|:-----------|
| data       | Object | Y         | 文件路径   |


**响应示例：**
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

## 银行卡

### 卡片类型列表
此接口用于获取所有的卡片类型

**HTTP请求**

POST /card/merchant/config/list

**请求参数：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":null,
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数                                        | 类型    | 是否必传 | 含义                 |
|-------------------------------------------|---------|----------|:---------------------|
| id                                        | Integer | Y        | 卡片类型ID           |
| type                                      | Integer  | Y        | 类型(0-Visa虚拟卡 1-Visa实体卡 2-MasterCard虚拟卡 3-MasterCard实体卡)            |
| typeDesc                                  | String  | Y        | 类型描述          |
| cardName                                  | String  | Y        | 卡名称          |
| cardDesc                                  | String  | Y        | 卡描述              |
| kycRequire                                | Boolean | Y        | 是否需要KYC认证      |
| rechargeCurrencyInfoList                  | List    | Y        | 可充值的币种列表配置 |
| rechargeCurrencyInfoList[0].currency      | String    | Y        | 充值币种 |
| rechargeCurrencyInfoList[0].rechargeMinQuota | BigDecimal    | Y        | 充值最小额度 |
| rechargeCurrencyInfoList[0].rechargeMaxQuota | BigDecimal    | Y        | 充值最大额度 |
| rechargeCurrencyInfoList[0].digital          | Long    | Y        | 精度 |
| needPhotoForActiveCard                    | Boolean    | Y        | 激活卡片时是否需要手持护照和银行卡照 |
| needPhotoForOperateCard                   | Boolean    | Y        | 找回密码时是否需要提供用户签名照 |

**响应示例：**

```JSON
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":[
    {
      "id":1,
      "type": 3,
      "typeDesc":"Master-实体卡",
      "cardName":"master 实体卡",
      "kycRequire":true,
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
    }
  ],
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 商户用户注册
此接口用于商户的用户注册，注册的信息将用于提交银行做KYC申请；
1、虚拟卡的申请需要提供手机号、手机国际区号、邮箱、姓、名和地址
2、实体卡的申请所有信息均需要提供，另外特殊的卡需要提供手持证件号

测试环境中提交KYC的要求:
id==3 or id==9：手机号、手机区号、邮箱、姓、名和地址
id==1 or id == 10：注册接口中所有的字段都不能为空
id==2:注册接口中所有的字段都需要，上传手持证件照(通过附件上传)

id == 1申请的国家必须为欧盟成员国，否则不支持
EEA国家参考dictionary_common.xlsx中的EEA国家

**建议所有的卡片注册用护照 **

生产环境ID待提供



**HTTP请求**

POST /user/merchant/register

**请求参数：**

| 参数        | 类型   | 是否必传 | 含义                                                          |
|-------------|--------|----------|:--------------------------------------------------------------|
| uniqueId    | String | Y        | 合作商用户的唯一ID                                             |
| areaCode    | String | Y        | 手机国际区号。例：+86                                           |
| mobile      | String | Y        | 手机号                                                        |
| email       | String | Y        | 邮箱                                                         |
| lastName    | String | Y        | 姓(只能是英文，单词之间支持一个英文空格, 长度不能超过64)                                                            |
| firstName   | String | Y        | 名(只能是英文，单词之间支持一个英文空格, 长度不能超过64)                                                            |
| birthday    | String | Y        | 生日 (yyyy-MM-dd)                                             |
| nationality | String | N        | 国籍。2位数code码。参见dictionary_common.xlsx（sheet. regin）     |
|             |        |          |                                                               |
| sex         | String | N        | 性别 (1: 男, 2: 女)                                           |
| country     | String | N        | 国家代码。2位数code码。参见dictionary_common.xlsx（sheet. regin） |
| town    | String | N        | 城市code码。参见dictionary_common.xlsx（sheet. city）|
| address    | String | Y        | 详细地址（英文地址,长度不能超过100)                                                            |
| postCode    | String |N        | 邮编                                                           |
| idType    | String | N        | 证件类型。参见dictionary_biz.pdf（1.1. Idno Type）。                                                            |
| idNo   | String | N        | 证件号                                                          |
| idNoExpiryDate   | String | N        | 证件过期时间(yyyy-MM-dd)                                                            |
| idPicture   | String | N        | 证件照片 (URL格式)。请调用接口 /order/merchant/file/upload 获取url                                                            |
| facePicture   | String | N        | 人脸照片(URL格式)。请调用接口 /order/merchant/file/upload 获取url                                                            |

**请求示例：**

```JSON
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
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
    "facePicture": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应示例：**
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

### 商户用户信息更新
此接口用于更新商户的用户信息

**HTTP请求**

POST /user/merchant/info/update

**请求参数：**

| 参数        | 类型   | 是否必传 | 含义                                                          |
|-------------|--------|----------|:--------------------------------------------------------------|
| uniqueId    | String | Y        | 合作商用户的唯一ID                                             |
| areaCode    | String | Y        | 手机国际区号。例：+86                                           |
| mobile      | String | Y        | 手机号                                                        |
| email       | String | Y        | 邮箱                                                          |
| lastName    | String | Y        | 姓                                                            |
| firstName   | String | Y        | 名                                                            |
| birthday    | String | Y        | 生日 (yyyy-MM-dd)                                             |
| nationality | String | N        | 国籍。2位数code码。参见dictionary_common.xlsx（sheet. regin）     |
|             |        |          |                                                               |
| sex         | String | N        | 性别 (1: 男, 2: 女)                                           |
| country     | String | N        | 国家代码。2位数code码。参见dictionary_common.xlsx（sheet. regin） |
| town    | String | N        | 城市code码。参见dictionary_common.xlsx（sheet. city）|
| address    | String | Y        | 详细地址                                                            |
| postCode    | String |N        | 邮编                                                           |
| idType    | String | N        | 证件类型。参见dictionary_biz.pdf（1.1. Idno Type）。                                                            |
| idNo   | String | N        | 证件号                                                          |
| idNoExpiryDate   | String | N        | 证件过期时间(yyyy-MM-dd)                                                            |
| idPicture   | String | N        | 证件照片 (URL格式)。请调用接口 /order/merchant/file/upload 获取url                                                            |
| facePicture   | String | N        | 人脸照片(URL格式)。请调用接口 /order/merchant/file/upload 获取url                                                            |


**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
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
    "facePicture": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应示例：**

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

### 商户用户KYC附件上传
此接口用于补缺失的KYC附件

**HTTP请求**

POST /user/merchant/kyc/attachments

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID    |
| type | Integer | Y        | 类型(1-EEA文件证明 2-手持照片)data    |
| data | String | Y        | 数据    |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId":"U898IPO8KH65445F",
    "type": 2,
    "data": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应示例：**
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

### 提交KYC认证
此接口用于KYC认证以便后续银行卡相关的业务操作

虚拟卡KYC一般十分钟内有结果，可通过查询接口也可等待回调；
实体卡KYC审核24内有结果；

**HTTP请求**

POST /user/merchant/submit/kyc/verification

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID    |
| cardTypeId | Integer | Y        | 卡片类型（通过卡片类型列表接口获取） |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardTypeId": 1
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应示例：**
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

### 查询KYC认证
此接口用于查询KYC认证是否认证成功

**HTTP请求**

POST /user/merchant/kyc/query

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID    |
| cardTypeId | Integer | Y        | 卡片类型（通过卡片类型列表接口获取） |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardTypeId": 1
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| cardTypeId | Integer | Y        | 卡片类型ID    |
| status | Integer | Y        | 状态(1-审核中 2-成功 3-拒绝) |
| statusDesc | String | Y        | 状态描述 |

**响应示例：**
```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":{
    "cardTypeId":1,
    "status":2,
    "statusDesc": "成功"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 虚拟卡开卡申请
HTTP请求

POST /card/merchant/virtual/card/apply

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商的唯一ID    |
| cardTypeId | Integer | Y        | 卡片类型（通过卡片类型接口获取） |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardTypeId": 1
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| orderNo | String | Y        | 订单号（用于查询开卡进度)    |

**响应示例：**
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

### 虚拟卡开卡进度查询
**HTTP请求**

POST /card/merchant/virtual/card/status

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| orderNo | String | Y        | 订单号    |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "orderNo":"240123173818711000"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| cardNo | String | Y        | 卡号    |
| expire | String | Y        | 过期时间    |
| cvv | String | Y        | cvv    |
| status | Integer | Y        | 状态(0-处理中 1-开卡成功）    |

**响应示例：**
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

### 银行卡激活
**HTTP请求**

POST /card/merchant/activation

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义                                                                                                                                                                                                             |
|----------|--------|----------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y        | 合作商用户的唯一ID                                                                                                                                                                                                     |
| cardNo | String | Y        | 银行卡号                                                                                                                                                                                                           |
| activePhoto | String | N        | 手持护照和银行卡照图片  (URL格式)。不能大于2M，支持格式.png, .jpeg, .jpg。当用户进行kyc时，参数cardTypeId所代表的卡片类型needPhotoForActiveCard=true时才需要。见接口/card/merchant/config/list参数needPhotoForActiveCard。请调用接口 /order/merchant/file/upload 获取图片路径 |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "activePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**
| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| status | Integer | Y        | 处理状态。0：处理中；1：成功；2：失败。当返回0时，会通过激活回调通知发送最终的处理状态。  |


**响应示例：**
```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "status": 1,
    "statusStr": "处理中"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 银行卡充值
此接口用于查询银行卡充值

**HTTP请求**

POST /card/merchant/recharge

**请求参数：**

| 参数       | 类型         | 是否必传 | 含义 |
|----------|------------|----------|:-----|
| uniqueId | String     | Y        | 合作商用户的唯一ID  |
| cardNo   | String     | Y        | 银行卡号  |
| amount   | BigDecimal | Y        | 充值金额  |
| currency | String     | N        | 币种(默认USDT)。支持的币种请参考接口/card/merchant/config/list中的rechargeCurrencyInfoList集合  |
| orderNo  | String     | Y        | 商户订单号  |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "currency": "USDT",
    "amount": 100,
    "orderNo": "2324a2dfga3435fg34353"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应参数：**

| 参数             | 类型         | 是否必传 | 含义                     |
|----------------|------------|------|:-----------------------|
| cardNo         | String     | Y    | 卡号                     |
| status         | Integer    | Y    | 卡片充值状态。1：成功；2：失败；3：处理中 |
| orderNo        | String       | Y    | 商户订单号           |
| currency       | String     | Y    | 币种                     |
| rechargeAmount | BigDecimal | Y    | 充值金额                   |
| receivedAmount | BigDecimal | N    | 到账金额                   |
| receivedCurrency | String | Y | 到账币种  |
| exchangeRate | String | Y | 兑换汇率  |
| fee            | BigDecimal | Y    | 手续费(扣充值金额之外的钱)         |
| msg            | String     | N    | 错误信息                   |


**响应示例：**

code为0则代表充值成功
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
    "receivedAmount": 99.67,
    "receivedCurrency": "USD",
    "exchangeRate": "0.99796725",
    "fee": 2,
    "msg": "success"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 银行卡充值订单查询
此接口用于查询银行卡充值订单状态

**HTTP请求**

POST /card/merchant/recharge/order/query

**请求参数：**

| 参数     | 类型         | 是否必传 | 含义  |
|----------|------------|----------|:----|
| orderNo | String     | Y        | 订单号 |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "orderNo": "U898IPO8KH65445F"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| orderNo | String | Y        | 订单号  |
| cardNo | String | Y        | 卡号  |
| currency | String | Y        | 币种  |
| amount | BigDecimal | Y        | 充值金额  |
| fee | BigDecimal | Y        | 充值手续费  |
| receivedAmount | BigDecimal | Y | 到账金额  |
| receivedCurrency | String | Y | 到账币种  |
| exchangeRate | String | Y | 兑换汇率  |
| status | Integer | Y        | 充值状态(1-成功 2-失败 3-处理中)  |
| time | String | Y        | 充值时间  |


**响应示例：**

```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
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
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 银行卡充值记录查询
此接口用于查询银行卡充值的记录

**HTTP请求**

POST /card/merchant/recharge/query

**请求参数：**

| 参数     | 类型         | 是否必传 | 含义 |
|----------|------------|----------|:-----|
| uniqueId | String     | Y        | 合作商用户的唯一ID  |
| cardNo | String       | N        | 银行卡号  |
| currency | String     | N        | 币种(EUR/USDT)  |
| beginTime | datetime  | N        | 开始时间(yyyy-MM-dd HH:mm:ss)  |
| endTime   | datetime  | N        | 结束时间(yyyy-MM-dd HH:mm:ss)  |
| page       | Integer  | Y        | 页码  |
| pageSize | Integer    | Y        | 页大小  |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "currency": "USDT",
    "beginTime": "2024-07-01 00:00:00",
    "endTime": "2024-07-07 00:00:00",
    "page": 1,
    "pageSize": 10
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```
**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| total | Long | Y        | 总页码  |
| current | Long | Y        | 当前页  |
| records | List | Y        | 数据集合  |
| records[0].orderNo | String | Y        | 订单号  |
| records[0].cardNo | String | Y        | 卡号  |
| records[0].currency | String | Y        | 币种  |
| records[0].amount | BigDecimal | Y        | 充值金额  |
| records[0].fee | BigDecimal | Y        | 充值手续费  |
| records[0].receivedAmount | BigDecimal | Y | 到账金额  |
| records[0].receivedCurrency | String | Y | 到账币种  |
| records[0].exchangeRate | String | Y | 兑换汇率  |
| records[0].status | Integer | Y        | 充值状态(1-成功 2-失败 3-处理中)  |
| records[0].time | String | Y        | 充值时间  |


**响应示例：**

```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "total": 1,
    "current": 1,
    "records":
    [
      {
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
      }
    ]
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 银行卡余额查询
此接口用于查询银行卡余额

**HTTP请求**

POST /card/merchant/balance/query

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID  |
| cardNo | String | Y        | 银行卡号  |


**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| amount | String | Y        | 余额  |
| currency | String | Y        | 币种  |


**响应示例：**

```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  [
    {
      "amount": "1000.90",
      "currency": "USDT"
    },
    {
      "amount": "800",
      "currency": "EUR"
    }
  ],
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 银行卡找回密码
此接口用于发送找回密码请求，银行收到请求后根据注册的手机或邮箱发送链接进行设置

**HTTP请求**

POST /card/merchant/payment/password/set

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义                                                                                                                                                               |
|----------|--------|----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y        | 合作商用户的唯一ID                                                                                                                                                       |
| cardNo | String | Y        | 银行卡号                                                                                                                                                             |
| signaturePhoto | String | Y        | 用户签名照片(URL格式)。不能大于2M，支持格式.png, .jpeg, .jpg。银行卡所代表的卡片类型needPhotoForOperateCard=true时才需要。见接口/card/merchant/config/list参数needPhotoForOperateCard。请调用接口 /order/merchant/file/upload 获取图片路径  |


**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "signaturePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应示例：**

code 为0则代表设置成功
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
````

### 银行卡交易信息查询
此接口用于查询银行卡交易信息

**HTTP请求**

POST /card/merchant/trade/query

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID  |
| cardNo | String | Y        | 银行卡号  |
| currency | String | N        | 币种。支持EUR/USDT，默认EUR |
| beginDate | String | Y        | 开始日期(yyyy-MM-dd) |
| endDate | String | Y        | 结束日期(yyyy-MM-dd) 。起始结束时间最大间隔 30 天 |
| page | Long | Y        | 页码 |
| pageSize | Long | Y        | 页大小。最大 30 条 |

**请求示例：**
```JSON
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "currency": "EUR",
    "beginDate": "2024-01-01",
    "endDate": "2024-01-01",
    "pageNum": 1,
    "pageSize": 30
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义                                                                                      |
|----------|--------|----------|:----------------------------------------------------------------------------------------|
| total | Long | Y        | 总页码                                                                                     |
| current | Long | Y        | 当前页                                                                                     |
| records | List | Y        | 数据集合                                                                                    |
| records[0].cardNo | String | Y        | 卡号                                                                                      |
| records[0].currency | String | Y        | 币种                                                                                      |
| records[0].amount | BigDecimal | Y        | 金额                                                                                      |
| records[0].fee | BigDecimal | Y        | 手续费                                                                                     |
| records[0].txnAmount | BigDecimal | Y        | 支付金额                                                                                    |
| records[0].businessDate | String | Y        | 业务日期                                                                                    |
| records[0].tradeId | String | Y        | 交易流水号                                                                                   |
| records[0].tradeType | Integer | Y        | 交易类型。1-预授权；2-支付；3-充值；4-提现；5-转入；6-转出；7-结算调整；8-余额查询；9-手续费；10-消费；11-消费失败；12-退款；13-撤销；14-其他 |
| records[0].tradeTypeStr | String | Y        | 交易类型描述                                                                                  |
| records[0].tradeStatus | String | Y        | 交易状态。SUCCESS-成功；REVERSAL-冲正；REVERSED-被冲正；REJECTED-被撤销；CANCELLED-撤销；REFUND-退货            |
| records[0].tradeStatusStr | String | Y        | 交易状态描述                                                                                  |
| records[0].remark | String | Y        | 备注                                                                                      |

**响应示例：**
```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "total": 1,
    "current": 1,
    "records":
    [
      {
        "amount": 8000.0,
        "businessDate": 1710950400000,
        "cardNo": "5554748800001227",
        "currency": "EUR",
        "fee": 1.0,
        "remark": "购买衣物",
        "tradeStatus": "SUCCESS",
        "tradeStatusStr": "成功",
        "tradeType": 5,
        "tradeTypeStr": "转入",
        "txnAmount": 0.0
      }
    ]
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 银行卡冻结
此接口用于银行卡冻结

**HTTP请求**

POST /card/merchant/freeze

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义                                                                                                                                                               |
|----------|--------|----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y        | 合作商用户的唯一ID                                                                                                                                                       |
| cardNo | String | Y        | 银行卡号                                                                                                                                                             |
| signaturePhoto | String | N        | 用户签名照片(URL格式)。不能大于2M，支持格式.png, .jpeg, .jpg。银行卡所代表的卡片类型needPhotoForOperateCard=true时才需要。见接口/card/merchant/config/list参数needPhotoForOperateCard。请调用接口 /order/merchant/file/upload 获取图片路径  |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "signaturePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| status | Integer | Y        | 处理状态。0：处理中；1：成功；2：失败。当返回0时，会通过回调通知发送最终的处理状态。  |
| statusStr | String | Y        | 处理状态描述  |


**响应示例：**

code为0则代表冻结成功

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

### 银行卡解冻
此接口用于银行卡解冻

**HTTP请求**

POST /card/merchant/unfreeze

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义                                                                                                                                                               |
|----------|--------|----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniqueId | String | Y        | 合作商用户的唯一ID                                                                                                                                                       |
| cardNo | String | Y        | 银行卡号                                                                                                                                                             |
| signaturePhoto | String | N        | 用户签名照片(URL格式)。不能大于2M，支持格式.png, .jpeg, .jpg。银行卡所代表的卡片类型needPhotoForOperateCard=true时才需要。见接口/card/merchant/config/list参数needPhotoForOperateCard。请调用接口 /order/merchant/file/upload 获取图片路径  |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "U898IPO8KH65445F",
    "cardNo": "12456782323",
    "signaturePhoto": "https://waefdf23asdf.cloudfront.net/media/88888888/test.png"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| status | Integer | Y        | 处理状态。0：处理中；1：成功；2：失败。当返回0时，会通过回调通知发送最终的处理状态。  |
| statusStr | String | Y        | 处理状态描述  |


**响应示例：**

code为0代表解冻成功
```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "status": 1,
    "statusStr": "Success"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

## 全球速汇
### 查询速汇银行及相关配置
此接口用于查询支持全速速汇的所有国家及其每个银行需要填写和传递的参数、手续费率等数据

**HTTP请求**

POST /order/merchant/globalTransfer/queryBankConfig

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|


**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data": null,
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型         | 是否必传 | 含义                                 |
|----------|------------|----------|:-----------------------------------|
| country | String     | Y        | 速汇到账国家代码                           |
| countryName | String     | Y        | 速汇到账国家名称                               |
| currency | String     | Y        | 速汇到账货币代码                               |
| currencyName | String     | Y        | 速汇到账货币名称                               |
| bankList | List       | Y        | 速汇到账银行列表                               |
| bankList[0].bankId | Long       | Y        | 银行id                               |
| bankList[0].bankName | String     | Y        | 银行名称                               |
| bankList[0].paymentType | Long       | Y        | 交易类型。2：SEPA；15：速汇                  |
| bankList[0].feeRate | BigDecimal | Y        | 收取手续费率。例 2=2%                      |
| bankList[0].feeAmount | BigDecimal | Y   | 收取固定手续费金额。默认EUR                    |
| bankList[0].payeeParams | List       | Y        | 收款人信息请求参数。每个银行需要的参数不一样，返回哪些参数就需要传递 |
| payeeParams[0].benAccountNum | String     | Y        | 收款人账号                              |
| payeeParams[0].benAccountName | String     | Y        | 收款人户名                              |
| payeeParams[0].benCountryCode | String     | N        | 收款人居住国家                            |
| payeeParams[0].benCityCode | String     | N        | 收款人居住城市                            |
| payeeParams[0].benAddress | String     | N        | 收款人地址                              |
| payeeParams[0].benPostCode | String     | N        | 收款人邮编                              |
| payeeParams[0].benBankCode | String     | N        | 收款人银行编码                            |
| payeeParams[0].benTransBankSwift | String     | N        | 收款人中转行                             |
| payeeParams[0].benLastName | String     | N        | 收款人姓                               |
| payeeParams[0].benFirstName | String     | N        | 收款人名                               |
| payeeParams[0].benNationalityCountry | String     | N        | 收款人国籍                              |
| payeeParams[0].benIdNoType | String     | N        | 收款人证件类型                            |
| payeeParams[0].benIdNo | String     | N        | 收款人证件号码                            |
| payeeParams[0].benIdExpirationDate | String     | N  | 收款人证件有效期                           |
| payeeParams[0].benBirthday | String     | N        | 收款人出生日期                            |
| payeeParams[0].benMobileCode | String     | N        | 收款人手机区号                            |
| payeeParams[0].benMobile | String     | N        | 收款人手机号                             |
| payeeParams[0].benBankAccountType | String     | N   | 收款人银行账户类型                          |

**响应示例：**

```JSON
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  [
    {
      "country": "SG",
      "countryName": "新加坡",
      "currency": "SGD",
      "currencyName": "新加坡元",
      "bankList":
      [
        {
          "bankId": 2671,
          "bankName": "PayNow",
          "feeRate": 2,
          "feeAmount": 0.5,
          "payeeParams":
          [
            "benAccountNum",
            "benAccountName",
            "benFirstName",
            "benLastName"
          ]
        },
        {
          "bankId": 2670,
          "bankName": "Sing Investments & Finance Limited",
          "feeRate": 3,
          "feeAmount": 0.4,
          "payeeParams":
          [
            "benAccountNum",
            "benAccountName",
            "benFirstName",
            "benLastName"
          ]
        }
      ]
    }
  ],
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 查询法币汇率
此接口用于查询法币之间的汇率。供参考，最终实际到账以银行处理为准。

**HTTP请求**

POST /order/merchant/globalTransfer/getExchangeRate

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| currency | String | Y        | 交易货币。只支持EUR  |
| targetCurrency | String | Y   | 目标货币代码。接口/order/merchant/globalTransfer/queryBankConfig响应参数currency  |
| targetCountry | String | Y        | 目标国家代码。接口/order/merchant/globalTransfer/queryBankConfig响应参数country  |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "currency": "EUR",
    "targetCurrency": "SGD",
    "targetCountry": "SG"
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| currency | String | Y        | 交易货币 |
| targetCurrency | String | Y        | 目标货币代码 |
| exchangeRate | BigDecimal | Y        | 汇率 |

**响应示例：**

```json
{
  "code": 0,
  "message": "success",
  "success": true,
  "data":
  {
    "currency": "EUR",
    "targetCurrency": "SGD",
    "exchangeRate": 1.4568
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa"
}
```

### 代付校验
此接口用于全球速汇代付相关参数校验。

**HTTP请求**

POST /order/merchant/globalTransfer/paymentVerify

**限频：** 200/5s

**请求参数：**

    和代付接口参数一样

**响应示例：**

成功示例
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

失败示例
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

### 代付
此接口用于全球速汇代付。EUR速汇至其他法币

**HTTP请求**

POST /order/merchant/globalTransfer/payment

**限频：** 200/5s

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID。[1 .. 50 ] 个字符 |
| originOrderNo | String | Y | 商户业务订单号。[12 .. 30 ] 个字符 |
| amount | BigDecimal | Y  | 速汇金额。只支持EUR，最低金额100EUR，最多2位小数 |
| postscript | String | Y        | 附言。仅支持英文[5 .. 64 ] 个字符 |
| relationship | String | N        | 收付款人关系。当paymentType=15时需要。参见dictionary_biz.pdf（2.4. Relationship）|
| sourceFunds | String | N        | 资金来源。当paymentType=15时需要。参见dictionary_biz.pdf（2.5. SourceFunds） |
| payPurpose | String | N        | 付款目的。参见dictionary_biz.pdf（2.6. PayPurpose） |
| payer | Object | Y | 付款人信息对象 |
| payer.payerType | String | Y | 付款人类型。INDIVIDUAL：个人 |
| payer.payerLastName | String | Y | 付款人姓。不支持中文字符[1 .. 50 ] 个字符 |
| payer.payerFirstName | String | Y | 付款人名。不支持中文字符[1 .. 50 ] 个字符 |
| payer.payerIdNo | String | Y | 付款人证件号码。不支持中文字符[1 .. 60 ] 个字符 |
| payer.payerIdNoType | String | Y | 付款人证件类型。参见dictionary_biz.pdf（1.1. Idno Type） |
| payer.payerIdCountry | String | Y | 付款人证件颁发国家。[1 .. 10 ] 个字符。国家代码（2位数）。参见dictionary_common.xlsx（sheet. regin） |
| payer.payerBirthday | String | Y | 付款人出生日期。yyyy-MM-dd |
| payer.payerNationalityCountry | String | Y | 付款人国籍。[1 .. 10 ] 个字符。国家代码（2位数）。参见dictionary_common.xlsx（sheet. regin）|
| payer.payerMobile | String | Y | 付款人手机号码。例如：+37012345678。[1 .. 20 ] 个字符 |
| payer.payerCountryCode | String | Y | 付款人居住国家。[1 .. 10 ] 个字符。国家代码（2位数）。参见dictionary_common.xlsx（sheet. regin） |
| payer.payerCityCode | String | Y | 付款人居住城市代码。[1 .. 10 ] 个字符。参见dictionary_common.xlsx（sheet. city） |
| payer.payerAddress | String | Y | 付款人地址。不支持中文字符[1 .. 200 ] 个字符 |
| payer.payerPostCode | String | Y | 付款人邮编。[1 .. 20 ] 个字符 |
| payer.payerOccupation | String | Y | 付款人职业。仅支持英文[3 .. 20 ] 个字符 |
| payee | Object | Y | 收款人信息对象 |
| payee.benAccountNum | String | Y | 收款人帐号。会进行大写转换处理。不支持中文字符[2 .. 48 ] 个字符 |
| payee.benAccountName | String | Y | 收款人户名。英文名称[1 .. 100 ] 个字符 |
| payee.benCountryCode | String | N | 收款人居住国家。国家代码（2位数）。参见dictionary_common.xlsx（sheet. regin） |
| payee.benCityCode | String | N | 收款人居住城市代码。参见dictionary_common.xlsx（sheet. city） |
| payee.benAddress | String | N | 收款人地址。英文地址[10 .. 100 ] 个字符 |
| payee.benPostCode | String | N | 收款人邮编。[0 .. 20 ] 个字符 |
| payee.benBankCode | String | N | 收款人银行编码。[0 .. 20 ] 个字符 |
| payee.benTransBankSwift | String | N | 收款人中转行。[0 .. 50 ] 个字符 |
| payee.benLastName | String | N | 收款人姓。不支持中文字符[0 .. 60 ] 个字符 |
| payee.benFirstName | String | N | 收款人名。不支持中文字符[0 .. 60 ] 个字符 |
| payee.benNationalityCountry | String | N | 收款人国籍。国家代码（2位数）。参见dictionary_common.xlsx（sheet. regin） |
| payee.benIdNoType | String | N | 收款人证件类型。参见dictionary_biz.pdf（1.1. Idno type） |
| payee.benIdNo | String | N | 收款人证件号码。[0 .. 50 ] 个字符 |
| payee.benIdExpirationDate | String | N | 收款人证件有效期。不能小于当前时间 yyyy-MM-dd |
| payee.benBirthday | String | N | 收款人出生日期。yyyy-MM-dd |
| payee.benMobileCode | String | N | 收款人手机区号。例：+86。[0 .. 10 ] 个字符 |
| payee.benMobile | String | N | 收款人手机号。[0 .. 20 ] 个字符 |
| payee.benBankAccountType | String | N | 收款人银行账户类型。参见dictionary_biz.pdf（2.1. Bank account type）|

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
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
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| orderNo | Long | Y | PayouCard订单号 |
| originOrderNo | String | Y | 商户原始订单号 |
| status | String | Y | 支付状态。B1:待处理 |

**响应示例：**
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


### 用户订单列表
此接口用于查询用户的订单列表

**HTTP请求**

POST /order/merchant/globalTransfer/queryOrderPage

**限频：** 200/5s

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| uniqueId | String | Y        | 合作商用户的唯一ID。[1 .. 50 ] 个字符 |
| originOrderNo | String | N | 商户业务订单号 |
| orderNo | Long | N | Payoucard订单号 |
| page | Long | Y | 当前页。默认1 |
| pageSize | Long | Y | 每页数量。默认10，最大30 |


**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "uniqueId": "23242",
    "originOrderNo": null,
    "orderNo": 12345678934355462,
    "page": 1,
    "pageSize": 10
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**响应参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| total | Long | Y        | 总数量 |
| current | Long | Y | 当前页 |
| size | Long | Y  | 每页数量 |
| records | List | Y        | 数据 |
| records[0].uniqueId | String | Y        | 合作商用户的唯一ID|
| records[0].paymentType | String | Y        | 交易类型。2：SEPA；15：速汇 |
| records[0].orderNo | Long | Y        | Payoucard订单号 |
| records[0].originOrderNo | String | Y        | 商户订单号 |
| records[0].status | String | Y | 订单状态。B1：待处理；B2：支付中；B3：支付成功；B4：支付失败；B6：退款；B11：调单_待提交；B12：调单_待审核；B13：调单_审核通过；B15：调单_审核拒绝 |
| records[0].paymentCurrency | String | Y | 支付币种 |
| records[0].paymentAmount | BigDecimal | Y | 支付金额 |
| records[0].paymentFee | BigDecimal | Y | 支付总手续费 |
| records[0].paymentFeeRate | BigDecimal | Y | 手续费率。2=2% |
| records[0].paymentFeeRateAmount | BigDecimal | Y | 手续费率金额 |
| records[0].paymentFixedFeeAmount | BigDecimal | Y | 固定手续费金额 |
| records[0].receivedAccountNum | String | Y | 收款账号 |
| records[0].receivedAccountName | String | Y | 收款账户名 |
| records[0].receivedCurrency | String | Y | 收款币种 |
| records[0].receivedAmount | BigDecimal | Y | 收款金额 |
| records[0].receivedCountry | String | Y | 收款国家2位数代码 |
| records[0].receivedCountryStr | String | Y | 收款国家英文名 |
| records[0].resultMsg | String | N | 订单备注 |
| records[0].transferOrderInfo | List | N | 调单订单信息code |
| records[0].transferOrderFile | List | N | 调单订单文件code |
| records[0].transferOrderDesc | String | N | 调单订单描述 |
| records[0].postscript | String | Y | 交易附言 |
| records[0].payer | Object | Y | 付款人信息 |
| records[0].payer.payerType | String | Y | 付款人类型。INDIVIDUAL:个人 |
| records[0].payer.payerLastName | String | Y | 付款人姓 |
| records[0].payer.payerFirstName | String | Y | 付款人名 |
| records[0].payer.payerIdNo | String | Y | 付款人证件号码 |
| records[0].payer.payerIdNoType | String | Y | 付款人证件类型。EUROPEAN_ID.身份证; PASSPORT.护照 |
| records[0].payer.payerIdCountry | String | Y | 付款人证件颁发国家。2位数code码 |
| records[0].payer.payerBirthday | LocalDate | Y | 付款人出生日期 yyyy-MM-dd |
| records[0].payer.payerNationalityCountry | String | Y | 付款人国籍。2位数code码 |
| records[0].payer.payerMobile | String | Y | 付款人手机号码 |
| records[0].payer.payerCountryCode | String | Y | 付款人居住国家代码 |
| records[0].payer.payerCityCode | String | Y | 付款人居住城市代码 |
| records[0].payer.payerAddress | String | Y | 付款人地址.英文地址 |
| records[0].payer.payerPostCode | String | Y | 付款人邮编 |
| records[0].payer.payerOccupation | String | Y | 付款人职业 |
| records[0].payee | Object | Y | 收款人信息 |
| records[0].payee.bankId | Long | Y | 收款银行id |
| records[0].payee.bankName | String | Y | 收款银行 |
| records[0].payee.benAccountNum | String | Y | 收款人帐号 |
| records[0].payee.benAccountName | String | Y | 收款人户名。英文名称 |
| records[0].payee.benCountryCode | String | N | 收款人居住国家代码 |
| records[0].payee.benCityCode | String | N | 收款人居住城市代码 |
| records[0].payee.benAddress | String | N | address。英文地址 |
| records[0].payee.benPostCode | String | N | 邮编 |
| records[0].payee.benBankCode | String | N | 银行编码 |
| records[0].payee.benTransBankSwift | String | N | 中转行 |
| records[0].payee.benLastName | String | N | 收款人姓 |
| records[0].payee.benFirstName | String | N | 收款人名 |
| records[0].payee.benNationalityCountry | String | N | 国籍代码 |
| records[0].payee.benIdNoType | String | N | 证件类型。EUROPEAN_ID.身份证; PASSPORT.护照 |
| records[0].payee.benIdNo | String | N | 证件号码 |
| records[0].payee.benIdExpirationDate | LocalDate | N | 证件有效期。yyyy-MM-dd |
| records[0].payee.benBirthday | LocalDate | N | 出生日期。yyyy-MM-dd |
| records[0].payee.benBankAccountType | String | N | 收款人银行账户类型。参见dictionary_biz.pdf（2.1. Bank account type） |

**响应示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
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
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

### 提交调单信息或文件
此接口用于提交调单信息或文件。当订单状态为B11、B15时订单进入调单模式，需要提交订单相关的信息进行审核才能继续速汇。请结合调单回调通知配合使用

ps：每次请求请传递该订单所有的文件信息

**HTTP请求**

POST /order/merchant/globalTransfer/submitTransferOrderInfo

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义                                                           |
|----------|--------|----------|:-------------------------------------------------------------|
| orderNo | Long | Y | PayouCard订单号                                                 |
| transferInfos | List | N | 订单信息集合                                                       |
| transferInfos[0].code | String | N | 订单信息code。参见dictionary_biz.pdf（2.2. Transfer order info type） |
| transferInfos[0].content | String | N | 订单信息内容。不支持中文字符，最多200个字符                                      |
| transferFiles | List | N | 订单文件集合。参见dictionary_biz.pdf（2.3. Transfer order file type）   |
| transferFiles[0].code | String | N | 订单文件code                                                     |
| transferFiles[0].content | String | N | 订单文件url。每个文件不能超过2M                                           |

**请求示例：**

```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "orderNo": 1234556778945342,
    "transferInfos":
    [
      {
        "code": "1",
        "content": "tom"
      },
      {
        "code": "16",
        "content": "2002-10-10"
      }
    ],
    "transferFiles":
    [
      {
        "code": "26",
        "content": "https://adfasfwe.cloudfront.net/media/88888888/123.png"
      }
    ]
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|

**响应示例：**
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

### 查询订单结果
此接口用于查询订单结果

**HTTP请求**

POST /order/merchant/globalTransfer/getOrderResult

**限频：**50/5s  维度 merchantId + orderNo

**请求参数：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| orderNo  | Long   | Y        | PayouCard订单号 |

**请求示例：**
```json
{
  "requestId": "PYC20240325164529237",
  "marchantId": "88888888",
  "data":
  {
    "orderNo": 1234556778945342
  },
  "signture": "asfasdfjioasnfasdfasfiwaefasdfa"
}
```

**请求响应：**

| 参数     | 类型   | 是否必传 | 含义 |
|----------|--------|----------|:-----|
| orderNo  | Long   | Y        | PayouCard订单号 |
| status  | String   | Y  | 订单状态。B1：待处理；B2：支付中；B3：支付成功；B4：支付失败；B6：退款；B11：调单_待提交；B12：调单_待审核；B13：调单_审核通过；B15：调单_审核拒绝 |
| paymentCurrency  | String   | Y        | 支付币种 |
| paymentAmount  | BigDecimal   | Y        | 支付金额 |
| paymentFee  | BigDecimal   | Y        | 支付手续费 |
| receivedAccountNum  | String   | Y        | 收款账号 |
| receivedAccountName  | String   | Y        | 收款账户名 |
| receivedCurrency  | String   | Y        | 收款币种 |
| receivedAmount  | BigDecimal   | N       | 收款金额。当status = B3、B4、B6时返回 |
| resultMsg  | String   | N        | 消息 |
| transferOrderInfo  | List   | N        | 调单订单信息code。当status =B11、B15时返回。参见dictionary_biz.pdf（2.2. Transfer order info type） |
| transferOrderFile  | List   | N        | 调单订单文件code。当status =B11、B15时返回。参见dictionary_biz.pdf（2.3. Transfer order file type） |
| transferOrderDesc  | String   | N       | 调单订单描述 |

**响应示例：**

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

## 回调通知
### 设置回调通知url
此接口用于设置回调通知url。

请商户在商户后台系统中 商户基本信息 处设置回调通知地址。所有的回调通知共用此url，通过notifyType区分不同的通知内容

### 回调通知类型

| notifyType | 业务 | 含义 |
|------------|------|------|
| 1          | 全球速汇    | 支付成功、支付失败、退款通知    |
| 2          | 全球速汇    | 调单通知    |
| 3          | 银行卡    | 用户注册审核完成后回调通知    |
| 4          | 银行卡    | 卡片充值结果回调通知    |
| 5          | 银行卡    | 卡片激活结果回调通知    |
| 6          | 银行卡    | 卡片冻结、解冻处理状态回调通知    |

### 全球速汇-支付结果回调通知
此通知notifyType = 1

**回调参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| orderNo | Long    | Y    | PayouCard订单号 |
| status | String    | Y    | 订单状态。B3：支付成功；B4：支付失败；B6：退款； |
| paymentCurrency | String    | Y    | 支付币种 |
| paymentAmount  | BigDecimal   | Y        | 支付金额 |
| paymentFee  | BigDecimal   | Y        | 支付手续费 |
| receivedAccountNum  | String   | Y        | 收款账号 |
| receivedAccountName  | String   | Y        | 收款账户名 |
| receivedCurrency  | String   | Y        | 收款币种 |
| receivedAmount  | BigDecimal   | N       | 收款金额。当status = B3、B4、B6时返回 |
| resultMsg  | String   | N        | 消息 |

**回调示例：**

```JSON
{
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
    "resultMsg": "success"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
  "notifyType": 1
}
```

**响应参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| code | Integer    | Y    | 0。返回0后不会再重复发起回调通知 |
| message | String    | N    | 信息 |

**响应示例：**

```json
{
  "code": 0,
  "message": "success"
}
```

### 全球速汇-调单回调通知

此通知notifyType = 2

**回调参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| orderNo | Long    | Y    | PayouCard订单号 |
| status | String    | Y    | 订单状态。B11：调单_待提交；B12：调单_待审核；B13：调单_审核通过；B15：调单_审核拒绝 |
| transferOrderInfo | List    | N    | 调单订单信息。参见dictionary_biz.pdf（2.2. Transfer order info type） |
| transferOrderFile | List    | N    | 调单订单文件。参见dictionary_biz.pdf（2.3. Transfer order file type） |
| transferOrderDesc | String    | N    | 调单订单信息 |


**回调示例：**

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
**响应参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| code | Integer    | Y    | 0。返回0后不会再重复发起回调通知 |
| message | String    | N    | 信息 |

**响应示例：**
```json
{
  "code": 0,
  "message": "success"
}
```

### 用户KYC回调通知
此通知notifyType = 3

**回调参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| uniqueId | String    | Y    | 合作商用户的唯一ID |
| cardTypeId | Integer    | Y    | 卡片类型 |
| status | Integer    | Y    | 状态 |
| statusDesc | String    | Y    | 状态描述 |

**回调示例：**
```json
{
  "data":
  {
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

**响应参数：**
| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| code | Integer    | Y    | 0。返回0后不会再重复发起回调通知 |
| message | String    | N    | 信息 |

**响应示例：**
```json
{
  "code": 0,
  "message": "success"
}
```

### 银行卡-卡片充值结果回调通知
此通知notifyType = 4

**响应参数：**

| 参数             | 类型         | 是否必传 | 含义                    |
|----------------|------------|-----|:----------------------|
| cardNo         | String     | Y   | 卡号                    |
| status         | Integer    | Y   | 卡片充值状态。1：成功；2：失败；     |
| orderNo        | String       | Y   | 商户订单号          |
| currency       | String     | Y   | 币种                    |
| rechargeAmount | BigDecimal | Y   | 充值金额                  |
| receivedAmount | BigDecimal | N   | 到账金额                  |
| receivedCurrency | String | N   | 到账币种                  |
| exchangeRate | String | N   | 兑换汇率                  |
| fee            | BigDecimal | Y   | 手续费(扣充值金额之外的钱)        |
| msg            | String     | N   | 错误信息                  |

**回调示例：**

```json
{
  "data":
  {
    "cardNo": "12456782323",
    "status": 1,
    "orderNo": "2324a2dfga3435fg34353",
    "currency": "USDT",
    "rechargeAmount": 100,
    "receivedAmount": 99.67,
    "receivedCurrency": "USD",
    "exchangeRate": "0.99796725",
    "fee": 2,
    "msg": "success"
  },
  "requestId": "PYC20240325164529237",
  "merchantId": "88888888",
  "signature": "2sadfj23sanfinasdfnawesamdfasdfasdfwasfasdfa",
  "notifyType": 4
}
```

**响应参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| code | Integer    | Y    | 0。返回0后不会再重复发起回调通知 |
| message | String    | N    | 信息 |

**响应示例：**
```json
{
  "code": 0,
  "message": "success"
}
```

### 银行卡-卡片激活结果回调通知
此通知notifyType = 5

**回调参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| cardNo | String    | Y    | 卡号 |
| status | Integer    | Y    | 状态。5：激活成功；12：激活审核失败 |
| msg | String    | N    | 错误信息 |


**回调示例：**
```JSON
{
  "data":
  {
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

**响应参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| code | Integer    | Y    | 0。返回0后不会再重复发起回调通知 |
| message | String    | N    | 信息 |

**响应示例：**
```json
{
  "code": 0,
  "message": "success"
}
```

### 银行卡-卡片冻结、解冻处理状态回调通知
此通知notifyType = 6

**回调参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| uniqueId | String    | Y    | 合作商用户的唯一ID |
| cardNo | String    | Y    | 卡号 |
| status | Integer    | Y    | 状态。1: 成功；2: 失败 |
| requestType | Integer    | Y    | 类型。1: 冻结；2: 解冻 |

**回调示例：**

```json
{
  "data":
  {
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

**响应参数：**

| 参数 | 类型 | 是否必传 | 含义 |
|------|------|------|:------|
| code | Integer    | Y    | 0。返回0后不会再重复发起回调通知 |
| message | String    | N    | 信息 |

**响应示例：**
```json
{
  "code": 0,
  "message": "success"
}
```