---
Description: Learn how Win32 C# apps can send local toast notifications and handle the user clicking the toast.
title: '데스크톱 C # 앱에서 로컬 알림 메시지 보내기'
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: Windows 10, uwp, win32, 데스크톱, 알림 메시지, 알림 보내기, 로컬 알림 보내기, 데스크톱 브리지, C#, C 샤프
ms.localizationpriority: medium
ms.openlocfilehash: 2c32ba5928d3c272ef77a70790640346fb000762
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2018
ms.locfileid: "8486723"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>데스크톱 C # 앱에서 로컬 알림 메시지 보내기

데스크톱 앱(데스크톱 브리지 및 클래식 Win32)은 UWP(유니버설 Windows 플랫폼) 앱과 마찬가지로 대화형 알림 메시지를 보낼 수 있습니다. 그러나 데스크톱 브리지를 사용하지 않는 경우 활성화 스키마가 다르고 패키지 ID가 부족할 수 있기 때문에 데스크톱 앱에 대한 몇 가지 특수 단계가 있습니다.

> [!IMPORTANT]
> UWP 앱을 작성하는 경우 [UWP 설명서](send-local-toast.md)를 참조하세요. 다른 데스크톱 언어는 [데스크톱 C++ WRL](send-local-toast-desktop-cpp-wrl.md)을 참조하세요.


## <a name="step-1-enable-the-windows-10-sdk"></a>1단계: Windows 10 SDK 활성화

Win32 앱용 Windows 10 SDK를 활성화하지 않았다면 먼저 활성화 해야 합니다.

프로젝트를 마우스 오른쪽 단추로 클릭한 다음 **프로젝트 언로드**를 선택합니다.

![프로젝트 언로드](images/win32-unload-project.png)

그런 다음 프로젝트를 마우스 오른쪽 단추로 클릭하고 **[projectname].csproj 편집**을 선택합니다.

![프로젝트 편집](images/win32-edit-project.png)

기존 `<TargetFrameworkVersion>`노드 아래에서 지원되는 최소 버전의 Windows 10을 지정한 새로운 `<TargetPlatformVersion>`노드를 추가합니다. 사용된 실제 SDK는 개발 시스템에 설치된 최신 SDK가 됩니다. 이것은 단순히 허용되는 최소 버전을 지정하며 Windows SDK를 참조할 수 있게 해줍니다.

```xml
...
<TargetFrameworkVersion>...</TargetFrameworkVersion>
<TargetPlatformVersion>10.0.10240.0</TargetPlatformVersion>
...
```

변경 사항을 저장하고 프로젝트를 다시 로드합니다.

![프로젝트를 다시 로드합니다.](images/win32-reload-project.png)


## <a name="step-2-reference-the-apis"></a>2단계: API 참조

참조 관리자를 열고(프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가-> 참조** 선택) **Windows-> Core**를 선택하고 다음 참조를 포함합니다.

* Windows.Data
* Windows.UI

![참조 관리자](images/win32-add-windows-reference.png)


## <a name="step-3-copy-compat-library-code"></a>3단계: 라이브러리 코드 복사

[GitHub에서 DesktopNotificationManagerCompat.cs 파일](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs)을 프로젝트에 복사합니다. 호환되는 라이브러리는 데스크톱 알림의 복잡성을 대부분 추상화합니다. 다음 지침은 호환되는 라이브러리가 필요합니다.


## <a name="step-4-implement-the-activator"></a>4단계: 활성자 구현

사용자가 알림을 클릭할 때 앱 작업을 수행할 수 있도록 알림 활성화에 대 한 처리기를 구현 해야 합니다. 이때 알림을 알림 센터에서 지속해야 합니다(앱을 종료하고 나서 며칠 후 알림을 클릭할 수 있기 때문). 이 클래스는 프로젝트의 어느 위치에나 배치할 수 있습니다.

**NotificationActivator** 클래스를 확장한 다음 아래에 나열된 세 가지 특성을 추가하고 많은 온라인 GUID 생성기 중 하나를 사용하여 고유한 GUID CLSID를 만듭니다. 이 CLSID(클래스 식별자)는 알림 센터가 COM에 활성화해야 하는 COM에 대한 클래스를 인식하는 방법입니다.

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-5-register-with-notification-platform"></a>5단계: 알림 플랫폼에 등록

그런 다음 알림 플랫폼에 등록해야 합니다. 데스크톱 브리지 또는 클래식 Win32 중 어느 것을 사용하는지에 따라 단계가 달라집니다. 이 둘을 모두 지원하는 경우 두 단계를 수행해야 합니다(하지만, 코드를 분기하지 않아도 되며 라이브러리가 이것을 자동으로 처리합니다!)


### <a name="desktop-bridge"></a>데스크톱 브리지

**Package.appxmanifest**에서 데스크톱 브리지(또는 이 두 가지 모두를 지원하는 경우)를 사용하는 경우 다음을 추가합니다.

1. **xmlns:com** 선언
2. **xmlns:desktop** 선언
3. **IgnorableNamespaces** 특성에서 **com**과 **desktop**
4. 4단계에서 GUID를 사용하여 COM 활성자에서 **com:Extension**. 알림에서 실행을 알 수 있도록 `Arguments="-ToastActivated"`를 포함해야 합니다.
5. **windows.toastNotificationActivation**에 대한 **desktop:Extension**는 알림 활성자 CLSID(4단계의 GUID)를 선언합니다.

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>클래식 Win32

클래식 Win32(또는 두 가지를 모두 지원하는 경우)를 사용하는 경우 시작 중 앱의 바로 가기에서 응용 프로그램 사용자 모델 ID(AUMID)와 알림 활성자 CLSID(4단계의 GUID)를 선언해야 합니다.

Win32 앱을 식별하는 고유한 AUMID를 선택합니다. 이것은 일반적으로 [CompanyName].[AppName]의 형태지만, 모든 앱에서 고유해야 합니다(끝 부분에 임의의 숫자를 자유롭게 추가할 수 있음).

#### <a name="step-51-wix-installer"></a>5.1단계: WiX 설치 관리자

설치 프로그램에 WiX를 사용하는 경우 **Product.wxs** 파일을 편집하여 두 개의 바로 가기 속성을 아래 보이는 시작 메뉴 바로 가기에 추가합니다. 4단계에서 GUID가 아래와 같이 `{}`로 묶여 있는지 확인해야 합니다.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 알림을 실제로 사용하려면 정상적으로 디버깅하기 전에 한 번 설치 프로그램을 통해 앱을 설치해야 AUMID 및 CLSID로 시작 바로 가기가 표시됩니다. 시작 바로 가기가 나타나면 Visual Studio에서 F5를 사용하여 디버깅할 수 있습니다.


#### <a name="step-52-register-aumid-and-com-server"></a>5.2단계: AUMID 및 COM 서버 등록

그런 다음 설치 프로그램과 관계없이 앱의 시작 코드(알림 API를 호출하기 전)에서 4단계의 알림 활성자 클래스와 위에 사용된 AUMID를 지정하여 **RegisterAumidAndComServer** 메서드를 호출합니다.

```csharp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

데스크톱 브리지와 클래식 Win32를 모두 지원하는 이 메서드를 자유롭게 호출합니다. 데스크톱 브리지에서 실행 중인 경우 이 메서드는 즉시 반환됩니다. 코드를 분기할 필요가 없습니다.

이 메서드를 사용하면 AUMID를 지속적으로 제공하지 않고도 compat API를 호출하여 알림을 보내고 관리할 수 있습니다. 또한 이 메서드는 COM 서버에 대해 LocalServer32 레지스트리 키를 삽입합니다.


## <a name="step-6-register-com-activator"></a>6단계: COM 활성자 등록

데스크톱 브리지 및 클래식 Win32 앱 모두에서 알림 활성화를 처리할 수 있도록 알림 활성자 유형을 등록해야 합니다.

앱의 시작 코드에서 다음 **RegisterActivator** 메서드를 호출하고 4단계에서 만든 **NotificationActivator** 클래스의 구현에서 전달합니다. 이것은 알림 활성화를 받을 수 있도록 호출되어야 합니다.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-7-send-a-notification"></a>7단계: 알림 보내기

알림 보내기는 **DesktopNotificationManagerCompat** 클래스를 사용하여 **ToastNotifier**를 만든다는 점만 제외하고 UWP 앱과 동일합니다. compat 라이브러리는 데스크톱 브리지와 클래식 Win32의 차이를 자동으로 처리하므로 코드를 분기하지 않아도 됩니다. 클래식 Win32의 경우 compat 라이브러리가 **RegisterAumidAndComServer**를 호출할 때 제공한 AUMID를 캐싱하므로 AUMID 제공 여부 시기를 걱정하지 않아도 됩니다.

> [!NOTE]
> [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치하여 원시 XML이 아닌 아래 나오는 C#를 사용하여 알림을 구성할 수 있습니다.

레거시 Windows 8.1 알림 메시지 템플릿은 4단계에서 생성한 COM 알림 활성자를 활성화하지 않기 때문에 아래에 나오는 **ToastContent**(또는 XML을 직접 만드는 경우 ToastGeneric 템플릿)을 사용해야 합니다.

> [!IMPORTANT]
> Http 이미지는 매니페스트에 인터넷 기능이 있는 데스크톱 브리지 앱에서만 지원됩니다. 클래식 Win32 앱은 http 이미지를 지원하지 않습니다. 따라서 로컬 앱 데이터에 이미지를 다운로드하고 로컬로 이것을 참조해야 합니다.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 클래식 Win32 앱은 레거시 알림 템플릿(예: ToastText02)을 사용할 수 없습니다. COM CLSID가 지정되었을 때 레거시 템플릿의 활성화는 실패합니다. 위에서 설명한 대로 Windows 10 ToastGeneric 템플릿을 사용해야 합니다.


## <a name="step-8-handling-activation"></a>8단계: 활성화 처리

사용자가 알림을 클릭할 때 **NotificationActivator** 클래스의 **OnActivated** 메서드가 호출됩니다.

OnActivated 메서드 내에서 알림에 지정된 인수를 구문 분석하고 사용자가 입력하거나 선택한 사용자 입력을 가져온 다음 그에 따라 앱을 활성화합니다.

> [!NOTE]
> **OnActivated** 메서드는 UI 스레드에서 호출되지 않습니다. UI 스레드 작업을 수행하려는 경우 `Application.Current.Dispatcher.Invoke(callback)`을 호출해야 합니다.

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

`App.xaml.cs` 파일에서 앱이 종료된 상태에서 제대로 실행되도록 지원하려면 **OnStartup** 메서드(WPF 앱용)를 재정의하여 알림에서 실행할지를 결정해야 합니다. 알림에서 시작하는 경우 "-ToastActivated"라는 시작 인수가 있습니다. 이것이 표시되면 정상적인 시작 활성화 코드 수행을 중단하고 **OnActivated** 코드 처리 실행을 허용해야 합니다.

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>이벤트의 활성화 시퀀스

WPF의 경우 활성화 시퀀스는 다음과 같습니다.

앱이 이미 실행되고 있는 경우:

1. **NotificationActivator**에서 **OnActivated**가 호출됨

앱이 실행되지 않는 경우:

1. "-ToastActivated"의 **Args**를 통해 `App.xaml.cs`에서 **OnStartup**가 호출됨
2. **NotificationActivator**에서 **OnActivated**가 호출됨


### <a name="foreground-vs-background-activation"></a>포그라운드 및 백그라운드 활성화
데스크톱 앱의 경우 전경 및 배경 활성화가 동일하게 처리되어 COM 활성화가 호출됩니다. 창을 표시할지 아니면 단순히 작업을 수행한 다음 종료할지 결정하는 것은 앱의 코드에 따라 달라집니다. 따라서 알림 콘텐츠에 **Background**의 **ActivationType**을 지정해도 동작이 바뀌지 않습니다.


## <a name="step-9-remove-and-manage-notifications"></a>9단계: 알림 제거 및 관리

알림 제거 및 관리는 UWP 앱과 동일합니다. 그렇지만 compat 라이브러리를 사용하여 **DesktopNotificationHistoryCompat**을 가져오는 것이 좋습니다. 따라서 클래식 Win32를 사용하는 경우 AUMID 제공에 대해 걱정하지 않아도 됩니다.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-10-deploying-and-debugging"></a>10단계: 배포 및 디버깅

데스크톱 브리지 앱을 배포하고 디버깅하려면 [패키지로 만든 데스크톱 앱 실행, 디버깅, 테스트](/porting/desktop-to-uwp-debug.md)를 참조하세요.

클래식 Win32 앱을 배포하고 디버깅하려면, 정상적으로 디버깅하기 전에 한 번 설치 프로그램을 통해 앱을 설치해야 해야만 AUMID 및 CLSID로 시작 바로 가기가 표시됩니다. 시작 바로 가기가 나타나면 Visual Studio에서 F5를 사용하여 디버깅할 수 있습니다.

클래식 Win32 앱에 알림이 표시되지 않고 예외가 발생하지 않으면 시작 바로 가기가 표시되지 않거나(설치 프로그램을 통해 앱 설치) 코드에 사용한 AUMID가 시작 바로 가기의 AUMID와 일치하지 않습니다.

알림이 나타나지만 알림 센터에 계속 표지되지 않으면(팝업이 해제된 후 사라짐) COM 활성자를 제대로 구현한 것이 아닙니다.

데스크톱 브리지와 클래식 Win32 앱을 모두 설치한 경우 알림 활성화를 처리할 때에는 데스크톱 브리지 앱이 클래식 Win32 앱보다 우선합니다. 즉, 클래식 Win32 앱의 알림은 클릭하면 계속해서 데스크톱 브리지 앱을 시작합니다. 데스크톱 브리지 앱을 제거하면 활성화가 다시 클래식 Win32 앱으로 되돌아갑니다.


## <a name="known-issues"></a>알려진 문제

**수정된 내용: 알림 클릭 후 앱의 초점이 맞춰지지 않음**: 빌드 15063 및 이전 버전에서는 전경 권한이 COM 서버를 활성화할 때 응용 프로그램으로 전달되지 않았습니다. 따라서 앱을 전경으로 옮기려고 하면 앱이 깜박입니다. 이 문제에 대한 해결 방법이 없었습니다. 빌드 16299 이상에서 이 문제를 해결했습니다.


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/desktop-toasts)
* [데스크톱 앱에서 알림 메시지](toast-desktop-apps.md)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)

