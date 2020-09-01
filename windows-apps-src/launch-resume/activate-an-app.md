---
title: 앱 활성화 처리
description: OnLaunched 메서드를 재정의 하 여 앱 활성화를 처리 하는 방법을 알아봅니다.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 0c3aa86fd8eee3724e092799eea6d34f0b9d453b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164997"
---
# <a name="handle-app-activation"></a>앱 활성화 처리

[**응용 프로그램의 OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 메서드를 재정의 하 여 앱 활성화를 처리 하는 방법을 알아봅니다.

## <a name="override-the-launch-handler"></a>시작 처리기 재정의

앱이 활성화 되 면 어떤 이유로 든 시스템은 [**CoreApplicationView**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 이벤트를 보냅니다. 활성화 유형 목록은 [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 열거를 참조 하세요.

[**응용 프로그램**](/uwp/api/Windows.UI.Xaml.Application) 은 다양 한 활성화 형식을 처리 하기 위해 재정의할 수 있는 메서드를 정의 합니다. 몇 가지 활성화 형식에는 재정의할 수 있는 특정 메서드가 있습니다. 다른 활성화 형식의 경우 [**onactivated**](/uwp/api/windows.ui.xaml.application.onactivated) 메서드를 재정의 합니다.

응용 프로그램에 대 한 클래스를 정의 합니다.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

[**Onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 된 메서드를 재정의 합니다. 이 메서드는 사용자가 앱을 시작할 때마다 호출 됩니다. [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 매개 변수는 앱의 이전 상태와 활성화 인수를 포함 합니다.

> [!NOTE]
> Windows에서 시작 타일 또는 앱 목록에서 일시 중단 된 앱을 시작 하면이 메서드가 호출 되지 않습니다.

```csharp
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }

      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }

         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```

```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
```

```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>앱이 일시 중단 되었다가 종료 된 경우 응용 프로그램 데이터 복원

사용자가 종료 된 앱으로 전환 하면 시스템은 **시작** 으로 설정 되 고 [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) 가 **종료** 됨 또는 **Closedbyuser**로 설정 된 [**활성화**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) [**된 이벤트**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) 를 보냅니다. 앱은 저장 된 응용 프로그램 데이터를 로드 하 고 표시 된 콘텐츠를 새로 고쳐야 합니다.

```csharp
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

[**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) 의 값이 **NotRunning**인 경우 앱은 응용 프로그램 데이터를 성공적으로 저장 하지 못했으며 앱은 처음 시작 된 것 처럼 시작 해야 합니다.

## <a name="remarks"></a>설명

> [!NOTE]
> 현재 창에 이미 콘텐츠가 설정 되어 있으면 앱에서 초기화를 건너뛸 수 있습니다. [**LaunchActivatedEventArgs TileId**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) 속성을 확인 하 여 앱이 기본 또는 보조 타일에서 시작 되었는지 여부를 확인 하 고, 해당 정보에 따라 새로 제공 하거나 앱 환경을 다시 시작할지 여부를 결정할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [Windows ApplicationModel. 활성화](/uwp/api/Windows.ApplicationModel.Activation)
* [Windows. .Xaml. .Xaml 응용 프로그램](/uwp/api/Windows.UI.Xaml.Application)

## <a name="related-topics"></a>관련 항목
* [앱 일시 중단 처리](suspend-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [앱 일시 중단 및 다시 시작에 대 한 지침](./index.md)
* [앱 수명 주기](app-lifecycle.md)