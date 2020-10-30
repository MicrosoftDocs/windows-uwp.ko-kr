---
description: 따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: 키보드 접근성
label: Keyboard accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79dd977cda50d8573cfeab2628ab6227cc9309c0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032506"
---
# <a name="keyboard-accessibility"></a>키보드 접근성  



따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>UI 요소 간 키보드 탐색  
컨트롤과 함께 키보드를 사용 하려면 컨트롤에 포커스가 있어야 하 고 포인터를 사용 하지 않고 포커스를 받으려면 탭 탐색을 통해 UI 디자인에서 컨트롤에 액세스할 수 있어야 합니다. 기본적으로 컨트롤의 탭 순서는 해당 컨트롤이 디자인 화면에 추가 되거나 XAML에 나열 되거나 프로그래밍 방식으로 컨테이너에 추가 되는 순서와 동일 합니다.

대부분의 경우 XAML에서 컨트롤을 정의 하는 방법에 따라 기본 순서가 가장 적합 합니다. 특히 컨트롤이 화면 판독기에서 읽는 순서 이기 때문입니다. 그러나 기본 순서는 시각적 순서와 반드시 일치 하지는 않습니다. 실제 표시 위치는 부모 레이아웃 컨테이너 및 자식 요소에 대해 설정 하 여 레이아웃에 영향을 줄 수 있는 특정 속성에 따라 달라질 수 있습니다. 앱의 탭 순서가 적절 한지 확인 하려면이 동작을 직접 테스트 합니다. 특히 레이아웃에 대 한 표 메타포 나 표 비유가 있는 경우 사용자가 탭 순서와 읽는 순서를 다르게 할 수 있습니다. 그리고 그 자체로는 항상 문제가 되지 않습니다. 하지만 앱의 기능을 touchable UI로 그리고 키보드 액세스 가능 UI로 테스트 하 고 UI가 어떤 방식으로든 사용 되는지 확인 합니다.

XAML을 조정 하 여 탭 순서가 시각적 순서와 일치 하도록 만들 수 있습니다. 또는 열 우선 탭 탐색을 사용 하는 [**표**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 레이아웃의 다음 예제와 같이 [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 속성을 설정 하 여 기본 탭 순서를 재정의할 수 있습니다.

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

탭 순서에서 컨트롤을 제외 하려고 할 수 있습니다. 일반적으로이 작업은 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 속성을 **false** 로 설정 하는 등의 방법으로 컨트롤을 비 대화형으로 만드는 경우에만 수행 합니다. 비활성화 된 컨트롤은 탭 순서에서 자동으로 제외 됩니다. 하지만 경우에 따라 탭 순서에서 컨트롤을 사용할 수 없는 경우에도 제외 하는 것이 좋습니다. 이 경우 [**Istabstop**](/uwp/api/windows.ui.xaml.controls.control.istabstop) 속성을 **false** 로 설정할 수 있습니다.

일반적으로 포커스를 가질 수 있는 모든 요소는 기본적으로 탭 순서에 있습니다. 이에 대 한 예외는 [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 와 같은 특정 텍스트 표시 형식에 포커스가 있을 수 있으므로 클립보드에서 텍스트를 선택 하 여 액세스할 수 있습니다. 그러나 정적 텍스트 요소가 탭 순서에 있는 것은 아니므로 탭 순서가 아닙니다. 이러한 항목은 일반적으로 대화형이 아니지만 호출할 수 없으며 텍스트 입력이 필요 하지는 않지만 텍스트에서 선택 지점의 찾기 및 조정을 지 원하는 [텍스트 컨트롤 패턴](/windows/desktop/WinAuto/uiauto-controlpatternsoverview) 을 지원 합니다. 텍스트에 포커스를 설정 하는 connotation가 있으면 가능한 일부 작업이 가능 합니다. 텍스트 요소는 계속 해 서 보조 기술에서 검색 되 고 화면 판독기에서 소리내어 읽게 되지만 실제 탭 순서에서 이러한 요소를 찾는 것 외의 기술에 의존 합니다.

[**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 값을 조정 하거나 기본 순서를 사용 하는지에 관계 없이 다음 규칙이 적용 됩니다.

* [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 가 0 인 UI 요소는 XAML 또는 자식 컬렉션의 선언 순서에 따라 탭 순서에 추가 됩니다.
* [**Tabindex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 가 0 보다 큰 UI 요소는 **tabindex** 값에 따라 탭 순서에 추가 됩니다.
* [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) 가 0 보다 작은 UI 요소는 탭 순서에 추가 되 고 0 값 앞에 표시 됩니다. 이는 잠재적으로 HTML의 **tabindex** 특성 처리와 다를 수 있으며, 음수 **TABINDEX** 는 이전 HTML 사양에서 지원 되지 않습니다.

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>UI 요소 내의 키보드 탐색  
복합 요소의 경우 포함 된 요소 간에 적절 한 내부 탐색을 보장 하는 것이 중요 합니다. 복합 요소는 현재 활성 자식을 관리 하 여 모든 자식 요소에 포커스를 둘 수 있게 하는 오버 헤드를 줄일 수 있습니다. 이러한 복합 요소는 탭 순서에 포함 되 고 키보드 탐색 이벤트 자체를 처리 합니다. 대부분의 복합 컨트롤에는 컨트롤의 이벤트 처리에 내장 된 내부 탐색 논리가 이미 있습니다. 예를 들어 항목의 화살표 키 순회는 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView), [**GridView**](/uwp/api/windows.ui.xaml.controls.gridview), [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 및 [**FlipView**](/uwp/api/Windows.UI.Xaml.Controls.FlipView) 컨트롤에서 기본적으로 사용 하도록 설정 됩니다.

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>특정 컨트롤 요소에 대 한 포인터 작업 및 이벤트에 대 한 키보드 대안  
클릭할 수 있는 UI 요소를 키보드를 사용 하 여 호출할 수도 있는지 확인 합니다. UI 요소에 키보드를 사용 하려면 요소에 포커스가 있어야 합니다. [**컨트롤**](/uwp/api/Windows.UI.Xaml.Controls.Control) 에서 파생 된 클래스만 포커스 및 탭 탐색을 지원 합니다.

호출할 수 있는 UI 요소에 대해 스페이스바 및 Enter 키에 대 한 키보드 이벤트 처리기를 구현 합니다. 이렇게 하면 기본 키보드 접근성 지원이 완료 되 고 사용자가 키보드만 사용 하 여 기본 앱 시나리오를 수행할 수 있습니다. 즉, 사용자가 모든 대화형 UI 요소에 연결 하 고 기본 기능을 활성화할 수 있습니다.

UI에서 사용 하려는 요소가 포커스를 가질 수 없는 경우 사용자 지정 컨트롤을 직접 만들 수 있습니다. 포커스를 사용 하려면 [**Istabstop**](/uwp/api/windows.ui.xaml.controls.control.istabstop) 속성을 **true** 로 설정 해야 하며, 포커스 표시기를 사용 하 여 UI를 데코레이팅하는 시각적 상태를 만들어 포커스가 있는 상태를 시각적으로 표시 해야 합니다. 그러나 컨트롤 컴퍼지션을 사용 하면 탭 정지, 포커스, Microsoft UI 자동화 피어 및 패턴에 대 한 지원이 콘텐츠를 작성 하도록 선택 하는 컨트롤에 의해 처리 되는 경우가 많습니다.

예를 들어 [**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image)에 대 한 포인터 누름 이벤트를 처리 하는 대신 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 에 해당 요소를 래핑하여 포인터, 키보드 및 포커스 지원을 받을 수 있습니다.

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>

## <a name="keyboard-shortcuts"></a>바로 가기 키  
앱에 대 한 키보드 탐색 및 활성화를 구현 하는 것 외에도 앱 기능에 대 한 바로 가기를 구현 하는 것이 좋습니다. 탭 탐색 기능을 사용 하면 기본적인 기본 수준의 키보드 지원을 제공 하지만 복잡 한 형식을 사용 하면 바로 가기 키에 대 한 지원도 추가할 수 있습니다. 이를 통해 키보드와 포인팅 장치를 모두 사용 하는 사용자에 대해서도 응용 프로그램을 보다 효율적으로 사용할 수 있습니다.

*바로 가기* 는 사용자가 앱 기능에 액세스 하는 효율적인 방법을 제공 하 여 생산성을 향상 시키는 키보드 조합입니다. 다음과 같은 두 가지 종류의 바로 가기가 있습니다.

* *액세스 키는* 앱에서 UI의 일부에 대 한 바로 가기입니다. 액세스 키는 Alt 키와 문자 키로 구성 됩니다.
* *액셀러레이터 키는* 앱 명령의 바로 가기입니다. 앱에는 명령과 정확히 일치 하는 UI가 있을 수도 있고 없을 수도 있습니다. 액셀러레이터 키는 Ctrl 키와 문자 키로 구성 됩니다.

따라서 화면 읽기 프로그램 및 다른 보조 기술을 사용하는 사용자에게 앱의 바로 가기 키를 찾을 수 있는 손쉬운 방법을 제공할 필요가 있습니다. 도구 설명, 액세스 가능한 이름, 액세스 가능한 설명 또는 다른 형태의 화면 간 통신을 사용 하 여 바로 가기 키를 전달 합니다. 최소한 바로 가기 키는 앱의 도움말 콘텐츠에 잘 설명 되어 있어야 합니다.

해당 바로 가기 키를 설명 하는 문자열로 [**Automationproperties. AccessKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.accesskeyproperty) 연결 된 속성을 설정 하 여 화면 판독기를 통해 액세스 키를 문서화할 수 있습니다. 일반적으로 화면 판독기는 두 속성을 모두 동일한 방식으로 처리 하지만 니모닉이 아닌 바로 가기 키를 문서화 하기 위한 AcceleratorKey 연결 된 속성도 있습니다 [**.**](/uwp/api/windows.ui.xaml.automation.automationproperties.acceleratorkeyproperty) 도구 설명, 자동화 속성 및 필기 도움말 설명서를 사용 하 여 여러 가지 방법으로 바로 가기 키를 문서화 해 보세요.

다음 예제에서는 미디어 재생, 일시 중지 및 중지 단추에 대 한 바로 가기 키를 문서화 하는 방법을 보여 줍니다.

XAML
```xml
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

> [!IMPORTANT]
> [**AcceleratorKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.acceleratorkeyproperty) 또는 [**Automationproperties. AccessKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.accesskeyproperty) 를 설정 하면 키보드 기능을 사용할 수 없습니다. UI 자동화 프레임 워크 에서만 사용 해야 하는 키를 보고 하므로 이러한 정보를 보조 기술을 통해 사용자에 게 전달할 수 있습니다. 키 처리 구현은 XAML이 아닌 코드에서 수행 해야 합니다. 앱에서 바로 가기 키 동작을 실제로 구현 하려면 관련 컨트롤에서 [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) 또는 [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) 이벤트에 대 한 처리기를 연결 해야 합니다. 또한 액세스 키에 대 한 밑줄 텍스트 장식이 자동으로 제공 되지 않습니다. UI에 밑줄이 그어진 텍스트를 표시 하려는 경우 니모닉의 특정 키에 대 한 텍스트를 인라인 [**밑줄**](/uwp/api/Windows.UI.Xaml.Documents.Underline) 형식으로 명시적으로 지정 해야 합니다.

간단히 하기 위해 앞의 예제에서는 "Ctrl + A"와 같은 문자열에 대 한 리소스 사용을 생략 합니다. 그러나 지역화 하는 동안 바로 가기 키도 고려해 야 합니다. 바로 가기 키를 지역화 하는 것은 일반적으로 요소에 대해 표시 되는 텍스트 레이블에 따라 달라 지는 키를 선택 하 여 바로 가기 키를 사용 하는 것이 좋습니다.

바로 가기 키를 구현 하는 방법에 대 한 자세한 지침은 Windows 사용자 환경 상호 작용 지침의 [바로 가기 키](/windows/win32/uxguide/inter-keyboard) 를 참조 하세요.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>키 이벤트 처리기 구현  
키 이벤트와 같은 입력 이벤트에서는 *라우트된 이벤트* 라는 이벤트 개념을 사용합니다. 라우트된 이벤트는 합성 컨트롤의 자식 요소를 통해 버블링 될 수 있습니다. 따라서 공용 컨트롤 부모는 여러 자식 요소에 대 한 이벤트를 처리할 수 있습니다. 이 이벤트 모델은 디자인에 의해 포커스가 없거나 탭 순서의 일부일 수 없는 여러 복합 부분이 포함 된 컨트롤에 대 한 바로 가기 키 작업을 정의 하는 데 편리 합니다.

Ctrl 키와 같은 한정자 확인을 포함 하는 키 이벤트 처리기를 작성 하는 방법을 보여 주는 예제 코드는 [키보드 상호 작용](../input/keyboard-interactions.md)을 참조 하세요.

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>사용자 지정 컨트롤에 대 한 키보드 탐색  
자식 요소에 서로 공간 관계가 있는 경우에는 자식 요소 간을 탐색 하기 위한 바로 가기 키로 화살표 키를 사용 하는 것이 좋습니다. 트리 뷰 노드에 확장-축소 및 노드 활성화를 처리 하기 위한 별도의 하위 요소가 있는 경우 왼쪽 및 오른쪽 화살표 키를 사용 하 여 키보드 확장-축소 기능을 제공 합니다. 컨트롤 콘텐츠 내에서 방향 트래버스를 지 원하는 지향 컨트롤이 있는 경우 적절 한 화살표 키를 사용 합니다.

일반적으로 [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) 및 [**OnKeyUp**](/uwp/api/windows.ui.xaml.controls.control.onkeyup) 메서드의 재정의를 클래스 논리의 일부로 포함 하 여 사용자 지정 컨트롤에 대 한 사용자 지정 키 처리를 구현 합니다.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>포커스 표시기의 시각적 상태 예  
사용자가 시각적 포커스 표시기를 사용할 수 있도록 하는 사용자 지정 컨트롤을 앞에서 언급 했습니다. 일반적으로 포커스 표시기는 컨트롤의 일반 경계 사각형 바로 위에 사각형 모양을 그리는 것 만큼 간단 합니다. 시각적 포커스의 [**사각형**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 은 컨트롤 템플릿에서 컨트롤의 나머지 컴퍼지션에 대 한 피어 요소 이지만, 컨트롤은 아직 포커스 되지 않으므로 [**표시 유형**](/uwp/api/windows.ui.xaml.uielement.visibility) 값이 **축소** 된 상태로 처음 설정 됩니다. 그런 다음 컨트롤이 포커스를 가져오면 시각적 상태가 호출 되어 포커스 시각적 개체의 **표시 여부** 를 **표시** 합니다. 포커스를 다른 곳으로 이동 하면 다른 시각적 상태가 호출 되 고 **표시 유형이** **축소** 됩니다.

포커스를 받을 수 있는 경우 모든 기본 XAML 컨트롤이 포커스를 받을 때 적절 한 시각적 포커스 표시기를 표시 합니다. 사용자가 선택한 테마에 따라 다르게 표시 될 수도 있습니다 (특히 사용자가 고대비 모드를 사용 하는 경우). UI에서 XAML 컨트롤을 사용 하 고 컨트롤 템플릿을 바꾸지 않는 경우 제대로 동작 하 고 표시 하는 컨트롤에 대 한 시각적 포커스 표시기를 얻기 위해 추가로 작업을 수행할 필요가 없습니다. 그러나 컨트롤을 retemplate 하려는 경우 또는 XAML 컨트롤이 시각적 포커스 표시기를 제공 하는 방법에 대 한 자세한 내용은이 섹션의 나머지 부분에서는 XAML 및 컨트롤 논리에서이 작업을 수행 하는 방법을 설명 합니다.

[**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button)에 대 한 기본 xaml 템플릿에서 제공 하는 몇 가지 예제 XAML은 다음과 같습니다.

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

지금 까지는 컴퍼지션만이 있습니다. 포커스 표시기의 표시 여부를 제어 하려면 [**표시 유형**](/uwp/api/windows.ui.xaml.uielement.visibility) 속성을 설정/해제 하는 시각적 상태를 정의 합니다. 이 작업은 컴퍼지션을 정의 하는 루트 요소에 적용 된 [r](/uwp/api/Windows.UI.Xaml.VisualStateManager) 및 r를 사용 하 여 연결 된 속성을 사용 합니다.

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

명명된 상태 중 하나만 [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility)를 직접 조정하고 다른 상태는 비어 있는 것처럼 표시됩니다. 시각적 상태의 작동 방식은 컨트롤이 동일한 [**Visualstategroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup)의 다른 상태를 사용 하는 즉시 이전 상태에서 적용 된 애니메이션이 즉시 취소 된다는 것입니다. 컴퍼지션의 기본 **표시 유형이** **축소** 되기 때문에 사각형이 표시 되지 않습니다. 제어 논리는 [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) 와 같은 포커스 이벤트를 수신 하 고 [**GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate)를 사용 하 여 상태를 변경 하 여이를 제어 합니다. 기본 컨트롤을 사용 하거나 이미 해당 동작을 가진 컨트롤에 따라 사용자 지정 하는 경우이는 이미 처리 된 것입니다.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>키보드 접근성 및 Windows Phone
일반적으로 Windows Phone 장치에는 전용 하드웨어 키보드가 없습니다. 그러나 SIP(Soft Input Panel)가 여러 가지 키보드 접근성 시나리오를 지원할 수 있습니다. 화면 읽기 권한자는 삭제 발표를 포함 하 여 SIP **텍스트** 에서 텍스트 입력을 읽을 수 있습니다. 화면 읽기 프로그램은 사용자가 키를 검색 하는 것을 감지할 수 있으며 스캔 한 키 이름을 소리내어 읽도록 하기 때문에 사용자는 손가락의 위치를 검색할 수 있습니다. 또한 키보드 중심의 접근성 개념 중 일부를 키보드를 사용 하지 않는 관련 보조 기술 동작에 매핑할 수도 있습니다. 예를 들어 SIP는 Tab 키를 포함 하지 않는 경우에도 Tab 키를 누르는 것과 동일한 터치 제스처를 지원 하므로 UI의 컨트롤을 통해 유용한 탭 순서를 유지 하는 것은 여전히 중요 한 액세스 가능성 원칙입니다. 복합 컨트롤 내에서 파트를 탐색 하는 데 사용 되는 화살표 키도 내레이터 터치 제스처를 통해 지원 됩니다. 포커스가 텍스트 입력이 아닌 컨트롤에 도달 하면 내레이터는 해당 컨트롤의 작업을 호출 하는 제스처를 지원 합니다.

SIP에는 제어 또는 키 키가 포함 되지 않기 때문에 바로 가기 키는 일반적으로 Windows Phone 앱과 관련이 없습니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목

* [접근성](accessibility.md)
* [키보드 조작](../input/keyboard-interactions.md)
* [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [XAML 접근성 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
