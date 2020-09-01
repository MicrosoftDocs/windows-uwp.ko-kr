---
title: 앱 일시 중단 처리
description: 시스템이 앱을 일시 중단 하는 경우 중요 한 응용 프로그램 데이터를 저장 하는 방법을 알아봅니다.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 1c75200a768efd258aa84b20493b9d296578a05c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171827"
---
# <a name="handle-app-suspend"></a>앱 일시 중단 처리

**중요 API**

- [**Suspending**](/uwp/api/windows.ui.xaml.application.suspending)

시스템이 앱을 일시 중단 하는 경우 중요 한 응용 프로그램 데이터를 저장 하는 방법을 알아봅니다. 이 예제에서는 [**일시 중단**](/uwp/api/windows.ui.xaml.application.suspending) 이벤트에 대 한 이벤트 처리기를 등록 하 고 문자열을 파일에 저장 합니다.

## <a name="register-the-suspending-event-handler"></a>일시 중단 이벤트 처리기 등록

응용 프로그램에서 응용 프로그램 데이터를 일시 중단 하기 전에 응용 프로그램 데이터를 저장 해야 함을 나타내는 [**일시 중단**](/uwp/api/windows.ui.xaml.application.suspending) 이벤트를 처리 하려면 등록 합니다.

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

응용 프로그램에서 [**일시 중단**](/uwp/api/windows.ui.xaml.application.suspending) 이벤트를 처리 하는 경우 처리기 함수에 중요 한 응용 프로그램 데이터를 저장할 수 있습니다. 앱은 [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) storage API를 사용 하 여 간단한 응용 프로그램 데이터를 동기식으로 저장 해야 합니다.

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

## <a name="release-resources"></a>릴리스 리소스

앱이 일시 중단 된 상태에서 다른 앱이 액세스할 수 있도록 배타 리소스 및 파일 핸들을 해제 해야 합니다. 전용 리소스의 예로는 카메라, i/o 장치, 외부 장치 및 네트워크 리소스가 있습니다. 배타적 리소스 및 파일 핸들을 명시적으로 해제 하면 앱이 일시 중단 된 동안 다른 앱에서 해당 리소스에 액세스할 수 있습니다. 앱이 다시 시작 되 면 단독 리소스 및 파일 핸들을 다시 가져와야 합니다.

## <a name="remarks"></a>설명

시스템은 사용자가 다른 앱 이나 바탕 화면 또는 시작 화면으로 전환할 때마다 앱을 일시 중단 합니다. 사용자가 다시 전환할 때마다 시스템이 앱을 다시 시작 합니다. 시스템이 앱을 다시 시작할 때 변수 및 데이터 구조의 내용은 시스템이 앱을 일시 중단 하기 전과 동일 합니다. 시스템은 백그라운드에서 실행 중인 것 처럼 사용자에 게 표시 되도록, 중단 된 위치에서 앱을 복원 합니다.

시스템은 일시 중단 된 상태에서 앱과 해당 데이터를 메모리에 유지 하려고 합니다. 그러나 시스템에 앱을 메모리에 보관 하는 리소스가 없는 경우 시스템에서 앱이 종료 됩니다. 사용자가 종료 된 일시 중단 된 앱으로 다시 전환 되 면 시스템은 [**활성화**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 된 이벤트를 보내고 해당 응용 프로그램 데이터를 [**onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) 된 메서드에서 복원 해야 합니다.

시스템은 종료 될 때 앱에 알리지 않으므로 응용 프로그램에서 응용 프로그램 데이터를 저장 하 고 일시 중단 된 경우 전용 리소스 및 파일 핸들을 해제 하 고 종료 후에 앱이 활성화 될 때 응용 프로그램을 복원 해야 합니다.

처리기 내에서 비동기 호출을 수행 하는 경우 해당 비동기 호출에서 제어가 즉시 반환 됩니다. 즉, 실행은 이벤트 처리기에서 반환 될 수 있으며 비동기 호출이 아직 완료 되지 않은 경우에도 앱이 다음 상태로 이동 됩니다. 반환 된 [**GetDeferral**](/uwp/api/Windows.ApplicationModel) 개체에서 [**Complete**](/uwp/api/windows.foundation.deferral.complete) 메서드를 호출할 때까지 일시 중단을 지연 시키려면 이벤트 처리기에 전달 되는 [**EnteredBackgroundEventArgs**](/uwp/api/Windows.ApplicationModel) 개체의 메서드를 사용 [**합니다.**](/uwp/api/windows.foundation.deferral)

지연이 발생 하면 앱이 종료 되기 전에 코드를 실행 해야 하는 양이 증가 하지 않습니다. 이는 지연의 *Complete* 메서드를 호출*하거나 최종 기한*에 도달할 때까지 종료를 지연 합니다. 일시 중단 상태의 시간을 확장 하려면 ExtendedExecutionSession을 사용 합니다. [ **ExtendedExecutionSession**](run-minimized-with-extended-execution.md)

> [!NOTE]
> Windows 8.1에서 시스템 응답성을 개선 하기 위해 앱은 일시 중단 된 후 리소스에 대 한 우선 순위가 낮은 액세스를 제공 합니다. 이 새로운 우선 순위를 지원 하기 위해 일시 중단 작업 시간 제한은 확장 되어 앱이 Windows에서 보통 우선 순위에 대 한 5 초 시간 제한에 해당 하거나 Windows Phone에서 1 ~ 10 초 사이에 해당 합니다. 이 시간 제한 기간은 확장 하거나 변경할 수 없습니다.

**Visual Studio를 사용한 디버깅에 대 한 참고 사항:** Visual Studio는 Windows에서 디버거에 연결 된 앱을 일시 중단 하지 못하도록 합니다. 이는 앱이 실행 되는 동안 사용자가 Visual Studio 디버그 UI를 볼 수 있도록 하기 위한 것입니다. 앱을 디버깅 하는 경우 Visual Studio를 사용 하 여 일시 중단 이벤트를 보낼 수 있습니다. **디버그 위치** 도구 모음이 표시 되는지 확인 하 고 **일시 중지** 아이콘을 클릭 합니다.

## <a name="related-topics"></a>관련 항목

* [앱 수명 주기](app-lifecycle.md)
* [앱 활성화 처리](activate-an-app.md)
* [앱 다시 시작 처리](resume-an-app.md)
* [시작, 일시 중단 및 다시 시작에 대 한 UX 지침](./index.md)
* [확장 실행](run-minimized-with-extended-execution.md)

 

 