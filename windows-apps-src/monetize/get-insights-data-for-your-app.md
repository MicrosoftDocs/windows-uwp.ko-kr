---
author: Xansky
description: Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 앱에 대 한 정보 데이터 가져오기.
title: 정보 데이터 가져오기
ms.author: mhopkins
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, 인 사이트
ms.localizationpriority: medium
ms.openlocfilehash: ff70f38ff636b0b1d885981fb9e353ac9afe69c2
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597013"
---
# <a name="get-insights-data"></a>정보 데이터 가져오기

사용 하 여 지정 된 날짜 범위 및 다른 선택 필터 동안 구입, 상태 및 앱에 대 한 사용량 메트릭을 관련 인 사이트 데이터를 가져오는 Microsoft Store 분석 API에서에서이 메서드. 이 정보는 Windows 개발자 센터 대시보드의 [인 사이트 보고서](../publish/insights-report.md) 에서 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | string | 인 사이트 데이터를 검색 하려는 앱의 [스토어 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 이 매개 변수를 지정 하지 않으면 경우 응답 본문에는 계정에 등록 된 모든 앱에 대 한 정보 데이터 포함 됩니다.  |  아니요  |
| startDate | date | 검색할 인 사이트 데이터의 날짜 범위에 대 한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | date | 검색할 인 사이트 데이터의 날짜 범위에 대 한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| filter | string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 예를 들어 *필터 dataType eq '취득' =*. <p/><p/>다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>취득</strong></li><li><strong>상태</strong></li><li><strong>사용</strong></li></ul> | 아니요   |

### <a name="request-example"></a>요청 예제

다음 예제에서는 인 사이트 데이터를 가져오는 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 앱에 대 한 인 사이트 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래 [통찰력 값](#insight-values) 섹션을 참조 하세요.                                                                                                                      |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                 |


### <a name="insight-values"></a>통찰력 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | 인 사이트 데이터를 검색할 앱의 스토어 ID입니다.     |
| insightDate                | string | 특정 메트릭 변화를 식별 하는 날짜입니다. 이 날짜는 크게 증가 감지 것 주가 끝날 나타내거나는 이전 주에 비해 메트릭이 감소 합니다. |
| 데이터 형식     | string | 이 정보를 설명 하는 일반 분석 영역을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>취득</strong></li><li><strong>상태</strong></li><li><strong>사용</strong></li></ul>   |
| insightDetail          | array | 하나 이상의 [InsightDetail 값](#insightdetail-values) 현재 통찰력에 대 한 세부 정보를 나타내는 합니다.    |


### <a name="insightdetail-values"></a>InsightDetail 값

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | **데이터 형식** 값에 따라 현재 통찰력 또는 현재 차원에 설명 하는 메트릭을 지정 하는 다음 값 중 하나.<ul><li>**상태**대 한이 값은 항상 **적중 횟수**입니다.</li><li>**구입**이 값은 항상 **AcquisitionQuantity**.</li><li>**사용**대 한이 값 다음 문자열 중 하나일 수 있습니다.<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  통찰력에 대 한 단일 메트릭을 설명 하는 하나 이상의 개체입니다.   |
| PercentChange            | string |  전체 고객 기반에 걸쳐 변경 된 메트릭을 백분율입니다.  |
| DimensionName           | string |  현재 차원에 설명 된 메트릭의 이름입니다. **EventType**, **지역/국가**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** 및 **성별**을 예로 들 수 있습니다.   |
| DimensionValue              | string | 현재 차원에 설명 된 메트릭의 값입니다. 예를 들어 **DimensionName** **EventType**인 경우에 **크래시** 또는 **중단** **DimensionValue** 수 있습니다.   |
| FactValue     | string | 통찰력은 감지 된 날짜에 메트릭의 절대 값입니다.  |
| 방향 | string |  방향 변경 내용 (**양수** 또는 **음수**)입니다.   |
| Date              | 문자열 |  현재 통찰력 또는 현재 차원 관련 된 변경 내용을 식별 하는 날짜입니다.   |

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>관련 항목

* [인사이트 보고서](../publish/insights-report.md)
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
