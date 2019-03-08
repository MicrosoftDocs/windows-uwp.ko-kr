---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Microsoft Store analytics API에서에서이 메서드를 사용 하 여 지정된 된 날짜 범위 및 다른 선택적 필터에 대 한 월별 앱 사용 현황 데이터를 가져오려고 합니다.
title: 월별 앱 사용 현황 가져오기
ms.date: 08/15/2018
ms.topic: article
keywords: Microsoft Store 분석 API 사용, 저장소 서비스, uwp, windows 10
ms.localizationpriority: medium
ms.openlocfilehash: 48ad049b3f310f8b375a28d9695dd9280d686c43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662928"
---
# <a name="get-monthly-app-usage"></a>월별 앱 사용 현황 가져오기

Microsoft Store 분석 API에서이 메서드를 사용 하 여 지정한 날짜 범위 (지난 90 일만 사용 가능) 및 다른 선택적 필터 중 응용 프로그램에 대 한 JSON 형식으로 집계 사용 현황 데이터 (Xbox 멀티 플레이 제외)을 가져오려고 합니다. 이 정보를 사용할 수 있습니다 합니다 [사용 현황 보고서](../publish/usage-report.md) 파트너 센터에서.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수     | 형식   |  설명                                                                                                    |  필수  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 문자열 | 리뷰 데이터를 검색할 앱의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. |  예       |
| startDate     | date   | 검색할 리뷰 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다.                   |  아니오        |
| endDate       | date   | 검색할 리뷰 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다.                     |  아니오        |
| top           | int    | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다.                          |  아니오        |
| skip          | int    | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다.                         |  아니오        |  
| filter        |문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에 포함 된 응답 본문 및 eq 또는 ne 연산자와 연관 된 값에서 필드 이름 및 문을 사용 하 여 결합할 수 있고 또는 합니다. 문자열 값 필터 매개 변수에 작은따옴표로 묶어야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다. <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 아니오         |  
| orderby       | 문자열 | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>order</em> 매개 변수는 옵션이며 **asc** 또는 **desc**로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니오        |
| groupby       | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  아니오        |


### <a name="request-example"></a>요청 예제

다음 예제에서는 월별 앱 사용 현황 데이터를 가져오기 위한 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 값      | 배열  | 집계 사용 현황 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요. |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 리뷰 데이터 행이 10000개보다 많은 경우 이 값이 반환됩니다.                 |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                                                                          |

 
### <a name="usage-values"></a>사용 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값                     | 형식    | 설명                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | 문자열  | 사용 현황 데이터에 대 한 날짜 범위에서 첫 번째 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다.                          |
| applicationId             | 문자열  | 사용 현황 데이터를 검색 하는 앱의 Store ID입니다.                            |
| applicationName           | 문자열  | 앱의 표시 이름                                                                |
| 출시                    | 문자열  | ISO 3166 국가 코드 시장의 고객 앱을 사용 하는 위치입니다.                   |
| packageVersion            | 문자열  | 사용이 발생 한 패키지의 버전입니다.                                            |
| deviceType                | 문자열  | 다음 문자열 중 하나로 지정 하는 사용량이 발생 하는 장치의 유형:<ul><li>**PC**</li><li>**Phone**</li><li>**콘솔**</li><li>**Tablet**</li><li>**IoT**</li><li>**Server**</li><li>**홀로그램**</li><li>**알 수 없음**</li></ul>                                                                                                                           |
| subscriptionName          | 문자열  | Xbox Game Pass 통해 사용 된 경우를 나타냅니다.                                              |
| monthlySessionCount       | long    | 해당 월 동안 사용자 세션의 수입니다.                                              |
| engagementDurationMinutes | double  | 여기에서 사용자는 적극적으로 앱이 시작 될 때 시작 시간의 고유 마침표로 측정 앱 (프로세스 시작)을 사용 하 고 (프로세스 종료)를 종료 될 때 또는 일정 한 비활성 기간 후 종료 분입니다.                               |
| monthlyActiveUsers        | long    | 해당 월의 앱을 사용 하 여 고객의 수입니다.                                           |
| monthlyActiveDevices      | long    | 장치의 고유 기간, 앱이 시작 될 때 시작 (프로세스 시작)에 대 한 앱을 실행 하 고 (프로세스 종료)를 종료 될 때 종료 또는 일정 한 비활성 기간 후 수입니다.                                                        |
| monthlyNewUsers           | long    | 처음으로 해당 월에 대 한 앱을 사용 하는 고객의 수입니다.                    |
| averageDailyActiveUsers   | double  | 매일 앱을 사용 하는 고객의 평균 수입니다.                             |
| averageDailyActiveDevices | double  | 매일의 모든 사용자가 앱과 상호 작용 하는 데 사용 되는 장치의 평균 수입니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [매일 앱 ussage 가져오기](get-app-usage-daily.md)
* [인 앱 구매 가져오기](get-app-acquisitions.md)
* [추가 기능 구매 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
