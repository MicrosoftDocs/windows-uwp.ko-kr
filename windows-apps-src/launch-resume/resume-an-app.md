---
title: 앱 다시 시작 처리
description: 시스템에서 앱을 다시 시작할 때 표시 콘텐츠를 새로 고치는 방법을 알아봅니다.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: d7f26e7a4ae05aaf3e197843e18273cb765754a0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371466"
---
# <a name="handle-app-resume"></a>앱 다시 시작 처리

**중요 한 Api**

- [**다시 시작**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming)

시스템이 앱을 다시 시작할 때 UI를 새로 고치는 위치에 대해 알아봅니다. 이 항목의 예제에서는 [**Resuming**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) 이벤트의 이벤트 처리기를 등록합니다.

## <a name="register-the-resuming-event-handler"></a>resuming 이벤트 처리기 등록

사용자가 앱에서 다른 곳으로 전환했다가 돌아왔음을 나타내는 [**Resuming**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) 이벤트를 처리하도록 등록합니다.

```csharp
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```

```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Resuming({ this, &MainPage::App_Resuming });
}
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming +=
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>표시 콘텐츠 새로 고침 및 리소스 다시 취득

사용자가 다른 앱 또는 데스크톱으로 전환한 후 몇 초 동안 시스템에서 앱을 일시 중단합니다. 사용자가 다시 돌아올 때 시스템에서 앱을 다시 시작합니다. 시스템에서 앱을 다시 시작할 때, 변수와 데이터 구조의 콘텐츠는 시스템에서 앱을 일시 중단하기 전과 동일합니다. 시스템은 중단되었던 곳에서 앱을 복원합니다. 사용자에게는 앱이 백그라운드에서 실행된 것처럼 나타납니다.

앱에서 [**Resuming**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) 이벤트를 처리한 때 몇 시간 또는 며칠 동안 앱이 일시 중단될 수 있습니다. 앱이 일시 중단된 동안 오래된 내용일 수 있는 모든 콘텐츠(뉴스 피드나 사용자 위치 등)를 새로 고쳐야 합니다.

앱이 일시 중단되었을 때 릴리스된 모든 단독 리소스(파일 핸들, 카메라, I/O 디바이스, 외부 디바이스 및 네트워크 리소스 등)를 복원하는 것이 좋습니다.

```csharp
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.

    End Sub
>
End Class
```

```cppwinrt
void MainPage::App_Resuming(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::Foundation::IInspectable const& /* e */)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

> [!NOTE]
> 때문에 합니다 [ **다시 시작 중** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) UI 스레드에서 이벤트가 발생 하지 않습니다, 디스패처 처리기에서 UI에 대 한 모든 호출을 발송 하는 데 사용 해야 합니다.

## <a name="remarks"></a>설명

앱이 Visual Studio 디버거에 연결되어 있는 경우 일시 중단되지 않습니다. 그러나 디버거에서 일시 중단하고 **Resume** 이벤트를 전송하여 코드를 디버그할 수 있습니다. **디버그 위치 도구 모음**이 표시되는지 확인하고 **일시 중단** 아이콘 옆에 있는 드롭다운을 클릭합니다. 그런 다음 **다시 시작**을 선택합니다.

Windows Phone 스토어 앱에서는 앱이 현재 일시 중단되었으며 사용자가 기본 타일이나 앱 목록에서 앱을 다시 시작하는 경우에도 [**Resuming**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) 이벤트 뒤에 항상 [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)이(가) 발생합니다. 현재 창에 이미 설정된 콘텐츠가 있는 경우 앱에서 초기화를 건너뛸 수 있습니다. [  **LaunchActivatedEventArgs.TileId**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) 속성을 검사하여 앱이 기본 타일에서 시작되었는지 또는 보조 타일에서 시작되었는지 확인할 수 있으며, 해당 정보에 따라 새로운 환경을 표시할지 또는 앱 환경을 다시 시작할지 결정할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [앱 수명 주기](app-lifecycle.md)
* [앱 활성화 처리](activate-an-app.md)
* [앱 일시 중단 처리](suspend-an-app.md)
