---
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox Live 멀티플레이 데이터를 가져옵니다.
title: Xbox Live 멀티플레이 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox Live 분석, 멀티플레이
ms.localizationpriority: medium
ms.openlocfilehash: 74f1a64bde32fe68a51527527a0b049d811d0853
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8880530"
---
# <a name="get-xbox-live-multiplayer-data"></a>Xbox Live 멀티플레이 데이터 가져오기


Microsoft Store 분석 API에서 이 메서드를 사용하여 [Xbox Live 지원 게임](../xbox-live/index.md)에 대한 일별 또는 월별 멀티플레이 데이터를 가져옵니다. 이 정보는 파트너 센터에서 [Xbox 분석 보고서](../publish/xbox-analytics-report.md) 에 사용할 수 있습니다.

> [!IMPORTANT]
> 이 방법은 Xbox용 게임 또는 Xbox Live 서비스를 사용하는 게임만 지원합니다. 이러한 게임은 [Microsoft 파트너](../xbox-live/developer-program-overview.md#microsoft-partners)에 의해 게시된 게임 및 [ID@Xbox 프로그램](../xbox-live/developer-program-overview.md#id)을 통해 제출한 게임을 포함하는 [개념 승인 프로세스](../gaming/concept-approval.md)를 거쳐야 합니다. 이 방법은 현재 [Xbox Live 크리에이터스 프로그램](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 통해 게시된 게임을 지원하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수


| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Xbox Live 멀티플레이 데이터를 검색하려는 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정하는 문자열입니다. 이 메서드에 대해 일별 멀티플레이 데이터를 가져오려면 **multiplayerdaily** 값을 지정하고 월별 멀티플레이 데이터를 가져오려면 **multiplayermonthly**를 지정합니다.  |  예  |
| startDate | 날짜 | 검색할 멀티플레이 데이터의 날짜 범위에 대한 시작 날짜입니다. **multiplayerdaily**의 기본값은 현재 날짜 전 3개월입니다. **multiplayermonthly**의 기본값은 현재 날짜 전 1년입니다. |  아니요  |
| endDate | 날짜 | 검색할 멀티플레이 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | string  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul> | 아니요   |
| groupby | string | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul><p/>하나 이상의 *groupby* 필드를 지정하는 경우 응답 본문에서 지정하지 않은 다른 *groupby* 필드의 값은 **All**입니다. |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 지원 게임의 멀티플레이 데이터를 가져오려는 요청에 대해 설명합니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 멀티플레이 데이터가 포함된 개체의 배열입니다. 여기서 각 개체는 지정된 일별 또는 월별 기간 동안 지정된 **filter** 및 **groupby** 값으로 구성된 데이터 세트를 나타냅니다. 각 개체의 데이터에 대한 자세한 내용은 [일별 멀티플레이 분석](#daily-multiplayer-analytics) 및 [월별 멀티플레이 분석](#monthly-multiplayer-analytics) 섹션을 참조하세요.     |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |


### <a name="daily-multiplayer-analytics"></a>일별 멀티플레이 분석

일별 멀티플레이 분석 데이터를 요청할 때(즉, **metricType** 매개 변수에 **multiplayerdaily**를 지정하는 경우) *Value* 배열의 요소에 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 멀티플레이 데이터의 날짜입니다. |
| applicationId       | 문자열 | 멀티플레이 데이터를 검색할 게임의 Store ID입니다.     |
| applicationName       | 문자열 |  멀티플레이 데이터를 검색할 게임의 이름입니다.     |
| market       | 문자열 | 멀티플레이 데이터가 발생한 지역에 대한 2자로 된 ISO 3166 국가 코드입니다.       |
| packageVersion     | 문자열 |  게임에 대한 네 부분으로 된 패키지 버전입니다.  |
| deviceType          | 문자열 | 멀티플레이 데이터가 발생한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>콘솔</strong></li><li><strong>PC</strong></li><li>**알 수 없음**</li></ul>  |
| subscriptionName     | 문자열 |  멀티플레이 데이터에 사용되는 구독 이름입니다. 가능한 값은 **Xbox Game Pass** 및 **""**(구독 없음)입니다.  |
| dailySessionCount     | 숫자 |  지정된 날짜에 게임의 멀티플레이 세션 수입니다.  |
| engagementDurationMinutes     | 숫자 |  지정된 날짜에 고객이 게임의 멀티플레이 세션에 참여한 총 시간(분)입니다.  |
| dailyActiveUsers     | 숫자 |  지정된 날짜에 게임에 참여한 활성 멀티플레이 사용자의 총 수입니다.  |
| dailyActiveDevices     | 숫자 |  지정된 날짜에 게임의 멀티플레이 세션을 플레이한 활성 장치의 총 수입니다.  |
| dailyNewUsers     | 숫자 |  지정된 날짜에 게임에 참여한 새 멀티플레이 사용자의 총 수입니다.  |
| monthlyActiveUsers     | 숫자 |  지정된 날짜가 발생한 달에 참여한 활성 멀티플레이 사용자의 총 수입니다.  |
| monthlyActiveDevices     | 숫자 | 지정된 날짜가 발생한 달에 게임의 멀티플레이 세션을 플레이한 활성 장치의 총 수입니다.   |
| monthlyNewUsers     | 숫자 |  지정된 날짜가 발생한 달에 게임에 참여한 새 멀티플레이 사용자의 총 수입니다.  |


### <a name="monthly-multiplayer-analytics"></a>월별 멀티플레이 분석

월별 멀티플레이 분석 데이터를 요청할 때 *Value* 배열의 요소에 다음 값이 포함됩니다(즉, **metricType** 매개 변수에 **multiplayermonthly**를 지정하는 경우).

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 멀티플레이 데이터에 대한 달의 시작 날짜입니다. |
| applicationId       | 문자열 | 멀티플레이 데이터를 검색할 게임의 Store ID입니다.     |
| applicationName       | 문자열 |  멀티플레이 데이터를 검색할 게임의 이름입니다.     |
| market       | 문자열 | 멀티플레이 데이터가 발생한 지역에 대한 2자로 된 ISO 3166 국가 코드입니다.       |
| packageVersion     | 문자열 |  게임에 대한 네 부분으로 된 패키지 버전입니다.  |
| deviceType          | 문자열 | 멀티플레이 데이터가 발생한 장치 유형을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>콘솔</strong></li><li><strong>PC</strong></li><li>**알 수 없음**</li></ul>  |
| subscriptionName     | 문자열 |  멀티플레이 데이터에 사용되는 구독 이름입니다. 가능한 값은 **Xbox Game Pass** 및 **""**(구독 없음)입니다.  |
| monthlySessionCount     | 숫자 |  지정된 달 동안 게임의 멀티플레이 세션 수입니다.   |
| engagementDurationMinutes     | 숫자 |  지정된 달 동안 고객이 게임의 멀티플레이 세션에 참여한 총 시간(분)입니다.  |
| monthlyActiveUsers     | 숫자 | 지정된 달 동안 게임에 참여한 활성 멀티플레이 사용자의 총 수입니다.   |
| monthlyActiveDevices     | 숫자 | 지정된 달 동안 게임의 멀티플레이 세션을 플레이한 활성 장치의 총 수입니다.   |
| monthlyNewUsers     | 숫자 |  지정된 달 동안 게임에 참여한 새 멀티플레이 사용자의 총 수입니다.  |
| averageDailyActiveUsers     | 숫자 |  지정된 달 동안 게임에 참여한 일별 활성 멀티플레이 사용자의 평균 수입니다.  |
| averageDailyActiveDevices     | 숫자 |  지정된 달 동안 게임의 멀티플레이 세션을 플레이한 활성 장치의 평균 수입니다.  |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청의 일별 메트릭 변형에 대한 JSON 응답 본문 예제를 설명합니다(즉, **metricType** 매개 변수에 **multiplayerdaily**를 지정하는 경우).

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
