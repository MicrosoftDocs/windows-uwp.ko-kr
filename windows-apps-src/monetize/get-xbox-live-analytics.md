---
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 Xbox Live 분석 데이터를 가져옵니다.
title: Xbox Live 분석 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, Xbox Live 분석
ms.localizationpriority: medium
ms.openlocfilehash: 74c898630641e8b0d53a181d1874c6df62baaa78
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485485"
---
# <a name="get-xbox-live-analytics-data"></a>Xbox Live 분석 데이터 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 [Xbox Live 지원 게임](../xbox-live/index.md)을 플레이하는 고객의 지난 30일 동안의 일반 분석 데이터를 가져옵니다. 여기에는 장치 액세서리 사용량, 인터넷 연결 유형, 게이머 점수 분포, 게임 통계 및 친구와 팔로워 데이터가 포함되어 있습니다. 이 정보는 파트너 센터에서 [Xbox 분석 보고서](../publish/xbox-analytics-report.md) 에 사용할 수 있습니다.

> [!IMPORTANT]
> 이 방법은 Xbox용 게임 또는 Xbox Live 서비스를 사용하는 게임만 지원합니다. 이러한 게임은 [Microsoft 파트너](../xbox-live/developer-program-overview.md#microsoft-partners)에 의해 게시된 게임 및 [ID@Xbox 프로그램](../xbox-live/developer-program-overview.md#id)을 통해 제출한 게임을 포함하는 [개념 승인 프로세스](../gaming/concept-approval.md)를 거쳐야 합니다. 이 방법은 현재 [Xbox Live 크리에이터스 프로그램](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 통해 게시된 게임을 지원하지 않습니다.

Xbox Live 지원 게임은 다음 방법을 통해 사용할 수 있습니다.
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
* [Xbox Live 동시 사용 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)

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

| 매개 변수        | 형식   |  설명      |  필수  
|---------------|--------|---------------|------|
| applicationId | 문자열 | 일반 Xbox Live 분석 데이터를 검색하려는 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정하는 문자열입니다. 이 메서드의 경우 값 **productvalues**를 지정합니다.  |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 지원 게임을 플레이하는 고객의 일반 분석 데이터를 가져오려는 요청에 대해 설명합니다. *applicationId* 값을 게임의 Store ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 다음 개체가 포함된 *값* 배열을 반환합니다.

| 개체      | 설명                  |
|-------------|---------------------------------------------------|
| ProductData   |   지난 30일 동안 게임의 장치 및 사용자 분석 데이터가 있는 1개의 [DeviceProperties](#deviceproperties) 개체와 1개의 [UserProperties](#userproperties) 개체를 포함합니다.    |  
| XboxwideData   |  지난 30일 동안 모든 Xbox Live 고객의 평균 장치 및 사용자 분석 데이터(백분율)가 있는 1개의 [DeviceProperties](#deviceproperties) 개체와 1개의 [UserProperties](#userproperties) 개체를 포함합니다. 이 데이터는 사용자 게임 데이터와 비교하기 위해 포함됩니다.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

이 리소스에는 지난 30일 동안 게임의 장치 사용량 데이터 또는 모든 Xbox Live 고객의 평균 장치 사용량 데이터가 포함되어 있습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
|  applicationId               |    문자열     |  분석 데이터를 검색한 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.   |
|  connectionTypeDistribution               |    배열     |   Xbox에서 유선 인터넷 연결을 사용하는 고객 대 무선 인터넷 연결을 사용하는 고객 수를 표시하는 개체가 포함됩니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**conType**: 연결 유형을 지정합니다.</li><li>**deviceCount**: **ProductData** 개체에서 이 필드는 해당 연결 유형을 사용하는 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 해당 연결 유형을 사용하는 모든 Xbox Live 고객의 백분율을 지정합니다.</li></ul>   |     
|  deviceCount               |   문자열      |  **ProductData** 개체에서 이 필드는 지난 30일 동안 게임이 플레이된 고객 장치 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 항상 1이며, 이는 모든 Xbox Live 고객의 데이터에 대해 100% 시작 비율을 나타냅니다.   |     
|  eliteControllerPresentDeviceCount               |   문자열      |  **ProductData** 개체에서 이 필드는 Xbox Elite 무선 컨트롤러를 사용하는 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 Xbox Elite 무선 컨트롤러를 사용하는 모든 Xbox Live 고객의 백분율을 지정합니다.  |     
|  externalDrivePresentDeviceCount               |   문자열      |  **ProductData** 개체에서 이 필드는 Xbox의 외장 하드 드라이브를 사용하는 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 Xbox의 외장 하드 드라이브를 사용하는 모든 Xbox Live 고객의 백분율을 지정합니다.  |


### <a name="userproperties"></a>UserProperties

이 리소스에는 지난 30일 동안 게임의 장치 사용자 데이터 또는 모든 Xbox Live 고객의 평균 사용자 데이터가 포함되어 있습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
|  applicationId               |    문자열     |   분석 데이터를 검색한 게임의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다.  |
|  userCount               |    문자열     |   **ProductData** 개체에서 이 필드는 지난 30일 동안 게임을 플레이한 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 항상 1이며, 이는 모든 Xbox Live 고객의 데이터에 대해 100% 시작 비율을 나타냅니다.   |     
|  dvrUsageCounts               |   배열      |  게임 플레이를 기록하고 보기 위해 게임 DVR을 사용한 고객 수를 표시하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**dvrName**: 사용되는 게임 DVR 기능을 지정합니다. 가능한 값은 **gameClipUploads**, **gameClipViews**, **screenshotUploads** 및 **screenshotViews**입니다.</li><li>**userCount**: **ProductData** 개체에서 이 필드는 지정된 게임 DVR 기능을 사용한 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 지정된 게임 DVR 기능을 사용한 모든 Xbox Live 고객의 백분율을 지정합니다.</li></ul>   |     
|  followerCountPercentiles               |   배열      |  고객의 팔로워 수에 대한 세부 정보를 제공하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**percentage**: 현재 이 값은 항상 50이며, 이는 팔로워 데이터가 중간 값으로 제공됨을 나타냅니다.</li><li>**value**: **ProductData** 개체에서 이 필드는 게임 고객의 팔로워 수를 중간 값으로 지정합니다. **XboxwideData** 개체에서 이 필드는 모든 Xbox Live 고객의 팔로워 수를 중간 값으로 지정합니다.</li></ul>  |   
|  friendCountPercentiles               |   배열      |  고객의 친구 수에 대한 세부 정보를 제공하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**percentage**: 현재 이 값은 항상 50이며, 이는 친구 데이터가 중간 값으로 제공됨을 나타냅니다.</li><li>**value**: **ProductData** 개체에서 이 필드는 게임 고객의 친구 수를 중간 값으로 지정합니다. **XboxwideData** 개체에서 이 필드는 모든 Xbox Live 고객의 친구 수를 중간 값으로 지정합니다.</li></ul>  |     
|  gamerScoreRangeDistribution               |   배열      |  고객의 게이머 점수 분포에 대한 세부 정보를 제공하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**scoreRange**: 다음 필드에서 사용량 데이터를 제공하는 게이머 점수 범위입니다. 예를 들어 **10K-25K**입니다.</li><li>**userCount**: **ProductData** 개체에서 이 필드는 고객이 플레이한 모든 게임에 대해 게이머 점수가 지정된 범위에 포함되는 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 고객이 플레이한 모든 게임에 대해 게이머 점수가 지정된 범위에 포함되는 모든 Xbox Live 고객의 백분율을 지정합니다.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   배열      |  게임의 게이머 점수 분포에 대한 세부 정보를 제공하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**scoreRange**: 다음 필드에서 사용량 데이터를 제공하는 게이머 점수 범위입니다. 예를 들어 **100-200**입니다.</li><li>**userCount**: **ProductData** 개체에서 이 필드는 사용자 게임에 대해 게이머 점수가 지정된 범위에 포함되는 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 사용자 게임에 대해 게이머 점수가 지정된 범위에 포함되는 모든 Xbox Live 고객의 백분율을 지정합니다.</li></ul>   |
|  socialUsageCounts               |   배열      |  고객의 소셜 사용에 대한 세부 정보를 제공하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**scName**: 소셜 사용 유형입니다. 예를 들어 **gameInvites** 및 **textMessages**입니다.</li><li>**userCount**: **ProductData** 개체에서 이 필드는 지정된 소셜 사용 유형에 참여한 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 지정된 소셜 사용 유형에 참여한 모든 Xbox Live 고객의 백분율을 지정합니다.</li></ul>   |
|  streamingUsageCounts               |   배열      |  고객의 스트리밍 사용에 대한 세부 정보를 제공하는 개체가 포함되어 있습니다. 각 개체에는 다음 두 개의 필드가 있습니다. <ul><li>**stName**: 스트리밍 플랫폼 유형입니다. 예를 들어 **youtubeUsage**, **twitchUsage** 및 **mixerUsage**입니다.</li><li>**userCount**: **ProductData** 개체에서 이 필드는 지정된 스트리밍 플랫폼을 사용한 게임 고객 수를 지정합니다. **XboxwideData** 개체에서 이 필드는 지정된 스트리밍 플랫폼을 사용한 모든 Xbox Live 고객의 백분율을 지정합니다.</li></ul>  |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
* [Xbox Live 동시 사용 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)
