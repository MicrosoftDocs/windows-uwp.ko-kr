---
author: mcleanbyron
Description: "UWP 앱에서 Windows 개발자 센터에서 보낸 푸시 알림을 받도록 등록하는 방법을 알아봅니다."
title: "개발자 센터 푸시 알림을 받도록 앱 구성"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: d840fbe66e5ccb439148c7849e44b923a5586740

---

# <a name="configure-your-app-to-receive-dev-center-push-notifications"></a>개발자 센터 푸시 알림을 받도록 앱 구성

Windows 개발자 센터 대시보드의 **푸시 알림** 페이지에서 UWP(유니버설 Windows 플랫폼) 앱이 설치된 디바이스에 대상 푸시 알림을 보내 고객 참여를 직접 유도할 수 있습니다. 예를 들어 대상 푸시 알림을 사용하면 앱 평가, 새 기능 확인 등 고객의 참여를 유도할 수 있습니다. 알림 메시지, 타일 알림, 원시 XML 알림과 같이 여러 가지 유형의 푸시 알림을 보낼 수 있습니다. 푸시 알림을 통해 앱 실행 속도 결과를 추적할 수도 있습니다. 이 기능에 대한 자세한 내용은 [앱의 고객에 게 푸시 알림 보내기](../publish/send-push-notifications-to-your-apps-customers.md)를 참조하세요.

개발자 센터에서 고객에게 대상 푸시 알림을 보내려면 먼저 Microsoft Store Services SDK에서 [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 클래스의 메서드를 사용하여 알림을 받도록 앱을 등록해야 합니다. 이 클래스에서 별도의 메서드를 사용하여 대상 푸시 알림에 대한 응답으로 앱이 시작되었음을 개발자 센터에 알리고(알림으로 발생한 앱 실행 속도를 추적하려는 경우) 알림 수신을 중지할 수 있습니다.

## <a name="configure-your-project"></a>프로젝트 구성

코드를 작성하려면 먼저 다음 단계에 따라 프로젝트에서 Microsoft Store Services SDK에 참조를 추가합니다.

1. 아직 수행하지 않은 경우에는 관리 컴퓨터에 [Microsoft Store Services SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)하세요. 알림을 받기 위해 앱을 등록하는 데 필요한 API 외에, 이 SDK는 A/B 테스트로 앱에서 실험 실행, 광고 표시 등의 다른 기능을 위한 API도 제공합니다.
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
4. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
5. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 클릭하고 **확인**을 클릭합니다.

## <a name="register-for-push-notifications"></a>푸시 알림 등록

앱에서 개발자 센터의 대상 푸시 알림을 수신하도록 등록하려면

1. 프로젝트에서, 시작 시 실행되는 코드에서 개발자 센터 알림을 받도록 앱을 등록할 수 있는 섹션을 찾습니다.
2. 코드 파일의 맨 위에 다음 문을 추가합니다.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) 개체를 가져와 이전에 식별한 시작 코드에서 [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync.aspx) 오버로드 중 하나를 호출합니다. 앱이 시작 될 때마다 이 메서드를 호출해야 합니다.

  * 개발자 센터에서 알림에 대한 고유한 채널 URI를 만들게 하려면 [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 오버로드를 호출합니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]

    <span/>
    >**중요**&nbsp;&nbsp;앱에서 [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx)를 호출하여 WNS에 대한 알림 채널도 만드는 경우 코드에서 [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) 및 [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 오버로드를 동시에 호출하지 않도록 하세요. 이 두 메서드를 모두 호출해야 하는 경우 순차적으로 호출하고 다른 메서드를 호출하기 전에 한 메서드가 반환될 때까지 기다려야 합니다.

  * 개발자 센터의 대상 푸시 알림에 사용할 채널 URI를 지정하려면 [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://msdn.microsoft.com/library/windows/apps/mt771191.aspx) 오버로드를 호출합니다. 예를 들어 앱에서 이미 WNS(Windows 푸시 알림 서비스)를 사용하는 데 동일한 채널 URI를 사용하려는 경우 이 작업이 필요할 수도 있습니다. 먼저 [StoreServicesNotificationChannelParameters](https://msdns.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.aspx) 개체를 만들고 채널 URI에 [CustomNotificationChannelUri](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri.aspx) 속성을 할당해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

  >#### <a name="understanding-how-your-app-responds-when-the-user-launches-your-app"></a>사용자가 앱을 시작할 때 앱의 응답 방식 이해

  >앱에서 알림을 수신하도록 등록한 후 [개발자 센터에서 앱 고객에게 푸시 알림을 보내고](../publish/send-push-notifications-to-your-apps-customers.md) 나면 사용자가 푸시 알림에 대한 응답으로 앱을 시작할 때 앱에서 다음 진입점 중 하나가 호출됩니다. 사용자가 앱을 시작할 때 실행할 코드 중 일부가 있는 경우에는 앱에서 이러한 진입점 중 하나에 코드를 추가할 수 있습니다.

  >* 푸시 알림이 포그라운드 활성화 유형인 경우 프로젝트에서 **App** 클래스의 [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) 메서드를 재정의하고 이 메서드에 코드를 추가합니다.

  >* 푸시 알림이 백그라운드 활성화 유형인 경우 [백그라운드 작업](../launch-resume/support-your-app-with-background-tasks.md)을 위해 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드에 코드를 추가합니다.

  >예를 들어 앱의 유료 추가 기능을 구입한 앱 사용자에게 무료 추가 기능 사용 권한을 부여하여 사용자에게 보상하길 원할 수 있습니다. 이 경우 이러한 사용자를 대상으로 하는 [고객층](../publish/create-customer-segments.md)에게 푸시 알림을 보낼 수 있습니다. 그런 다음 위에 나열된 진입점 중 하나에서 [앱에서 바로 구매](in-app-purchases-and-trials.md) 무료 사용 권한을 부여하도록 코드를 추가할 수 있습니다.

## <a name="notify-dev-center-of-your-app-launch"></a>개발자 센터에 앱 시작 알림

개발자 센터 푸시 알림에 대해 [Track app launch rate](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx)(앱 실행 속도 추적) 옵션을 선택한 경우 앱의 적절한 진입점에서 **ParseArgumentsAndTrackAppLaunch** 메서드를 호출하여 푸시 알림에 대한 응답으로 앱이 시작되었음을 개발자 센터에 알립니다.

이 메서드는 앱에 대한 원래 실행 인수도 반환합니다. 개발자 센터 푸시 알림에 대해 앱 시작 속도를 추적하도록 선택하고 나면 불특정 추적 id가 시작 인수에 추가되어 개발자 센터에서 앱 실행 추적이 지원됩니다. 앱에 대한 실행 인수는 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 메서드에 전달해야 합니다. 그러면 이 메서드는 개발자 센터에 추적 ID를 보내고, 시작 인수에서 추적 ID를 제거하고, 코드에 원래 실행 인수를 반환합니다.

이 메서드를 호출하는 방법은 대상 푸시 알림의 활성화 유형에 따라 다릅니다.

* 푸시 알림이 포그라운드 활성화 유형인 경우 앱의 [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) 메서드 재정의에서 이 메서드를 호출하고, 이 메서드에 전달된 [ToastNotificationActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.toastnotificationactivatedeventargs.aspx) 개체에서 사용할 수 있는 인수를 전달합니다. 다음 코드 예제에서는 코드 파일에 **Microsoft.Services.Store.Engagement** 및 **Windows.ApplicationModel.Activation** 네임스페이스에 대한 **using** 문이 있다고 가정합니다.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* 푸시 알림이 백그라운드 활성화 유형인 경우 [백그라운드 작업](../launch-resume/support-your-app-with-background-tasks.md)에 대한 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 메서드에서 이 메서드를 호출하고 이 메서드에 전달된 [ToastNotificationActionTriggerDetail](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.toastnotificationactiontriggerdetail.aspx) 개체에서 사용할 수 있는 인수를 전달합니다. 다음 코드 예제에서는 코드 파일에 **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** 및 **Windows.UI.Notifications** 네임스페이스에 대한 **using** 문이 있다고 가정합니다.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

## <a name="unregister-for-push-notifications"></a>푸시 알림 등록 취소

앱에서 대상이 지정된 Windows 개발자 센터 푸시 알림 수신을 중지하려면 [UnregisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 메서드를 호출합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

이 메서드는 앱에서 *모든* 서비스의 푸시 알림을 받지 않도록 알림에 사용되고 있는 채널을 무효화합니다. 알림이 종료되면 채널은 대상이 지정된 Windows 개발자 센터 푸시 알림 및 WNS를 사용하는 다른 알림을 포함하여 어떤 서비스에 대해서도 다시 사용할 수 없게 됩니다. 이 앱에 푸시 알림 보내기를 다시 시작하려면 앱에서 새 채널을 요청 해야합니다.

## <a name="related-topics"></a>관련 항목

* [앱의 고객에게 푸시 알림 보내기](../publish/send-push-notifications-to-your-apps-customers.md)
* [WNS(Windows 푸시 알림 서비스) 개요](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [알림 채널 요청, 만들기 및 저장 방법](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868221)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Dec16_HO1-->


