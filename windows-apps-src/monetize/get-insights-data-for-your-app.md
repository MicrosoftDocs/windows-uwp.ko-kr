---
author: mcleanbyron
description: Microsoft 저장소 분석 API에서에서이 메서드를 사용 하 여 앱에 대 한 통찰력 데이터를 가져옵니다.
title: 인 사이트 데이터 가져오기
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 저장소 서비스, Microsoft 저장소 분석 API insights (영문)
ms.localizationpriority: medium
ms.openlocfilehash: 53fbd91437e5dc702f8672c6cbadeea32a8a96bf
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2884252"
---
# <a name="get-insights-data"></a>인 사이트 데이터 가져오기

사용 하 여 특정된 날짜 범위 및 기타 선택적 필터 하는 동안 합병, 상태 및 응용 프로그램에 대 한 사용 현황 메트릭 관련 인 사이트 데이터를 가져올 Microsoft 저장소 분석 API에서에서이 메서드 합니다. 이 정보는 Windows 개발자 센터 대시보드에서 [정보 보고서](../publish/insights-report.md) 에서 사용할 수 있는 이기도합니다.

## <a name="prerequisites"></a>필수 구성 요소


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
| applicationId | string | 인 사이트 데이터를 검색 하려는 응용 프로그램의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 이 매개 변수를 지정 하지 않으면 하는 경우 사용자의 계정에 등록 된 모든 앱에 대 한 통찰력 데이터 응답 본문에 포함 됩니다.  |  아니요  |
| startDate | date | 검색할 인 사이트 데이터의 날짜 범위 내에서 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | date | 검색할 인 사이트 데이터의 날짜 범위 내에서 끝 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| filter | string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 예, *필터 dataType eq '취득' =* 합니다. <p/><p/>다음 필터 필드를 지정할 수 있습니다.<p/><ul><li><strong>취득</strong></li><li><strong>상태</strong></li><li><strong>사용 현황</strong></li></ul> | 아니요   |

### <a name="request-example"></a>요청 예제

다음 예제에서는 인 사이트 데이터를 가져오기 위한 요청 합니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 응용 프로그램에 대 한 통찰력 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래 [통찰력 값](#insight-values) 섹션을 참조 하십시오.                                                                                                                      |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                 |


### <a name="insight-values"></a>필요한 정보를 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | 인 사이트 데이터를 검색 하는 앱의 저장소 ID입니다.     |
| insightDate                | string | 앞서 특정 메트릭에의 변경 사항을 확인 하는 날짜입니다. 이 날짜 크게 증가 하는데이는 주의 끝을 나타내는 또는 그 이전에 주에 비해 메트릭을 감소 합니다. |
| 데이터 형식     | string | 이 정보를 설명 하는 일반 분석 영역을 지정 하는 문자열은 다음 중 하나입니다.<p/><ul><li><strong>취득</strong></li><li><strong>상태</strong></li><li><strong>사용 현황</strong></li></ul>   |
| insightDetail          | array | 하나 이상의 [InsightDetail 값](#insightdetail-values) 현재 정보에 대 한 세부 정보를 표시 하는 합니다.    |


### <a name="insightdetail-values"></a>InsightDetail 값

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | **데이터 형식** 값에 따라 필요한 정보를 현재 또는 현재 차원에 설명 하는 메트릭을 지정 하는 다음 값 중 하나입니다.<ul><li>**상태**대 한이 값은 항상 **적중 횟수**입니다.</li><li>**취득**이 값은 항상 **AcquisitionQuantity**.</li><li>**사용 현황**대 한이 값 중 하나일 수 있습니다 다음 문자열:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  정보에 대 한 단일 메트릭을 설명 하는 하나 이상의 개체입니다.   |
| PercentChange            | string |  전체 고객 기반 별 메트릭을 변경 하는 백분율입니다.  |
| DimensionName           | string |  현재 차원에 설명 된 메트릭의 이름입니다. **이벤트 유형**, **시장**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** 및 **성별**을 예로 들 수 있습니다.   |
| DimensionValue              | string | 현재 차원에 설명 된 메트릭의 값입니다. 예, **이벤트 유형** **DimensionName** 을 사용 하는 경우에 **크래시** 또는 **중단** **DimensionValue** 수 있습니다.   |
| FactValue     | string | 필요한 정보를 찾을 수 날짜 메트릭의 절대 값입니다.  |
| 방향 | string |  방향 변경 (**양수** 또는 **음수**)입니다.   |
| Date              | 문자열 |  앞서 현재 통찰력 또는 현재 차원 관련 된 변경 내용을 확인 하는 날짜입니다.   |

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

* [인 사이트 보고서](../publish/insights-report.md)
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
