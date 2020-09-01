---
description: Microsoft Store analytics API에서이 방법을 사용 하 여 Xbox Live 멀티 플레이 데이터를 가져옵니다.
title: Xbox Live 멀티 플레이 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, Xbox Live 분석, 멀티 플레이
ms.localizationpriority: medium
ms.openlocfilehash: 546151e1cf95adc8ecbb64513a66671067869143
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171387"
---
# <a name="get-xbox-live-multiplayer-data"></a>Xbox Live 멀티 플레이 데이터 가져오기


Microsoft Store analytics API에서이 방법을 사용 하 여 매일 또는 매월 [Xbox Live 사용 게임](/gaming/xbox-live/index.md) 을 위한 멀티 플레이 데이터를 가져올 수 있습니다. 이 정보는 파트너 센터의 [Xbox analytics 보고서](../publish/xbox-analytics-report.md) 에서도 사용할 수 있습니다.

> [!IMPORTANT]
> 이 방법은 xbox Live 서비스를 사용 하는 Xbox 또는 게임의 게임만 지원 합니다. 이러한 게임은 [Microsoft 파트너](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) 에서 게시 한 게임과 [ ID@Xbox 프로그램](/gaming/xbox-live/developer-program-overview.md#id)을 통해 제출 된 게임을 포함 하는 [개념 승인 프로세스](../gaming/concept-approval.md)를 통과 해야 합니다. 이 메서드는 현재 [Xbox Live 크리에이터 프로그램](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 통해 게시 된 게임을 지원 하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수


| 매개 변수        | 형식   |  Description      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Xbox Live 멀티 플레이 데이터를 검색 하려는 게임의 [상점 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정 하는 문자열입니다. 이 방법에 대해 **multiplayerdaily** 값을 지정 하 여 일일 여럿이 데이터를 가져오거나 **multiplayermonthly** 를 지정 하 여 월간 멀티 플레이 데이터를 가져옵니다.  |  예  |
| startDate | date | 검색할 멀티 플레이 데이터의 날짜 범위에 있는 시작 날짜입니다. **Multiplayerdaily**의 경우 기본값은 현재 날짜의 3 개월입니다. **Multiplayermonthly**의 경우 기본값은 현재 날짜의 1 년입니다. |  예  |
| endDate | date | 검색할 멀티 플레이 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  예  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  예  |
| filter | 문자열  | 응답의 행을 필터링 하는 하나 이상의 문입니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결 된 응답 본문 및 값의 필드 이름이 포함 되며, **and** 또는 **or**를 사용 하 여 문을 결합할 수 있습니다. 문자열 값은 *필터* 매개 변수에서 작은따옴표로 묶어야 합니다. 응답 본문에서 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>시장</strong></li><li><strong>subscriptionName</strong></li></ul> | 예   |
| groupby | 문자열 | 지정 된 필드에만 데이터 집계를 적용 하는 문입니다. 응답 본문에서 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>시장</strong></li><li><strong>subscriptionName</strong></li></ul><p/>하나 이상의 *groupby* 필드를 지정 하는 경우 지정 하지 않은 다른 모든 *groupby* 필드에는 **모두** 값이 응답 본문에 포함 됩니다. |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 사용 게임을 위한 멀티 플레이 데이터를 가져오기 위한 요청을 보여 줍니다. *ApplicationId* 값을 게임의 상점 ID로 바꿉니다.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

| 값      | 형식   | Description                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 여럿이 가능 데이터를 포함 하는 개체의 배열입니다. 각 개체는 지정 된 일별 또는 월별 기간의 데이터 집합을 나타내고 지정 된 **필터** 및 **groupby** 값을 기준으로 구성 됩니다. 각 개체의 데이터에 대 한 자세한 내용은 [일일 다중 플레이어 분석](#daily-multiplayer-analytics) 및 [월별 멀티 플레이 분석](#monthly-multiplayer-analytics) 섹션을 참조 하세요.     |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대 한 데이터 행이 1만 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다. |


### <a name="daily-multiplayer-analytics"></a>일일 다중 플레이어 분석

**MetricType** 매개 변수에 대해 **multiplayerdaily** 를 지정 하는 경우, *값* 배열의 요소에는 일일 멀티 플레이 분석 데이터를 요청할 때 다음 값이 포함 됩니다.

| 값               | 형식   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 멀티 플레이 데이터의 날짜입니다. |
| applicationId       | 문자열 | 멀티 플레이 데이터를 검색 하는 게임의 저장소 ID입니다.     |
| applicationName       | 문자열 |  멀티 플레이 데이터를 검색 하는 게임의 이름입니다.     |
| market       | 문자열 | 멀티 플레이어 데이터를 가져온 시장에서 두 문자로 된 ISO 3166 국가 코드입니다.       |
| packageVersion     | 문자열 |  게임의 네 부분으로 구성 된 패키지 버전입니다.  |
| deviceType          | 문자열 | 멀티 플레이어 데이터를 가져온 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>콘솔</strong></li><li><strong>컴퓨터</strong></li><li>**알 수 없음**</li></ul>  |
| subscriptionName     | 문자열 |  멀티 플레이 데이터에 사용 되는 구독의 이름입니다. 가능한 값에는 **Xbox Game Pass** 및 **""** (구독 없음)이 포함 됩니다.  |
| dailySessionCount     | number |  지정 된 날짜에서 게임에 대 한 멀티 플레이 세션의 수입니다.  |
| engagementDurationMinutes     | number |  고객이 지정 된 날짜에서 게임에 대 한 멀티 플레이 세션에 참여 한 총 시간 (분)입니다.  |
| dailyActiveUsers     | number |  지정 된 날짜에서 게임에 대 한 활성 여럿이 사용자의 총 수입니다.  |
| dailyActiveDevices     | number |  지정 된 날짜에서 게임에 대 한 멀티 플레이 세션을 재생 한 활성 장치의 총 수입니다.  |
| dailyNewUsers     | number |  지정 된 날짜에서 게임에 대 한 새 멀티 플레이어 사용자의 총 수입니다.  |
| monthlyActiveUsers     | number |  지정 된 날짜가 발생 한 달의 총 활성 여럿이 사용자 수입니다.  |
| monthlyActiveDevices     | number | 지정 된 날짜가 발생 한 달 동안 게임에 대 한 멀티 플레이 세션을 재생 한 활성 장치의 총 수입니다.   |
| monthlyNewUsers     | number |  지정 된 날짜가 발생 한 달의 게임에 대 한 새 멀티 플레이어 사용자의 총 수입니다.  |


### <a name="monthly-multiplayer-analytics"></a>월간 멀티 플레이 분석

**MetricType** 매개 변수에 대해 **multiplayermonthly** 를 지정 하는 경우, *값* 배열의 요소에는 월간 멀티 플레이 분석 데이터를 요청할 때 다음 값이 포함 됩니다.

| 값               | 형식   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | 문자열 | 멀티 플레이 데이터를 위한 월의 첫 번째 날짜입니다. |
| applicationId       | 문자열 | 멀티 플레이 데이터를 검색 하는 게임의 저장소 ID입니다.     |
| applicationName       | 문자열 |  멀티 플레이 데이터를 검색 하는 게임의 이름입니다.     |
| market       | 문자열 | 멀티 플레이어 데이터를 가져온 시장에서 두 문자로 된 ISO 3166 국가 코드입니다.       |
| packageVersion     | 문자열 |  게임의 네 부분으로 구성 된 패키지 버전입니다.  |
| deviceType          | 문자열 | 멀티 플레이어 데이터를 가져온 장치 유형을 지정 하는 다음 문자열 중 하나입니다.<p/><ul><li><strong>콘솔</strong></li><li><strong>컴퓨터</strong></li><li>**알 수 없음**</li></ul>  |
| subscriptionName     | 문자열 |  멀티 플레이 데이터에 사용 되는 구독의 이름입니다. 가능한 값에는 **Xbox Game Pass** 및 **""** (구독 없음)이 포함 됩니다.  |
| monthlySessionCount     | number |  지정 된 달에 발생 한 게임의 멀티 플레이 세션 수입니다.   |
| engagementDurationMinutes     | number |  고객이 지정 된 월 동안 게임에 대 한 멀티 플레이 세션을 사용한 총 시간 (분)입니다.  |
| monthlyActiveUsers     | number | 지정 된 달에 활성화 된 총 사용자 수입니다.   |
| monthlyActiveDevices     | number | 지정 된 달 동안 게임에 대 한 멀티 플레이 세션을 재생 한 활성 장치의 총 수입니다.   |
| monthlyNewUsers     | number |  지정 된 달 동안 게임에 대 한 새 멀티 플레이어 사용자의 총 수입니다.  |
| averageDailyActiveUsers     | number |  지정 된 월에 게임의 평균 일별 활성 여럿이 사용자 수입니다.  |
| averageDailyActiveDevices     | number |  지정 된 월에 게임의 멀티 플레이 세션을 재생 한 평균 활성 장치 수입니다.  |


### <a name="response-example"></a>응답 예제

다음 예에서는 **metricType** 매개 변수에 대해 **multiplayerdaily** 를 지정 하는 경우이 요청의 일별 메트릭 변형에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)