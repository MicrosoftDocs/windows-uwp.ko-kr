---
title: 다중 스레드 환경에서 Windows 런타임 개체 사용 | Microsoft Docs
description: '이 문서에서는 .NET Framework에서 c #에서의 호출을 처리 하는 방법과 Windows 런타임에서 제공 하는 개체 또는 Windows 런타임 구성 요소에서 제공 하는 Visual Basic 코드를 처리 하는 방법을 설명 합니다.'
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: windows 10, uwp, 타이머, 스레드
ms.localizationpriority: medium
ms.openlocfilehash: 4287ed5fd5659b7d39d52ada9e3958c592771d59
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164137"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>다중 스레드 환경에서 Windows 런타임 개체 사용
이 문서에서는 .NET Framework에서 c #에서의 호출을 처리 하는 방법과 Windows 런타임에서 제공 하는 개체 또는 Windows 런타임 구성 요소에서 제공 하는 Visual Basic 코드를 처리 하는 방법을 설명 합니다.

.NET Framework에서는 기본적으로 특수한 처리 없이 여러 스레드에서 모든 개체에 액세스할 수 있습니다. 개체에 대한 참조만 있으면 됩니다. Windows 런타임에서 이러한 개체를 *agile*이라고 합니다. 대부분의 Windows 런타임 클래스는 agile 이지만 몇몇 클래스는 그렇지 않으며 agile 클래스도 특별 한 처리가 필요할 수 있습니다.

가능 하면 CLR (공용 언어 런타임)은 Windows 런타임 같은 다른 소스의 개체를 .NET Framework 개체인 것 처럼 처리 합니다.

- 개체가 [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) 인터페이스를 구현 하거나 [marshalingtype.inhibit](/uwp/api/Windows.Foundation.Metadata.MarshalingType)를 사용 하는 [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) 특성이 있는 경우 CLR은이를 agile로 처리 합니다.

- CLR에서 대상 개체의 스레딩 컨텍스트에 적용하는 스레드의 호출을 마샬링할 수 있는 경우는 투명하게 합니다.

- 개체의 [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) 특성이 [marshalingtype.inhibit](/uwp/api/Windows.Foundation.Metadata.MarshalingType)인 경우 클래스는 마샬링 정보를 제공 하지 않습니다. CLR은 호출을 마샬링할 수 없으므로 개체가 만들어진 스레딩 컨텍스트에서만 개체를 사용할 수 있음을 나타내는 메시지와 함께 [InvalidCastException](/dotnet/api/system.invalidcastexception) 예외를 throw 합니다.

다음 섹션에서는 다양한 소스에서 개체에 이 동작이 미치는 효과를 설명합니다.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>C # 또는 Visual Basic 작성 된 Windows 런타임 구성 요소의 개체
활성화할 수 있는 구성 요소의 모든 유형은 기본적으로 agile합니다.

> [!NOTE]
>  민첩성은 스레드로부터의 안전성을 의미하지 않습니다. Windows 런타임와 .NET Framework 둘 다에서 대부분의 클래스는 스레드로부터 안전 하지 않습니다. 스레드 보안에는 성능 비용이 있고 대부분의 개체는 여러 스레드에서 액세스 하지 않기 때문입니다. 필요한 경우에만 개별 개체로 액세스를 동기화하거나 스레드로부터 안전한 클래스를 사용하는 것이 더 효율적입니다.

Windows 런타임 구성 요소를 작성 하는 경우 기본값을 재정의할 수 있습니다. [에서 icustomqueryinterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) 인터페이스 및 [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) 인터페이스를 참조 하세요.

## <a name="objects-from-the-windows-runtime"></a>Windows 런타임 개체
Windows 런타임의 대부분 클래스는 agile 이며 CLR은 agile로 처리 합니다. 이러한 클래스에 대한 설명서의 클래스 특성에는 "MarshalingBehaviorAttribute(Agile)"가 나열됩니다. 그러나 XAML 컨트롤과 같은 이러한 agile 클래스의 일부 멤버에서는 마치 UI 스레드에서 호출되지 않은 것처럼 예외를 throw합니다. 예를 들어, 다음 코드에서는 배경 스레드를 사용하여 클릭된 단추의 속성을 설정하려고 합니다. 단추의 [Content](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) 속성이 예외를 throw 합니다.

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

[디스패처](/uwp/api/Windows.UI.Xaml.DependencyObject) 속성이 나 `Dispatcher` UI 스레드의 컨텍스트에 있는 모든 개체의 속성 (예: 단추가 있는 페이지)을 사용 하 여 안전 하 게 단추에 액세스할 수 있습니다. 다음 코드는 [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체의 [runasync](/uwp/api/Windows.UI.Core.CoreDispatcher) 메서드를 사용 하 여 UI 스레드에서 호출을 디스패치합니다.

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
>  `Dispatcher`속성은 다른 스레드에서 호출 될 때 예외를 throw 하지 않습니다.

UI 스레드에서 만들어진 Windows 런타임 개체의 수명은 스레드의 수명에 따라 제한 됩니다. 창이 닫힌 후 UI 스레드의 개체에 액세스하지 마세요.

XAML 컨트롤을 상속하거나의 XAML 컨트롤의 집합을 작성하여 컨트롤을 만들면 컨트롤은 .NET Framework 개체이기 때문에 agile합니다. 그러나 기본 클래스나 구성 클래스의 멤버를 호출하거나 호출에서 멤버를 상속한 경우에는 UI 스레드를 제외한 모든 스레드에서 호출할 경우에 해당 멤버에서 예외를 throw합니다.

### <a name="classes-that-cant-be-marshaled"></a>마샬링할 수 없는 클래스
마샬링 정보를 제공 하지 않는 Windows 런타임 클래스에는 Marshalingtype.inhibit가 포함 된 [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) 특성이 있습니다 [.](/uwp/api/Windows.Foundation.Metadata.MarshalingType) 이러한 클래스에 대한 설명서의 특성에는 "MarshalingBehaviorAttribute(None)"가 나열됩니다.

다음 코드는 UI 스레드에 [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 개체를 만든 다음 스레드 풀 스레드에서 개체의 속성을 설정 하려고 합니다. CLR에서 호출을 마샬링할 수 없으며 개체가 만들어진 스레딩 컨텍스트에서만 개체를 사용할 수 있음을 나타내는 메시지를 사용 하 여 [InvalidCastException](/dotnet/api/system.invalidcastexception) 예외를 throw 합니다.

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

[CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 에 대 한 설명서에는 UI 스레드와 같은 단일 스레드 컨텍스트에서 생성 되어야 하기 때문에 클래스의 특성 중 "THREADINGATTRIBUTE (STA)"도 나열 됩니다.

다른 스레드에서 [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 개체에 액세스 하려는 경우 UI 스레드에 대 한 [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체를 캐시 하 고 나중에이를 사용 하 여 해당 스레드에서 호출을 디스패치할 수 있습니다. 또는 다음 코드에서와 같이 페이지 등의 XAML 개체에서 발송자를 가져올 수 있습니다.

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

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>C + +로 작성 된 Windows 런타임 구성 요소의 개체
기본적으로 구성 요소에서 활성화할 수 있는 클래스는 agile합니다. 그러나 c + +에서는 스레딩 모델과 마샬링 동작을 상당히 제어할 수 있습니다. 이 문서의 앞부분에서 설명한 것 처럼 CLR은 agile 클래스를 인식 하 고, 클래스가 agile이 아닐 때 호출을 마샬링하 려 고, 클래스에 마샬링 정보가 없을 때 [InvalidCastException](/dotnet/api/system.invalidcastexception) 예외를 throw 합니다.

Ui 스레드에서 실행 되 고 ui 스레드가 아닌 스레드에서 호출 될 때 예외를 throw 하는 개체의 경우 UI 스레드의 [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체를 사용 하 여 호출을 디스패치할 수 있습니다.

## <a name="see-also"></a>관련 항목
[C# 가이드](/dotnet/csharp/)

[Visual Basic 가이드](/dotnet/visual-basic/)