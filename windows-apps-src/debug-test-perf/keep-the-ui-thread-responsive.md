---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: UI 스레드 응답 유지
description: 사용자는 컴퓨터 유형에 관계없이 계산하는 동안 앱이 계속 응답할 것으로 기대합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b9a129e8b780e85df2c38c50ab712641d3849a34
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339856"
---
# <a name="keep-the-ui-thread-responsive"></a>UI 스레드 응답 유지


사용자는 컴퓨터 유형에 관계없이 계산하는 동안 앱이 계속 응답할 것으로 기대합니다. 이는 앱에 따라 다른 항목을 의미합니다. 일부는 이는 더 실제적인 물리학을 제공하거나, 디스크나 웹에서 더 빠르게 데이터를 로드하거나, 빠르게 복잡한 화면을 제공하고 페이지를 탐색하거나, 즉시 방향을 찾거나, 데이터를 빠르게 처리하도록 변환됩니다. 계산 유형에 관계없이 사용자는 앱이 입력에 대해 작동하고 “판단”하는 동안 응답하지 않는 것으로 보이지 않기를 원합니다.

앱은 이벤트 기반이므로 코드는 이벤트에 대한 응답으로 작업을 수행한 후 다음 이벤트까지 유휴 상태를 유지합니다. UI에 대한 플랫폼 코드(레이아웃, 입력, 이벤트 발생 등)와 앱 코드 모두 동일한 UI 스레드에서 실행됩니다. 이 스레드에서는 한 번에 하나의 명령만 실행될 수 있으므로 앱 코드가 이벤트를 처리하는 데 너무 오래 걸리는 경우 프레임워크에서 레이아웃을 실행할 수 없거나 사용자 개입을 나타내는 새 이벤트를 발생시킬 수 있습니다. 앱의 응답성은 작업을 처리하는 UI 스레드의 가용성과 관련이 있습니다.

UI 유형 만들기 및 해당 멤버 액세스를 포함하여 UI 스레드에 대한 거의 모든 변경 작업에는 UI 스레드를 사용해야 합니다. 백그라운드 스레드에서 UI를 업데이트할 수는 없지만 [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)를 사용하여 코드가 해당 위치에서 실행되도록 메시지를 게시할 수 있습니다.

> **참고**  The 한 가지 예외는 입력 처리 방법 또는 기본 레이아웃에 영향을 주지 않는 UI 변경을 적용할 수 있는 별도의 렌더링 스레드가 있다는 것입니다. 예를 들어 레이아웃에 영향을 주지 않는 많은 애니메이션 및 전환은 이 렌더링 스레드에서 실행할 수 있습니다.

## <a name="delay-element-instantiation"></a>요소 인스턴스화 지연

앱의 가장 느린 단계에는 시작 및 보기 전환이 포함할 수 있습니다. 초기에 사용자에게 표시되는 UI를 불러오는 데 필요한 것 이상으로 많은 작업을 수행하지 마세요. 예를 들어 점진적으로 공개되는 UI 및 팝업 내용에 대한 UI를 만들지 마세요.

-   [x:Load attribute](../xaml-platform/x-load-attribute.md) 또는 [x:DeferLoadStrategy](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute)를 사용하여 요소를 지연-인스턴스화합니다.
-   요청 시 프로그래밍 방식으로 트리에 요소를 삽입합니다.

[**CoreDispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync) 는 UI 스레드가 사용 중이 아닌 경우 처리 하기 위해 작동 합니다.

## <a name="use-asynchronous-apis"></a>비동기 API 사용

이 플랫폼은 앱을 응답 가능한 상태로 유지할 수 있도록 여러 API의 비동기 버전을 제공합니다. 비동기 API를 사용하면 활성 실행 스레드가 상당한 시간 동안 차단되는 일이 없습니다. UI 스레드에서 API를 호출할 때 비동기 버전이 있다면 이 버전을 사용하세요. **async** 패턴으로 프로그래밍하는 방법에 대한 자세한 내용은 [비동기 프로그래밍](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps) 또는 [C# 또는 Visual Basic에서 비동기식 API 호출](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)을 참조하세요.

## <a name="offload-work-to-background-threads"></a>작업을 백그라운드 스레드로 오프로드

신속하게 반환하도록 이벤트 처리기를 작성합니다. 적지 않은 작업을 수행해야 하는 경우 백그라운드 스레드에서 작업을 예약하고 반환합니다.

C#의 **await** 연산자, Visual Basic의 **Await** 연산자 또는 C++의 대리자를 사용하여 작업을 비동기 방식으로 예약할 수 있습니다. 그러나 이렇게 해도 예약한 작업이 백그라운드 스레드에서 실행된다고 보장할 수 없습니다. UWP(유니버설 Windows 플랫폼) API 일정은 대부분 백그라운드에서 작동하지만 **await**만 사용하거나 대리자를 사용하여 앱 코드를 호출하면 해당 대리자가 메서드가 UI 스레드에서 실행됩니다. 백그라운드 스레드에서 앱 코드를 실행하려면 명시적으로 지정해야 합니다. C# 및 Visual Basic 작업에 [**Task.Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run) 코드를 전달하여 이 작업을 수행할 수 있습니다.

UI 요소는 UI 스레드에서만 액세스할 수도 있습니다. 백그라운드 작업을 시작하기 전에 UI 스레드를 사용하여 UI 요소에 액세스하거나 백그라운드 스레드에서 [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 또는 [**CoreDispatcher.RunIdleAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync)를 사용합니다.

백그라운드 스레드에서 수행할 수 있는 작업의 예는 게임에서 컴퓨터 AI를 계산하는 것입니다. 컴퓨터의 다음 움직임을 계산하는 코드를 실행하는 데에는 시간이 오래 걸릴 수 있습니다.

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

이 예제에서는 UI 스레드의 응답을 유지하기 위해 `NextMove_Click` 처리기가 **await**에서 반환합니다. 그러나 백그라운드 스레드에서 실행되는 `ComputeNextMove`가 완료된 후에는 해당 처리기에서 실행이 다시 선택됩니다. 처리기의 나머지 코드는 결과와 함께 UI를 업데이트합니다.

> **참고**@no__t-비슷한 시나리오에 사용할 수 있는 UWP에 대 한 [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) 및 [**windows.system.threading.threadpooltimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer) API도 있습니다. 자세한 내용은 [스레딩 및 비동기 프로그래밍](https://docs.microsoft.com/windows/uwp/threading-async/index)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [사용자 지정 사용자 조작](https://docs.microsoft.com/windows/uwp/design/layout/index)
