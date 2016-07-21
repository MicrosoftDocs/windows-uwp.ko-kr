---
author: TylerMSFT
title: "앱 다시 시작 처리"
description: "시스템에서 앱을 다시 시작할 때 표시 콘텐츠를 새로 고치는 방법을 알아봅니다."
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
translationtype: Human Translation
ms.sourcegitcommit: e6957dd44cdf6d474ae247ee0e9ba62bf17251da
ms.openlocfilehash: dd3d75c7f3dfe325324e1fe31c039cd207b68d0b

---

# 앱 다시 시작 처리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**다시 시작**](https://msdn.microsoft.com/library/windows/apps/br242339)

시스템에서 앱을 다시 시작할 때 표시 콘텐츠를 새로 고치는 방법을 알아봅니다. 이 항목의 예제에서는 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트의 이벤트 처리기를 등록합니다.

**로드맵:** 이 항목은 다음 항목과 연관되어 있습니다. 참고 항목:

-   [C# 또는 Visual Basic으로 작성한 Windows 런타임 앱용 로드맵](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [C++로 작성한 Windows 런타임 앱용 로드맵](https://msdn.microsoft.com/library/windows/apps/hh700360)

## resuming 이벤트 처리기 등록

사용자가 앱에서 다른 곳으로 전환했다가 돌아왔음을 나타내는 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트를 처리하도록 등록합니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
>    }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Resuming, AddressOf App_Resuming
>    End Sub
>
> End Class
> ```
> ```cpp
> MainPage::MainPage()
> {
>     InitializeComponent();
>     Application::Current->Resuming +=
>         ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
> }
> ```

## 일시 중단 후 표시 콘텐츠 새로 고침

앱에서 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트를 처리하면 표시 콘텐츠를 새로 고칠 기회가 생깁니다.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data
> }
> ```

> **참고** [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트는 UI 스레드에서 발생되지 않으므로 처리기에서 수행하려는 작업일 경우 디스패처를 사용하여 UI 스레드에 연결하고 UI에 업데이트를 주입해야 합니다.

## 설명


사용자가 다른 앱 또는 데스크톱으로 전환할 때마다 시스템에서 앱을 일시 중단합니다. 사용자가 다시 돌아올 때마다 시스템에서 앱을 다시 시작합니다. 시스템에서 앱을 다시 시작할 때, 변수와 데이터 구조의 콘텐츠는 시스템에서 앱을 일시 중단하기 전과 동일합니다. 앱은 중단되었던 곳에서 정확히 복원되므로, 사용자에게는 앱이 배경에서 실행되고 있었던 것처럼 보입니다. 그러나 앱은 상당히 오랜 기간 동안 일시 중단된 것일 수 있으므로, 일시 중단 기간 중에 변경되었을 수 있는 표시 콘텐츠를 새로 고쳐야 합니다. 예를 들면 뉴스 피드나 사용자 위치 등이 있습니다.

앱에 새로 고칠 표시 콘텐츠가 없으면 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트를 처리할 필요가 없습니다.

> **참고** 앱이 Visual Studio 디버거에 연결되어 있는 경우 **다시 시작** 이벤트로 보낼 수 있습니다. **디버그 위치 도구 모음**이 표시되는지 확인하고 **일시 중단** 아이콘 옆에 있는 드롭다운을 클릭합니다. 그런 다음 **다시 시작**을 선택합니다.

> **참고** Windows Phone 스토어 앱에서는 앱이 현재 일시 중단되었으며 사용자가 기본 타일이나 앱 목록에서 앱을 다시 시작하는 경우에도 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 이벤트 뒤에 항상 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)가 발생합니다. 현재 창에 이미 설정된 콘텐츠가 있는 경우 앱에서 초기화를 건너뛸 수 있습니다. [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 속성을 검사하여 앱이 기본 타일에서 시작되었는지 또는 보조 타일에서 시작되었는지 확인할 수 있으며, 해당 정보에 따라 새로운 환경을 표시할지 또는 앱 환경을 다시 시작할지 결정할 수 있습니다.

## 관련 항목

* [앱 활성화 처리](activate-an-app.md)
* [앱 일시 중단 처리](suspend-an-app.md)
* [앱 일시 중단 및 다시 시작에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [앱 수명 주기](app-lifecycle.md)



<!--HONumber=Jun16_HO5-->


