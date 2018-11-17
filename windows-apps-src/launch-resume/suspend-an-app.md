---
author: TylerMSFT
title: 앱 일시 중단 처리
description: 시스템에서 앱을 일시 중단할 때 중요한 응용 프로그램 데이터를 저장하는 방법을 배웁니다.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 7cb93c410f583884f75f21d9beda03db87c024f9
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7144919"
---
# <a name="handle-app-suspend"></a>앱 일시 중단 처리

**중요 API**

- [**일시 중단**](https://msdn.microsoft.com/library/windows/apps/br242341)

시스템에서 앱을 일시 중단할 때 중요한 응용 프로그램 데이터를 저장하는 방법을 배웁니다. 아래의 예제에서는 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트용 이벤트 처리기를 등록하고 문자열을 파일에 저장합니다.

## <a name="register-the-suspending-event-handler"></a>suspending 이벤트 처리기 등록

시스템이 앱을 일시 중단하기 전에 앱에서 응용 프로그램 데이터를 저장해야 함을 나타내는 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트를 처리하도록 등록합니다.

```csharp
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```

```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
```

```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending +=
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## <a name="save-application-data-before-suspension"></a>일시 중단 전에 응용 프로그램 데이터 저장

앱에서 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 이벤트를 처리하면, 처리기 함수에서 중요한 응용 프로그램 데이터를 저장할 기회가 생깁니다. 간단한 응용 프로그램 데이터를 동기적으로 저장하기 위해 앱에서 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 저장소 API를 사용해야 합니다.

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>리소스 해제

앱이 일시 중단된 동안 다른 앱이 액세스할 수 있도록 단독 리소스와 파일 핸들을 해제해야 합니다. 단독 리소스의 예로는 카메라, I/O 디바이스, 외부 디바이스 및 네트워크 리소스가 있습니다. 단독 리소스와 파일 핸들을 명시적으로 해제하면 앱이 일시 중단된 동안 다른 앱이 액세스하는 데 도움이 됩니다. 앱이 다시 시작되면 단독 리소스와 파일 핸들을 다시 획득해야 합니다.

## <a name="remarks"></a>설명

사용자가 다른 앱이나 데스크톱 또는 시작 화면으로 전환할 때마다 시스템에서 앱을 일시 중단합니다. 사용자가 다시 돌아올 때마다 시스템에서 앱을 다시 시작합니다. 시스템에서 앱을 다시 시작할 때, 변수와 데이터 구조의 콘텐츠는 시스템에서 앱을 일시 중단하기 전과 동일합니다. 앱은 중단되었던 곳에서 정확히 복원되므로, 사용자에게는 앱이 배경에서 실행되고 있었던 것처럼 보입니다.

앱이 일시 중단된 동안 시스템은 앱과 데이터를 메모리에 유지하려고 합니다. 그러나 앱을 메모리에 유지할 리소스가 없으면 시스템은 앱을 종료합니다. 사용자가 일시 중단 후 종료된 앱으로 다시 돌아오면, 시스템은 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 이벤트를 전송하며 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 메서드에서 응용 프로그램 데이터를 복원해야 합니다.

시스템은 앱 종료 시 앱에 알리지 않으므로, 앱은 일시 중단될 때 응용 프로그램 데이터를 저장하고 단독 리소스와 파일 핸들을 해제하며 앱이 종료 후 활성화될 때 이 리소스와 파일 핸들을 복원해야 합니다.

처리기 내에서 비동기 호출을 수행하면 컨트롤이 해당 비동기 호출에서 즉시 반환됩니다. 즉, 비동기 호출이 아직 완료되지 않은 경우에도 실행이 이벤트 처리기에서 반환될 수 있고 앱이 다음 상태로 이동합니다. 이벤트 처리기에 전달된 [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4) 개체의 [**GetDeferral**](http://aka.ms/Kt66iv) 메서드를 사용하여 반환된 [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 개체에서 [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) 메서드가 호출될 때까지 일시 중단을 지연할 수 있습니다.

지연을 사용해도 앱이 종료되기 전에 코드를 실행할 수 있는 시간이 증가하지는 않습니다. 단지 지연의 *Complete* 메서드 호출이나 기한 경과 중 *더 빠른 시간*까지 종료가 지연됩니다. 일시 중단 상태 사용 [ **ExtendedExecutionSession** 시간을 확장 하려면](run-minimized-with-extended-execution.md)

> [!NOTE]
> Windows8.1에서 시스템 응답을 향상 하려면 앱에 제공 됩니다 낮은 우선 순위 액세스 리소스는 일시 중단 시 합니다. 이 새 우선 순위를 지원하기 위해 일시 중단 작업 제한 시간이 확장되어 앱에 Windows의 일반 우선 순위에 대한 5초 제한 시간이나 Windows Phone의 1-10초 제한 시간에 해당하는 제한 시간이 부여됩니다. 이 제한 시간은 확장하거나 변경할 수 없습니다.

**Visual Studio를 사용하는 디버깅에 대한 참고 사항:** Visual Studio에서는 Windows가 디버거에 연결되어 있는 앱을 일시 중단하지 못하도록 합니다. 이렇게 하는 것은 앱이 실행되는 동안 Visual Studio 디버그 UI를 사용자가 볼 수 있도록 하기 위한 것입니다. 앱을 디버그할 때에는 Visual Studio를 사용하여 앱을 일시 중단 이벤트로 보낼 수 있습니다. **디버그 위치** 도구 모음이 표시되는지 확인한 다음 **일시 중단** 아이콘을 클릭합니다.

## <a name="related-topics"></a>관련 항목

* [앱 수명 주기](app-lifecycle.md)
* [앱 활성화 처리](activate-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [시작, 일시 중단 및 다시 시작에 대한 UX 지침](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [확장 실행](run-minimized-with-extended-execution.md)

 

 
