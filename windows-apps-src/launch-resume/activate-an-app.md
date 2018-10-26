---
author: TylerMSFT
title: 앱 활성화 처리
description: OnLaunched 메서드를 재정의하여 앱 활성화를 처리하는 방법을 알아봅니다.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.author: twhitney
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 4d69680df1684da756219c180bbe6d47263801b9
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5622062"
---
# <a name="handle-app-activation"></a>앱 활성화 처리

[**Application.OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 메서드를 재정의 하 여 앱 활성화를 처리 하는 방법을 알아봅니다.

## <a name="override-the-launch-handler"></a>실행 처리기 재정의

어떤 이유로 든 앱이 활성화 되 면 시스템은 [**CoreApplicationView.Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 이벤트를 보냅니다. 활성화 유형 목록을 보려면 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 열거형을 참조하세요.

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 클래스는 다양한 활성화 유형을 처리하기 위해 재정의할 수 있는 메서드를 정의합니다. 몇 가지 활성화 유형에는 재정의 가능한 특정 메서드가 있습니다. 기타 활성화 유형의 경우 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 메서드를 재정의합니다.

응용 프로그램용 클래스를 정의합니다.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

[**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드를 재정의합니다. 사용자가 앱을 실행하면 이 메서드가 호출됩니다. [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 매개 변수는 이전 앱 상태 및 활성화 인수를 포함합니다.

> [!NOTE]
> Windows, 시작 타일이 나 앱 목록에서 일시 중단 된 앱을 시작 해도이 메서드가 호출 되지 않습니다.

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

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>앱이 일시 중단된 후 종료된 경우 응용 프로그램 데이터 복원

사용자가 종료된 앱으로 전환하면 시스템은 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728)를 **Launch**로, [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)를 **Terminated** 또는 **ClosedByUser**로 설정하여 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 전송합니다. 앱은 저장된 응용 프로그램 데이터를 로드하고 표시 콘텐츠를 새로 고쳐야 합니다.

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

[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)의 값이 **NotRunning**이면, 앱에서 응용 프로그램 데이터를 저장하지 못하며 마치 처음 실행하는 것처럼 앱이 다시 시작됩니다.

## <a name="remarks"></a>설명

> [!NOTE]
> 현재 창에 이미 설정된 콘텐츠가 있는 경우 앱에서 초기화를 건너뛸 수 있습니다. [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 속성 주 또는 보조 타일에서 시작 했다고 여부를 결정 하 고 해당 정보를 기반으로 새로 표시 또는 앱 환경을 다시 해야 하는지 여부를 결정을 확인할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [Windows.ApplicationModel.Activation](https://msdn.microsoft.com/library/windows/apps/br224766)
* [Windows.UI.Xaml.Application](https://msdn.microsoft.com/library/windows/apps/br242324)

## <a name="related-topics"></a>관련 항목
* [앱 일시 중단 처리](suspend-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [앱 수명 주기](app-lifecycle.md)
