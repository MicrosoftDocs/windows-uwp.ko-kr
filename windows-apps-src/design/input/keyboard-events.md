---
description: 키보드 및 클래스 이벤트 처리기를 사용 하 여 앱의 하드웨어 또는 소프트웨어 키보드에서 키 입력 작업에 응답 합니다.
title: 키보드 이벤트
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: 키보드, 게임 패드, 원격, 접근성, 탐색, 포커스, 텍스트, 입력, 사용자 조작, 키 위로, 키 아래로
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: efd8a2bb205974efdcf13d614cb6fa7848f96dc7
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033696"
---
# <a name="keyboard-events"></a>키보드 이벤트

## <a name="keyboard-events-and-focus"></a>키보드 이벤트 및 포커스

하드웨어와 터치 키보드 모두에 대해 다음과 같은 키보드 이벤트가 발생할 수 있습니다.

| 이벤트                                      | 설명                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) | 키를 누를 때 발생합니다.  |
| [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)     | 키를 놓을 때 발생합니다. |

> [!IMPORTANT]
> 일부 Windows 런타임 컨트롤은 입력 이벤트를 내부적으로 처리 합니다. 이러한 경우 이벤트 수신기가 연결 된 처리기를 호출 하지 않으므로 입력 이벤트가 발생 하지 않는 것으로 나타날 수 있습니다. 일반적으로이 키 하위 집합은 기본 키보드 접근성의 기본 제공 지원을 제공 하기 위해 클래스 처리기에 의해 처리 됩니다. 예를 들어 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 클래스는 Space 키와 Enter 키 ( [**Onpointerpressed**](/uwp/api/windows.ui.xaml.controls.control.onpointerpressed)) 모두에 대 한 [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) 이벤트를 재정의 하 고 컨트롤의 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트로 라우팅합니다. 컨트롤 클래스에서 키 누름을 처리 하는 경우 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트는 발생 하지 않습니다.  
> 그러면 마우스로 단추를 누르거나 마우스로 단추를 클릭 하는 것과 유사 하 게 단추를 호출 하는 것과 같은 기본 제공 키보드 기능이 제공 됩니다. 공백이 나 Enter 키 이외의 키는 여전히 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트를 발생 시킵니다. 이벤트의 클래스 기반 처리 작동 방식에 대 한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](../../xaml-platform/events-and-routed-events-overview.md)를 참조 하세요.


UI의 컨트롤은 입력 포커스가 있는 경우에만 키보드 이벤트를 생성 합니다. 개별 컨트롤은 사용자가 레이아웃에서 해당 컨트롤을 직접 클릭 하거나 탭 하거나 Tab 키를 사용 하 여 콘텐츠 영역 내에서 탭 시퀀스를 한 단계씩 실행 하는 경우 포커스를 얻습니다.

컨트롤의 [**포커스**](/uwp/api/windows.ui.xaml.controls.control.focus) 메서드를 호출 하 여 포커스를 강제할 수도 있습니다. 이는 UI가 로드 될 때 키보드 포커스가 기본적으로 설정 되지 않기 때문에 바로 가기 키를 구현할 때 필요 합니다. 자세한 내용은이 항목의 뒷부분에 나오는 **바로 가기 키 예** 를 참조 하세요.

컨트롤이 입력 포커스를 받도록 하려면이 컨트롤을 사용 하도록 설정 하 고, 표시 하 고, [**Istabstop**](/uwp/api/windows.ui.xaml.controls.control.istabstop) 및 [**HitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) 속성 값이 **true** 여야 합니다. 대부분의 컨트롤에 대 한 기본 상태입니다. 컨트롤에 입력 포커스가 있으면이 항목의 뒷부분에 설명 된 대로 키보드 입력 이벤트를 발생 시키고 응답할 수 있습니다. [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) 및 [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) 이벤트를 처리 하 여 포커스를 받거나 잃는 컨트롤에 응답할 수도 있습니다.

기본적으로 컨트롤의 탭 시퀀스는 Extensible Application Markup Language (XAML)에 표시 되는 순서입니다. 그러나 [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 속성을 사용 하 여이 순서를 수정할 수 있습니다. 자세한 내용은 [키보드 접근성 구현](/previous-versions/windows/apps/hh868161(v=win.10))을 참조 하세요.

## <a name="keyboard-event-handlers"></a>키보드 이벤트 처리기


입력 이벤트 처리기는 다음 정보를 제공 하는 대리자를 구현 합니다.

-   이벤트의 송신자입니다. 발신자는 이벤트 처리기가 연결 된 개체를 보고 합니다.
-   이벤트 데이터입니다. 키보드 이벤트의 경우 해당 데이터는 [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs)의 인스턴스입니다. 처리기의 대리자는 [**Keyeventhandler**](/uwp/api/windows.ui.xaml.input.keyeventhandler)입니다. 대부분의 처리기 시나리오에서 **KeyRoutedEventArgs** 의 가장 관련성이 높은 속성은 [**키**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) 와 [**keystatus**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus)입니다.
-   [**Originalsource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource). 키보드 이벤트는 라우트된 이벤트 이므로 이벤트 데이터는 **Originalsource** 를 제공 합니다. 의도적으로 이벤트를 개체 트리를 통해 버블링 하도록 허용 하는 경우 **Originalsource** 는 때때로 보낸 사람이 아닌 중요 한 개체입니다. 그러나이는 디자인에 따라 다릅니다. 보낸 사람 대신 **Originalsource** 를 사용 하는 방법에 대 한 자세한 내용은이 항목의 "키보드 라우트된 이벤트" 섹션 또는 [이벤트 및 라우트된 이벤트 개요](../../xaml-platform/events-and-routed-events-overview.md)를 참조 하세요.

### <a name="attaching-a-keyboard-event-handler"></a>키보드 이벤트 처리기 연결

이벤트를 멤버로 포함 하는 모든 개체에 대해 키보드 이벤트 처리기 함수를 연결할 수 있습니다. 여기에는 모든 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 파생 클래스가 포함 됩니다. 다음 XAML 예제에서는 [**표에**](/uwp/api/Windows.UI.Xaml.Controls.Grid)대 한 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트에 대 한 처리기를 연결 하는 방법을 보여 줍니다.

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

코드에 이벤트 처리기를 연결할 수도 있습니다. 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](../../xaml-platform/events-and-routed-events-overview.md)를 참조하세요.

### <a name="defining-a-keyboard-event-handler"></a>키보드 이벤트 처리기 정의

다음 예제에서는 앞의 예제에서 연결 된 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트 처리기에 대 한 불완전 한 이벤트 처리기 정의를 보여 줍니다.

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

모든 키보드 이벤트는 이벤트 데이터에 대해 [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) 를 사용 하 고 **KeyRoutedEventArgs** 에는 다음 속성이 포함 됩니다.

-   [**키**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key)
-   [**KeyStatus**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus)
-   [**처리됨**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)
-   [**Originalsource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) ( [**system.windows.routedeventargs.handled**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs)에서 상속)

### <a name="key"></a>키

키를 누르면 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 이벤트가 발생 합니다. 마찬가지로, 키가 해제 된 경우에는 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이 발생 합니다. 일반적으로 특정 키 값을 처리 하는 이벤트를 수신 대기 합니다. 눌러져 있거나 해제 된 키를 확인 하려면 이벤트 데이터의 [**키**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) 값을 확인 합니다. **키** 가 [**virtualkey**](/uwp/api/Windows.System.VirtualKey) 값을 반환 합니다. **Virtualkey** 열거형에는 지원 되는 모든 키가 포함 되어 있습니다.

### <a name="modifier-keys"></a>보조 키

보조키는 사용자가 일반적으로 다른 키와 함께 누르는 Ctrl 또는 Shift와 같은 키입니다. 앱은 이러한 조합을 바로 가기 키로 사용 하 여 앱 명령을 호출할 수 있습니다.

[**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트 처리기에서 바로 가기 키 조합을 검색할 수 있습니다. 보조키가 아닌 키에 대해 키보드 이벤트가 발생 하는 경우 보조키가 눌린 상태 인지 여부를 확인할 수 있습니다.

또는 [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) ( [**CoreWindow**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread))를 통해 얻은 [**getkeystate ()**](/uwp/api/windows.ui.core.corewindow.getkeystate) 함수를 사용 하 여 보조키가 아닌 키를 누를 때 한정자 상태를 확인할 수도 있습니다.

다음 예제에서는 첫 번째 구현에 대 한 스텁 코드도 포함 하는 동시에이 두 번째 메서드를 구현 합니다.

> [!NOTE]
> Alt 키는 **Virtualkey. 메뉴** 값으로 표시 됩니다.

### <a name="shortcut-keys-example"></a>바로 가기 키 예제

다음 예제에서는 바로 가기 키를 구현 하는 방법을 보여 줍니다. 이 예제에서 사용자는 재생, 일시 중지, 중지 단추 또는 Ctrl + P, Ctrl + A 및 Ctrl + S 바로 가기 키를 사용 하 여 미디어 재생을 제어할 수 있습니다. 단추 XAML은 단추 레이블에 도구 설명 및 [**Automationproperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) 속성을 사용 하 여 바로 가기를 표시 합니다. 이 자체 설명서는 앱의 유용성 및 접근성을 높이는 데 중요 합니다. 자세한 내용은 [키보드 접근성](../accessibility/keyboard-accessibility.md)을 참조 하세요.

또한 페이지는 로드 될 때 입력 포커스를 자신에 게 설정 합니다. 이 단계를 수행 하지 않으면 초기 입력 포커스가 있는 컨트롤이 없으며, 사용자가 입력 포커스를 수동으로 설정 하는 경우 (예: 컨트롤을 클릭 하거나 컨트롤을 클릭 하 여) 앱에서 입력 이벤트를 발생 시 키 지 않습니다.

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
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) 
{
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
    if (IsCtrlKeyPressed()) 
    {
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
> [**AcceleratorKey**](/dotnet/api/system.windows.automation.automationproperties.acceleratorkey) 또는 [**automationproperties**](/dotnet/api/system.windows.automation.automationproperties.accesskey) 를 설정 하면 XAML에서 AccessKey를 설정 하 여 특정 작업을 호출 하는 바로 가기 키를 설명 하는 문자열 정보를 제공 합니다. 이 정보는 내레이터와 같은 Microsoft UI 자동화 클라이언트에서 캡처되고 일반적으로 사용자에 게 직접 제공 됩니다.
>
> **AcceleratorKey** 또는 **automationproperties** 를 설정 하는 경우에는 해당 작업을 수행할 수 없습니다. 앱에서 바로 가기 키 동작을 실제로 구현 하기 위해 여전히 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 또는 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트에 대 한 처리기를 연결 해야 합니다. 또한 액세스 키에 대 한 밑줄 텍스트 장식이 자동으로 제공 되지 않습니다. UI에 밑줄이 그어진 텍스트를 표시 하려는 경우 니모닉의 특정 키에 대 한 텍스트를 인라인 [**밑줄**](/uwp/api/Windows.UI.Xaml.Documents.Underline) 형식으로 명시적으로 지정 해야 합니다.

 

## <a name="keyboard-routed-events"></a>키보드 라우트된 이벤트


특정 이벤트는 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)를 비롯 한 라우트된 이벤트입니다. 라우트된 이벤트는 버블링 라우팅 전략을 사용 합니다. 버블링 라우팅 전략은 이벤트가 자식 개체에서 발생 한 다음 개체 트리의 연속 부모 개체로 라우팅되는 것을 의미 합니다. 이는 동일한 이벤트를 처리 하 고 동일한 이벤트 데이터와 상호 작용할 수 있는 또 다른 기회를 제공 합니다.

[**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 와 두 개의 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 개체에 대 한 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트를 처리 하는 다음 XAML 예제를 고려해 보세요. 이 경우 **Button** 개체 중 하나에 포커스가 있을 때 키를 놓으면 **KeyUp** 이벤트가 발생 합니다. 그러면 이벤트가 부모 **캔버스로** 버블링 됩니다.

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

다음 예제에서는 이전 예제의 해당 XAML 콘텐츠에 대해 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트 처리기를 구현하는 방법을 보여 줍니다.

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

이전 처리기에서 [**Originalsource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) 속성을 사용 하는 것을 확인 합니다. 여기서 **Originalsource** 는 이벤트를 발생 시킨 개체를 보고 합니다. **Stackpanel** 은 컨트롤이 아니므로 포커스를 가질 수 없으므로 개체는 [**stackpanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 일 수 없습니다. **StackPanel** 내에서 두 단추 중 하나만 이벤트를 발생 시킬 수 있지만 그 중 하나는 이벤트를 발생 시킬 수 있습니다. 부모 개체에서 이벤트를 처리 하는 경우 **Originalsource** 를 사용 하 여 실제 이벤트 소스 개체를 구분 합니다.

### <a name="the-handled-property-in-event-data"></a>이벤트 데이터의 처리 된 속성

이벤트 처리 전략에 따라 하나의 이벤트 처리기만 버블링 이벤트에 반응 하도록 할 수 있습니다. 예를 들어, [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 컨트롤 중 하나에 연결 된 특정 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 처리기가 있는 경우 해당 이벤트를 처리할 수 있는 첫 번째 기회가 있습니다. 이 경우 부모 패널에서 이벤트를 처리 하는 것을 원하지 않을 수 있습니다. 이 시나리오에서는 이벤트 데이터에서 [**처리**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) 된 속성을 사용할 수 있습니다.

라우트된 이벤트 데이터 클래스에서 처리 된 속성의 목적은 이전에 이벤트 경로에 등록 한 다른 처리기가 이미 [**처리**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) 되었음을 보고 하는 것입니다. 이 속성은 라우트된 이벤트 시스템의 동작에 영향을 줍니다. 이벤트 처리기에서 **처리** 됨을 **true** 로 설정 하면 해당 이벤트는 라우팅을 중지 하 고 연속 부모 요소로 보내지지 않습니다.

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler 및 이미 처리 한 키보드 이벤트

이미 처리 된 것으로 표시 된 이벤트에 대 한 작업을 수행할 수 있는 처리기를 연결 하는 특수 기법을 사용할 수 있습니다. 이 기술은 C에서 + =와 같이 처리기를 추가 하는 데 XAML 특성 또는 언어별 구문을 사용 하지 않고 [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) 메서드를 사용 하 여 처리기를 등록 합니다 \# .

이 기술의 일반적인 제한 사항은 **AddHandler** API가 [**2csystem.delegate**](/uwp/api/Windows.UI.Xaml.RoutedEvent) idnentifying 형식의 매개 변수를 사용 하 여 해당 라우트된 이벤트를 확인 하는 것입니다. 모든 라우트된 이벤트에서 **2csystem.delegate** 식별자를 제공 하는 것은 아닙니다. 따라서 이러한 고려 사항은 [**처리**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) 된 사례에서 처리 될 수 있는 라우트된 이벤트에 영향을 줍니다. [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트에는 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)의 라우트된 이벤트 식별자 ( [**KeyDownEvent**](/uwp/api/windows.ui.xaml.uielement.keydownevent) 및 [**keyupevent**](/uwp/api/windows.ui.xaml.uielement.keyupevent))가 있습니다. 그러나 [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 등의 다른 이벤트에는 라우트된 이벤트 식별자가 없으므로 **AddHandler** 기술과 함께 사용할 수 없습니다.

### <a name="overriding-keyboard-events-and-behavior"></a>키보드 이벤트 및 동작 재정의

특정 컨트롤 (예: [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView))의 키 이벤트를 재정의 하 여 키보드 및 게임 패드를 비롯 한 다양 한 입력 장치에 대해 일관 된 포커스 탐색 기능을 제공할 수 있습니다.

다음 예제에서는 컨트롤의 서브 클래스를 만들고, 화살표 키를 누를 때 GridView 콘텐츠로 포커스를 이동 하도록 KeyDown 동작을 재정의 합니다.

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
> 레이아웃에만 GridView를 사용 하는 경우 [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 와 같은 다른 컨트롤을 [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)와 함께 사용 하는 것이 좋습니다.

## <a name="commanding"></a>명령

적은 수의 UI 요소는 명령에 대 한 기본 제공 지원을 제공 합니다. 명령에서는 내부 구현에서 입력 관련 라우트된 이벤트를 사용 합니다. 단일 명령 처리기를 호출 하 여 특정 포인터 작업 또는 특정 액셀러레이터 키와 같은 관련 UI 입력을 처리할 수 있습니다.

UI 요소에 대해 명령을 사용할 수 있는 경우 불연속 입력 이벤트 대신 해당 명령 Api를 사용 하는 것이 좋습니다. 자세한 내용은 [**Buttonbase. 명령**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)을 참조 하세요.

[**ICommand**](/uwp/api/Windows.UI.Xaml.Input.ICommand) 를 구현 하 여 일반 이벤트 처리기에서 호출 하는 명령 기능을 캡슐화 할 수도 있습니다. 이를 통해 명령 속성을 사용할 수 없는 경우에도 **명령을** 사용할 수 있습니다.

## <a name="text-input-and-controls"></a>텍스트 입력 및 컨트롤

특정 컨트롤은 자체 처리를 사용 하 여 키보드 이벤트에 반응 합니다. 예를 들어 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 는 키보드를 사용 하 여 입력 한 텍스트를 캡처하고 시각적으로 표시 하도록 디자인 된 컨트롤입니다. 자체 논리에서 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 및 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 을 사용 하 여 키 입력을 캡처한 다음, 텍스트가 실제로 변경 된 경우에도 자체 [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 이벤트를 발생 시킵니다.

일반적으로 텍스트 입력을 처리 하기 위해 사용 하는 모든 관련 컨트롤 또는 텍스트 [**상자**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)에 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 및 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 에 대 한 처리기를 추가할 수 있습니다. 그러나 의도 된 디자인의 일부로 컨트롤은 키 이벤트를 통해 전달 되는 모든 키 값에 응답 하지 않을 수 있습니다. 동작은 각 컨트롤에만 적용 됩니다.

예를 들어 [**buttonbase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) ( [**단추의**](/uwp/api/Windows.UI.Xaml.Controls.Button)기본 클래스)는 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 를 처리 하 여 스페이스바 또는 Enter 키를 확인할 수 있도록 합니다. **Buttonbase** 는 [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트를 발생 시키기 위해 마우스 왼쪽 단추와 동일한 **KeyUp** 을 고려 합니다. 이 이벤트 처리는 **Buttonbase** 가 가상 메서드 [**OnKeyUp**](/uwp/api/windows.ui.xaml.controls.control.onkeyup)를 재정의 하는 경우에 수행 됩니다. 구현에서는 [**처리**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) 됨을 **true** 로 설정 합니다. 그 결과, 스페이스바의 경우 키 이벤트를 수신 하는 단추의 부모는 자체 처리기에 대해 이미 처리 된 이벤트를 수신 하지 않습니다.

또 다른 예는 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)입니다. 화살표 키와 같은 일부 키는 **TextBox By TextBox** 로 간주 되지 않으며 대신 컨트롤 UI 동작에 특정 한 것으로 간주 됩니다. **텍스트 상자** 는 이러한 이벤트 사례를 처리 된 것으로 표시 합니다.

사용자 지정 컨트롤은 [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown)OnKeyUp를 재정의 하 여 키 이벤트에 대해 이와 유사한 재정의 동작을 구현할 수 있습니다  /  [**OnKeyUp**](/uwp/api/windows.ui.xaml.controls.control.onkeyup). 사용자 지정 컨트롤에서 특정 액셀러레이터 키를 처리 하거나 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)에 대해 설명 된 시나리오와 유사한 컨트롤 또는 포커스 동작이 있는 경우이 논리를 고유한 **OnKeyDown**  /  **OnKeyUp** 재정의에 두어야 합니다.

## <a name="the-touch-keyboard"></a>터치 키보드

텍스트 입력 컨트롤은 터치 키보드에 대 한 자동 지원을 제공 합니다. 사용자가 터치식 입력을 사용 하 여 입력 포커스를 텍스트 컨트롤로 설정 하면 터치 키보드가 자동으로 표시 됩니다. 입력 포커스가 텍스트 컨트롤에 없으면 터치 키보드는 숨겨집니다.

터치 키보드가 나타나면 포커스가 있는 요소가 계속 표시 되도록 UI의 위치를 자동으로 다시 표시 합니다. 이렇게 하면 UI의 다른 중요 한 영역을 화면 밖으로 이동할 수 있습니다. 그러나 터치 키보드가 나타날 때 기본 동작을 사용 하지 않도록 설정 하 고 사용자 고유의 UI 조정을 수행할 수 있습니다. 자세한 내용은 [Touch 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)을 참조 하세요.

텍스트 입력이 필요 하지만 표준 텍스트 입력 컨트롤에서 파생 되지 않은 사용자 지정 컨트롤을 만드는 경우 올바른 UI 자동화 컨트롤 패턴을 구현 하 여 터치 키보드 지원을 추가할 수 있습니다. 자세한 내용은 [Touch 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)을 참조 하세요.

터치 키보드에서 키를 누르면 하드웨어 키보드에서 키 누름과 같은 방식으로 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 및 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트를 발생 시킵니다. 그러나 터치 키보드는 입력 컨트롤에서 텍스트 조작을 위해 예약 된 ctrl + A, Ctrl + Z, Ctrl + X, Ctrl + C 및 Ctrl + V에 대 한 입력 이벤트를 발생 시 키 지 않습니다.

사용자가 입력할 것으로 예상되는 데이터 종류와 일치하도록 텍스트 컨트롤의 입력 범위를 설정하여 사용자가 앱에서 데이터를 쉽고 빠르게 입력할 수 있도록 지원할 수 있습니다. 입력 범위는 컨트롤이 필요로 하는 텍스트 입력 형식에 대 한 힌트를 제공 하므로 시스템에서 입력 형식에 대 한 특수 터치 키보드 레이아웃을 제공할 수 있습니다. 예를 들어 텍스트 상자를 4 자리 PIN을 입력 하는 데만 사용 하는 경우 [**Inputscope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 속성을 [**Number**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)로 설정 합니다. 그러면 사용자가 더 쉽게 PIN을 입력할 수 있도록 하는 숫자 키패드 레이아웃을 시스템에 표시 합니다. 자세한 내용은 [입력 범위를 사용 하 여 터치 키보드 변경](./use-input-scope-to-change-the-touch-keyboard.md)을 참조 하세요.

## <a name="related-articles"></a>관련된 문서

### <a name="developers"></a>개발자

- [키보드 조작](keyboard-interactions.md)
- [입력 디바이스 식별](identify-input-devices.md)
- [터치 키보드의 현재 상태에 응답](respond-to-the-presence-of-the-touch-keyboard.md)

### <a name="designers"></a>디자이너

- [키보드 디자인 지침](./keyboard-interactions.md)

### <a name="samples"></a>샘플

- [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [기본 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [짧은 대기 시간 입력 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [포커스 화면 효과 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>보관 샘플

- [입력 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [입력: 장치 기능 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [입력: 터치 키보드 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Touch%20keyboard%20sample%20(Windows%208))
- [화상 키보드 샘플의 모양에 대 한 응답](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [XAML 텍스트 편집 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
