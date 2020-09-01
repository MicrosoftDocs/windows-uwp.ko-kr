---
description: 엄격 하 게 지침을 제공 하 고 UI 구성 요소를 포함 하지 않는 간단한 범용 푸시 알림과 같은 원시 알림에 대해 알아봅니다.
title: 푸시 알림 개요
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6b01f961a28e3a6db52725c4f2f26dc3b3aaee9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172367"
---
# <a name="raw-notification-overview"></a>푸시 알림 개요


원시 알림은 짧고 일반적인 목적의 알림으로, 오로지 사용 안내만 하며 UI 구성 요소는 포함하지 않습니다. 다른 푸시 알림과 마찬가지로 WNS (Windows Push Notification Services) 기능은 클라우드 서비스에서 앱에 대 한 원시 알림을 제공 합니다.

사용자가 앱 사용 권한을 부여 받은 경우 백그라운드 작업을 실행 하도록 앱을 트리거하는 등의 다양 한 용도로 원시 알림을 사용할 수 있습니다. WNS를 사용 하 여 앱과 통신 하는 경우 영구 소켓 연결을 만들고 HTTP GET 메시지를 보내고 기타 서비스 간 연결에 대 한 처리 오버 헤드를 피할 수 있습니다.

> [!IMPORTANT]
> 원시 알림을 이해 하려면 [WNS (Windows Push Notification Services) 개요](windows-push-notification-services--wns--overview.md)에 설명 된 개념에 대해 잘 알고 있어야 합니다.

 

알림, 타일 및 배지 푸시 알림과 마찬가지로, 할당 된 채널 URI (Uniform Resource Identifier)를 통해 앱의 클라우드 서비스에서 WNS로 원시 알림이 푸시됩니다. 그러면 WNS는 해당 채널과 연결 된 장치 및 사용자 계정에 알림을 전달 합니다. 다른 푸시 알림과 달리 원시 알림은 지정 된 형식이 아닙니다. 페이로드의 콘텐츠는 완전히 앱 정의 됩니다.

원시 알림의 이점을 누릴 수 있는 앱에 대 한 그림으로, 이론상 문서 공동 작업 앱을 살펴보겠습니다. 동일한 문서를 동시에 편집 하는 두 명의 사용자를 고려 합니다. 공유 문서를 호스트 하는 클라우드 서비스는 다른 사용자가 변경 내용을 적용할 때 각 사용자에 게 알리기 위해 원시 알림을 사용할 수 있습니다. 원시 알림에는 문서에 대 한 변경 내용이 반드시 포함 되는 것은 아닙니다. 대신 각 사용자의 앱 복사본에 중앙 위치에 연결 하 여 사용 가능한 변경 내용을 동기화 하도록 신호를 보냅니다. 앱과 해당 클라우드 서비스는 원시 알림을 사용 하 여 문서가 열려 있는 전체 시간 동안 영구 연결을 유지 관리 하는 오버 헤드를 절감할 수 있습니다.

## <a name="how-raw-notifications-work"></a>원시 알림의 작동 방법


모든 원시 알림은 푸시 알림입니다. 따라서 푸시 알림을 보내고 받는 데 필요한 설정은 원시 알림에도 적용 됩니다.

-   원시 알림을 보내려면 유효한 WNS 채널이 있어야 합니다. 푸시 알림 채널을 얻는 방법에 대 한 자세한 내용은 [알림 채널을 요청 하 고 만들고 저장 하는 방법](/previous-versions/windows/apps/hh465412(v=win.10))을 참조 하세요.
-   앱의 매니페스트에 **인터넷** 기능을 포함 해야 합니다. Microsoft Visual Studio 매니페스트 편집기의 **기능** 탭 아래에서 **Internet (Client)** 로이 옵션을 찾을 수 있습니다. 자세한 내용은 [**기능**](/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities)을 참조 하세요.

알림의 본문은 앱 정의 형식입니다. 클라이언트는 응용 프로그램에서 인식 해야 하는 null로 끝나는 문자열 (**Hstring**)로 데이터를 수신 합니다.

클라이언트가 오프 라인 상태인 경우에는 [캐시 정책](/previous-versions/windows/apps/hh465435(v=win.10)) 헤더가 알림에 포함 된 경우에만 기본 알림이 WNS에 의해 캐시 됩니다. 그러나 장치가 다시 온라인 상태가 되 면 하나의 원시 알림만 캐시 되 고 배달 됩니다.

클라이언트에서 사용 하는 원시 알림에는 세 개의 가능한 경로만 있습니다. 알림 배달 이벤트를 통해 실행 중인 앱에 전달 되거나 백그라운드 작업으로 전송 되거나 삭제 됩니다. 따라서 클라이언트가 오프 라인 상태이 고 WNS가 원시 알림을 배달 하려고 하면 알림이 삭제 됩니다.

## <a name="creating-a-raw-notification"></a>원시 알림 만들기


원시 알림을 보내는 것은 타일, 알림 또는 배지 푸시 알림을 보내는 것과 유사 하며 다음과 같은 차이점이 있습니다.

-   HTTP Content-type 헤더는 "응용 프로그램/8 진수 스트림"으로 설정 해야 합니다.
-   HTTP [X wns](/previous-versions/windows/apps/hh465435(v=win.10)) 헤더는 "wns/raw"로 설정 해야 합니다.
-   알림 본문의 크기는 5, 000KB 보다 작은 모든 문자열 페이로드를 포함할 수 있습니다.

원시 알림은 서비스에 직접 연결 하 여 더 많은 양의 데이터를 동기화 하거나 알림 콘텐츠를 기반으로 로컬 상태를 수정 하는 등의 작업을 수행 하기 위해 응용 프로그램을 트리거하는 짧은 메시지로 사용 됩니다. WNS 푸시 알림은 배달 되지 않을 수 있으므로 앱 및 클라우드 서비스는 클라이언트가 오프 라인인 경우와 같이 원시 알림이 클라이언트에 도달 하지 못할 가능성을 고려해 야 합니다.

푸시 알림을 보내는 방법에 대 한 자세한 내용은 [빠른 시작: 푸시 알림 보내기](/previous-versions/windows/apps/hh868252(v=win.10))를 참조 하세요.

## <a name="receiving-a-raw-notification"></a>원시 알림 받기


앱에서 원시 알림을 받을 수 있는 두 가지 방법이 있습니다.

-   응용 프로그램을 실행 하는 동안 [알림 배달 이벤트](#notification-delivery-events) 를 통해.
-   앱이 백그라운드 작업을 실행할 수 있도록 설정 된 경우 [원시 알림에 의해 트리거되는 백그라운드 작업](#background-tasks-triggered-by-raw-notifications) 을 통해.

앱은 두 가지 메커니즘을 모두 사용 하 여 원시 알림을 받을 수 있습니다. 앱이 알림 배달 이벤트 처리기와 원시 알림에 의해 트리거되는 백그라운드 작업을 둘 다 구현 하는 경우 앱이 실행 중일 때 알림 배달 이벤트가 우선적으로 적용 됩니다.

-   앱이 실행 중인 경우 알림 배달 이벤트가 백그라운드 작업 보다 우선적으로 적용 되 고 앱이 알림을 처리할 수 있는 첫 번째 기회를 갖게 됩니다.
-   알림 배달 이벤트 처리기는 이벤트의 [**PushNotificationReceivedEventArgs**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) 속성을 **true**로 설정 하 여 처리기가 종료 된 후 원시 알림이 백그라운드 작업에 전달 되지 않아야 함을 지정할 수 있습니다. **Cancel** 속성이 **false** 로 설정 되거나 설정 되지 않은 경우 (기본값은 **false**) 알림 배달 이벤트 처리기가 작업을 완료 한 후에 원시 알림이 백그라운드 작업을 트리거합니다.

### <a name="notification-delivery-events"></a>알림 배달 이벤트

앱을 사용 하는 동안 앱에서 알림 배달 이벤트 ([**Pushnotificationreceived**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived))를 사용 하 여 원시 알림을 받을 수 있습니다. 클라우드 서비스가 원시 알림을 보낼 때 실행 중인 앱은 채널 URI에서 알림 배달 이벤트를 처리 하 여 수신할 수 있습니다.

앱이 실행 되 고 있지 않고 [백그라운드 작업](#background-tasks-triggered-by-raw-notifications)을 사용 하지 않는 경우 해당 앱에 전송 된 모든 원시 알림이 수신 시 WNS에 의해 삭제 됩니다. 클라우드 서비스의 리소스를 낭비 하지 않으려면 응용 프로그램이 활성 상태 인지 여부를 추적 하는 논리를 서비스에 구현 하는 것을 고려해 야 합니다. 이 정보에는 두 가지 소스가 있습니다. 앱은 알림 수신을 시작할 준비가 되었음을 서비스에 명시적으로 알릴 수 있으며, WNS는 서비스를 중지할 때이를 알릴 수 있습니다.

-   앱은 **클라우드 서비스에 알립니다**. 앱은 해당 서비스에 연결 하 여 응용 프로그램이 포그라운드에서 실행 되 고 있음을 알 수 있습니다. 이 방법의 단점은 앱에서 서비스에 매우 자주 연결 하는 것입니다. 그러나 앱이 들어오는 원시 알림을 받을 준비가 되 면 서비스에서 항상 알 수 있는 이점이 있습니다. 또 다른 이점은 앱이 서비스에 연결 될 때 서비스는 브로드캐스트 대신 해당 앱의 특정 인스턴스로 원시 알림을 보내도록 합니다.
-   **클라우드 서비스는 wns 응답 메시지에 응답 합니다** . app SERVICE는 wns에서 반환 된 [x-wns-deviceconnectionstatus](/previous-versions/windows/apps/hh465435(v=win.10)) [정보를 사용](/previous-versions/windows/apps/hh465435(v=win.10)) 하 여 앱에 대 한 원시 알림 전송을 중지할 시기를 결정할 수 있습니다. 서비스에서 HTTP 게시물로 채널에 알림을 보내면 응답에서 다음 메시지 중 하나를 수신할 수 있습니다.

    -   **X WNS-NotificationStatus: 삭제**됨: 클라이언트가 알림을 받지 못했음을 나타냅니다. **삭제** 된 응답은 앱이 더 이상 사용자 장치의 포그라운드에 있지 않기 때문에 발생 하는 것이 안전 합니다.
    -   **X-wns-deviceconnectionstatus: disconnected** 또는 **x-wns-x-wns-deviceconnectionstatus: Tempconnected**: Windows 클라이언트에 더 이상 WNS에 대 한 연결이 없음을 나타냅니다. WNS에서이 메시지를 받으려면 알림의 HTTP POST에서 [X WNS-RequestForStatus](/previous-versions/windows/apps/hh465435(v=win.10)) 헤더를 설정 하 여 요청 해야 합니다.

    앱의 클라우드 서비스는 이러한 상태 메시지의 정보를 사용 하 여 원시 알림을 통한 통신 시도를 중단할 수 있습니다. 앱이 다시 포그라운드로 전환 되 면 서비스는 앱에서 연결 된 후 원시 알림 전송을 다시 시작할 수 있습니다.

    알림이 성공적으로 클라이언트에 전달 되었는지 여부를 확인 하기 위해 [X WNS-NotificationStatus](/previous-versions/windows/apps/hh465435(v=win.10)) 를 사용 하면 안 됩니다.

    자세한 내용은 [푸시 알림 서비스 요청 및 응답 헤더](/previous-versions/windows/apps/hh465435(v=win.10)) 를 참조 하세요.

### <a name="background-tasks-triggered-by-raw-notifications"></a>원시 알림에 의해 트리거되는 백그라운드 작업

> [!IMPORTANT]
> 원시 알림 백그라운드 작업을 사용 하기 전에 [**BackgroundExecutionManager**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)를 통해 앱에 백그라운드 액세스를 부여 해야 합니다.

 

백그라운드 작업은 [**Pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)를 사용 하 여 등록 해야 합니다. 등록 되지 않은 경우 원시 알림이 수신 될 때 작업이 실행 되지 않습니다.

앱이 실행 되 고 있지 않더라도 앱이 실행 되 고 있지 않더라도 원시 알림에 의해 트리거되는 백그라운드 작업을 통해 앱의 클라우드 서비스에서 앱에 연결할 수 있습니다. 이는 앱이 연속 연결을 유지 관리할 필요 없이 발생 합니다. 원시 알림은 백그라운드 작업을 트리거할 수 있는 유일한 알림 유형입니다. 그러나 알림, 타일 및 배지 푸시 알림은 백그라운드 작업을 트리거할 수 없지만, 원시 알림에 의해 트리거되는 백그라운드 작업은 타일을 업데이트 하 고 로컬 API 호출을 통해 알림 메시지를 호출할 수 있습니다.

원시 알림에 의해 트리거되는 백그라운드 작업이 작동 하는 방식에 대 한 그림으로, 전자 서적 읽기에 사용 되는 앱을 살펴보겠습니다. 먼저, 사용자가 다른 장치에서 온라인으로 온라인 설명서를 구입 합니다. 이에 대 한 응답으로 앱의 클라우드 서비스는 각 사용자의 장치에 대 한 원시 알림을 보낼 수 있습니다. 페이로드를 사용 하 여 책을 구입 하 고 앱에서 다운로드 해야 합니다. 그런 다음 앱은 앱의 클라우드 서비스에 직접 연결 하 여 나중에 사용자가 앱을 시작할 때 책이 이미 있고 읽을 준비가 되도록 새 책의 백그라운드 다운로드를 시작 합니다.

백그라운드 작업을 트리거하기 위해 원시 알림을 사용 하려면 앱에서 다음을 수행 해야 합니다.

1.  [**BackgroundExecutionManager**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)를 사용 하 여 사용자가 언제 든 지 취소할 수 있는 백그라운드에서 작업을 실행할 수 있는 권한을 요청 합니다.
2.  백그라운드 작업을 구현 합니다. 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](../../../launch-resume/support-your-app-with-background-tasks.md) 을 참조 하세요.

그러면 앱에 대 한 원시 알림이 수신 될 때마다 [**푸시 작업이 Pushnotificationtrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)에 대 한 응답으로 호출 됩니다. 백그라운드 작업은 원시 알림의 앱 별 페이로드를 해석 하 고이에 대해 작동 합니다.

각 앱에 대해 한 번에 하나의 백그라운드 작업을 실행할 수 있습니다. 백그라운드 작업이 이미 실행 중인 앱에 대해 백그라운드 작업이 트리거되면 첫 번째 백그라운드 작업이 완료 되어야 새 작업을 실행할 수 있습니다.

## <a name="other-resources"></a>기타 리소스


Windows 8.1에 대 한 [원시 알림 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Raw%20notifications%20sample%20(Windows%208)) 및 Windows 8.1에 대 한 [푸시 및 정기 알림 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) 을 다운로드 하 고 Windows 10 앱에서 소스 코드를 다시 사용 하 여 자세히 알아볼 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [원시 알림에 대 한 지침]()
* [빠른 시작: 원시 알림 백그라운드 작업 만들기 및 등록](/previous-versions/windows/apps/jj676800(v=win.10))
* [빠른 시작: 실행 중인 앱에 대 한 푸시 알림 가로채기](/previous-versions/windows/apps/jj709908(v=win.10))
* [**RawNotification**](/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 