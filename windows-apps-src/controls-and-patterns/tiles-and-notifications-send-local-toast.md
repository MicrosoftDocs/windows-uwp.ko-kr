---
author: mijacobs
Description: "로컬 알림 메시지를 보내고 사용자의 알림 클릭을 처리하는 방법을 알아봅니다."
title: "로컬 알림 메시지 보내기"
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7f21c6a72c00ae2677adb0c1196030997ed48491
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2017
---
# <a name="send-a-local-toast-notification"></a>로컬 알림 메시지 보내기

알림 메시지는 사용자가 앱 내에 있지 않을 때 앱에서 생성하여 사용자에게 제공할 수 있는 메시지입니다. 이 빠른 시작에서는 새로운 적응형 템플릿과 대화형 작업을 사용하여 Windows 10 알림 메시지를 만들고, 배달하고, 표시하는 단계를 안내합니다. 가장 간단하게 구현할 수 있는 로컬 알림을 통해 이러한 작업을 보여 줍니다. 다음과 같은 작업을 수행합니다.

### <a name="sending-a-toast"></a>알림 보내기

* 알림의 시각적 부분(텍스트 및 이미지) 생성
* 알림에 작업 추가
* 알림에 만료 시간 설정
* 나중에 알림을 바꾸고 제거할 수 있도록 태그/그룹 설정
* 로컬 API를 사용하여 알림 보내기

### <a name="handling-activation"></a>활성화 처리

* 본문 또는 단추 클릭 시 활성화 처리
* 포그라운드 활성화 처리
* 백그라운드 활성화 처리


## <a name="prerequisites"></a>사전 요구 사항

다음은 이 문서의 내용을 완전히 이해하는 데 도움이 되는 항목입니다.

* 알림 메시지 용어 및 개념에 관한 실무 지식 자세한 내용은 [Toast and action center overview(알림 및 알림 센터 개요)](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)를 참조하세요.
* Windows 10 알림 메시지 콘텐츠에 대한 기본적인 지식 자세한 내용은 [알림 콘텐츠 설명서](tiles-and-notifications-adaptive-interactive-toasts.md)를 참조하세요.
* Windows 10 UWP 앱 프로젝트 만들기

**참고**: Windows 8/8.1과 달리 이제 앱에서 알림 메시지를 표시할 수 있음을 앱의 매니페스트에 선언할 필요가 없습니다. 모든 앱에서 알림 메시지를 보내고 표시할 수 있습니다.

**Windows 8/8.1 앱**: 대신 [보관된 문서](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868254.aspx)를 사용하세요.


## <a name="install-nuget-packages"></a>NuGet 패키지 설치

프로젝트에 다음 2개의 NuGet 패키지를 설치하는 것이 좋습니다. 코드 샘플에서 이러한 패키지를 사용합니다. 문서 끝에서 NuGet 패키지를 사용하지 않는 “Vanilla” 코드 조각을 제공합니다.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): 원시 XML 대신 개체를 통해 알림 페이로드를 생성합니다.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): C#으로 쿼리 문자열 생성 및 구문 분석


## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가

Windows.UI.Notifications에는 알림 API가 포함됩니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>알림 보내기

Windows 10에서는 알림 모양을 유연하게 지정할 수 있게 하는 적응형 언어를 사용하여 알림 메시지 콘텐츠를 설명합니다. 자세한 내용은 [알림 콘텐츠 설명서](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/02/adaptive-and-interactive-toast-notifications-for-windows-10.aspx)를 참조하세요.

### <a name="constructing-the-visual-part-of-the-content"></a>콘텐츠의 시각적 부분 생성

사용자에게 표시할 텍스트와 이미지 콘텐츠를 포함한 콘텐츠의 시각적 부분을 생성하는 것으로 시작하겠습니다.

알림 라이브러리 덕분에 XML 콘텐츠 생성이 쉬워졌습니다. NuGet에서 알림 라이브러리를 설치하지 않는 경우 XML을 수동으로 구성해야 합니다. 이는 오류의 여지를 남깁니다.

참고: 앱의 패키지, 앱의 로컬 저장소 또는 웹에서 이미지를 사용할 수 있습니다. 웹 이미지는 크기가 200KB 미만이어야 합니다.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://unsplash.it/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
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
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>콘텐츠의 작업 부분 생성

이제 콘텐츠에 작업을 추가해 보겠습니다.

아래 예에서는 사용자의 텍스트 입력을 허용하는 입력 요소를 포함했습니다. 각 작업에 대해 상호 작용이 어떻게 정의되었는지에 따라 포그라운드 또는 백그라운드에서 이 입력 요소가 활성화되면 앱에서 해당 ID를 사용하여 입력 요소를 검색할 수 있습니다.

그런 다음 각각 자체 활성화 유형, 콘텐츠 및 인수를 지정하는 2개의 단추를 만들었습니다.
* ActivationType은 사용자가 이 작업을 수행할 때 앱을 어떻게 활성화할지를 지정하는 데 사용됩니다. 포그라운드에서 앱을 실행하거나, 백그라운드 작업을 실행하거나, 다른 앱에 대해 프로토콜 실행을 수행하도록 선택할 수 있습니다. 앱에서 활성화 유형을 포그라운드 또는 백그라운드로 선택하는지 여부에 관계없이 항상 사용자 입력과 인수 특성에 미리 정의한 인수를 검색하는 방법이 있으므로 사용자가 알림에 어떻게 대응했는지를 완전히 파악할 수 있습니다.

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>위를 결합하여 전체 콘텐츠 생성

콘텐츠 생성을 마쳤으므로 콘텐츠를 사용하여 ToastNotification 개체를 인스턴스화할 수 있습니다.

**참고**: 루트 요소 내에 활성화 유형을 제공하여 사용자가 알림 메시지의 본문을 탭할 때 수행할 활성화 유형을 지정할 수도 있습니다. 일반적으로 알림의 본문을 탭하면 일관성 있는 사용자 환경 조성을 위해 포그라운드에서 앱이 실행되어야 하지만 사용자에게 가장 적합한 특정 시나리오에 맞게 다른 활성화 유형을 사용할 수 있습니다.

전과 마찬가지로 루트 요소에 항상 실행 매개 변수를 추가할 수 있고 추가해야 하므로 사용자가 알림 본문을 탭할 때 알림의 콘텐츠와 관련된 보기로 앱을 계속 실행할 수 있습니다.

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>만료 시간 설정

Windows 10에서 사용자가 알림 메시지를 해제하거나 무시한 후 모든 알림 메시지가 알림 센터(이전에는 전화에서만 사용할 수 있었지만 이제는 모든 Windows 장치에서 사용 가능)로 이동하므로 사용자가 팝업이 사라진 후 알림을 볼 수 있습니다.

그러나 알림의 메시지가 일정 기간과만 관계가 있는 경우 사용자가 앱에서 오래된 정보를 보지 않도록 알림 메시지에 만료 시간을 설정해야 합니다. 아래 코드에서 만료 시간을 2일로 설정합니다.

**참고**: 로컬 알림 메시지의 기본 및 최대 만료 시간은 3일입니다.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>알림에 대한 기본 키 제공

보내는 알림을 프로그래밍 방식으로 제거하거나 바꾸려면 Tag 속성과 Group 속성(옵션)을 사용하여 알림에 대한 기본 키를 제공해야 합니다. 그런 다음 나중에 이 기본 키를 사용하여 알림을 제거하거나 바꿀 수 있습니다.

이미 배달한 알림 메시지 바꾸기/제거에 대한 자세한 내용은 [빠른 시작: 관리 센터에서 알림 메시지 관리(XAML)](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dn631260.aspx)를 참조하세요.

결합된 Tag와 Group은 복합 기본 키 역할을 합니다. Group은 "wallPosts", "messages", "friendRequests"와 같은 그룹에 할당할 수 있는 더 일반적인 식별자이고, Tag는 그룹 내에서 알림을 고유하게 식별해야 합니다. 그러면 일반 그룹을 사용하여 [RemoveGroup API](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)로 해당 그룹에서 모든 알림을 제거할 수 있습니다.

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>알림 보내기

알림을 구성한 후에는 ToastNotifier를 생성하고 Show()를 호출하여 알림 메시지를 전달하면 됩니다.

```
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>알림 지우기

UWP 앱은 자체 알림을 제거하고 지우는 역할을 합니다. 앱이 실행될 때 알림을 자동으로 지우지 않습니다.

사용자가 명시적으로 알림을 클릭할 경우에만 Windows에서 알림을 자동으로 제거합니다.

다음은 메시지 앱이 수행해야 하는 작업의 예입니다.

1. 사용자가 대화 중 새 메시지에 대한 여러 알림을 받습니다.
2. 사용자가 다음 알림 중 하나를 탭하여 대화를 엽니다.
3. 앱에서 대화를 열고 해당 대화에 대한 앱 제공 그룹에 [RemoveGroup](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)을 사용하여 해당 대화에 대한 모든 알림을 지웁니다.
4. 이제 알림 센터에 남은 대화에 대한 오래된 알림이 없으므로 사용자의 알림 센터에 알림 상태가 적절히 반영됩니다.

모든 알림 지우기 및 특정 알림 제거에 대한 자세한 내용은 [빠른 시작: 관리 센터에서 알림 메시지 관리(XAML)](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dn631260.aspx)를 참조하세요.


## <a name="handling-activation"></a>활성화 처리

Windows 10에서 사용자가 알림을 클릭할 때 알림에서 두 가지 방식으로 앱을 활성화하도록 할 수 있습니다.

* 포그라운드 활성화
* 백그라운드 활성화

**참고**: Windows 8.1에서 레거시 알림 템플릿을 사용할 경우 OnLaunched가 먼저 호출됩니다. 다음 설명서는 알림 라이브러리를 활용하는 최신 Windows 10 알림이나 ToastGeneric 템플릿(원시 XML을 사용하는 경우)에만 적용됩니다.


### <a name="handling-foreground-activation"></a>포그라운드 활성화 처리

Windows 10에서 사용자가 최신 알림을 클릭하면 새 활성화 종류인 ToastNotification과 함께 OnLaunched 대신 OnActivated가 호출됩니다. 따라서 개발자가 쉽게 알림 활성화를 구별하고 그에 따라 작업을 수행할 수 있습니다.

아래 예에서는 알림 콘텐츠에 처음 제공한 인수 문자열을 검색할 수 있습니다. 또한 사용자가 텍스트 상자와 선택 상자에 제공한 입력을 검색할 수 있습니다.

**참고**: OnLaunched와 마찬가지로 프레임을 초기화하고 창을 활성화해야 합니다. 앱이 닫혀 있고 처음 실행되더라도 **사용자가 알림을 클릭할 때 OnLaunched가 호출되지 않습니다**. 양쪽에서 동일한 초기화가 발생해야 하므로 자체 OnLaunchedOrActivated 메서드에 OnLaunched와 OnActivated를 결합하는 것이 좋습니다.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
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


## <a name="handling-background-activation"></a>백그라운드 활성화 처리

알림이나 알림 내의 단추에 백그라운드 활성화를 지정하면 포그라운드 앱을 활성화하는 대신 백그라운드 작업이 실행됩니다.

백그라운드 작업에 대한 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks)을 참조하세요.

빌드 14393 이상을 대상으로 하는 경우 처리 중인 백그라운드 작업을 사용하여 작업을 크게 단순화할 수 있습니다. 오래된 컴퓨터에서는 처리 중인 백그라운드 작업이 실행되지 않습니다. 이 코드 샘플에서는 처리 중인 백그라운드 작업을 사용하겠습니다.

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
    Name = "MyToastNotificationActionTrigger",
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


그런 다음 App.xaml.cs에서 OnBackgroundActivated 메서드를 재정의하면 포그라운드 활성화와 같이 미리 정의된 인수와 사용자 입력을 검색할 수 있습니다.

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



## <a name="plain-vanilla-code-snippets"></a>일반 "Vanilla" 코드 조각

NuGet의 알림 라이브러리를 사용하지 않을 경우 아래와 같이 XML을 수동으로 구성하여 ToastNotification을 생성할 수 있습니다.

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

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [알림 콘텐츠 설명서](tiles-and-notifications-adaptive-interactive-toasts.md)