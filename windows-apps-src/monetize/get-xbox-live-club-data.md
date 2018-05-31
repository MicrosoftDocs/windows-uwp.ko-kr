---
author: mcleanbyron
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox Live 클럽 데이터를 가져옵니다.
title: Xbox Live 클럽 데이터 가져오기
ms.author: mcleans
ms.date: 04/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox Live 분석, 클럽
ms.localizationpriority: medium
ms.openlocfilehash: 09721bf1259c3f44ce2acd5014476aa7e962eaa5
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816688"
---
# <a name="get-xbox-live-club-data"></a>Xbox Live 클럽 데이터 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 [Xbox Live 지원 게임](../xbox-live/index.md)의 클럽 데이터를 가져옵니다. 이 정보는 Windows 개발자 센터 대시보드의 [Xbox 분석 보고서](../publish/xbox-analytics-report.md)를 통해서도 확인할 수 있습니다.

> [!IMPORTANT]
> 이 메서드는 현재 [Microsoft 파트너](../xbox-live/developer-program-overview.md#microsoft-partners)가 게시하고 [ID@Xbox 프로그램](../xbox-live/developer-program-overview.md#id)을 통해 제출되는 Xbox Live 지원 게임만 지원합니다. [Xbox Live 크리에이터스 프로그램](../xbox-live/developer-program-overview.md#xbox-live-creators-program)을 통해 제출된 게임 데이터는 반환되지 않습니다.

## <a name="prerequisites"></a>필수 조건

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


| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | Xbox Live 클럽 데이터를 검색하려는 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정하는 문자열입니다. 이 메서드의 경우 값 **communitymanagerclub**을 지정합니다.  |  예  |
| startDate | 날짜 | 검색할 클럽 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | 날짜 | 검색할 클럽 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 지원 게임의 클럽 데이터를 가져오려는 요청에 대해 설명합니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

| 값      | 유형   | 설명                  |
|------------|--------|-------------------------------------------------------|
| 값      | 배열  | 사용자 게임과 관련된 클럽 데이터가 있는 1개의 [ProductData](#productdata) 개체가 포함되고 모든 Xbox Live 고객의 클럽 데이터가 있는 1개의 [XboxwideData](#xboxwidedata) 개체가 포함된 배열입니다. 이 데이터는 사용자 게임 데이터와 비교하기 위해 포함됩니다.  |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다. |


### <a name="productdata"></a>ProductData

이 리소스에는 사용자 게임의 클럽 데이터가 포함되어 있습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
| date            |  문자열 |   클럽 데이터의 날짜입니다.   |
|  applicationId               |    문자열     |  클럽 데이터를 검색한 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.   |
|  clubsWithTitleActivity               |    int     |  소셜에서 사용자 게임에 참여하는 클럽 수입니다.   |     
|  clubsExclusiveToGame               |   int      |  소셜에서 사용자 게임에만 참여하는 클럽 수입니다.   |     
|  clubFacts               |   배열      |   소셜에서 사용자 게임에 참여하는 각 클럽에 대한 하나 이상의 [ClubFacts](#clubfacts) 개체를 포함합니다.   |


### <a name="xboxwidedata"></a>XboxwideData

이 리소스에는 모든 Xbox Live 고객의 평균 클럽 데이터가 있습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
| date            |  문자열 |   클럽 데이터의 날짜입니다.   |
|  applicationId  |    문자열     |   **XboxwideData** 개체에서 이 문자열은 항상 값 **XBOXWIDE**입니다.  |
|  clubsWithTitleActivity               |   int     |  소셜에서 Xbox Live 지원 게임에 참여하는 고객의 평균 클럽 수입니다.    |     
|  clubsExclusiveToGame               |   int      |  소셜에서 Xbox Live 지원 게임에만 참여하는 평균 클럽 수입니다.   |     
|  clubFacts               |   개체      |  한 [ClubFacts](#clubfacts) 개체를 포함합니다. 이 개체는 **XboxwideData** 개체의 컨텍스트에서는 의미가 없으며 기본값을 가집니다.  |


### <a name="clubfacts"></a>ClubFacts

**ProductData** 개체에서 이 개체에는 사용자 게임과 관련된 활동을 수행하는 특정 클럽의 데이터가 포함됩니다. **XboxwideData** 개체에서 이 개체는 의미가 없으며 기본값을 가집니다.

| 값           | 유형    | 설명        |
|-----------------|---------|--------------------|
|  name            |  문자열  |   **ProductData** 개체에서 이 문자열은 클럽 이름입니다. **XboxwideData** 개체에서 이 문자열은 항상 값 **XBOXWIDE**입니다.           |
|  memberCount               |    int     | **ProductData** 개체에서 이 수는 클럽을 방문하기만 한 구성원이 아닌 사용자를 제외한 클럽의 구성원 수입니다. **XboxwideData** 개체에서 이 수는 항상 0입니다.    |
|  titleSocialActionsCount               |    int     |  **ProductData** 개체에서 이 수는 클럽의 구성원이 사용자 게임과 관련하여 수행한 소셜 작업 수입니다. **XboxwideData** 개체에서 이 수는 항상 0입니다.   |
|  isExclusiveToGame               |    부울     |  **ProductData** 개체에서 이는 소셜에서 현재 클럽이 사용자 게임에만 참여하는지 여부를 표시합니다. **XboxwideData** 개체에서 이는 항상 true입니다.  |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 분석 데이터 가져오기](get-xbox-live-analytics.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
