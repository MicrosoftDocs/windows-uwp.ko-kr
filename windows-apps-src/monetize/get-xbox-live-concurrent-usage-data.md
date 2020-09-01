---
description: Microsoft Store analytics API에서이 방법을 사용 하 여 Xbox Live 동시 사용 데이터를 가져옵니다.
title: Xbox Live 동시 사용량 현황 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, Xbox Live 분석, 동시 사용
ms.localizationpriority: medium
ms.openlocfilehash: 9e8d36ce9336d5fc65a73a19233b8a1f0a05a8bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167587"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Xbox Live 동시 사용량 현황 데이터 가져오기


Microsoft Store analytics API에서이 메서드를 사용 하 여 지정 된 시간 범위 동안 분, 시간 또는 일 마다 [Xbox Live 사용 가능 게임](/gaming/xbox-live/index.md) 을 재생 하는 평균 고객 수에 대 한 실시간 사용 데이터 (5-15 분 대기 시간)를 거의 실시간으로 가져올 수 있습니다. 이 정보는 파트너 센터의 [Xbox analytics 보고서](../publish/xbox-analytics-report.md) 에서도 사용할 수 있습니다.

> [!IMPORTANT]
> 이 방법은 xbox Live 서비스를 사용 하는 Xbox 또는 게임의 게임만 지원 합니다. 이러한 게임은 [Microsoft 파트너](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) 에서 게시 한 게임과 [ ID@Xbox 프로그램](/gaming/xbox-live/developer-program-overview.md#id)을 통해 제출 된 게임을 포함 하는 [개념 승인 프로세스](../gaming/concept-approval.md)를 통과 해야 합니다. 이 메서드는 현재 [Xbox Live 크리에이터 프로그램](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 통해 게시 된 게임을 지원 하지 않습니다.

## <a name="prerequisites"></a>필수 조건

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수


| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Xbox Live 동시 사용 데이터를 검색 하려는 게임의 [상점 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정 하는 문자열입니다. 이 메서드의 경우 값 **동시성**을 지정 합니다.  |  예  |
| startDate | date | 동시 사용 데이터를 검색할 날짜 범위의 시작 날짜입니다. 기본 동작에 대 한 *aggregationLevel* 설명을 참조 하세요. |  아니요  |
| endDate | date | 검색할 동시 사용 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본 동작에 대 한 *aggregationLevel* 설명을 참조 하세요. |  아니요  |
| aggregationLevel | 문자열 | 집계 데이터를 검색 하는 시간 범위를 지정 합니다. 다음 문자열 중 하나일 수 있습니다. **분**, **시간**또는 **일** 지정 하지 않으면 기본값은 **day**입니다. <p/><p/>날짜/ *endDate*을 지정 *startDate* 하지 않으면 응답 본문은 다음과 같이 설정 됩니다. <ul><li>**minute**: 사용 가능한 데이터의 마지막 60 레코드입니다.</li><li>**hour**: 사용 가능한 데이터의 마지막 24 레코드입니다.</li><li>**day**: 사용 가능한 데이터의 마지막 7 레코드입니다.</li></ul><p/>다음 집계 수준에는 반환 될 수 있는 레코드 수에 대 한 크기 제한이 있습니다. 요청한 시간 범위가 너무 크면 레코드가 잘립니다. <ul><li>**분**: 최대 1440 레코드 (24 시간 데이터)</li><li>**hour**: 최대 720 레코드 (데이터 30 일)</li><li>**일**: 최대 60 레코드 (60 일 데이터)</li></ul>  |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 사용 게임의 동시 사용 현황 데이터를 가져오는 요청을 보여 줍니다. 이 요청은 1 2018 년 2 월 사이 2 2018에서 1 분 마다 데이터를 검색 합니다. *ApplicationId* 값을 게임의 상점 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

응답 본문에는 지정 된 분, 시간 또는 일에 대 한 동시 사용 현황 데이터 집합이 각각 하나씩 포함 된 개체 배열이 포함 되어 있습니다. 각 개체에는 다음 값이 포함 됩니다.

| 값      | 형식   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 개수      | number  | Xbox Live를 사용 하 여 지정 된 분, 시간 또는 일 동안 Xbox Live를 재생 하는 평균 고객 수입니다. <p/><p/>**Note** &nbsp; 참고 &nbsp; 값이 0 이면 지정 된 간격 동안 동시 사용자가 없거나 지정 된 간격 동안 게임에 대 한 동시 사용자 데이터를 수집 하는 동안 오류가 발생 했음을 나타냅니다. |
| 날짜  | 문자열 | 동시 사용 데이터가 발생 한 분, 시간 또는 일을 지정 하는 날짜와 시간입니다.  |
| SeriesName | 문자열    | 이 값에는 항상 **Userconcurrency**값이 있습니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는 데이터 집계가 분당이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티 플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)