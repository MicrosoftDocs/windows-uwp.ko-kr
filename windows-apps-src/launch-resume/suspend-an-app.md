---
author: TylerMSFT
title: "앱 일시 중단 처리"
description: "시스템에서 앱을 일시 중단할 때 중요한 응용 프로그램 데이터를 저장하는 방법을 알아봅니다."
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.sourcegitcommit: fb83213a4ce58285dae94da97fa20d397468bdc9
ms.openlocfilehash: 3ad58dc20a660d89622d215c46d263adf27a0542

---

# 앱 일시 중단 처리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**일시 중단**](https://msdn.microsoft.com/library/windows/apps/br242341)

시스템에서 앱을 일시 중단할 때 중요한 응용 프로그램 데이터를 저장하는 방법을 배웁니다. 아래의 예제에서는 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트용 이벤트 처리기를 등록하고 문자열을 파일에 저장합니다.

## suspending 이벤트 처리기 등록


시스템이 앱을 일시 중단하기 전에 앱에서 응용 프로그램 데이터를 저장해야 함을 나타내는 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트를 처리하도록 등록합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
>    }
> }
> ```
> ```vb
> Public NotInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Suspending, AddressOf App_Suspending
>    End Sub
>    
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel;
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
>
> MainPage::MainPage()
> {
>    InitializeComponent();
>    Application::Current->Suspending +=
>        ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
> }
> ```

## [!div class="tabbedCodeSnippets"]


일시 중단 전에 응용 프로그램 데이터 저장 앱에서 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트를 처리하면, 처리기 함수에서 중요한 응용 프로그램 데이터를 저장할 기회가 생깁니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     async void App_Suspending(
>         Object sender,
>         Windows.ApplicationModel.SuspendingEventArgs e)
>     {
>         // TODO: This is the time to save app data in case the process is terminated
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Suspending(
>         sender As Object,
>         e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending
>
>         ' TODO: This is the time to save app data in case the process is terminated
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
> {
>     // TODO: This is the time to save app data in case the process is terminated
> }
> ```

## 간단한 응용 프로그램 데이터를 동기적으로 저장하기 위해 앱에서 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 저장소 API를 사용해야 합니다.


[!div class="tabbedCodeSnippets"] 설명 사용자가 다른 앱이나 데스크톱 또는 시작 화면으로 전환할 때마다 시스템에서 앱을 일시 중단합니다. 사용자가 다시 돌아올 때마다 시스템에서 앱을 다시 시작합니다.

시스템에서 앱을 다시 시작할 때, 변수와 데이터 구조의 콘텐츠는 시스템에서 앱을 일시 중단하기 전과 동일합니다. 앱은 중단되었던 곳에서 정확히 복원되므로, 사용자에게는 앱이 배경에서 실행되고 있었던 것처럼 보입니다. 앱이 일시 중단된 동안 시스템은 앱과 데이터를 메모리에 유지하려고 합니다.

그러나 앱을 메모리에 유지할 리소스가 없으면 시스템은 앱을 종료합니다.

> 사용자가 일시 중단 후 종료된 앱으로 다시 돌아오면, 시스템은 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 전송하며 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드에서 응용 프로그램 데이터를 복원해야 합니다. 시스템은 앱이 종료되었을 때 앱에게 알리지 않으므로, 앱은 일시 중단될 때 응용 프로그램 데이터를 저장하고 단독 리소스와 파일 핸들을 해제하며 앱이 종료 후 활성화될 때 이 리소스와 파일 핸들을 복원해야 합니다.

> **참고** 앱이 일시 중단 상태에서 비동기 작업을 수행해야 할 경우 작업이 완료될 때까지 일시 중단 종료를 지연해야 합니다. 이벤트 인수를 통해 사용할 수 있는 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 개체의 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 메서드를 사용하여 반환된 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 개체에서 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 메서드가 호출될 때까지 일시 중단 완료를 지연할 수 있습니다. **참고** Windows 8.1에서 시스템 응답을 향상시키기 위해 앱에는 일시 중단 시 리소스에 대한 낮은 우선 순위 액세스 권한이 제공됩니다.

> 이 새 우선 순위를 지원하기 위해 일시 중단 작업 제한 시간이 확장되어 앱에 Windows의 일반 우선 순위에 대한 5초 제한 시간이나 Windows Phone의 1-10초 제한 시간에 해당하는 제한 시간이 부여됩니다. 이 제한 시간은 확장하거나 변경할 수 없습니다. **Visual Studio를 사용하는 디버깅에 대한 참고 사항:** Visual Studio에서는 Windows가 디버거에 연결되어 있는 앱을 일시 중단하지 못하도록 합니다. 이렇게 하는 것은 앱이 실행되는 동안 Visual Studio 디버그 UI를 사용자가 볼 수 있도록 하기 위한 것입니다.

## 앱을 디버그할 때에는 Visual Studio를 사용하여 앱을 일시 중단 이벤트로 보낼 수 있습니다.


* [**디버그 위치** 도구 모음이 표시되는지 확인한 다음 **일시 중단** 아이콘을 클릭합니다.](activate-an-app.md)
* [관련 항목](resume-an-app.md)
* [앱 활성화 처리](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [앱 다시 시작 처리](app-lifecycle.md)

 

 



<!--HONumber=Jun16_HO5-->


