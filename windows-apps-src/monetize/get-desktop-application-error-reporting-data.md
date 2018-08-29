---
author: mcleanbyron
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 데스크톱 응용 프로그램의 집계 오류 보고 데이터를 가져옵니다.
title: 데스크톱 응용 프로그램에 대한 오류 보고 데이터 가져오기
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 오류, 데스크톱 응용 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: 71c566ff375f36108d724f3c550570b3332f4c6b
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2905786"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>데스크톱 응용 프로그램에 대한 오류 보고 데이터 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 [Windows 데스크톱 응용 프로그램 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504)에 추가한 데스크톱 응용 프로그램의 집계 오류 보고 데이터를 가져옵니다. 이 메서드는 지난 30 일 동안에서 발생 한 오류만 검색할 수 있습니다. 이 정보는 Windows 개발자 센터 대시보드에서 데스크톱 응용 프로그램의 [상태 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504)를 통해서도 확인할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 오류 보고 데이터를 검색할 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 가져오려면 [데스크톱 응용 프로그램의 개발자 센터 분석 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504)(예: **상태 보고서**)를 열고 URL에서 제품 ID를 검색합니다. |  예  |
| startDate | 날짜 | 검색할 오류 보고 데이터의 날짜 범위에 대한 시작 날짜입니다. 형식은 ```mm/dd/yyyy```입니다. 기본값은 현재 날짜입니다.<p/><p/>**참고:**&nbsp;&nbsp;이 메서드는 지난 30 일 동안에서 발생 한 오류만 검색할 수 있습니다.  |  아니요  |
| endDate | 날짜 | 검색할 오류 보고 데이터의 날짜 범위에 대한 종료 날짜입니다. 형식은 ```mm/dd/yyyy```입니다. 기본값은 현재 날짜입니다.   |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter |string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>출시</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul> | 아니요   |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. **day**, **week** 또는 **month** 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 **day**입니다. **week** 또는 **month**를 지정하는 경우 *failureName* 및 *failureHash* 값은 1000개 버킷으로 제한됩니다.<p/>  | 아니요 |
| orderby | string | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 *orderby=field [order],field [order],...* 입니다. *field* 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>출시</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul>*order* 매개 변수는 옵션이며 **asc** 또는 **desc**로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 *orderby* 문자열 예입니다. *orderby=date,market*</p> |  아니요  |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**OSVersion**</li><li>**eventType**</li><li>**출시**</li><li>**deviceType**</li></ul><p>반환되는 데이터 행은 *groupby* 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>*groupby* 매개 변수는 *aggregationLevel* 매개 변수와 함께 사용할 수 있습니다. 예: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 오류 보고 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 데스크톱 응용 프로그램의 제품 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형    | 설명     |
|------------|---------|--------------|
| 값      | 배열   | 집계 오류 보고 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [오류 값](#error-values) 섹션을 참조하세요.     |
| @nextLink  | string  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 오류의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | 정수 | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.     |


### <a name="error-values"></a>오류 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 유형    | 설명        |
|-----------------|---------|---------------------|
| date            | 문자열  | 오류 데이터에 대한 날짜 범위의 시작 날짜입니다. 형식은 ```yyyy-mm-dd```입니다. 요청에서 하루를 지정하는 경우 이 값은 해당 날짜입니다. 요청에서 더 긴 날짜 범위를 지정하는 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. **hour**의 *aggregationLevel* 값을 지정하는 요청의 경우 이 값에는 ```hh:mm:ss``` 형식의 시간 값도 포함됩니다.  |
| applicationId   | 문자열  | 오류 데이터를 검색한 데스크톱 응용 프로그램의 제품 ID입니다.    |
| productName | 문자열  | 연결된 실행 파일의 메타데이터에서 파생된 데스크톱 응용 프로그램의 표시 이름입니다.   |
| appName | 문자열  |  TBD  |
| fileName | 문자열  | 데스크톱 응용 프로그램의 실행 파일 이름입니다.   |
| failureName     | 문자열  | 오류 이름은 하나 이상의 문제 클래스, 예외/버그 확인 코드, 오류가 발생한 이미지의 이름 및 관련된 기능 이름 등 네 부분으로 구성됩니다.  |
| failureHash     | string  | 오류의 고유 식별자입니다.   |
| symbol          | string  | 이 오류에 할당된 기호입니다. |
| osBuild       | 문자열  | 오류가 발생한 OS의 네 부분으로 된 빌드 번호입니다.  |
| osVersion       | 문자열  | 데스크톱 응용 프로그램이 설치된 OS 버전을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows10</strong></li><li><strong>WindowsServer 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>알 수 없음</strong></li></ul>   |   
| osRelease | 문자열  | 오류가 발생한 OS 릴리스 또는 플라이팅 링(OS 버전 내 하위 집단)을 나타내는 다음 문자열 중 하나입니다.<p/><p>Windows 10:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>초기 참가자</strong></li><li><strong>이후 참가자</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1:</p><ul><li><strong>업데이트 1</strong></li></ul><p>Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p> |
| eventType       | 문자열  | 오류 이벤트 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>       |
| market          | string  | 디바이스 시장의 ISO 3166 국가 코드입니다.   |
| deviceType      | string  | 오류가 발생한 디바이스 유형을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>PC</strong></li><li><strong>서버</strong></li><li><strong>태블릿</strong></li><li><strong>알 수 없음</strong></li></ul>    |
| applicationVersion     | 문자열  |   오류가 발생한 응용 프로그램 실행 파일의 버전입니다.    |
| eventCount      | integer | 지정된 집계 수준 중에 이 오류를 발생시킨 이벤트의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [데스크톱 응용 프로그램에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)
* [데스크톱 응용 프로그램에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [데스크톱 응용 프로그램의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-desktop-application.md)
