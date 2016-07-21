---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "이 메서드를 사용하여 Windows 스토어 키를 갱신합니다."
title: "Windows 스토어 ID 키 갱신"
translationtype: Human Translation
ms.sourcegitcommit: f7e67a4ff6cb900fb90c5d5643e2ddc46cbe4dd2
ms.openlocfilehash: a3cef13e84c5bb06be4f3e3d4b2db4e02650df62

---

# Windows 스토어 ID 키 갱신


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 메서드를 사용하여 Windows 스토어 키를 갱신합니다. [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 또는 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 메서드를 호출하여 생성한 Windows 스토어 ID 키의 유효 기간은 90일입니다. 키가 만료되면 만료된 키를 사용하여 이 메서드를 통해 새 키를 다시 협상할 수 있습니다.

## 필수 조건


이 메서드를 사용하려면 다음이 필요합니다.

-   `https://onestore.microsoft.com` 대상 URI를 사용하여 만든 Azure AD 액세스 토큰
-   앱의 클라이언트 쪽 코드에서 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 또는 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 메서드를 호출하여 생성했던 만료된 Windows 스토어 ID 키입니다.

자세한 내용은 [서비스에서 제품 보기 및 권한 부여](view-and-grant-products-from-a-service.md)를 참조하세요.

## 요청


### 요청 구문

| 키 유형    | 메서드 | 요청 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 컬렉션 | 게시   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| 구입    | 게시   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |

<span/>

### 요청 헤더

| 헤더         | 유형   | 설명                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 호스트           | 문자열 | **collections.mp.microsoft.com** 또는 **purchase.mp.microsoft.com** 값으로 설정해야 합니다.           |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | 문자열 | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |

<span/>

### 요청 본문

| 매개 변수     | 유형   | 설명                       | 필수 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 문자열 | Azure AD 액세스 토큰입니다.        | 예      |
| 키           | 문자열 | 만료된 Windows 스토어 ID 키입니다. | 아니요       |

<span/> 

### 요청 예제

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

## 응답


### 응답 본문

| 매개 변수 | 유형   | 설명                                                                                                            | 필수 |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| 키       | 문자열 | Windows 스토어 컬렉션 API 또는 구매 API에 나중에 호출할 때 사용할 수 있는 새로 고친 Windows 서비스 키입니다. | 아니요       |

<span/>

### 응답 예제

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

## 오류 코드


| 코드 | 오류        | 내부 오류 코드           | 설명                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | 권한 없음 | AuthenticationTokenInvalid | Azure AD 액세스 토큰이 잘못되었습니다. ServiceError의 세부 정보에 토큰이 만료되거나 *appid* 클레임이 누락되는 경우와 같은 자세한 정보가 포함되는 경우도 있습니다. |
| 401  | 권한 없음 | InconsistentClientId       | Windows 스토어 ID 키의 *clientId* 클레임과 Azure AD 액세스 토큰의 *appid* 클레임이 일치하지 않습니다.                                                                     |

<span/>

## 관련 항목


* [서비스에서 제품 보기 및 권한 부여](view-and-grant-products-from-a-service.md)
* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [무료 제품에 대한 권한 부여](grant-free-products.md)



<!--HONumber=Jul16_HO1-->


