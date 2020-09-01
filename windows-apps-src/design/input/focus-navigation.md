---
title: 마우스 없이 포커스 탐색
Description: 포커스 탐색을 사용 하 여 Windows 앱에 대 한 포괄적이 고 일관적인 상호 작용 환경을 제공 하 고, 장애가 있는 사용자 및 기타 접근성 요구 사항이 있는 사용자 지정 컨트롤과 tv 화면 및 Xbox One의 10 피트 환경을 제공 하는 방법을 알아봅니다.
label: ''
template: detail.hbs
keywords: 키보드, 게임 컨트롤러, 원격 제어, 탐색, 방향 내부 탐색, 방향 영역, 탐색 전략, 입력, 사용자 조작, 접근성, 유용성
ms.date: 03/02/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4ad0e986de3f3084cd33f217df7715c955cb6b57
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172577"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>키보드, 게임 패드, 원격 제어 및 내게 필요한 옵션 도구에 대 한 포커스 탐색

![키보드, 원격 및 D-패드](images/dpad-remote/dpad-remote-keyboard.png)

포커스 탐색을 사용 하 여 Windows 앱에 대 한 포괄적이 고 일관적인 상호 작용 환경을 제공 하 고, 장애가 발생 한 키보드 및 기타 접근성 요구 사항이 있는 사용자 지정 컨트롤과 텔레비전 화면 및 Xbox One의 10 피트 환경을 제공 합니다.

## <a name="overview"></a>개요

포커스 탐색은 사용자가 키보드, 게임 패드 또는 원격 제어를 사용 하 여 Windows 응용 프로그램의 UI를 탐색 하 고 상호 작용할 수 있도록 하는 기본 메커니즘을 나타냅니다.

> [!NOTE]
> 입력 장치는 일반적으로 터치, 터치 패드, 펜, 마우스, 키보드, 게임 패드, 원격 제어 등의 비 포인팅 장치와 같은 포인팅 장치로 분류 됩니다.

이 항목에서는 Windows 응용 프로그램을 최적화 하 고 가리키지 않는 입력 형식을 사용 하는 사용자에 대 한 사용자 지정 상호 작용 환경을 빌드하는 방법에 대해 설명 합니다. 

Pc에서 Windows 앱의 사용자 지정 컨트롤에 대 한 키보드 입력에 초점을 맞춘 경우에도 터치 키보드 및 화상 키보드 (OSK)와 같은 소프트웨어 키보드, Windows 내레이터와 같은 내게 필요한 옵션 도구 지원, 10 피트 환경 지원 등의 소프트웨어 키보드에도 잘 디자인 된 키보드 환경이 중요 합니다.

포인팅 장치의 Windows 응용 프로그램에서 사용자 지정 환경을 빌드하는 방법에 대 한 지침은 [포인터 입력 처리](handle-pointer-input.md) 를 참조 하세요.

키보드의 앱 및 환경을 빌드하는 방법에 대 한 일반적인 내용은 [키보드 상호 작용](keyboard-interactions.md)을 참조 하세요.

## <a name="general-guidance"></a>일반 지침

사용자 조작이 필요한 UI 요소만 포커스 탐색을 지원 해야 합니다. 정적 이미지와 같이 작업을 필요로 하지 않는 요소는 키보드 포커스가 필요 하지 않습니다. 화면 판독기와 유사한 접근성 도구는 포커스 탐색에 포함 되지 않은 경우에도 이러한 정적 요소를 계속 발표 합니다. 

마우스 또는 터치와 같은 포인터 장치로 이동 하는 것과는 달리 포커스 탐색은 선형입니다. 포커스 탐색을 구현 하는 경우 사용자가 응용 프로그램과 상호 작용 하는 방식과 논리적 탐색을 고려해 야 합니다. 대부분의 경우 사용자 지정 포커스 탐색 동작은 사용자 문화권의 기본 읽기 패턴을 따르는 것이 좋습니다.

몇 가지 다른 포커스 탐색 고려 사항은 다음과 같습니다.

- 컨트롤은 논리적으로 그룹화 됩니까?
- 중요도가 더 높은 컨트롤 그룹이 있나요?
  - 그렇다면 해당 그룹에 하위 그룹이 포함 됩니까?
- 레이아웃에 사용자 지정 방향 탐색 (화살표 키) 및 탭 순서가 필요 한가요?

[내게 필요한 옵션 전자책 엔지니어링 소프트웨어](https://www.microsoft.com/download/details.aspx?id=19262) 에는 *논리적 계층 설계*에 대 한 훌륭한 장이 있습니다.

## <a name="2d-directional-navigation-for-keyboard"></a>키보드를 위한 2D 양방향 탐색

컨트롤 또는 컨트롤 그룹의 2D 내부 탐색 영역을 "방향성 영역" 이라고 합니다. 이 개체로 포커스가 이동 하면 키보드 화살표 키 (왼쪽, 오른쪽, 위쪽 및 아래쪽)를 사용 하 여 방향성 영역 내의 자식 요소 간을 탐색할 수 있습니다.

![](images/keyboard/directional-area-small.png)
*컨트롤 그룹의 방향 영역 2d 내부 탐색 영역 또는 방향 영역*

[XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) 속성 ( [자동](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [사용](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)또는 [사용 안 함](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)의 값이 있는 경우)을 사용 하 여 키보드 화살표 키로 2d 내부 탐색을 관리할 수 있습니다.

> [!NOTE]
> 탭 순서는이 속성의 영향을 받지 않습니다. 혼란 스러운 탐색 환경을 방지 하려면 응용 프로그램의 탭 탐색 순서에서 방향 영역의 자식 요소를 명시적으로 지정 *하지* 않는 것이 좋습니다. 요소에 대 한 탭 이동 동작에 대 한 자세한 내용은 [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 및 [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 참조 하세요.

### <a name="auto-default-behavior"></a>[Auto](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (기본 동작)

Auto로 설정 하면 요소의 상위 구조 또는 상속 계층 구조에 따라 방향 탐색 동작이 결정 됩니다. 모든 상위 항목이 기본 모드 ( **자동**으로 설정) 이면 키보드를 사용 하 여 방향 탐색이 지원 *되지 않습니다* .

### <a name="disabled"></a>[사용 안 함](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

**XYFocusKeyboardNavigation** 를 **Disabled** 로 설정 하 여 컨트롤 및 해당 자식 요소에 대 한 방향성 탐색을 차단 합니다.

![XYFocusKeyboardNavigation disabled 동작 ](images/keyboard/xyfocuskeyboardnav-disabled.gif)
 *XYFocusKeyboardNavigation 사용 안 함 동작*

이 예에서는 기본 [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary)의 **XYFocusKeyboardNavigation** 가 **Enabled**로 설정 되어 있습니다. 모든 자식 요소는이 설정을 상속 하며 화살표 키를 사용 하 여 탐색할 수 있습니다. 그러나 B3 및 B4 요소는 **XYFocusKeyboardNavigation** 가 **Disabled**로 설정 된 보조 [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary)에 있습니다 .이는 기본 컨테이너를 재정의 하 고 해당 자식 요소 간에 화살표 키 탐색을 사용 하지 않도록 설정 합니다.

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

### <a name="enabled"></a>[Enabled](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

**XYFocusKeyboardNavigation** 을 **사용** 으로 설정 하 여 컨트롤과 각 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 자식 개체에 대 한 2d 방향 탐색을 지원 합니다.

설정 하는 경우 화살표 키를 사용 하 여 탐색은 방향 영역 내의 요소로 제한 됩니다. 탭 순서 계층 구조를 통해 모든 컨트롤에 액세스할 수 있으므로 탭 탐색은 영향을 받지 않습니다.

![XYFocusKeyboardNavigation enabled behavior ](images/keyboard/xyfocuskeyboardnav-enabled.gif)
 *XYFocusKeyboardNavigation enabled 동작*

이 예에서는 기본 [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary)의 **XYFocusKeyboardNavigation** 가 **Enabled**로 설정 되어 있습니다. 모든 자식 요소는이 설정을 상속 하며 화살표 키를 사용 하 여 탐색할 수 있습니다. B3 및 B4 요소는 보조 [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary)에 있습니다. 여기서 **XYFocusKeyboardNavigation** 가 설정 되지 않은 경우 기본 컨테이너 설정이 상속 됩니다. B5 요소는 선언 된 방향성 영역 내에 있지 않으며 화살표 키 탐색을 지원 하지 않지만 표준 탭 탐색 동작을 지원 합니다.

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

여러 수준을 중첩 된 방향 영역으로 지정할 수 있습니다. 모든 부모 요소의 XYFocusKeyboardNavigation가 Enabled로 설정 된 경우 내부 탐색 영역 경계는 무시 됩니다.

다음은 2D 방향 탐색을 명시적으로 지원 하지 않는 요소 내에서 두 개의 중첩 된 방향 영역에 대 한 예입니다. 이 경우 두 중첩 영역 간에 방향 탐색이 지원 되지 않습니다.

![XYFocusKeyboardNavigation 사용 및 중첩 동작 ](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)
 *XYFocusKeyboardNavigation 사용 및 중첩 동작*

다음은 세 개의 중첩 된 방향 영역에 대 한 보다 복잡 한 예제입니다.

-   B1에 포커스가 있을 때 XYFocusKeyboardNavigation를 Disabled로 설정 하 고, B2, B3 및 B4에 화살표 키가 연결할 수 없는 방향 영역 경계가 있으므로 B5만 (또는 그 반대로) 탐색할 수 있습니다.
-   B2에 포커스가 있는 경우에는 방향 영역 경계에서 B1, B4 및 B5로의 화살표 키 탐색을 방지 하므로 B3만 탐색할 수 있습니다 (그 반대의 경우도 해당).
-   B4에 포커스가 있는 경우 Tab 키를 사용 하 여 컨트롤 사이를 이동 해야 합니다.

![XYFocusKeyboardNavigation 사용 및 복잡 한 중첩 동작](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*XYFocusKeyboardNavigation 사용 및 복잡 한 중첩 동작*

## <a name="tab-navigation"></a>탭 탐색

Witin 컨트롤 또는 컨트롤 그룹에 대 한 2D 방향 탐색에 화살표 키를 사용할 수 있지만, Tab 키를 사용 하 여 Windows 응용 프로그램의 모든 컨트롤을 탐색할 수 있습니다. 

모든 대화형 컨트롤은 기본적으로 Tab 키 탐색을 지원 합니다 ([IsEnabled](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) 및 [istabstop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성은 **true**임). 여기에는 응용 프로그램의 컨트롤 레이아웃에서 파생 된 논리적 탭 순서가 포함 됩니다. 그러나 기본 순서는 시각적 순서와 반드시 일치 하지는 않습니다. 실제 표시 위치는 부모 레이아웃 컨테이너 및 자식 요소에 대해 설정 하 여 레이아웃에 영향을 줄 수 있는 특정 속성에 따라 달라질 수 있습니다.

응용 프로그램에서 포커스를 이동할 수 있도록 하는 사용자 지정 탭 순서를 사용 하지 않습니다. 예를 들어 양식에 있는 컨트롤의 목록에는 로캘에 따라 위쪽에서 아래쪽으로 이동 하 고 왼쪽에서 오른쪽으로 흐르는 탭 순서가 있어야 합니다.

이 섹션에서는 앱에 맞게이 탭 순서를 완전히 사용자 지정 하는 방법을 설명 합니다.

### <a name="set-the-tab-navigation-behavior"></a>탭 탐색 동작 설정

[UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 의 [TabFocusNavigation](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 속성은 전체 개체 트리 또는 방향 영역에 대 한 탭 탐색 동작을 지정 합니다.

> [!NOTE]
> [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) 를 사용 하지 않는 개체의 모양을 정의 하는 데이 속성을 사용 [합니다.](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation)

이전 섹션에서 설명한 것 처럼 혼동 스러운 탐색 환경을 방지 하기 위해 응용 프로그램의 탭 탐색 순서에서 방향 영역의 자식 요소를 명시적으로 지정 *하지* 않는 것이 좋습니다. 요소에 대 한 탭 이동 동작에 대 한 자세한 내용은 [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 및 [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 참조 하세요.   
> Windows 10 크리에이터 업데이트 (build 10.0.15063) 보다 오래 된 버전의 경우 탭 설정은 [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) 개체로 제한 됩니다. 자세한 내용은 [TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation)을 참조 하세요.

[TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 에는 다음과 같은 가능한 값을 가진 [KeyboardNavigationMode](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) 형식의 값이 있습니다. 이러한 예제는 사용자 지정 컨트롤 그룹이 아니고 화살표 키를 사용 하 여 내부 탐색이 필요 하지 않습니다.

- **Local** (기본값)   
  탭 인덱스는 컨테이너 내부의 로컬 하위 트리에서 인식 됩니다. 이 예에서는 탭 순서가 B1, B2, B3, B4, B5, B6, B7, B1입니다.

   !["로컬" 탭 탐색 동작](images/keyboard/tabnav-local.gif)

   *"로컬" 탭 탐색 동작*

- **한 번**  
  컨테이너와 모든 자식 요소는 포커스를 한 번 받습니다. 이 예에서는 탭 순서가 B1, B2, B7, B1 (화살표 키를 사용 하는 내부 탐색도)입니다.

   !["한 번" 탭 탐색 동작](images/keyboard/tabnav-once.gif)

   *"한 번" 탭 탐색 동작*

- **순환이**   
  컨테이너 내에서 포커스를 받을 수 있는 초기 요소로 포커스를 되돌립니다. 이 예에서는 탭 순서가 B1, B2, B3, B4, B5, B6, B2 ...입니다.

   !["주기" 탭 탐색 동작](images/keyboard/tabnav-cycle.gif)

   *"주기" 탭 탐색 동작*

위의 예제에 대 한 코드는 다음과 같습니다 (TabFocusNavigation = "Cycle").

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

### <a name="tabindex"></a>[TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

[TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 를 사용 하 여 사용자가 Tab 키를 사용 하 여 컨트롤을 탐색할 때 요소가 포커스를 받는 순서를 지정 합니다. 하위 탭 인덱스를 사용 하 여 컨트롤을 더 높은 인덱스를 사용 하 여 컨트롤을 하기 전에 포커스를 받습니다.

컨트롤에 [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 가 지정 되지 않은 경우 범위에 따라 시각적 트리의 모든 대화형 컨트롤에 대 한 현재 가장 높은 인덱스 값 (가장 낮은 우선 순위) 보다 높은 인덱스 값이 할당 됩니다. 

컨트롤의 모든 자식 요소는 범위로 간주 되며, 이러한 요소 중 하나에도 자식 요소가 있으면 다른 범위로 간주 됩니다. 범위의 시각적 트리에서 첫 번째 요소를 선택 하 여 모호성을 해결 합니다. 

탭 순서에서 컨트롤을 제외 하려면 [Istabstop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 속성을 **false**로 설정 합니다.

[TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성을 설정 하 여 기본 탭 순서를 재정의 합니다.

> [!NOTE] 
> [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 는 [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 및 [컨트롤 탐색](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation)을 모두 사용 하 여 동일한 방식으로 작동 합니다.


여기서는 특정 요소에 대 한 [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 속성의 포커스 탐색의 영향을 받는 방법을 보여 줍니다. 

![TabIndex 동작을 사용한 "로컬" 탭 탐색](images/keyboard/tabnav-tabindex.gif)

*TabIndex 동작을 사용한 "로컬" 탭 탐색*

앞의 예제에는 두 가지 범위가 있습니다. 
- B1, 방향성 area (B2-B6) 및 B7
- 방향성 영역 (B2-B6)

B3 (방향 영역)에 포커스가 있는 경우 범위를 변경 하 고 탭 탐색을 방향 영역으로 전송 합니다 .이 영역에는 이후 포커스의 가장 적합 한 후보가 식별 됩니다. 이 경우 B2와 B4, B5 및 B6이 차례로 나옵니다. 그런 다음 범위를 다시 변경 하 고 B1로 포커스를 이동 합니다.

이 예제에 대 한 코드는 다음과 같습니다.

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

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>키보드, 게임 패드 및 원격 제어를 위한 2D 방향 탐색

키보드, 게임 패드, 원격 제어, Windows 내레이터와 같은 내게 필요한 옵션 도구 등 포인터가 아닌 입력 형식은 Windows 응용 프로그램의 UI를 탐색 하 고 상호 작용 하는 데 일반적으로 사용 되는 기본 메커니즘을 공유 합니다.

이 섹션에서는 기본 탐색 전략을 지정 하 고 포인터를 사용 하지 않는 모든 포커스 기반 입력 유형을 지 원하는 탐색 전략 속성 집합을 통해 응용 프로그램 내에서 포커스 탐색을 미세 조정 하는 방법을 다룹니다.

Xbox/TV에 대 한 앱 및 환경을 빌드하는 방법에 대 한 일반적인 내용은 [키보드 조작](keyboard-interactions.md), [xbox 및 Tv 디자인](../devices/designing-for-tv.md), [게임 패드 및 원격 제어 상호 작용](gamepad-and-remote-interactions.md)을 참조 하세요.

### <a name="navigation-strategies"></a>탐색 전략

> 탐색 전략은 키보드, 게임 패드, 원격 제어 및 다양 한 내게 필요한 옵션 도구에 적용할 수 있습니다.

다음 탐색 전략 속성을 사용 하면 화살표 키, 방향 패드 (D-패드) 단추 또는 이와 유사한 눌린 컨트롤에 따라 포커스를 받는 컨트롤에 영향을 줄 수 있습니다. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

이러한 속성은 [Auto](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (기본값), [NavigationDirectionDistance](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [프로젝션](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)또는 [RectilinearDistance ](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)값을 사용할 수 있습니다.

**Auto**로 설정 된 경우 요소의 동작은 요소의 상위 항목을 기반으로 합니다. 모든 요소를 **Auto**로 설정 하면 **프로젝션이** 사용 됩니다.

> [!NOTE]
> 이전에 초점을 맞춘 요소나 탐색 방향의 축과 같은 다른 요소는 결과에 영향을 줄 수 있습니다.

### <a name="projection"></a>프로젝션

프로젝션 전략은 현재 포커스가 있는 요소의 가장자리가 탐색 방향으로 *프로젝션* 될 때 발생 하는 첫 번째 요소로 포커스를 이동 합니다.

이 예제에서 각 포커스 탐색 방향은 프로젝션으로 설정 됩니다. B3이 B1에서 B4로 어떻게 이동 하는지 확인 합니다. 이는 B3이 프로젝션 영역에 있지 않기 때문입니다. 또한 B1에서 왼쪽으로 이동할 때 포커스 후보가 식별 되지 않는 방법도 확인할 수 있습니다. B1을 기준으로 B2의 위치에서 B3이 후보로 제거 되기 때문입니다. B3이 B2와 동일한 행에 있는 경우 왼쪽 탐색에 사용할 수 있는 후보가 될 수 있습니다. B2는 탐색 방향의 축에 대 한 근접성 때문에 실행 가능한 후보입니다.

![프로젝션 탐색 전략](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*프로젝션 탐색 전략*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 전략은 탐색 방향의 축에 가장 가까운 요소로 포커스를 이동 합니다.

탐색 방향에 해당 하는 경계 사각형의 가장자리가 *확장* 되 고 *투영* 되어 후보 대상을 식별 합니다. 발견 된 첫 번째 요소는 대상으로 식별 됩니다. 여러 후보의 경우 가장 가까운 요소가 대상으로 식별 됩니다. 아직 여러 후보가 있는 경우 맨 위/가장 왼쪽에 있는 요소가 후보로 식별 됩니다.

![NavigationDirectionDistance 탐색 전략](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*NavigationDirectionDistance 탐색 전략*

### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 전략은 2D 직각선 거리 ([택시 geometry](https://en.wikipedia.org/wiki/Taxicab_geometry))에 따라 가장 가까운 요소로 포커스를 이동 합니다.

가장 적합 한 candidtate를 식별 하는 데 사용 되는 기본 거리의 합계와 각 잠재적 후보에 대 한 보조 거리가 사용 됩니다. 연결에서 왼쪽의 첫 번째 요소는 요청 된 방향이 up 또는 down 인 경우 선택 되 고, 맨 왼쪽에 있는 첫 번째 요소는 요청 된 방향이 왼쪽 또는 오른쪽 인 경우 선택 됩니다.

![RectilinearDistance 탐색 전략](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*RectilinearDistance 탐색 전략*

이 이미지는 B1에 포커스가 있고 down이 요청 된 방향이 면 B3이 RectilinearDistance 포커스 후보가 되는 방법을 보여 줍니다. 이 예에서는 다음 calcualations을 기반으로 합니다.
-   거리 (B1, B3, Down)는 10 + 0 = 10입니다.
-   거리 (B1, B2, Down)는 0 + 40 = 30입니다.
-   거리 (B1, D, Down)는 30 + 0 = 30입니다.


## <a name="related-articles"></a>관련된 문서
- [프로그래밍 방식 포커스 탐색](focus-navigation-programmatic.md)
- [키보드 조작](keyboard-interactions.md)
- [키보드 접근성](../accessibility/keyboard-accessibility.md)