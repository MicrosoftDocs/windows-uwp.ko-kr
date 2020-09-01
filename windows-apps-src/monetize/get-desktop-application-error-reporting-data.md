---
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에 대 한 데스크톱 응용 프로그램에 대 한 집계 오류 보고 데이터를 가져올 수 있습니다.
title: 데스크톱 애플리케이션에 대한 오류 보고 데이터 가져오기
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, 저장소 서비스, Microsoft Store 분석 API, 오류, 데스크톱 응용 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: bb9c0bd3162f603bd9965a98c5e75bad5aae9c54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167597"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>데스크톱 애플리케이션에 대한 오류 보고 데이터 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 [Windows 데스크톱 응용](/windows/desktop/appxpkg/windows-desktop-application-program)프로그램에 추가한 데스크톱 응용 프로그램에 대 한 집계 오류 보고 데이터를 가져올 수 있습니다. 이 메서드는 지난 30 일 동안 발생 한 오류만 검색할 수 있습니다. 이 정보는 파트너 센터의 데스크톱 응용 프로그램에 대 한 [상태 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 에서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  Description      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 오류 보고 데이터를 검색 하려는 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 얻으려면 파트너 센터 (예: **상태 보고서**) [에서 데스크톱 응용 프로그램에 대 한 분석 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 를 열고 URL에서 제품 id를 검색 합니다. |  예  |
| startDate | date | 검색할 오류 보고 데이터의 날짜 범위의 시작 날짜 (형식) ```mm/dd/yyyy``` 입니다. 기본값은 현재 날짜입니다.<p/><p/>**참고:** &nbsp; &nbsp; 이 메서드는 지난 30 일 동안 발생 한 오류만 검색할 수 있습니다.  |  예  |
| endDate | date | 검색할 오류 보고 데이터의 날짜 범위에 있는 종료 날짜 (형식) ```mm/dd/yyyy``` 입니다. 기본값은 현재 날짜입니다.   |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  예  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  예  |
| filter |문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다. 응답 본문에서 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>이름도</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>화살표</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>시장</strong></li><li><strong>deviceType</strong></li><li><strong>제품</strong></li><li><strong>date</strong></li></ul> | 예   |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. **일**, **주**또는 **월**문자열 중 하나일 수 있습니다. 지정 하지 않으면 기본값은 **day**입니다. **주** 또는 **월**을 지정 하는 경우 *failurename* 및 *failureHash* 값은 1000 버킷으로 제한 됩니다.<p/>  | 예 |
| orderby | 문자열 | 결과 데이터 값을 정렬 하는 문입니다. 구문은 *orderby = field [order], field [order],...* 입니다. *Field* 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>이름도</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>화살표</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>시장</strong></li><li><strong>deviceType</strong></li><li><strong>제품</strong></li><li><strong>date</strong></li></ul>*Order* 매개 변수는 선택 사항이 며 **asc** 또는 **desc** 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 *orderby* 문자열의 예입니다. *orderby = date, market*</p> |  예  |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li>**failureName**</li><li>**failureHash**</li><li>**화살표**</li><li>**osVersion**</li><li>**eventType**</li><li>**시장**</li><li>**deviceType**</li></ul><p>반환 된 데이터 행에는 *groupby* 매개 변수 및 다음에 지정 된 필드가 포함 됩니다.</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>*Groupby* 매개 변수는 *aggregationLevel* 매개 변수와 함께 사용할 수 있습니다. 예: * &amp; Groupby = failureName, market &amp; aggregationLevel = week*</p></p> |  예  |


### <a name="request-example"></a>요청 예제

다음 예에서는 오류 보고 데이터를 가져오는 몇 가지 요청을 보여 줍니다. *ApplicationId* 값을 데스크톱 응용 프로그램의 제품 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
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

| 값           | 형식    | Description        |
|-----------------|---------|---------------------|
| date            | 문자열  | 오류 데이터에 대 한 날짜 범위의 첫 번째 날짜 (형식) ```yyyy-mm-dd``` 입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 더 긴 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다. *AggregationLevel* 값을 **hour**로 지정 하는 요청의 경우이 값에는 형식의 시간 값도 포함 됩니다 ```hh:mm:ss``` .  |
| applicationId   | 문자열  | 오류 데이터를 검색 한 데스크톱 응용 프로그램의 제품 ID입니다.    |
| productName | 문자열  | 연결 된 실행 파일의 메타 데이터에서 파생 된 데스크톱 응용 프로그램의 표시 이름입니다.   |
| appName | 문자열  |  TBD  |
| fileName | 문자열  | 데스크톱 응용 프로그램에 대 한 실행 파일의 이름입니다.   |
| failureName     | 문자열  | 오류 이름으로, 하나 이상의 문제 클래스, 예외/버그 검사 코드, 오류가 발생 한 이미지 이름 및 연결 된 함수 이름을 네 부분으로 구성 되어 있습니다.  |
| failureHash     | 문자열  | 오류에 대 한 고유 식별자입니다.   |
| 기호          | 문자열  | 이 오류에 할당 된 기호입니다. |
| osBuild       | 문자열  | 오류가 발생 한 OS의 네 부분으로 구성 된 빌드 번호입니다.  |
| osVersion       | 문자열  | 데스크톱 응용 프로그램이 설치 되는 OS 버전을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>알 수 없음</strong></li></ul>   |   
| osRelease | 문자열  | 오류가 발생 한 os 릴리스 또는 플 라이팅 링 (os 버전 내의 subpopulation)을 지정 하는 다음 문자열 중 하나입니다.<p/><p>Windows 10의 경우:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>빠른 참가자</strong></li><li><strong>참가자 느림</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>출시</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1의 경우:</p><ul><li><strong>Update 1</strong></li></ul><p>Windows 7의 경우:</p><ul><li><strong>서비스 팩 1</strong></li></ul><p>OS 릴리스 또는 플 라이팅 링을 알 수 없는 경우이 필드에 <strong>알 수 없는</strong>값이 있습니다.</p> |
| eventType       | 문자열  | 오류 이벤트의 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li>**크래시**</li><li>**끊어**</li><li>**ram**</li><li>**.jse**</li></ul>       |
| market          | 문자열  | 장치 시장의 ISO 3166 국가 코드입니다.   |
| deviceType      | 문자열  | 오류가 발생 한 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>컴퓨터</strong></li><li><strong>Server</strong></li><li><strong>태블릿</strong></li><li><strong>알 수 없음</strong></li></ul>    |
| applicationVersion     | 문자열  |   오류가 발생 한 응용 프로그램 실행 파일의 버전입니다.    |
| eventCount      | number | 지정된 집계 수준 중에 이 오류를 발생시킨 이벤트의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [데스크톱 애플리케이션에서 오류 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)
* [데스크톱 애플리케이션에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [데스크톱 애플리케이션의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-desktop-application.md)