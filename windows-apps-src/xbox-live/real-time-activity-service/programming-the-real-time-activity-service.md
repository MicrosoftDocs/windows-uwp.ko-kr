---
title: 실시간 활동 서비스 프로그래밍
description: Xbox Live 실시간 작업 서비스 c + + Api로 프로그래밍 하는 방법을 알아봅니다.
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 실시간 활동
ms.localizationpriority: medium
ms.openlocfilehash: f8846d57343f4f7262bbeea2cec03465fa23b2ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629998"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>C + + Api를 사용 하 여 실시간 작업 서비스 프로그래밍

이 문서에는 다음 섹션이 포함 되어 있습니다.

* Xbox Live에서 실시간 활동 서비스에 연결
* 실시간 작업 서비스에서 연결 끊김
* 통계 만들기
* 실시간 활동에서 통계를 구독합니다.
* 실시간 작업 서비스에서 통계에서 구독 취소
* 샘플

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>Xbox Live에서 실시간 활동 서비스에 연결

응용 프로그램은 Xbox Live에서 이벤트 정보를 가져오기 위해 활동 RTA (실시간) 서비스에 연결 해야 합니다. 이 항목에서는 이러한 연결을 확인 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 항목의 예는 한 명의 사용자에 대 한 메서드 호출을 나타냅니다. 그러나을 제목에 연결 및 분리 작업 RTA (실시간) 서비스에서 모든 사용자가이 함수를이 호출 해야 합니다.

### <a name="connecting-to-the-real-time-activity-service"></a>실시간 작업 서비스에 연결

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>통계 만들기

통계에서 만든 XDP는 XDK 개발자 또는 작업 간 play 제목을 합니다.  순수 UWP Windows 10을 실행 하는 경우 파트너 센터에서 통계를 만듭니다.

#### <a name="xdk-developers"></a>XDK 개발자

XDP에는 상태를 만드는 방법에 대 한 자세한 내용은 참조 하십시오 합니다 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events)합니다.  프로그램 상태를 생성 한 이벤트를 정의 후 실행 해야 합니다는 [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx) 응용 프로그램에서 사용 하는 헤더를 생성 합니다.  이 헤더에는 통계를 수정 하는 이벤트를 보내기 위해 호출할 수는 함수가 포함 됩니다.

#### <a name="uwp-developers"></a>UWP 개발자

통계 간 play 제목이 없는 Windows 10에서 UWP 개발 하는 경우 정의한 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 읽기를 [파트너 센터 통계 구성 문서](../leaderboards-and-stats-2017/player-stats-configure-2017.md) 하려면 파트너 센터에서 통계를 구성 하는 방법을 알아봅니다.

> [!NOTE]
> 통계 2013 개발자와 관련 된 정보에 대 한 해당 파악을 문의 해야 합니다 [통계 2013 구성](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013) 에 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.

### <a name="disconnecting-from-the-real-time-activity-service"></a>실시간 작업 서비스에서 연결 끊기

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>실시간 활동에서 통계를 구독합니다.

응용 프로그램에는 활동 RTA (실시간) Xbox 개발자 포털 (XDP) 또는 파트너 센터에서 구성 하는 통계 변경 될 때 업데이트를 구독 합니다.

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>통계는 실시간 활동 서비스에서 구독

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Partner Center
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>실시간 작업 서비스에서 통계에서 구독 취소

응용 프로그램 통계 변경 될 때 업데이트를 받을 활동 RTA (실시간) 서비스의 통계를 구독 합니다. 이러한 업데이트는 더 이상 필요 없는, 구독, 종료와이 항목의 코드에는 작업을 수행 하는 방법을 보여 줍니다.

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>실시간 서비스 통계에서 구독 취소

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

> [!IMPORTANT]
> 서비스를 사용 하 여 두 시간 후 끊어집니다 실시간 활동을 코드를이 감지 하 여 여전히 필요할 경우 실시간 작업 서비스에 대 한 연결을 다시 설정 수 있어야 합니다. 인증 토큰 만료 시 새로 고침을 확인 하는 데 주로 수행 됩니다.
> 
> 클라이언트가 멀티 플레이 세션에 대 한 RTA를 사용 하 여 30 초에 대 한 연결이 끊어지면를 멀티 플레이 세션 Directory(MPSD) RTA 세션을 닫고 사용자 세션에서 로그 아웃 시작을 검색 합니다. 에 달려 RTA 클라이언트 연결이 닫힌 경우를 감지 하 고 다시 연결 시작 MPSD 세션을 종료 하기 전에 재구독 합니다.