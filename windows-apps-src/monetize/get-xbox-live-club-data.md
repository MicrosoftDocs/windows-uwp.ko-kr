---
description: Microsoft Store analytics API에서이 방법을 사용 하 여 Xbox Live 클럽 데이터를 가져옵니다.
title: Xbox Live 클럽 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 분석 API, Xbox Live 분석, 클럽
ms.localizationpriority: medium
ms.openlocfilehash: 155e8a48f9e9e207709af4e55bb8e052b4419867
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162367"
---
# <a name="get-xbox-live-club-data"></a>Xbox Live 클럽 데이터 가져오기

Microsoft Store analytics API에서이 방법을 사용 하 여 [Xbox Live 사용 게임](/gaming/xbox-live/index.md)에 대 한 클럽 데이터를 가져옵니다. 이 정보는 파트너 센터의 [Xbox analytics 보고서](../publish/xbox-analytics-report.md) 에서도 사용할 수 있습니다.

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
| applicationId | 문자열 | Xbox Live 클럽 데이터를 검색 하려는 게임의 [상점 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정 하는 문자열입니다. 이 메서드의 경우 **communitymanagerclub**값을 지정 합니다.  |  예  |
| startDate | date | 검색할 클럽 데이터의 날짜 범위에 있는 시작 날짜입니다. 기본값은 현재 날짜의 30 일입니다. |  예  |
| endDate | date | 검색할 클럽 데이터의 날짜 범위에 있는 종료 날짜입니다. 기본값은 현재 날짜입니다. |  예  |
| top | int | 요청에 반환할 데이터 행 수입니다. 지정 되지 않은 경우 최대값 및 기본값은 1만입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 다음 링크가 포함 됩니다. |  예  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용 하 여 많은 데이터 집합을 페이징 합니다. 예를 들어 top = 10000과 skip = 0은 처음 1만 개의 데이터 행을 검색 하 고 top = 10000 및 skip = 10000은 데이터의 다음 1만 행을 검색 하는 식입니다. |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 사용 게임의 클럽 데이터를 가져오는 요청을 보여 줍니다. *ApplicationId* 값을 게임의 상점 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

| 값      | 형식   | Description                  |
|------------|--------|-------------------------------------------------------|
| 값      | array  | 게임과 관련 된 클럽 데이터를 포함 하는 하나의 [제품 데이터](#productdata) 개체와 모든 Xbox Live 고객에 대 한 클럽 데이터를 포함 하는 하나의 [XboxwideData](#xboxwidedata) 개체를 포함 하는 배열입니다. 이 데이터는 게임의 데이터와 비교 하기 위해 포함 됩니다.  |
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 1만로 설정 되어 있지만 쿼리에 대 한 데이터 행이 1만 개를 초과 하는 경우이 값이 반환 됩니다. |
| TotalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수입니다. |


### <a name="productdata"></a>제품 데이터

이 리소스는 게임에 대 한 클럽 데이터를 포함 합니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
| date            |  문자열 |   클럽 데이터의 날짜입니다.   |
|  applicationId               |    문자열     |  클럽 데이터를 검색 한 게임의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.   |
|  clubsWithTitleActivity               |    int     |  게임과 사회 공학적 된 클럽의 수입니다.   |     
|  clubsExclusiveToGame               |   int      |  게임에 독점적으로 사회 공학적 된 클럽의 수입니다.   |     
|  clubFacts               |   array      |   게임과 사회 공학적 된 각 클럽에 대 한 [ClubFacts](#clubfacts) 개체를 하나 이상 포함 합니다.   |


### <a name="xboxwidedata"></a>XboxwideData

이 리소스는 모든 Xbox Live 고객의 평균 클럽 데이터를 포함 합니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
| date            |  문자열 |   클럽 데이터의 날짜입니다.   |
|  applicationId  |    문자열     |   **XboxwideData** 개체에서이 문자열은 항상 **xboxwide**값입니다.  |
|  clubsWithTitleActivity               |   int     |  평균적으로 Xbox Live 사용 게임과 사회 공학적 된 고객의 클럽 수입니다.    |     
|  clubsExclusiveToGame               |   int      |  평균적으로 Xbox Live 사용 게임과 사회 공학적 된 클럽의 수입니다.   |     
|  clubFacts               |   개체      |  하나의 [ClubFacts](#clubfacts) 개체를 포함 합니다. 이 개체는 **XboxwideData** 개체의 컨텍스트에서 의미가 없으며 기본값이 있습니다.  |


### <a name="clubfacts"></a>ClubFacts

**제품 데이터** 개체에서이 개체에는 게임과 관련 된 활동이 있는 특정 클럽의 데이터가 포함 됩니다. **XboxwideData** 개체에서이 개체는 의미가 없으며 기본값을 갖습니다.

| 값           | 형식    | Description        |
|-----------------|---------|--------------------|
|  name            |  문자열  |   **제품 데이터** 개체에서 클럽의 이름입니다. **XboxwideData** 개체에서이 값은 항상 **xboxwide**값입니다.           |
|  memberCount               |    int     | **제품 데이터** 개체에서 클럽을 방문 하는 비 구성원을 제외 하 고 클럽의 멤버 수입니다. **XboxwideData** 개체에서이는 항상 0입니다.    |
|  titleSocialActionsCount               |    int     |  **제품 데이터** 개체에서 클럽의 구성원이 게임에 관련 된 작업을 수행한 소셜 작업 수입니다. **XboxwideData** 개체에서이는 항상 0입니다.   |
|  isExclusiveToGame               |    부울     |  **제품 데이터** 개체에서 현재 클럽이 게임과만 사회 공학적 여부를 나타냅니다. **XboxwideData** 개체에서이는 항상 true입니다.  |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 멀티 플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)