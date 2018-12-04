---
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: Microsoft Store 대상 제품 API에서 이 메서드를 사용하여 현재 앱의 컨텍스트에서 현재 사용자에게 제공되는 대상 제품을 가져옵니다.
title: 대상 제품 가져오기
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 대상 제품 API, 대상 제품 가져오기
ms.localizationpriority: medium
ms.openlocfilehash: 71cd6ce3b9736b812f8ccdf4d21d35357928c63c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8482218"
---
# <a name="get-targeted-offers"></a>대상 제품 가져오기

이 메서드를 사용하여 사용자가 대상 제품의 고객 세그먼트에 포함되는지 여부에 따라 현재 사용자에게 제공되는 대상 제품을 가져옵니다. 자세한 내용은 [스토어 서비스를 사용하여 대상 제품 관리](manage-targeted-offers-using-windows-store-services.md)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 먼저 현재 앱에 로그인한 사용자의 [Microsoft 계정 토큰을 획득](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token)해야 합니다. 이 메서드의 ```Authorization``` 요청 헤더에 이 토큰을 전달해야 합니다. 이 토큰은 Microsoft Store에서 현재 사용자의 대상 제품을 가져오는 데 사용됩니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명  |
|---------------|--------|--------------|
| 권한 부여 | 문자열 | 필수. 현재 앱에 로그인한 사용자의 Microsoft 계정 토큰이며 **Bearer**&lt;*token*&gt; 형식입니다. |


### <a name="request-parameters"></a>요청 매개 변수

이 메서드에는 URI 또는 쿼리 매개 변수가 없습니다.

### <a name="request-example"></a>요청 예제

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>응답

이 메서드는 다음 필드가 포함된 개체 배열이 들어 있는 JSON 형식의 응답 본문을 반환합니다. 배열의 각 개체는 특정 고객 세그먼트의 일부로 지정된 사용자에게 제공되는 대상 제품을 나타냅니다.

| 필드      | 유형   | 설명         |
|------------|--------|------------------|
| offers      | 배열  | 현재 사용자에게 제공되는 대상 제품과 연결된 추가 기능에 대한 제품 ID의 배열입니다. 이러한 제품 Id는 파트너 센터에서 앱에 대 한 **대상 지정을 제공** 하는 페이지에 지정 됩니다.            |
| trackingId  | 문자열 | 코드나 서비스에서 대상 제품을 추적하기 위해 선택적으로 사용할 수 있는 GUID입니다. |


### <a name="example"></a>예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 대상 제품 관리](manage-targeted-offers-using-windows-store-services.md)

 

 
