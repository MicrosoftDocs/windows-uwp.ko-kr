---
title: 앱 사전 실행 처리
description: OnLaunched 메서드를 재정의 하 고 CoreApplication (true)를 호출 하 여 앱 사전 실행을 처리 하는 방법을 알아봅니다.
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 219ca73115d5605f1e1483f2af224a13c28791ff
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164867"
---
# <a name="handle-app-prelaunch"></a>앱 사전 실행 처리

[**Onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 된 메서드를 재정의 하 여 앱 사전 실행을 처리 하는 방법을 알아봅니다.

## <a name="introduction"></a>소개

사용 가능한 시스템 리소스를 허용 하는 경우 백그라운드에서 사용자의 가장 자주 사용 되는 앱을 사전에 실행 하 여 데스크톱 장치 제품군 장치에서 UWP 앱의 시작 성능이 향상 됩니다. 사전 시작 된 앱은 시작 된 직후 일시 중단 된 상태로 전환 됩니다. 그러면 사용자가 앱을 호출할 때 일시 중단 됨 상태에서 실행 중 상태로 전환 하 여 앱을 다시 시작 합니다 .이는 앱 콜드을 시작 하는 것 보다 더 빠릅니다. 사용자의 경험은 앱이 매우 빠르게 시작 되는 것입니다.

Windows 10 이전에는 앱이 사전 실행을 자동으로 사용 하지 않았습니다. Windows 10 버전 1511에서는 모든 UWP (유니버설 Windows 플랫폼) 앱이 사전에 출시 될 것입니다. Windows 10 버전 1607에서는 [CoreApplication (true)](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)를 호출 하 여 사전 호출 동작을 옵트인 (opt in) 해야 합니다. 이 호출을 수행 하는 데 적합 한 `OnLaunched()` 위치는 확인 하는 위치 근처에 `if (e.PrelaunchActivated == false)` 있습니다.

앱이 사전에 실행 되는지 여부는 시스템 리소스에 따라 달라 집니다. 시스템에 리소스가 부족 한 경우에는 앱이 사전에 시작 되지 않습니다.

일부 유형의 앱은 사전 실행에서 잘 작동 하도록 시작 동작을 변경 해야 할 수 있습니다. 예를 들어, 앱이 시작 될 때 음악을 재생 하 고, 사용자가 있고 앱이 시작 될 때 정교한 시각적 개체를 표시 하는 게임, 시작 중에 사용자의 온라인 표시 여부를 변경 하는 메시징 앱은 앱이 사전에 시작 된 시기를 식별 하 고 아래 섹션에 설명 된 대로 시작 동작을 변경할 수 있습니다.

Visual Studio 2015 업데이트 3의 XAML 프로젝트 (c #, VB, c + +) 및 WinJS에 대 한 기본 템플릿은 사전에이를 수용 합니다.

## <a name="prelaunch-and-the-app-lifecycle"></a>사전 사전 및 앱 수명 주기

앱이 사전에 시작 된 후 일시 중단 상태가 됩니다. [앱 일시 중단 처리](suspend-an-app.md)를 참조 하세요.

## <a name="detect-and-handle-prelaunch"></a>사전 검사 검색 및 처리

앱은 활성화 중에 [**LaunchActivatedEventArgs 활성화**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.prelaunchactivated) 된 플래그를 수신 합니다. 다음 [**응용 프로그램**](/uwp/api/windows.ui.xaml.application.onlaunched)수정에 표시 된 것 처럼 사용자가 명시적으로 앱을 시작할 때만 실행 해야 하는 코드를 실행 하려면이 플래그를 사용 합니다.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // CoreApplication.EnablePrelaunch was introduced in Windows 10 version 1607
    bool canEnablePrelaunch = Windows.Foundation.Metadata.ApiInformation.IsMethodPresent("Windows.ApplicationModel.Core.CoreApplication", "EnablePrelaunch");

    // NOTE: Only enable this code if you are targeting a version of Windows 10 prior to version 1607
    // and you want to opt-out of prelaunch.
    // In Windows 10 version 1511, all UWP apps were candidates for prelaunch.
    // Starting in Windows 10 version 1607, the app must opt-in to be prelaunched.
    //if ( !canEnablePrelaunch && e.PrelaunchActivated == true)
    //{
    //    return;
    //}

    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        // On Windows 10 version 1607 or later, this code signals that this app wants to participate in prelaunch
        if (canEnablePrelaunch)
        {
            TryEnablePrelaunch();
        }

        // TODO: This is not a prelaunch activation. Perform operations which
        // assume that the user explicitly launched the app such as updating
        // the online presence of the user on a social network, updating a
        // what's new feed, etc.

        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();
    }
}

/// <summary>
/// Encapsulates the call to CoreApplication.EnablePrelaunch() so that the JIT
/// won't encounter that call (and prevent the app from running when it doesn't
/// find it), unless this method gets called. This method should only
/// be called when the caller determines that we are running on a system that
/// supports CoreApplication.EnablePrelaunch().
/// </summary>
private void TryEnablePrelaunch()
{
    Windows.ApplicationModel.Core.CoreApplication.EnablePrelaunch(true);
}
```

`TryEnablePrelaunch()`위의 함수를 확인 합니다. 를 호출 하는 이유는 `CoreApplication.EnablePrelaunch()` 메서드가 호출 될 때 JIT (just-in-time 컴파일)가 전체 메서드를 컴파일하려고 시도 하기 때문입니다. 앱이를 지원 하지 않는 버전의 Windows 10에서 실행 되는 경우 `CoreApplication.EnablePrelaunch()` JIT는 실패 합니다. 응용 프로그램에서 플랫폼이 지 원하는 것으로 확인 될 때만 호출 되는 메서드로 호출을 팩터링 하 여 `CoreApplication.EnablePrelaunch()` 해당 문제를 방지 합니다.

위의 예제에는 앱이 Windows 10, 버전 1511에서 실행 되는 경우 실행 전 옵트아웃 (opt out) 해야 하는 경우 주석을 제거할 수 있는 코드도 있습니다. 버전 1511에서는 모든 UWP 앱이 자동으로 사전에 옵트인 (opt in) 되어 앱에 적합 하지 않을 수 있습니다.

## <a name="use-the-visibilitychanged-event"></a>VisibilityChanged 이벤트 사용

사전 인증으로 활성화 된 앱은 사용자에 게 표시 되지 않습니다. 사용자가 전환할 때 표시 됩니다. 앱의 주 창이 표시 될 때까지 특정 작업을 연기할 수 있습니다. 예를 들어 앱이 피드의 새로운 항목 목록을 표시 하는 경우 앱이 실행 될 때 작성 된 목록을 사용 하는 대신 [**VisibilityChanged**](/uwp/api/windows.ui.xaml.window.visibilitychanged) 이벤트 중에 사용자가 앱을 활성화 하는 시간에 만료 될 수 있으므로 목록을 업데이트할 수 있습니다. 다음 코드는 **Mainpage**에 대 한 **VisibilityChanged** 이벤트를 처리 합니다.

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## <a name="directx-games-guidance"></a>DirectX 게임 지침

DirectX 게임은 일반적으로 사전 실행을 사용 하도록 설정 해야 합니다. Windows 1607, 기념일 버전부터 게임이 기본적으로 시작 되지 않습니다.  게임에서 사전 실행을 활용 하려면 [CoreApplication (true)](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)를 호출 합니다.

게임이 이전 버전의 Windows 10을 대상으로 하는 경우 사전 작업 조건을 처리 하 여 응용 프로그램을 종료할 수 있습니다.

```cppwinrt
void ViewProvider::OnActivated(CoreApplicationView const& /* appView */, Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Launch)
    {
        auto launchArgs{ args.as<Windows::ApplicationModel::Activation::LaunchActivatedEventArgs>()};
        if (launchArgs.PrelaunchActivated())
        {
            // Opt-out of Prelaunch.
            CoreApplication::Exit();
        }
    }
}

void ViewProvider::Initialize(CoreApplicationView const & appView)
{
    appView.Activated({ this, &App::OnActivated });
}
```

```cpp
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## <a name="winjs-app-guidance"></a>WinJS 앱 지침

WinJS 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 [onactivated](/previous-versions/windows/apps/br212679(v=win.10)) 된 처리기에서 사전 작업 조건을 처리할 수 있습니다.

```javascript
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>일반 지침

-   앱이 신속 하 게 일시 중단 될 수 없는 경우 종료 되기 때문에 앱이 실행 전 장기 실행 작업을 수행 하면 안 됩니다.
-   앱은 응용 프로그램에서 오디오 재생을 시작 하면 안 [**됩니다. onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 앱이 표시 되지 않으므로 오디오가 재생 되는 이유는 드러나지 않습니다.
-   앱이 시작 중에는 앱이 사용자에 게 표시 되는 것으로 가정 하거나 사용자가 앱을 명시적으로 실행 한 것으로 가정 하는 작업을 수행 하지 않아야 합니다. 이제 명시적인 사용자 작업 없이 백그라운드에서 앱을 시작할 수 있으므로 개발자는 개인 정보, 사용자 환경 및 성능 영향을 고려해 야 합니다.
    -   개인 정보 취급 방침 고려 사항은 소셜 앱이 사용자 상태를 온라인으로 변경 해야 하는 경우입니다. 앱이 사전에 시작 될 때 상태를 변경 하는 대신 사용자가 앱으로 전환할 때까지 기다려야 합니다.
    -   사용자 환경 고려 사항 예를 들어 게임과 같은 앱이 시작 될 때 소개 시퀀스를 표시 하는 경우 사용자가 앱으로 전환할 때까지 소개 시퀀스를 연기할 수 있습니다.
    -   예를 들어 사용자가 앱을 시작할 때 로드 하는 대신 사용자가 앱으로 전환 하 여 현재 날씨 정보를 검색 한 다음, 해당 정보가 최신 상태 인지 확인 하기 위해 앱이 표시 될 때 다시 로드 해야 하는 경우에 발생할 수 있습니다.
-   앱이 시작 될 때 라이브 타일을 지우면 표시 유형이 변경 된 이벤트까지이 작업을 지연 시킵니다.
-   앱에 대 한 원격 분석은 일반적인 타일 활성화와 사전 실행 활성화를 구분 하 여 문제가 발생 하는 경우 시나리오의 범위를 쉽게 좁힐 수 있도록 해야 합니다.
-   2015 업데이트 1 및 windows 10 버전 1511 Microsoft Visual Studio 있는 경우 **Debug** &gt; **다른 디버그 대상** 디버그 &gt; **Windows 유니버설 앱 사전 디버그 디버그**를 선택 하 여 Visual Studio 2015에서 앱 앱에 대 한 사전 로그인을 시뮬레이션할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [앱 수명 주기](app-lifecycle.md)
* [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)