---
author: Xansky
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: Microsoft Store 구매 API에서 이 메서드를 사용하여 지정된 사용자에게 무료 앱 또는 추가 기능에 대한 권한을 부여합니다.
title: 무료 제품에 대한 권한 부여
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 구매 API, 제품 권한 부여
ms.localizationpriority: medium
ms.openlocfilehash: 432d5976cb018148ba0f53aae6446a046f0a3b2f
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5158284"
---
# <a name="grant-free-products"></a>무료 제품에 대한 권한 부여

Microsoft Store 구매 API에서 이 메서드를 사용하여 지정된 사용자에게 무료 앱 또는 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)에 대한 권한을 부여합니다.

현재 무료 제품에 대해서만 권한을 부여할 수 있습니다. 서비스에서 이 메서드를 사용하여 유료 제품에 대해 권한을 부여하려고 시도하면 이 메서드가 오류를 반환합니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음이 필요합니다.

* 대상 URI 값이 `https://onestore.microsoft.com`인 Azure AD 액세스 토큰.
* Microsoft Store ID 키는 무료 제품 권한을 부여하려는 사용자의 ID를 나타냅니다.

자세한 내용은 [서비스에서 제품 권리 유형 관리](view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |


### <a name="request-header"></a>요청 헤더

| 헤더         | 유형   | 설명                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 권한 부여  | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다.                           |
| 호스트           | 문자열 | **purchase.mp.microsoft.com** 값으로 설정해야 합니다.                                            |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | 문자열 | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수      | 유형   | 설명        | 필수 |
|----------------|--------|---------------------|----------|
| availabilityId | 문자열 | Microsoft Store 카탈로그에서 권한을 부여할 제품의 가용성 ID입니다.         | 예      |
| b2bKey         | 문자열 | 제품의 권한을 부여하려는 사용자의 ID를 나타내는 [Microsoft Store ID 키](view-and-grant-products-from-a-service.md#step-4)입니다.    | 예      |
| devOfferId     | 문자열 | 구매 후 컬렉션 항목에 표시되는 개발자가 지정한 제품 ID입니다.        |
| language       | 문자열 | 사용자의 언어.  | 예      |
| 출시         | 문자열 | 사용자의 지역/국가입니다.       | 예      |
| orderId        | GUID   | 주문에 대해 생성된 GUID입니다. 이 값은 사용자에 대해 고유하지만 모든 주문에서 고유할 필요는 없습니다.    | 예      |
| productId      | 문자열 | Microsoft Store 카탈로그의 [제품](in-app-purchases-and-trials.md#products-skus-and-availabilities)에 대한 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. 제품에 대한 스토어 ID의 예는 9NBLGGH42CFD입니다. | 예      |
| quantity       | int    | 구매할 수량입니다. 현재, 1 값만 지원됩니다. 지정되지 않은 경우 기본값은 1입니다.   | 아니요       |
| skuId          | 문자열 | Microsoft Store 카탈로그의 제품 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities)에 대한 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. SKU에 대한 스토어 ID의 예는 0010입니다.     | 예      |


### <a name="request-example"></a>요청 예제

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 매개 변수                 | 유형                        | 설명             | 필수 |
|---------------------------|-----------------------------|-----------------------|----------|
| clientContext             | ClientContextV6             | 이 주문에 대한 클라이언트 문맥 정보입니다. 이 정보는 Azure AD 토큰의 *clientID* 값에 할당됩니다.    | 예      |
| createdtime               | datetimeoffset              | 주문한 시간입니다.         | 예      |
| currencyCode              | 문자열                      | *totalAmount* 및 *totalTaxAmount*에 대한 통화 코드입니다. 무료 항목의 경우 해당되지 않습니다.     | 예      |
| FriendlyName              | 문자열                      | 주문의 식별 이름입니다. Microsoft Store 구매 API를 사용하여 생성된 주문에 대해서는 N/A입니다. | 예      |
| isPIRequired              | boolean                     | 결제 방법(PI)이 구매 주문의 일부로 필요한지를 나타냅니다.  | 예      |
| 언어                  | 문자열                      | 주문에 대한 언어 ID입니다(예: “en”).       | 예      |
| 출시                    | 문자열                      | 주문에 대한 지역/국가 ID입니다(예: “US”).  | 예      |
| orderId                   | 문자열                      | 특정 사용자의 주문을 식별하는 ID입니다.                | 예      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | 주문에 대한 품목 목록입니다. 일반적으로 주문당 1개 품목입니다.       | 예      |
| orderState                | 문자열                      | 주문 상태입니다. 유효한 상태는 **Editing**,**CheckingOut**, **Pending**, **Purchased**, **Refunded**,**ChargedBack** 및 **Cancelled**입니다. | 예      |
| orderValidityEndTime      | 문자열                      | 주문서 제출 전에 주문 가격 설정이 유효한 마지막 시간입니다. 무료 앱의 경우 해당되지 않습니다.      | 예      |
| orderValidityStartTime    | 문자열                      | 주문서 제출 전에 주문 가격 설정이 유효한 처음 시간입니다. 무료 앱의 경우 해당되지 않습니다.          | 예      |
| purchaser                 | IdentityV6                  | 구매자의 ID를 설명하는 개체입니다.       | 예      |
| totalAmount               | 10진수                     | 세금을 포함한 모든 주문 항목의 총 구매 금액입니다.       | 예      |
| totalAmountBeforeTax      | 10진수                     | 세금을 포함하지 않은 모든 주문 항목의 총 구매 금액입니다.      | 예      |
| totalChargedToCsvTopOffPI | 10진수                     | 별도의 결제 방법이나 CSV(저장된 값)를 사용하는 경우 CSV로 청구되는 금액입니다.            | 예      |
| totalTaxAmount            | 10진수                     | 모든 품목에 대한 세금의 총 금액입니다.    | 예      |


ClientContext 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수 | 유형   | 설명                           | 필수 |
|-----------|--------|---------------------------------------|----------|
| client    | 문자열 | 주문한 클라이언트의 ID입니다. | 아니요       |


OrderLineItemV6 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수               | 유형           | 설명                                                                                                  | 필수 |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agent                   | IdentityV6     | 품목을 마지막으로 편집한 에이전트입니다. 이 개체에 대한 자세한 내용은 아래 표를 참조하세요.       | 아니요       |
| availabilityId          | 문자열         | Microsoft Store 카탈로그에서 구매할 제품의 가용성 ID입니다.                           | 예      |
| beneficiary             | IdentityV6     | 주문 수취인 ID입니다.                                                                | 아니요       |
| billingState            | 문자열         | 주문의 청구 상태입니다. 완료되면 이 상태가 **Charged**로 설정됩니다.                                   | 아니요       |
| campaignId              | 문자열         | 이 주문에 대한 캠페인 ID입니다.                                                                              | 아니요       |
| currencyCode            | 문자열         | 가격에 사용되는 통화 코드입니다.                                                                    | 예      |
| 설명             | 문자열         | 품목에 대한 지역화된 설명입니다.                                                                    | 예      |
| devofferId              | 문자열         | 특정 주문이 있을 경우 제품 ID입니다.                                                           | 아니요       |
| fulfillmentDate         | datetimeoffset | 처리된 날짜입니다.                                                                           | 아니요       |
| fulfillmentState        | 문자열         | 이 항목의 처리 상태입니다. 완료되면 이 상태가 **Fulfilled**로 설정됩니다.                      | 아니요       |
| isPIRequired            | boolean        | 이 품목에 결제 방법이 필요한지를 나타냅니다.                                       | 예      |
| isTaxIncluded           | boolean        | 해당 항목의 가격에 세금이 포함되는지를 나타냅니다.                                        | 예      |
| legacyBillingOrderId    | 문자열         | 레거시 청구 ID입니다.                                                                                       | 아니요       |
| lineItemId              | 문자열         | 이 주문의 항목에 대한 품목 ID입니다.                                                                 | 예      |
| listPrice               | 10진수        | 이 주문에 있는 항목의 정가입니다.                                                                    | 예      |
| productId               | 문자열         | Microsoft Store 카탈로그에서 품목을 나타내는 [제품](in-app-purchases-and-trials.md#products-skus-and-availabilities)에 대한 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. 제품에 대한 Store ID의 예는 9NBLGGH42CFD입니다.   | 예      |
| productType             | 문자열         | 제품 유형입니다. 지원되는 값은 **Durable**, **Application** 및 **UnmanagedConsumable**입니다. | 예      |
| quantity                | int            | 주문한 항목의 수량입니다.                                                                            | 예      |
| retailPrice             | 10진수        | 주문 항목의 소매 가격입니다.                                                                        | 예      |
| revenueRecognitionState | 문자열         | 수익 인식 상태입니다.                                                                               | 예      |
| skuId                   | 문자열         | Microsoft Store 카탈로그에서 품목을 나타내는 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities)에 대한 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. SKU에 대한 Store ID의 예는 0010입니다.                                                                   | 예      |
| taxAmount               | 10진수        | 품목에 대한 세금 금액입니다.                                                                            | 예      |
| taxType                 | 문자열         | 해당 세금에 대한 세금 유형입니다.                                                                       | 예      |
| 제목                   | 문자열         | 품목의 지역화된 제목입니다.                                                                        | 예      |
| totalAmount             | 10진수        | 세금을 포함한 품목의 총 구매 금액입니다.                                                    | 예      |


IdentityV6 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수     | 유형   | 설명                                                                        | 필수 |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | 문자열 | **"pub"** 값을 포함합니다.                                                      | 예      |
| identityValue | 문자열 | 지정된 Microsoft Store ID 키에 있는 *publisherUserId*의 문자열 값입니다. | 예      |


### <a name="response-example"></a>응답 예제

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## <a name="error-codes"></a>오류 코드


| 코드 | 오류        | 내부 오류 코드           | 설명   |
|------|--------------|----------------------------|----------------|
| 401  | 권한 없음 | AuthenticationTokenInvalid | Azure AD 액세스 토큰이 잘못되었습니다. ServiceError의 세부 정보에 토큰이 만료되거나 *appid* 클레임이 누락되는 경우와 같은 자세한 정보가 포함되는 경우도 있습니다. |
| 401  | 권한 없음 | PartnerAadTicketRequired   | Azure AD 액세스 토큰이 권한 부여 헤더의 서비스에 전달되지 않았습니다.   |
| 401  | 권한 없음 | InconsistentClientId       | 요청 본문에서 Microsoft Store ID 키의 *clientId* 클레임과 권한 부여 헤더에서 Azure AD 액세스 토큰의 *appid* 클레임이 일치하지 않습니다.       |
| 400  | 불량 요청   | InvalidParameter           | 요청 본문에 대한 정보 및 어떤 필드에 잘못된 값이 있는지에 대한 정보가 세부 사항에 포함됩니다.           |

<span/> 

## <a name="related-topics"></a>관련 항목

* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [Microsoft Store ID 키 갱신](renew-a-windows-store-id-key.md)
