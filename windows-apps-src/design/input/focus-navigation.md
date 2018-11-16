---
author: Karl-Bridge-Microsoft
title: 키보드, 게임 패드, 원격 제어 및 접근성 도구에 대한 포커스 탐색
Description: ''
label: ''
template: detail.hbs
keywords: 키보드, 게임 컨트롤러, 원격 제어, 탐색, 방향 내부 탐색, 방향 영역, 탐색 전략, 입력, 사용자 조작, 접근성, 유용성
ms.date: 03/02/2018
ms.author: kbridge
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f4c86847e02c4ba987f8b1dcc42016145b175193
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981112"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>키보드, 게임 패드, 원격 제어 및 접근성 도구에 대한 포커스 탐색

![키보드, 원격 및 D 패드](images/dpad-remote/dpad-remote-keyboard.png)

포커스 탐색을 사용하면 텔레비전 화면 및 Xbox One의 3m 환경뿐만 아니라 UWP 앱과 키보드 고급 사용자용 사용자 지정 컨트롤(장애가 있는 사용자 및 기타 접근성 요구 사항 포함)에서 포괄적이고 일관된 상호 작용 환경을 제공할 수 있습니다.

## <a name="overview"></a>개요

포커스 탐색은 사용자가 키보드, 게임 패드 또는 원격 제어를 사용하여 UWP 응용 프로그램의 UI를 사용하여 탐색 및 조작을 수행하도록 해주는 기본 메커니즘입니다.

> [!NOTE] 
> 입력 장치는 일반적으로 터치, 터치 패드, 펜, 마우스 등의 포인팅 장치와 키보드, 게임 패드, 원격 제어 등의 비 포인팅 장치로 분류됩니다.

이 항목에서는 비 포인팅 입력 유형을 사용하는 사용자를 위해 UWP 응용 프로그램을 최적화하고 사용자 지정 조작 환경을 빌드하는 방법을 설명합니다. 

이 문서에서는 PC의 UWP 앱에서 사용자 지정 컨트롤을 사용할 수 있는 키보드 입력에 대해 중점적으로 설명하지만, Windows 내레이터와 같은 접근성 도구를 지원하고 3m 환경을 지원하는 데 있어 터치 키보드 및 화상 키보드(OSK)와 같은 소프트웨어 키보드의 경우 잘 설계된 키보드 환경도 중요합니다.

포인팅 장치에 대한 UWP 응용 프로그램의 사용자 지정 환경 구축에 대한 지침은 [포인터 입력 처리](handle-pointer-input.md)를 참조하세요.

키보드용 환경 및 앱 빌드에 대한 일반적인 내용을 [키보드 조작](keyboard-interactions.md)을 참조하세요.

## <a name="general-guidance"></a>일반 지침
사용자 조작이 필요한 UI 요소만 포커스 탐색을 지원해야 하며, 정적 이미지와 같이 동작이 필요하지 않은 요소의 경우 포커스 탐색이 필요하지 않습니다. 화면 읽기 프로그램 및 이와 유사한 접근성 도구는 포커스 탐색에 포함되지 않은 경우에도 이러한 고정 요소가 들어 있습니다. 

마우스 또는 터치와 같은 포인터 장치를 사용하여 탐색하는 것과는 다르게 포커스 탐색은 선형이라는 점에 주의해야 합니다. 포커스 탐색을 구현하는 경우, 사용자가 응용 프로그램을 조작하는 방법과 논리적 탐색을 어떻게 구현할지를 고려해야 합니다. 대부분의 경우, 사용자 지정 포커스 탐색 동작이 사용자의 문화의 기본 읽기 패턴을 따르는 것이 좋습니다.

포커스 탐색이 고려해야 하는 몇 가지 다른 요소는 다음과 같습니다.
- 컨트롤이 논리적으로 그룹화되어 있습니까?
- 중요도가 더 큰 컨트롤 그룹이 있습니까? 
   - 있는 경우, 이러한 그룹에 하위 그룹이 포함되어 있습니까?
- 레이아웃에서 사용자 지정 방향 탐색(화살표 키) 및 탭 순서를 필요로 합니까?

[Engineering Software for Accessibility(접근성을 위해 소프트웨어 엔지니어링)](https://www.microsoft.com/download/details.aspx?id=19262) 전자책의 *Designing the Logical Hierarchy(논리 계층 디자인)* 장에 이 주제에 대해 자세히 설명되어 있습니다.

## <a name="2d-directional-navigation-for-keyboard"></a>키보드용 2D 방향 탐색

2D 내부 탐색 컨트롤 지역 또는 컨트롤 그룹을 "방향 영역"이라고 부릅니다. 이 개체로 포커스가 옮겨 가면, 키보드의 화살표 키(왼쪽, 오른쪽, 위쪽 및 아래쪽)을 사용하여 방향 영역 내 하위 요소 간을 탐색할 수 있습니다.

![방향 영역](images/keyboard/directional-area-small.png)

*2D 내부 탐색 지역 또는 컨트롤 그룹의 방향 영역*

[XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) 속성([가능한 값: 자동](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [사용](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) 또는 [사용 안 함](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode))을 사용하면 키보드 화살표 키를 사용하여 2D 내부 탐색을 관리할 수 있습니다.

> [!NOTE]  
> 탭 순서는 이 속성의 영향을 받지 않습니다. 탐색 환경의 혼란을 방지하기 위해 방향 영역의 하위 요소를 응용 프로그램의 탭 탐색 순서에서 명시적으로 지정하지 *않아야* 합니다. 요소의 탭 동작에 대한 자세한 내용은 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 및 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 참조하세요.  

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[자동](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)(기본 동작)

자동으로 설정되어 있으면, 방향 탐색 동작이 요소의 상위 구조 또는 상속 계층을 기반으로 결정됩니다. 모든 상위 항목이 기본 모드(**자동**으로 설정됨)에 있으면 키보드를 사용한 방향 탐색이 지원되지 *않습니다*.

### [<a name="disabled"></a>사용 안 함](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

**XYFocusKeyboardNavigation**을 **사용 안 함**으로 설정하면 컨트롤 및 하위 요소에 대해 방향 탐색을 차단할 수 있습니다.

![XYFocusKeyboardNavigation을 사용하지 않는 동작](images/keyboard/xyfocuskeyboardnav-disabled.gif)

*XYFocusKeyboardNavigation을 사용하지 않는 동작*

이 예에서는 기본 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)(ContainerPrimary)의 **XYFocusKeyboardNavigation**이 **사용**으로 설정되어 있습니다. 모든 하위 요소가 이 설정을 상속하며 화살표 키로 이를 탐색할 수 있습니다. 그러나 B3와 B4 요소는 보조 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary)에 **XYFocusKeyboardNavigation** 설정이 **사용 안 함**으로 설정되어 있으므로, 기본 컨테이너를 재정의하여 화살표 키가 하위 요소 자체를 및 하위 요소 간을 탐색할 수 없습니다.

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### [<a name="enabled"></a>활성화](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

**XYFocusKeyboardNavigation**을 **사용**으로 설정하면 컨트롤 및 해당 [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 하위 개체 각각에 대한 2D 방향 탐색을 사용할 수 있습니다.

설정 시 화살표 키를 사용하는 탐색은 방향 영역 내 요소로 제한됩니다. 모든 컨트롤이 탭 순서 계층 구조를 통해 액세스 가능 상태로 유지되므로 탭 탐색은 영향을 받지 않습니다.

![XYFocusKeyboardNavigation을 사용하는 동작](images/keyboard/xyfocuskeyboardnav-enabled.gif)

*XYFocusKeyboardNavigation을 사용하는 동작*

이 예에서는 기본 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)(ContainerPrimary)의 **XYFocusKeyboardNavigation**이 **사용**으로 설정되어 있습니다. 모든 하위 요소가 이 설정을 상속하며 화살표 키로 이를 탐색할 수 있습니다. B3 및 B4 요소에서 보조 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary)의 **XYFocusKeyboardNavigation**이 설정되어 있지 않으므로 기본 컨테이너 설정을 상속합니다. B5 요소는 선언된 방향 영역 내에 있지 않으므로 화살표 키 탐색은 지원하지 않지만 표준 탭 탐색 동작은 지원합니다.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

다양한 수준의 중첩된 방향 영역을 사용할 수 있습니다. 모든 부모 요소에서 XYFocusKeyboardNavigation이 사용으로 설정되면 내부 탐색 지역 경계가 무시됩니다.

다음은 2D 방향 탐색을 명시적으로 지원하지 않는 요소 내에 있는 두 개의 중첩 방향 영역의 예입니다. 이 경우, 두 개의 중첩된 영역 간에 방향 탐색이 지원되지 않습니다.

![XYFocusKeyboardNavigation을 사용하는 중첩 동작](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)

*XYFocusKeyboardNavigation을 사용하는 중첩 동작*

다음은 좀 더 복잡한 3개의 중첩 방향 영역의 예입니다.

-   B1에 포커스가 있는 경우 방향 영역 경계의 XYFocusKeyboardNavigation이 사용 안 함으로 설정되어 있으므로 B5로만 탐색할 수 있으며 B2, B3 및 B4는 화살표 키로 도달할 수 없습니다(반대의 경우도 마찬가지임).
-   B2에 포커스가 있는 경우 방향 영역 경계에서 B1, B4 및 B5로의 화살표 키 탐색을 차단하므로 B3으로만 탐색할 수 있습니다(반대의 경우도 마찬가지임).
-   B4에 포커스가 있는 경우, 탭 키를 사용해야만 컨트롤 간을 탐색할 수 있습니다.

![XYFocusKeyboardNavigation을 사용하는 보다 복잡한 중첩 동작](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*XYFocusKeyboardNavigation을 사용하는 보다 복잡한 중첩 동작*

## <a name="tab-navigation"></a>탭 탐색

화살표 키는 컨트롤 또는 컨트롤 그룹 내 2D 방향 탐색에 사용할 수 있으므로, 탭 키를 사용하면 UWP 응용 프로그램에 있는 모든 컨트롤 간을 탐색할 수 있습니다. 

모든 대화형 컨트롤은 기본적으로 탭 키 탐색을 지원하며([IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) 및 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성이 **True**), 이때 논리적 탭 순서는 응용 프로그램의 컨트롤 레이아웃으로부터 파생됩니다. 그러나 기본 순서가 시각적 순서와 반드시 일치하는 것은 아닙니다. 실제 디스플레이 위치는 레이아웃에 적용하기 위해 자식 요소에 설정할 수 있는 부모 레이아웃 컨테이너 및 특정 속성에 따라 달라질 수 있습니다.

포커스가 응용 프로그램 내에서 무질서하게 이동하도록 사용자 지정 탭 순서를 지정하면 안 됩니다. 예를 들어 양식의 컨트롤 목록은 탭 순서가 위에서 아래로, 왼쪽에서 오른쪽으로 흘러야 합니다(로캘에 따라 다름).

이 섹션에서는 앱에 맞게 이러한 탭 순서를 완전히 사용자 지정하는 방법을 설명합니다.

### <a name="set-the-tab-navigation-behavior"></a>탭 탐색 동작 설정

[UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)의 [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 속성은 전체 개체 영역 트리(또는 방향 영역)의 탭 탐색 동작을 지정합니다.

> [!NOTE]
> [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate)를 사용하지 않는 객체의 경우 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) 속성 대신 이 속성을 사용하여 모양을 정의하세요.

이전 섹션에서 설명한 대로 탐색 환경의 혼란을 방지하기 위해 방향 영역의 하위 요소를 응용 프로그램의 탭 탐색 순서에서 명시적으로 지정하지 *않아야* 합니다. 요소의 탭 동작에 대한 자세한 내용은 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 및 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 참조하세요.   
> Windows 10 크리에이터스 업데이트(빌드 10.0.15063) 이전 버전의 경우, 탭 설정이 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) 개체로 제한되어 있습니다. 자세한 내용은 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation)을 참조하세요.

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation)의 값 유형은 [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode)이며, 가능한 값은 다음과 같습니다(이 예제는 사용자 지정 컨트롤 그룹이 아니며 화살표 키를 사용한 내부 탐색을 요구하지 않습니다).

- **로컬**(기본값)   
  탭 인덱스가 컨테이너 내의 로컬 하위 트리에서 인식됩니다. 이 예에서는 탭 순서가 B1, B2, B3, B4, B5, B6, B7, B1입니다.

   !["로컬" 탭 탐색 동작](images/keyboard/tabnav-local.gif)

   *"로컬" 탭 탐색 동작*

- **한 번**  
  컨테이너 및 모든 하위 요소가 포커스를 한 번씩 받습니다. 이 예의 경우 탭 순서가 B1, B2, B7, B1입니다(화살표 키를 사용한 내부 탐색도 설명됨).

   !["한 번" 탭 탐색 동작](images/keyboard/tabnav-once.gif)

   *"한 번" 탭 탐색 동작*

- **주기**   
  컨테이너 내의 첫 포커스 가능 요소로 되돌아오는 포커스 주기입니다. 이 예에서는 탭 순서가 B1, B2, B3, B4, B5, B6, B2...입니다.

   !["주기" 탭 탐색 동작](images/keyboard/tabnav-cycle.gif)

   *"주기" 탭 탐색 동작*

다음은 이전 예의 코드입니다(TabFocusNavigation = "주기"로 설정).

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### [<a name="tabindex"></a>TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

[TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)를 사용하면 사용자가 탭 키를 사용하여 컨트롤을 탐색할 때 포커스를 받는 요소의 순서를 지정할 수 있습니다. 탭 인덱스가 낮은 컨트롤이 인덱스가 높은 컨트롤보다 먼저 포커스를 받습니다.

컨트롤에 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex)가 지정되어 있지 않으면, 범위를 기반으로 하여 시각적 트리에 있는 모든 대화형 컨트롤의 현재 가장 높은 인덱스 값(및 가장 낮은 우선 순위)보다 높은 인덱스 값이 할당됩니다. 

모든 하위 요소 컨트롤은 범위로 간주되며, 이러한 요소 중 하나에도 하위 요소가 있는 경우 다른 범위로 간주됩니다. 범위가 모호한 경우 범위의 시각적 트리에서 첫 번째 요소를 선택하면 됩니다. 

탭 순서에서 컨트롤을 제외하려면 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성을 **False**로 설정하세요.

[TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 설정하여 기본 탭 순서를 재정의하세요.

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex)는 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 및 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) 모두에 대해 동일한 방식으로 작동합니다.


다음은 특정 요소의 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성이 포커스 탐색에 미치는 영향에 대한 설명입니다. 

![TabIndex 동작을 사용한 "로컬" 탭 탐색](images/keyboard/tabnav-tabindex.gif)

*TabIndex 동작을 사용한 "로컬" 탭 탐색*

위 예제에서 두 개의 범위는 다음과 같습니다. 
- B1, 방향 영역(B2 - B6) 및 B7
- 방향 영역(B2 - B6)

방향 영역에서 B3에 포커스가 있으면, 범위가 바뀌며, 이후 포커스가 이동할 가능성이 가장 큰 방향 영역으로 탭 탐색이 이동합니다. 이 경우 B2 다음에 B4, B5 및 B6가 옵니다. 범위가 지정된 다음 다시 변경되고 포커스가 B1으로 이동합니다.

다음은 이 예에 대한 코드입니다.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>키보드, 게임 패드 및 원격 제어에 대한 2D 방향 탐색

키보드, 게임 패드, 원격 제어와 같은 비 포인터 입력 유형과 Windows 내레이터와 같은 접근성 도구는 UWP 응용 프로그램의 UI를 탐색하고 이를 조작하는 데 있어 공통되는 기본 메커니즘을 사용합니다.

이 섹션에서는 포커스 기반의 비 포인터 입력 유형 모두를 지원하는 탐색 전략 속성 세트를 통해 기본 탐색 전략을 지정하고 응용 프로그램 내 포커스 탐색을 세부 조정하는 방법을 설명합니다.

Xbox/TV용 앱 및 환경을 구축하는 방법에 대한 자세한 내용은 [키보드 조작](keyboard-interactions.md) 및 [Xbox 및 TV용 디자인](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction)를 참조하세요.

### <a name="navigation-strategies"></a>탐색 전략

> 탐색 전략은 키보드, 게임 패드, 원격 제어 및 다양한 접근성 도구에 적용됩니다.

다음 탐색 전략 속성을 사용하면 화살표 키, 방향 패드(D 패드) 단추 또는 이와 유사한 누름 항목을 기반으로 어떤 컨트롤이 포커스를 받을지에 대해 영향을 줄 수 있습니다. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

이러한 속성의 가능한 값으로는 [자동](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)(기본값), [NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) 또는 [RectilinearDistance ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)가 있습니다.

이 값을 **자동**으로 설정하면 요소의 상위 항목에 따라 요소의 동작이 결정됩니다. 모든 요소가 **자동**으로 설정되면 **프로젝션**이 사용됩니다.

> [!NOTE]
> 이전에 포커스가 설정된 요소, 탐색 방향 축과의 근접성 등의 기타 요소가 결과에 영향을 미칠 수 있습니다.

### <a name="projection"></a>프로젝션

프로젝션 전략은 탐색 방향에서 현재 포커스된 요소의 테두리를 *표시*할 때 발생하는 첫 번째 요소로 포커스를 이동합니다.

이 예에서는 각 포커스 탐색 방향이 프로젝션으로 설정되어 있습니다. 포커스가 B3을 우회하여 B1에서 B4로 이동하는 것을 알 수 있습니다. 이는 B3이 프로젝션 영역에 있지 않기 때문입니다. 또한 B1에서 왼쪽으로 이동할 때 포커스 후보가 식별되지 않는 것을 볼 수 있습니다. 이는 B1을 기준으로 한 B2의 위치로 인해 B3가 후보에서 제외되기 때문입니다. B3이 B2와 동일한 줄에 있었다면, 왼쪽 탐색 시 강력한 후보가 되었을 것입니다. 탐색 방향 축으로의 이동에 장애물이 없으므로 B2가 강력한 후보가 됩니다.

![프로젝션 탐색 전략](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*프로젝션 탐색 전략*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 전략은 탐색 방향의 축과 가장 가까운 요소로 포커스를 이동합니다.

탐색 방향과 일치하는 경계 직사각형의 가장자리가 *확장* 및 *프로젝션*되어 후보 대상을 식별합니다. 발생하는 첫 번째 요소는 대상으로 식별됩니다. 후보가 여러 개인 경우 가장 가까운 요소가 대상으로 식별됩니다. 그래도 후보가 여러 개인 경우 맨 위/맨 왼쪽에 있는 요소가 후보로 식별됩니다.

![NavigationDirectionDistance 탐색 전략](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*NavigationDirectionDistance 탐색 전략*

### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 전략은 2D 일직선 거리를 기준으로 가장 가까이에 있는 요소에 포커스를 이동합니다([택시 기하 도형](https://en.wikipedia.org/wiki/Taxicab_geometry)).

각 잠재 후보와의 기본 거리 및 보조 거리를 더한 값으로 최상의 후보를 식별합니다. 거리가 같은 경우 요청된 방향이 위쪽 또는 아래쪽인 경우 왼쪽 첫 번째 요소가 선택되고, 요청된 방향이 왼쪽 또는 오른쪽이면 위쪽 첫 번째 요소가 선택됩니다.

![RectilinearDistance 탐색 전략](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*RectilinearDistance 탐색 전략*

다음은 B1에 포커스가 있고 요청된 방향이 아래쪽일 때 B3가 RectilinearDistance 포커스 후보가 되는 과정을 보여주는 이미지입니다. 이 예에서는 다음 계산을 토대로 진행됩니다.
-   거리(B1, B3, 아래로)는 10 + 0 = 10
-   거리(B1, B2, 아래로)는 0 + 40 = 30
-   거리(B1, D, 아래로)는 30 + 0 = 30


## <a name="related-articles"></a>관련 문서
- [프로그래밍 방식 포커스 탐색](focus-navigation-programmatic.md)
- [키보드 조작](keyboard-interactions.md)
- [키보드 접근성](../accessibility/keyboard-accessibility.md) 



