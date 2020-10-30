---
description: 알림 수신기를 사용 하 여 모든 사용자의 알림에 액세스 하는 방법에 대해 알아봅니다.
title: 알림 수신기
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: windows 10, uwp, 알림 수신기, usernotificationlistener, 설명서, 액세스 알림
ms.localizationpriority: medium
ms.openlocfilehash: 90d1d0757bad5a62a987144e6a81d0a6550db384
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033666"
---
# <a name="notification-listener-access-all-notifications"></a>알림 수신기: 모든 알림 액세스

알림 수신기는 사용자의 알림에 대 한 액세스를 제공 합니다. Smartwatches 및 기타 착용 식 장치용는 알림 수신기를 사용 하 여 휴대폰의 알림을 wearable 장치로 보낼 수 있습니다. Home automation 앱은 알림을 수신할 때 알림 수신기를 사용 하 여 특정 작업을 수행할 수 있습니다. 예를 들어 통화를 받을 때 표시등이 깜박일 수 있습니다. 

> [!IMPORTANT]
> **기념일 업데이트 필요** : 알림 수신기를 사용 하려면 SDK 14393을 대상으로 하 고 빌드 14393 이상을 실행 해야 합니다.


> **중요 한 api** : [usernotificationlistener 클래스](/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [usernotificationchangedtrigger 클래스](/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>사용자 알림 기능을 추가 하 여 수신기를 사용 하도록 설정 

알림 수신기를 사용 하려면 앱 매니페스트에 사용자 알림 수신기 기능을 추가 해야 합니다.

1. Visual Studio의 솔루션 탐색기에서 파일을 두 번 클릭 `Package.appxmanifest` 하 여 매니페스트 디자이너를 엽니다.
2. 기능 탭을 엽니다.
3. **사용자 알림 수신기** 기능을 확인 합니다.


## <a name="check-whether-the-listener-is-supported"></a>수신기가 지원 되는지 여부를 확인 합니다.

앱에서 이전 버전의 Windows 10을 지 원하는 경우 [Apiinformation 클래스](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 를 사용 하 여 수신기가 지원 되는지 여부를 확인 해야 합니다.  수신기가 지원 되지 않는 경우 수신기 Api에 대 한 호출을 실행 하지 마십시오.

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


## <a name="requesting-access-to-the-listener"></a>수신기에 대 한 액세스 요청

수신기는 사용자의 알림에 대 한 액세스를 허용 하므로 사용자는 자신의 알림에 액세스할 수 있는 권한을 앱에 부여 해야 합니다. 앱의 첫 실행 경험을 사용 하는 동안 알림 수신기를 사용 하기 위한 액세스를 요청 해야 합니다. 원하는 경우 사용자가 액세스를 허용 해야 하는 이유를 이해 하도록 [Requestaccessasync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync)를 호출 하기 전에 앱이 사용자의 알림에 액세스 해야 하는 이유를 설명 하는 몇 가지 예비 UI를 표시할 수 있습니다.

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

사용자는 언제 든 지 Windows 설정을 통해 액세스 권한을 해지할 수 있습니다. 따라서 앱은 알림 수신기를 사용 하는 코드를 실행 하기 전에 항상 [Getaccessstatus](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) 메서드를 통해 액세스 상태를 확인 해야 합니다. 사용자가 액세스를 취소 하면 api가 예외를 throw 하는 대신 자동으로 실패 합니다. 예를 들어 모든 알림을 가져오는 API는 단순히 빈 목록을 반환 합니다.


## <a name="access-the-users-notifications"></a>사용자의 알림 액세스

알림 수신기를 사용 하 여 사용자의 현재 알림 목록을 가져올 수 있습니다. [GetNotificationsAsync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) 메서드를 호출 하 고 가져오려는 알림 유형을 지정 합니다. 현재 지원 되는 알림 유형은 알림 메시지 뿐입니다.

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>알림 표시

각 알림은 알림이 있는 앱에 대 한 정보, 알림이 생성 된 시간, 알림 ID 및 알림 자체에 대 한 정보를 제공 하는 [Usernotification](/uwp/api/windows.ui.notifications.usernotification)로 표시 됩니다.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

[AppInfo](/uwp/api/windows.ui.notifications.usernotification.AppInfo) 속성은 알림을 표시 하는 데 필요한 정보를 제공 합니다.

> [!NOTE]
> 단일 알림을 캡처할 때 예기치 않은 예외가 발생 하는 경우 try/catch에서 단일 알림을 처리 하는 모든 코드를 둘러싸는 것이 좋습니다. 하나의 특정 알림과 관련 된 문제로 인해 다른 알림은 완전히 표시 하지 못할 수 있습니다.

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

알림 텍스트와 같은 알림 자체의 내용은 [알림](/uwp/api/windows.ui.notifications.usernotification.Notification) 속성에 포함 되어 있습니다. 이 속성에는 알림의 시각적 부분이 포함 되어 있습니다. Windows에서 알림을 보내는 방법에 대해 잘 알고 있는 경우 알림 개체의 [시각적](/uwp/api/windows.ui.notifications.notification.Visual) 개체 및 [시각적 개체 바인딩](/uwp/api/windows.ui.notifications.notificationvisual.Bindings) 속성 [은 알림을 볼](/uwp/api/windows.ui.notifications.notification) 때 개발자가 전송 하는 내용에 해당 합니다.

알림 바인딩을 검색 하려고 합니다 (오류 증명 코드의 경우 바인딩이 null이 아님을 확인 해야 함). 바인딩에서 텍스트 요소를 가져올 수 있습니다. 원하는 수 만큼 텍스트 요소를 표시 하도록 선택할 수 있습니다. (이상적으로는 모두 표시 해야 합니다.) 텍스트 요소를 다르게 처리 하도록 선택할 수 있습니다. 예를 들어 첫 번째 항목을 제목 텍스트로, 후속 요소를 본문 텍스트로 처리 합니다.

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

Wearable 또는 서비스에서 사용자가 알림을 해제할 수 있는 경우 실제 알림을 제거 하 여 사용자가 나중에 휴대폰 이나 PC에서이를 볼 수 없도록 할 수 있습니다. 제거 하려는 알림의 알림 ID ( [usernotification](/uwp/api/windows.ui.notifications.usernotification) 개체에서 가져옴)를 제공 하면 됩니다. 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>모든 알림 지우기

[Usernotificationlistener. ClearNotifications](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) 메서드는 모든 사용자의 알림을 지웁니다. 이 메서드는 주의 해 서 사용 합니다. Wearable 또는 서비스가 모든 알림을 표시 하는 경우에만 모든 알림을 지워야 합니다. Wearable 또는 서비스에 특정 알림만 표시 되는 경우 사용자가 "알림 지우기" 단추를 클릭 하면 사용자에 게 특정 알림만 제거 될 것으로 예상 됩니다. 그러나 [clearnotifications](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) 메서드를 호출 하면 wearable 또는 서비스가 표시 하지 않는 알림을 비롯 한 모든 알림이 제거 될 수 있습니다.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>추가/해제 된 알림에 대 한 백그라운드 작업

앱이 알림을 수신할 수 있도록 하는 일반적인 방법은 앱이 현재 실행 중인지 여부에 관계 없이 알림이 추가 되거나 해제 된 시기를 알 수 있도록 백그라운드 작업을 설정 하는 것입니다.

기념일 업데이트에 추가 된 [단일 프로세스 모델](../../../launch-resume/create-and-register-an-inproc-background-task.md) 덕분에 백그라운드 작업을 추가 하는 작업은 매우 간단 합니다. 주 앱의 코드에서 알림 수신기에 대 한 사용자 액세스 권한을 획득 하 고 백그라운드 작업을 실행 하기 위한 액세스 권한을 얻은 후에는 새 백그라운드 작업을 등록 하 고 알림 [메시지 종류](/uwp/api/windows.ui.notifications.notificationkinds)를 사용 하 여 [Usernotificationchangedtrigger](/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) 를 설정 하면 됩니다.

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

그런 다음 App.xaml.cs에서 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) 메서드를 재정의 하 고, 아직 작업 이름에 대해 switch 문을 사용 하 여 여러 백그라운드 작업 트리거를 호출 했는지 확인 합니다.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

백그라운드 작업은 단순히 "어깨 탭" 이며, 추가 되거나 제거 된 특정 알림에 대 한 정보를 제공 하지 않습니다. 백그라운드 작업이 트리거되면 플랫폼의 알림을 반영 하도록 wearable의 알림을 동기화 해야 합니다. 이렇게 하면 백그라운드 작업이 실패 하는 경우 다음에 백그라운드 작업이 실행 될 때 wearable에 대 한 알림을 계속 복구할 수 있습니다.

`SyncNotifications` 는 구현 하는 방법입니다. 다음 섹션에서는 방법을 보여 줍니다. 


## <a name="determining-which-notifications-were-added-and-removed"></a>추가 및 제거 된 알림 확인

`SyncNotifications`메서드에서 추가 또는 제거 된 알림 (wearable와의 동기화 동기화)을 확인 하려면 현재 알림 컬렉션과 플랫폼의 알림 사이의 델타를 계산 해야 합니다.

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


## <a name="foreground-event-for-notification-addeddismissed"></a>추가/해제 된 알림에 대 한 포그라운드 이벤트

> [!IMPORTANT] 
> 알려진 문제: 빌드 17763/10 월 2018 업데이트/버전 1809 이전 빌드에서 포그라운드 이벤트는 CPU 루프가 발생 하거나 작동 하지 않습니다. 이전 빌드에 대 한 지원이 필요한 경우 백그라운드 작업을 대신 사용 합니다.

메모리 내 이벤트 처리기에서 알림을 수신할 수도 있습니다.

```csharp
// Subscribe to foreground event
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // Your code for handling the notification
}
```


## <a name="how-to-fix-delays-in-the-background-task"></a>백그라운드 작업 지연 문제를 해결 하는 방법

앱을 테스트할 때 백그라운드 작업이 지연 되는 경우도 있고 몇 분 동안 트리거되지 않을 수도 있습니다. 지연을 해결 하려면 사용자에 게 시스템 설정으로 이동 하 라는 메시지를 표시 합니다. > 시스템-앱에의 한 배터리 사용 > 배터리 > 목록에서 앱을 찾아 선택 하 고 "항상 허용 됩니다."로 설정 합니다. 그런 다음, 백그라운드 작업은 수신 되는 알림 중 두 번째를 기준으로 항상 트리거됩니다.
