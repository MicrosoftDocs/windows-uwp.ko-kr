---
Description: UWP 앱에서 로컬 알림 메시지를 보내고 알림 메시지를 클릭 하 여 사용자를 처리 하는 방법을 알아봅니다.
title: UWP 앱에서 로컬 알림 메시지 보내기
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, 알림 메시지 보내기, 알림, 알림 보내기, 알림 메시지, 방법, 빠른 시작, 시작, 코드 샘플, 연습
ms.localizationpriority: medium
ms.openlocfilehash: 7b669ad3c846fec0b60ae01134b80a6d87586c62
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100291"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>UWP 앱에서 로컬 알림 메시지 보내기


알림 메시지는 앱이 현재 앱 내부에 있지 않은 상태에서 사용자를 생성 하 고 제공할 수 있는 메시지입니다. 이 빠른 시작에서는 새로운 적응 템플릿 및 대화형 작업을 통해 Windows 10 알림 메시지를 만들고, 제공 하 고, 표시 하는 단계를 안내 합니다. 이러한 작업은 구현 하는 가장 간단한 알림 인 로컬 알림을 통해 보여 줍니다.

> [!IMPORTANT]
> 데스크톱 응용 프로그램 (패키지 된 [Msix 개](/windows/msix/desktop/source-code-overview) 앱, 패키지 id를 가져오는 데 [스파스 패키지](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 앱 및 클래식 패키지 되지 않은 데스크톱 앱 포함)은 알림을 보내고 활성화를 처리 하는 서로 다른 단계를 포함 합니다. 알림을을 구현 하는 방법에 대 한 자세한 내용은 [데스크톱 앱](toast-desktop-apps.md) 설명서를 참조 하세요.

> **중요 한 api**: [to notification 클래스](/uwp/api/Windows.UI.Notifications.ToastNotification), [ToastNotificationActivatedEventArgs 클래스](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)



## <a name="step-1-install-nuget-package"></a>1 단계: NuGet 패키지 설치

[Microsoft Toolkit. 알림 NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치 합니다. 코드 샘플에서는이 패키지를 사용 합니다. 문서의 끝 부분에서는 NuGet 패키지를 사용 하지 않는 "일반" 코드 조각을 제공 합니다. 이 패키지를 사용 하면 XML을 사용 하지 않고 알림 메시지를 만들 수 있습니다.


## <a name="step-2-add-namespace-declarations"></a>2 단계: 네임 스페이스 선언 추가

`Windows.UI.Notifications` 알림 Api를 포함 합니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>3 단계: 알림 보내기

Windows 10에서는 알림이 표시 되는 방식을 유연 하 게 사용할 수 있는 적응 언어를 사용 하 여 알림 콘텐츠를 설명 합니다. 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md) 를 참조 하세요.

간단한 텍스트 기반 알림을 시작 합니다. 알림 라이브러리를 사용 하 여 알림 콘텐츠를 생성 하 고 알림을 표시 합니다.

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show();
```

## <a name="step-4-handling-activation"></a>4 단계: 활성화 처리

사용자가 알림을 클릭 하면 (또는 전경 활성화를 사용 하는 알림의 단추) 앱의 **App.xaml.cs** **onactivated** 이 호출 됩니다.

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> 사용자의 프레임을 초기화 하 고 **Onlaunched** 코드와 마찬가지로 창을 활성화 해야 합니다. 앱이 닫히고 처음 실행 되는 경우에도 **사용자가 알림을 클릭 하면 Onlaunched이 호출 되지**않습니다. 동일한 초기화가 둘 다에서 발생 해야 하기 때문에 **Onlaunched** 및 **onlaunched** 된 메서드를 사용자 고유의 메서드에 결합 하는 것이 좋습니다 `OnLaunchedOrActivated` .


## <a name="activation-in-depth"></a>심층 활성화

알림을 사용 하도록 설정 하는 첫 번째 단계는 사용자가 알림을 클릭할 때 앱이 시작할 항목을 알 수 있도록 알림에 일부 시작 인수를 추가 하는 것입니다 .이 경우에는 나중에 대화를 열어야 하는 특정 대화를 알려 줍니다.

아래와 같이 알림 인수에 대 한 쿼리 문자열을 생성 하 고 구문 분석 하는 데 도움이 되는 [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/) NuGet 패키지를 설치 하는 것이 좋습니다.

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


활성화를 처리 하는 더 복잡 한 예제는 다음과 같습니다.

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="adding-images"></a>이미지 추가

알림에 다양 한 콘텐츠를 추가할 수 있습니다. 인라인 이미지와 프로필 (앱 로고 재정의) 이미지를 추가 합니다.

> [!NOTE]
> 이미지는 앱의 패키지, 앱의 로컬 저장소 또는 웹에서 사용할 수 있습니다. 가 중 작성자 업데이트를 기준으로 웹 이미지는 일반 연결의 경우 최대 3mb, 요금제 연결의 경우 1mb가 될 수 있습니다. 아직가 중 작성자 업데이트를 실행 하지 않는 장치에서는 웹 이미지가 200 KB 보다 크지 않아야 합니다.

> [!IMPORTANT]
> Http 이미지는 매니페스트에 인터넷 기능이 있는 UWP/MSIX/sparse 앱 에서만 지원 됩니다. 데스크톱 비 m 6/스파스 앱은 http 이미지를 지원 하지 않습니다. 로컬 앱 데이터에 이미지를 다운로드 하 고 로컬에서 참조 해야 합니다.

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>단추 및 입력 추가

단추와 입력을 추가 하 여 알림을 대화형으로 만들 수 있습니다. 단추는 전경 앱, 프로토콜 또는 백그라운드 작업을 시작할 수 있습니다. 회신 텍스트 상자, "좋아요" 단추 및 이미지를 여는 "보기" 단추를 추가 합니다.

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

전경 단추의 활성화는 기본 알림 본문 (App.xaml.cs OnActivated이 호출 됨)과 동일한 방식으로 처리 됩니다.



## <a name="handling-background-activation"></a>백그라운드 활성화 처리

알림 (또는 알림 내부의 단추)에서 백그라운드 활성화를 지정 하면 포그라운드 앱을 활성화 하는 대신 백그라운드 작업이 실행 됩니다.

백그라운드 작업에 대 한 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](/windows/uwp/launch-resume/support-your-app-with-background-tasks)을 참조 하세요.

빌드 14393 이상을 대상으로 하는 경우 처리 중인 백그라운드 작업을 사용 하 여 작업을 크게 간소화할 수 있습니다. In-process 백그라운드 작업은 이전 버전의 Windows에서 실행 되지 않습니다. 이 코드 샘플에서는 in-process 백그라운드 작업을 사용 합니다.

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


그런 다음 App.xaml.cs에서 OnBackgroundActivated 메서드를 재정의 합니다. 그러면 전경 활성화와 비슷하게 미리 정의 된 인수 및 사용자 입력을 검색할 수 있습니다.

**App.xaml.cs**

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```


## <a name="set-an-expiration-time"></a>만료 시간 설정

Windows 10에서 모든 알림 알림은 사용자가 해제 하거나 무시 한 후에 작업 센터에 표시 되므로 사용자는 팝업이 사라진 후 알림을 볼 수 있습니다.

그러나 알림의 메시지가 일정 기간 동안만 관련 된 경우에는 사용자가 앱에서 오래 된 정보를 볼 수 없도록 알림 메시지의 만료 시간을 설정 해야 합니다. 예를 들어 프로 모션이 12 시간 동안만 유효 하면 만료 시간을 12 시간으로 설정 합니다. 아래 코드에서는 만료 시간을 2 일로 설정 합니다.

> [!NOTE]
> 로컬 알림 알림의 기본 및 최대 만료 시간은 3 일입니다.

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>알림에 대 한 기본 키 제공

보내는 알림을 프로그래밍 방식으로 제거 하거나 교체 하려는 경우에는 Tag 속성 (필요한 경우 그룹 속성)을 사용 하 여 알림에 대 한 기본 키를 제공 해야 합니다. 그런 다음 나중에이 기본 키를 사용 하 여 알림을 제거 하거나 바꿀 수 있습니다.

이미 제공 된 알림 메시지를 교체/제거 하는 방법에 대 한 자세한 내용은 빠른 시작: 알림 메시지 [관리 센터 (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))를 참조 하세요.

태그 및 그룹 결합은 복합 기본 키로 작동 합니다. Group은 보다 일반적인 식별자로, "wallPosts", "messages", "friendRequests" 등의 그룹을 할당할 수 있습니다. 그런 다음 태그는 그룹 내에서 알림 자체를 고유 하 게 식별 해야 합니다. 일반 그룹을 사용 하면 [REMOVEGROUP API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)를 사용 하 여 해당 그룹에서 모든 알림을 제거할 수 있습니다.

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>알림 지우기

UWP 앱은 자체 알림을 제거 하 고 제거 해야 합니다. 앱이 시작 되 면 알림을 자동으로 지우지 않습니다.

사용자가 알림을 명시적으로 클릭 하면 Windows에서 알림을 자동으로 제거 합니다.

다음은 메시징 앱이 수행 해야 하는 작업의 예입니다.

1. 사용자가 대화의 새 메시지에 대해 여러 알림을를 수신 합니다.
2. 사용자가 이러한 알림을 중 하나를 탭 하 여 대화를 엽니다.
3. 앱은 대화를 연 다음 해당 대화에 대해 앱에서 제공 하는 그룹에서 [Removegroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 을 사용 하 여 해당 대화에 대 한 모든 알림을를 지웁니다.
4. 이제 사용자의 작업 센터는 알림 상태를 올바르게 반영 합니다. 해당 대화에 대 한 부실 알림이 작업 센터에 남아 있기 때문입니다.

모든 알림을 지우거 나 특정 알림을 제거 하는 방법에 대 한 자세한 내용은 [빠른 시작: 동작 센터에서 알림 메시지 관리 (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))를 참조 하세요.

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>일반 코드 조각

NuGet에서 알림 라이브러리를 사용 하지 않는 경우 아래 표시 된 대로 수동으로 XML을 생성 하 여 [To notification](/uwp/api/Windows.UI.Notifications.ToastNotification)을 만들 수 있습니다.

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
* [To Notification 클래스](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 클래스](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)