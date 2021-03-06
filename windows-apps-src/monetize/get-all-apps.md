---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: 파트너 센터 계정에 등록 된 모든 앱에 대 한 정보를 검색할 Microsoft Store 제출 API에서에서이 메서드를 사용 합니다.
title: 모든 앱 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱
ms.localizationpriority: medium
ms.openlocfilehash: 267e1d4de3917ae332cdfe15309f3871ef7b6647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641988"
---
# <a name="get-all-apps"></a>모든 앱 가져오기


파트너 센터 계정에 등록 된 앱에 대 한 데이터를 검색할 Microsoft Store 제출 API에서에서이 메서드를 사용 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

모든 요청 매개 변수는 이 메서드에 대한 옵션입니다. 매개 변수 없이이 메서드를 호출 하면 응답 처음 10 개에 대 한 데이터를 포함 하는 계정에 등록 된 앱입니다.

|  매개 변수  |  형식  |  설명  |  필수  |
|------|------|------|------|
|  top  |  int  |  요청에 반환할 항목 수(즉, 반환할 앱 수)입니다. 계정에 쿼리에서 지정한 값보다 더 많은 앱이 있을 경우 응답 본문에는 데이터의 다음 페이지를 요청하기 위해 메서드 URI에 추가할 수 있는 상대 URI 경로가 포함됩니다.  |  아니오  |
|  skip  |  int  |  나머지 항목을 반환하기 전에 쿼리에서 바이패스할 항목 수입니다. 이 매개 변수를 사용하여 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10이고 skip=0이면 1-10 항목을 검색하고 top=10이고 skip=10이면 11-20 항목을 검색합니다.  |  아니오  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-examples"></a>요청 예제

다음 예제에서는 앱 계정에 등록된 처음 10개의 앱을 검색하는 방법을 보여 줍니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제에서는 계정에 등록된 모든 앱에 대한 정보를 검색하는 방법을 보여 줍니다. 먼저 상위 10 개의 앱을 가져옵니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

다음을 재귀적으로 호출할 `GET https://manage.devcenter.microsoft.com/v1.0/my/{@nextLink}` 될 때까지 `{@nextlink}` null 이거나 응답에 존재 하지 않습니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```
  
```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=20&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=30&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

계정에 있는 앱의 총 수를 이미 알고 있는 경우 해당 숫자에 간단히 전달할 수 있습니다 합니다 **위쪽** 매개 변수로 모든 앱에 대 한 정보를 가져올 수 있습니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=23 HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>응답

다음 예제에서는 개발자 계정에 등록된 총 21개의 앱에서 처음 10개의 앱을 성공적으로 요청하여 반환된 JSON 응답 본문을 보여 줍니다. 편의를 위해 이 예제에서는 요청으로 반환된 처음 두 개의 앱에 대한 데이터만 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | 배열  | 계정에 등록된 각 앱에 대한 정보가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 [응용 프로그램 리소스](get-app-data.md#application_object)를 참조하세요.                                                                                                                           |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하기 위해 기본 `https://manage.devcenter.microsoft.com/v1.0/my/` 요청 URI를 추가할 수 있는 상대 경로가 포함됩니다. 예를 들어 초기 요청 본문의 *top* 매개 변수는 10으로 설정되어 있지만 계정에 등록된 앱이 20개인 경우 응답 본문에는 `applications?skip=10&top=10`의 @nextLink 값이 포함되며 이는 `https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10`을 호출하여 다음 10개의 앱을 호출할 수 있음을 나타냅니다. |
| totalCount | int    | 쿼리에 대한 데이터 결과의 총 행 수(즉, 계정에 등록된 총 앱 수)입니다.                                                |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 앱이 없습니다. |
| 409  | 파트너 센터 기능을 사용 하는 앱 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [앱 가져오기](get-an-app.md)
* [앱에 대 한 패키지 항공편을 가져오기](get-flights-for-an-app.md)
* [앱에 대 한 추가 기능 얻기](get-add-ons-for-an-app.md)
