---
Description: 나중에 표시 되도록 로컬 알림 메시지를 예약 하는 방법에 대해 알아봅니다.
title: 알림 메시지 예약
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10, uwp, 예약 된 알림, scheduledtoastnotification, 방법, 빠른 시작, 시작, 코드 샘플, 연습
ms.localizationpriority: medium
ms.openlocfilehash: 04bbf3da388bf065b2b96684cf3f27cd7534ff51
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984739"
---
# <a name="schedule-a-toast-notification"></a>알림 메시지 예약

예약 된 알림 메시지를 사용 하면 해당 시간에 앱이 실행 되 고 있는지 여부에 관계 없이 나중에 알림이 표시 되도록 예약할 수 있습니다. 이는 사용자에 대 한 미리 알림 또는 기타 후속 작업을 표시 하는 등의 시나리오에 유용 합니다 .이 경우 알림의 시간과 콘텐츠가 미리 알려집니다.

예약 된 알림 메시지의 배달 기간은 5 분입니다. 예약 된 배달 시간 동안 컴퓨터가 꺼져 있고 5 분 넘게 꺼진 상태를 유지 하는 경우 사용자와 더 이상 관련이 없는 것 처럼 알림이 "삭제" 됩니다. 컴퓨터가 꺼진 기간에 관계 없이 알림의 배달이 보장 되어야 하는 경우 [이 코드 샘플](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)에 나와 있는 것 처럼 시간 트리거와 함께 백그라운드 작업을 사용 하는 것이 좋습니다.

> [!IMPORTANT]
> Win32 응용 프로그램 (MSIX/sparse 패키지 및 클래식 Win32)에는 알림을 보내고 활성화를 처리 하는 단계가 약간 다릅니다. 그러나 아래 지침을 따릅니다. 하지만을 `ToastNotificationManager` `DesktopNotificationManagerCompat` [Win32 앱](toast-desktop-apps.md) 설명서의 클래스로 바꿉니다.

> **중요 한 api**: [ScheduledToastNotification 클래스](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>필수 구성 요소

이 항목을 완벽 하 게 이해 하려면 다음을 수행 하는 것이 좋습니다.

* 알림 메시지 알림 용어 및 개념에 대 한 작업 정보입니다. 자세한 내용은 [알림 및 알림 센터 개요](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10)를 참조 하세요.
* Windows 10 알림 메시지 내용에 대해 잘 알고 있어야 합니다. 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)를 참조 하세요.
* Windows 10 UWP 앱 프로젝트


## <a name="step-1-install-nuget-package"></a>1 단계: NuGet 패키지 설치

[Microsoft Toolkit. 알림 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치 합니다. 코드 샘플에서는이 패키지를 사용 합니다. 문서의 끝 부분에서는 NuGet 패키지를 사용 하지 않는 "일반" 코드 조각을 제공 합니다. 이 패키지를 사용 하면 XML을 사용 하지 않고 알림 메시지를 만들 수 있습니다.


## <a name="step-2-add-namespace-declarations"></a>2 단계: 네임 스페이스 선언 추가

`Windows.UI.Notifications` 알림 Api를 포함 합니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-schedule-the-notification"></a>3 단계: 알림 예약

지금까지 학생 들에 대해 학생 들에 게 알려 주는 간단한 텍스트 기반 알림을 사용 합니다. 알림 및 일정을 생성 합니다!

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("itemsDueToday", ToastActivationType.Foreground)
    .AddText("ASTR 170B1")
    .AddText("You have 3 items due today!");
    .GetToastContent();

    
// Create the scheduled notification
var toast = new ScheduledToastNotification(content.GetXml(), DateTime.Now.AddSeconds(5));


// Add your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="provide-a-primary-key-for-your-toast"></a>알림에 대 한 기본 키 제공

예약 된 알림을 프로그래밍 방식으로 취소, 제거 또는 교체 하려는 경우에는 Tag 속성 (필요한 경우 그룹 속성)을 사용 하 여 알림에 대 한 기본 키를 제공 해야 합니다. 그런 다음 나중에이 기본 키를 사용 하 여 알림을 취소, 제거 또는 바꿀 수 있습니다.

이미 제공 된 알림 메시지를 교체/제거 하는 방법에 대 한 자세한 내용은 빠른 시작: 알림 메시지 [관리 센터 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))를 참조 하세요.

태그 및 그룹 결합은 복합 기본 키로 작동 합니다. Group은 보다 일반적인 식별자로, "wallPosts", "messages", "friendRequests" 등의 그룹을 할당할 수 있습니다. 그런 다음 태그는 그룹 내에서 알림 자체를 고유 하 게 식별 해야 합니다. 일반 그룹을 사용 하면 [REMOVEGROUP API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)를 사용 하 여 해당 그룹에서 모든 알림을 제거할 수 있습니다.

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="cancel-scheduled-notifications"></a>예약된 알림 취소

예약 된 알림을 취소 하려면 먼저 모든 예약 된 알림 목록을 검색 해야 합니다.

그런 다음 앞에서 지정한 태그 (및 필요에 따라 그룹)와 일치 하는 예약 된 알림을 찾고 RemoveFromSchedule ()를 호출 합니다.

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>활성화 처리

활성화 처리에 대 한 자세한 내용은 [로컬 알림 보내기](send-local-toast.md) 문서를 참조 하세요. 예약 된 알림 메시지의 활성화는 로컬 알림 메시지의 활성화와 동일 하 게 처리 됩니다.


## <a name="adding-actions-inputs-and-more"></a>작업, 입력 등 추가

작업 및 입력과 같은 고급 항목에 대해 자세히 알아보려면 [로컬 알림 보내기](send-local-toast.md) 문서를 참조 하세요. 작업 및 입력은 예약 된 알림을에서와 같이 로컬 알림을에서 동일 하 게 작동 합니다.


## <a name="resources"></a>리소스

* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
* [ScheduledToastNotification 클래스](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)