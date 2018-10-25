---
author: andrewleader
Description: Learn how to create multi-step interactions in your notifications.
title: 보류 중인 업데이트 활성화를 적용한 알림
label: Toast with pending update activation
template: detail.hbs
ms.author: mijacobs
ms.date: 12/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 알림, 보류 중인 업데이트, 대기 중 업데이트, 다단계 대화형 작업, 다단계 조작
ms.localizationpriority: medium
ms.openlocfilehash: f5efccbb73758d0e6541e59812801c22a22c87b5
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5516246"
---
# <a name="toast-with-pending-update-activation"></a>보류 중인 업데이트 활성화를 적용한 알림

**PendingUpdate**를 사용하여 알림 메시지 내에서 다단계 상호 작용을 만들 수 있습니다. 예를 들어 아래와 같이 이전 알림의 응답에 따라 후속 알림이 달라지는 일련의 알림을 만들 수 있습니다.

![대기 중인 업데이트 알림](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **알림 라이브러리의 Desktop Fall Creators Update 및 2.0.0 필요**: 보류 중인 업데이트 작업을 보려면 데스크톱 빌드 16299 이상을 실행해야 합니다. 버전 2.0.0 이상의 [UWP 커뮤니티 도구 키트 알림 NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용해야만 단추에서 **PendingUpdate**를 할당할 수 있습니다. **PendingUpdate**는 데스크톱에서만 지원되며 다른 디바이스에서는 무시됩니다.


## <a name="prerequisites"></a>필수 구성 요소

이 문서에서는 다음 실무 지식을 알고 있다고 가정합니다.

- [알림 콘텐츠 생성](adaptive-interactive-toasts.md)
- [알림 보내기 및 백그라운드 활성화 처리](send-local-toast.md)


## <a name="overview"></a>개요

보류 중인 업데이트를 활성화 후 동작으로 사용하는 알림을 구현하려면...

1. 알림 백그라운드 활성화 단추에서 **PendingUpdate**의 **AfterActivationBehavior** 지정

2. 알림을 보낼 때 **Tag**(그리고 선택적으로 **Group**) 할당

3. 사용자가 단추를 클릭하면 백그라운드 작업이 활성화되고 알림은 보류 중인 업데이트 상태로 화면에 유지됨

4. 백그라운드 작업에서 동일한 **Tag** 및 **Group**을 사용하여 새 콘텐츠로 새 알림 보내기


## <a name="assign-pendingupdate"></a>PendingUpdate 할당

백그라운드 활성화 단추에서 **AfterActivationBehavior**를 **PendingUpdate**로 설정 이것은 **Background**의 **ActivationType**이 있는 단추에서만 작동합니다.

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>알림에서 태그 사용

나중에 통지를 대체하기 위해 통지에 **Tag**(그리고 선택적으로 **Group**)를 지정해야 합니다.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>새 콘텐츠로 알림 바꾸기

사용자가 버튼을 클릭하면 백그라운드 작업이 시작되므로 알림을 새 콘텐츠로 바꿔야 합니다. 동일한 **Tag** 및 **Group**을 사용하여 새로운 알림을 보내기만 하면 알림이 바뀝니다.

사용자가 이미 알림과 상호 작용하고 있기 때문에 버튼 클릭에 대한 응답으로 변경 시 **오디오를 무음으로 설정**하는 것이 가장 좋습니다.

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>관련 항목

- [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [로컬 알림 보내기 및 활성화 처리](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
- [알림 진행률 표시줄](toast-progress-bar.md)