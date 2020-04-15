---
Description: 나중에 표시 되도록 로컬 알림 메시지를 예약 하는 방법에 대해 알아봅니다.
title: 알림 메시지 예약
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10, uwp, 예약 된 알림, scheduledtoastnotification, 방법, 빠른 시작, 시작, 코드 샘플, 연습
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386432"
---
# <a name="schedule-a-toast-notification"></a>알림 메시지 예약

예약 된 알림 메시지를 사용 하면 해당 시간에 앱이 실행 되 고 있는지 여부에 관계 없이 나중에 알림이 표시 되도록 예약할 수 있습니다. 이는 사용자에 대 한 미리 알림 또는 기타 후속 작업을 표시 하는 등의 시나리오에 유용 합니다 .이 경우 알림의 시간과 콘텐츠가 미리 알려집니다.

예약 된 알림 메시지의 배달 기간은 5 분입니다. 예약 된 배달 시간 동안 컴퓨터가 꺼져 있고 5 분 넘게 꺼진 상태를 유지 하는 경우 사용자와 더 이상 관련이 없는 것 처럼 알림이 "삭제" 됩니다. 컴퓨터가 꺼진 기간에 관계 없이 알림의 배달이 보장 되어야 하는 경우 [이 코드 샘플](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)에 나와 있는 것 처럼 시간 트리거와 함께 백그라운드 작업을 사용 하는 것이 좋습니다.

> [!IMPORTANT]
> 데스크톱 응용 프로그램 (MSIX/sparse 패키지 및 클래식 Win32)에는 알림을 보내고 활성화를 처리 하는 단계가 약간 다릅니다. 아래 지침을 따릅니다. 하지만 [데스크톱 앱](toast-desktop-apps.md) 설명서에서 `ToastNotificationManager` `DesktopNotificationManagerCompat` 클래스로 바꿉니다.

> **중요 한 api**: [ScheduledToastNotification 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>필수 조건

다음은 이 문서의 내용을 완전히 이해하는 데 도움이 되는 항목입니다.

* 알림 메시지 용어 및 개념에 관한 실무 지식 자세한 내용은 [알림 및 알림 센터 개요](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)를 참조 하세요.
* Windows 10 알림 메시지 콘텐츠에 대한 기본적인 지식 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)를 참조하세요.
* Windows 10 UWP 앱 프로젝트 만들기


## <a name="install-nuget-packages"></a>NuGet 패키지 설치

프로젝트에 다음 2개의 NuGet 패키지를 설치하는 것이 좋습니다. 코드 샘플에서 이러한 패키지를 사용합니다.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): 원시 XML 대신 개체를 통해 알림 페이로드를 생성합니다.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): C#으로 쿼리 문자열 생성 및 구문 분석


## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가

`Windows.UI.Notifications`에는 알림 Api가 포함 됩니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>알림 콘텐츠 생성

Windows 10에서는 알림 모양을 유연하게 지정할 수 있게 하는 적응형 언어를 사용하여 알림 메시지 콘텐츠를 설명합니다. 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)를 참조하세요.

알림 라이브러리 덕분에 XML 콘텐츠를 생성 하는 것은 간단 합니다. NuGet에서 알림 라이브러리를 설치하지 않는 경우 XML을 수동으로 구성해야 합니다. 이는 오류의 여지를 남깁니다.

**Launch** 속성을 항상 설정해야 합니다. 사용자가 알림 본문을 눌러 앱을 시작하면 앱은 표시해야 하는 콘텐츠를 인식합니다.

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = title
                },
     
                new AdaptiveText()
                {
                    Text = content
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>예약 된 알림 만들기

알림 콘텐츠를 초기화 한 후에는 새 [ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) 을 만들고 콘텐츠의 XML을 전달 하 고 알림을 배달 하려는 시간을 전달 합니다.

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>알림에 대한 기본 키 제공

예약 된 알림을 프로그래밍 방식으로 취소, 제거 또는 교체 하려는 경우에는 Tag 속성 (필요한 경우 그룹 속성)을 사용 하 여 알림에 대 한 기본 키를 제공 해야 합니다. 그런 다음 나중에이 기본 키를 사용 하 여 알림을 취소, 제거 또는 바꿀 수 있습니다.

이미 배달한 알림 메시지 바꾸기/제거에 대한 자세한 내용은 [빠른 시작: 관리 센터에서 알림 메시지 관리(XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))를 참조하세요.

결합된 Tag와 Group은 복합 기본 키 역할을 합니다. Group은 "wallPosts", "messages", "friendRequests"와 같은 그룹에 할당할 수 있는 더 일반적인 식별자이고, Tag는 그룹 내에서 알림을 고유하게 식별해야 합니다. 그러면 일반 그룹을 사용하여 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)로 해당 그룹에서 모든 알림을 제거할 수 있습니다.

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>알림 예약

마지막으로, [to ](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) notification을 만들고 예약 된 알림 메시지를 전달 하 여 고 간에 일정 ()을 호출 합니다.

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>예약 된 알림 취소

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
* [ScheduledToastNotification 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
