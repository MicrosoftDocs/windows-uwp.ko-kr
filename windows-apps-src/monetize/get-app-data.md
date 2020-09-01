---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Microsoft Store 제출 API에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 데이터를 검색 합니다.
title: 앱 데이터 가져오기
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 데이터
ms.localizationpriority: medium
ms.openlocfilehash: 7dfbad9d0aa2bfb69479f168ec262fe67bedb49c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162417"
---
# <a name="get-app-data"></a>앱 데이터 가져오기

Microsoft Store 제출 API에서 다음 메서드를 사용 하 여 파트너 센터 계정에서 기존 앱에 대 한 데이터를 가져옵니다. API를 사용 하기 위한 필수 구성 요소를 포함 하 여 Microsoft Store 제출 API에 대 한 소개는 [Microsoft Store services를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조 하세요.

이러한 방법을 사용 하려면 앱이 파트너 센터 계정에 이미 있어야 합니다. 앱에 대 한 제출을 만들거나 관리 하려면 [앱 제출 관리](manage-app-submissions.md)의 메서드를 참조 하세요.

| 메서드 | URI                                                                                             | 설명                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [모든 앱에 대 한 데이터 가져오기](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [특정 앱에 대 한 데이터 가져오기](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [앱에 대한 추가 기능 가져오기](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>필수 조건

아직 수행 하지 않은 경우 이러한 메서드를 사용 하기 전에 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.

## <a name="data-resources"></a>데이터 리소스

앱 데이터를 가져오는 Microsoft Store 제출 API 메서드는 다음 JSON 데이터 리소스를 사용 합니다.

<span id="application_object" />

### <a name="application-resource"></a>애플리케이션 리소스

이 리소스는 계정에 등록 된 앱을 나타냅니다.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": true
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description       |
|-----------------|---------|---------------------|
| id            | 문자열  | 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.   |
| primaryName   | 문자열  | 응용 프로그램의 기본 이름입니다.      |
| packageFamilyName | 문자열  | 앱의 패키지 패밀리 이름입니다.      |
| packageIdentityName          | 문자열  | 앱의 패키지 id 이름입니다.                       |
| publisherName       | 문자열  | 앱과 연결 된 Windows 게시자 ID입니다. 이 값은 파트너 센터의 앱에 대 한 [앱 id](../publish/view-app-identity-details.md) 페이지에 표시 되는 **패키지/id/게시자** 값에 해당 합니다.       |
| firstPublishedDate      | 문자열  | 앱이 처음 게시 된 날짜 (ISO 8601 형식)입니다.   |
| lastPublishedApplicationSubmission       | 개체 | 앱에 대해 마지막으로 게시 된 전송에 대 한 정보를 제공 하는 [제출 리소스](#submission_object) 입니다.    |
| pendingApplicationSubmission        | 개체  |  앱에 대해 현재 보류 중인 전송에 대 한 정보를 제공 하는 [제출 리소스](#submission_object) 입니다.   |   
| hasAdvancedListingPermission        | boolean  |  앱에 대 한 전송에 대해 [gamingOptions](manage-app-submissions.md#gaming-options-object) 또는 [트레일러](manage-app-submissions.md#trailer-object) 를 구성할 수 있는지 여부를 나타냅니다. 이 값은 2017 년 5 월 이후에 생성 된 전송에 대해 true입니다. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>추가 기능 리소스

이 리소스는 추가 기능에 대 한 정보를 제공 합니다.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명         |
|-----------------|---------|----------------------|
| inAppProductId            | 문자열  | 추가 기능에 대 한 저장소 ID입니다. 이 값은 저장소에서 제공 됩니다. 예 매장 ID는 9NBLGGH4TNMP입니다.   |


<span id="flight-object" />

### <a name="flight-resource"></a>비행 리소스

이 리소스는 앱에 대 한 패키지 비행 정보를 제공 합니다.

```json
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
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명           |
|-----------------|---------|------------------------|
| flightId            | 문자열  | 비행 패키지의 ID입니다. 이 값은 파트너 센터에서 제공 합니다.  |
| friendlyName           | 문자열  | 개발자가 지정한 패키지의 이름입니다.   |
| lastPublishedFlightSubmission       | 개체 | 비행 패키지에 대해 마지막으로 게시 된 전송에 대 한 정보를 제공 하는 [제출 리소스](#submission_object) 입니다.   |
| pendingFlightSubmission        | 개체  |  진행 중인 패키지에 대 한 현재 보류 중인 전송에 대 한 정보를 제공 하는 [제출 리소스](#submission_object) 입니다.  |    
| groupIds           | array  | 항공편 패키지와 연결 된 비행 그룹의 Id를 포함 하는 문자열의 배열입니다. 비행 그룹에 대 한 자세한 내용은 [항공편 패키지](../publish/package-flights.md)를 참조 하세요.   |
| rankHigherThan           | 문자열  | 현재 패키지 비행 보다 즉시 순위가 매겨진 패키지 비행의 이름입니다. 비행 그룹의 순위를 지정 하는 방법에 대 한 자세한 내용은 [항공편 패키지](../publish/package-flights.md)를 참조 하세요.  |


<span id="submission_object" />

### <a name="submission-resource"></a>제출 리소스

이 리소스는 제출에 대 한 정보를 제공 합니다. 다음 예제에서는이 리소스의 형식을 보여 줍니다.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

이 리소스의 값은 다음과 같습니다.

| 값              | 형식   | Description               |
|--------------------|--------|---------------------------|
| id                 | 문자열 | 제출 ID입니다. |
| resourceLocation   | 문자열 | ```https://manage.devcenter.microsoft.com/v1.0/my/```전송에 대 한 전체 데이터를 검색 하기 위해 기본 요청 URI에 추가할 수 있는 상대 경로입니다. |

 
## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 제출 API를 사용 하 여 앱 서브 미션 관리](manage-app-submissions.md)
* [모든 앱 가져오기](get-all-apps.md)
* [앱 가져오기](get-an-app.md)
* [앱에 대한 추가 기능 가져오기](get-add-ons-for-an-app.md)
* [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md)