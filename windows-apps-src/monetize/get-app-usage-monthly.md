---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 및 기타 선택적 필터에 대 한 월간 앱 사용 데이터를 가져올 수 있습니다.
title: 월별 앱 사용 현황 가져오기
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store analytics API, 사용
ms.localizationpriority: medium
ms.openlocfilehash: a1b82e1f538a0abfda8cb8d4f7ac677464025c8e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492818"
---
# <a name="get-monthly-app-usage"></a>월별 앱 사용 현황 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 날짜 범위 (지난 90 일간만 해당) 및 기타 선택적 필터를 사용 하 여 응용 프로그램에 대 한 JSON 형식으로 집계 사용 데이터 (Xbox 여럿이를 포함 하지 않음)를 가져올 수 있습니다. 이 정보는 파트너 센터의 [사용량 보고서](../publish/usage-report.md) 에서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 사항입니다. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수     | Type   |  설명                                                                                                    |  필요한 공간  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 문자열 | 검토 데이터를 검색 하려는 앱의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. |  예       |
| startDate     | date   | 검색할 검토 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜입니다.                   |  아니요        |
| endDate       | date   | 검색할 검토 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다.                     |  예        |
| top           | int    | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다.                          |  아니요        |
| skip          | int    | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다.                         |  아니요        |  
| filter        |문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 eq 또는 ne 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, and 또는 or를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 필터 매개 변수에서 작은따옴표로 묶어야 합니다. 응답 본문에서 다음 필드를 지정할 수 있습니다. <ul><li>**시장**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 아니요         |  
| orderby       | 문자열 | 결과 데이터 값을 정렬 하는 문입니다. 구문은 <em>orderby = field [order], field [order],...</em>입니다. <em>Field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**시장**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>Order</em> 매개 변수는 선택 사항이 며 **asc** 또는 **desc** 로 각 필드에 대해 오름차순 또는 내림차순을 지정할 수 있습니다. 기본값은 **asc**입니다.</p><p>다음은 <em>orderby</em> 문자열의 예입니다. <em>orderby = date, market</em></p> |  아니요        |
| groupby       | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 응답 본문에서 다음 필드를 지정할 수 있습니다.<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**시장**</li><li>**date**</li></ul><p>반환 된 데이터 행에는 <em>groupby</em> 매개 변수 및 다음에 지정 된 필드가 포함 됩니다.</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>Groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em> &amp; Groupby = agegroup, market &amp; aggregationLevel = week</em></p> |  아니요        |


### <a name="request-example"></a>요청 예제

다음 예제에서는 월간 앱 사용 현황 데이터를 가져오는 요청을 보여 줍니다. *ApplicationId* 값을 앱의 스토어 ID로 바꿉니다.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 값      | array  | 집계 사용 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 다음 표를 참조 하십시오. |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대 한 데이터 검토 행이 1만 개를 초과 하는 경우이 값이 반환 됩니다.                 |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.                                                                          |

 
### <a name="usage-values"></a>사용 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값                     | 형식    | 설명                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | 문자열  | 사용 현황 데이터에 대 한 날짜 범위의 첫 번째 날짜입니다. 요청에서 하루를 지정 하는 경우이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정 하는 경우이 값은 해당 날짜 범위의 첫 번째 날짜입니다.                          |
| applicationId             | 문자열  | 사용 현황 데이터를 검색 하는 앱의 저장소 ID입니다.                            |
| applicationName           | 문자열  | 응용 프로그램의 표시 이름입니다.                                                                |
| market                    | 문자열  | 고객이 앱을 사용한 시장의 ISO 3166 국가 코드입니다.                   |
| packageVersion            | 문자열  | 사용이 발생 한 패키지의 버전입니다.                                            |
| deviceType                | 문자열  | 사용이 발생 한 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<ul><li>**컴퓨터**</li><li>**내선**</li><li>**콘솔-Xbox One**</li><li>**콘솔-Xbox 시리즈 X**</li><li>**태블릿**</li><li>**IoT**</li><li>**Server**</li><li>**Holographic**</li><li>**알 수 없음**</li></ul>                                                                                                                           |
| subscriptionName          | 문자열  | Xbox Game Pass를 통해 사용량을 사용 하 고 있는지 여부를 나타냅니다.                                              |
| monthlySessionCount       | long    | 해당 월 동안의 사용자 세션 수입니다.                                              |
| engagementDurationMinutes | double  | 사용자가 앱을 사용 하 여 사용자가 앱을 적극적으로 사용 하는 시간 (분)으로, 앱이 시작 (프로세스 시작) 되 고 종료 될 때 (프로세스 종료) 또는 비활성 기간 후에 시작 됩니다.                               |
| monthlyActiveUsers        | long    | 해당 월 앱을 사용 하는 고객 수입니다.                                           |
| monthlyActiveDevices      | long    | 앱이 시작 (프로세스 시작) 되 고 종료 될 때 (프로세스 종료) 또는 비활성 기간 후에 종료 될 때부터 앱을 실행 하는 장치 수입니다.                                                        |
| monthlyNewUsers           | long    | 매월 처음으로 앱을 사용한 고객 수입니다.                    |
| averageDailyActiveUsers   | double  | 매일 앱을 사용 하는 평균 고객 수입니다.                             |
| averageDailyActiveDevices | double  | 매일 모든 사용자가 앱과 상호 작용 하는 데 사용 하는 평균 장치 수입니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [매일 앱 ussage 가져오기](get-app-usage-daily.md)
* [앱 인수 가져오기](get-app-acquisitions.md)
* [추가 기능 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
