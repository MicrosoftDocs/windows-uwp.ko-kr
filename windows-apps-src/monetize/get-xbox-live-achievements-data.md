---
author: Xansky
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox Live 도전 과제 데이터를 가져옵니다.
title: Xbox Live 도전 과제 데이터 가져오기
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox Live 분석, 도전 과제
ms.localizationpriority: medium
ms.openlocfilehash: 6b635a659a8516184998b5f0b05d2d7692a42af1
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5979556"
---
# <a name="get-xbox-live-achievements-data"></a>Xbox Live 도전 과제 데이터 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 도전 과제 데이터가 사용 가능한 최근 날짜, 해당 날짜 이전 30일 및 해당 날짜까지 게임의 총 수명 동안 [Xbox Live 지원 게임](../xbox-live/index.md)에 대한 각 도전 과제를 잠금 해제한 고객 수를 가져옵니다. 이 정보는 파트너 센터에서 [Xbox 분석 보고서](../publish/xbox-analytics-report.md) 에 사용할 수 있습니다.

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
| applicationId | 문자열 | Xbox Live 도전 과제 데이터를 검색하려는 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정하는 문자열입니다. 이 메서드의 경우 값 **achievements**를 지정합니다.  |  예  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 지원 게임에 대한 처음 10개의 도전 과제를 잠금 해제한 고객의 도전 과제 데이터를 가져오라는 요청에 대해 설명합니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 게임의 각 도전 과제 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요.                                                                                                                      |
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 100으로 설정되어 있지만 쿼리에 대한 데이터의 행이 100개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.  |


*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 문자열 | 도전 과제 데이터를 검색할 게임의 Store ID입니다.     |
| reportDateTime     | 문자열 |  도전 과제 데이터의 날짜입니다.    |
| achievementId          | 숫자 |  도전 과제의 ID입니다. |
| achievementName           | 문자열 | 도전 과제의 이름입니다.  |
| gamerscore           | 숫자 |  도전 과제에 대한 게이머 점수 보상입니다.  |
| dailyUnlocks           | 숫자 |  *reportDateTime*으로 지정된 날짜에 도전 과제를 잠금 해제한 고객 수입니다.  |
| monthlyUnlocks              | 숫자 |  *reportDateTime*으로 지정된 날짜 이전의 30일 동안 도전 과제를 잠금 해제한 고객 수입니다.   |
| totalUnlocks | 숫자 |  최대 *reportDateTime*으로 지정된 날짜까지인 게임 수명 동안 도전 과제를 잠금 해제한 고객 수입니다.   |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
