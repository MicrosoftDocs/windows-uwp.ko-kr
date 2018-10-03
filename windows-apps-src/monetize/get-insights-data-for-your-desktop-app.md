---
author: mcleanbyron
description: Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 데스크톱 응용 프로그램에 대 한 인 사이트 데이터를 가져옵니다.
title: 데스크톱 응용 프로그램에 대한 정보 데이터 가져오기
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, 인 사이트
ms.localizationpriority: medium
ms.openlocfilehash: e7ca6eed40af37276b5b4c98ec7b1b709bdadfb9
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4316907"
---
# <a name="get-insights-data-for-your-desktop-application"></a>데스크톱 응용 프로그램에 대한 정보 데이터 가져오기

Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)을 추가 하는 데스크톱 응용 프로그램에 대 한 메트릭을 상태와 관련 된 데이터 통찰력을 얻으세요. 이 데이터는 또한 Windows 개발자 센터 대시보드에서 데스크톱 응용 프로그램에 대 한 [상태 보고서](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) 에서 사용할 수 있습니다.

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

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | string | 정보 데이터 가져오기 하려는 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 가져오려면 [데스크톱 응용 프로그램의 개발자 센터 분석 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504)(예: **상태 보고서**)를 열고 URL에서 제품 ID를 검색합니다. 이 매개 변수를 지정 하지 않으면 경우 응답 본문에는 계정에 등록 된 모든 앱에 대 한 인 사이트 데이터 포함 됩니다.  |  아니요  |
| startDate | date | 검색할 인 사이트 데이터의 날짜 범위에 대 한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | date | 검색할 인 사이트 데이터의 날짜 범위에 대 한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| filter | string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 예를 들어 *필터 dataType eq '취득' =*. <p/><p/>현재이 메서드는 **상태**필터 지원합니다.  | 아니요   |

### <a name="request-example"></a>요청 예제

다음 예제에서는 인 사이트 데이터를 가져오는 요청을 보여 줍니다. 데스크톱 응용 프로그램에 대 한 적절 한 값을 사용 하 여 *응용 프로그램 Id* 값을 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
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
| applicationId       | string | 제품 ID를 검색 한 인 사이트 데이터 데스크톱 응용 프로그램입니다.     |
| insightDate                | string | 특정 메트릭 변경을 식별 하는 날짜입니다. 이 날짜는 크게 증가 감지 하는 주의 끝을 나타내는 또는 그 전에 주에 비해 메트릭이 감소 합니다. |
| 데이터 형식     | string | 이 정보는 일반 분석 영역을 지정 하는 문자열입니다. 현재이 메서드는 **상태**만 지원 합니다.    |
| insightDetail          | array | 하나 이상의 [InsightDetail 값](#insightdetail-values) 현재 통찰력에 대 한 세부 정보를 나타내는 합니다.    |


### <a name="insightdetail-values"></a>InsightDetail 값

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | 현재 통찰력 또는 현재 차원에 설명 하는 메트릭을 나타내는 문자열입니다. 현재이 메서드는 **적중 횟수**값만 지원 합니다.  |
| SubDimensions         | array |  단일 메트릭은 통찰력을 설명 하는 하나 이상의 개체입니다.   |
| PercentChange            | string |  메트릭은 전체 고객 기반에서 변경 된 백분율입니다.  |
| DimensionName           | string |  현재 차원에 설명 된 메트릭의 이름입니다. **EventType**, **시장**, **DeviceType**및 **PackageVersion**을 예로 들 수 있습니다.   |
| DimensionValue              | string | 현재 차원에 설명 된 메트릭의 값입니다. 예를 들어 **DimensionName** **EventType**인 경우에 **크래시** 또는 **중단** **DimensionValue** 수 있습니다.   |
| FactValue     | string | 통찰력은 감지 된 날짜에 메트릭의 절대 값입니다.  |
| 방향 | string |  방향 변경 내용 (**양수** 또는 **음수**)입니다.   |
| Date              | 문자열 |  현재 통찰력 또는 현재 차원 관련 변경을 식별 하는 날짜입니다.   |

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
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
