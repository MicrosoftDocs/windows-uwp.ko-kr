---
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 탐색 보기
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b61c36143749ddb358cea1d4cf59f43ecb8c6338
ms.sourcegitcommit: a60ab85e9f2f9690e0141050ec3aa51f18ec61ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "9037095"
---
# <a name="navigation-view"></a>탐색 보기

NavigationView 컨트롤 앱에 최상위 탐색을 제공 합니다. 다양 한 화면 크기에 맞게 조정 되 고 _위쪽_ 및 _왼쪽_ 탐색 스타일을 모두 지원 합니다.

![상단 탐색](images/nav-view-header.png)<br/>
_상단 왼쪽된 탐색 창 또는 메뉴 탐색 보기 지원_

> **플랫폼 Api**: [Windows.UI.Xaml.Controls.NavigationView 클래스](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 라이브러리 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 클래스](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> NavigationView, _상단_ 탐색 등의 일부 기능은 Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk))이 필요 이상 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)입니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

NavigationView는 잘 작동 하는 적응형 탐색 컨트롤입니다.

- 앱 전체에서 일관 된 탐색 환경을 제공합니다.
- 작은 창에서 화면 공간을 유지 합니다.
- 많은 탐색 범주에 대 한 액세스를 구성 합니다.

다른 탐색 패턴에 대 한 [탐색 디자인 기본 사항](../basics/navigation-basics.md)참조 하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/NavigationView">앱을 열고 작동 중인 NavigationView를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>디스플레이 모드

> PaneDisplayMode 속성에는 Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 필요 이상 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)입니다.

PaneDisplayMode 속성을 사용 하 여 여러 탐색 스타일을 구성 하거나 NavigationView에 대 한 디스플레이 모드를 수 있습니다.

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    창이는 콘텐츠 위에 놓입니다.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![상단 탐색의 예](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

_상단_ 탐색 권장 경우:

- 해야 5 또는 동일 하 게 중요 하다 고 더 적은 최상위 수준 탐색 범주 및 모든 추가 최상위 수준 탐색 범주 드롭다운 오버플로 메뉴에는 덜 중요 한 것으로 간주 됩니다.
- 화면에서 모든 탐색 옵션을 표시 해야 합니다.
- 원하는 앱 콘텐츠에 대 한 더 많은 공간 합니다.
- 아이콘 명확 하 게 앱의 탐색 범주를 나타내는 수 없습니다.

:::row:::
    :::column:::
    ### <a name="left"></a>왼쪽
    창이 확장 하 고 콘텐츠 왼쪽에 배치 됩니다.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![확장 된 왼쪽된 탐색 창의 예](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

_왼쪽된_ 탐색 권장 경우:

- 5-10 만큼 중요 한 역할 최상위 수준 탐색 범주 있습니다.
- 원하는 탐색 범주를 다른 앱 콘텐츠에 대 한 더 적은 공간으로 매우 중요 합니다.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    열리고 콘텐츠 왼쪽에 배치 될 때까지 아이콘만 표시 됩니다.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![컴팩트 왼쪽된 탐색 창의 예](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    창이 열릴 때까지 메뉴 단추만 표시 됩니다. 열 때 콘텐츠 왼쪽에 배치 됩니다.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![최소한의 왼쪽된 탐색 창의 예](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>자동

기본적으로 PaneDisplayMode 자동으로 설정 됩니다. 자동 모드에서 탐색 보기 창이 좁은 경우 LeftCompact를 LeftMinimal 사이 맞춰 조정 하 고 왼쪽 창 높아질수록 더 넓은 합니다. 자세한 내용은 [적응형 동작](#adaptive-behavior) 섹션을 참조 하세요.

![왼쪽된 탐색 기본 적응형 동작](images/displaymode-auto.png)<br/>
_탐색 보기 기본 적응형 동작_

## <a name="anatomy"></a>구조

이러한 이미지 창, 헤더 및 콘텐츠 영역의 _위쪽_ 또는 _왼쪽_ 탐색 하도록 구성 된 경우 컨트롤의 레이아웃을 보여 줍니다.

![상단 탐색 보기 레이아웃](images/topnav-anatomy.png)<br/>
_상단 탐색 레이아웃_

![왼쪽된 탐색 보기 레이아웃](images/leftnav-anatomy.png)<br/>
_왼쪽된 탐색 레이아웃_

### <a name="pane"></a>창

창 왼쪽에 있는 콘텐츠 또는 콘텐츠 위에 배치 하려면 PaneDisplayMode 속성을 사용할 수 있습니다.

NavigationView 창에 포함 될 수 있습니다.

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) 개체입니다. 특정 페이지를 탐색 하는 것에 대 한 항목을 탐색 합니다.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) 개체입니다. 탐색 항목 그룹화를 위한 구분 합니다. 공백으로 구분 기호를 렌더링 하는 0으로 [Opacity](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) 속성을 설정 합니다.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 개체입니다. 항목 그룹에 레이블을 지정 헤더입니다.
- 앱 수준 검색을 허용할 옵션 [AutoSuggestBox](auto-suggest-box.md) 컨트롤입니다. 컨트롤 [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) 속성을 할당 합니다.
- [앱 설정](../app-settings/app-settings-and-data.md)에 대한 선택적 진입점 설정 항목을 숨기려면 [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 속성을 **false**로 설정 합니다.

왼쪽된 창 포함 되어 있습니다.

- 창이 열리고 닫힐 전환 하는 메뉴 단추입니다. 큰 앱 창에서 창이 열리면 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 속성을 사용하여 이 단추를 숨기도록 선택할 수 있습니다.

탐색 보기에 뒤로 단추는 창의 왼쪽 위 모서리에 배치 됩니다. 그러나는 자동으로 뒤로 탐색을 처리 하 고 백 스택에 콘텐츠를 추가 합니다. 뒤로 탐색을 사용 하려면 참조를 [뒤로 탐색](#backwards-navigation) 섹션.

위쪽 / 왼쪽 창 위치에 대 한 자세한 창 구조는 다음과 같습니다.

#### <a name="top-navigation-pane"></a>상단 탐색 창

![탐색 보기 상단 창 분석](images/navview-pane-anatomy-horizontal.png)

1. 헤더
1. 탐색 항목
1. 구분 기호
1. AutoSuggestBox (선택 사항)
1. 설정 버튼 (선택 사항)

#### <a name="left-navigation-pane"></a>왼쪽된 탐색 창

![왼쪽 창 구조 탐색 보기](images/navview-pane-anatomy-vertical.png)

1. 메뉴 버튼
1. 탐색 항목
1. 구분 기호
1. 헤더
1. AutoSuggestBox (선택 사항)
1. 설정 버튼 (선택 사항)

#### <a name="pane-footer"></a>창 폴더

[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 속성에 추가 하 여 창의 폴더에서 자유 형식의 콘텐츠를 배치할 수 있습니다.

:::row:::
    :::column:::
    ![폴더의 상단 탐색 창](images/navview-freeform-footer-top.png)<br>
     _상단 창 폴더_<br>
    :::column-end:::
    :::column:::
    ![폴더의 왼쪽된 탐색 창](images/navview-freeform-footer-left.png)<br>
    _왼쪽된 창 폴더_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>창 제목 및 헤더

[PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 속성을 설정 하 여 창 헤더 영역에 텍스트 콘텐츠를 배치할 수 있습니다. 문자열을 사용 하 고 메뉴 단추 옆에 있는 텍스트를 표시 합니다.

이미지 또는 로고 같은 텍스트가 아닌 콘텐츠를 추가 하려면 [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) 속성에 추가 하 여 창의 헤더의 모든 요소를 배치할 수 있습니다.

PaneTitle와 PaneHeader 설정 되 면 콘텐츠 메뉴 단추에 가장 가까운 PaneTitle 사용 하 여 메뉴 단추 옆 가로로 누적 됩니다.

:::row:::
    :::column:::
    ![헤더의 상단 탐색 창](images/navview-freeform-header-top.png)<br>
     _상단 창 헤더_<br>
    :::column-end:::
    :::column:::
    ![헤더의 왼쪽된 탐색 창](images/navview-freeform-header-left.png)<br>
    _왼쪽된 창 헤더_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>창 콘텐츠

[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) 속성에 추가 하 여 창에서 자유 형식의 콘텐츠를 배치할 수 있습니다.

:::row:::
    :::column:::
    ![사용자 지정 콘텐츠 상단 탐색 창](images/navview-freeform-pane-top.png)<br>
     _사용자 지정 콘텐츠 상단 창_<br>
    :::column-end:::
    :::column:::
    ![왼쪽 탐색 창 사용자 지정 콘텐츠](images/navview-freeform-pane-left.png)<br>
    _왼쪽된 창 사용자 지정 콘텐츠_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>헤더

[Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) 속성을 설정 하 여 페이지 제목에 추가할 수 있습니다.

![탐색 보기 머리글 영역은의 예](images/nav-header.png)<br/>
_탐색 보기 헤더_

머리글 영역은 위치 왼쪽된 창에서에서 탐색 단추와 세로 방향으로 정렬 되며와 창 맨 위에 있는 창 위치에 아래에 있습니다. 높이가 52 픽셀로 고정된 되었기 px 합니다. 선택한 탐색 범주의 페이지 제목을 유지하는 것이 목적입니다. 머리글이 페이지 위쪽에 고정되고 콘텐츠 영역에 대한 스크롤 자르기 지점 역할을 합니다.

헤더는 NavigationView가 최소 디스플레이 모드 언제 든 지 표시 됩니다. 더 큰 창 너비에 사용되는 다른 모드에서 머리글을 숨기도록 선택할 수 있습니다. 헤더를 숨기려면 [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 속성을 **false**로 설정 합니다.

### <a name="content"></a>콘텐츠

![탐색 보기 콘텐츠 영역의 예](images/nav-content.png)<br/>
_탐색 보기 콘텐츠_

콘텐츠 영역은 선택한 탐색 범주에 대한 대부분의 정보가 표시되는 곳입니다.

NavigationView가 **최소** 모드 경우 콘텐츠 영역에 대해 12px 여백이 고 여백이 24px 일 그렇지 않으면 권장.

## <a name="adaptive-behavior"></a>적응형 동작

기본적으로 탐색 보기가 자동으로 사용 가능한 화면 공간의 크기에 따라 디스플레이 모드를 변경 합니다. [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 및 [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) 속성 표시 모드가 변경 중단점을 지정 합니다. 적응형 표시 모드 동작을 사용자 지정 하는 이러한 값을 수정할 수 있습니다.

### <a name="default"></a>Default

PaneDisplayMode **자동**의 기본 값으로 설정 된 경우 적응 동작이 표시 하는:

- 큰 창 너비에는 확장 된 왼쪽된 창 (1008px 이상).
- A 왼쪽, 아이콘 전용 중간 창 너비에 탐색 창 (LeftCompact) (641px ~ 1007px).
- 만 메뉴에 있는 단추 (LeftMinimal) 창 너비가 작을 때는 (640px 이하의).

적응형 동작에 대 한 창 크기에 대 한 자세한 내용은 [화면 크기 및 중단점을](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)참조 하세요.

![왼쪽된 탐색 기본 적응형 동작](images/displaymode-auto.png)<br/>
_탐색 보기 기본 적응형 동작_

### <a name="minimal"></a>최소

두 번째 일반적인 적응 패턴 큰 창 너비와 중간 크기 및 작은 창 너비 모두에 메뉴 단추에는 확장 된 왼쪽된 창 사용 하는 것입니다.

이 경우를 사용 하는 것이 좋습니다.

- 원하는 앱 콘텐츠를 더 작은 창 너비에 대 한 더 많은 공간 합니다.
- 아이콘이 있는 탐색 범주를 명확 하 게 나타낼 수 없습니다.

![왼쪽된 탐색 최소한의 적응 동작](images/adaptive-behavior-minimal.png)<br/>
_탐색 보기 "최소" 적응형 동작_

이 동작을 구성 하려면 CompactModeThresholdWidth 축소 하 고 창 너비를 설정 합니다. 여기에서는 640를 1007 기본값에서 변경 됩니다. ExpandedModeThresholdWidth 값 충돌 하지 않는 되도록 설정 해야 합니다.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>컴팩트

세 번째 일반적인 적응 패턴 큰 창 너비와는 LeftCompact, 아이콘 전용 모두 중간 크기 및 작은 창 너비에 탐색 창에는 확장 된 왼쪽된 창을 사용 하는 것입니다.

이 경우를 사용 하는 것이 좋습니다.

- 것이 항상 화면에서 모든 탐색 옵션을 표시 하는 것이 중요 합니다.
- 아이콘이 있는 탐색 범주를 명확 하 게 나타낼 수 있습니다.

![왼쪽된 탐색 컴팩트 적응형 동작](images/adaptive-behavior-compact.png)<br/>
_탐색 보기 "압축" 적응형 동작_

이 동작을 구성 하려면 CompactModeThresholdWidth 0으로 설정 합니다.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>적응형 동작 없음

자동 적응형 동작을 비활성화 하려면 PaneDisplayMode 자동 이외의 값으로 설정 합니다. 여기서 설정에 LeftMinimal, 메뉴 단추만 창 너비에 관계 없이 표시 됩니다.

![왼쪽 탐색 적응형 동작 없음](images/adaptive-behavior-none.png)<br/>
_PaneDisplayMode LeftMinimal로 설정 된 탐색 보기_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

_디스플레이 모드_ 섹션에서 앞에서 설명한 대로 창이 항상 확장, 항상 컴팩트 또는 항상 최소한의 위쪽에 항상 되도록 설정할 수 있습니다. 또한 앱 코드에서 디스플레이 모드를 직접 관리할 수도 있습니다. 이 예제는 다음 섹션에 표시 됩니다.

### <a name="top-to-left-navigation"></a>왼쪽된 탐색 위쪽

상단 탐색을 사용 하 여 앱에서 탐색 항목이 오버플로 메뉴에 창 너비 감소로 축소 합니다. 앱 창이 좁은 오버플로 메뉴에 항목을 축소 모든 수 있도록 하는 대신 LeftMinimal 탐색 위에서 PaneDisplayMode 전환 하는 더 나은 사용자 환경을 제공할 수 것입니다.

큰 창 크기와 작은에서 왼쪽된 탐색에서 상단 탐색을 사용 하는 것이 좋습니다 창 크기는 경우:

- 이 집합에서 하나의 범주는 화면에 맞지 않는, 바람직한 수 있도록 왼쪽된 탐색을 축소 되도록 함께 표시할 동일 하 게 중요 한 최상위 수준 탐색 범주 집합이 있습니다.
- 작은 창 크기에 최대한 많은 콘텐츠 공간으로 유지 하려는 합니다.

이 예제에서는 위쪽 및 LeftMinimal 탐색 간에 전환 하려면 [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) 및 [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 속성을 사용 하는 방법을 보여 줍니다.

![위쪽 또는 왼쪽 적응형 동작 1의 예](images/navigation-top-to-left.png)

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
> AdaptiveTrigger.MinWindowWidth를 사용 하면 창이 지정된 된 최소 너비 보다 더 넓은 시각적 상태가 트리거됩니다. 즉, 기본 XAML 좁은 창을 정의 하 고는 VisualState 정의 창을 더 넓은 가져올 때 적용 되는 수정 합니다. 기본 PaneDisplayMode 탐색 보기가 자동으로 되어 있으므로 창 너비가 CompactModeThresholdWidth, 보다 작거나 같은 경우 LeftMinimal 탐색 사용 합니다. 창 너비를 받으면는 VisualState 기본값을 재정의 하 고 위쪽 탐색 사용 됩니다.

## <a name="navigation"></a>탐색

탐색 보기는 자동으로 탐색 작업을 수행 하지 않습니다. 사용자가 탐색 항목에 탭 탐색 보기 해당 항목을 선택된 되며 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 이벤트를 발생 시킵니다. 탭에 새 항목이 선택 되 고 결과가 [때마다 SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 이벤트가 발생 합니다.

요청 된 탐색과 관련 된 작업을 수행 하려면 두 이벤트를 처리할 수 있습니다. 하나의 처리 해야 하는 앱에 대해 원하는 동작에 따라 달라 집니다. 일반적으로 요청 된 페이지로 이동 하 고 이러한 이벤트에 대 한 응답으로 탐색 보기 헤더를 업데이트 합니다.

**ItemInvoked** 이 이미 선택 하는 경우에 사용자가 탐색 항목을 탭 할 때마다 발생 합니다. (항목 마우스, 키보드 또는 다른 입력을 사용 하 여 이와 동등한 작업으로 호출할 수 수도 있습니다. 자세한 내용은 참조 [입력 및 조작](../input/index.md)합니다.) ItemInvoked 처리기에서 이동 하면 기본적으로 페이지 다시 로드할 수, 고 중복 된 항목 탐색 스택에 추가 됩니다. 항목 호출 될 때 이동 하면 로드 페이지를 허용 하지 않습니다. 하거나 고 페이지가 다시 로드 될 때 중복 된 항목 탐색 백 스택에에서 작성 되지는 확인 해야 합니다. (코드 예제 참조).

**SelectionChanged** 현재 선택 되지 않은 항목 호출은 사용자 또는 프로그래밍 방식으로 선택된 된 항목을 변경 하 여 발생할 수 있습니다. 사용자가 항목을 호출 하기 때문에 선택 변경이 발생 하는 경우 먼저 ItemInvoked 이벤트 발생 합니다. 선택 변경 내용이 프로그래밍 방식으로 ItemInvoked 발생 하지 않습니다.

### <a name="backwards-navigation"></a>뒤로 탐색

NavigationView에 기본 제공 뒤로 단추가 있습니다. 하지만 앞으로 탐색와 마찬가지로 것도 하지 뒤로 탐색 자동으로. 사용자가 뒤로 단추를 탭 하면 [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 이벤트는 발생 합니다. 뒤로 탐색을 수행 하려면이 이벤트를 처리할 수 있습니다. 자세한 내용과 코드 예제를 보려면 [탐색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)합니다.

최소 또는 컴팩트 모드 탐색 보기 창 플라이 아웃으로 열릴 합니다. 이 경우 뒤로 단추를 클릭 하 여 창을 닫습니다 되며 대신 **PaneClosing** 이벤트를 발생 시킵니다.

숨기 거 나 이러한 속성을 설정 하 여 뒤로 단추를 사용 하지 않도록 설정할 수 있습니다.

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): 사용 하 여 표시 하 고 뒤로 단추를 숨깁니다. 이 속성은 [navigationviewbackbuttonvisible 열거](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) 열거형의 값을 사용 하 고 기본적으로 **자동** 으로 설정 됩니다. 단추를 축소 하면 레이아웃에 공백이에 예약 되어 있습니다.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): 뒤로 단추를 사용할지 여부를 사용 합니다. 이 속성을 탐색 프레임의 [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) 속성을 데이터 바인딩할 수 있습니다. **IsBackEnabled** 가 **false**이면 **BackRequested** 발생 하지 않습니다.

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>코드 예제

이 예제에서는 큰 창 크기에 상단 탐색 창 및 작은 창 크기에는 왼쪽된 탐색 창 모두 NavigationView를 사용 하는 방법을 보여 줍니다. VisualStateManager에서 _상단_ 탐색 설정을 제거 하 여 왼쪽 전용 탐색에 적용 될 수 있습니다.

이 예제에서는 여러 일반적인 시나리오에 대해 작동 하는 탐색 데이터를 설정 하는 권장된 방법을 보여 줍니다. 또한 뒤로 탐색 NavigationView의 뒤로 단추 및 키보드 탐색을 구현 하는 방법을 보여 줍니다.

이 코드는 앱 페이지를 탐색 하려면 다음과 같은 이름으로 포함 되어 있다고 가정 합니다.: _홈 페이지_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_및 _SettingsPage_합니다. 이 페이지에 대 한 코드 표시 되지 않습니다.

> [!IMPORTANT]
> 앱의 페이지에 대 한 정보는 [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple)에 저장 됩니다. 이 구조체는 응용 프로그램 프로젝트에 대 한 최소 버전 17763 이상 SDK 여야 필요 합니다. NavigationView의 WinUI 버전을 사용 하 여 이전 버전의 Windows 10을 대상으로 하는 경우 대신 [System.ValueTuple NuGet 패키지](https://www.nuget.org/packages/System.ValueTuple/) 를 사용할 수 있습니다.

> [!IMPORTANT]
> 이 코드는 NavigationView의 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/) 버전을 사용 하는 방법을 보여 줍니다. NavigationView의 플랫폼 버전을 대신 사용 하는 경우 앱 프로젝트에 대 한 최소 버전 SDK 17763 이상 이어야 합니다. 플랫폼 버전을 사용 하려면 모든 참조를 제거 `muxc:`.

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
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
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
> 이 코드는 NavigationView의 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/) 버전을 사용 하는 방법을 보여 줍니다. NavigationView의 플랫폼 버전을 대신 사용 하는 경우 앱 프로젝트에 대 한 최소 버전 SDK 17763 이상 이어야 합니다. 플랫폼 버전을 사용 하려면 모든 참조를 제거 `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

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
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
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

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
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

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
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

다음은 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/index) C# 코드 예제에서는 위의 **NavView_ItemInvoked** 처리기의 버전입니다. 기술은 C + + WinRT 처리기를 포함 하면 ( [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)태그)에 저장 된 먼저 페이지의 전체 형식 이름을 탐색할 수도 있습니다. 해당 값을 언박싱 처리기에서 [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) 개체를 변환 및 대상 페이지로 이동 하는 사용 합니다. 라는 매핑 변수의 필요가 없습니다 `_pages` C# 예제에서는;에서 볼 수 있는 및 유효한 형식이 태그 내에서 값이 있는지 확인 하는 단위 테스트를 만들 수 있게 됩니다. 참고 [Boxing 및 unboxing 스칼라 값을 IInspectable로 C + + WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

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

## <a name="navigation-view-customization"></a>탐색 보기 사용자 지정

### <a name="pane-backgrounds"></a>창 배경

기본적으로 NavigationView 창 디스플레이 모드에 따라 다른 배경을 사용합니다.

- 창이 회색 단색 나란히 (왼쪽된 모드)에서 콘텐츠를 사용 하 여 왼쪽에 확장 합니다.
- 창이 열릴 때 인 앱 아크릴을 사용 하 여 (위쪽, 최소 또는 컴팩트 모드)에서 콘텐츠 위에 오버레이로 합니다.

창 배경을 수정 하려면 각 모드의 배경을 렌더링 하는 데 XAML 테마 리소스를 재정의할 수 있습니다. (이 기술을 사용 단일 PaneBackground 속성이 아닌 다른 배경 다양 한 디스플레이 모드에 대 한 지원 하기 위해.)

이 표에서 각 디스플레이 모드에서 사용 되는 테마 리소스를 보여줍니다.

| 디스플레이 모드 | 테마 리소스 |
| ------------ | -------------- |
| 왼쪽 | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Top | NavigationViewTopPaneBackground |

이 예제에서는 App.xaml에서 테마 리소스를 재정의 하는 방법을 보여 줍니다. 테마 리소스를 재정의 하면 항상 제공 해야 최소한 "기본" 및 "고대비" 리소스 사전 및 사전 "밝게" 또는 "어둡게" 리소스 필요에 따라 합니다. 자세한 내용은 [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)를 참조 하세요.

> [!IMPORTANT]
> 이 코드의 AcrylicBrush [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/) 의 버전을 사용 하는 방법을 보여 줍니다. 플랫폼 버전 AcrylicBrush 대신 사용 하는 경우 앱 프로젝트에 대 한 최소 버전 SDK 16299 이상 이어야 합니다. 플랫폼 버전을 사용 하려면 모든 참조를 제거 `muxm:`.

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

## <a name="related-topics"></a>관련 항목

- [NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [마스터/세부](master-details.md)
- [탐색 기본 사항](../basics/navigation-basics.md)
- [UWP용 흐름 디자인 개요](../fluent-design-system/index.md)
