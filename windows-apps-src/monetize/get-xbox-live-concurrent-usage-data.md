---
author: Xansky
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox Live 동시 사용 데이터를 가져옵니다.
title: Xbox Live 동시 사용 데이터 가져오기
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox Live 분석, 동시 사용
ms.localizationpriority: medium
ms.openlocfilehash: 4506176dcd62a4699c57343c50dfc1b0e7a40b14
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6155211"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Xbox Live 동시 사용 데이터 가져오기


Microsoft Store 분석 API에서 이 메서드를 사용하여 지정된 시간 범위 동안 매분, 매시간, 매일 [Xbox Live 지원 게임](../xbox-live/index.md)을 플레이하는 평균 고객 수에 대한 근 실시간 사용 데이터(5~15분의 대기 시간이 있음)를 가져옵니다. 이 정보는 파트너 센터에서 [Xbox 분석 보고서](../publish/xbox-analytics-report.md) 에 사용할 수 있습니다.

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
| applicationId | 문자열 | Xbox Live 동시 사용 데이터를 검색하려는 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정하는 문자열입니다. 이 메서드의 경우 값 **concurrency**를 지정합니다.  |  예  |
| startDate | 날짜 | 검색할 동시 사용 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본 동작에 대해서는 *aggregationLevel* 설명을 참조하세요. |  아니요  |
| endDate | 날짜 | 검색할 동시 사용 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본 동작에 대해서는 *aggregationLevel* 설명을 참조하세요. |  아니요  |
| aggregationLevel | string | 집계 데이터를 검색할 시간 범위를 지정합니다. **minute**, **hour** 또는 **day** 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 **day**입니다. <p/><p/>*startDate* 또는 *endDate*를 지정하지 않을 경우 기본 응답 본문은 다음과 같습니다. <ul><li>**minute**: 사용 가능한 데이터의 마지막 60개 레코드.</li><li>**hour**: 사용 가능한 데이터의 마지막 24개 레코드.</li><li>**day**: 사용 가능한 데이터의 마지막 7개 레코드.</li></ul><p/>다음 집계 수준에는 반환할 수 있는 레코드 수에 대한 크기 제한이 있습니다. 요청된 시간 범위가 너무 큰 경우 레코드가 잘립니다. <ul><li>**minute**: 최대 1440개 레코드(24 시간 데이터).</li><li>**hour**: 최대 720개 레코드(30일 데이터).</li><li>**day**: 최대 60개 레코드(60일 데이터).</li></ul>  |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 지원 게임의 동시 사용 데이터를 가져오려는 요청에 대해 설명합니다. 이 요청은 2018년 2월 1일부터 2018년 2월 2일 사이에 데이터를 1분마다 검색합니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

응답 본문에는 지정된 분, 시간 또는 일에 대한 하나의 동시 사용 데이터 세트가 각각의 개체에 포함되는 개체 배열이 있습니다. 각 개체에는 다음 값이 포함됩니다.

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| Count      | 숫자  | 지정된 분, 시간 또는 날 동안 Xbox Live 지원 게임을 플레이하는 평균 고객 수입니다. <p/><p/>**참고**&nbsp;&nbsp;값 0은 지정된 기간 동안 동시 사용자가 없었거나 지정된 기간 동안 게임의 동시 사용자 데이터를 수집하는 중에 오류가 발생했음을 나타냅니다. |
| Date  | 문자열 | 동시 사용 데이터가 발생한 분, 시간 또는 일을 지정하는 날짜 및 시간입니다.  |
| SeriesName | 문자열    | 이 값은 항상 **UserConcurrency**입니다. |


### <a name="response-example"></a>응답 예제

다음 예제에서는 분당 데이터 집계가 포함된 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
