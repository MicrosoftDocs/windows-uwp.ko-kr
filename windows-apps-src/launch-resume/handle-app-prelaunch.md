---
author: TylerMSFT
title: 앱 사전 실행 처리
description: OnLaunched 메서드를 재정의하고 CoreApplication.EnablePrelaunch(true)를 호출하여 앱 사전 실행을 처리하는 방법을 알아봅니다.
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a13ec942080d7fe517a10b837bea9ae8fae27750
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5641039"
---
# <a name="handle-app-prelaunch"></a>앱 사전 실행 처리

[**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드를 재정의하여 앱 사전 실행을 처리하는 방법을 알아봅니다.

## <a name="introduction"></a>소개

사용 가능한 시스템 리소스를 허용 하는 경우 사전 백그라운드에서 사용자가 가장 자주 사용 하는 앱을 시작 하 여 데스크톱 디바이스 패밀리 디바이스에서 UWP 앱의 시작 성능을 향상 되었습니다. 사전 실행된 앱은 시작된 후 일시 중단 상태가 됩니다. 그런 다음 사용자가 앱을 호출하면 앱이 일시 중단 상태에서 실행 상태로 전환되어 다시 시작합니다. 이는 앱 콜드를 시작하는 것보다 빠릅니다. 사용자 환경은 앱이 쉽고 빠르게 시작되는 것입니다.

Windows 10 이전 버전에서는 앱이 자동으로 사전 실행을 활용하지 않았습니다. Windows10, 버전 1511에서는 모든 유니버설 Windows 플랫폼 (UWP) 앱 사전 실행 되는 것에 대 한 후보를 했습니다. Windows 10 버전 1607에서는 [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)를 호출하여 사전 실행 동작에 옵트인(opt-in)해야 합니다. 이 호출을 배치하기 좋은 위치는 `if (e.PrelaunchActivated == false)` 확인이 수행되는 위치 근처의 `OnLaunched()` 내입니다.

앱의 사전 실행 여부는 시스템 리소스에 따라 달라집니다. 시스템에 리소스가 부족하면 앱은 사전 실행되지 않습니다.

일부 유형의 앱은 사전 실행이 잘 작동하려면 시작 동작을 변경해야 할 수 있습니다. 예를 들어 앱이 시작할 때 음악을 재생하는 앱, 앱이 시작할 때 사용자가 있다고 가정하여 정교한 시각 효과를 표시하는 게임, 시작하는 동안 사용자의 온라인 표시를 변경하는 메시지 앱은 앱이 사전 실행된 시점을 식별하여 아래 섹션에 설명된 대로 시작 동작을 변경할 수 있습니다.

XAML 프로젝트(C#, VB, C++)와 WinJS의 기본 템플릿은 Visual Studio 2015 업데이트 3에서 사전 실행을 수용합니다.

## <a name="prelaunch-and-the-app-lifecycle"></a>사전 실행 및 앱 수명 주기

앱이 사전 실행되면 일시 중단 상태가 됩니다. ([앱 일시 중단 처리](suspend-an-app.md)를 참조하세요.)

## <a name="detect-and-handle-prelaunch"></a>사전 실행 감지 및 처리

앱이 활성화되는 동안 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 플래그를 수신합니다. 이 플래그를 사용 하 여 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)를 다음과 같이 수정에 표시 된 대로 앱을 명시적으로 시작할 때을 실행만 해야 하는 코드를 실행 합니다.

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

참고는 `TryEnablePrelaunch()` 위에 작동 합니다. 호출 하는 이유를 `CoreApplication.EnablePrelaunch()` 는 포함이 함수에 대 한 아웃 JIT (컴파일)에 전체 메서드를 컴파일할 하려고 메서드를 호출할 때 하기 때문입니다. 앱이 지원 하지 않는 Windows 10 버전에서 실행 하는 경우 `CoreApplication.EnablePrelaunch()`, JIT 실패 합니다. 예상 되는 앱 플랫폼에서 지를 결정 하는 경우에 라고 하는 메서드를 호출 하 여 `CoreApplication.EnablePrelaunch()`,이 문제를 방지 하는 것입니다.

위 예제에서 코드도 앱 옵트아웃 사전 실행의 Windows 10 버전 1511에서 실행 해야 하는 경우 주석 수 있습니다. 자동으로 옵트인 된 모든 UWP 앱 버전 1511에서에서으로 사전 실행 하지 않을 앱에 적합 합니다.

## <a name="use-the-visibilitychanged-event"></a>VisibilityChanged 이벤트 사용

사전 실행으로 활성화된 앱이 사용자에게 표시되지 않습니다. 사용자가 전환할 때 표시됩니다. 앱의 주 창이 표시될 때까지 특정 작업을 지연시킬 수 있습니다. 예를 들어 앱이 피드에서 새 항목 목록을 표시하면 앱이 사전 실행되었을 때 작성되어 사용자가 앱을 활성화하는 시점까지 상태가 오래될 수 있기 때문에 목록을 사용하는 대신 [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 이벤트가 발생하는 동안 목록을 업데이트할 수 있습니다. 다음 코드는 **MainPage**의 **VisibilityChanged** 이벤트를 처리합니다.

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

많은 DirectX 게임이 사전 실행을 검색하기 전에 초기화를 수행하므로 DirectX 게임은 일반적으로 사전 실행을 사용하지 않아야 합니다. Windows 1607 1주년 버전부터 게임은 기본적으로 사전 실행되지 않습니다.  게임에 사전 실행을 활용하려면 [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)를 호출합니다.

게임이 Windows 10의 이전 버전을 대상으로 하는 경우 사전 실행 조건을 처리하여 응용 프로그램을 종료할 수 있습니다.

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

WinJS 앱이 Windows 10의 이전 버전을 대상으로 하는 경우 [onactivated](https://msdn.microsoft.com/library/windows/apps/br212679.aspx) 처리기에서 사전 실행 조건을 처리할 수 있습니다.

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

-   앱이 신속하게 일시 중단 상태가 되지 못하면 앱이 종료되므로 사전 실행 동안에는 장기 실행 작업을 수행하면 안 됩니다.
-   앱이 표시되지 않고 오디오 재생의 이유가 명확하지 않으므로 앱을 사전 실행할 때 앱이 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)의 오디오 재생을 시작하면 안 됩니다.
-   사용자에게 표시되거나 명시적으로 사용자가 시작했다고 가정하는 앱은 시작하는 동안 어떤 작업도 수행하면 안 됩니다. 앱이 현재 명시적인 사용자 작업 없이 백그라운드에서 시작되었기 때문에 개발자는 개인 정보, 사용자 환경 및 성능 영향을 고려해야 합니다.
    -   개인 정보가 고려되어야 하는 예는 소셜 앱이 사용자의 상태를 온라인으로 변경해야 하는 경우입니다. 앱이 사전 실행되면 상태를 변경하는 대신 사용자가 앱으로 전환할 때까지 기다려야 합니다.
    -   사용자 환경이 고려되는 예는 시작할 때 소개 시퀀스를 표시하는 게임과 같은 앱의 경우 사용자가 앱으로 전환할 때까지 소개 시퀀스를 지연시킬 수 있는 경우입니다.
    -   성능 영향의 예는 앱이 사전 실행될 때 현재 날씨 정보를 로드하는 대신 사용자가 앱으로 전환하여 이 정보를 검색한 다음 앱이 표시되면 다시 로드하여 해당 정보가 최신 정보인지 확인해야 하는 경우입니다.
-   앱이 시작되었을 때 라이브 타일을 지우면 VisibilityChanged 이벤트가 발생할 때까지 이를 연기합니다.
-   앱에 대한 원격 분석으로 일반적인 타일 활성화와 사전 실행된 활성화를 구별하여 문제 발생 시 시나리오의 범위를 쉽게 좁힐 수 있습니다.
-   Microsoft Visual Studio2015 업데이트 1 및 Windows10, 버전 1511 경우 시뮬레이션할 수 있습니다 사전 실행 시각적 Studio2015에서 앱에 대 한 **디버그** 선택 하 여 &gt; **기타 디버그 대상** &gt; **Windows 유니버설 앱 디버그 사전 실행**.

## <a name="related-topics"></a>관련 항목

* [앱 수명 주기](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)
