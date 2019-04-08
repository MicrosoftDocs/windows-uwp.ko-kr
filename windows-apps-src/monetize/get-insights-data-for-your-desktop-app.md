---
description: Microsoft Store analytics API에서에서이 메서드를 사용 하 여 데스크톱 응용 프로그램에 대 한 insights 데이터를 가져오려고 합니다.
title: 데스크톱 응용 프로그램에 대한 정보 데이터 가져오기
ms.date: 07/31/2018
ms.topic: article
keywords: Microsoft Store 분석 API, insights, 저장소 서비스, uwp, windows 10
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5545d27668b23e5b7ae91201421dfa4c92f9c8ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618138"
---
# <a name="get-insights-data-for-your-desktop-application"></a>데스크톱 응용 프로그램에 대한 정보 데이터 가져오기

사용 하 여 데스크톱 응용 프로그램에 추가한 상태 메트릭을 관련 insights 데이터를 가져올 수 있도록 Microsoft Store 분석 API에서에서이 메서드는 [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)합니다. 또한이 데이터를 사용할 수는 [상태 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) 파트너 센터에서 데스크톱 응용 프로그램에 대 한 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Insights 데이터를 가져오려는 데스크톱 응용 프로그램의 제품 ID입니다. 열 데스크톱 응용 프로그램의 제품 ID를 가져오려면 [파트너 센터에서 데스크톱 응용 프로그램에 대 한 분석 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504) (같은 합니다 **상태 보고서**) URL에서 제품 ID를 검색 합니다. 이 매개 변수를 지정 하지 않으면 하는 경우 응답 본문에는 계정에 등록 된 모든 앱에 대 한 정보 데이터가 포함 됩니다.  |  아니오  |
| startDate | date | 검색할 insights 데이터의 날짜 범위의 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니오  |
| endDate | date | 검색할 insights 데이터의 날짜 범위의 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니오  |
| filter | 문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 예를 들어 *필터 dataType eq '구입' =* 합니다. <p/><p/>이 메서드는 필터만 지원 되는 현재 **상태**합니다.  | 아니오   |

### <a name="request-example"></a>요청 예제

다음 예제에서는 insights 데이터를 가져오기 위한 요청을 보여 줍니다. 대체는 *applicationId* 데스크톱 응용 프로그램에 대 한 적절 한 값입니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 앱에 대 한 insights 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 참조는 [Insight 값](#insight-values) 섹션 아래.                                                                                                                      |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                 |


### <a name="insight-values"></a>정보 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 형식   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 문자열 | Insights 데이터를 검색 하는 데스크톱 응용 프로그램의 제품 ID입니다.     |
| insightDate                | 문자열 | 특정 메트릭이 변경을 식별 하는 날짜입니다. 이 날짜는 상당한 증가 감지 하는 주 끝을 나타내는 또는 이전 주에 비해 메트릭이 감소 합니다. |
| 데이터 형식     | 문자열 | 이 정보를 알려 주는 일반 analytics 영역을 지정 하는 문자열입니다. 현재이 메서드는 지원 **상태**합니다.    |
| insightDetail          | 배열 | 하나 이상의 [InsightDetail 값](#insightdetail-values) 현재 정보에 대 한 세부 정보를 나타냅니다.    |


### <a name="insightdetail-values"></a>InsightDetail 값

| 값               | 형식   | 설명                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 문자열 | 현재 정보 또는 현재 차원에 설명 하는 메트릭을 표시 하는 문자열입니다. 이 메서드는 값만 지원 되는 현재 **히트 카운트**합니다.  |
| SubDimensions         | 배열 |  해당 정보에 대 한 단일 메트릭을 설명 하는 하나 이상의 개체입니다.   |
| PercentChange            | 문자열 |  전체 고객 기반에서 메트릭을 변경 비율입니다.  |
| DimensionName           | 문자열 |  현재 차원에 설명 된 메트릭의 이름입니다. 예를 들면 **EventType**, **시장**를 **DeviceType**, 및 **PackageVersion**합니다.   |
| DimensionValue              | 문자열 | 현재 차원에 설명 된 메트릭 값입니다. 예를 들어 경우 **DimensionName** 은 **EventType**, **DimensionValue** 않을 **크래시** 또는 **중단** .   |
| FactValue     | 문자열 | 정보가 검색 된 날짜에서 메트릭의 절대 값입니다.  |
| 방향 | 문자열 |  변경의 방향 (**양의** 하거나 **음수**).   |
| 날짜              | 문자열 |  현재 정보 또는 현재 차원과 관련 된 변경 내용을 식별 하는 날짜입니다.   |

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

* [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [상태 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
