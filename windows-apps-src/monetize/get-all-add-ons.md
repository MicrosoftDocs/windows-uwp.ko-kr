---
author: Xansky
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 모든 앱에 대 한 모든 추가 기능 데이터를 검색 합니다.
title: 모든 추가 기능 가져오기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 4d58b29a959ed791665af52018062d0cf0a3a969
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6029722"
---
# <a name="get-all-add-ons"></a>모든 추가 기능 가져오기

Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 모든 앱에 대 한 모든 추가 기능에 대 한 데이터를 검색 합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

모든 요청 매개 변수는 이 메서드에 대한 옵션입니다. 매개 변수 없이 이 메서드를 호출하는 경우 응답에는 계정에 등록된 모든 앱의 모든 추가 기능에 대한 데이터가 포함됩니다.

|  매개 변수  |  유형  |  설명  |  필수  |
|------|------|------|------|
|  top  |  int  |  요청에 반환할 항목 수(즉, 반환할 추가 기능 수)입니다. 계정에 쿼리에서 지정한 값보다 더 많은 추가 기능이 있을 경우 응답 본문에는 데이터의 다음 페이지를 요청하기 위해 메서드 URI에 추가할 수 있는 상대 URI 경로가 포함됩니다.  |  아니요  |
|  skip  |  int  |  나머지 항목을 반환하기 전에 쿼리에서 바이패스할 항목 수입니다. 이 매개 변수를 사용하여 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10이고 skip=0이면 1-10 항목을 검색하고 top=10이고 skip=10이면 11-20 항목을 검색합니다.  |  아니요  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-examples"></a>요청 예제

다음 예제에서는 계정에 등록된 모든 앱의 모든 추가 기능 데이터를 검색하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제에서는 처음 10개의 추가 기능만 검색하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 개발자 계정에 등록된 총 1072개의 추가 기능에서 처음 5개의 추가 기능을 성공적으로 요청하여 반환된 JSON 응답 본문을 보여 줍니다. 편의를 위해 이 예제에서는 요청으로 반환된 처음 두 개의 추가 기능에 대한 데이터만 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMP",
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하기 위해 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI를 추가할 수 있는 상대 경로가 포함됩니다. 예를 들어 초기 요청 본문의 *top* 매개 변수는 10으로 설정되어 있지만 계정에 등록된 추가 기능이 100개인 경우 응답 본문에는 ```inappproducts?skip=10&top=10```의 @nextLink 값이 포함되며 이는 ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10```을 호출하여 다음 10개의 추가 기능을 호출할 수 있음을 나타냅니다. |
| value            | 배열  |  각 추가 기능에 대한 정보를 제공하는 개체가 들어 있는 배열입니다. 자세한 내용은 [추가 기능 리소스](manage-add-ons.md#add-on-object)를 참조하세요.   |
| totalCount   | int  | 응답 본문의 *value* 배열에서 앱 개체의 수입니다.     |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 추가 기능이 없습니다. |
| 409  | 앱 또는 추가 기능이 있는 [Microsoft Store 제출 API에서 지원 되지 않는 현재](create-and-manage-submissions-using-windows-store-services.md#not_supported)파트너 센터 기능을 사용 합니다.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [추가 기능 가져오기](get-an-add-on.md)
* [추가 기능 만들기](create-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)
