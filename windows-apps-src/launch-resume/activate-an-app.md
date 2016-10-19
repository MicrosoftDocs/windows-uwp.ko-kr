---
author: TylerMSFT
title: "앱 활성화 처리"
description: "OnLaunched 메서드를 재정의하여 앱 활성화를 처리하는 방법을 알아봅니다."
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
translationtype: Human Translation
ms.sourcegitcommit: a1bb0d5d24291fad1acab41c149dd9d763610907
ms.openlocfilehash: e41a683026a4543545556e98f6b4e9194099b362

---

# 앱 활성화 처리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드를 재정의하여 앱 활성화를 처리하는 방법을 알아봅니다.

## 실행 처리기 재정의

어떤 이유로든 앱이 활성화되면 시스템은 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 전송합니다. 활성화 유형 목록을 보려면 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 열거형을 참조하세요.

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 클래스는 다양한 활성화 유형을 처리하기 위해 재정의할 수 있는 메서드를 정의합니다. 몇 가지 활성화 유형에는 재정의 가능한 특정 메서드가 있습니다. 기타 활성화 유형의 경우 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 메서드를 재정의합니다.

응용 프로그램용 클래스를 정의합니다.

```xml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AppName.App" >
```

[**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드를 재정의합니다. 사용자가 앱을 실행하면 이 메서드가 호출됩니다. [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 매개 변수는 이전 앱 상태 및 활성화 인수를 포함합니다.

**참고** Windows Phone 스토어 앱에서는 앱이 현재 메모리에서 일시 중단된 경우에도 사용자가 시작 타일이나 앱 목록에서 앱을 시작할 때마다 이 메서드가 호출됩니다. Windows에서는 시작 타일이나 앱 목록에서 일시 중단된 앱을 시작해도 이 메서드가 호출되지 않습니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> namespace AppName
> {
>    public partial class App
>    {
>       async protected override void OnLaunched(LaunchActivatedEventArgs args)
>       {
>          EnsurePageCreatedAndActivate();
>       }
>
>       // Creates the MainPage if it isn't already created.  Also activates
>       // the window so it takes foreground and input focus.
>       private MainPage EnsurePageCreatedAndActivate()
>       {
>          if (Window.Current.Content == null)
>          {
>              Window.Current.Content = new MainPage();
>          }
>
>          Window.Current.Activate();
>          return Window.Current.Content as MainPage;
>       }
>    }
> }
> ```
> ```vb
> Class App
>    Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
>       Window.Current.Content = New MainPage()
>       Window.Current.Activate()
>    End Sub
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
> void App::OnLaunched(LaunchActivatedEventArgs^ args)
> {
>    EnsurePageCreatedAndActivate();
> }
>
> // Creates the MainPage if it isn't already created.  Also activates
> // the window so it takes foreground and input focus.
> void App::EnsurePageCreatedAndActivate()
> {
>     if (_mainPage == nullptr)
>     {
>         // Save the MainPage for use if we get activated later
>         _mainPage = ref new MainPage();
>     }
>     Window::Current->Content = _mainPage;
>     Window::Current->Activate();
> }
> ```

## 앱이 일시 중단된 후 종료된 경우 응용 프로그램 데이터 복원


사용자가 종료된 앱으로 전환하면 시스템은 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728)를 **Launch**로, [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)를 **Terminated** 또는 **ClosedByUser**로 설정하여 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 전송합니다. 앱은 저장된 응용 프로그램 데이터를 로드하고 표시 콘텐츠를 새로 고쳐야 합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> async protected override void OnLaunched(LaunchActivatedEventArgs args)
> {
>    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
>        args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```
> ```vb
> Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
>    Dim restoreState As Boolean = False
>
>    Select Case args.PreviousExecutionState
>       Case ApplicationExecutionState.Terminated
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case ApplicationExecutionState.ClosedByUser
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case Else
>          ' TODO: Populate the UI with defaults
>    End Select
>
>    Window.Current.Content = New MainPage(restoreState)
>    Window.Current.Activate()
> End Sub
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
> {
>    if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
>        args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```

[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)의 값이 **NotRunning**이면, 앱에서 응용 프로그램 데이터를 저장하지 못하며 마치 처음 실행하는 것처럼 앱이 다시 시작됩니다.

## 설명

> **참고** Windows Phone 스토어 앱에서는 앱이 현재 일시 중단되었으며 사용자가 기본 타일이나 앱 목록에서 앱을 다시 시작하는 경우에도 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트 뒤에 항상 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)가 발생합니다. 현재 창에 이미 설정된 콘텐츠가 있는 경우 앱에서 초기화를 건너뛸 수 있습니다. [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 속성을 검사하여 앱이 기본 타일에서 시작되었는지 또는 보조 타일에서 시작되었는지 확인할 수 있으며, 해당 정보에 따라 새로운 환경을 표시할지 또는 앱 환경을 다시 시작할지 결정할 수 있습니다.

## 관련 항목

* [앱 일시 중단 처리](suspend-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [앱 수명 주기](app-lifecycle.md)

**참조**

* [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 



<!--HONumber=Aug16_HO3-->


