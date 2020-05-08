---
Description: 알림 미러링을 사용 하는 방법에 대해 알아봅니다.
title: 알림 미러링
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 클라우드의 동작 센터, 알림 미러링, 알림, 교차 장치
ms.localizationpriority: medium
ms.openlocfilehash: b897c6574f6cbfe78406d1c624f2e3b7286ef582
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971058"
---
# <a name="notification-mirroring"></a>알림 미러링

클라우드의 알림 미러링을 통해 PC에서 휴대폰의 알림을 볼 수 있습니다.

> [!IMPORTANT]
> **기념일 업데이트 필요**: 알림 미러링 작업을 보려면 빌드 14393 이상을 실행 해야 합니다. 알림 미러링의 앱을 옵트아웃 (opt out) 하려면 SDK 14393를 대상으로 하 여 미러링 Api에 액세스 해야 합니다.

알림 미러링과 Cortana를 사용 하 여 사용자는 자신의 PC에서 편리 하 게 휴대폰의 알림 (Windows Mobile 및 Android)을 받고 작업할 수 있습니다. 개발자는 알림 미러링을 사용 하기 위해 어떠한 작업도 수행할 필요가 없습니다. 미러링이 자동으로 작동 합니다. 메시지 빠른 회신 등의 미러된 알림 단추를 클릭 하면 휴대폰으로 다시 라우팅되고 백그라운드 작업을 호출 하거나 포그라운드 앱을 시작 합니다.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

개발자는 알림 미러링과 관련 하 여 두 가지 이점을 누릴 수 있습니다. 미러된 알림은 서비스에 대 한 더 많은 사용자 참여를 초래 하 고 사용자가 Microsoft Store 데스크톱 앱을 검색 하는 데 도움이 됩니다. 사용자는 Windows 10 desktop에 사용할 수 있는 멋진 Windows 앱이 있다고 모를 수도 있습니다. 사용자가 휴대폰에서 미러된 알림을 받으면 사용자가 Windows 앱을 설치할 수 있는 Microsoft Store에 대 한 알림 메시지를 클릭할 수 있습니다.

미러링은 Windows Phone와 Android 모두에서 작동 합니다. 알림 미러링이 작동 하려면 사용자가 휴대폰 및 데스크톱 모두에서 Cortana에 로그인 해야 합니다.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>앱이 두 장치에 설치 되어 있으면 어떻게 되나요?

사용자의 PC에 앱이 이미 있는 경우 중복 된 알림이 표시 되지 않도록 미러된 전화 알림이 자동으로 음소거 됩니다. 다음 기준에 따라 미러된 알림이 자동으로 음소거 됩니다.

1. PC의 앱이 **동일한 표시 이름 또는 동일한 PFN** (패키지 제품군 이름)으로 존재 합니다.
2. PC 앱에서 알림 메시지를 보냈습니다.

PC 앱에서 아직 알림을 보내지 않은 경우 사용자가 실제로 PC 앱을 아직 시작 하지 않았기 때문에 여전히 전화 알림을 표시 합니다.


## <a name="how-to-opt-out-of-mirroring"></a>미러링을 옵트아웃 하는 방법

Windows 앱 개발자, 엔터프라이즈 및 사용자가 알림 미러링을 사용 하지 않도록 선택할 수 있습니다.

> [!NOTE]
> 미러링을 사용 하지 않도록 설정 하면 [유니버설 해제](universal-dismiss.md)도 사용 하지 않도록 설정 됩니다.


### <a name="as-a-developer-opt-out-an-individual-notification"></a>개발자로 서 개별 알림을 옵트아웃 (opt out) 합니다.

때때로 다른 장치에 미러링되지 않으려는 장치 관련 알림이 있을 수 있습니다. 알림 알림에서 **미러링** 속성을 설정 하 여 특정 알림이 미러링되지 않도록 차단할 수 있습니다. 현재이 미러링 속성은 로컬 알림에 대해서만 설정할 수 있습니다 (WNS 푸시 알림을 보낼 때 지정할 수 없음).

**알려진 문제**: `ToastNotificationHistory.GetHistory()` API를 통해 미러링 속성을 검색 하면 지정 된 옵션 대신 항상 기본값 (**허용 됨**)이 반환 됩니다. 모든 것이 작동 하는 것은 걱정 하지 마세요. 손상 된 값만 검색 하는 것입니다.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>개발자로 서 완전히 옵트아웃

일부 개발자는 알림 미러링의 앱을 완전히 옵트아웃 (opt out) 할 수 있습니다. 모든 앱이 미러링의 이점을 누릴 수 있지만 쉽게 옵트아웃 하 게 됩니다. 다음 메서드를 한 번만 호출 하면 앱이 옵트아웃 됩니다. 예를 들어, 내 `App.xaml.cs`앱의 생성자에이 호출을 추가할 수 있습니다.

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>기업에서 옵트아웃 (opt out) 하려면 어떻게 해야 하나요?

기업은 알림 미러링을 완전히 사용 하지 않도록 선택할 수 있습니다. 이렇게 하려면 그룹 정책을 편집 하 여 알림 미러링을 해제 하면 됩니다.


### <a name="as-a-user-how-do-i-opt-out"></a>사용자는 어떻게 옵트아웃 하나요?

사용자는 개별 앱에서 옵트아웃 (opt out) 하거나 기능을 사용 하지 않도록 설정 하 여 완전히 옵트아웃 (opt out) 할 수 있습니다. 특정 앱의 알림이 데스크톱에 미러링되지 않도록 할 수 있으므로 특정 앱을 사용 하지 않도록 설정할 수 있습니다. 이러한 옵션은 휴대폰 및 PC에서 Cortana의 설정에서 찾을 수 있습니다.
