---
author: Xansky
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱의 패키지 플라이트 정보를 검색합니다.
title: 앱의 패키지 플라이트 가져오기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트, 패키지 플라이트
ms.localizationpriority: medium
ms.openlocfilehash: 847e837c67990b5cbdb3a7a28c0c8115e96c8644
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821558"
---
# <a name="get-package-flights-for-an-app"></a>앱의 패키지 플라이트 가져오기

Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱의 패키지 플라이트를 나열합니다. 패키지 플라이트에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

|  이름  |  유형  |  설명  |  필수  |
|------|------|------|------|
|  applicationId  |  문자열  |  패키지 플라이트를 검색하려는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |  예  |
|  top  |  int  |  요청에 반환할 항목 수(즉, 반환할 패키지 플라이트 수)입니다. 계정에 쿼리에서 지정한 값보다 더 많은 패키지 플라이트가 있을 경우 응답 본문에는 데이터의 다음 페이지를 요청하기 위해 메서드 URI에 추가할 수 있는 상대 URI 경로가 포함됩니다.  |  아니요  |
|  skip  |  int  |  나머지 항목을 반환하기 전에 쿼리에서 바이패스할 항목 수입니다. 이 매개 변수를 사용하여 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10이고 skip=0이면 1-10 항목을 검색하고 top=10이고 skip=10이면 11-20 항목을 검색합니다.  |  아니요  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-examples"></a>요청 예제

다음 예제에서는 앱의 모든 패키지 플라이트를 나열하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제에서는 앱의 처음 패키지 플라이트를 나열하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 총 3개의 패키지 플라이트가 있는 앱의 처음 패키지 플라이트를 성공적으로 요청하여 반환된 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명       |
|------------|--------|---------------------|
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하기 위해 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI를 추가할 수 있는 상대 경로가 포함됩니다. 예를 들어 초기 요청 본문의 *top* 매개 변수는 2로 설정되어 있지만 앱의 패키지 플라이트가 4개인 경우 응답 본문에는 ```applications/{applicationid}/listflights/?skip=2&top=2```의 @nextLink 값이 포함되며 이는 ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2```를 호출하여 다음 2개의 패키지 플라이트를 호출할 수 있음을 나타냅니다. |
| value      | 배열  | 지정된 앱의 패키지 플라이트에 대한 정보를 제공하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 [플라이트 리소스](get-app-data.md#flight-object)를 참조하세요.               |
| totalCount | int    | 쿼리에 대한 데이터 결과의 총 행 수(즉, 지정한 앱의 총 패키지 플라이트 수)입니다.   |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 패키지 플라이트가 없습니다. |
| 409  | 앱이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [모든 앱 가져오기](get-all-apps.md)
* [앱 가져오기](get-an-app.md)
* [앱에 대한 추가 기능 가져오기](get-add-ons-for-an-app.md)
