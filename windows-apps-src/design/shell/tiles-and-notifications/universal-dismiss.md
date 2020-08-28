---
title: 유니버설 해제
description: 유니버설 해제를 사용 하 여 한 장치에서 알림 메시지를 해제 하 고 다른 장치에서 동일한 알림이 해제 되도록 하는 방법에 대해 알아봅니다.
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 클라우드의 동작 센터, 유니버설 해제, 알림, 교차 장치, 모든 곳에서 해제 한 후 해제
ms.localizationpriority: medium
ms.openlocfilehash: fff9315b9a5645d8a981ca899c1c37acb04de07c
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043455"
---
# <a name="universal-dismiss"></a>유니버설 해제

클라우드의 관리 센터에서 제공 하는 유니버설 해제는 한 장치에서 알림을 해제할 때 다른 장치 에서도 동일한 알림이 해제 됨을 의미 합니다.

> [!IMPORTANT]
> **기념일 업데이트 필요**: 유니버설 해제를 사용 하려면 SDK 14393을 대상으로 하 고 빌드 14393 이상을 실행 해야 합니다.

이 시나리오의 일반적인 예는 일정 미리 알림 ... 두 장치 모두에 일정 앱이 있습니다 ... 휴대폰 및 바탕 화면에 대 한 미리 알림을 받게 됩니다. 바탕 화면에서 해제를 클릭 합니다. 유니버설 해제 덕분에 휴대폰에 대 한 미리 알림이 해제 됩니다. **유니버설 해제를 사용 하도록 설정 하려면 코드 한 줄만 있으면 됩니다.**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

이 시나리오에서 핵심 사실은 **동일한 앱이 여러 장치에 설치**되어 있음을 의미 합니다. 즉, **각 장치가 이미 알림을 수신**하 고 있는 것입니다. 일정 앱은 아이콘 예제입니다. 일반적으로 동일한 일정 앱을 Windows PC와 휴대폰에 모두 설치 하 고 앱의 각 인스턴스는 각 장치에서 미리 알림을 보냅니다. 유니버설 해제에 대 한 지원을 추가 하 여 동일한 미리 알림의 인스턴스를 장치 간에 연결할 수 있습니다.


## <a name="how-to-enable-universal-dismiss"></a>유니버설 해제를 사용 하도록 설정 하는 방법

개발자는 유니버설 해제를 사용 하는 것이 매우 쉽습니다. 사용자가 한 장치에서 알림을 해제할 때 다른 장치에서 해당 연결 된 알림이 해제 되도록 장치에 각 알림을 연결할 수 있는 ID를 제공 하기만 하면 됩니다.

![유니버설 해제 RemoteId 다이어그램](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: *장치에서*알림을 고유 하 게 식별 하는 식별자입니다.

t는 RemoteId를 추가 하는 코드 줄 하나를 사용 하 여 유니버설 해제를 지원 합니다. RemoteId를 생성 하는 방법은 사용자가 결정 해야 합니다. 그러나 장치 간에 알림을 고유 하 게 식별 하 고 다른 장치에서 실행 되는 앱의 다른 인스턴스에서 동일한 식별자를 생성할 수 있는지 확인 해야 합니다.

예를 들어 내 숙제 planner 앱에서 RemoteId는 "미리 알림" 유형이 며, 여기에는 온라인 계정 ID와 숙제 항목의 온라인 식별자가 포함 되어 있습니다. 알림을 전송 하는 장치에 관계 없이 동일한 RemoteId를 일관 되 게 생성할 수 있습니다. 이러한 온라인 Id는 장치에서 공유 되기 때문입니다.

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

다음 코드는 내 휴대폰 및 데스크톱 앱에서 실행 되며,이는 두 장치의 알림이 동일한 RemoteId을 갖게 된다는 것을 의미 합니다.

모든 작업을 수행 해야 합니다. 사용자가 알림을 해제 하거나 클릭 하면 RemoteId 있는지 확인 하 고, 그럴 경우 모든 사용자의 장치에서 해당 RemoteId의 해제를 팬 합니다.

**알려진 문제**: API를 통해 **RemoteId** 를 검색 하면 `ToastNotificationHistory.GetHistory()` 지정 된 **RemoteId** 대신 항상 빈 문자열이 반환 됩니다. 모든 것이 작동 하는 것은 걱정 하지 마세요. 손상 된 값만 검색 하는 것입니다.

> [!NOTE]
> 사용자 또는 엔터프라이즈에서 앱에 대 한 [알림 미러링을](notification-mirroring.md) 사용 하지 않도록 설정 하는 경우 (또는 알림 미러링을 완전히 사용 하지 않도록 설정 하는 경우) 클라우드에 알림이 없기 때문에 유니버설 해제가 작동 하지 않습니다.


## <a name="supported-devices"></a>지원되는 디바이스

기념일 업데이트부터 유니버설 해제는 Windows Mobile 및 Windows Desktop에서 지원 됩니다. 유니버설 해제는 PC PC, PC 전화, 휴대폰 및 휴대폰 사이에서 양방향으로 작동 합니다.