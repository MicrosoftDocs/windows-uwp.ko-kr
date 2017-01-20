---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "Windows 스토어 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱에 대한 데이터를 검색합니다."
title: "Windows 스토어 제출 API를 사용하여 앱 데이터 가져오기"
translationtype: Human Translation
ms.sourcegitcommit: 020c8b3f4d9785842bbe127dd391d92af0962117
ms.openlocfilehash: 23839faca120976a07e666b9d6861aa8750898ad

---

# <a name="get-app-data-using-the-windows-store-submission-api"></a>Windows 스토어 제출 API를 사용하여 앱 데이터 가져오기

Windows 스토어 제출 API에서 다음 메서드를 사용하여 앱에 대한 데이터를 가져옵니다. Windows 스토어 제출 API에 대한 자세한 내용은 [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

>**참고**&nbsp;&nbsp;이러한 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한을 가진 Windows 개발자 센터 계정에 대해서만 사용할 수 있습니다. 일부 계정은 이 권한을 사용할 수 없습니다. 이러한 메서드는 앱에 대한 데이터를 가져오는 데만 사용할 수 있습니다. 앱에 대한 제출을 만들거나 관리하려면 [앱 제출 관리](manage-app-submissions.md)의 메서드를 참조하세요.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">메서드</th>
<th align="left">URI</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications```</td>
<td align="left">[모든 앱에 대한 데이터 가져오기](get-all-apps.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}```</td>
<td align="left">[특정 앱에 대한 데이터 가져오기](get-an-app.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts```</td>
<td align="left">[앱에 대한 추가 기능 가져오기](get-add-ons-for-an-app.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights```</td>
<td align="left">[앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md)</td>
</tr>
</tbody>
</table>

<span/>

## <a name="prerequisites"></a>필수 조건

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Windows 스토어 제출 API를 위한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## <a name="data-resources"></a>데이터 리소스

앱 데이터를 가져오는 Windows 스토어 제출 API 메서드는 다음과 같은 JSON 데이터 리소스를 사용합니다.

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
  }
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명       |
|-----------------|---------|---------------------|
| id            | 문자열  | 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.   |
| primaryName   | 문자열  | 앱의 기본 이름입니다.      |
| packageFamilyName | 문자열  | 앱의 패키지 패밀리 이름입니다.      |
| packageIdentityName          | 문자열  | 앱의 패키지 ID 이름입니다.                       |
| publisherName       | 문자열  | 앱과 연결된 Windows 게시자 ID입니다. 이 ID는 Windows 개발자 센터 대시보드에서 앱의 [앱 ID](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) 페이지에 나타나는 **패키지/ID/게시자** 값에 해당합니다.       |
| firstPublishedDate      | 문자열  | ISO 8601 형식으로 앱이 처음 게시된 날짜입니다.   |
| lastPublishedApplicationSubmission       | 개체 | 앱의 마지막 게시된 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.    |
| pendingApplicationSubmission        | 개체  |  앱의 현재 보류 중인 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.   |   |


<span id="add-on-object" />
### <a name="add-on-resouce"></a>추가 기능 리소스

이 리소스는 추가 기능에 대한 정보를 제공합니다.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명         |
|-----------------|---------|----------------------|
| inAppProductId            | 문자열  | 추가 기능의 스토어 ID입니다. 이 값은 스토어에서 제공됩니다. 예를 들어 스토어 ID는 9NBLGGH4TNMP입니다.   |


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

| 값           | 유형    | 설명           |
|-----------------|---------|------------------------|
| flightId            | 문자열  | 패키지 플라이트의 ID입니다. 이 값은 개발자 센터에서 제공됩니다.  |
| FriendlyName           | 문자열  | 개발자가 지정한 패키지 플라이트 이름입니다.   |
| lastPublishedFlightSubmission       | 개체 | 패키지 플라이트의 마지막 게시된 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.   |
| pendingFlightSubmission        | 개체  |  패키지 플라이트의 현재 보류 중인 제출에 대한 정보를 제공하는 [제출 리소스](#submission_object)입니다.  |    
| groupIds           | 배열  | 패키지 플라이트와 연결된 플라이트 그룹의 ID가 포함된 문자열의 배열입니다. 플라이트 그룹에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.   |
| rankHigherThan           | 문자열  | 현재 패키지 플라이트보다 순위가 바로 아래인 패키지 플라이트의 식별 이름입니다. 플라이트 그룹의 순위 지정에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.  |


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

| 값           | 유형    | 설명                 |
|-----------------|---------|------------------------------|
| id            | 문자열  | 제출의 ID입니다.    |
| resourceLocation   | 문자열  | 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI에 추가하여 제출에 대한 전체 데이터를 검색할 수 있는 상대 경로입니다.            |
 
<span/>

## <a name="related-topics"></a>관련 항목

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Windows 스토어 제출 API를 사용하여 앱 제출 관리](manage-app-submissions.md)
* [모든 앱 가져오기](get-all-apps.md)
* [앱 가져오기](get-an-app.md)
* [앱에 대한 추가 기능 가져오기](get-add-ons-for-an-app.md)
* [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md)



<!--HONumber=Dec16_HO3-->


