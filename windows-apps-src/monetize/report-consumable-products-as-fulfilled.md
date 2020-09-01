---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Microsoft Store collection API에서이 메서드를 사용 하 여 지정 된 고객에 대해 사용 가능한 제품을 보고 합니다. 사용자가 사용할 수 있는 제품을 다시 구입 하려면 앱 이나 서비스에서 해당 사용자에 대해 충족 된 사용 가능 제품을 보고 해야 합니다.
title: 소모성 제품을 처리됨으로 보고
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store collection API, 충족, 소비
ms.localizationpriority: medium
ms.openlocfilehash: 88ff4f9bd2c490c8fae4deb2cfa4cbf5c74956c8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174947"
---
# <a name="report-consumable-products-as-fulfilled"></a>소모성 제품을 처리됨으로 보고

Microsoft Store collection API에서이 메서드를 사용 하 여 지정 된 고객에 대해 사용 가능한 제품을 보고 합니다. 사용자가 사용할 수 있는 제품을 다시 구입 하려면 앱 이나 서비스에서 해당 사용자에 대해 충족 된 사용 가능 제품을 보고 해야 합니다.

다음 두 가지 방법으로이 방법을 사용 하 여 사용 가능한 제품을 충족 된 것으로 보고할 수 있습니다.

* [제품 쿼리의](query-for-products.md) **itemId** 매개 변수에서 반환 되는 사용할 수 있는 항목 id 및 사용자가 제공 하는 고유 추적 id를 제공 합니다. 여러 시도에 동일한 추적 ID를 사용 하는 경우 항목이 이미 사용 되는 경우에도 동일한 결과가 반환 됩니다. 사용 요청이 성공 했는지 확실 하지 않은 경우 서비스는 동일한 추적 ID를 사용 하 여 사용 요청을 다시 제출 해야 합니다. 추적 ID는 항상 요청을 사용 하는에 연결 되며 무기한 다시 전송 될 수 있습니다.
* 제품 [에 대 한 쿼리의](query-for-products.md) **productId** 매개 변수에서 반환 되는 제품 id와 아래의 요청 본문 섹션에 있는 **transactionId** 매개 변수에 대 한 설명에 나열 된 원본 중 하나에서 가져온 트랜잭션 id를 제공 합니다.

## <a name="prerequisites"></a>필수 구성 요소


이 방법을 사용 하려면 다음이 필요 합니다.

* 대상 URI 값을 포함 하는 Azure AD 액세스 토큰입니다 `https://onestore.microsoft.com` .
* 사용 가능한 제품을 충족 된 것으로 보고 하려는 사용자의 id를 나타내는 Microsoft Store ID 키입니다.

자세한 내용은 [서비스에서 제품 자격 관리](view-and-grant-products-from-a-service.md)를 참조 하세요.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>요청 헤더

| header         | 유형   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 권한 부여  | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다.                           |
| 호스트           | 문자열 | **Collections.mp.microsoft.com**값으로 설정 해야 합니다.                                            |
| Content-Length | number | 요청 본문의 길이입니다.                                                                       |
| 콘텐츠 형식   | 문자열 | 요청 및 응답 유형을 지정 합니다. 현재 유일 하 게 지원 되는 값은 **application/json**입니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수     | 형식         | Description         | 필수 |
|---------------|--------------|---------------------|----------|
| 수혜자   | UserIdentity | 이 항목을 사용 중인 사용자입니다. 자세한 내용은 다음 표를 참조하세요.        | 예      |
| itemId        | 문자열       | [제품에 대 한 쿼리에서](query-for-products.md)반환 된 *itemId* 값입니다. *TrackingId* 에이 매개 변수 사용      | 예       |
| trackingId    | guid         | 개발자가 제공 하는 고유 추적 ID입니다. *ItemId*에이 매개 변수를 사용 합니다.         | 예       |
| productId     | 문자열       | [제품에 대 한 쿼리에서](query-for-products.md)반환 된 *productId* 값입니다. *TransactionId* 에이 매개 변수 사용   | 예       |
| transactionId | guid         | 다음 원본 중 하나에서 가져온 트랜잭션 ID 값입니다. *ProductId*에이 매개 변수를 사용 합니다.<ul><li>[PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) 클래스의 [TransactionID](/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) 속성입니다.</li><li>[RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)또는 [GetAppReceiptAsync](/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync)에서 반환 되는 앱 또는 제품 수신입니다.</li><li>[제품에 대 한 쿼리에서](query-for-products.md)반환 된 *transactionId* 매개 변수입니다.</li></ul>   | 예       |


UserIdentity 개체에는 다음 매개 변수가 포함 됩니다.

| 매개 변수            | 형식   | Description       | 필수 |
|----------------------|--------|-------------------|----------|
| identityType         | 문자열 | 문자열 값 **b2b**를 지정 합니다.    | 예      |
| identityValue        | 문자열 | 사용 가능한 제품을 충족 된 것으로 보고 하려는 사용자의 id를 나타내는 [MICROSOFT STORE id 키](view-and-grant-products-from-a-service.md#step-4) 입니다.      | 예      |
| localTicketReference | 문자열 | 반환 된 응답에 대해 요청 된 식별자입니다. Microsoft Store ID 키에서 *userId*  [클레임](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key) 과 동일한 값을 사용 하는 것이 좋습니다. | 예      |


### <a name="request-examples"></a>요청 예제

다음 예에서는 *itemId* 및 *trackingId*를 사용 합니다.

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

다음 예에서는 *productId* 및 *transactionId*를 사용 합니다.

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

소비를 성공적으로 실행 한 경우 콘텐츠가 반환 되지 않습니다.

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


| 코드 | 오류        | 내부 오류 코드           | Description           |
|------|--------------|----------------------------|-----------------------|
| 401  | 권한 없음 | AuthenticationTokenInvalid | Azure AD 액세스 토큰이 잘못 되었습니다. 일부 경우에는 토큰이 만료 되었거나 *appid* 클레임이 누락 된 경우와 같이 serviceerror의 세부 정보에 자세한 정보가 포함 됩니다. |
| 401  | 권한 없음 | PartnerAadTicketRequired   | Azure AD 액세스 토큰이 인증 헤더의 서비스에 전달 되지 않았습니다.                                                                                                   |
| 401  | 권한 없음 | InconsistentClientId       | 요청 본문의 Microsoft Store ID 키와 인증 헤더의 Azure AD 액세스 토큰에 있는 *appid* 클레임의 *clientId* 클레임이 일치 하지 않습니다.                     |

<span/> 

## <a name="related-topics"></a>관련 항목

* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [제품에 대한 쿼리](query-for-products.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
* [Microsoft Store ID 키 갱신](renew-a-windows-store-id-key.md)