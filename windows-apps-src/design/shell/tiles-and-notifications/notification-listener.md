---
Description: Learn how to use Notification Listener to access all of the user's notifications.
title: 알림 수신기
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, uwp, 알림 수신기, usernotificationlistener, 설명서, 액세스 알림
ms.localizationpriority: medium
ms.openlocfilehash: de1032eb3d0d364a62beff0a1af8f84240c11d87
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8807465"
---
# <a name="notification-listener-access-all-notifications"></a>알림 수신기: 모든 알림에 액세스

알림 수신기는 사용자의 알림에 대한 액세스를 제공합니다. 스마트 워치 및 기타 착용식 장치는 알림 수신기를 사용하여 휴대 전화의 알림을 착용식 장치로 전송할 수 있습니다. 홈 자동화 앱 깜박이게은 호출 수신 시 표시등이 만드는 등, 알림 수신 시 특정 작업을 수행 하려면 알림 수신기를 사용할 수 있습니다. 

> [!IMPORTANT]
> **1주년 업데이트 필요**: 알림 수신기를 사용하려면 SDK 15063을 대상으로 하고 빌드 14393 이상을 실행하고 있어야 합니다.


> **중요 API**: [UserNotificationListener 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [UserNotificationChangedTrigger 클래스](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>사용자 알림 기능을 추가하여 수신기 활성화 

알림 수신기를 사용하려면 사용자 알림 수신기 기능을 앱 매니페스트에 추가해야 합니다.

1. Visual Studio의 솔루션 탐색기에서 `Package.appxmanifest` 파일을 두 번 클릭하여 매니페스트 디자이너를 엽니다.
2. 기능 탭을 엽니다.
3. **사용자 알림 수신기** 기능을 선택합니다.


## <a name="check-whether-the-listener-is-supported"></a>수신기 지원 여부를 선택합니다.

앱에서 이전 버전의 Windows 10을 지원하는 경우 [ApiInformation 클래스](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation)를 사용해 수신기 지원 여부를 확인해야 합니다.  수신기가 지원되지 않는 경우 수신기 API에 대한 호출을 실행하지 마세요.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>수신기에 대한 액세스 요청

수신기는 사용자의 알림에 대한 액세스를 허용하므로, 사용자는 알림에 액세스할 수 있는 권한을 앱에 부여해야 합니다. 앱을 처음 실행하는 동안 알림 수신기를 사용할 수 있는 액세스 권한을 요청해야 합니다. 필요에 따라 [RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync)를 호출하기 전에 앱이 사용자의 알림에 액세스해야 하는 이유가 설명된 예비 UI를 표시하여, 사용자가 액세스를 허용해야 하는 이유를 납득하도록 할 수 있습니다.

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

사용자는 Windows 설정을 통해 언제든 액세스 권한을 취소할 수 있습니다. 따라서 앱은 항상 알림 수신기를 사용 하는 코드를 실행 하기 전에, [GetAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) 메서드를 통해 액세스 상태를 확인 합니다. 사용자가 액세스 권한을 취소하면 예외가 throw되는 대신 API가 자동으로 실패합니다(예: API를 사용하여 모든 알림을 가져오려 하면 빈 목록이 반환됨).


## <a name="access-the-users-notifications"></a>사용자의 알림에 액세스

알림 수신기를 사용하면 사용자의 현재 알림 목록을 가져올 수 있습니다. [GetNotificationsAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) 메서드를 호출하고, 가져오고자 하는 알림 유형을 지정하기만 하면 됩니다(현재 지원되는 유일한 알림 유형은 알림 메시지).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>알림 표시

각각의 알림은 알림이 생성된 앱, 알림이 생성된 시간, 알림의 ID 및 알림 자체에 대한 정보를 제공하는 [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification)으로 표시됩니다.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

[AppInfo](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.AppInfo) 속성은 알림을 표시하는 데 필요한 정보를 제공합니다.

> [!NOTE]
> 단일 알림을 캡처할 때 예기치 않은 예외가 발생하는 경우를 대비하여 try/catch에서 단일 알림을 처리하기 위한 모든 코드를 감싸는 것이 좋습니다. 특정 알림 하나에 발생한 문제로 인해 다른 알림을 표시하는 데 완전히 실패해서는 안 됩니다.

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

알림 텍스트와 같은 알림 자체의 내용은 [알림](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.Notification) 속성에 포함되어 있습니다. 이 속성에는 알림의 시각적 부분도 포함되어 있습니다. (Windows에서 알림을 보내는 데 익숙하다면 [알림](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification)> 개체의 [Visual](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification.Visual) 및 [Visual.Bindings](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationvisual.Bindings) 속성이 알림 발생 시 개발자가 보내는 것과 동일하다는 것을 알 수 있을 것입니다.)

알림 바인딩을 찾아야 한다고 가정하겠습니다(코드에 오류가 없으려면 바인딩이 null이 아님을 확인해야 함). 바인딩에서 텍스트 요소를 가져올 수 있습니다. 원하는 개수의 텍스트 요소를 표시하기로 선택할 수 있습니다. (모두 표시하는 것이 가장 이상적입니다.) 텍스트 요소를 다르게 처리하도록 선택할 수 있습니다. 예를 들어 첫 번째 텍스트를 제목 텍스트로 처리하고 후속 요소를 본문 텍스트로 처리합니다.

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>특정 알림 제거

착용식 장치 또는 서비스에서 사용자가 알림을 닫도록 허용하는 경우, 실제 알림을 제거하여 사용자가 나중에 휴대 전화나 PC에서 해당 알림을 볼 수 없도록 할 수 있습니다. 제거하려는 알림의 알림 ID([UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification) 개체에서 가져옴)를 입력하면 됩니다. 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>모든 알림 지우기

[UserNotificationListener.ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) 메서드는 사용자의 모든 알림을 지웁니다. 이 메서드는 신중하게 사용해야 합니다. 착용식 장치 또는 서비스가 모든 알림을 표시하는 경우에만 모든 알림을 지워야 합니다. 착용식 장치나 서비스가 특정 알림만 표시하는 경우, 사용자는 '알림 지우기' 버튼을 클릭할 때 해당 알림만 제거될 것으로 예상합니다. 그러나 [ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) 메서드를 호출하면 착용식 장치나 서비스에 표시되지 않는 알림을 포함하여 모든 알림이 실제로 제거됩니다.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>추가/해제된 알림의 백그라운드 작업

앱이 알림을 수신할 수 있게 하는 일반적인 방법은 백그라운드 작업을 설정하여 앱의 현재 실행 여부에 관계없이 알림이 추가 또는 해제된 시기를 알아내는 것입니다.

기념일 업데이트에 추가된 [단일 프로세스 모델](../../../launch-resume/create-and-register-an-inproc-background-task.md) 덕분에 매우 간단하게 백그라운드 작업을 추가할 수 있습니다. 메인 앱의 코드에서 알림 수신기에 대한 사용자의 액세스 권한을 얻고 백그라운드 작업을 실행할 수 있는 액세스 권한을 얻은 다음, 새 백그라운드 작업을 등록하고 [알림 메시지 종류](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationkinds)를 사용하여 [UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger)를 설정합니다.

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

그런 다음, App.xaml.cs에서 아직 [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) 메서드를 재정의하지 않았다면 재정의하고, 작업 이름에 switch 문을 사용하여 많은 백그라운드 작업 트리거 중 어떤 것이 호출되었는지 확인합니다.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

백그라운드 작업은 단순히 "Shoulder tap"일 뿐이며 추가되거나 제거된 특정 알림에 대한 정보는 제공하지 않습니다. 백그라운드 작업이 트리거되면 착용식 장치에 있는 알림이 플랫폼의 알림을 반영하도록 동기화해야 합니다. 이렇게 하면 백그라운드 작업이 실패하더라도 다음에 백그라운드 작업을 실행할 때 착용식 장치에 대한 알림이 복구될 수 있습니다.

`SyncNotifications` 은(는) 구현하는 메서드로, 다음 섹션에서 자세히 설명합니다. 


## <a name="determining-which-notifications-were-added-and-removed"></a>어떤 알림이 추가되고 제거되었는지 확인

`SyncNotifications` 메서드에서 어떤 알림이 추가되거나 제거되었는지 확인하려면(착용식 장치와 알림 동기화) 현재 알림 컬렉션과 플랫폼에 표시된 알림 사이의 델타를 계산해야 합니다.

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Otherwise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>추가/해제된 알림의 포그라운드 이벤트

> [!IMPORTANT] 
> 알려진 문제: 포그라운드 이벤트가 CPU 루프 하면 최신 버전의 Windows 및 이전에 그 전에 작동 하지 않습니다. 포그라운드 이벤트를 사용 하지 마세요. Windows에 대 한 향후 업데이트를에서는이 해결 됩니다.

포그라운드 이벤트를 사용 하는 대신 [단일 프로세스 모델](../../../launch-resume/create-and-register-an-inproc-background-task.md) 백그라운드 작업에 대 한 앞의 코드를 사용 합니다. 백그라운드 작업은 변경 이벤트 알림의 받을 두 앱이 종료 되거나 실행 중인 동안 수 있습니다.

```csharp
// Subscribe to foreground event (DON'T USE THIS)
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // NOTE: This event WILL CAUSE CPU LOOPS, DO NOT USE. Use the background task instead.
}
```


## <a name="howto-fixdelays-in-the-background-task"></a>백그라운드 작업에서 fixdelays 방법

앱을 테스트 하는 경우 백그라운드 작업이 때로는 지연 되 고 몇 분 동안 트리거되지 않는 알 수 있습니다. 시스템 설정으로 사용자 토고 프롬프트 지연을 해결 하려면 시스템-> 배터리-> 앱의 한 배터리 사용-> 목록에서 앱을 찾을 선택 하 고 항상 허용 됨 "백그라운드에서."로 설정그러면 백그라운드 작업이 항상 트리거하도록 내에서 약 1 초의 알림 수신 합니다.
