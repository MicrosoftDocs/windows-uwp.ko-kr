---
Description: If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: 키보드 접근성
label: Keyboard accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 938b5b3cdd2e23995a1031875a28f178e0c97a26
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7699768"
---
# <a name="keyboard-accessibility"></a>키보드 접근성  



따라서 앱의 키보드 접근성이 좋지 않을 경우 시각 장애나 이동성 문제가 있는 사용자가 앱을 사용하는 데 어려움을 겪거나 아예 사용하지 못할 수 있습니다.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>UI 요소 간 키보드 탐색  
컨트롤로 키보드를 사용하려면 컨트롤에 포커스가 있어야 하고, (포인터를 사용하지 않고) 포커스를 받으려면 탭 탐색을 통해 UI 디자인에서 컨트롤에 액세스할 수 있어야 합니다. 기본적으로 컨트롤의 탭 순서는 디자인 화면에 추가되는 순서, XAML에 표시되는 순서 또는 컨테이너에 프로그래밍 방식으로 추가된 순서와 같습니다.

대부분의 경우, XAML에서 컨트롤을 정의한 방법에 따른 기본 순서가 특히 화면 읽기 프로그램에서 컨트롤을 읽는 순서이므로 최적의 순서입니다. 그러나 기본 순서가 시각적 순서와 반드시 일치하는 것은 아닙니다. 실제 디스플레이 위치는 레이아웃에 적용하기 위해 자식 요소에 설정할 수 있는 부모 레이아웃 컨테이너 및 특정 속성에 따라 달라질 수 있습니다. 앱이 적합한 탭 순서를 갖도록 하려면 이 동작을 직접 테스트하세요. 특히 레이아웃에 대한 그리드 메타포 또는 테이블 메타포가 있는 경우 사용자가 읽는 순서와 탭 순서가 달라질 수 있습니다. 이것은 자체로는 항상 문제는 아닙니다. 하지만 터치 가능한 UI와 키보드로 액세스 가능한 UI로서의 앱의 기능을 테스트하여 두 방식 모두 UI가 제대로 작동하는지 확인해야 합니다.

XAML을 조정하여 탭 순서가 시각적 순서와 일치하도록 할 수 있습니다. 또는 열 우선 탭 탐색을 사용하는 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 레이아웃의 다음 예에 표시된 대로 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 속성을 설정하여 기본 탭 순서를 재정의할 수 있습니다.

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

탭 순서에서 컨트롤을 제외할 수도 있습니다. 일반적으로 컨트롤을 비대화형으로 설정하여 이 작업을 수행합니다. 예를 들어 해당 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419) 속성을 **false**로 설정합니다. 사용할 수 없는 컨트롤은 탭 순서에서 자동으로 제외됩니다. 그러나 경우에 따라 사용할 수 있는 경우에도 탭 순서에서 컨트롤을 제외할 수 있습니다. 이 경우 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) 속성을 **false**로 설정할 수 있습니다.

일반적으로 포커스가 있는 모든 요소는 기본적으로 탭 순서에 있습니다. 텍스트 선택을 위해 클립보드에서 액세스할 수 있도록 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) 같은 특정 텍스트 표시 형식에 포커스가 있는 경우는 예외입니다. 그러나 정적 텍스트 요소가 탭 순서에 있어서는 안 되므로 탭 순서에 없습니다. 정적 요소는 기본적으로 대화형이 아니므로 호출할 수 없으며 텍스트 입력을 요구하지 않지만 텍스트의 선택 포인트 찾기 및 조정을 지원하는 [텍스트 컨트롤 패턴](https://msdn.microsoft.com/library/windows/desktop/Ee671194)은 지원합니다. 텍스트는 포커스를 설정하면 가능한 일부 동작을 사용하도록 설정하는 의미가 없어야 합니다. 텍스트 요소는 보조 기술에 의해 검색되고 화면 프로그램에서 소리 내어 읽지만 특정 탭 순서에서 해당 요소를 찾는 것과는 다른 기술을 사용합니다.

[**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 값을 조정할지 기본 순서를 사용할지에 따라 다음 규칙이 적용됩니다.

* [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)가 0인 UI 요소는 XAML 또는 자식 컬렉션의 선언 순서를 기준으로 탭 순서에 추가됩니다.
* [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)가 0보다 큰 UI 요소는 **TabIndex** 값을 기준으로 탭 순서에 추가됩니다.
* [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)가 0보다 작은 UI 요소는 탭 순서에 추가되며 0 값 앞에 나타납니다. 이것은 잠재적으로 HTML에서 **tabindex** 특성을 처리하는 것과는 다르며, 이전 HTML 사양에서는 음의 **tabindex**가 지원되지 않았습니다.

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>UI 요소 내 키보드 탐색  
복합 요소의 경우 포함된 요소 간에 적절한 내부 탐색이 보장되어야 합니다. 복합 요소는 현재 활성 자식 요소를 관리하여 모든 자식 요소를 포커스 가능 상태가 되게 하는 오버헤드를 줄일 수 있습니다. 그러한 복합 요소는 탭 순서대로 포함되며 키보드 탐색 이벤트 자체를 처리합니다. 많은 복합 컨트롤에는 몇 가지 내부 탐색 논리가 컨트롤의 이벤트 처리에 이미 내장되어 있습니다. 예를 들어 항목의 화살표 키 통과는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878), [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview), [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 및 [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678) 컨트롤에서 기본적으로 사용됩니다.

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>특정 컨트롤 이벤트의 포인터 동작 및 이벤트를 대신하는 키보드  
클릭할 수 있는 UI 요소는 키보드를 사용해서도 호출할 수 있어야 합니다. UI 요소와 함께 키보드를 사용하려면 요소에 포커스가 있어야 합니다. [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390)에서 파생되는 클래스만 포커스 및 탭 탐색을 지원합니다.

호출할 수 있는 UI 요소의 경우 스페이스바 및 Enter 키에 대한 키보드 이벤트 처리기를 구현합니다. 이렇게 하면 기본 키보드 접근성 지원이 완벽해지고 사용자가 키보드만 사용하여 기본 앱 시나리오를 수행할 수 있습니다. 즉, 사용자는 모든 대화형 UI 요소에 액세스하고 기본 기능을 활성화할 수 있습니다.

UI에서 사용하려는 요소에 포커스가 없는 경우 사용자 지정 컨트롤을 직접 만들 수 있습니다. [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) 속성을 **true**로 설정하여 포커스를 사용하도록 설정하고 포커스 표시기로 UI를 데코레이트하는 시각적 상태를 만들어 포커스된 상태의 시각적 표시를 제공해야 합니다. 그러나 포함된 콘텐츠를 구성하기 위해 선택한 컨트롤로 탭 중지, 포커스, Microsoft UI 자동화 피어 및 패턴에 대한 지원을 처리하도록 컨트롤 컴퍼지션을 이용하는 것이 더욱 간편합니다.

예를 들어 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752)에서 포인터 누름 이벤트를 처리하는 대신 포인터, 키보드 및 포커스 지원을 가져오도록 해당 요소를 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)에 래핑할 수 있습니다.

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
앱의 키보드 탐색 및 활성화 구현뿐 아니라 앱 기능에 대한 바로 가기를 구현하는 것이 좋습니다. 탭 탐색은 기본 수준의 키보드 지원을 제공하지만 복잡한 양식을 사용할 경우 바로 가기 키에 대한 지원을 추가해야 합니다. 이를 통해 키보드와 포인팅 디바이스를 모두 사용하는 사람에게도 훨씬 사용하기 편리한 응용 프로그램이 될 수 있습니다.

*바로 가기*는 사용자가 앱 기능에 효율적으로 액세스할 수 있도록 하여 생산성을 향상하는 키보드 조합입니다. 다음과 같은 두 종류의 바로 가기가 있습니다.

* *선택키*는 앱의 UI에 대한 바로 가기입니다. 선택키는 Alt 키와 문자 키로 구성됩니다.
* *바로 가기 키*는 앱 명령에 대한 바로 가기입니다. 명령과 정확히 일치하는 UI가 앱에 없을 수도 있습니다. 바로 가기 키는 Ctrl 키와 문자 키로 구성됩니다.

따라서 화면 읽기 프로그램 및 다른 보조 기술을 사용하는 사용자에게 앱의 바로 가기 키를 찾을 수 있는 손쉬운 방법을 제공할 필요가 있습니다. 도구 설명, 접근성 있는 이름, 접근성 있는 설명 또는 몇 가지 다른 형식의 화면 통신을 사용하여 바로 가기 키를 제공합니다. 최소한 바로 가기 키에 대한 설명이 앱의 도움말 콘텐츠에 잘 나타나 있어야 합니다.

[**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) 연결된 속성을 바로 가기 키를 설명하는 문자열로 설정하여 화면 읽기 프로그램을 통해 액세스 키를 문서화할 수 있습니다. 또한 니모닉이 아닌 바로 가기 키를 문서화하는 데 사용하는 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) 연결된 속성도 있습니다. 단, 화면 읽기 프로그램은 대개 두 속성을 같은 방식으로 처리합니다. 도구 설명, 자동화 속성 및 작성된 도움말 문서를 사용하여 여러 가지 방법으로 바로 가기 키를 문서화합니다.

다음 예제에서는 미디어 재생, 일시 중지 및 중지 단추에 대한 바로 가기 키를 문서화하는 방법을 보여 줍니다.

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
> [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) 또는 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) 설정을 통해 키보드 기능을 사용하도록 설정할 수 없습니다. 이 설정은 사용해야 하는 키를 UI 자동화 프레임워크에 보고하여 보조 기술을 통해 이러한 정보를 사용자에게 전달할 수 있도록 하는 역할만 합니다. 키 처리 구현은 XAML이 아니라 코드에서 수행해야 합니다. 앱에서 바로 가기 키 동작을 실제로 구현하려면 관련 컨트롤에 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941) 또는 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942) 이벤트 처리기를 연결해야 합니다. 또한 액세스 키에 대한 밑줄로 표시된 텍스트 장식은 자동으로 제공되지 않습니다. UI에서 밑줄로 표시된 텍스트를 표시하려면 니모닉에서 명시적으로 특정 키의 텍스트에 밑줄을 인라인 [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982) 서식으로 표시해야 합니다.

간단하게 하기 위해 위 예제에서는 "Ctrl+A" 같은 문자열에 대한 리소스 사용을 생략합니다. 그러나 지역화할 때도 바로 가기 키를 고려해야 합니다. 바로 가기 키로 사용할 키의 선택은 일반적으로 해당 요소에 대해 표시되는 텍스트 레이블에 따라 달라지므로 바로 가기 키 지역화가 관련이 있습니다.

바로 가기 키 구현에 대한 자세한 내용은 Windows 사용자 환경 조작 지침에서 [바로 가기 키](http://go.microsoft.com/fwlink/p/?linkid=221825)(영문)를 참조하세요.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>키보드 이벤트 처리기 구현  
키 이벤트와 같은 입력 이벤트에서는 *라우트된 이벤트*라는 이벤트 개념을 사용합니다. 라우트된 이벤트는 복합 컨트롤의 자식 요소로 넘쳐날 수 있으므로 공용 컨트롤 부모에서 여러 자식 요소에 대한 이벤트를 처리할 수 있습니다. 이 이벤트 모델은 기본적으로 포커스가 없거나 탭 순서의 일부가 아닐 수 있는 여러 복합 부분을 포함하는 컨트롤의 바로 가기 키 동작을 정의하는 데 편리합니다.

Ctrl 키 등의 보조키 확인을 포함하는 키 이벤트 처리기 작성 방법을 보여 주는 예제 코드는 [키보드 조작](https://msdn.microsoft.com/library/windows/apps/Mt185607)을 참조하세요.

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>사용자 지정 컨트롤의 키보드 탐색  
자식 요소를 탐색하는 키보드 바로 가기로 화살표 키를 사용하는 것이 좋습니다. 이 경우 자식 요소는 서로 특별한 관계가 있습니다. 트리 뷰 노드에 확장/축소 처리와 노드 활성화에 대한 별도의 하위 요소가 있는 경우 왼쪽 및 오른쪽 화살표 키를 사용하여 키보드 확장/축소 기능을 제공합니다. 컨트롤 콘텐츠 내에 방향 통과를 지원하는 방향이 지정된 컨트롤이 있는 경우 적절한 화살표 키를 사용합니다.

일반적으로 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982.aspx) 및 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983.aspx) 메서드의 재정의를 클래스 논리의 일부로 포함하여 사용자 지정 컨트롤에 대한 사용자 지정 키 처리를 구현합니다.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>포커스 표시기의 시각적 상태 예제  
앞에서 사용자가 포커스를 설정할 수 있는 모든 사용자 지정 컨트롤에 시각적 포커스 표시기가 있어야 한다고 설명했습니다. 일반적으로 포커스 표시기는 컨트롤의 일반 경계 사각형 주위에 사각형 모양을 그리는 것처럼 단순합니다. 시각적 포커스의 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)은 컨트롤 템플릿에 있는 컨트롤 컴퍼지션의 나머지 부분에 대한 피어 요소이지만 컨트롤이 아직 포커스되지 않았으므로 초기에 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 값 **Collapsed**를 사용하여 설정됩니다. 그런 다음 컨트롤에 포커스가 있으면 구체적으로 시각적 포커스의 **Visibility**를 **Visible**로 설정하는 시각적 상태가 호출됩니다. 포커스가 다른 곳으로 이동되면 다른 시각적 상태가 호출되고 **Visibility**는 **Collapsed**가 됩니다.

모든 기본 XAML 컨트롤은 포커스되었을 때 해당하는 시각적 포커스 표시기를 표시합니다(포커스될 수 있는 경우). 또한 사용자가 선택한 테마에 따라 모양이 다를 수 있습니다(특히 사용자가 고대비 모드를 사용 중인 경우). UI에서 XAML 컨트롤을 사용 중이며 컨트롤 템플릿을 바꾸지 않는 경우 제대로 작동하고 표시하는 컨트롤에 대한 시각적 포커스 표시기를 얻기 위해 추가로 어떤 작업도 수행할 필요가 없습니다. 하지만 컨트롤을 다시 템플릿으로 지정하려는 경우나 XAML 컨트롤이 시각적 포커스 지시기를 제공하는 방법을 알고 싶은 경우를 위해 이 섹션의 나머지 부분에서는 XAML 및 컨트롤 논리에서 이를 수행하는 방법을 설명합니다.

다음은 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)에 대한 기본 XAML 템플릿에서 가져온 일부 XAML입니다.

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

지금까지는 단순히 컴퍼지션이었습니다. 포커스 표시기의 표시 형식을 제어하려면 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 속성을 전환하는 시각적 상태를 정의합니다. 이 작업은 컴퍼지션을 정의하는 루트 요소에 적용되는 [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) 연결된 속성을 사용하여 수행합니다.

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

명명된 상태 중 하나만 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)를 직접 조정하고 다른 상태는 비어 있는 것처럼 표시됩니다. 시각적 상태는 컨트롤이 동일한 [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014)의 다른 상태를 사용하는 즉시 이전 상태에 의해 적용된 모든 애니메이션이 즉시 취소되는 방식으로 작동합니다. 컴퍼지션의 기본 **Visibility**는 **Collapsed**이므로 사각형이 표시되지 않습니다. 컨트롤 논리는 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927) 같은 포커스 이벤트를 수신 대기하고 [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025)를 통해 상태를 변경하여 이를 제어합니다. 기본 컨트롤을 사용하거나 이미 해당 동작이 포함된 컨트롤을 기반으로 사용자 지정하는 경우 이 작업이 이미 처리되어 있습니다.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>키보드 접근성 및 Windows Phone
Windows Phone 장치에는 일반적으로 전용 하드웨어 키보드가 없습니다. 그러나 SIP(Soft Input Panel)가 여러 가지 키보드 접근성 시나리오를 지원할 수 있습니다. 화면 읽기 프로그램은 **텍스트** SIP에서 삭제 알림을 포함한 텍스트 입력을 읽을 수 있습니다. 화면 읽기 프로그램은 사용자가 키를 검색하고 있음을 감지하고 검색한 키 이름을 소리 내어 읽을 수 있으므로 사용자가 손가락의 위치를 검색할 수 있습니다. 또한 키보드 지향 접근성 개념 일부를 키보드를 전혀 사용하지 않는 관련 보조 기술 동작에 매핑할 수 있습니다. 예를 들어 SIP에 Tab 키가 포함되지 않은 경우에도 내레이터는 Tab 키를 누르는 것과 동일한 터치 제스처를 지원하므로 UI의 컨트롤을 통과하는 유용한 탭 순서가 있는 것도 중요한 접근성 원칙입니다. 복잡한 컨트롤 내 일부를 탐색하는 데 사용되는 화살표 키도 내레이터 터치 제스처를 통해 지원됩니다. 포커스가 텍스트 입력용이 아닌 컨트롤에 도달하면 내레이터는 해당 컨트롤의 동작을 호출하는 제스처를 지원합니다.

바로 가기 키는 일반적으로 Windows Phone 앱에 적합하지 않습니다. SIP에 Control 키나 Alt 키가 없기 때문입니다.

<span id="related_topics"/>

## <a name="related-topics"></a>관련 항목  
* [접근성](accessibility.md)
* [키보드 조작](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [터치 키보드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [XAML 접근성 샘플](http://go.microsoft.com/fwlink/p/?linkid=238570)

