---
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 다른 선택적 필터에 대 한 일일 앱 사용 현황 데이터를 가져옵니다.
title: 일일 앱 사용 현황 가져오기
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, 사용
ms.localizationpriority: medium
ms.openlocfilehash: d3460b61e6a9a7c36be6fd87c4dc7fcc1ab811d1
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7847635"
---
# <a name="get-daily-app-usage"></a>일일 앱 사용 현황 가져오기

Microsoft Store 분석 API에서이 메서드를 사용 하 여 (지난 90 일만)는 지정 된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램에 대 한 집계 사용 현황 데이터 (Xbox 멀티 플레이 포함 하지 않음)를 JSON 형식으로 가져옵니다. 이 정보는 파트너 센터에서 [사용 보고서](../publish/usage-report.md) 에서 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수     | 유형   |  설명                                                                                                    |  필수  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 문자열 | 리뷰 데이터를 검색할 앱의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. |  예       |
| startDate     | date   | 검색할 리뷰 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다.                   |  아니요        |
| endDate       | date   | 검색할 리뷰 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다.                     |  아니요        |
| top           | int    | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다.                          |  아니요        |
| skip          | int    | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다.                         |  아니요        |  
| filter        |string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 eq 또는 ne 연산자와 연결된 값이 포함되어 있으며 문은 and 또는 or를 사용하여 결합될 수 있습니다. 문자열 값은 filter 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다. <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 아니요         |  
| orderby       | string | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**출시**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>order</em> 매개 변수는 옵션이며 **asc** 또는 **desc**로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p>                                                                                                   |  아니요        |
| groupby       | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다. <ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p>                                                                                                             |  아니요        |


### <a name="request-example"></a>요청 예제

다음 예제에서는 일일 앱 사용 현황 데이터를 가져오는 데 필요한 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답



### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 값      | array  | 집계 사용 현황 데이터를 포함 된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요. |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 리뷰 데이터 행이 10000개보다 많은 경우 이 값이 반환됩니다.                 |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                                                                          |

 
### <a name="usage-values"></a>사용법 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값                     | 유형    | 설명                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | string  | 사용 현황 데이터에 대 한 날짜 범위에 대 한 첫 번째 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다.        |
| applicationId             | string  | 사용 현황 데이터를 검색할 앱의 스토어 ID입니다.          |
| applicationName           | string  | 앱의 표시 이름                                              |
| deviceType                | string  | 다음 문자열 중 하나 나타내는 사용량 발생 한 디바이스 유형입니다.<ul><li>**PC**</li><li>**Phone**</li><li>**콘솔**</li><li>**태블릿**</li><li>**IoT**</li><li>**서버**</li><li>**홀로그램**</li><li>**Unknown**</li></ul>                                                                                                         |
| packageVersion            | string  | 사용량 발생 한 패키지의 버전입니다.                          |
| market                    | string  | 고객이 앱을 사용한 시장의 ISO 3166 국가 코드입니다. |
| subscriptionName          | 문자열  | Xbox Game Pass 통해 사용 했는지를 나타냅니다.                            |
| dailySessionCount         | long    | 해당 날짜에 사용자 세션 수입니다.                                  |
| engagementDurationMinutes | double  | 사용자가 적극적으로 측정 고유한 기간의 시간, 앱이 시작할 때 앱 (프로세스 시작)를 사용 하 여 있고 때 (프로세스 종료) 또는 일정 기간의 비활성 상태 후 종료 있는 분입니다.             |
| dailyActiveUsers          | long    | 해당 날짜는 앱을 사용 하 여 고객의 수입니다.                           |
| dailyActiveDevices        | long    | 모든 사용자가 앱과 함께 작용 하는 데 사용한 일일 장치의 수입니다.  |
| dailyNewUsers             | long    | 처음으로 해당 날짜에 앱을 사용 하는 고객의 수입니다.    |
| monthlyActiveUsers        | long    | 해당 월의 앱을 사용 하 여 고객의 수입니다.                         |
| monthlyActiveDevices      | long    | 일정 기간의 비활성 상태 후 또는 고유한 기간의 시간, 앱이 시작할 때 (프로세스 시작)에 대 한 앱을 실행 하 고 (프로세스 종료) 시점까지 장치의 수입니다.                                      |
| monthlyNewUsers           | long    | 처음으로 해당 월에 대 한 앱을 사용한 고객의 수입니다.  |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2018-08-10",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 17718,
      "engagementDurationMinutes": 582540.2,
      "dailyActiveUsers": 7078,
      "dailyActiveDevices": 6964,
      "dailyNewUsers": 1727,
      "monthlyActiveUsers": 123609,
      "monthlyActiveDevices": 116723,
      "monthlyNewUsers": 72271
    },
    {
      "date": "2018-08-11",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 19899,
      "engagementDurationMinutes": 655379.7,
      "dailyActiveUsers": 7877,
      "dailyActiveDevices": 7789,
      "dailyNewUsers": 2062,
      "monthlyActiveUsers": 123666,
      "monthlyActiveDevices": 116770,
      "monthlyNewUsers": 72276
    },
    {
      "date": "2018-08-12",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 21349,
      "engagementDurationMinutes": 735640.5,
      "dailyActiveUsers": 8456,
      "dailyActiveDevices": 8309,
      "dailyNewUsers": 2433,
      "monthlyActiveUsers": 124241,
      "monthlyActiveDevices": 117289,
      "monthlyNewUsers": 72732
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [월별 앱 ussage 가져오기](get-app-usage-monthly.md)
* [앱 취득 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
