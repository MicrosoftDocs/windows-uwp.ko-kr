---
title: 실시간 활동 서비스 프로그래밍
author: KevinAsgari
description: Xbox Live 실시간 활동 서비스 c + + Api를 사용 하 여 프로그래밍 하는 방법을 알아봅니다.
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 실시간으로 활동
ms.localizationpriority: medium
ms.openlocfilehash: d34b1fd2a40508d100e39b784d24b5604237d499
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5938544"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>C + + Api를 사용 하 여 실시간 활동 서비스 프로그래밍

이 문서에는 다음 섹션이 포함 되어 있습니다.

* Xbox Live에서 실시간 활동 서비스에 연결
* 실시간 활동 서비스에서 분리
* 통계 만들기
* 실시간 활동에서 통계에 가입
* 실시간 활동 서비스에서 통계 가입 취소
* 샘플

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>Xbox Live에서 실시간 활동 서비스에 연결

응용 프로그램에서 Xbox Live 이벤트 정보를 가져오는 실시간 활동 (RTA) 서비스에 연결 해야 합니다. 이 항목에서는 이러한 연결을 확인 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 항목에 사용 되는 예제 메서드 호출에서 한 사용자를 나타냅니다. 그러나 타이틀 연결 하 고 실시간 활동 (RTA) 서비스에서 분리 하는 모든 사용자에 대해 이러한 호출을 확인 해야 합니다.

### <a name="connecting-to-the-real-time-activity-service"></a>실시간 활동 서비스에 연결

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

XDK 개발자 또는 작업 하는 경우 XDP에 통계를 만들 크로스 플레이 제목에 있습니다.  Windows 10에서 실행 되는 순수 UWP 하는 경우 개발자 센터에서 통계를 만듭니다.

#### <a name="xdk-developers"></a>XDK 개발자

XDP에는 상태를 만드는 방법에 대 한 자세한 내용은 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events)를 참조 하세요.  사용자 상태를 생성 한 이벤트 정의 후 응용 프로그램에서 사용 되는 헤더를 생성 하려면 [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx) 실행 해야 합니다.  이 헤더 함수가 통계를 수정 하는 이벤트를 보내는 호출할 수 있습니다.

#### <a name="uwp-developers"></a>UWP 개발자

크로스 플레이 제목이 되지 않은 Windows 10 UWP 개발 하는 경우에 [Windows 개발자 센터](https://developer.microsoft.com/dashboard/windows/overview)에서 사용자 통계를 정의 합니다. 개발자 센터에서 통계를 구성 하는 방법은 [개발자 센터 통계 구성 문서](../leaderboards-and-stats-2017/player-stats-configure-2017.md) 를 읽습니다.

> [!NOTE]
> 통계 2013 개발자가 댐을 [개발자 센터](https://developer.microsoft.com/dashboard/windows/overview)의 [통계 2013 구성](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013) 관한 정보에 대 한 문의 해야 합니다.

### <a name="disconnecting-from-the-real-time-activity-service"></a>실시간 활동 서비스에서 연결 끊기

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>실시간 활동에서 통계에 가입

응용 프로그램 가입 하는 실시간 활동 (RTA)를 변경 하 여 Xbox 개발자 포털 (XDP) 또는 Windows 개발자 센터에서 구성 통계 업데이트를 가져옵니다.

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>실시간 활동 서비스에서 통계에 가입

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
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Dev Center
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

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>실시간 활동 서비스에서 통계 가입 취소

통계 변경 될 때 업데이트를 가져오려면 실시간 활동 (RTA) 서비스에서 응용 프로그램 통계에 가입 합니다. 이러한 업데이트 필요 하지 않으며, 구독을 종료할 수 하 고이 항목의 코드는 작업을 수행 하는 방법을 보여 줍니다.

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
> 서비스를 사용 하 여 2 시간 후 끊어집니다 실시간 활동, 코드는이 감지 하 여 계속 필요할 경우 실시간 활동 서비스에 대 한 연결을 다시 설정 있어야 합니다. 만료 시 인증 토큰 새로 고쳐집니다 있는지 확인 하는 주로 수행 됩니다.
> 
> 클라이언트를 RTA 멀티 플레이 세션에 대 한 사용 30 초 동안 연결이 멀티 플레이 세션 Directory(MPSD) RTA 세션을 닫히고 사용자 세션에서 진행을 감지 합니다. 여 연결이 닫힐 때 검색 하 고 다시 연결을 시작 하 고, MPSD 세션을 종료 하기 전에 다시 RTA 클라이언트에 됩니다.