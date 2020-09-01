---
description: 보류 중인 업데이트 활성화와 함께 알림 기능을 사용 하 여 알림 메시지에 다중 단계 상호 작용을 만드는 방법에 대해 알아봅니다.
title: 업데이트 활성화가 보류 중인 알림
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 보류 중인 업데이트, pendingupdate, 다단계 상호 작용, 다중 단계 상호 작용
ms.localizationpriority: medium
ms.openlocfilehash: 00551414fbefe5591813731337653964bd2524f3
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054543"
---
# <a name="toast-with-pending-update-activation"></a>업데이트 활성화가 보류 중인 알림

**PendingUpdate** 를 사용 하 여 알림 메시지에 다중 단계 상호 작용을 만들 수 있습니다. 예를 들어 아래와 같이 후속 알림을이 이전 알림을의 응답에 의존 하는 일련의 알림을을 만들 수 있습니다.

![업데이트 보류 중인 알림](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **데스크톱 수준 작성자 업데이트 및 알림 라이브러리 2.0.0 필요**: 보류 중인 업데이트 작업을 보려면 데스크톱 빌드 16299 이상을 실행 해야 합니다. 2.0.0 버전 이상의 [UWP Community Toolkit Notification NuGet 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 사용 하 여 단추에 **PendingUpdate** 을 할당 해야 합니다. **PendingUpdate** 는 데스크톱 에서만 지원 되며 다른 장치에서 무시 됩니다.


## <a name="prerequisites"></a>필수 구성 요소

이 문서에서는 다음에 대 한 실무 지식이 있다고 가정 합니다.

- [알림 콘텐츠 생성](adaptive-interactive-toasts.md)
- [알림 보내기 및 백그라운드 활성화 처리](send-local-toast.md)


## <a name="overview"></a>개요

보류 중인 업데이트를 after 활성화 동작으로 사용 하는 알림을 구현 하려면 ...

1. 알림 백그라운드 활성화 단추에서 **AfterActivationBehavior** 의 **PendingUpdate** 를 지정 합니다.

2. 알림 메시지를 보낼 때 **태그** (및 필요에 따라 **그룹**) 할당

3. 사용자가 단추를 클릭 하면 백그라운드 작업이 활성화 되 고 알림이 보류 중인 업데이트 상태로 화면에 유지 됩니다.

4. 백그라운드 작업에서 동일한 **태그** 및 **그룹** 을 사용 하 여 새 콘텐츠를 포함 하는 새 알림 메시지를 보냅니다.


## <a name="assign-pendingupdate"></a>PendingUpdate 할당

백그라운드 활성화 단추에서 **AfterActivationBehavior** 를 **PendingUpdate**로 설정 합니다. 이는 **배경이** **ActivationType** 된 단추에만 적용 됩니다.

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


## <a name="use-a-tag-on-the-notification"></a>알림에 태그 사용

나중에 알림을 교체 하기 위해 알림에 **태그** (및 필요에 따라 **그룹**)를 할당 해야 합니다.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>알림 메시지를 새 콘텐츠로 바꾸기

사용자가 단추를 클릭 하면 백그라운드 태스크가 트리거되고 알림을 새 내용으로 바꾸어야 합니다. 동일한 **태그** 및 **그룹**을 사용 하 여 새 알림 메시지를 전송 하 여 알림을 대체 합니다.

사용자가 이미 알림과 상호 작용 하 고 있으므로 단추 클릭에 대 한 응답으로 대체에서 **오디오를 자동으로 설정** 하는 것이 좋습니다.

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