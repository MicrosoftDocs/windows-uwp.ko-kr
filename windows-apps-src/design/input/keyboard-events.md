---
author: Karl-Bridge-Microsoft
Description: Respond to keystroke actions from hardware or software keyboards in your apps using both keyboard and class event handlers.
title: 키보드 이벤트
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: 키보드, 게임 패드, 리모컨, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 조작, 위쪽 키, 아래쪽 키
ms.author: kbridge
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 78448081b81e7e28c4b97fcfdd7aa71ae32aeb0c
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5473838"
---
# <a name="keyboard-events"></a>키보드 이벤트

## <a name="keyboard-events-and-focus"></a>키보드 이벤트 및 포커스

다음 키보드 이벤트는 하드웨어 및 터치 키보드 둘 다에서 발생할 수 있습니다.

| 이벤트                                      | 설명                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | 키를 누를 때 발생합니다.  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | 키를 놓을 때 발생합니다. |

> [!IMPORTANT]
> 일부 Windows 런타임 컨트롤은 입력 이벤트를 내부적으로 처리합니다. 이 경우 이벤트 수신기가 연결된 처리기를 호출하지 않으므로 입력 이벤트가 발생하지 않는 것처럼 보일 수도 있습니다. 일반적으로 이러한 키 하위 집합은 기본 키보드 접근성을 기본적으로 지원하기 위해 클래스 처리기에서 처리됩니다. 예를 들어 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 클래스는 Space 키와 Enter 키 둘 다에 대해 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) 이벤트(및 [**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989))를 재정의하고 컨트롤의 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트로 라우팅합니다. 컨트롤 클래스에서 키 누름을 처리하는 경우 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 및 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트가 발생하지 않습니다.  
> 손가락으로 탭하기 또는 마우스로 클릭과 유사하게 단추를 호출하기 위한 기본 제공 키보드 기능이 제공됩니다. Space 키 또는 Enter 키 이외의 키를 누르면 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 및 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트가 발생합니다. 클래스 기반 이벤트 처리의 작동 방식(특히 "컨트롤의 입력 이벤트 처리기" 섹션)에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.


UI의 컨트롤은 입력 포커스가 있는 경우에만 키보드 이벤트를 생성합니다. 개별 컨트롤은 사용자가 레이아웃에서 해당 컨트롤을 직접 클릭 또는 탭하거나 Tab 키를 사용하여 콘텐츠 영역 내에서 탭 시퀀스를 단계별로 이동할 때 포커스를 얻습니다.

컨트롤의 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161) 메서드를 호출하여 포커스를 강제 적용할 수도 있습니다. 이 작업은 사용자의 UI가 로드될 때 기본적으로 키보드 포커스가 설정되지 않으므로 바로 가기 키를 구현하는 경우에 필요합니다. 자세한 내용은 이 항목의 뒷부분에 있는 **바로 가기 키 예제**를 참조하세요.

컨트롤에 입력 포커스를 적용하려면 컨트롤이 활성화되고 표시되어야 하며 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) 및 [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 속성 값이 **true**여야 합니다. 대부분의 컨트롤은 이것이 기본 상태입니다. 컨트롤에 입력 포커스가 있으면 이 항목의 뒷부분에 설명된 대로 키보드 입력 이벤트를 발생시키고 응답할 수 있습니다. [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 및 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 이벤트를 처리하여 포커스를 받거나 잃는 컨트롤에 응답할 수도 있습니다.

기본적으로 컨트롤의 탭 시퀀스는 XAML(Extensible Application Markup Language)에 표시되는 순서입니다. 그러나 이 순서는 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) 속성을 사용하여 수정할 수 있습니다. 자세한 내용은 [키보드 접근성 구현](https://msdn.microsoft.com/library/windows/apps/hh868161)을 참조하세요.

## <a name="keyboard-event-handlers"></a>키보드 이벤트 처리기


입력 이벤트 처리기는 다음 정보를 제공하는 대리자를 구현합니다.

-   이벤트를 보낸 사람. 보낸 사람은 이벤트 처리기가 연결된 개체를 보고합니다.
-   이벤트 데이터. 키보드 이벤트의 경우 이 데이터는 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)의 인스턴스입니다. 처리기의 대리자는 [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904)입니다. 대부분의 처리기 시나리오에서 **KeyRoutedEventArgs**의 가장 관련된 속성은 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)와 [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)입니다.
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810). 키보드 이벤트는 라우트된 이벤트이므로 이벤트 데이터에서 **OriginalSource**를 제공합니다. 의도적으로 이벤트가 개체 트리를 통해 버블 업되도록 허용하는 경우 **OriginalSource**가 보낸 사람이 아니라 관련 개체일 수도 있지만 이것은 디자인에 따라 달라집니다. 보낸 사람 대신 **OriginalSource**를 사용하는 방법에 대한 자세한 내용은 이 항목의 "키보드 라우트된 이벤트" 섹션 또는 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

### <a name="attaching-a-keyboard-event-handler"></a>키보드 이벤트 처리기 연결

이벤트를 멤버로 포함하는 모든 개체에 대해 키보드 이벤트 처리기 함수를 연결할 수 있습니다. 임의의 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 파생 클래스도 여기에 포함됩니다. 다음 XAML 예제는 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br208942)의 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br242704) 이벤트에 대해 처리기를 연결하는 방법을 보여 줍니다.

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

코드에서 이벤트 처리기를 연결할 수도 있습니다. 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

### <a name="defining-a-keyboard-event-handler"></a>키보드 이벤트 처리기 정의

다음 예제에서는 이전 예제에서 연결된 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트 처리기의 불완전한 이벤트 처리기 정의를 보여 줍니다.

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>KeyRoutedEventArgs 사용

모든 키보드 이벤트는 이벤트 데이터에 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)를 사용하며 **KeyRoutedEventArgs**에 다음 속성이 포함되어 있습니다.

-   [**키**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)([**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)에서 상속됨)

### <a name="key"></a>키

키를 누르면 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 이벤트가 발생합니다. 마찬가지로, 키를 놓으면 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)이 발생합니다. 일반적으로 특정 키 값을 처리하기 위해 이벤트를 수신 대기합니다. 어떤 키를 누르거나 놓았는지 알아보려면 이벤트 데이터에서 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) 값을 확인하세요. **Key**는 [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812) 값을 반환합니다. **VirtualKey** 열거형에는 지원되는 모든 키가 포함됩니다.

### <a name="modifier-keys"></a>보조 키

보조 키는 Ctrl 또는 Shift 키와 같이 사용자가 일반적으로 다른 키와 함께 누르는 키입니다. 앱에서 이러한 조합을 바로 가기 키로 사용하여 앱 명령을 호출할 수 있습니다.

[**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 및 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트 처리기에 코드를 사용하여 바로 가기 키 조합을 검색합니다. 그런 후 관심 있는 보조 키의 누른 상태를 추적할 수 있습니다. 비보조 키에 대해 키보드 이벤트가 발생하는 경우 보조 키가 누른 상태인지 여부도 동시에 확인할 수 있습니다.

> [!NOTE]
> Alt 키는 **VirtualKey.Menu** 값으로 표시됩니다.

 

### <a name="shortcut-keys-example"></a>바로 가기 키 예제


다음 예제에서는 바로 가기 키를 구현하는 방법을 보여 줍니다. 이 예제에서 사용자는 [재생], [일시 중지] 및 [증지] 단추나 .Ctrl+P, Ctrl+A 및 Ctrl+S 바로 가기 키를 사용하여 미디어 재생을 제어할 수 있습니다. 단추 XAML은 단추 레이블의 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081) 속성 및 도구 설명을 사용하여 바로 가기를 표시합니다. 이 자체 설명서는 앱의 유용성과 접근성을 향상시키는 데 중요합니다. 자세한 내용은 [키보드 접근성](https://msdn.microsoft.com/library/windows/apps/mt244347)을 참조하세요.

페이지를 로드하면 입력 포커스가 페이지 자체에 설정됩니다. 이 단계가 없으면 컨트롤에 초기 입력 포커스가 없으며, 사용자가 컨트롤을 탭하거나 클릭하여 입력 포커스를 수동으로 설정할 때까지 앱에서 입력 이벤트를 발생시키지 않습니다.

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> XAML에서 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762) 또는 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763)를 설정하면 문자열 정보(해당 특정 작업을 호출하는 바로 가기 키를 문서화함)를 제공합니다. 이 정보는 Narrator와 같은 Microsoft UI 자동화 클라이언트에 의해 캡처되며 보통 사용자에게 직접 제공됩니다.
>
> **AutomationProperties.AcceleratorKey** 또는 **AutomationProperties.AccessKey**를 설정해도 그 자체로는 작업이 수행되지 않습니다. 앱에서 바로 가기 키 동작을 실제로 구현하려면 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 또는 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트에 대한 처리기를 연결해야 합니다. 또한 액세스 키에 대한 밑줄로 표시된 텍스트 장식은 자동으로 제공되지 않습니다. UI에서 밑줄로 표시된 텍스트를 표시하려면 니모닉에서 명시적으로 특정 키의 텍스트에 밑줄을 인라인 [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982) 서식으로 표시해야 합니다.

 

## <a name="keyboard-routed-events"></a>키보드 라우트된 이벤트


[**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941), [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 등의 특정 이벤트는 라우트된 이벤트입니다. 라우트된 이벤트는 버블링 라우팅 전략을 사용합니다. 버블링 라우팅 전략은 이벤트가 자식 개체에서 발생한 다음 개체 트리에서 다음 부모 개체로 라우트됨을 의미합니다. 이 경우 동일한 이벤트를 처리하고 동일한 이벤트 데이터로 상호 작용할 수 있는 기회가 제공됩니다.

[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br208942)와 두 개의 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209267) 개체에 대해 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br209265) 이벤트를 처리하는 다음 XAML 예제를 살펴보세요. 이 경우 포커스가 **Button** 개체 중 하나에 있을 때 키를 놓으면 **KeyUp** 이벤트가 발생합니다. 이 이벤트는 부모 **Canvas**로 버블 업됩니다.

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

다음 예제에서는 이전 예제의 해당 XAML 콘텐츠에 대해 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트 처리기를 구현하는 방법을 보여 줍니다.

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

이전 처리기의 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 속성 사용을 살펴보세요. 여기서 **OriginalSource**는 이벤트를 발생시킨 개체를 보고합니다. [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)은 컨트롤이 아니며 포커스를 가질 수 없으므로 개체가 **StackPanel**일 수는 없습니다. **StackPanel** 내의 두 단추 중 하나만 이벤트를 발생시킬 수 있는데 어떤 단추일까요? 부모 개체에서 이벤트를 처리하는 경우 **OriginalSource**를 사용하여 실제 이벤트 원본 개체를 구별합니다.

### <a name="the-handled-property-in-event-data"></a>이벤트 데이터의 Handled 속성

이벤트 처리 전략에 따라 하나의 이벤트 처리기만 버블링 이벤트에 반응하도록 할 수 있습니다. 예를 들어 특정 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 처리기가 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 컨트롤 중 하나에 연결되어 있는 경우 해당 이벤트를 처리할 수 있는 첫 번째 기회가 제공됩니다. 이 경우 부모 패널에서는 이벤트를 처리하지 않는 것이 좋습니다. 이 시나리오에서는 이벤트 데이터에 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 속성을 사용할 수 있습니다.

라우트된 이벤트 데이터 클래스의 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 속성은 이전에 이벤트 경로에 등록한 다른 처리기가 이미 적용되었음을 보고하는 데 사용됩니다. 이 속성은 라우트된 이벤트 시스템의 동작에 영향을 줍니다. 이벤트 처리기에서 **Handled**를 **true**로 설정하면 이 이벤트는 라우팅을 중지하고 다음 부모 요소로 전송되지 않습니다.

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler 및 이미 처리된 키보드 이벤트

이미 처리된 것으로 표시된 이벤트에서 작동할 수 있는 처리기를 연결하는 특별한 기술을 사용할 수 있습니다. 이 기술은 XAML 특성 또는 C\#에서 += 등의 처리기를 추가하는 언어별 구문을 사용하는 대신 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 메서드를 사용하여 처리기를 등록합니다.

이 기술의 일반적인 제한 사항은 **AddHandler** API가 해당 라우트된 이벤트를 식별하는 [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) 유형의 매개 변수를 사용한다는 데 있습니다. 일부 라우트된 이벤트는 **RoutedEvent** 식별자를 제공하지 않으므로 이 경우 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)에서 처리할 수 있는 라우트된 이벤트에 영향을 미칠 수 있습니다. [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 및 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트는 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/hh702416)에 라우트된 이벤트 식별자([**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418) 및 [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/br208911))가 있습니다. 그러나 [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706) 등의 다른 이벤트에는 라우트된 이벤트 식별자가 없으므로 **AddHandler** 기술에 사용할 수 없습니다.

### <a name="overriding-keyboard-events-and-behavior"></a>키보드 이벤트 및 동작 재정의

특정 컨트롤에 대한 키 이벤트를 재정의하여(예: [**GridView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridView)) 키보드와 게임 패드와 같이 다양한 입력 장치에 대해 일관된 포커스 탐색 기능을 제공할 수 있습니다.

다음 예제에서에서는 화살표 키를 누를 때 콘텐츠를 하위 클래스 컨트롤 및 GridView에 포커스를 이동 하려고 KeyDown 동작을 재정의 합니다.

```csharp
public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> GridView를 레이아웃용으로만 사용할 경우 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl)과 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsWrapGrid)와 같은 다른 컨트롤을 사용하는 것이 좋습니다.

## <a name="commanding"></a>명령

일부 UI 요소는 명령 지원을 기본 제공합니다. 기본 구현에서 명령은 입력 관련 라우트된 이벤트를 사용합니다. 단일 명령 처리기를 호출하여 특정 포인터 작업이나 특정 액셀러레이터 키와 같은 관련 UI 입력을 처리할 수 있습니다.

UI 요소에 명령을 사용할 수 있는 경우 불연속 입력 이벤트 대신 명령 API를 사용하는 것이 좋습니다. 자세한 내용은 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)를 참조하세요.

[**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885)를 구현하여 일반 이벤트 처리기에서 호출하는 명령 기능을 캡슐화할 수도 있습니다. 이렇게 하면 사용 가능한 **Command** 속성이 없는 경우에도 명령을 사용할 수 있습니다.

## <a name="text-input-and-controls"></a>텍스트 입력 및 컨트롤

일부 컨트롤은 직접 처리를 통해 키보드 이벤트에 반응합니다. 예를 들어 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)는 키보드로 입력된 텍스트를 캡처한 다음 시각적으로 표현하는 컨트롤입니다. 해당 논리에 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 및 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)을 사용하여 키 입력을 캡처한 다음 텍스트가 실제로 변경된 경우 고유한 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706) 이벤트도 발생시킵니다.

일반적으로 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 및 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)에 대한 처리기를 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 또는 텍스트 입력을 처리하도록 고안된 관련 컨트롤에 추가할 수 있습니다. 그러나 디자인에서 의도된 대로 컨트롤이 키 이벤트를 통해 전달되는 모든 키 값에 응답하지 않을 수 있습니다. 동작은 각 컨트롤마다 다릅니다.

한 가지 예로 [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736)([**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)의 기본 클래스)는 스페이스바 또는 Enter 키를 확인할 수 있게 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)을 처리합니다. **ButtonBase**는 **KeyUp**을 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 이벤트를 발생시키기 위해 마우스 왼쪽 단추를 누르는 것과 동일한 것으로 간주합니다. 이러한 이벤트 처리는 **ButtonBase**가 가상 메서드 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983)을 재정의할 때 수행됩니다. 구현 시 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)를 **true**로 설정합니다. 이 결과, 스페이스바의 경우 키 이벤트를 수신 대기하는 단추의 부모가 이미 처리된 이벤트를 해당 처리기에 받지 않습니다.

또 다른 예로 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)를 들 수 있습니다. 화살표 키와 같은 일부 키는 **TextBox**에서 텍스트로 간주되지 않고 컨트롤 UI 동작과 관련된 것으로 간주됩니다. **TextBox**는 이러한 이벤트를 처리된 것으로 표시합니다.

사용자 지정 컨트롤은 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983)을 재정의하여 키 이벤트에 대해 유사한 재정의 동작을 구현할 수 있습니다. 사용자 지정 컨트롤이 특정 액셀러레이터 키를 처리하거나 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)에 대해 설명된 시나리오와 비슷한 컨트롤 또는 포커스 동작을 갖는 경우 이 논리를 해당 **OnKeyDown** / **OnKeyUp** 재정의에 적용해야 합니다.

## <a name="the-touch-keyboard"></a>터치 키보드

텍스트 입력 컨트롤은 터치 키보드를 자동으로 지원합니다. 사용자가 터치식 입력을 사용하여 텍스트 컨트롤에 입력 포커스를 설정하면 터치 키보드가 자동으로 나타납니다. 텍스트 컨트롤에 입력 포커스가 없으면 터치 키보드가 숨겨집니다.

터치 키보드가 나타나면 포커스가 있는 요소가 표시되도록 UI 위치가 자동으로 조정됩니다. 이로 인해 UI의 다른 중요한 영역이 화면 바깥쪽으로 이동할 수 있습니다. 그러나 기본 동작을 사용하지 않도록 설정하고 터치 키보드가 나타날 때 직접 UI를 조정할 수 있습니다. 자세한 내용은 [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)을 참조하세요.

텍스트 입력이 필요하지만 표준 텍스트 입력 컨트롤에서 파생되지 않는 사용자 지정 컨트롤을 만드는 경우 올바른 UI 자동화 제어 패턴을 구현하여 터치 키보드 지원을 추가할 수 있습니다. 자세한 내용은 [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)을 참조하세요.

터치 키보드의 키를 누르면 하드웨어 키보드의 키를 누른 것처럼 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 및 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 이벤트가 발생합니다. 그러나 터치 키보드는 입력 컨트롤에서 텍스트 조작에 예약된 Ctrl+A, Ctrl+Z, Ctrl+X, Ctrl+C 및 Ctrl+V에 대해 입력 이벤트를 발생시키지 않습니다.

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 사용자가 앱에서 데이터를 쉽고 빠르게 입력할 수 있도록 지원할 수 있습니다. 입력 범위는 시스템에서 해당 입력 유형에 맞는 특수한 터치 키보드를 제공할 수 있도록 컨트롤에서 예상되는 텍스트 입력 유형에 대한 힌트를 제공합니다. 예를 들어 텍스트 상자가 4자리 숫자의 PIN을 입력하는 목적으로만 사용될 경우 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 속성을 [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028)로 설정합니다. 이렇게 하면 사용자가 PIN을 쉽게 입력할 수 있도록 시스템에서 숫자 키패드 레이아웃이 표시됩니다. 자세한 내용은 [입력 범위를 사용해서 터치 키보드 변경](https://msdn.microsoft.com/library/windows/apps/mt280229)을 참조하세요.

## <a name="related-articles"></a>관련 문서

**개발자**
* [키보드 조작](keyboard-interactions.md)
* [입력 장치 식별](identify-input-devices.md)
* [터치 키보드의 현재 상태에 응답](respond-to-the-presence-of-the-touch-keyboard.md)

**디자이너**
* [키보드 디자인 지침](https://msdn.microsoft.com/library/windows/apps/hh972345)

**샘플**
* [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [기본 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [짧은 대기 시간 입력 샘플](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [포커스 화면 효과 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**보관 샘플**
* [입력 샘플](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [입력: 장치 기능 샘플](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [입력: 터치 키보드 샘플](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [화상 키보드의 모양에 응답 샘플](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 텍스트 편집 샘플](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 
