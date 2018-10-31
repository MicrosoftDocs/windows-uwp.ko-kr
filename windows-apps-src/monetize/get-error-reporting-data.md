---
author: Xansky
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 집계 오류 보고 데이터를 가져옵니다.
title: 앱에 대한 오류 보고 데이터 가져오기
ms.author: mhopkins
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 오류
ms.localizationpriority: medium
ms.openlocfilehash: 81dabcf92136174b08c6a20fd9a98122fcd2c813
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5833755"
---
# <a name="get-error-reporting-data-for-your-app"></a>앱에 대한 오류 보고 데이터 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 앱의 집계 오류 보고 데이터를 JSON 형식으로 가져올 수 있습니다. 이 메서드는 지난 30 일 동안에서 발생 한 오류만 검색할 수 있습니다. 이 정보는 Windows 개발자 센터 대시보드에서 [상태 보고서](../publish/health-report.md)의 **오류** 섹션을 통해서도 확인할 수 있습니다.

[오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md), [스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md), [CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md) 메서드를 사용해 추가 오류 정보를 검색할 수 있습니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 오류 보고 데이터를 검색할 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 사용할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| startDate | date | 검색할 오류 보고 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. *aggregationLevel*이 **day**, **week** 또는 **month**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정해야 합니다. *aggregationLevel*이 **hour**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정하거나 ```yyyy-mm-dd hh:mm:ss``` 형식의 날짜 및 시간을 지정할 수 있습니다.<p/><p/>**참고:**&nbsp;&nbsp;이 메서드는 지난 30 일 동안에서 발생 한 오류만 검색할 수 있습니다.  |  아니요  |
| endDate | date | 검색할 오류 보고 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. *aggregationLevel*이 **day**, **week** 또는 **month**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정해야 합니다. *aggregationLevel*이 **hour**인 경우 이 매개 변수는 ```mm/dd/yyyy``` 형식의 날짜를 지정하거나 ```yyyy-mm-dd hh:mm:ss``` 형식의 날짜 및 시간을 지정할 수 있습니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter |string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**출시**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | 아니요   |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. **hour**, **day**, **week** 또는 **month** 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 **day**입니다. **week** 또는 **month**를 지정하는 경우 *failureName* 및 *failureHash* 값은 1000개 버킷으로 제한됩니다.<p/><p/>**참고:**&nbsp;&nbsp;**hour**를 지정하면 지난 72시간의 오류 데이터만 검색할 수 있습니다. 72시간보다 오래된 오류 데이터를 검색하려면 **day**를 지정하거나 다른 집계 수준 중 하나를 지정합니다.  | 아니요 |
| orderby | string | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 *orderby=field [order],field [order],...* 입니다. *field* 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**출시**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>*order* 매개 변수는 옵션이며 **asc** 또는 **desc**로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 *orderby* 문자열 예입니다. *orderby=date,market*</p> |  아니요  |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 다음 필드를 지정할 수 있습니다.<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**OSVersion**</li><li>**eventType**</li><li>**출시**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>반환되는 데이터 행은 *groupby* 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>*groupby* 매개 변수는 *aggregationLevel* 매개 변수와 함께 사용할 수 있습니다. 예: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 오류 보고 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
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
| applicationId   | 문자열  | 오류 데이터를 검색할 앱의 스토어 ID입니다.   |
| applicationName | string  | 앱의 표시 이름   |
| failureName     | 문자열  | 오류 이름은 하나 이상의 문제 클래스, 예외/버그 확인 코드, 오류가 발생한 이미지의 이름 및 관련된 기능 이름 등 네 부분으로 구성됩니다.  |
| failureHash     | string  | 오류의 고유 식별자입니다.   |
| symbol          | string  | 이 오류에 할당된 기호입니다. |
| osVersion       | 문자열  | 오류가 발생한 OS 버전을 나타내는 다음 문자열 중 하나입니다.<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows8**</li><li>**Windows 8.1**</li><li>**Windows10**</li><li>**알 수 없음**</li></ul>  |
| osRelease       | 문자열  |  오류가 발생한 OS 릴리스 또는 플라이팅 링(OS 버전 내 하위 집단)을 나타내는 다음 문자열 중 하나입니다.<p/><p>Windows 10:</p><ul><li><strong>버전 1507</strong></li><li><strong>버전 1511</strong></li><li><strong>버전 1607</strong></li><li><strong>버전 1703</strong></li><li><strong>버전 1709</strong></li><li><strong>버전 1803</strong></li><li><strong>릴리스 미리 보기</strong></li><li><strong>초기 참가자</strong></li><li><strong>이후 참가자</strong></li></ul><p/><p>Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Windows Server 2016:</p><ul><li><strong>버전 1607</strong></li></ul><p>Windows 8.1:</p><ul><li><strong>업데이트 1</strong></li></ul><p>Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>OS 릴리스 또는 플라이팅 링을 알 수 없는 경우 이 필드의 값은 <strong>알 수 없음</strong>입니다.</p>    |
| eventType       | string  | 다음 문자열 중 하나입니다.<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>      |
| market          | string  | 디바이스 시장의 ISO 3166 국가 코드입니다.   |
| deviceType      | 문자열  | 오류가 발생한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<ul><li>**PC**</li><li>**Phone**</li><li>**콘솔**</li><li>**IoT**</li><li>**홀로그램**</li><li>**알 수 없음**</li></ul>    |
| packageName     | 문자열  | 이 오류와 연결된 앱 패키지의 고유 이름입니다.      |
| packageVersion  | 문자열  | 이 오류와 연결된 앱 패키지의 버전입니다.   |
| deviceCount     | 숫자 | 지정된 집계 수준 중에 이 오류에 해당하는 고유 장치 수입니다.  |
| eventCount      | 숫자 | 지정된 집계 수준 중에 이 오류를 발생시킨 이벤트의 수입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [앱 취득 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [앱 등급 가져오기](get-app-ratings.md)
* [앱 리뷰 가져오기](get-app-reviews.md)
