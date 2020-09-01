---
description: Microsoft Store analytics API에서이 방법을 사용 하 여 Xbox Live 분석 데이터를 가져옵니다.
title: Xbox Live 분석 데이터 가져오기
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store analytics API, Xbox Live 분석
ms.localizationpriority: medium
ms.openlocfilehash: 5649a81e1c6e869e9e010841cb31432350bb33d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172887"
---
# <a name="get-xbox-live-analytics-data"></a>Xbox Live 분석 데이터 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 장치 액세서리 사용, 인터넷 연결 유형, gamerscore 배포, 게임 통계, 친구 및 팔로 워 데이터를 포함 하 여 [Xbox Live 사용 게임](/gaming/xbox-live/index.md)을 실행 하는 고객을 위해 최근 30 일간의 일반 분석 데이터를 가져옵니다. 이 정보는 파트너 센터의 [Xbox analytics 보고서](../publish/xbox-analytics-report.md) 에서도 사용할 수 있습니다.

> [!IMPORTANT]
> 이 방법은 xbox Live 서비스를 사용 하는 Xbox 또는 게임의 게임만 지원 합니다. 이러한 게임은 [Microsoft 파트너](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) 에서 게시 한 게임과 [ ID@Xbox 프로그램](/gaming/xbox-live/developer-program-overview.md#id)을 통해 제출 된 게임을 포함 하는 [개념 승인 프로세스](../gaming/concept-approval.md)를 통과 해야 합니다. 이 메서드는 현재 [Xbox Live 크리에이터 프로그램](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 통해 게시 된 게임을 지원 하지 않습니다.

Xbox Live 사용 게임의 추가 분석 데이터는 다음 방법을 통해 사용할 수 있습니다.
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티 플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
* [Xbox Live 동시 사용량 현황 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)

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
| applicationId | 문자열 | 일반 Xbox Live 분석 데이터를 검색 하려는 게임의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |  예  |
| metricType | 문자열 | 검색할 Xbox Live 분석 데이터의 유형을 지정 하는 문자열입니다. 이 메서드의 경우 값으로 **제품 값**을 지정 합니다.  |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 Xbox Live 사용 게임을 실행 하는 고객에 대 한 일반 분석 데이터를 얻기 위한 요청을 보여 줍니다. *ApplicationId* 값을 게임의 상점 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 다음 개체를 포함 하는 *값* 배열을 반환 합니다.

| 개체      | Description                  |
|-------------|---------------------------------------------------|
| 제품 데이터   |   에는 한 개의 [Deviceproperties](#deviceproperties) 개체와 게임의 사용자 분석 데이터와 지난 30 일간의 장치를 포함 하는 하나의 [UserProperties](#userproperties) 개체가 포함 되어 있습니다.    |  
| XboxwideData   |  모든 Xbox Live 고객의 평균 장치 및 사용자 분석 데이터를 백분율로 포함 하는 하나의 [Deviceproperties](#deviceproperties) 개체와 하나의 [UserProperties](#userproperties) 개체를 포함 합니다. 이 데이터는 게임의 데이터와 비교 하기 위해 포함 됩니다.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

이 리소스에는 지난 30 일 동안 모든 Xbox Live 고객의 평균 장치 사용 데이터 또는 게임의 장치 사용 데이터가 포함 되어 있습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
|  applicationId               |    문자열     |  분석 데이터를 검색 한 게임의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.   |
|  connectionTypeDistribution               |    array     |   Xbox에서 유선 인터넷 연결 및 무선 인터넷 연결을 사용 하는 고객 수를 나타내는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>연결 **형식: 연결**유형을 지정 합니다.</li><li>**Devicecount**: **제품 데이터** 개체에서이 필드는 연결 형식을 사용 하는 게임 고객의 번호를 지정 합니다. **XboxwideData** 개체에서이 필드는 연결 형식을 사용 하는 모든 Xbox Live 고객의 비율을 지정 합니다.</li></ul>   |     
|  deviceCount               |   문자열      |  **제품 데이터** 개체에서이 필드는 지난 30 일 동안 게임이 재생 된 고객 장치의 수를 지정 합니다. **XboxwideData** 개체에서이 필드는 항상 1 이며 모든 Xbox Live 고객의 데이터에 대해 100%의 시작 비율을 나타냅니다.   |     
|  eliteControllerPresentDeviceCount               |   문자열      |  **제품 데이터** 개체에서이 필드는 Xbox 정예 무선 컨트롤러를 사용 하는 게임 고객의 번호를 지정 합니다. **XboxwideData** 개체에서이 필드는 Xbox 정예 무선 컨트롤러를 사용 하는 모든 xbox Live 고객의 비율을 지정 합니다.  |     
|  externalDrivePresentDeviceCount               |   문자열      |  **제품 데이터** 개체에서이 필드는 Xbox에서 외장 하드 드라이브를 사용 하는 게임 고객의 번호를 지정 합니다. **XboxwideData** 개체에서이 필드는 xbox에서 외부 하드 드라이브를 사용 하는 모든 xbox Live 고객의 비율을 지정 합니다.  |


### <a name="userproperties"></a>UserProperties

이 리소스에는 지난 30 일 동안 모든 Xbox Live 고객에 대 한 사용자 데이터 또는 사용자의 평균 사용자 데이터가 포함 됩니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
|  applicationId               |    문자열     |   분석 데이터를 검색 한 게임의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다.  |
|  userCount               |    문자열     |   **제품 데이터** 개체에서이 필드는 지난 30 일 동안 게임을 재생 한 고객 수를 지정 합니다. **XboxwideData** 개체에서이 필드는 항상 1 이며 모든 Xbox Live 고객의 데이터에 대해 100%의 시작 비율을 나타냅니다.   |     
|  dvrUsageCounts               |   array      |  게임 DVR을 사용 하 여 게임을 기록 하 고 볼 수 있는 고객 수를 나타내는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**dvrName**: 사용 되는 게임 DVR 기능을 지정 합니다. 가능한 값은 **gameClipUploads**, **gameClipViews**, **ScreenshotUploads**및 **screenshotviews**입니다.</li><li>**userCount**: **제품 데이터** 개체에서이 필드는 지정 된 게임 DVR 기능을 사용한 게임 고객의 번호를 지정 합니다. **XboxwideData** 개체에서이 필드는 지정 된 게임 DVR 기능을 사용한 모든 Xbox Live 고객의 비율을 지정 합니다.</li></ul>   |     
|  followerCountPercentiles               |   array      |  고객의 팔로 워 수에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**백분율**: 현재이 값은 종동체 데이터가 중앙값으로 제공 됨을 나타내는 항상 50입니다.</li><li>**값**: **제품 데이터** 개체에서이 필드는 게임 고객에 대 한 최대 팔로 워 수를 지정 합니다. **XboxwideData** 개체에서이 필드는 모든 Xbox Live 고객에 대 한 중앙값의 수를 지정 합니다.</li></ul>  |   
|  friendCountPercentiles               |   array      |  고객의 친구 수에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**백분율**: 현재이 값은 항상 50입니다 .이 값은 friend 데이터가 중앙값으로 제공 됨을 나타냅니다.</li><li>**값**: **제품 데이터** 개체에서이 필드는 게임 고객의 중간 친구 수를 지정 합니다. **XboxwideData** 개체에서이 필드는 모든 Xbox Live 고객에 대 한 중간의 친구 수를 지정 합니다.</li></ul>  |     
|  gamerScoreRangeDistribution               |   array      |  고객에 대 한 gamerscore 배포에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**scoreRange**: 다음 필드에서 사용 데이터를 제공 하는 gamerscore 범위입니다. 예: **10k-25,000**.</li><li>**userCount**: **제품 데이터** 개체에서이 필드는 지정 된 범위에서 재생 한 모든 게임에 대해 gamerscore이 있는 게임 고객의 수를 지정 합니다. **XboxwideData** 개체에서이 필드는 모든 Xbox Live 고객의 비율을 지정 된 범위 내에서 재생 한 모든 게임의 비율을 지정 합니다.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   array      |  게임의 gamerscore 배포에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**scoreRange**: 다음 필드에서 사용 데이터를 제공 하는 gamerscore 범위입니다. 예를 들면 **100-200**입니다.</li><li>**userCount**: **제품 데이터** 개체에서이 필드는 게임의 지정 된 범위에 gamerscore이 있는 게임 고객의 수를 지정 합니다. **XboxwideData** 개체에서이 필드는 게임의 지정 된 범위에 gamerscore이 있는 모든 Xbox Live 고객의 비율을 지정 합니다.</li></ul>   |
|  socialUsageCounts               |   array      |  고객의 소셜 사용에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**Scname**: 소셜 사용의 유형입니다. 예를 들면 **gameInvites** 및 **textmessages**가 있습니다.</li><li>**userCount**: **제품 데이터** 개체에서이 필드는 지정 된 소셜 사용 유형에 참여 한 게임 고객의 번호를 지정 합니다. **XboxwideData** 개체에서이 필드는 지정 된 소셜 사용 유형에 참여 한 모든 Xbox Live 고객의 비율을 지정 합니다.</li></ul>   |
|  streamingUsageCounts               |   array      |  고객의 스트리밍 사용에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 각 개체에는 두 개의 문자열 필드가 있습니다. <ul><li>**stName**: 스트리밍 플랫폼의 유형입니다. 예를 들어, **twitchusage**, **mixerUsage** **등이 있습니다**.</li><li>**userCount**: **제품 데이터** 개체에서이 필드는 지정 된 스트리밍 플랫폼을 사용한 게임 고객의 번호를 지정 합니다. **XboxwideData** 개체에서이 필드는 지정 된 스트리밍 플랫폼을 사용한 모든 Xbox Live 고객의 비율을 지정 합니다.</li></ul>  |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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

* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox Live 도전 과제 데이터 가져오기](get-xbox-live-achievements-data.md)
* [Xbox Live 상태 데이터 가져오기](get-xbox-live-health-data.md)
* [Xbox Live 게임 허브 데이터 가져오기](get-xbox-live-game-hub-data.md)
* [Xbox Live 클럽 데이터 가져오기](get-xbox-live-club-data.md)
* [Xbox Live 멀티 플레이 데이터 가져오기](get-xbox-live-multiplayer-data.md)
* [Xbox Live 동시 사용량 현황 데이터 가져오기](get-xbox-live-concurrent-usage-data.md)