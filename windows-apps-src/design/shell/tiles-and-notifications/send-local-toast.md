---
Description: 로컬 알림 메시지를 보내고 알림 메시지를 클릭 하 여 사용자를 처리 하는 방법을 알아봅니다.
title: 로컬 알림 메시지 보내기
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, 알림 메시지 보내기, 알림, 알림 보내기, 알림 메시지, 방법, 빠른 시작, 시작, 코드 샘플, 연습
ms.localizationpriority: medium
ms.openlocfilehash: 23a1739b8f5859d128c97ff28350a548b61286d2
ms.sourcegitcommit: 63597f83f154ce41ebaf69c075093919c430297c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82034187"
---
# <a name="send-a-local-toast-notification"></a>로컬 알림 메시지 보내기


알림 메시지는 앱이 현재 앱 내부에 있지 않은 상태에서 사용자를 생성 하 고 제공할 수 있는 메시지입니다. 이 빠른 시작에서는 새로운 적응 템플릿 및 대화형 작업을 통해 Windows 10 알림 메시지를 만들고, 제공 하 고, 표시 하는 단계를 안내 합니다. 이러한 작업은 구현 하는 가장 간단한 알림 인 로컬 알림을 통해 보여 줍니다.

> [!IMPORTANT]
> 데스크톱 응용 프로그램 (패키지 된 [Msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) 앱, 패키지 id를 얻기 위해 [스파스 패키지](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 앱, 기본 패키지 되지 않은 Win32 앱 포함)은 알림을 보내고 활성화를 처리 하는 서로 다른 단계를 포함 합니다. 알림을을 구현 하는 방법에 대 한 자세한 내용은 [데스크톱 앱](toast-desktop-apps.md) 설명서를 참조 하세요.

다음 작업을 진행 합니다.

### <a name="sending-a-toast"></a>알림 보내기

* 알림의 시각적 요소 (텍스트 및 이미지) 구성
* 알림에 작업 추가
* 알림 만료 시간 설정
* 태그/그룹을 설정 하 여 나중에 알림 바꾸기/제거 가능
* 로컬 Api를 사용 하 여 알림 보내기

### <a name="handling-activation"></a>활성화 처리

* 본문이 나 단추를 클릭할 때 활성화 처리
* 포그라운드 활성화 처리
* 백그라운드 활성화 처리

> **중요 한 api**: [to notification 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification), [ToastNotificationActivatedEventArgs 클래스](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>사전 요구 사항

이 항목을 완벽 하 게 이해 하려면 다음을 수행 하는 것이 좋습니다.

* 알림 메시지 알림 용어 및 개념에 대 한 작업 정보입니다. 자세한 내용은 [알림 및 알림 센터 개요](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)를 참조 하세요.
* Windows 10 알림 메시지 내용에 대해 잘 알고 있어야 합니다. 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)를 참조 하세요.
* Windows 10 UWP 앱 프로젝트

> [!NOTE]
> Windows 8/8.1과 달리 앱에서 알림 메시지를 표시할 수 있다는 것을 응용 프로그램의 매니페스트에서 더 이상 선언할 필요가 없습니다. 모든 앱은 알림 메시지를 보내고 표시할 수 있습니다.

> [!NOTE]
> **Windows 8/8.1 앱**: [보관 된 설명서](https://docs.microsoft.com/previous-versions/windows/apps/hh868254(v=win.10))를 사용 하세요.


## <a name="install-nuget-packages"></a>NuGet 패키지 설치

두 개의 다음 NuGet 패키지를 프로젝트에 설치 하는 것이 좋습니다. 코드 샘플에서는 이러한 패키지를 사용 합니다. 문서 끝에는 NuGet 패키지를 사용 하지 않는 "바닐라" 코드 조각이 제공 됩니다.

* [Microsoft Toolkit: 알림](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): 원시 XML 대신 개체를 통해 알림 페이로드를 생성 합니다.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): C를 사용 하 여 쿼리 문자열 생성 및 구문 분석 #


## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가

`Windows.UI.Notifications`알림 Api를 포함 합니다.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>알림 보내기

Windows 10에서는 알림이 표시 되는 방식을 유연 하 게 사용할 수 있는 적응 언어를 사용 하 여 알림 콘텐츠를 설명 합니다. 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md) 를 참조 하세요.

### <a name="constructing-the-visual-part-of-the-content"></a>콘텐츠의 시각적 부분 구성

사용자에 게 표시할 텍스트와 이미지를 포함 하는 콘텐츠의 시각적 부분을 구성 하는 것부터 시작 해 보겠습니다.

알림 라이브러리 덕분에 XML 콘텐츠를 생성 하는 것은 간단 합니다. NuGet에서 알림 라이브러리를 설치 하지 않는 경우에는 오류에 대 한 공간을 확보 하는 XML을 수동으로 생성 해야 합니다.

> [!NOTE]
> 이미지는 앱의 패키지, 앱의 로컬 저장소 또는 웹에서 사용할 수 있습니다. 가 중 작성자 업데이트를 기준으로 웹 이미지는 일반 연결의 경우 최대 3mb, 요금제 연결의 경우 1mb가 될 수 있습니다. 아직가 중 작성자 업데이트를 실행 하지 않는 장치에서는 웹 이미지가 200 KB 보다 크지 않아야 합니다.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
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


### <a name="constructing-actions-part-of-the-content"></a>콘텐츠의 작업 구성 부분

이제 콘텐츠에 작업을 추가 해 보겠습니다.

아래 예제에서는 사용자가 단추 중 하나를 클릭할 때 앱에 반환 되는 텍스트를 입력할 수 있는 input 요소를 포함 했습니다.

그런 다음 각각 고유한 활성화 형식, 콘텐츠 및 인수를 사용 하 여 두 개의 단추를 추가 했습니다.
* **ActivationType** 는 사용자가이 작업을 수행할 때 앱을 활성화 하는 방법을 지정 하는 데 사용 됩니다. 응용 프로그램을 포그라운드에서 시작 하거나 백그라운드 작업을 시작 하거나 프로토콜 다른 앱을 시작 하도록 선택할 수 있습니다. 앱에서 포그라운드 또는 백그라운드를 선택 하는지 여부에 관계 없이 항상 사용자 입력 및 지정 된 인수를 받게 되므로 앱이 메시지 전송 또는 대화 열기와 같은 올바른 작업을 수행할 수 있습니다.

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


### <a name="combining-the-above-to-construct-the-full-content"></a>위의를 결합 하 여 전체 콘텐츠 생성

이제 콘텐츠 생성이 완료 되었으며,이를 사용 하 여 [**To notification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) 개체를 인스턴스화할 수 있습니다.

**참고**: 사용자가 알림 메시지 본문을 탭 할 때 수행 해야 하는 정품 인증 유형을 지정 하기 위해 root 요소 내에 활성화 유형을 제공할 수도 있습니다. 일반적으로 알림의 본문을 탭 하 여 일관 된 사용자 환경을 만들기 위해 포그라운드에서 앱을 시작 해야 하지만, 사용자에 게 가장 적합 한 특정 시나리오에 맞게 다른 활성화 유형을 사용할 수 있습니다.

사용자가 알림 메시지의 본문을 탭 하 고 앱이 시작 되 면 앱에서 표시 해야 하는 콘텐츠를 알고 있으므로 항상 **Launch** 속성을 설정 해야 합니다.

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

Windows 10에서 모든 알림 알림은 사용자가 해제 하거나 무시 한 후에 작업 센터에 표시 되므로 사용자는 팝업이 사라진 후 알림을 볼 수 있습니다.

그러나 알림의 메시지가 일정 기간 동안만 관련 된 경우에는 사용자가 앱에서 오래 된 정보를 볼 수 없도록 알림 메시지의 만료 시간을 설정 해야 합니다. 예를 들어 프로 모션이 12 시간 동안만 유효 하면 만료 시간을 12 시간으로 설정 합니다. 아래 코드에서는 만료 시간을 2 일로 설정 합니다.

> [!NOTE]
> 로컬 알림 알림의 기본 및 최대 만료 시간은 3 일입니다.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>알림에 대 한 기본 키 제공

보내는 알림을 프로그래밍 방식으로 제거 하거나 교체 하려는 경우에는 Tag 속성 (필요한 경우 그룹 속성)을 사용 하 여 알림에 대 한 기본 키를 제공 해야 합니다. 그런 다음 나중에이 기본 키를 사용 하 여 알림을 제거 하거나 바꿀 수 있습니다.

이미 제공 된 알림 메시지를 교체/제거 하는 방법에 대 한 자세한 내용은 빠른 시작: 알림 메시지 [관리 센터 (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))를 참조 하세요.

태그 및 그룹 결합은 복합 기본 키로 작동 합니다. Group은 보다 일반적인 식별자로, "wallPosts", "messages", "friendRequests" 등의 그룹을 할당할 수 있습니다. 그런 다음 태그는 그룹 내에서 알림 자체를 고유 하 게 식별 해야 합니다. 일반 그룹을 사용 하면 [REMOVEGROUP API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)를 사용 하 여 해당 그룹에서 모든 알림을 제거할 수 있습니다.

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>알림 보내기

알림을 초기화 한 후에는 알림 메시지를 전달 [하 고 Show](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) ()를 호출 하 여 알림 메시지를 생성 하면 됩니다.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
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


## <a name="activation-handling"></a>활성화 처리

Windows 10에서 사용자가 알림을 클릭 하면 두 가지 방법으로 알림 메시지를 활성화할 수 있습니다.

* 포그라운드 활성화
* 백그라운드 활성화

> [!NOTE]
> Windows 8.1에서 레거시 알림 템플릿을 사용 하는 경우 onlaunched이 **활성화**되지 않고 **onlaunched** 이 호출 됩니다. 다음 설명서는 알림 라이브러리 (또는 원시 XML을 사용 하는 경우 To Generic 템플릿)를 활용 하는 최신 Windows 10 알림에만 적용 됩니다.


### <a name="handling-foreground-activation"></a>포그라운드 활성화 처리

Windows 10에서 사용자가 최신 알림 (또는 알림 단추)을 클릭 **하면 onactivated이 새** 활성화 종류 ( **toastnotification**)를 사용 하 여 **onactivated**대신 호출 됩니다. 따라서 개발자는 알림 활성화를 쉽게 구분 하 고 그에 따라 작업을 수행할 수 있습니다.

아래에 표시 된 예제에서는 알림 콘텐츠에 처음 제공 된 인수 문자열을 검색할 수 있습니다. 텍스트 상자와 선택 상자에서 사용자가 제공한 입력을 검색할 수도 있습니다.

> [!IMPORTANT]
> 사용자의 프레임을 초기화 하 고 **Onlaunched** 코드와 마찬가지로 창을 활성화 해야 합니다. 앱이 닫히고 처음 실행 되는 경우에도 **사용자가 알림을 클릭 하면 Onlaunched이 호출 되지**않습니다. 동일한 초기화가 둘 다에서 발생 해야 하기 때문에 **Onlaunched** 및 **onlaunched** 된 메서드를 사용자 고유의 `OnLaunchedOrActivated` 메서드에 결합 하는 것이 좋습니다.

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


그런 다음 App.xaml.cs에서 OnBackgroundActivated 메서드를 재정의 하 여 전경 활성화와 비슷한 미리 정의 된 인수 및 사용자 입력을 검색할 수 있습니다.

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



## <a name="plain-vanilla-code-snippets"></a>일반 "바닐라" 코드 조각

NuGet에서 알림 라이브러리를 사용 하지 않는 경우 아래 표시 된 대로 수동으로 XML을 생성 하 여 [To notification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)을 만들 수 있습니다.

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
* [To Notification 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 클래스](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
