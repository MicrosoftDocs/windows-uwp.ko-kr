---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Microsoft Store 제출 API에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 데이터를 검색 합니다.
title: 앱 데이터 가져오기
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 데이터
ms.localizationpriority: medium
ms.openlocfilehash: cfbe8df46f51b41ccdd840f609caf2c593735e1f
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210979"
---
# <a name="get-app-data"></a>앱 데이터 가져오기

Microsoft Store 제출 API에서 다음 메서드를 사용 하 여 파트너 센터 계정에서 기존 앱에 대 한 데이터를 가져옵니다. API 사용을 위한 필수 조건을 비롯하여 Microsoft Store 제출 API에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

이러한 방법을 사용 하려면 앱이 파트너 센터 계정에 이미 있어야 합니다. 앱에 대한 제출을 만들거나 관리하려면 [앱 제출 관리](manage-app-submissions.md)의 메서드를 참조하세요.

| 메서드 | URI                                                                                             | 설명                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| 가져오기    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [모든 앱에 대 한 데이터 가져오기](get-all-apps.md)               |
| 가져오기    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [특정 앱에 대 한 데이터 가져오기](get-an-app.md)                |
| 가져오기    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [앱에 대 한 추가 기능 가져오기](get-add-ons-for-an-app.md)         |
| 가져오기    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [앱에 대 한 패키지 항공편 가져오기](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>필수 조건

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Microsoft Store 제출 API에 대한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## <a name="data-resources"></a>데이터 리소스

앱 데이터를 가져오는 Microsoft Store 제출 API 메서드는 다음과 같은 JSON 데이터 리소스를 사용합니다.

<span id="application_object" />

### <a name="application-resource"></a>응용 프로그램 리소스

이 리소스는 계정에 등록된 앱을 나타냅니다.

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

| 값           | 형식    | 설명       |
|-----------------|---------|---------------------|
| id            | string  | 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.   |
| primaryName   | string  | 앱의 기본 이름입니다.      |
| packageFamilyName | string  | 앱의 패키지 패밀리 이름입니다.      |
| packageIdentityName          | string  | 앱의 패키지 ID 이름입니다.                       |
| publisherName       | string  | 앱과 연결된 Windows 게시자 ID입니다. 이 값은 파트너 센터의 앱에 대 한 [앱 id](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details) 페이지에 표시 되는 **패키지/id/게시자** 값에 해당 합니다.       |
| firstPublishedDate      | string  | ISO 8601 형식으로 앱이 처음 게시된 날짜입니다.   |
| lastPublishedApplicationSubmission       | object | 앱의 마지막 게시된 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.    |
| pendingApplicationSubmission        | object  |  앱의 현재 보류 중인 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.   |   
| hasAdvancedListingPermission        | boolean  |  앱 제출에서 [gamingOptions](manage-app-submissions.md#gaming-options-object)이나 [예고편](manage-app-submissions.md#trailer-object)을 구성할 수 있는지 알려줍니다 2017년 5월 이후에 만든 제출의 경우 이 값은 true입니다. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>추가 기능 리소스

이 리소스는 추가 기능에 대한 정보를 제공합니다.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명         |
|-----------------|---------|----------------------|
| inAppProductId            | string  | 추가 기능의 스토어 ID입니다. 이 값은 스토어에서 제공됩니다. 예를 들어 스토어 ID는 9NBLGGH4TNMP입니다.   |


<span id="flight-object" />

### <a name="flight-resource"></a>플라이트 리소스

이 리소스는 앱의 패키지 플라이트에 대한 정보를 제공합니다.

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
| flightId            | string  | 패키지 플라이트의 ID입니다. 이 값은 파트너 센터에서 제공 합니다.  |
| FriendlyName           | string  | 개발자가 지정한 패키지 플라이트 이름입니다.   |
| lastPublishedFlightSubmission       | object | 패키지 플라이트의 마지막 게시된 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.   |
| pendingFlightSubmission        | object  |  패키지 플라이트의 현재 보류 중인 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.  |    
| groupIds           | array  | 패키지 플라이트와 연결된 플라이트 그룹의 ID가 포함된 문자열의 배열입니다. 플라이트 그룹에 대한 자세한 내용은 [패키지 플라이트](https://docs.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.   |
| rankHigherThan           | string  | 현재 패키지 플라이트보다 순위가 바로 아래인 패키지 플라이트의 식별 이름입니다. 플라이트 그룹의 순위 지정에 대한 자세한 내용은 [패키지 플라이트](https://docs.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.  |


<span id="submission_object" />

### <a name="submission-resource"></a>제출 리소스

이 리소스는 제출에 대한 정보를 제공합니다. 다음 예제에서는 이 리소스의 형식을 보여 줍니다.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

이 리소스의 값은 다음과 같습니다.

| 값              | 형식   | 설명               |
|--------------------|--------|---------------------------|
| id                 | string | 제출의 ID입니다. |
| resourceLocation   | string | 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI에 추가하여 제출에 대한 전체 데이터를 검색할 수 있는 상대 경로입니다. |

 
## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 제출 API를 사용 하 여 앱 서브 미션 관리](manage-app-submissions.md)
* [모든 앱 가져오기](get-all-apps.md)
* [앱 가져오기](get-an-app.md)
* [앱에 대 한 추가 기능 가져오기](get-add-ons-for-an-app.md)
* [앱에 대 한 패키지 항공편 가져오기](get-flights-for-an-app.md)
