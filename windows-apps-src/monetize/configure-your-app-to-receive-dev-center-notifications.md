---
author: Xansky
Description: Learn how to register your UWP app to receive push notifications that you send from Windows Dev Center.
title: 대상 푸시 알림을 위한 앱 구성
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, 대상 푸시 알림, 개발자 센터
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: 451643387076a8f944ffe94206ea6fac08524026
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762179"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>앱에서 대상 푸시 알림 구성

Windows 개발자 센터 대시보드의 **푸시 알림** 페이지에서 UWP(유니버설 Windows 플랫폼) 앱이 설치된 디바이스에 대상 푸시 알림을 보내 고객 참여를 직접 유도할 수 있습니다. 예를 들어 대상 푸시 알림을 사용하면 앱 평가, 새 기능 확인 등 고객의 참여를 유도할 수 있습니다. 알림 메시지, 타일 알림, 원시 XML 알림과 같이 여러 가지 유형의 푸시 알림을 보낼 수 있습니다. 푸시 알림을 통해 앱 실행 속도 결과를 추적할 수도 있습니다. 이 기능에 대한 자세한 내용은 [앱의 고객에 게 푸시 알림 보내기](../publish/send-push-notifications-to-your-apps-customers.md)를 참조하세요.

개발자 센터에서 고객에게 대상 푸시 알림을 보내려면 먼저 Microsoft Store Services SDK에서 [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 클래스의 메서드를 사용하여 알림을 받도록 앱을 등록해야 합니다. 이 클래스에서 별도의 메서드를 사용하여 대상 푸시 알림에 대한 응답으로 앱이 시작되었음을 개발자 센터에 알리고(알림으로 발생한 앱 실행 속도를 추적하려는 경우) 알림 수신을 중지할 수 있습니다.

## <a name="configure-your-project"></a>프로젝트 구성

코드를 작성하려면 먼저 다음 단계에 따라 프로젝트에서 Microsoft Store Services SDK에 참조를 추가합니다.

1. 아직 수행하지 않은 경우에는 개발 컴퓨터에 [Microsoft Store Services SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)하세요. 
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
4. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
5. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 클릭하고 **확인**을 클릭합니다.

## <a name="register-for-push-notifications"></a>푸시 알림 등록

앱에서 개발자 센터의 대상 푸시 알림을 수신하도록 등록하려면

1. 프로젝트에서, 시작 시 실행되는 코드에서 알림을 받도록 앱을 등록할 수 있는 섹션을 찾습니다.
2. 코드 파일의 맨 위에 다음 문을 추가합니다.

    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 개체를 가져와 이전에 식별한 시작 코드에서 [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버로드 중 하나를 호출합니다. 앱이 시작 될 때마다 이 메서드를 호출해야 합니다.

  * 개발자 센터에서 알림에 대한 고유한 채널 URI를 만들고 싶으면 [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버로드를 호출합니다.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > 앱에서 [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync)를 호출하여 WNS에 대한 알림 채널도 만드는 경우 코드에서 [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 및 [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버로드를 동시에 호출하지 않도록 하세요. 이 두 메서드를 모두 호출해야 하는 경우 순차적으로 호출하고 다른 메서드를 호출하기 전에 한 메서드가 반환될 때까지 기다려야 합니다.

  * 개발자 센터의 대상 푸시 알림에 사용할 채널 URI를 지정하려면 [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버로드를 호출합니다. 예를 들어 앱에서 이미 WNS(Windows 푸시 알림 서비스)를 사용하는 데 동일한 채널 URI를 사용하려는 경우 이 작업이 필요할 수도 있습니다. 먼저 [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) 개체를 만들고 채널 URI에 [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) 속성을 할당해야 합니다.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> **RegisterNotificationChannelAsync** 메서드를 호출하면 앱의 로컬 앱 데이터 저장소([ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder) 속성이 반환하는 폴더)에 MicrosoftStoreEngagementSDKId.txt라는 파일이 생성됩니다. 이 파일에는 대상 푸시 알림 인프라에서 사용하는 ID가 포함되어 있습니다. 앱에서 이 파일을 수정하거나 삭제하지 않도록 하세요. 그러지 않으면 사용자가 여러 개의 알림을 받거나, 알림이 다른 면에서 제대로 동작하지 않을 수 있습니다.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>대상 푸시 알림이 고객에게 라우팅되는 방법

앱에서 **RegisterNotificationChannelAsync**를 호출하면 이 메서드가 현재 디바이스에 로그인되어 있는 고객의 Microsoft 계정을 수집합니다. 나중에 이 고객이 포함된 세그먼트로 대상 푸시 알림을 보내면 개발자 센터에서 이 고객의 Microsoft 계정과 연결된 디바이스로 알림을 보냅니다.

앱을 시작한 고객이 Microsoft 계정으로 여전히 디바이스에 로그인된 상태에서 다른 사람에게 장치를 사용하라고 줄 경우 다른 사람이 원래 고객을 대상으로 하는 알림을 볼 수 있다는 점에 유의하세요. 이로 인해 의도치 않은 결과가 초래될 수 있으며, 고객이 로그인 후 사용하는 서비스를 제공하는 앱의 경우 특히 유의해야 합니다. 이 시나리오에서 다른 사용자가 대상 알림을 보지 못하도록 방지하려면 고객이 앱에서 로그아웃할 때 [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 메서드를 호출합니다. 자세한 내용은 이 문서의 뒷부분에 나오는 [푸시 알림 등록 취소](#unregister)를 참조하세요.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>사용자가 앱을 시작할 때 앱의 응답 방식

앱에서 알림을 수신하도록 등록한 후 [개발자 센터에서 앱 고객에게 푸시 알림을 보내고](../publish/send-push-notifications-to-your-apps-customers.md) 나면 사용자가 푸시 알림에 대한 응답으로 앱을 시작할 때 앱에서 다음 진입점 중 하나가 호출됩니다. 사용자가 앱을 시작할 때 실행할 코드 중 일부가 있는 경우에는 앱에서 이러한 진입점 중 하나에 코드를 추가할 수 있습니다.

  * 푸시 알림이 포그라운드 활성화 유형인 경우 프로젝트에서 **App** 클래스의 [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) 메서드를 재정의하고 이 메서드에 코드를 추가합니다.

  * 푸시 알림이 백그라운드 활성화 유형인 경우 [백그라운드 작업](../launch-resume/support-your-app-with-background-tasks.md)을 위해 [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드에 코드를 추가합니다.

예를 들어 앱의 유료 추가 기능을 구입한 앱 사용자에게 무료 추가 기능 사용 권한을 부여하여 사용자에게 보상하길 원할 수 있습니다. 이 경우 이러한 사용자를 대상으로 하는 [고객층](../publish/create-customer-segments.md)에게 푸시 알림을 보낼 수 있습니다. 그런 다음 위에 나열된 진입점 중 하나에서 [앱에서 바로 구매](in-app-purchases-and-trials.md) 무료 사용 권한을 부여하도록 코드를 추가할 수 있습니다.

## <a name="notify-dev-center-of-your-app-launch"></a>개발자 센터에 앱 시작 알림

개발자 센터의 대상 푸시 알림에 대해 **앱 시작 속도 추적** 옵션을 선택한 경우 앱의 적절한 진입점에서 [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 메서드를 호출하여 푸시 알림에 대한 응답으로 앱이 시작되었음을 개발자 센터에 알립니다.

이 메서드는 앱의 원래 시작 인수도 반환합니다. 푸시 알림에 대해 앱 시작 속도를 추적하도록 선택하고 나면 불특정 추적 ID가 시작 인수에 추가되어 개발자 센터에서 앱 시작 추적이 지원됩니다. 앱에 대한 실행 인수는 [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 메서드에 전달해야 합니다. 그러면 이 메서드는 개발자 센터에 추적 ID를 보내고, 시작 인수에서 추적 ID를 제거하고, 코드에 원래 실행 인수를 반환합니다.

이 메서드를 호출하는 방법은 푸시 알림의 활성화 유형에 따라 다릅니다.

* 푸시 알림이 포그라운드 활성화 유형인 경우 앱의 [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) 메서드 재정의에서 이 메서드를 호출하고, 이 메서드에 전달된 [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) 개체에서 사용할 수 있는 인수를 전달합니다. 다음 코드 예제에서는 코드 파일에 **Microsoft.Services.Store.Engagement** 및 **Windows.ApplicationModel.Activation** 네임스페이스에 대한 **using** 문이 있다고 가정합니다.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* 푸시 알림이 백그라운드 활성화 유형인 경우 [백그라운드 작업](../launch-resume/support-your-app-with-background-tasks.md)에 대한 [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드에서 이 메서드를 호출하고 이 메서드에 전달된 [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) 개체에서 사용할 수 있는 인수를 전달합니다. 다음 코드 예제에서는 코드 파일에 **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** 및 **Windows.UI.Notifications** 네임스페이스에 대한 **using** 문이 있다고 가정합니다.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>푸시 알림 등록 취소

앱에서 개발자 센터의 대상 푸시 알림 수신을 중지하려면 [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 메서드를 호출합니다.

[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

이 메서드는 앱에서 *모든* 서비스의 푸시 알림을 받지 않도록 알림에 사용되고 있는 채널을 무효화합니다. 알림이 종료되면 개발자 센터의 대상 푸시 알림 및 WNS를 사용하는 다른 알림을 포함한 어떤 서비스에도 채널을 다시 사용할 수 없습니다. 이 앱에 푸시 알림 보내기를 다시 시작하려면 앱에서 새 채널을 요청해야 합니다.

## <a name="related-topics"></a>관련 항목

* [앱의 고객에게 푸시 알림 보내기](../publish/send-push-notifications-to-your-apps-customers.md)
* [WNS(Windows 푸시 알림 서비스) 개요](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [알림 채널 요청, 만들기 및 저장 방법](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
