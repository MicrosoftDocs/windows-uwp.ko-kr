---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에 대 한 집계 오류 보고 데이터를 가져올 수 있습니다.
title: 앱에 대 한 오류 보고 데이터 가져오기
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, 저장소 서비스, Microsoft Store 분석 API, 오류
ms.localizationpriority: medium
ms.openlocfilehash: b1ecfad64598bb74ead0a912fa67b9e1c5c5d0c2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492798"
---
# <a name="get-error-reporting-data-for-your-app"></a>앱에 대 한 오류 보고 데이터 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에 대 한 JSON 형식으로 앱에 대 한 집계 오류 보고 데이터를 가져올 수 있습니다. 이 메서드는 지난 30 일 동안 발생 한 오류만 검색할 수 있습니다. 이 정보는 파트너 센터의 [상태 보고서](../publish/health-report.md) 의 **오류** 섹션 에서도 사용할 수 있습니다.

[오류 정보 가져오기](get-details-for-an-error-in-your-app.md), [스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)및 [CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md) 메서드를 사용 하 여 추가 오류 정보를 검색할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소


이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | Type   |  설명      |  필요한 공간  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 오류 보고 데이터를 검색 하려는 앱의 저장소 ID입니다. 저장소 ID는 파트너 센터의 [앱 id 페이지](../publish/view-app-identity-details.md) 에서 사용할 수 있습니다. 예 매장 ID는 9WZDNCRFJ3Q8입니다. |  예  |
| startDate | date | 검색할 오류 보고 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다. *AggregationLevel* 가 **day**, **week**또는 **month**이면이 매개 변수는 형식의 날짜를 지정 해야 합니다 ```mm/dd/yyyy``` . *AggregationLevel* 가 **hour**인 경우이 매개 변수는 형식의 날짜를 지정 ```mm/dd/yyyy``` 하거나 형식의 날짜와 시간을 지정할 수 있습니다 ```yyyy-mm-dd hh:mm:ss``` .<p/><p/>**참고:** &nbsp; &nbsp; 이 메서드는 지난 30 일 동안 발생 한 오류만 검색할 수 있습니다.  |  아니요  |
| endDate | date | 검색할 오류 보고 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. *AggregationLevel* 가 **day**, **week**또는 **month**이면이 매개 변수는 형식의 날짜를 지정 해야 합니다 ```mm/dd/yyyy``` . *AggregationLevel* 가 **hour**인 경우이 매개 변수는 형식의 날짜를 지정 ```mm/dd/yyyy``` 하거나 형식의 날짜와 시간을 지정할 수 있습니다 ```yyyy-mm-dd hh:mm:ss``` . |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  아니요  |
| filter |문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다. 응답 본문에서 다음 필드를 지정할 수 있습니다.<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**화살표**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**시장**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. 는 **시간**, **일**, **주**또는 **월**문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 **day**입니다. **주** 또는 **월**을 지정 하는 경우 *failurename* 및 *failureHash* 값은 1000 버킷으로 제한 됩니다.<p/><p/>**참고:** &nbsp; &nbsp; **Hour**를 지정 하는 경우에는 이전 72 시간에서 오류 데이터만 검색할 수 있습니다. 72 시간 보다 오래 된 오류 데이터를 검색 하려면 **일** 또는 다른 집계 수준 중 하나를 지정 합니다.  | 아니요 |
| orderby | 문자열 | 결과 데이터 값을 정렬 하는 문입니다. 구문은 *orderby = field [order], field [order],...* 입니다. *Field* 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**화살표**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**시장**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>*Order* 매개 변수는 선택 사항이 며 **asc** 또는 **desc** 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 *orderby* 문자열의 예입니다. *orderby = date, market*</p> |  아니요  |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li>**failureName**</li><li>**failureHash**</li><li>**화살표**</li><li>**osVersion**</li><li>**eventType**</li><li>**시장**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>반환 된 데이터 행에는 *groupby* 매개 변수 및 다음에 지정 된 필드가 포함 됩니다.</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>*Groupby* 매개 변수는 *aggregationLevel* 매개 변수와 함께 사용할 수 있습니다. 예: * &amp; Groupby = failureName, market &amp; aggregationLevel = week*</p></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예에서는 오류 보고 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식    | Description     |
|------------|---------|--------------|
| 값      | array   | 집계 오류 보고 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [오류 값](#error-values) 섹션을 참조 하십시오.     |
| @nextLink  | 문자열  | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **맨 위** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대해 1만 개 이상의 행이 있는 경우이 값이 반환 됩니다. |
| TotalCount | integer | 쿼리의 데이터 결과에 있는 총 행 수입니다.     |


### <a name="error-values"></a>오류 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값           | 형식    | 설명        |
|-----------------|---------|---------------------|
| date            | 문자열  | 오류 데이터에 대 한 날짜 범위의 첫 번째 날짜 (형식) ```yyyy-mm-dd``` 입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 더 긴 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. *AggregationLevel* 값을 **hour**로 지정 하는 요청의 경우이 값에는 형식의 시간 값도 포함 됩니다 ```hh:mm:ss``` .  |
| applicationId   | 문자열  | 오류 데이터를 검색 하려는 앱의 저장소 ID입니다.   |
| applicationName | 문자열  | 응용 프로그램의 표시 이름입니다.   |
| failureName     | 문자열  | 오류 이름으로, 하나 이상의 문제 클래스, 예외/버그 검사 코드, 오류가 발생 한 이미지 이름 및 연결 된 함수 이름을 네 부분으로 구성 되어 있습니다.  |
| failureHash     | 문자열  | 오류에 대 한 고유 식별자입니다.   |
| 기호          | 문자열  | 이 오류에 할당 된 기호입니다. |
| osVersion       | 문자열  | 오류가 발생 한 OS 버전을 지정 하는 다음 문자열 중 하나입니다.<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**알 수 없음**</li></ul>  |
| osRelease       | 문자열  |  오류가 발생 한 os 릴리스 또는 플 라이팅 링 (os 버전 내의 subpopulation)을 지정 하는 다음 문자열 중 하나입니다.<p/><p>Windows 10의 경우:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>빠른 참가자</strong></li><li><strong>참가자 느림</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>출시</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1의 경우:</p><ul><li><strong>Update 1</strong></li></ul><p>Windows 7의 경우:</p><ul><li><strong>서비스 팩 1</strong></li></ul><p>OS 릴리스 또는 플 라이팅 링을 알 수 없는 경우이 필드에 <strong>알 수 없는</strong>값이 있습니다.</p>    |
| eventType       | 문자열  | Key, Input, Predict, PredictOnly, None<ul><li>**크래시**</li><li>**끊어**</li><li>**ram**</li><li>**.jse**</li></ul>      |
| market          | 문자열  | 장치 시장의 ISO 3166 국가 코드입니다.   |
| deviceType      | 문자열  | 오류가 발생 한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li>**컴퓨터**</li><li>**내선**</li><li>**콘솔-Xbox One**</li><li>**콘솔-Xbox 시리즈 X**</li><li>**IoT**</li><li>**Holographic**</li><li>**알 수 없음**</li></ul>    |
| packageName     | 문자열  | 이 오류와 연결된 앱 패키지의 고유 이름입니다.      |
| packageVersion  | 문자열  | 이 오류와 연결 된 응용 프로그램 패키지의 버전입니다.   |
| deviceCount     | number | 지정된 집계 수준 중에 이 오류에 해당하는 고유 디바이스의 수입니다.  |
| eventCount      | number | 지정된 집계 수준 중에 이 오류를 발생시킨 이벤트의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md)
* [앱에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)
* [앱의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 인수 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [앱 평점 가져오기](get-app-ratings.md)
* [앱 리뷰 가져오기](get-app-reviews.md)
