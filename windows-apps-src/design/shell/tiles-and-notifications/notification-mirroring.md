---
Description: 알림 토스트 알림을에서 미러링을 사용 하는 방법에 알아봅니다.
title: 알림 미러링
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, uwp, 알림, 클라우드의 알림 센터, 알림 미러링, 알림, 크로스 디바이스
ms.localizationpriority: medium
ms.openlocfilehash: dc870601159a80bc6d03a287fd19f082e968e09e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657318"
---
# <a name="notification-mirroring"></a>알림 미러링

클라우드의 알림 센터에서 제공하는 알림 미러링을 사용하면 PC에서 사용자 전화의 알림을 볼 수 있습니다.

> [!IMPORTANT]
> **1 주년 업데이트를 설치 해야**: 실행 해야 빌드 14393 이상을 작업을 미러링 하는 알림이 표시 됩니다. 알림 미러링에서 앱을 제외하려면 SDK 14393을 대상으로 하고 미러링 API에 액세스해야 합니다.

알림 미러링과 Cortana를 통해 사용자는 자체 PC의 편의성을 통해 전화 알림(Windows Mobile 및 Android)을 수신하고 이에 대응할 수 있습니다. 개발자는 알림 미러링을 활성화하기 위해 아무 것도 할 필요가 없으며 미러링이 자동으로 작동합니다! 빠른 메시지 회신과 같은 미러링된 알림에 대한 단추를 클릭하면 경로를 전화로 다시 지정하여 백그라운드 작업을 호출하거나 포그라운드 앱을 시작합니다.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

개발자는 알림 미러링에서 두 가지 혜택을 가져옵니다. 미러된 알림 서비스를 사용 하 여 더 많은 사용자 참여 고 데스크톱 Microsoft Store 앱을 검색 하는 사용자는 데도 도움이! 사용자는 Windows 10 데스크톱에서 유용한 UWP 앱을 사용할 수 있는지 조차도 알지 못할 수도 있습니다. 휴대폰에서 미러된 알림을 수신 하는 사용자, 사용자 데스크톱 UWP 앱을 설치할 수 있는 Microsoft Store 이동 하려면 알림 클릭 수 있습니다.

미러링 기능은 Windows Phone과 Android에서 작동합니다. 알림 미러링을 작동하려면 사용자가 전화와 데스크톱에서 Cortana에 로그인해야 합니다.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>앱을 두 디바이스에 모두에 설치하면 어떻게 되나요?

사용자가 이미 PC에서 앱을 실행한 경우 미러링된 전화 알림 음을 자동으로 소거하므로 중복된 알림이 표시되지 않습니다. 미러링된 알림은 다음 기준에 따라 자동으로 음이 소거됩니다...

1. PC의 앱에는 **동일한 표시 이름 또는 동일한 PFN**(패키지 패밀리 이름)이 있습니다.
2. 해당 PC 앱이 알림 메시지를 전송했습니다

PC 앱이 아직 알림을 보내지 않았다면, 실제로 사용자가 PC 앱을 아직 실행하지 않았을 가능성이 높기 때문에 계속 전화 알림이 표시됩니다.


## <a name="how-to-opt-out-of-mirroring"></a>미러링을 옵트아웃하는 방법

UWP 앱 개발자, 기업 및 사용자는 알림 미러링을 비활성화하도록 선택할 수 있습니다.

> [!NOTE]
> 미러링을 비활성화하면 [유니버설 해제](universal-dismiss.md)가 비활성화됩니다.


### <a name="as-a-developer-opt-out-an-individual-notification"></a>개발자가 개별 알림을 옵트아웃

경우에 따라 다른 디바이스에 미러링하지 않을 디바이스별 알림이 있을 수 있습니다. 알림 메시지에서 **Mirroring** 속성을 설정하여 특정 알림이 미러링되지 않도록 할 수 있습니다. 현재이 미러링 속성은 로컬 알림에서만 설정할 수 있습니다(WNS 푸시 알림을 보낼 때 지정할 수 없음).

**알려진 문제**: 통해 미러링 속성을 검색 하는 `ToastNotificationHistory.GetHistory()` API의 기본값을 항상 반환 됩니다 (**허용**) 지정한 옵션을 대신 합니다. 모든 기능이 잘 작동하므로 걱정하지 마세요. 손상된 값만을 검색합니다.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>개발자가 완전히 옵트아웃

일부 개발자는 알림 미러링에서 앱을 완전히 옵트아웃할 수 있습니다. 하지만 모든 앱에서 미러링 이익이 있는지를 쉽게 참여할 수 있습니다. 다음 메서드를 한 번 호출 하기만 하 고 앱을 옵트아웃 됩니다. 예를 들어, 내 앱의 생성자에서이 호출을 배치할 수 있습니다 `App.xaml.cs`...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>기업에서는 어떻게 옵트아웃합니까?

기업에서는 알림 미러링을 완전히 활성화할 수 있습니다. 이 작업을 하려면 간단히 그룹 정책을 편집하여 알림 미러링을 해제합니다.


### <a name="as-a-user-how-do-i-opt-out"></a>사용자는 어떻게 옵트아웃합니까?

사용자는 개별 앱을 옵트아웃하거나 기능을 비활성화하여 완전히 옵트아웃할 수 있습니다. 특정 앱의 알림이 데스크톱에 미러링되지 않도록 하려면 간단히 이 특정 앱을 비활성화하면 됩니다. 이 옵션은 전화와 PC 모두에서 Cortana의 설정에 있습니다.