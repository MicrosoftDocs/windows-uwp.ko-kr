---
description: Microsoft Store analytics API에서이 방법을 사용 하 여 데스크톱 응용 프로그램에 대 한 정보 데이터를 가져옵니다.
title: 데스크톱 애플리케이션에 대한 인사이트 데이터 가져오기
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, 통찰력
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd60425a5ec3c040417aded818c766db80eb59eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172897"
---
# <a name="get-insights-data-for-your-desktop-application"></a>데스크톱 애플리케이션에 대한 인사이트 데이터 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 [Windows 데스크톱 응용](/windows/desktop/appxpkg/windows-desktop-application-program)프로그램에 추가한 데스크톱 응용 프로그램의 상태 메트릭과 관련 된 정보 제공 데이터를 가져옵니다. 이 데이터는 파트너 센터의 데스크톱 응용 프로그램에 대 한 [상태 보고서](/windows/desktop/appxpkg/windows-desktop-application-program#health-report) 에서도 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  Description      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Insights 데이터를 가져오려는 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 얻으려면 파트너 센터 (예: **상태 보고서**) [에서 데스크톱 응용 프로그램에 대 한 분석 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 를 열고 URL에서 제품 id를 검색 합니다. 이 매개 변수를 지정 하지 않으면 계정에 등록 된 모든 앱에 대 한 정보 데이터가 응답 본문에 포함 됩니다.  |  예  |
| startDate | date | 검색할 정보 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜의 30 일입니다. |  예  |
| endDate | date | 검색할 정보 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| filter | 문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다. 예: *filter = dataType eq ' 획득 '*. <p/><p/>현재이 메서드는 필터 **상태**를 지원 합니다.  | 예   |

### <a name="request-example"></a>요청 예제

다음 예제에서는 insights 데이터를 가져오는 요청을 보여 줍니다. *ApplicationId* 값을 데스크톱 응용 프로그램에 대 한 적절 한 값으로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 앱에 대 한 정보 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [통찰력 값](#insight-values) 섹션을 참조 하십시오.                                                                                                                      |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다.                 |


### <a name="insight-values"></a>통찰력 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값               | 형식   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 문자열 | Insights 데이터를 검색 한 데스크톱 응용 프로그램의 제품 ID입니다.     |
| insightDate                | 문자열 | 특정 메트릭에 대 한 변경 내용을 확인 한 날짜입니다. 이 날짜는 해당 주 이전 주에 비해 메트릭이 크게 증가 하거나 감소 하는 주 끝을 나타냅니다. |
| dataType     | 문자열 | 이 통찰력이 알리는 일반 분석 영역을 지정 하는 문자열입니다. 현재이 메서드는 **상태**를 지원 합니다.    |
| insightDetail          | array | 현재 정보에 대 한 세부 정보를 나타내는 하나 이상의 [InsightDetail 값](#insightdetail-values) 입니다.    |


### <a name="insightdetail-values"></a>InsightDetail 값

| 값               | 형식   | Description                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 문자열 | 현재 정보 또는 현재 차원에서 설명 하는 메트릭을 나타내는 문자열입니다. 현재이 메서드는 **HitCount**값만 지원 합니다.  |
| 하위 차원         | array |  정보에 대 한 단일 메트릭을 설명 하는 하나 이상의 개체입니다.   |
| PercentChange            | 문자열 |  전체 고객 기준에서 메트릭이 변경 된 비율입니다.  |
| DimensionName           | 문자열 |  현재 차원에 설명 된 메트릭의 이름입니다. 예를 들면 **EventType**, **Market**, **DeviceType**및 **packageversion**이 있습니다.   |
| DimensionValue              | 문자열 | 현재 차원에 설명 된 메트릭의 값입니다. 예를 들어 **DimensionName** 가 **EventType**인 경우 **dimensionvalue** 는 **충돌** 하거나 **중단**될 수 있습니다.   |
| FactValue     | 문자열 | 정보가 검색 된 날짜에 대 한 메트릭의 절대값입니다.  |
| 방향 | 문자열 |  변경의 방향 (**양수** 또는 **음수**)입니다.   |
| 날짜              | 문자열 |  현재 정보 또는 현재 차원과 관련 된 변경 내용을 식별 한 날짜입니다.   |

### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [Windows 데스크톱 응용 프로그램 프로그램](/windows/desktop/appxpkg/windows-desktop-application-program)
* [상태 보고서](/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)