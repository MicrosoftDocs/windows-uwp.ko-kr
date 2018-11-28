---
Description: Learn how to use Universal Dismiss on your toast notifications.
title: 유니버설 해제
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, uwp, 알림, 클라우드의 알림 센터, 유니버셜 해제, 알림, 크로스 디바이스, 한 번 해제 모든 경우에 해제
ms.localizationpriority: medium
ms.openlocfilehash: 0dc87e8856e35d60660c2643b70b820b2857b488
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7835179"
---
# <a name="universal-dismiss"></a>유니버설 해제

클라우드의 알림 센터를 통해 작동되는 유니버셜 해제는 한 디바이스에서 알림을 해제할 때 다른 디바이스에서도 동일한 알림이 해제된다는 것을 의미합니다.

> [!IMPORTANT]
> **1주년 업데이트 필요**: 유니버셜 해제를 사용하려면 SDK 14393을 대상으로 하고 빌드 14393 이상을 실행하고 있어야 합니다.

이 시나리오의 일반적인 예는 캘린더 미리 알림입니. 두 디바이스 모두에서 일정 앱을 사용할 수 있습니다. 휴대폰과 데스크톱에서 미리 알림을 받습니다. 데스크톱에서 해제를 클릭합니다. 유니버셜 해제 덕분에 휴대폰의 미리 알림도 해제됩니다! **유니버셜 해제를 활성화하려면 코드 한 줄만 추가하면 됩니다!**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

이 시나리오에서 중요한 사실은 **여러 디바이스에 동일한 앱이 설치되어 있으므로** **각 디바이스는 이미 알림을 수신하고 있다**는 점입니다. 일반적으로 일정 앱은 Windows PC와 휴대폰에 동일하게 설치되고 각 앱 인스턴스가 이미 각 디바이스에서 미리 알림을 전송하기 때문에 일정 앱이 대표적인 예가 됩니다. 유니버셜 해제 지원을 추가하면 동일한 미리 알림의 인스턴스가 여러 디바이스에서 연결될 수 있습니다.


## <a name="how-to-enable-universal-dismiss"></a>유니버셜 해제를 사용하는 방법

개발자로서 유니버셜 해제를 사용하는 것은 매우 쉽습니다. 사용자가 한 디바이스에서 알림을 해제할 때 연결된 해당 알림은 다른 디바이스에서 해제되도록 전체 디바이스에서 각 알림을 연결해 주는 ID를 간단히 제공하기만 하면 됩니다.

![유니버설 해제 RemoteId 다이어그램](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: *여러 디바이스*에서 고유하게 식별하는 식별자입니다.

RemoteId를 추가할 한 줄의 코드만이 있으면 유니버설 해제 지원을 사용할 수 있습니다! RemoteId 생성 방법은 사용자에 따라 다르지만, 여러 디바이스에서 알림을 고유하게 식별해야 하고 여러 디바이스에서 실행되는 앱의 다른 인스턴스에서 동일한 식별자를 생성할 수 있어야 합니다.

예를 들어 숙제 플래너 앱에서 RemoteId를 생성하기 위해 "미리 알림" 유형이라고 지정한 다음 숙제 항목의 온라인 계정 ID와 온라인 식별자를 포함시킵니다. 이러한 온라인 ID는 여러 디바이스에서 공유되므로 알림을 보내는 디바이스와 상관없이 정확히 동일한 RemoteId를 일관되게 생성할 수 있습니다.

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

다음 코드는 내 전화 및 데스크톱 앱에서 실행되므로 두 디바이스의 알림에 동일한 RemoteId가 있음습니다.

이렇게 간단합니다! 사용자가 알림을 해제(또는 클릭)하면 RemoteId가 있는지 확인하고, 있으면 모든 사용자의 디바이스 전체에서 RemoteId를 해제시킵니다.

**알려진 문제**: `ToastNotificationHistory.GetHistory()` API를 통해 **RemoteId**를 검색하면 항상 지정한 **RemoteId**보다는 빈 문자열을 반환합니다. 모든 기능이 잘 작동하므로 걱정하지 마세요. 손상된 값만을 검색합니다.

> [!NOTE]
> 사용자 또는 기업이 앱에 대해 [알림 미러링](notification-mirroring.md)을 비활성화하거나 알림 미러링을 완전히 비활성화하면 클라우드에 알림이 없으므로 유니버셜 해제가 작동하지 않습니다.


## <a name="supported-devices"></a>지원되는 디바이스

1주년 업데이트 이후 Windows Mobile 및 Windows Desktop에서 유니버설 해제가 지원됩니다. 유니버설 해제는 PC-PC, PC-휴대폰, 휴대폰-휴대폰 등 양방향으로 작동합니다.