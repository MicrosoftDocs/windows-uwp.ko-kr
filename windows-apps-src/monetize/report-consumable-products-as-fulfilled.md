---
author: Xansky
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Microsoft Store 컬렉션 API에서 이 메서드를 사용하여 지정된 고객에 대해 소모성 제품을 처리됨으로 보고합니다. 사용자가 소모성 제품을 다시 구입하려면 앱 또는 서비스에서 해당 사용자에 대해 소모성 제품이 처리됨으로 보고되어야 합니다.
title: 소모성 제품을 처리됨으로 보고
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 컬렉션 API, 처리, 소모성
ms.localizationpriority: medium
ms.openlocfilehash: 2cbacd35a25e8eaf9673d118fcbece835572e289
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4685295"
---
# <a name="report-consumable-products-as-fulfilled"></a>소모성 제품을 처리됨으로 보고

Microsoft Store 컬렉션 API에서 이 메서드를 사용하여 지정된 고객에 대해 소모성 제품을 처리됨으로 보고합니다. 사용자가 소모성 제품을 다시 구입하려면 앱 또는 서비스에서 해당 사용자에 대해 소모성 제품이 처리됨으로 보고되어야 합니다.

두 가지 방법으로 이 메서드를 사용하여 소모성 제품을 처리됨으로 보고할 수 있습니다.

* 소모성 제품의 항목 ID와([제품에 대한 쿼리](query-for-products.md)의 **itemId** 매개 변수에 반환된 ID) 고유한 추적 ID를 제공합니다. 동일한 추적 ID가 여러 번 사용되는 경우 항목이 이미 사용된 경우에도 동일한 결과가 반환됩니다. 사용 요청이 성공했는지 확실하지 않으면, 서비스에서 동일한 추적 ID를 사용하여 사용 요청을 다시 제출해야 합니다. 추적 ID는 항상 사용 요청과 연결되며 무제한으로 다시 제출할 수 있습니다.
* 제품 ID([제품에 대한 쿼리](query-for-products.md)의 **productId** 매개 변수에 반환된 ID) 및 아래 요청 본문 섹션의 **transactionId** 매개 변수에 대한 설명에 나열된 소스 중 하나에서 가져온 트랜잭션 ID를 제공합니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음이 필요합니다.

* 대상 URI 값이 `https://onestore.microsoft.com`인 Azure AD 액세스 토큰.
* 소모성 제품을 처리됨으로 보고할 사용자의 ID를 나타내는 Microsoft Store ID 키.

자세한 내용은 참조 [서비스에서 제품 권리 유형 관리](view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>요청 헤더

| 헤더         | 유형   | 설명                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 권한 부여  | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다.                           |
| 호스트           | 문자열 | **collections.mp.microsoft.com** 값으로 설정해야 합니다.                                            |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | 문자열 | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수     | 유형         | 설명         | 필수 |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | 이 항목을 사용 중인 사용자입니다. 자세한 내용은 다음 표를 참조하세요.        | 예      |
| itemId        | 문자열       | [제품에 대한 쿼리](query-for-products.md)에 의해 반환되는 *itemId* 값. 이 매개 변수를 *trackingId*에 사용하지 마세요.      | 아니요       |
| trackingId    | guid         | 개발자가 제공하는 고유한 추적 ID입니다. 이 매개 변수를 *itemId*에 사용하지 마세요.         | 아니요       |
| productId     | 문자열       | [제품에 대한 쿼리](query-for-products.md)에 의해 반환되는 *productId* 값. 이 매개 변수를 *transactionId*에 사용하지 마세요.   | 아니요       |
| transactionId | guid         | 다음 소스 중 하나에서 가져온 트랜잭션 ID 값입니다. 이 매개 변수를 *productId*에 사용하지 마세요.<ul><li>[PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 클래스의 [TransactionID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) 속성.</li><li>[RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 또는 [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync)에서 반환되는 앱 또는 제품 영수증입니다.</li><li>[제품에 대한 쿼리](query-for-products.md)에 의해 반환되는 *transactionId* 매개 변수.</li></ul>   | 아니요       |


UserIdentity 개체에는 다음 매개 변수가 포함됩니다.

| 매개 변수            | 유형   | 설명       | 필수 |
|----------------------|--------|-------------------|----------|
| identityType         | 문자열 | 문자열 값 **b2b**를 지정합니다.    | 예      |
| identityValue        | 문자열 | 소모성 제품을 처리됨으로 보고할 사용자의 ID를 나타내는 [Microsoft Store ID 키](view-and-grant-products-from-a-service.md#step-4)입니다.      | 예      |
| localTicketReference | 문자열 | 반환된 응답에 대해 요청된 식별자입니다. Microsoft Store ID 키의 *userId* [클레임](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key)과 동일한 값을 사용하는 것이 좋습니다. | 예      |


### <a name="request-examples"></a>요청 예제

다음 예에서는 *itemId* 및 *trackingId*를 사용합니다.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

다음 예에서는 *productId* 및 *transactionId*를 사용합니다.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>응답

사용이 성공적으로 실행된 경우 콘텐츠가 반환되지 않습니다.

### <a name="response-example"></a>응답 예제

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>오류 코드


| 코드 | 오류        | 내부 오류 코드           | 설명           |
|------|--------------|----------------------------|-----------------------|
| 401  | 권한 없음 | AuthenticationTokenInvalid | Azure AD 액세스 토큰이 잘못되었습니다. ServiceError의 세부 정보에 토큰이 만료되거나 *appid* 클레임이 누락되는 경우와 같은 자세한 정보가 포함되는 경우도 있습니다. |
| 401  | 권한 없음 | PartnerAadTicketRequired   | Azure AD 액세스 토큰이 권한 부여 헤더의 서비스에 전달되지 않았습니다.                                                                                                   |
| 401  | 권한 없음 | InconsistentClientId       | 요청 본문에서 Microsoft Store ID 키의 *clientId* 클레임과 권한 부여 헤더에서 Azure AD 액세스 토큰의 *appid* 클레임이 일치하지 않습니다.                     |

<span/> 

## <a name="related-topics"></a>관련 항목

* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [제품에 대한 쿼리](query-for-products.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
* [Microsoft Store ID 키 갱신](renew-a-windows-store-id-key.md)
