---
Description: NavigationView는 앱에 대한 최상위 탐색 패턴을 구현하는 적응형 컨트롤입니다.
title: 탐색 보기
template: detail.hbs
ms.date: 05/02/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2ac2e95e4233a490187066e0d19f1eeb4f330265
ms.sourcegitcommit: 3b8fac693c0b031def5cedc8ae3632a2aa00f1f6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84467753"
---
# <a name="navigation-view"></a>탐색 보기

NavigationView 컨트롤은 앱에 대한 최상위 탐색을 제공합니다. 이 컨트롤은 다양한 화면 크기에 맞게 조정되고 ‘위쪽’ 및 ‘왼쪽’ 탐색 스타일을 둘 다 지원합니다. 

![위쪽 탐색](images/nav-view-header.png)<br/>
탐색 보기는 위쪽 및 왼쪽 탐색 창 또는 메뉴를 둘 다 지원함

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **NavigationView** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되어 있습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](/uwp/toolkits/winui/)를 참조하세요. |

> **플랫폼 API**: [Windows.UI.Xaml.Controls.NavigationView 클래스](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 라이브러리 API**: [Microsoft.UI.Xaml.Controls.NavigationView 클래스](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> _위쪽_ 및 _계층적_ 탐색과 같은 NavigationView의 몇 가지 기능을 사용하려면 Windows 10 버전 1809([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상이나 [Windows UI 라이브러리](/uwp/toolkits/winui/)가 필요합니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

NavigationView는 다음 작업에 적합한 적응형 탐색 컨트롤입니다.

- 앱 전체에 일관성 있는 탐색 환경 제공
- 더 작은 창에서 화면 공간 유지
- 여러 탐색 범주에 대한 액세스 구성

다른 탐색 패턴의 경우 [탐색 디자인 기본 사항](../basics/navigation-basics.md)을 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon-sm.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/NavigationView">앱을 열고 작동 중인 NavigationView를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>디스플레이 모드

> PaneDisplayMode 속성을 사용하려면 Windows 10 버전 1809([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상 또는 [Windows UI 라이브러리](/uwp/toolkits/winui/)가 필요합니다.

PaneDisplayMode 속성을 사용하여 NavigationView에 대한 다른 탐색 스타일 또는 디스플레이 모드를 구성할 수 있습니다.

:::row:::
    :::column:::
    ### <a name="top"></a>맨 위로
    창이 콘텐츠 위에 배치됩니다.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![위쪽 탐색의 예](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

다음과 같은 경우 ‘위쪽’ 탐색을 권장합니다.

- 중요성이 같은 최상위 탐색 범주가 5개 이하이며 드롭다운 오버플로 메뉴에서 끝나는 추가적인 최상위 탐색 범주가 덜 중요한 것으로 간주되는 경우
- 화면에 모든 탐색 옵션을 표시해야 하는 경우
- 앱 콘텐츠에 더 많은 공간이 필요한 경우
- 아이콘으로 앱의 탐색 범주를 명확하게 설명할 수 없는 경우

:::row:::
    :::column:::
    ### <a name="left"></a>왼쪽
    창이 확장되고 콘텐츠 왼쪽에 배치됩니다.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![확장된 왼쪽 탐색 창의 예](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

다음과 같은 경우 ‘왼쪽’ 탐색을 권장합니다.

- 중요성이 같은 최상의 탐색 범주가 5-10개인 경우
- 다른 앱 콘텐츠의 공간을 줄여 탐색 범주를 눈에 띄게 하려는 경우

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    창은 열릴 때까지 아이콘만 표시하며 콘텐츠의 왼쪽에 배치됩니다.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![컴팩트 왼쪽 탐색 창의 예](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    창이 열릴 때까지 메뉴 단추만 표시됩니다. 열린 창은 콘텐츠 왼쪽에 배치됩니다.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![최소 왼쪽 탐색 창의 예](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>자동

기본적으로 PaneDisplayMode는 자동으로 설정됩니다. 자동 모드에서 탐색 보기는 창이 좁을 때 LeftMinimal이고, 창이 넓어지면서 LeftCompact로 조정된 후 Left로 조정됩니다. 자세한 내용은 [적응형 동작](#adaptive-behavior) 섹션을 참조하세요.

![왼쪽 탐색 기본 적응형 동작](images/displaymode-auto.png)<br/>
탐색 보기 기본 적응형 동작

## <a name="anatomy"></a>구조

이 이미지에서는 ‘위쪽’ 또는 ‘왼쪽’ 탐색용으로 구성되는 경우 컨트롤의 창, 헤더 및 콘텐츠 영역에 대한 레이아웃을 보여 줍니다. 

![위쪽 탐색 보기 레이아웃](images/topnav-anatomy.png)<br/>
위쪽 탐색 레이아웃

![왼쪽 탐색 보기 레이아웃](images/leftnav-anatomy.png)<br/>
왼쪽 탐색 레이아웃

### <a name="pane"></a>창

PaneDisplayMode 속성을 사용하여 창을 콘텐츠 위에 배치하거나 콘텐츠 왼쪽에 배치할 수 있습니다.

NavigationView 창은 다음을 포함할 수 있습니다.

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) 개체. 특정 페이지로 이동하기 위한 탐색 항목입니다.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) 개체. 탐색 항목을 그룹화하기 위한 구분 기호입니다. 구분 기호를 공백으로 렌더링하려면 [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) 속성을 0으로 설정합니다.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 개체. 항목 그룹에 레이블을 지정하기 위한 헤더입니다.
- 앱 수준 검색을 허용하기 위한 선택적 [AutoSuggestBox](auto-suggest-box.md) 컨트롤. [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) 속성에 컨트롤을 할당합니다.
- [앱 설정](../app-settings/app-settings-and-data.md)의 선택적 진입점. 설정 항목을 숨기려면 [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 속성을 **false**로 설정합니다.

왼쪽 창에는 다음도 포함됩니다.

- 열린 창과 닫힌 창을 전환하는 메뉴 단추. 더 큰 앱 창에서 창이 열리면 [IsPaneToggleButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 속성을 사용하여 이 단추를 숨기도록 선택할 수 있습니다.

탐색 보기의 뒤로 단추는 창의 왼쪽 위 왼쪽 모서리에 배치됩니다. 그러나 탐색 보기는 뒤로 탐색을 자동으로 처리하지 않으며 뒤로 스택에 콘텐츠를 추가합니다. 뒤로 탐색을 사용하려면 [뒤로 탐색](#backwards-navigation) 섹션을 참조하세요.

다음은 위쪽 및 왼쪽 창 위치에 대한 자세한 창 구조입니다.

#### <a name="top-navigation-pane"></a>위쪽 탐색 창

![탐색 보기 위쪽 창 구조](images/navview-pane-anatomy-horizontal.png)

1. 헤더
1. 탐색 항목
1. 구분 기호
1. AutoSuggestBox(선택 사항)
1. 설정 단추(선택 사항)

#### <a name="left-navigation-pane"></a>왼쪽 탐색 창

![탐색 보기 왼쪽 창 구조](images/navview-pane-anatomy-vertical.png)

1. 메뉴 버튼
1. 탐색 항목
1. 구분 기호
1. 헤더
1. AutoSuggestBox(선택 사항)
1. 설정 단추(선택 사항)

#### <a name="pane-footer"></a>창 바닥글

자유 형식 콘텐츠를 [PaneFooter](/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 속성에 추가하여 창의 바닥글에 배치할 수 있습니다.

:::row:::
    :::column:::
    ![창 바닥글 위쪽 탐색](images/navview-freeform-footer-top.png)<br>
     위쪽 창 바닥글<br>
    :::column-end:::
    :::column:::
    ![창 바닥글 왼쪽 탐색](images/navview-freeform-footer-left.png)<br>
    왼쪽 창 바닥글<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>창 제목 및 헤더

[PaneTitle](/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 속성을 설정하여 창 헤더 영역에 텍스트 콘텐츠를 배치할 수 있습니다. 문자열을 사용하고 메뉴 단추 옆에 있는 텍스트를 표시합니다.

이미지나 로고와 같은 텍스트가 아닌 콘텐츠를 추가하려면 해당 요소를 [PaneHeader](/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) 속성에 추가하여 창 헤더에 요소를 배치할 수 있습니다.

PaneTitle 및 PaneHeader를 둘 다 설정하면 콘텐츠는 메뉴 단추 옆에 가로로 누적되며 PaneTitle이 메뉴 단추에 가장 가깝습니다.

:::row:::
    :::column:::
    ![창 헤더 위쪽 탐색](images/navview-freeform-header-top.png)<br>
     위쪽 창 헤더<br>
    :::column-end:::
    :::column:::
    ![창 헤더 왼쪽 탐색](images/navview-freeform-header-left.png)<br>
    왼쪽 창 헤더<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>창 콘텐츠

자유 형식 콘텐츠를 [PaneCustomContent](/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) 속성에 추가하여 창에 배치할 수 있습니다.

:::row:::
    :::column:::
    ![창 사용자 지정 콘텐츠 위쪽 탐색](images/navview-freeform-pane-top.png)<br>
     위쪽 창 사용자 지정 콘텐츠<br>
    :::column-end:::
    :::column:::
    ![창 사용자 지정 콘텐츠 왼쪽 탐색](images/navview-freeform-pane-left.png)<br>
    왼쪽 창 사용자 지정 콘텐츠<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>헤더

[Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) 속성을 설정하여 페이지 제목을 추가할 수 있습니다.

![탐색 보기 헤더 영역의 예](images/nav-header.png)<br/>
탐색 보기 헤더

헤더 영역은 왼쪽 창 위치에서는 탐색 단추에 세로로 맞춰지며 위쪽 창에서는 창 아래에 배치됩니다. 헤더 영역에는 고정 높이 52px이 사용됩니다. 선택한 탐색 범주의 페이지 제목을 유지하는 것이 목적입니다. 헤더가 페이지 위쪽에 고정되고 콘텐츠 영역에 대한 스크롤 자르기 지점 역할을 합니다.

NavigationView가 최소 디스플레이 모드이면 항상 헤더가 표시됩니다. 더 큰 창 너비에 사용되는 다른 모드에서는 헤더를 숨기도록 선택할 수 있습니다. 헤더를 숨기려면 [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 속성을 **false**로 설정합니다.

### <a name="content"></a>콘텐츠

![탐색 보기 콘텐츠 영역의 예](images/nav-content.png)<br/>
탐색 보기 콘텐츠

콘텐츠 영역은 선택한 탐색 범주에 대한 대부분의 정보가 표시되는 곳입니다.

NavigationView가 **최소** 모드이고 여백이 24px일 경우 콘텐츠 영역에 대해 12px 여백이 적당합니다.

## <a name="adaptive-behavior"></a>적응형 동작

기본적으로 사용 가능한 화면 공간을 기준으로 탐색 보기가 자동으로 디스플레이 모드를 변경합니다. [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 및 [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) 속성은 디스플레이 모드가 변경되는 중단점을 지정합니다. 이 값을 수정하여 적응형 디스플레이 모드 동작을 사용자 지정할 수 있습니다.

### <a name="default"></a>Default

PaneDisplayMode가 기본값인 **자동**으로 설정되면 적응형 동작은 다음을 표시합니다.

- 큰 창 너비(1008px 이상)의 확장된 왼쪽 창.
- 중간 창 너비(641px-1007px)의 왼쪽, 아이콘 전용, 탐색 창(LeftCompact).
- 작은 창 너비(640px 이하)의 메뉴 단추만(LeftMinimal).

적응형 동작 창 크기에 대한 자세한 내용은 [화면 크기 및 중단점](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)을 참조하세요.

![왼쪽 탐색 기본 적응형 동작](images/displaymode-auto.png)<br/>
탐색 보기 기본 적응형 동작

### <a name="minimal"></a>최소

두 번째 일반적인 적응형 패턴은 큰 창 너비에서는 확장된 왼쪽 창을 사용하고 중간 및 작은 창 너비에서는 메뉴 단추만 사용하는 것입니다.

다음과 같은 경우에 권장됩니다.

- 더 작은 창 너비에서 앱 콘텐츠에 더 많은 공간을 원하는 경우
- 아이콘으로 탐색 범주를 명확하게 표시할 수 없는 경우

![왼쪽 탐색 최소 적응형 동작](images/adaptive-behavior-minimal.png)<br/>
탐색 보기 “최소” 적응형 동작

이 동작을 구성하려면 CompactModeThresholdWidth를 창을 축소할 너비로 설정합니다. 여기서는 기본값 640부터 1007까지 변경됩니다. 또한 값이 충돌하지 않도록 ExpandedModeThresholdWidth를 설정해야 합니다.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>컴팩트

세 번째 일반적인 적응형 패턴은 큰 창 너비에서는 확장된 왼쪽 창을 사용하고 중간 및 작은 창 너비에서는 LeftCompact, 아이콘 전용, 탐색 창을 사용하는 것입니다.

다음과 같은 경우에 권장됩니다.

- 항상 화면에 모든 탐색 옵션을 표시하는 것이 중요합니다.
- 아이콘으로 탐색 범주를 명확하게 표시할 수 있습니다.

![왼쪽 탐색 컴팩트 적응형 동작](images/adaptive-behavior-compact.png)<br/>
탐색 보기 “컴팩트” 적응형 동작

이 동작을 구성하려면 CompactModeThresholdWidth를 0으로 설정합니다.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>적응형 동작 없음

자동 적응형 동작을 사용하지 않으려면 PaneDisplayMode를 자동 이외의 값으로 설정합니다. 여기서는 LeftMinimal로 설정되므로 창 너비에 관계없이 메뉴 단추만 표시됩니다.

![왼쪽 탐색 적응형 동작 없음](images/adaptive-behavior-none.png)<br/>
PaneDisplayMode가 LeftMinimal로 설정된 탐색 보기

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

앞서 ‘디스플레이 모드’ 섹션에서 설명한 대로 창이 항상 위쪽에 있거나, 항상 확장되거나, 항상 컴팩트하거나, 항상 최소화되도록 설정할 수 있습니다. 앱 코드에서 직접 디스플레이 모드를 관리할 수도 있습니다. 이 예제는 다음 섹션에 나와 있습니다.

### <a name="top-to-left-navigation"></a>위쪽에서 왼쪽으로 탐색

앱에서 위쪽 탐색을 사용하면 창 너비가 감소하면서 탐색 항목이 오버플로 메뉴로 축소됩니다. 앱 창이 좁은 경우에는 모든 항목이 오버플로 메뉴로 축소되게 하지 않고 위쪽에서 LeftMinimal 탐색으로 PaneDisplayMode를 전환하도록 향상된 사용자 환경을 제공할 수 있습니다.

다음과 같은 경우에는 큰 창 크기에서 위쪽 탐색을 사용하고 작은 창 크기에서 왼쪽 탐색을 사용하는 것이 좋습니다.

- 중요성이 같은 최상위 탐색 범주 세트를 함께 표시하려는 경우, 이 세트의 한 범주가 화면에 맞춰지지 않으면 왼쪽 탐색으로 축소하여 같은 중요성을 부여합니다.
- 작은 창 크기에서 가능한 한 많은 콘텐츠 공간을 유지하려고 합니다.

이 예제에서는 [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) 및 [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 속성을 사용하여 Top 및 LeftMinimal 탐색 사이에서 전환하는 방법을 보여 줍니다.

![위쪽 또는 왼쪽 적응형 동작의 예 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> AdaptiveTrigger.MinWindowWidth를 사용하는 경우 창이 지정된 최소 너비보다 넓으면 시각적 상태가 트리거됩니다. 이는 기본 XAML은 좁은 창을 정의하며 VisualState는 창이 넓어질 때 적용되는 수정 사항을 정의함을 의미합니다. 탐색 보기의 기본 PaneDisplayMode는 자동이므로, 창 너비가 CompactModeThresholdWidth보다 작거나 같은 경우에는 LeftMinimal 탐색이 사용됩니다. 창이 넓어지면 VisualState는 기본값을 재정의하며 Top 탐색이 사용됩니다.

## <a name="navigation"></a>탐색

탐색 보기는 탐색 작업을 자동으로 수행하지 않습니다. 사용자가 탐색 항목을 탭하면 탐색 보기는 해당 항목을 선택된 상태로 표시하고 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 이벤트를 발생시킵니다. 탭하면 새 항목이 선택되는 경우 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 이벤트도 발생합니다.

요청된 탐색과 관련된 작업을 수행하도록 한쪽 이벤트를 처리할 수 있습니다. 처리해야 하는 이벤트는 앱에서 필요한 동작에 따라 달라집니다. 일반적으로 요청된 페이지로 이동하여 해당 이벤트에 대한 응답으로 탐색 보기 헤더를 업데이트합니다.

**ItemInvoked**는 이미 선택된 경우에도 사용자가 탐색 항목을 탭하면 항상 발생합니다. 마우스, 키보드 또는 기타 입력을 사용하여 해당 작업으로 항목을 호출할 수도 있습니다. 자세한 내용은 [입력 및 상호 작용](../input/index.md)을 참조하세요. ItemInvoked 처리기에서 탐색하는 경우 기본적으로 페이지가 다시 로드되고 중복 항목이 탐색 스택에 추가됩니다. 항목이 호출될 때 탐색하는 경우 페이지 다시 로드를 허용하지 않거나, 페이지가 다시 로드될 때 탐색 백스택에서 중복 입력이 생성되지 않는지 확인해야 합니다. 코드 예제를 참조하세요.

**SelectionChanged**는 사용자가 현재 선택되지 않은 항목을 호출하거나 선택된 항목을 프로그래밍 방식으로 변경하여 발생할 수 있습니다. 사용자가 항목을 호출하여 선택이 변경되는 경우에는 ItemInvoked 이벤트가 먼저 발생합니다. 선택이 프로그래밍 방식으로 변경되면 ItemInvoked가 발생하지 않습니다.

### <a name="backwards-navigation"></a>뒤로 탐색

NavigationView에는 기본 제공 뒤로 단추가 있지만, 앞으로 탐색처럼 뒤로 탐색이 자동으로 수행되는지 않습니다. 사용자가 뒤로 단추를 탭하면 [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 이벤트가 발생합니다. 뒤로 탐색을 수행하도록 이 이벤트를 처리합니다. 자세한 내용 및 코드 예제는 [탐색 기록 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조하세요.

최소 또는 컴팩트 모드에서는 탐색 보기 창이 플라이아웃으로 열립니다. 이 경우 뒤로 단추를 클릭하면 창이 닫히고 대신 **PaneClosing** 이벤트가 발생합니다.

다음 속성을 설정하여 뒤로 단추를 숨기거나 사용하지 않을 수 있습니다.

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): 뒤로 단추를 표시하고 숨기는 데 사용합니다. 이 속성은 [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) 열거형 값을 사용하며 기본적으로 **자동**으로 설정됩니다. 단추가 축소되면 레이아웃에서 단추 공간이 예약되지 않습니다.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): 뒤로 단추를 사용하거나 사용하지 않도록 설정하는 데 사용합니다. 이 속성을 탐색 프레임의 [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) 속성에 데이터 바인딩할 수 있습니다. **IsBackEnabled**가 **false**이면 **BackRequested**가 발생하지 않습니다.

:::row:::
    :::column:::
        ![왼쪽 탐색 창의 탐색 보기 뒤로 단추](images/leftnav-back.png)<br/>
        _왼쪽 탐색 창의 뒤로 단추_
    :::column-end:::
    :::column:::
        ![위쪽 탐색 창의 탐색 보기 뒤로 단추](images/topnav-back.png)<br/>
        _위쪽 탐색 창의 뒤로 단추_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>코드 예제

> [!IMPORTANT]
> Windows UI(WinUI) 라이브러리 도구 키트를 사용하는 프로젝트의 경우 동일한 예비 설정 단계를 수행합니다. 자세한 배경, 설정 및 지원 정보는 [Windows UI 라이브러리 시작](/uwp/toolkits/winui/getting-started)을 참조하세요.

이 예제는 **NavigationView**를 큰 창 크기의 위쪽 탐색 창과 작은 창 크기의 왼쪽 탐색 창과 함께 사용하는 방법을 보여줍니다. **VisualStateManager**에서 *위쪽* 탐색 설정을 제거하여 왼쪽 전용 탐색에 맞게 조정할 수 있습니다.

예제에서는 다양한 일반적인 시나리오에 적합한 탐색 데이터를 설정하는 권장 방법을 보여 줍니다. 또한 **NavigationView**의 뒤로 단추 및 키보드 탐색을 사용하여 뒤로 탐색을 구현하는 방법을 보여 줍니다.

이 코드에서는 앱에 포함된 이동할 페이지가 이름으로 *HomePage*, *AppsPage*, *GamesPage*, *MusicPage*, *MyContentPage* 및 *SettingsPage*를 사용한다고 가정합니다. 이와 같은 페이지에 대한 코드는 표시되지 않습니다.

> [!IMPORTANT]
> 앱 페이지 정보는 [ValueTuple](/dotnet/api/system.valuetuple)에 저장됩니다. 이 구조체를 사용하려면 앱 프로젝트의 최소 버전이 SDK 17763 이상이어야 합니다. NavigationView의 WinUI 버전을 사용하여 초기 버전의 Windows 10을 대상으로 하는 경우 [System.ValueTuple NuGet 패키지](https://www.nuget.org/packages/System.ValueTuple/)를 대신 사용합니다.

> [!IMPORTANT]
> 이 코드는 NavigationView의 [Windows UI 라이브러리](/uwp/toolkits/winui/) 버전을 사용하는 방법을 보여 줍니다. NavigationView의 플랫폼 버전을 사용하는 경우에는 앱 프로젝트의 최소 버전이 SDK 17763 이상이어야 합니다. 플랫폼 버전을 사용하려면 `muxc:`에 대한 모든 참조를 제거합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavViewCompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> 이 코드는 NavigationView의 [Windows UI 라이브러리](/uwp/toolkits/winui/) 버전을 사용하는 방법을 보여 줍니다. NavigationView의 플랫폼 버전을 사용하는 경우에는 앱 프로젝트의 최소 버전이 SDK 17763 이상이어야 합니다. 플랫폼 버전을 사용하려면 `muxc`에 대한 모든 참조를 제거합니다.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private double NavViewCompactModeThresholdWidth { get { return NavView.CompactModeThresholdWidth; } }

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new Windows.UI.Xaml.Media.Animation.EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = Windows.System.VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = Windows.System.VirtualKey.Left,
        Modifiers = Windows.System.VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, Windows.UI.Xaml.Media.Animation.NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

> [!NOTE]
> 이 코드 예제의 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) 버전의 경우 **Blank App(C++/WinRT)** 프로젝트 템플릿을 기반으로 새 프로젝트를 만든 다음, 목록의 코드를 표시된 소스 코드 파일에 추가합니다. 목록에 표시된 그대로 소스 코드를 사용하려면 새 프로젝트의 이름을 *NavigationViewCppWinRT*로 지정합니다.

```cppwinrt
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    ...
    Double NavViewCompactModeThresholdWidth{ get; };
}

// pch.h
...
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Media.Animation.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace wuxc
{
    using namespace winrt::Windows::UI::Xaml::Controls;
};

namespace winrt::NavigationViewCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage()
        {
            InitializeComponent();
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"home", winrt::xaml_typename<NavigationViewCppWinRT::HomePage>()));
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"apps", winrt::xaml_typename<NavigationViewCppWinRT::AppsPage>()));
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"games", winrt::xaml_typename<NavigationViewCppWinRT::GamesPage>()));
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"music", winrt::xaml_typename<NavigationViewCppWinRT::MusicPage>()));
        }

        double MainPage::NavViewCompactModeThresholdWidth()
        {
            return NavView().CompactModeThresholdWidth();
        }

        void ContentFrame_NavigationFailed(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args)
        {
            throw winrt::hresult_error(E_FAIL, winrt::hstring(L"Failed to load Page ") + args.SourcePageType().Name);
        }

        // List of ValueTuple holding the Navigation Tag and the relative Navigation Page
        std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;

        void NavView_Loaded(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
        {
            // You can also add items in code.
            NavView().MenuItems().Append(muxc::NavigationViewItemSeparator());
            muxc::NavigationViewItem navigationViewItem;
            navigationViewItem.Content(winrt::box_value(L"My content"));
            navigationViewItem.Icon(wuxc::SymbolIcon(static_cast<wuxc::Symbol>(0xF1AD)));
            navigationViewItem.Tag(winrt::box_value(L"content"));
            NavView().MenuItems().Append(navigationViewItem);
            m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(L"content", winrt::xaml_typename<NavigationViewCppWinRT::MyContentPage>()));

            // Add handler for ContentFrame navigation.
            ContentFrame().Navigated({ this, &MainPage::On_Navigated });

            // NavView doesn't load any page by default, so load home page.
            NavView().SelectedItem(NavView().MenuItems().GetAt(0));
            // If navigation occurs on SelectionChanged, this isn't needed.
            // Because we use ItemInvoked to navigate, we need to call Navigate
            // here to load the home page.
            NavView_Navigate(L"home", Windows::UI::Xaml::Media::Animation::EntranceNavigationTransitionInfo());

            // Add keyboard accelerators for backwards navigation.
            Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
            goBack.Key(Windows::System::VirtualKey::GoBack);
            goBack.Invoked({ this, &MainPage::BackInvoked });
            KeyboardAccelerators().Append(goBack);

            // ALT routes here
            Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
            goBack.Key(Windows::System::VirtualKey::Left);
            goBack.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
            goBack.Invoked({ this, &MainPage::BackInvoked });
            KeyboardAccelerators().Append(altLeft);
        }

        void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const& /* sender */, muxc::NavigationViewItemInvokedEventArgs const& args)
        {
            if (args.IsSettingsInvoked())
            {
                NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
            }
            else if (args.InvokedItemContainer())
            {
                NavView_Navigate(winrt::unbox_value_or<winrt::hstring>(args.InvokedItemContainer().Tag(), L"").c_str(), args.RecommendedNavigationTransitionInfo());
            }
        }

        // NavView_SelectionChanged is not used in this example, but is shown for completeness.
        // You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
        // but not both.
        void NavView_SelectionChanged(muxc::NavigationView const& /* sender */, muxc::NavigationViewSelectionChangedEventArgs const& args)
        {
            if (args.IsSettingsSelected())
            {
                NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
            }
            else if (args.SelectedItemContainer())
            {
                NavView_Navigate(winrt::unbox_value_or<winrt::hstring>(args.SelectedItemContainer().Tag(), L"").c_str(), args.RecommendedNavigationTransitionInfo());
            }
        }

        void NavView_Navigate(std::wstring navItemTag, Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo)
        {
            Windows::UI::Xaml::Interop::TypeName pageTypeName;
            if (navItemTag == L"settings")
            {
                pageTypeName = winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>();
            }
            else
            {
                for (auto&& eachPage : m_pages)
                {
                    if (eachPage.first == navItemTag)
                    {
                        pageTypeName = eachPage.second;
                        break;
                    }
                }
            }
            // Get the page type before navigation so you can prevent duplicate
            // entries in the backstack.
            Windows::UI::Xaml::Interop::TypeName preNavPageType = ContentFrame().CurrentSourcePageType();

            // Navigate only if the selected page isn't currently loaded.
            if (pageTypeName.Name != L"" && preNavPageType.Name != pageTypeName.Name)
            {
                ContentFrame().Navigate(pageTypeName, nullptr, transitionInfo);
            }
        }

        void NavView_BackRequested(muxc::NavigationView const& /* sender */, muxc::NavigationViewBackRequestedEventArgs const& /* args */)
        {
            On_BackRequested();
        }

        void BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */, Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
        {
            On_BackRequested();
            args.Handled(true);
        }

        bool On_BackRequested()
        {
            if (!ContentFrame().CanGoBack())
                return false;

            // Don't go back if the nav pane is overlaid.
            if (NavView().IsPaneOpen() &&
                (NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Compact ||
                    NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Minimal))
                return false;

            ContentFrame().GoBack();
            return true;
        }

        void On_Navigated(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::Navigation::NavigationEventArgs const& args)
        {
            NavView().IsBackEnabled(ContentFrame().CanGoBack());

            if (ContentFrame().SourcePageType().Name == winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>().Name)
            {
                // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
                NavView().SelectedItem(NavView().SettingsItem().as<muxc::NavigationViewItem>());
                NavView().Header(winrt::box_value(L"Settings"));
            }
            else if (ContentFrame().SourcePageType().Name != L"")
            {
                for (auto&& eachPage : m_pages)
                {
                    if (eachPage.second.Name == args.SourcePageType().Name)
                    {
                        for (auto&& eachMenuItem : NavView().MenuItems())
                        {
                            auto navigationViewItem = eachMenuItem.try_as<muxc::NavigationViewItem>();
                            {
                                if (navigationViewItem)
                                {
                                    winrt::hstring hstringValue = winrt::unbox_value_or<winrt::hstring>(navigationViewItem.Tag(), L"");
                                    if (hstringValue == eachPage.first)
                                    {
                                        NavView().SelectedItem(navigationViewItem);
                                        NavView().Header(navigationViewItem.Content());
                                    }
                                }
                            }
                        }
                        break;
                    }
                }
            }
        }
    };
}

namespace winrt::NavigationViewCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::NavigationViewCppWinRT::implementation
{
}
```

### <a name="alternative-cwinrt-implementation"></a>대체 C++/WinRT 구현

위의 C# 및 C++/WinRT 코드는 두 버전에 동일한 XAML 태그를 사용할 수 있도록 디자인되었습니다. 그러나 이 섹션에서 설명하는 C++/WinRT 버전을 구현하는 또 다른 방법이 있습니다.

아래는 **NavView_ItemInvoked** 처리기의 대체 버전입니다. 이 버전의 처리기의 기술은 먼저 탐색하려는 페이지의 전체 유형 이름을 [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)의 태그에 저장하는 것과 관련이 있습니다. 처리기에서는 해당 값을 unboxing하고 [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) 개체로 전환한 다음, 이 개체를 사용하여 대상 페이지로 이동합니다. 위 예제에 표시되는 `_pages`라는 매핑 변수를 사용할 필요가 없으며, 태그 내부의 값이 유효한 형식인지 확인하는 단위 테스트를 만들 수 있습니다. [C++/WinRT를 사용해 스칼라 값을 IInspectable로 boxing 및 unboxing](/windows/uwp/cpp-and-winrt-apis/boxing)을 참조하세요.

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="hierarchical-navigation"></a>계층적 탐색
일부 앱은 단순한 탐색 항목 목록 이상을 필요로 하는 더 복잡한 계층 구조를 포함할 수 있습니다. 최상위 탐색 항목을 사용하여 특정 페이지를 표시하는 자식 항목을 포함하는 페이지 범주를 표시할 수도 있습니다. 다른 페이지로만 연결되는 허브 스타일 페이지가 있는 경우에도 유용합니다. 이러한 경우 계층적 NavigationView를 만들어야 합니다.

창에서 중첩된 탐색 항목의 계층적 목록을 표시하려면 [MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitems?view=winui-2.4) 속성 또는 **NavigationViewItem**의 [MenuItemsSource](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitemssource?view=winui-2.4) 속성을 사용합니다.
각 NavigationViewItem은 다른 NavigationViewItems를 포함하고 항목 헤더 및 구분 기호와 같은 요소를 구성할 수 있습니다. `MenuItemsSource`를 사용할 때 계층적 목록을 표시하려면 `ItemTemplate`을 NavigationViewItem으로 설정하고 해당 `MenuItemsSource` 속성을 계층의 다음 수준에 바인딩합니다.

NavigationViewItem은 중첩된 수준을 원하는 만큼 포함할 수 있지만, 앱의 탐색 계층 구조를 단순하게 유지하는 것이 좋습니다. 두 수준이 유용성과 이해력에 이상적입니다.

NavigationView는 위쪽, 왼쪽 및 LeftCompact 창 표시 모드의 계층 구조를 표시합니다. 다음은 각 창 표시 모드에서 확장된 하위 트리가 표시되는 모양입니다.

![계층 구조가 포함된 NavigationView](images/navigation-view-hierarchy-labeled.png)

### <a name="adding-a-hierarchy-of-items-in-markup"></a>태그에 항목의 계층 구조 추가
태그에서 앱 탐색 계층 구조를 선언합니다.

```Xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:NavigationView>
    <muxc:NavigationView.MenuItems>
        <muxc:NavigationViewItem Content="Home" Icon="Home" ToolTipService.ToolTip="Home"/>
        <muxc:NavigationViewItem Content="Collections" Icon="Keyboard" ToolTipService.ToolTip="Collections">
            <muxc:NavigationViewItem.MenuItems>
                <muxc:NavigationViewItem Content="Notes" Icon="Page" ToolTipService.ToolTip="Notes"/>
                <muxc:NavigationViewItem Content="Mail" Icon="Mail" ToolTipService.ToolTip="Mail"/>
            </muxc:NavigationViewItem.MenuItems>
        </muxc:NavigationViewItem>
    </muxc:NavigationView.MenuItems>
</muxc:NavigationView>
```

### <a name="adding-a-hierarchy-of-items-using-data-binding"></a>데이터 바인딩을 사용하여 항목의 계층 구조 추가

다음을 통해 메뉴 항목의 계층 구조를 NavigationView에 추가 
* MenuItemsSource 속성을 계층적 데이터에 바인딩
* 항목 템플릿을 NavigationViewMenuItem으로 정의하고, 해당 콘텐츠는 메뉴 항목의 레이블로 설정 및 MenuItemsSource 속성은 계층 구조의 다음 수준에 바인딩됨

또한 이 예에서는 [Expanding](/uwp/api/microsoft.ui.xaml.controls.navigationview.expanding?view=winui-2.4) 및 [Collapsed](/uwp/api/microsoft.ui.xaml.controls.navigationview.collapsed?view=winui-2.4) 이벤트를 설명합니다. 이러한 이벤트는 자식 항목을 포함하는 메뉴 항목에 대해 발생합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
    <muxc:NavigationViewItem Content="{x:Bind Name}" MenuItemsSource="{x:Bind Children}"/>
</DataTemplate>
<muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}" 
    ItemInvoked="{x:Bind OnItemInvoked}" 
    Expanding="OnItemExpanding" 
    Collapsed="OnItemCollapsed" 
    PaneDisplayMode="Left">
    
    <StackPanel Margin="10,10,0,0">
        <TextBlock Margin="0,10,0,0" x:Name="ExpandingItemLabel" Text="Last Expanding: N/A"/>
        <TextBlock x:Name="CollapsedItemLabel" Text="Last Collapsed: N/A"/>
    </StackPanel>    
</muxc:NavigationView>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String Icon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
}
    
public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  
    
    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu Item 1",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
               new Category(){
                    Name = "Menu Item 2",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { 
                            Name  = "Menu Item 2", 
                            Icon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu Item 3", Icon = "Icon" },
                                new Category() { Name  = "Menu Item 4", Icon = "Icon" }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu Item 5",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu Item 6",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu Item 7", Icon = "Icon" },
                        new Category() { Name  = "Menu Item 8", Icon = "Icon" }
                    }
                }
            }
        },
        new Category(){ Name = "Menu Item 9", Icon = "Icon" }
    };
    private void OnItemInvoked(object sender, NavigationViewItemInvokedEventArgs e)
    {
        var clickedItem = e.InvokedItem;
        var clickedItemContainer = e.InvokedItemContainer;
    }
    private void OnItemExpanding(object sender, NavigationViewItemExpandingEventArgs e)
    {
        var nvib = e.ExpandingItemContainer;
        var name = "Last Expanding: " + nvib.Content.ToString();
        ExpandingItemLabel.Text = name;
    }
    private void OnItemCollapsed(object sender, NavigationViewItemCollapsedEventArgs e)
    {
        var nvib = e.CollapsedItemContainer;
        var name = "Last Collapsed: " + nvib.Content;
        CollapsedItemLabel.Text = name;
    }
}
```
### <a name="selection"></a>선택
기본적으로 모든 항목은 자식을 포함하거나 호출 및 선택할 수 있습니다.
사용자에게 탐색 옵션의 계층 트리를 제공할 때 앱에 부모 항목에 연결된 대상 페이지가 없는 경우와 같이 부모 항목을 선택할 수 없도록 설정할 수도 있습니다. 부모 항목을 선택할 수 _있는_ 경우 왼쪽 확장 또는 위쪽 창 표시 모드를 사용하는 것이 좋습니다. LeftCompact 모드를 사용하면 사용자가 부모 항목을 탐색하여 호출될 때마다 자식 하위 트리를 열 수 있습니다.

선택 항목을 선택하면 왼쪽 모드의 경우 왼쪽 가장자리를 따라, 위쪽 모드에서는 아래쪽 가장자리를 따라 선택 표시기가 표시됩니다. 아래에는 부모 항목이 선택된 왼쪽 및 위쪽 모드의 NavigationViews가 나와 있습니다.

![부모가 선택된 왼쪽 모드의 NavigationView](images/navigation-view-selection.png)

![부모가 선택된 위쪽 모드의 NavigationView](images/navigation-view-selection-top.png)

선택한 항목이 항상 표시되는 것은 아닙니다. 축소되거나 확장되지 않은 하위 트리의 자식을 선택하면 첫 번째로 표시되는 상위 항목이 선택된 상태로 표시됩니다. 하위 트리가 확장되면 선택 표시기가 선택된 항목으로 다시 이동합니다.

예를 들어 위의 이미지에서 사용자가 일정 항목을 선택한 다음, 사용자가 해당 하위 트리를 축소할 수 있습니다. 이 경우 계정이 일정의 첫 번째 상위 항목이기 때문에 계정 항목 아래에 선택 표시기가 표시됩니다. 사용자가 하위 트리를 다시 확장하면 선택 표시기가 일정 항목으로 다시 이동합니다. 

전체 NavigationView에는 둘 이상의 선택 표시기가 표시되지 않습니다.

위쪽과 왼쪽 모드 모두에서 NavigationViewItems의 화살표를 클릭하면 하위 트리가 확장되거나 축소됩니다. NavigationViewItem에서 _아무데나_ 클릭하거나 탭하면 `ItemInvoked` 이벤트가 트리거되고 하위 트리도 축소 또는 확장됩니다.

항목이 호출될 때 선택 표시기를 표시하지 않도록 하려면 아래와 같이 [SelectsOnInvoked](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.selectsoninvoked?view=winui-2.3) 속성을 False로 설정합니다.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
    <muxc:NavigationViewItem Content="{x:Bind Name}" 
        MenuItemsSource="{x:Bind Children}"
        SelectsOnInvoked="{x:Bind IsLeaf}" />
</DataTemplate>
<muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}">
   
</muxc:NavigationView>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String Icon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
    public bool IsLeaf { get; set; }
}
    
public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }      
    
    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu Item 1",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu Item 2",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { 
                            Name  = "Menu Item 2", 
                            Icon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu Item 3", Icon = "Icon", IsLeaf = true },
                                new Category() { Name  = "Menu Item 4", Icon = "Icon", IsLeaf = true }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu Item 5",
            Icon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu Item 6",
                    Icon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu Item 7", Icon = "Icon", IsLeaf = true },
                        new Category() { Name  = "Menu Item 8", Icon = "Icon", IsLeaf = true }
                    }
                }
            }
        },
        new Category(){ Name = "Menu Item 9", Icon = "Icon", IsLeaf = true }
    };
}
```

### <a name="keyboarding-within-hierarchical-navigationview"></a>계층적 NavigationView 내의 키보드 사용
사용자는 [키보드](/windows/uwp/design/input/keyboard-interactions)를 사용하여 탐색 보기 주위에서 포커스를 이동할 수 있습니다. 화살표 키는 창 내에서 “내부 탐색”을 노출하고 [트리 보기](/windows/uwp/design/controls-and-patterns/tree-view)에서 제공하는 상호 작용을 따릅니다. 키 동작은 HierarchicalNavigationView의 위쪽 및 왼쪽 컴팩트 모드에서 표시되는 NavigationView 또는 플라이아웃 메뉴를 통해 탐색할 때 변경됩니다. 다음은 각 키가 계층적 NavigationView에서 수행할 수 있는 특정 작업입니다.

| 키      |      왼쪽 모드에서      |  위쪽 모드에서 | 플라이아웃에서  |
|----------|------------------------|--------------|------------|
| 위로 |현재 포커스를 둔 항목 바로 위에 있는 항목으로 포커스를 이동합니다. | 아무 작업도 하지 않습니다. |현재 포커스를 둔 항목 바로 위에 있는 항목으로 포커스를 이동합니다.|
| 아래로|현재 포커스를 둔 항목 바로 아래에 포커스를 이동합니다.* | 아무 작업도 하지 않습니다. | 현재 포커스를 둔 항목 바로 아래에 포커스를 이동합니다.* |
| 오른쪽 |아무 작업도 하지 않습니다.  |현재 포커스를 둔 항목 바로 오른쪽에 있는 항목으로 포커스를 이동합니다. |아무 작업도 하지 않습니다.|
| 왼쪽 |아무 작업도 하지 않습니다. | 현재 포커스를 둔 항목 바로 왼쪽에 있는 항목으로 포커스를 이동합니다.  |아무 작업도 하지 않습니다. |
| 스페이스바/Enter 키 |항목에 자식 항목이 있으면 항목을 확장/축소하고 포커스를 변경하지 않습니다.   | 항목에 자식 항목이 있으면 자식 항목을 플라이아웃으로 확장하고 플라이아웃의 첫 번째 항목에 포커스를 둡니다. | 항목을 호출하거나 선택하고 플라이아웃을 닫습니다. |
| Esc | 아무 작업도 하지 않습니다. | 아무 작업도 하지 않습니다. | 플라이아웃을 닫습니다.|

스페이스바 또는 Enter 키는 항상 항목을 호출하거나 선택합니다.

*항목이 시각적으로 인접하지 않아도 되는 경우 창의 목록에 있는 마지막 항목에서 설정 항목으로 포커스가 이동합니다. 

## <a name="navigation-view-customization"></a>탐색 보기 사용자 지정

### <a name="pane-backgrounds"></a>창 배경

기본적으로 NavigationView 창은 디스플레이 모드에 따라 다른 배경을 사용합니다.

- 창이 콘텐츠와 나란히 왼쪽에서 확장되는 경우(Left 모드) 회색으로 표시됩니다.
- 창이 콘텐츠 위쪽에 오버레이로 열리는 경우(Top, Minimal 또는 Compact 모드) 앱 내 아크릴을 사용합니다.

창 배경을 수정하기 위해 각 모드에서 배경을 렌더링하는 데 사용되는 XAML 테마 리소스를 재정의할 수 있습니다. 이 기술은 디스플레이 모드에 따라 다른 배경을 지원하기 위해 단일 PaneBackground 속성 대신 사용됩니다.

이 표에서는 각 디스플레이 모드에서 사용되는 테마 리소스를 보여 줍니다.

| 디스플레이 모드 | 테마 리소스 |
| ------------ | -------------- |
| 왼쪽 | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| 맨 위로 | NavigationViewTopPaneBackground |

이 예제는 App.xaml에서 테마 리소스를 재정의하는 방법을 보여 줍니다. 테마 리소스를 재정의하는 경우에는 항상 최소한 “Default” 및 “HighContrast” 리소스 사전을 제공하며 필요에 따라 “Light” 또는 “Dark” 리소스 사전을 제공해야 합니다. 자세한 내용은 [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)를 참조하세요.

> [!IMPORTANT]
> 이 코드는 AcrylicBrush의 [Windows UI 라이브러리](/uwp/toolkits/winui/) 버전을 사용하는 방법을 보여 줍니다. AcrylicBrush 플랫폼 버전을 사용하는 경우에는 앱 프로젝트의 최소 버전이 SDK 16299 이상이어야 합니다. 플랫폼 버전을 사용하려면 `muxm:`에 대한 모든 참조를 제거합니다.

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### <a name="top-whitespace"></a>상단 공백
일부 앱은 [해당 창의 제목 표시줄을 사용자 지정](/windows/uwp/design/shell/title-bar)하여 해당 앱 콘텐츠를 제목 표시줄 영역으로 확장하도록 선택할 수 있습니다. NavigationView가 [ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar) API**를 사용하여 제목 표시줄** 로 확장되는 앱의 루트 요소인 경우, 컨트롤은 대화형 요소의 위치를 자동으로 조정하여 [끌기 가능 영역](/windows/uwp/design/shell/title-bar#draggable-regions)과 겹치지 않도록 합니다. 
![제목 표시줄로 확장되는 앱](images/navigation-view-with-titlebar-padding.png)

앱이 [Window.SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) 메서드를 호출하여 끌기 가능 영역을 지정하고 뒤로 및 메뉴 단추를 앱 창의 상단에 더 가깝게 나타내려면 `IsTitleBarAutoPaddingEnabled`를 False로 설정합니다.

![추가 패딩 없이 제목 표시줄로 확장되는 앱](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>설명
NavigationView의 헤더 영역 위치를 추가로 조정하려면 *NavigationViewHeaderMargin* XAML 테마 리소스를 재정의합니다(예: 페이지 리소스에서).

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

이 테마 리소스가 [NavigationView.Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) 주위의 여백을 수정합니다.

## <a name="related-topics"></a>관련 항목

- [NavigationView 클래스](/uwp/api/windows.ui.xaml.controls.navigationview)
- [마스터/세부](master-details.md)
- [탐색 기본 사항](../basics/navigation-basics.md)
- [Fluent Design 개요](/windows/apps/fluent-design-system)
