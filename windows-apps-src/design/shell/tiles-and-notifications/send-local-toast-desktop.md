---
description: '데스크톱 c # 앱이 로컬 알림 메시지를 보내고 알림 메시지를 클릭 하 여 사용자를 처리 하는 방법을 알아봅니다.'
title: 데스크톱 C# 앱에서 로컬 알림 메시지 보내기
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: 'windows 10, win32, 데스크톱, 알림 메시지, 알림 보내기, 로컬 알림 메시지 보내기, 데스크톱 브리지, msix, 스파스 패키지, c #, c sharp, 알림 메시지, wpf, 알림 메시지 보내기 wpf, 알림 메시지 보내기 winforms, 알림 메시지 보내기 c #, 알림 wpf, 알림 c # 보내기 #'
ms.localizationpriority: medium
ms.openlocfilehash: cb91a76db38623b533a925ea1df4728bc0fead78
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034476"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>데스크톱 C# 앱에서 로컬 알림 메시지 보내기

데스크톱 앱 (패키지 된 [Msix](/windows/msix/desktop/source-code-overview) 앱, 패키지 id를 가져오는 데 [스파스 패키지](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 앱 및 클래식 패키지 되지 않은 데스크톱 앱 포함)은 Windows 앱과 마찬가지로 대화형 알림 메시지를 보낼 수 있습니다. 그러나 MSIX 또는 스파스 패키지를 사용 하지 않는 경우 다양 한 활성화 체계와 패키지 id의 잠재적 부족으로 인해 데스크톱 앱에 대 한 몇 가지 특별 한 단계가 있습니다.

> [!IMPORTANT]
> UWP 앱을 작성 하는 경우 [uwp 설명서](send-local-toast.md)를 참조 하세요. 다른 데스크톱 언어는 [Win32 c + + WRL](send-local-toast-desktop-cpp-wrl.md)를 참조 하세요.


## <a name="step-1-install-the-notifications-library"></a>1 단계: 알림 라이브러리 설치

`Microsoft.Toolkit.Uwp.Notifications`프로젝트에 [NuGet 패키지](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 설치 합니다.

이 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 는 데스크톱 앱에서 알림 메시지를 사용 하기 위한 호환 라이브러리 코드를 추가 합니다. 또한 UWP Sdk를 참조 하 고 원시 XML 대신 c #을 사용 하 여 알림을 생성할 수 있습니다. 이 빠른 시작의 나머지 부분은 알림 라이브러리에 따라 달라 집니다.


## <a name="step-2-implement-the-activator"></a>2 단계: 활성기 구현

사용자가 알림을 클릭할 때 앱에서 어떤 작업을 수행할 수 있도록 알림 활성화에 대 한 처리기를 구현 해야 합니다. 알림 메시지는 나중에 앱을 닫을 때 며칠을 클릭할 수 있으므로 알림 메시지를 알림 센터에 보관 하는 데 필요 합니다. 이 클래스는 프로젝트의 어느 위치에 나 배치할 수 있습니다.

새 **Mynotificationactivator** 클래스를 만들고 **notificationactivator** 클래스를 확장 합니다. 아래에 나열 된 세 가지 특성을 추가 하 고 여러 온라인 GUID 생성기 중 하나를 사용 하 여 앱에 대 한 고유한 GUID CLSID를 만듭니다. 이 CLSID (클래스 식별자)는 Action Center가 COM에서 활성화할 클래스를 인식 하는 방법입니다.

**MyNotificationActivator.cs** (이 파일 만들기)

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


## <a name="step-3-register-with-notification-platform"></a>3 단계: 알림 플랫폼에 등록

그런 다음 알림 플랫폼에 등록 해야 합니다. MSIX/sparse 패키지를 사용 하는지 아니면 클래식 데스크톱이 사용 되는지에 따라 다른 단계가 있습니다. 두 단계를 모두 지 원하는 경우에는 두 단계를 모두 수행 해야 합니다 (그러나 코드를 분기할 필요는 없으며 라이브러리에서 사용자를 위해 처리 함).


#### <a name="msixsparse-packages"></a>[MSIX/sparse 패키지](#tab/msix-sparse)

[Msix](/windows/msix/desktop/source-code-overview) 또는 [스파스 패키지](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 경우 (또는 둘 다를 지 원하는 경우) appxmanifest.xml에서 다음을 추가 **합니다** .

1. **Xmlns: com** 에 대 한 선언
2. **Xmlns: desktop** 에 대 한 선언
3. **IgnorableNamespaces** 특성, **com** 및 **desktop**
4. **com:** #2 단계에서 GUID를 사용 하 여 com 활성기에 대 한 확장입니다. `Arguments="-ToastActivated"`알림 메시지의 시작을 알 수 있도록를 포함 해야 합니다.
5. **desktop:** TOAST 활성기 CLSID (#2의 GUID)를 선언 하는 **windows. toastNotificationActivation** 용 확장입니다.

**Package.appxmanifest**

```xml
<!--Add these namespaces-->
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


#### <a name="unpackaged"></a>[패키지 되지 않은](#tab/classic)

MSIX/sparse를 사용 하지 않는 경우 (또는 둘 다를 지 원하는 경우) 시작의 앱 바로 가기에서 응용 프로그램 사용자 모델 ID (AUMID) 및 toast 활성기 CLSID (#2 단계에서 GUID)를 선언 해야 합니다.

데스크톱 앱을 식별 하는 고유한 AUMID를 선택 합니다. 이는 일반적으로 [CompanyName] 형식입니다. [AppName] 이지만 모든 앱에서 고유한 지 확인 하는 것이 좋습니다 (끝에 일부 숫자를 추가 하는 데 사용 가능).

### <a name="step-31-wix-installer"></a>3.1 단계: WiX 설치 관리자

설치 관리자에 대해 WiX를 사용 하는 경우 다음에 표시 된 것 처럼 **Product. wxs** 파일을 편집 하 여 두 개의 바로 가기 속성을 시작 메뉴 바로 가기에 추가 합니다. 아래와 같이 #2 단계에서 GUID가로 묶여 있는지 확인 `{}` 합니다.

**Product. wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 실제로 알림을 사용 하려면 AUMID 및 CLSID를 사용 하 여 바로 가기 키가 표시 되도록 정상적으로 디버깅 하기 전에 설치 관리자를 통해 앱을 설치 해야 합니다. 시작 바로 가기가 있는 후 Visual Studio에서 F5 키를 사용 하 여 디버그할 수 있습니다.


### <a name="step-32-register-aumid-and-com-server"></a>3.2 단계: AUMID 및 COM 서버 등록

그런 다음, 응용 프로그램의 시작 코드 (알림 Api를 호출 하기 전에)에서 설치 관리자에 관계 없이 **RegisterAumidAndComServer** 메서드를 호출 하 여 #2 단계에서 알림 활성기 클래스를 지정 하 고 위에서 사용 했던 AUMID를 지정 합니다.

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

MSIX/sparse 패키지와 클래식 데스크톱을 모두 지 원하는 경우에는이 메서드를 자유롭게 호출할 수 있습니다. MSIX/스파스 패키지에서 실행 하는 경우이 메서드는 단순히를 즉시 반환 합니다. 코드를 분기 하지 않아도 됩니다.

이 방법을 사용 하면 AUMID를 지속적으로 제공 하지 않고도 호환성 Api를 호출 하 여 알림을 보내고 관리할 수 있습니다. 그리고 COM 서버에 대 한 LocalServer32 레지스트리 키를 삽입 합니다.

---


## <a name="step-4-register-com-activator"></a>4 단계: COM 활성기 등록

MSIX/sparse 패키지와 클래식 데스크톱 앱 모두에 대해 알림 활성화를 처리할 수 있도록 알림 활성기 유형을 등록 해야 합니다.

응용 프로그램의 시작 코드에서 다음 **registeractivator** 메서드를 호출 하 여 #2 단계에서 만든 **notificationactivator** 클래스의 구현을 전달 합니다. 이는 알림 활성화를 수신 하기 위해 호출 해야 합니다.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>5 단계: 알림 보내기

Notification notification manager **호환** 클래스를 사용 하 여 **to** notification을 만드는 것을 제외 하 고는 UWP 앱과 동일 합니다. 호환 라이브러리는 MSIX/sparse 패키지와 클래식 데스크톱 간의 차이점을 자동으로 처리 하므로 코드를 분기할 필요가 없습니다. 클래식 데스크톱의 경우에는 **RegisterAumidAndComServer** 를 호출할 때 사용자가 제공한 AUMID를 캐시 하 여 제공 하거나 제공 하지 않을 시기를 걱정 하지 않아도 됩니다.

> [!NOTE]
> 원시 XML을 사용 하는 대신 아래와 같이 c #을 사용 하 여 알림을 생성할 수 있도록 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 를 설치 합니다.

레거시 Windows 8.1 알림 템플릿이 #2 단계에서 만든 COM 알림 활성기를 활성화 하지 않으므로 아래 (또는 XML을 직접 작성 하는 경우 Toastcontent 템플릿) **에서 볼 수 있는지 확인 합니다.**

> [!IMPORTANT]
> Http 이미지는 자신의 매니페스트에 인터넷 기능이 있는 MSIX/sparse 패키지 앱 에서만 지원 됩니다. 클래식 데스크톱 앱은 http 이미지를 지원 하지 않습니다. 로컬 앱 데이터에 이미지를 다운로드 하 고 로컬에서 참조 해야 합니다.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 클래식 데스크톱 앱은 레거시 알림 템플릿 (예: ToastText02)을 사용할 수 없습니다. COM CLSID를 지정 하면 레거시 템플릿의 활성화가 실패 합니다. 위에 표시 된 대로 Windows 10 To Generic 템플릿을 사용 해야 합니다.


## <a name="step-6-handling-activation"></a>6 단계: 활성화 처리

사용자가 알림을 클릭 하면 **Notificationactivator** 클래스의 **onactivated** 된 메서드가 호출 됩니다.

OnActivated 메서드 내에서 알림 메시지에 지정 된 인수를 구문 분석 하 고 사용자가 입력 하거나 선택한 사용자 입력을 가져온 다음 적절 하 게 앱을 활성화할 수 있습니다.

> [!NOTE]
> **Onactivated** 된 메서드는 UI 스레드에서 호출 되지 않습니다. UI 스레드 작업을 수행 하려면를 호출 해야 `Application.Current.Dispatcher.Invoke(callback)` 합니다.

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

응용 프로그램을 닫는 동안 적절히 시작을 지원 하려면 `App.xaml.cs` 파일에서 **onstartup** 메서드 (WPF 앱의 경우)를 재정의 하 여 알림에서 시작 하 고 있는지 여부를 확인 합니다. 알림에서 시작 하는 경우 "-ToastActivated 됨"의 시작 인수가 있습니다. 이 메시지가 표시 되 면 일반 시작 활성화 코드의 실행을 중지 하 고 **Onactivated** 된 코드 핸들이 시작 되도록 허용 해야 합니다.

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


### <a name="activation-sequence-of-events"></a>이벤트 활성화 시퀀스

WPF의 경우 활성화 시퀀스는 다음과 같습니다.

앱이 이미 실행 중인 경우:

1. **Notificationactivator** 에서 **onactivated** 됨을 호출 합니다.

앱이 실행 되 고 있지 않은 경우:

1. 에서 **Onstartup** `App.xaml.cs` 은 "-toastactivated 됨"의 **Args** 를 사용 하 여 호출 됩니다.
2. **Notificationactivator** 에서 **onactivated** 됨을 호출 합니다.


### <a name="foreground-vs-background-activation"></a>포그라운드 vs 백그라운드 활성화
데스크톱 앱의 경우 포그라운드 및 백그라운드 활성화는 동일 하 게 처리 됩니다. COM 활성기가 호출 됩니다. 창 표시 여부를 결정 하는 응용 프로그램의 코드는 단순히 작업을 수행 하 고 종료 하는 방법을 결정 하는 것입니다. 따라서 알림 콘텐츠에서 배경 **ActivationType** 지정 **Background** 하면 동작이 변경 되지 않습니다.


## <a name="step-7-remove-and-manage-notifications"></a>7 단계: 알림 제거 및 관리

알림 제거 및 관리는 UWP 앱과 동일 합니다. 그러나 클래식 데스크톱을 사용 하는 경우 AUMID를 제공 하는 것에 대해 걱정 하지 않아도 되는 호환 라이브러리를 사용 하 여 **DesktopNotificationHistoryCompat** 를 얻는 것이 좋습니다.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>8 단계: 배포 및 디버깅

MSIX 앱을 배포 하 고 디버그 하려면 [패키지 된 데스크톱 앱 실행, 디버그 및 테스트](/windows/msix/desktop/desktop-to-uwp-debug)를 참조 하세요.

클래식 데스크톱 앱을 배포 하 고 디버그 하려면 AUMID 및 CLSID를 사용 하 여 바로 가기 키가 표시 되도록 정상적으로 디버깅 하기 전에 설치 관리자를 통해 앱을 설치 해야 합니다. 시작 바로 가기가 있는 후 Visual Studio에서 F5 키를 사용 하 여 디버그할 수 있습니다.

알림이 클래식 데스크톱 앱에 표시 되지 않고 예외가 throw 되지 않는 경우에는 바로 가기 키가 없거나 (설치 관리자를 통해 앱을 설치 하는 경우), 코드에서 사용 된 AUMID 시작 바로 가기의 AUMID 일치 하지 않을 수 있습니다.

알림이 표시 되지만 동작 센터 (popup이 해제 된 후 사라짐)에서 지속 되지 않는 경우 COM 활성기를 올바르게 구현 하지 않은 것입니다.

MSIX/sparse 패키지와 클래식 데스크톱 앱을 모두 설치한 경우 알림 활성화를 처리할 때 MSIX/sparse 패키지 앱이 클래식 데스크톱 앱을 대체 합니다. 즉, 클래식 데스크톱 앱의 알림을 클릭 하면 MSIX/sparse 패키지 앱이 계속 실행 됩니다. MSIX/sparse 패키지 앱을 제거 하면 정품 인증을 다시 클래식 데스크톱 앱으로 되돌립니다.


## <a name="known-issues"></a>알려진 문제

**수정 됨: 알림 클릭 후 앱이 집중 되지 않습니다** . 빌드 15063 이전 버전에서는 COM 서버를 활성화할 때 포그라운드 권한이 응용 프로그램에 전송 되지 않습니다. 따라서 앱은 포그라운드로 이동 하려고 할 때만 깜박입니다. 이 문제에 대 한 해결 방법이 없습니다. 빌드 16299 이상에서이 문제를 해결 했습니다.


## <a name="resources"></a>리소스

* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/desktop-toasts)
* [데스크톱 앱의 알림 메시지](toast-desktop-apps.md)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
