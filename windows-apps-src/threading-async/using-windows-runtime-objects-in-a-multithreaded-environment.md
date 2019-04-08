---
title: 다중 스레드 환경에서 Windows 런타임 개체 사용 | Microsoft Docs
description: 이 문서에서는 .NET Framework가 C# 및 Visual Basic 코드에서 Windows 런타임 또는 Windows 런타임 구성 요소에서 제공하는 개체로의 호출을 처리하는 방식을 설명합니다.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: windows 10, uwp, 타이머, 스레드
ms.localizationpriority: medium
ms.openlocfilehash: f11207a774b1ffcebde95e316634592020e6ed49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631218"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>다중 스레드 환경에서 Windows 런타임 개체 사용
이 문서에서는 .NET Framework가 C# 및 Visual Basic 코드에서 Windows 런타임 또는 Windows 런타임 구성 요소에서 제공하는 개체로의 호출을 처리하는 방식을 설명합니다.

기본적으로 .NET Framework에서는 특수한 처리 없이 여러 스레드의 개체에 액세스할 수 있습니다. 개체에 대한 참조만 있으면 됩니다. Windows 런타임에서는 이러한 개체를 *Agile*이라고 합니다. 대부분의 Windows 런타임 클래스는 Agile이지만 몇몇은 그렇지 않으며, Agile 클래스에도 특수한 처리가 필요할 수 있습니다.

가능한 경우 CLR(공용 언어 런타임)은 Windows 런타임 같은 다른 원본의 개체를 마치 .NET Framework 개체인 것처럼 처리합니다.

- 개체가 [IAgileObject](https://msdn.microsoft.com/library/Hh802476.aspx) 인터페이스를 구현하거나 [MarshalingType.Agile](https://go.microsoft.com/fwlink/p/?LinkId=256023)인 [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) 특성을 갖고 있는 경우 CLR은 이 개체를 Agile로 처리합니다.

- CLR은 대상 개체의 스레딩 컨텍스트에 대해 만들어진 스레드의 호출을 마샬링할 수 있는 경우 마샬링을 투명하게 수행합니다.

- 개체에 [MarshalingType.None](https://go.microsoft.com/fwlink/p/?LinkId=256023)인 [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) 특성이 있는 경우 클래스에서 마샬링 정보를 제공하지 않습니다. CLR은 호출을 마샬링할 수 없으며, 따라서 개체가 만들어진 스레딩 컨텍스트에만 개체를 사용할 수 있다는 내용의 [InvalidCastException](/dotnet/api/system.invalidcastexception) 예외를 throw합니다.

다음 섹션에서는 다양한 원본의 개체에 이 동작이 미치는 효과에 대해 설명합니다.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>C# 또는 Visual Basic으로 작성된 Windows 런타임 구성 요소의 개체
구성 요소에 있는 활성화 가능한 모든 유형은 기본적으로 Agile입니다.

> [!NOTE]
>  Agile이 스레드 안전을 의미하는 것은 아닙니다. 스레드 안전에는 성능 저하가 수반되며 여러 스레드에서 액세스하는 개체가 별로 없기 때문에 Windows 런타임 및 .NET Framework에서 대부분 클래스는 스레드로부터 안전하게 보호되지 않습니다. 필요할 때만 개별 개체에 대한 액세스를 동기화(또는 스레드 안전 클래스 사용)하는 것이 보다 효율적입니다.

Windows 런타임 구성 요소를 작성할 때 기본값을 무시할 수 있습니다. [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) 인터페이스 및 [IAgileObject](https://msdn.microsoft.com/library/Hh802476.aspx) 인터페이스를 참조하세요.

## <a name="objects-from-the-windows-runtime"></a>Windows 런타임의 개체
대부분의 Windows 런타임 클래스는 Agile이며, CLR은 이러한 클래스를 Agile로 처리합니다. 이러한 클래스에 대한 설명서에는 클래스 특성 중 "MarshalingBehaviorAttribute(Agile)"이 나열되어 있습니다. 그러나 이러한 Agile 클래스 구성원 중 일부는(예: XAML 컨트롤) UI 스레드에서 호출되지 않으면 예외를 throw합니다. 예를 들어 다음 코드는 배경 스레드를 사용하여 클릭된 단추의 속성을 설정하려고 시도합니다. 단추의 [콘텐츠](https://go.microsoft.com/fwlink/p/?LinkId=256025) 속성에서 예외를 throw합니다.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

단추의 [Dispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256026) 속성 또는 UI 스레드 컨텍스트에서 존재하는 개체의 `Dispatcher` 속성(예: 단추가 켜져 있는 페이지)을 사용하여 단추에 안전하게 액세스할 수 있습니다. 다음 코드는 [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029) 개체의 [RunAsync](https://go.microsoft.com/fwlink/p/?LinkId=256030) 메서드를 사용하여 UI 스레드에서 호출을 발송합니다.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  `Dispatcher`속성은 다른 스레드에서 호출될 때 예외를 throw하지 않습니다.

UI 스레드에서 생성되는 Windows 런타임 개체의 수명은 스레드 수명으로 제한됩니다. 창이 닫힌 후 UI 스레드에서 개체에 액세스하려고 시도하지 마세요.

XAML 컨트롤을 상속하거나 XAML 컨트롤 집합을 작성하여 사용자 고유의 컨트롤을 만드는 경우 해당 컨트롤은 .NET Framework 개체이므로 Agile입니다. 그러나 해당 기본 클래스 또는 구성 클래스의 구성원을 호출하거나 상속된 구성원을 호출하는 경우 해당 구성원은 UI 스레드를 제외한 다른 스레드에서 호출되면 예외를 throw합니다.

### <a name="classes-that-cant-be-marshaled"></a>마샬링할 수 없는 클래스
마샬링 정보를 제공하지 않는 Windows 런타임 클래스는 [MarshalingType.None](https://go.microsoft.com/fwlink/p/?LinkId=256023)인 [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) 특성을 갖습니다. 이러한 클래스에 대한 설명서에는 "MarshalingBehaviorAttribute(None)" 특성이 나열됩니다.

다음 코드는 UI 스레드에 [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027) 개체를 만든 후 스레드 풀 스레드의 개체 속성을 설정하려고 시도합니다. CLR은 호출을 마샬링할 수 없으며, 따라서 개체가 만들어진 스레딩 컨텍스트에만 개체를 사용할 수 있다는 내용의 [System.InvalidCastException](/dotnet/api/system.invalidcastexception) 예외를 throw합니다.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

또한 [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027)에 대한 설명서에는 클래스 특성 중 "ThreadingAttribute(STA)"가 나열됩니다. UI 스레드 같은 단일 스레드 컨텍스트에서 만들어야 하기 때문입니다.

다른 스레드의 [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027) 개체에 액세스하려는 경우 UI 스레드에 대한 [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029) 개체를 캐시한 후 나중에 해당 스레드에서 호출을 보내는 데 사용할 수 있습니다. 또는 다음 코드에서 보여드리는 것처럼 페이지 같은 XAML 개체에서 발송자를 가져올 수 있습니다.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>C++으로 작성된 Windows 런타임 구성 요소의 개체
기본적으로 구성 요소에 있는 활성화 가능한 클래스는 Agile입니다. 그러나 C++는 스레딩 모델 및 마샬링 동작을 통해 대량의 컨트롤이 가능합니다. 이 문서의 앞부분에서 설명했듯이, CLR은 Agile 클래스를 인식하고, 클래스가 Agile이 아니면 호출을 마샬링하려고 시도하고, 클래스에 마샬링 정보가 없으면 [System.InvalidCastException](/dotnet/api/system.invalidcastexception) 예외를 throw합니다.

UI에서 실행되고 UI 스레드가 아닌 다른 스레드에서 호출되면 예외를 throw하는 개체의 경우 UI 스레드의 [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029) 개체를 사용하여 호출을 발송할 수 있습니다.

## <a name="see-also"></a>참고 항목
[C# 가이드](/dotnet/csharp/)

[Visual Basic 가이드](/dotnet/visual-basic/)
