---
author: Xansky
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: 이 메서드를 사용하여 Microsoft Store 키를 갱신합니다.
title: Microsoft Store ID 키 갱신
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 컬렉션 API, Microsoft Store 구매 API, Microsoft Store ID 키, 갱신
ms.localizationpriority: medium
ms.openlocfilehash: 70bda5022e52c0b18a43563a0492bd56d09b88a0
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5468474"
---
# <a name="renew-a-microsoft-store-id-key"></a>Microsoft Store ID 키 갱신


이 메서드를 사용하여 Microsoft Store 키를 갱신합니다. [Microsoft Store ID 키를 생성](view-and-grant-products-from-a-service.md#step-4)하면 키의 유효 기간은 90일입니다. 키가 만료되면 만료된 키를 사용하여 이 메서드를 통해 새 키를 다시 협상할 수 있습니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음이 필요합니다.

* 대상 URI 값이 `https://onestore.microsoft.com`인 Azure AD 액세스 토큰.
* [앱의 클라이언트 쪽 코드에서 생성된](view-and-grant-products-from-a-service.md#step-4) 만료된 Microsoft Store ID 키입니다.

자세한 내용은 참조 [서비스에서 제품 권리 유형 관리](view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 키 유형    | 메서드 | 요청 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 컬렉션 | 게시   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| 구입    | 게시   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>요청 헤더

| 헤더         | 유형   | 설명                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 호스트           | 문자열 | **collections.mp.microsoft.com** 또는 **purchase.mp.microsoft.com** 값으로 설정해야 합니다.           |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | 문자열 | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수     | 유형   | 설명                       | 필수 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 문자열 | Azure AD 액세스 토큰입니다.        | 예      |
| 키           | 문자열 | 만료된 Microsoft Store ID 키입니다. | 예       |


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

| 매개 변수 | 유형   | 설명                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| 키       | 문자열 | Microsoft Store 컬렉션 API 또는 구매 API에 나중에 호출할 때 사용할 수 있는 새로 고친 Microsoft Store 키입니다. |


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


| 코드 | 오류        | 내부 오류 코드           | 설명   |
|------|--------------|----------------------------|---------------|
| 401  | 권한 없음 | AuthenticationTokenInvalid | Azure AD 액세스 토큰이 잘못되었습니다. ServiceError의 세부 정보에 토큰이 만료되거나 *appid* 클레임이 누락되는 경우와 같은 자세한 정보가 포함되는 경우도 있습니다. |
| 401  | 권한 없음 | InconsistentClientId       | Microsoft Store ID 키의 *clientId* 클레임과 Azure AD 액세스 토큰의 *appid* 클레임이 일치하지 않습니다.                                                                     |


## <a name="related-topics"></a>관련 항목


* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)
