---
title: 앱 다시 시작 처리
description: 시스템이 앱을 다시 시작할 때 표시 되는 콘텐츠를 새로 고치는 방법에 대해 알아봅니다.
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
ms.openlocfilehash: d7898dd727ffb4c9255b66725ea69d2005e8d650
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175147"
---
# <a name="handle-app-resume"></a>앱 다시 시작 처리

**중요 API**

- [**Resuming**](/uwp/api/windows.ui.xaml.application.resuming)

시스템이 앱을 다시 시작할 때 UI를 새로 고치는 위치를 알아봅니다. 이 항목의 예제에서는 다시 [**시작**](/uwp/api/windows.ui.xaml.application.resuming) 이벤트에 대 한 이벤트 처리기를 등록 합니다.

## <a name="register-the-resuming-event-handler"></a>다시 시작 이벤트 처리기를 등록 합니다.

를 등록 하 여 다시 [**시작**](/uwp/api/windows.ui.xaml.application.resuming) 이벤트를 처리 합니다 .이 이벤트는 사용자가 앱에서 다른 위치로 전환 된 후 다시 해당 이벤트로 전환 되었음을 나타냅니다.

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

## <a name="refresh-displayed-content-and-reacquire-resources"></a>표시 된 콘텐츠 새로 고침 및 리소스 다시 가져오기를

사용자가 다른 앱 이나 바탕 화면으로 전환한 후 몇 초 후에 시스템이 앱을 일시 중단 합니다. 사용자가 다시 전환할 때 시스템이 앱을 다시 시작 합니다. 시스템이 앱을 다시 시작할 때 변수와 데이터 구조의 내용은 시스템이 앱을 일시 중단 하기 전과 동일 합니다. 시스템이 종료 되 면 응용 프로그램이 복원 됩니다. 사용자에 게 앱이 백그라운드에서 실행 되는 것 처럼 표시 됩니다.

앱이 다시 [**시작**](/uwp/api/windows.ui.xaml.application.resuming) 이벤트를 처리할 때 앱이 몇 시간 또는 며칠 동안 일시 중단 될 수 있습니다. 뉴스 피드 또는 사용자 위치와 같이 앱이 일시 중단 된 상태에서 부실 해질 수 있는 모든 콘텐츠를 새로 고쳐야 합니다.

또한 파일 핸들, 카메라, i/o 장치, 외부 장치 및 네트워크 리소스와 같이 앱이 일시 중단 되었을 때 릴리스된 모든 배타 리소스를 복원 하는 것이 좋습니다.

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
> 다시 [**시작**](/uwp/api/windows.ui.xaml.application.resuming) 이벤트는 ui 스레드에서 발생 하지 않기 때문에 ui에 대 한 호출을 디스패치하기 위해 처리기에서 디스패처를 사용 해야 합니다.

## <a name="remarks"></a>설명

앱이 Visual Studio 디버거에 연결 되어 있으면 일시 중단 되지 않습니다. 그러나 디버거에서 일시 중단 한 후 코드를 디버그할 수 있도록 **다시 시작** 이벤트를 보낼 수 있습니다. **디버그 위치 도구 모음이** 표시 되는지 확인 하 고 **일시 중단** 아이콘 옆에 있는 드롭다운을 클릭 합니다. 그런 다음 **다시 시작**을 선택 합니다.

앱이 현재 일시 중단 되 고 사용자가 기본 타일 또는 앱 목록에서 앱을 다시 시작 하는 경우에도, Windows Phone 스토어 앱의 경우 다시 [**시작**](/uwp/api/windows.ui.xaml.application.resuming) 이벤트는 항상 [**onlaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)됩니다. 현재 창에 이미 콘텐츠가 설정 되어 있으면 앱에서 초기화를 건너뛸 수 있습니다. [**LaunchActivatedEventArgs**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) 속성을 확인 하 여 앱이 기본 또는 보조 타일에서 시작 되었는지 확인 하 고 해당 정보에 따라 새로 제공 하거나 앱 환경을 다시 시작할지 여부를 결정할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [앱 수명 주기](app-lifecycle.md)
* [앱 활성화 처리](activate-an-app.md)
* [앱 일시 중단 처리](suspend-an-app.md)