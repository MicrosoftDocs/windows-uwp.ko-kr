---
author: mcleanbyron
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: "Windows 스토어 컬렉션 API에서 이 메서드를 사용하여 Azure AD 클라이언트 ID와 연관된 앱에 대해 고객이 소유한 모든 제품을 가져옵니다. 특정 제품으로 쿼리의 범위를 지정하거나 다른 필터를 사용할 수 있습니다."
title: "제품에 대한 쿼리"
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: b8661d73487dde61b207159d11a0583700fa22bc

---

# 제품에 대한 쿼리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 스토어 컬렉션 API에서 이 메서드를 사용하여 Azure AD 클라이언트 ID와 연관된 앱에 대해 고객이 소유한 모든 제품을 가져옵니다. 특정 제품으로 쿼리의 범위를 지정하거나 다른 필터를 사용할 수 있습니다.

이 메서드는 앱에서 메시지에 대한 응답으로 서비스 호출하도록 설계되었습니다. 서비스는 일정에 있는 사용자에 대해 정기적으로 폴링하지 않습니다.

## 필수 조건


이 메서드를 사용하려면 다음이 필요합니다.

-   `https://onestore.microsoft.com` 대상 URI를 사용하여 만든 Azure AD 액세스 토큰
-   앱의 클라이언트 쪽 코드에서 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 메서드를 호출하여 생성된 Windows 스토어 ID 키

자세한 내용은 [서비스에서 제품 보기 및 권한 부여](view-and-grant-products-from-a-service.md)를 참조하세요.

## 요청

### 요청 구문

| 메서드 | 요청 URI                                                 |
|--------|-------------------------------------------------------------|
| POST   | `https://collections.mp.microsoft.com/v6.0/collections/query` |

<br/>
 
### 요청 헤더

| 헤더         | 유형   | 설명                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 권한 부여  | 문자열 | 필수. **Bearer**&lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다.                           |
| 호스트           | 문자열 | **collections.mp.microsoft.com** 값으로 설정해야 합니다.                                            |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | 문자열 | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |

 <br/>

### 요청 본문

| 매개 변수         | 유형         | 설명                                                                                                                                                                                                                                                          | 필수 |
|-------------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| beneficiaries     | UserIdentity | 제품에 대해 쿼리 중인 사용자를 나타내는 UserIdentity 개체입니다.                                                                                                                                                                                           | 예      |
| continuationToken | 문자열       | 제품 집합이 여러 개 있는 경우 페이지 제한에 도달할 때 응답 본문이 연속 토큰을 반환합니다. 나머지 제품을 검색하는 후속 호출에서 그 연속 토큰을 제공합니다.                                                      | 아니요       |
| maxPageSize       | 숫자       | 하나의 응답에 반환하는 제품의 최대 수입니다. 기본 및 최대 값은 100입니다.                                                                                                                                                                      | 아니요       |
| modifiedAfter     | datetime     | 지정한 경우 서비스는 이 날짜 이후 수정된 제품만 반환합니다.                                                                                                                                                                             | 아니요       |
| parentProductId   | 문자열       | 지정한 경우 서비스는 지정된 앱에 해당하는 IAP만 반환합니다.                                                                                                                                                                                    | 아니요       |
| productSkuIds     | ProductSkuId | 지정한 경우 서비스는 제공된 Product/SKU 쌍에 해당하는 제품만 반환합니다.                                                                                                                                                                        | 아니요       |
| productTypes      | 문자열       | 지정한 경우 서비스는 지정된 제품 형식과 일치하는 제품만 반환합니다. 지원 되는 제품 유형은 **Application**, **Durable**, 및 **UnmanagedConsumable**입니다.                                                                                       | 아니요       |
| validityType      | 문자열       | **All**로 설정된 경우 만료된 항목을 포함하여 사용자의 모든 제품이 반환됩니다. **Valid**로 설정된 경우 이 시점에 유효한 제품만 반환됩니다(즉, 현재 활성 상태인 제품, 시작 날짜가 &lt;지금 이전인 제품, 종료 날짜가 &gt;지금 이후인 제품이 있습니다). | 아니요       |

<br/> 

UserIdentity 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수            | 유형   | 설명                                                                                                                                                                                                                  | 필수 |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | 문자열 | 문자열 값 **b2b**를 지정합니다.                                                                                                                                                                                            | 예      |
| identityValue        | 문자열 | Windows 스토어 ID 키의 문자열 값입니다.                                                                                                                                                                                    | 예      |
| localTicketReference | 문자열 | 반환된 제품에 대해 요청된 식별자입니다. 응답 본문에 반환된 항목에는 일치하는 *localTicketReference*가 있습니다. Windows 스토어 ID 키의 *userId* 클레임과 동일한 값을 사용하는 것이 좋습니다. | 예      |

<br/> 

ProductSkuId 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수 | 유형   | 설명                                                                                                                                                                                                                                                                                                            | 필수 |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| productId | 문자열 | Windows 스토어 카탈로그의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 사용할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. | 예      |
| skuID     | 문자열 | Windows 스토어 카탈로그의 SKU ID입니다. SKU ID의 예로는 "0010"이 있습니다.                                                                                                                                                                                                                                                | 예      |

<br/> 

### 요청 예제

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
    "maxPageSize": 100,
    "beneficiaries": [
            {
                "localTicketReference": "1055521810674918",
                "identityValue": "eyJ0eXAiOiJ……",
                "identityType": "b2b"
            }
        ],
    "modifiedAfter": "\/Date(-62135568000000)\/",
    "productSkuIds": [
            {
                "productId": "9NBLGGH5WVP6",
                "skuId": "0010"
            }
        ],
    "productTypes": [
            "UnmanagedConsumable"
        ],
    "validityType": "All"
}
```

## 응답


### 응답 본문

| 매개 변수         | 유형                     | 설명                                                                                                                                                                                | 필수 |
|-------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| continuationToken | 문자열                   | 제품 집합이 여러 개 있는 경우 페이지 제한에 도달할 때 이 토큰이 반환됩니다. 나머지 제품을 검색하는 후속 호출에서 그 연속 토큰을 지정할 수 있습니다. | 아니요       |
| 항목             | CollectionItemContractV6 | 사용자가 지정된 제품의 배열입니다.                                                                                                                                               | 아니요       |

<br/> 

CollectionItemContractV6 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수            | 유형               | 설명                                                                                                                                        | 필수 |
|----------------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| acquiredDate         | datetime           | 사용자가 항목을 획득한 날짜입니다.                                                                                                      | 예      |
| campaignId           | 문자열             | 구매 시 이 항목에 대해 제공된 캠페인 ID입니다.                                                                                  | 아니요       |
| devOfferId           | 문자열             | 앱에서 바로 구매 시 제공되는 ID입니다.                                                                                                              | 아니요       |
| endDate              | datetime           | 항목의 종료 날짜입니다.                                                                                                                          | 예      |
| fulfillmentData      | 문자열             | 해당 없음                                                                                                                                                | 아니요       |
| inAppOfferToken      | 문자열             | Windows 개발자 센터 대시보드에서 항목에 할당되는 개발자가 지정한 제품 ID 문자열입니다. 제품 ID의 예로는 "product123"이 있습니다. | 아니요       |
| itemId               | 문자열             | 사용자가 소유한 다른 항목에서 이 컬렉션 항목을 식별하는 ID입니다. 이 ID는 제품마다 고유합니다.                                          | 예      |
| localTicketReference | 문자열             | 요청 본문에서 이전에 제공한 `localTicketReference`의 ID입니다.                                                                      | 예      |
| modifiedDate         | datetime           | 이 항목을 마지막으로 수정한 날짜입니다.                                                                                                              | 예      |
| orderId              | 문자열             | 있는 경우 이 항목을 받은 주문 ID입니다.                                                                                          | 아니요       |
| orderLineItemId      | 문자열             | 있는 경우 이 항목을 받은 특정 주문의 품목입니다.                                                                | 아니요       |
| ownershipType        | 문자열             | 문자열 "OwnedByBeneficiary"입니다.                                                                                                                   | 예      |
| productId            | 문자열             | Windows 스토어 카탈로그의 앱에 대한 스토어 ID입니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다.                                                            | 예      |
| productType          | 문자열             | 다음 제품 유형 중 하나: **Application**, **Durable**, 및 **UnmanagedConsumable**.                                                     | 예      |
| purchasedCountry     | 문자열             | 해당 없음.                                                                                                                                               | 아니요       |
| purchaser            | IdentityContractV6 | 있는 경우 항목 구매자의 ID입니다. 아래에 나오는 이 개체에 대한 세부 정보를 참조하세요.                                      | 아니요       |
| Quantity             | 숫자             | 항목의 수량입니다. 현재 수량은 항상 1입니다.                                                                                        | 아니요       |
| skuId                | 문자열             | Windows 스토어 카탈로그의 SKU ID입니다. SKU ID의 예로는 "0010"이 있습니다.                                                                            | 예      |
| skuType              | 문자열             | SKU의 유형입니다. 가능한 값은 **Trial**, **Full** 및 **Rental**입니다.                                                                      | 예      |
| startDate            | datetime           | 항목이 유효한 시작 날짜입니다.                                                                                                         | 예      |
| 상태               | 문자열             | 항목의 상태입니다. 가능한 값은 **Active**, **Expired**, **Revoked** 및 **Banned**입니다.                                              | 예      |
| 태그                 | 문자열             | 해당 없음                                                                                                                                                | 예      |
| transactionId        | GUID               | 이 항목의 구매 결과인 트랜잭션 ID입니다. 항목을 처리됨으로 보고하는 데 사용할 수 있습니다.                                       | 예      |

<br/> 

IdentityContractV6 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수     | 유형   | 설명                                                                        | 필수 |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | 문자열 | **"pub"** 값을 포함합니다.                                                      | 예      |
| identityValue | 문자열 | 지정된 Windows 스토어 ID 키에 있는 *publisherUserId*의 문자열 값입니다. | 예      |

<br/> 

### 응답 예제

```syntax
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
    "items" : [
        {
            "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
            "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
            "endDate" : "9999-12-31T23:59:59.9999999+00:00",
            "fulfillmentData" : [],
            "inAppOfferToken" : "consumable2",
            "itemId" : "4b8fbb13127a41f299270ea668681c1d",
            "localTicketReference" : "1055521810674918",
            "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
            "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
            "ownershipType" : "OwnedByBeneficiary",
            "productId" : "9NBLGGH5WVP6",
            "productType" : "UnmanagedConsumable",
            "purchaser" : {
                "identityType" : "pub",
                "identityValue" : "user123"
            },
            "skuId" : "0010",
            "skuType" : "Full",
            "startDate" : "2015-09-22T19:22:51.2068724+00:00",
            "status" : "Active",
            "tags" : [],
            "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
        }
    ]
}
```

## 관련 항목

* [서비스에서 제품 보기 및 권한 부여](view-and-grant-products-from-a-service.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
* [Windows 스토어 ID 키 갱신](renew-a-windows-store-id-key.md)



<!--HONumber=Jun16_HO4-->


