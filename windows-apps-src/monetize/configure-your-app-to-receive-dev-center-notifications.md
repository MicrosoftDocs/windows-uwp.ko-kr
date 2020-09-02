---
Description: 파트너 센터에서 보내는 푸시 알림을 받기 위해 UWP 앱을 등록 하는 방법에 대해 알아봅니다.
title: 앱에 대상 푸시 알림 구성
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, 대상 푸시 알림, 파트너 센터
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: abb901c1b067dcf3609cbfb5c4cf3f81c9dc465c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364136"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>앱에 대상 푸시 알림 구성

파트너 센터의 **푸시 알림** 페이지를 사용 하 여 유니버설 WINDOWS 플랫폼 (UWP) 앱이 설치 된 장치에 대상 푸시 알림을 전송 하 여 고객과 직접 연결할 수 있습니다. 예를 들어 대상 푸시 알림을 사용 하 여 고객이 앱 등급 지정 또는 새로운 기능 시도와 같은 작업을 수행할 수 있도록 할 수 있습니다. 알림 메시지, 타일 알림 및 원시 XML 알림을 포함 하 여 여러 가지 형식의 푸시 알림을 보낼 수 있습니다. 푸시 알림에서 생성 된 앱 시작의 비율도 추적할 수 있습니다. 이 기능에 대 한 자세한 내용은 [앱 고객에 게 푸시 알림 보내기](../publish/send-push-notifications-to-your-apps-customers.md)를 참조 하세요.

파트너 센터에서 고객에 게 대상 푸시 알림을 보내려면 먼저 Microsoft Store Services SDK에서 [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 클래스의 메서드를 사용 하 여 알림을 수신 하도록 앱을 등록 해야 합니다. 이 클래스의 추가 메서드를 사용 하 여 파트너 센터에 대상 푸시 알림에 대 한 응답으로 앱이 시작 되었음을 알리고 (알림에 의해 발생 된 앱 시작의 요금을 추적 하려는 경우) 알림 수신을 중지할 수 있습니다.

## <a name="configure-your-project"></a>프로젝트 구성

코드를 작성 하기 전에 다음 단계에 따라 프로젝트의 Microsoft Store Services SDK에 대 한 참조를 추가 합니다.

1. 아직 수행 하지 않은 경우 개발 컴퓨터에 [Microsoft Store 서비스 SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk) 합니다. 
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트에 대 한 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 클릭 합니다.
4. **참조 관리자**에서 **유니버설 Windows** 를 확장 하 고 **확장**을 클릭 합니다.
5. Sdk 목록에서 **Microsoft Engagement 프레임 워크** 옆의 확인란을 클릭 하 고 **확인**을 클릭 합니다.

## <a name="register-for-push-notifications"></a>푸시 알림 등록

파트너 센터에서 대상 푸시 알림을 받도록 앱을 등록 하려면:

1. 프로젝트에서 시작 중에 실행 되는 코드 섹션을 찾아 앱을 등록 하 여 알림을 수신 합니다.
2. 코드 파일의 맨 위에 다음 문을 추가 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="EngagementNamespace":::

3. [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) 개체를 가져오고 앞에서 확인 한 시작 코드에서 [Registernotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버 로드 중 하나를 호출 합니다. 앱이 시작 될 때마다이 메서드를 호출 해야 합니다.

  * 파트너 센터가 알림에 대 한 자체 채널 URI를 만들도록 하려면 [Registernotificationchannelasync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버 로드를 호출 합니다.

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync1":::
      > [!IMPORTANT]
      > 앱이 [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 를 호출 하 여 WNS에 대 한 알림 채널을 만드는 경우 코드에서 [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) 및 [registernotificationchannelasync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버 로드를 동시에 호출 하지 않도록 해야 합니다. 이러한 두 메서드를 모두 호출 해야 하는 경우에는 메서드를 순차적으로 호출 하 고 다른 메서드를 호출 하기 전에 메서드 반환을 대기 해야 합니다.

  * 파트너 센터의 대상 푸시 알림에 사용할 채널 URI를 지정 하려면 [Registernotificationchannelasync (StoreServicesNotificationChannelParameters)](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 오버 로드를 호출 합니다. 예를 들어 앱이 이미 WNS (Windows Push Notification Services)를 사용 하 고 동일한 채널 URI를 사용 하려는 경우이 작업을 수행할 수 있습니다. 먼저 [StoreServicesNotificationChannelParameters](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) 개체를 만들고 [customnotificationchanneluri](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) 속성을 채널 URI에 할당 해야 합니다.

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync2":::

> [!NOTE]
> **Registernotificationchannelasync** 메서드를 호출 하면 앱에 대 한 로컬 앱 데이터 저장소 ( [Applicationdata. localfolder](/uwp/api/Windows.Storage.ApplicationData.LocalFolder) 속성에 의해 반환 되는 폴더)에 MicrosoftStoreEngagementSDKId.txt 라는 파일이 만들어집니다. 이 파일에는 대상 푸시 알림 인프라에서 사용 하는 ID가 포함 되어 있습니다. 앱이이 파일을 수정 하거나 삭제 하지 않는지 확인 합니다. 그렇지 않으면 사용자가 알림 인스턴스를 여러 개 받거나 알림이 다른 방법으로 제대로 동작 하지 않을 수 있습니다.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>대상 푸시 알림이 고객에 게 라우팅되는 방법

앱이 **Registernotificationchannelasync**를 호출 하면이 메서드는 현재 장치에 로그인 한 고객의 Microsoft 계정을 수집 합니다. 나중에이 고객을 포함 하는 세그먼트로 대상 푸시 알림을 보내면 파트너 센터에서이 고객의 Microsoft 계정와 연결 된 장치로 알림을 보냅니다.

앱을 시작한 고객이 자신의 장치를 해당 Microsoft 계정로 장치에 로그인 하는 동안 다른 사용자가 사용할 수 있도록 장치를 제공 하는 경우 다른 사용자는 원래 고객에 게 대상이 지정 된 알림을 볼 수 있습니다. 특히 고객이 사용 하기 위해 로그인 할 수 있는 서비스를 제공 하는 앱의 경우 의도 하지 않은 결과가 발생할 수 있습니다. 이 시나리오에서 다른 사용자가 대상 알림을 볼 수 없도록 하려면 고객이 앱에서 로그 아웃할 때 [Unregisternotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 메서드를 호출 합니다. 자세한 내용은이 문서의 뒷부분에 나오는 [푸시 알림에 대 한 등록 취소](#unregister) 를 참조 하세요.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>사용자가 앱을 시작할 때 앱이 응답 하는 방식

알림을 수신 하도록 앱을 등록 하 고 [파트너 센터에서 앱의 고객에 게 푸시 알림을 보내면](../publish/send-push-notifications-to-your-apps-customers.md)앱에서 다음 진입점 중 하나가 푸시 알림에 대 한 응답으로 앱을 시작할 때 호출 됩니다. 사용자가 앱을 시작할 때 몇 가지 코드를 실행 해야 하는 경우 앱의 이러한 진입점 중 하나에 코드를 추가할 수 있습니다.

  * 푸시 알림에 포그라운드 활성화 형식이 있는 경우 프로젝트에서 **App** 클래스의 [onactivated](/uwp/api/windows.ui.xaml.application.onactivated) 메서드를 재정의 하 고 코드를이 메서드에 추가 합니다.

  * 푸시 알림에 백그라운드 활성화 형식이 있는 경우 [백그라운드 작업](../launch-resume/support-your-app-with-background-tasks.md)에 대 한 [실행](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드에 코드를 추가 합니다.

예를 들어 무료 추가 기능을 부여 하 여 앱에서 유료 추가 기능을 구매한 앱의 사용자에 게 보상을 원할 수 있습니다. 이 경우 이러한 사용자를 대상으로 하는 [고객 세그먼트로](../publish/create-customer-segments.md) 푸시 알림을 보낼 수 있습니다. 그런 다음, 위에 나열 된 진입점 중 하나에서 무료 [앱 구매](in-app-purchases-and-trials.md) 를 허용 하는 코드를 추가할 수 있습니다.

## <a name="notify-partner-center-of-your-app-launch"></a>앱 시작의 파트너 센터에 알림

파트너 센터에서 대상 푸시 알림에 대 한 **앱 시작 시간 추적** 옵션을 선택 하는 경우 앱의 적절 한 진입점에서 [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 메서드를 호출 하 여 앱이 푸시 알림에 대 한 응답으로 시작 되었음을 파트너 센터에 알립니다.

또한이 메서드는 앱에 대 한 원래 시작 인수를 반환 합니다. 푸시 알림에 대 한 앱 시작 율을 추적 하도록 선택 하면 시작 인수에 불투명 추적 ID가 추가 되어 파트너 센터에서 앱 시작을 추적 하는 데 도움이 됩니다. 앱의 시작 인수를 [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 메서드에 전달 해야 합니다 .이 메서드는 추적 Id를 파트너 센터에 보내고, 시작 인수에서 추적 id를 제거 하 고, 원래 시작 인수를 코드에 반환 합니다.

이 메서드를 호출 하는 방법은 푸시 알림의 정품 인증 유형에 따라 달라 집니다.

* 푸시 알림이 포그라운드 활성화 유형인 경우 앱의 [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) 메서드 재정의에서 이 메서드를 호출하고, 이 메서드에 전달된 [ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) 개체에서 사용할 수 있는 인수를 전달합니다. 다음 코드 예제에서는 코드 파일이 **Microsoft.** . r **using** o **w** e r. r e s.

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/App.xaml.cs" id="OnActivated":::

* 푸시 알림에 백그라운드 활성화 형식이 있는 경우 [백그라운드 작업](../launch-resume/support-your-app-with-background-tasks.md) 에 대해 [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 메서드에서이 메서드를 호출 하 고이 메서드에 전달 된 [todetail notificationactiontriggerdetail](/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) 개체에서 사용할 수 있는 인수를 전달 합니다. 다음 코드 예제에서는 코드 파일이 **Microsoft.**. r **using** e s e r. l i s e. p r o **w**e. **Windows.UI.Notifications**

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="Run":::

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>푸시 알림 등록 취소

앱이 파트너 센터에서 대상 푸시 알림 받기를 중지 하려면 [Unregisternotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) 메서드를 호출 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="UnregisterNotificationChannelAsync":::

이 메서드는 알림에 사용 되는 채널을 무효화 하므로 앱이 더 이상 *모든* 서비스에서 푸시 알림을 받을 수 없습니다. 이를 닫은 후에는 파트너 센터의 대상 푸시 알림과 WNS를 사용 하는 기타 알림을 비롯 한 모든 서비스에 대해 채널을 다시 사용할 수 없습니다. 이 앱에 푸시 알림 보내기를 다시 시작 하려면 앱에서 새 채널을 요청 해야 합니다.

## <a name="related-topics"></a>관련 항목

* [앱의 고객에게 푸시 알림 보내기](../publish/send-push-notifications-to-your-apps-customers.md)
* [WNS(Windows 푸시 알림 서비스) 개요](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
* [알림 채널을 요청, 생성 및 저장 하는 방법](/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
