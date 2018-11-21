---
author: mijacobs
Description: Raw notifications are short, general purpose push notifications.
title: 원시 알림 개요
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e1a015d5d51ad0c15f20755afcb0d324acd1f36
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7560737"
---
# <a name="raw-notification-overview"></a>원시 알림 개요


원시 알림은 짧고 일반적인 목적의 알림으로, 오로지 사용 안내만 하며 UI 구성 요소는 포함하지 않습니다. 다른 푸시 알림과 마찬가지로, WNS(Windows 푸시 알림 서비스) 기능은 클라우드 서비스에서 앱으로 원시 알림을 전달합니다.

원시 알림은 사용자에게 적절한 앱 권한이 있는 경우 앱이 백그라운드 작업을 실행하도록 트리거하는 것을 비롯하여 다용도로 사용할 수 있습니다. WNS를 사용하여 앱과 통신하면 영구적 소켓 연결 만들기, HTTP GET 메시지 보내기 및 다른 서비스-앱 연결에 대한 처리 오버헤드를 피할 수 있습니다.

> [!IMPORTANT]
> 원시 알림을 이해하려면 [WNS(Windows 푸시 알림 서비스) 개요](windows-push-notification-services--wns--overview.md)에 설명된 개념을 익히는 것이 가장 좋습니다.

 

알림 메시지, 타일 및 배지 푸시 알림과 마찬가지로, 원시 알림은 할당된 채널 URI(Uniform Resource Identifier)를 통해 앱의 클라우드 서비스에서 WNS로 푸시됩니다. 그러면 WNS이 채널과 관련된 디바이스 및 사용자 계정에 다시 알림을 전달합니다. 다른 푸시 알림과는 달리, 원시 알림에는 지정된 형식이 없습니다. 페이로드의 콘텐츠는 전적으로 앱에서 정의됩니다.

원시 알림을 사용하여 이점을 누릴 수 있는 앱의 실제 사례로, 이론상의 문서 공동 작업 앱을 살펴보겠습니다. 두 명의 사용자가 동시에 같은 문서를 편집 중이라고 가정해 보세요. 공유 문서를 호스트하는 클라우드 서비스는 원시 알림을 사용하여 한 사용자가 문서를 변경하면 이를 각 사용자에게 알릴 수 있습니다. 문서에 대한 변경 사항을 반드시 원시 알림에 포함할 필요는 없으며, 대신 중앙 위치에 연결하여 적용 가능한 변경 사항을 동기화하도록 각 사용자의 앱 사본을 알립니다. 원시 알림을 사용하여 앱과 앱의 클라우드 서비스는 문서가 열려 있는 전체 시간 동안 연결을 유지하는 데 따르는 오버헤드를 줄일 수 있습니다.

## <a name="how-raw-notifications-work"></a>원시 알림 작동 방법


모든 원시 알림은 푸시 알림입니다. 따라서 원시 알림을 보내고 받는 데 필요한 설정은 원시 알림에도 적용됩니다.

-   원시 알림을 보낼 유효한 WNS 채널이 있어야 합니다. 푸시 알림 채널 획득에 대한 자세한 내용은 [알림 채널을 요청, 생성 및 저장하는 방법](https://msdn.microsoft.com/library/windows/apps/hh465412)을 참조하세요.
-   앱의 매니페스트에 **Internet** 접근 권한 값을 포함해야 합니다. Microsoft Visual Studio 매니페스트 편집기에는 **접근 권한 값** 탭 아래에 **인터넷(클라이언트)** 이라는 옵션이 있습니다. 자세한 내용은 [**Capabilities**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities)를 참조하세요.

알림의 본문은 앱에서 정의하는 형식입니다. 클라이언트는 앱에서 인식하기만 하면 되는 null로 끝나는 문자열(**HSTRING**)로 데이터를 받습니다.

클라이언트가 오프라인인 경우 원시 알림은 [X-WNS-Cache-Policy](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache) 헤더가 알림에 포함되어 있을 때만 WNS에 의해 캐시됩니다. 그러나 디바이스가 온라인 상태로 돌아오면 하나의 원시 알림만 캐시되고 전달됩니다.

클라이언트에서 원시 알림이 취할 수 있는 가능한 경로는 세 개뿐입니다. 이러한 경로는 알림 전달 이벤트를 통해 실행 중인 앱에 전달되고, 백그라운드 작업에 보내지거나, 삭제됩니다. 따라서 클라이언트가 오프라인인 상태에서 WNS가 원시 알림을 보내려고 시도하면 알림은 삭제됩니다.

## <a name="creating-a-raw-notification"></a>원시 알림 만들기


원시 알림 보내기는 타일, 알림 메시지 또는 배지 푸시 알림을 보내는 것과 유사하지만 다음과 같은 차이점이 있습니다.

-   HTTP Content-Type 헤더를 "application/octet-stream"으로 설정해야 합니다.
-   HTTP [X-WNS-Type](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_type) 헤더를 "wns/raw"로 설정해야 합니다.
-   알림 본문에는 5KB 미만의 문자열 페이로드가 포함될 수 있습니다.

원시 알림은 앱이 서비스에 직접 연결하여 대량의 데이터를 동기화하거나, 알림 콘텐츠를 기반으로 로컬 상태를 수정하는 등의 동작을 취하도록 트리거하는 짧은 메시지로 사용하기 위한 것입니다. WNS 푸시 알림은 전달이 보장되지 않으므로 앱과 클라우드 서비스에서는 클라이언트가 오프라인인 경우와 같이 원시 알림이 클라이언트에 도달하지 않을 가능성을 고려해야 합니다.

푸시 알림 보내기에 대한 자세한 내용은 [빠른 시작: 푸시 알림 보내기](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)를 참조하세요.

## <a name="receiving-a-raw-notification"></a>원시 알림 받기


앱이 원시 알림을 받을 수 있는 경로는 두 가지입니다.

-   앱이 실행 중인 동안 [알림 전송 이벤트](#notification-delivery-events)를 통해서
-   앱에서 백그라운드 작업을 실행할 수 있는 경우 [원시 알림에 의해 트리거된 백그라운드 작업](#background-tasks-triggered-by-raw-notifications)을 통해서

앱은 두 메커니즘을 모두 사용하여 원시 알림을 받을 수 있습니다. 앱이 알림 전달 이벤트 처리기와 원시 알림에서 트리거된 백그라운드 작업 둘 다를 구현하는 경우 앱이 실행 중이면 알림 전달 이벤트가 우선권을 갖게 됩니다.

-   앱이 실행 중일 때는 알림 전달 이벤트가 백그라운드 작업보다 우선하며 앱은 알림을 처리할 첫 번째 기회를 갖습니다.
-   알림 전달 이벤트 처리기는 이벤트의 [**PushNotificationReceivedEventArgs.Cancel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) 속성을 **true**로 설정하여 처리기가 있으면 원시 알림이 백그라운드 작업으로 전달되지 않도록 지정할 수 있습니다. **Cancel** 속성이 **false**로 설정되거나 또는 설정되지 않을 경우(기본값이 **false**) 알림 전달 이벤트 처리기가 작업을 완료한 후 원시 알림이 백그라운드 작업을 트리거합니다.

### <a name="notification-delivery-events"></a>알림 전달 이벤트

앱이 사용 중인 동안 앱에서는 알림 전달 이벤트([**PushNotificationReceived**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived))를 사용하여 원시 알림을 받을 수 있습니다. 클라우드 서비스에서 원시 알림을 보내면 실행 중인 앱이 채널 URI에서 알림 전달 이벤트를 처리하여 원시 알림을 받을 수 있습니다.

앱이 실행 중이 아니고 [백그라운드 작업](#background-tasks-triggered-by-raw-notifications)을 사용하지 않는 경우에는 WNS에서 해당 앱으로 보내진 원시 알림이 수신되면 삭제합니다. 클라우드 서비스의 리소스를 낭비하지 않으려면 앱이 활성 상태인지 여부를 추적하도록 서비스에 논리를 구현해야 합니다. 이 정보의 원본은 두 가지입니다. 앱은 알림을 받을 준비가 되었음을 명시적으로 서비스에 알릴 수 있고, WNS는 중지해야 할 시점을 서비스에 알릴 수 있습니다.

-   **앱이 클라우드 서비스에 알림**: 앱은 서비스에 연결하여 앱이 포그라운드로 실행 중임을 알릴 수 있습니다. 이러한 접근 방식의 단점은 앱이 서비스 연결을 아주 빈번하게 끊을 수 있다는 것입니다. 그러나 앱이 들어오는 원시 알림을 받을 준비가 되었을 때를 항상 서비스가 알게 된다는 장점도 있습니다. 또 다른 장점은 앱이 서비스에 연결되면 서비스가 브로드캐스트가 아닌 해당 앱의 특정 인스턴스에 원시 알림을 보내야 한다는 것을 아는 것입니다.
-   **클라우드 서비스가 WNS 응답 메시지에 응답**: 앱 서비스는 WNS에서 반환된 [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) 및 [X-WNS-DeviceConnectionStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_dcs) 정보를 사용하여 앱에 원시 알림 보내기를 언제 중지할지 결정할 수 있습니다. 서비스가 HTTP POST로 채널에 알림을 보내면 응답으로 다음 메시지 중 하나를 받을 수 있습니다.

    -   **X-WNS-NotificationStatus: 삭제**: 클라이언트가 알림을 받지 않았음을 나타냅니다. 사용자의 디바이스에서 더 이상 포그라운드로 실행 중이지 않은 앱에서 **dropped** 응답이 발생되었다고 추정할 수 있습니다.
    -   **X-WNS-DeviceConnectionStatus: 연결 끊김** 또는 **X-WNS-DeviceConnectionStatus: 일시적 연결**: Windows 클라이언트가 더 이상 WNS에 연결되어 있지 않음을 나타냅니다. WNS에서 이 메시지를 받으려면 알림의 HTTP POST에 [X-WNS-RequestForStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_request) 헤더를 설정하여 요청해야 합니다.

    앱의 클라우드 서비스는 이러한 상태 메시지의 정보를 사용하여 원시 알림을 통해 통신을 시도할 수 있습니다. 앱이 포그라운드로 다시 전환되고 서비스와 연결되면 서비스는 원시 알림 보내기를 다시 시작할 수 있습니다.

    알림이 성공적으로 클라이언트에 전달되었는지 여부를 확인하기 위해 [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification)를 사용해서는 안 됩니다.

    자세한 내용은 [푸시 알림 서비스 요청 및 응답 헤더](https://msdn.microsoft.com/library/windows/apps/hh465435)를 참조하세요.

### <a name="background-tasks-triggered-by-raw-notifications"></a>원시 알림에 의해 트리거된 백그라운드 작업

> [!IMPORTANT]
> 원시 알림 백그라운드 작업을 사용하기 전에 [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)를 통한 백그라운드 액세스가 앱에 허용되어야 합니다.

 

백그라운드 작업을 [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)에 등록해야 합니다. 등록하지 않을 경우 원시 알림이 수신될 때 작업이 실행되지 않습니다.

원시 알림에 의해 트리거된 백그라운드 작업은 앱의 클라우드 서비스가 앱에 연결될 수 있도록 합니다. 이는 앱이 실행되지 않는 경우에도 마찬가지입니다(앱이 실행되도록 트리거될 수는 있음). 이러한 상황은 앱이 지속적 연결을 유지할 필요 없이 이루어집니다. 원시 알림은 백그라운드 작업을 트리거할 수 있는 유일한 알림 형식입니다. 알림 메시지, 타일 및 배지 푸시 알림이 백그라운드 작업을 트리거할 수는 없지만 원시 알림에 의해 트리거된 백그라운드 작업이 로컬 API 호출을 통해 타일을 업데이트하고 알림 메시지를 호출할 수 있습니다.

원시 알림에 의해 트리거된 백그라운드 작업의 작동 방식 사례로, 전자책을 읽는 데 사용되는 앱을 살펴보겠습니다. 먼저 사용자가 다른 디바이스에서 온라인으로 책을 구입합니다. 이에 대해 앱의 클라우드 서비스는 사용자의 각 디바이스에 책을 구입했고 앱이 이를 다운로드해야 한다는 내용이 명시된 페이로드와 함께 원시 알림을 보낼 수 있습니다. 앱은 나중에 사용자가 앱을 실행하면 바로 읽을 수 있는 상태로 책이 준비되도록 앱의 클라우드 서비스에 직접 연결하여 새 책의 백그라운드 다운로드를 시작합니다.

원시 알림을 사용하여 백그라운드 작업을 트리거하려면 앱이 다음 요구 사항을 충족해야 합니다.

1.  [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)를 사용하여 백그라운드에서 작업을 실행할 수 있는 권한(사용자가 언제든지 해지할 수 있는)을 요청합니다.
2.  백그라운드 작업을 구현해야 합니다. 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](../../../launch-resume/support-your-app-with-background-tasks.md)을 참조하세요.

그러면 백그라운드 작업이 앱에 대한 원시 알림이 수신될 때마다 [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)에 대한 응답으로 호출됩니다. 백그라운드 작업에서 원시 알림의 앱별 페이로드를 해석하고 그에 따른 조치를 취합니다.

각 앱에 대해 한 번에 하나의 백그라운드 작업만 실행할 수 있습니다. 백그라운드 작업이 이미 실행 중인 앱에 대해 백그라운드 작업이 트리거되면 새 백그라운드 작업이 실행되기 전에 첫 번째 백그라운드 작업을 완료해야 합니다.

## <a name="other-resources"></a>기타 리소스


Windows8.1에 대 한 Windows8.1, 및는 [푸시 및 정기 알림 샘플에](http://go.microsoft.com/fwlink/p/?LinkId=231476) 대 한 [원시 알림 샘플](http://go.microsoft.com/fwlink/p/?linkid=241553) 을 다운로드 하 고 Windows10 앱에서 해당 소스 코드를 다시 사용 하 여 자세히 알아볼 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [원시 알림에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh761463)
* [빠른 시작: 원시 알림 백그라운드 작업 만들기 및 등록](https://msdn.microsoft.com/library/windows/apps/jj676800)
* [빠른 시작: 실행 중인 앱에 대한 푸시 알림 가로채기](https://msdn.microsoft.com/library/windows/apps/jj709908)
* [**RawNotification**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 




