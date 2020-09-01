---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Microsoft Store 컬렉션의 갱신 방법 및 구매 Api를 사용 하 여 만료 된 Microsoft Store ID 키를 갱신 하는 방법에 대해 알아봅니다.
title: Microsoft Store ID 키 갱신
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store collection API, Microsoft Store 구매 API, Microsoft Store ID 키, 갱신
ms.localizationpriority: medium
ms.openlocfilehash: fe19b446f88e16b87ff40288f5d4480f9230469e
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238258"
---
# <a name="renew-a-microsoft-store-id-key"></a>Microsoft Store ID 키 갱신


이 메서드를 사용 하 여 Microsoft Store 키를 갱신 합니다. [MICROSOFT STORE ID 키를 생성](view-and-grant-products-from-a-service.md#step-4)하는 경우 키는 90 일 동안 유효 합니다. 키가 만료 된 후이 메서드를 사용 하 여 만료 된 키를 사용 하 여 새 키를 재협상 할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소


이 방법을 사용 하려면 다음이 필요 합니다.

* 대상 URI 값을 포함 하는 Azure AD 액세스 토큰입니다 `https://onestore.microsoft.com` .
* [앱의 클라이언트 쪽 코드에서 생성](view-and-grant-products-from-a-service.md#step-4)된 만료 된 Microsoft Store ID 키입니다.

자세한 내용은 [서비스에서 제품 자격 관리](view-and-grant-products-from-a-service.md)를 참조 하세요.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 키 유형    | 방법 | 요청 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 컬렉션 | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Purchase    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>요청 헤더

| header         | 유형   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 호스트           | 문자열 | **Collections.mp.microsoft.com** 또는 **purchase.mp.microsoft.com**값으로 설정 해야 합니다.           |
| Content-Length | number | 요청 본문의 길이입니다.                                                                       |
| 콘텐츠 형식   | 문자열 | 요청 및 응답 유형을 지정 합니다. 현재 유일 하 게 지원 되는 값은 **application/json**입니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수     | 형식   | Description                       | 필수 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 문자열 | Azure AD 액세스 토큰입니다.        | 예      |
| key           | 문자열 | 만료 된 Microsoft Store ID 키입니다. | 예       |


### <a name="request-example"></a>요청 예제

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 매개 변수 | 형식   | Description                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| key       | 문자열 | 나중에 Microsoft Store collection API 또는 구매 API에 대 한 호출에 사용할 수 있는 새로 고친 Microsoft Store 키입니다. |


### <a name="response-example"></a>응답 예제

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>오류 코드


| 코드 | 오류        | 내부 오류 코드           | Description   |
|------|--------------|----------------------------|---------------|
| 401  | 권한 없음 | AuthenticationTokenInvalid | Azure AD 액세스 토큰이 잘못 되었습니다. 일부 경우에는 토큰이 만료 되었거나 *appid* 클레임이 누락 된 경우와 같이 serviceerror의 세부 정보에 자세한 정보가 포함 됩니다. |
| 401  | 권한 없음 | InconsistentClientId       | Azure AD 액세스 토큰에서 Microsoft Store ID 키와 *appid* 클레임의 *clientId* 클레임이 일치 하지 않습니다.                                                                     |


## <a name="related-topics"></a>관련 항목


* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
