---
author: QuinnRadich
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 탐색 보기
template: detail.hbs
ms.author: quradic
ms.date: 06/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6c75169f118e2c8ef575fa251a7badc8cfe44247
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4155442"
---
# <a name="navigation-view-preview-version"></a>탐색 보기 (미리 보기 버전)

> **이 미리 보기 버전**: 새 버전의 개발 중인 NavigationView 컨트롤에 설명 합니다. 사용 하 여 이제 하려면 [최신 Windows 참가자 빌드 및 SDK](https://insider.windows.com/for-developers/) 나 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/).

NavigationView 컨트롤 앱에 최상위 탐색을 제공 합니다. 적응 하는 다양 한 화면 크기에서 지 원하는 여러 탐색 스타일.

> **Windows UI 라이브러리 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 클래스](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **플랫폼 Api**: [Windows.UI.Xaml.Controls.NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>Windows UI 라이브러리 가져오기

이 컨트롤은 Windows UI 라이브러리 새 컨트롤 및 UWP 앱에 대 한 UI 기능을 포함 하는 NuGet 패키지의 일부로 포함 합니다. 설치 지침을 비롯 한 자세한 내용은 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조 하세요. 

## <a name="navigation-styles"></a>탐색 스타일

NavigationView를 지원합니다.

**왼쪽된 탐색 창 또는 메뉴**

![확장 된 탐색 창](images/displaymode-left.png)

**상단 탐색 창 또는 메뉴**

![상단 탐색](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

NavigationView는 잘 작동 하는 적응형 탐색 컨트롤입니다.

- 앱 전체에서 일관 된 탐색 환경을 제공 합니다.
- 작은 창에서 화면 공간을 유지 합니다.
- 많은 탐색 범주에 대 한 액세스를 구성 합니다.

다른 탐색 컨트롤을 [탐색 디자인 기본 사항](../basics/navigation-basics.md)을 참조 하세요.

탐색을 위해 NavigationView에서 지원되지 않는 보다 복잡한 동작이 필요할 경우에는 [마스터/세부 정보](master-details.md) 패턴을 대신 고려할 수 있습니다.

:::row:::
    :::column:::
        ![일부 이미지](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: 열 범위 = "2"::: **XAML 컨트롤 갤러리**<br>
        XAML 컨트롤 갤러리 앱이 설치 된 경우 클릭 <a href="xamlcontrolsgallery:/item/NavigationView">여기</a> 앱을 열고 중인 NavigationView를 참조 하세요.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>디스플레이 모드

NavigationView를 통해 다양 한 디스플레이 모드를 설정할 수는 `PaneDisplayMode` 속성:

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

왼쪽된 탐색 권장 경우:

- 동일 하 게 중요 한 최상위 수준 탐색 범주의 중간에서 높은 쪽 (5-10) 해야합니다.
- 원하는 다른 앱 콘텐츠에 대 한 더 적은 공간으로 매우 중요 탐색 범주입니다.

:::row:::
    :::column:::
    ### Top
    Displays a top positioned pane.
    :::column-end:::
    :::column span="2":::
    ![top navigation](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

상단 탐색 권장 경우:

- 5 하거나 덜 중요 한 동일 하 게 최상위 수준 탐색 범주 같은 추가 최상위 수준 탐색 범주 드롭다운 목록에서 결국에 모두 오버플로 메뉴는로 간주 된다는 덜 중요 한 합니다.
- 화면의 탐색 옵션을 모두 표시 해야 합니다.
- 원하는 앱 콘텐츠에 대 한 더 많은 공간.
- 아이콘을 앱의 탐색 범주 설명 명확 하 게 수 없습니다.

:::row:::
    :::column:::
    ### LeftCompact
    Displays a thin sliver with icons on the left.
    :::column-end:::
    :::column span="2":::
    ![nav pane compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### LeftMinimal
    Displays only the menu button.
    :::column-end:::
    :::column span="2":::
    ![nav pane minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>자동

![gif leftnav 기본 적응형 동작](images/displaymode-auto.png)

작은 화면 LeftMinimal, LeftCompact 사이의 중간 화면 및 큰 화면에서 왼쪽으로 적응합니다. 자세한 내용은 [적응형 동작](#adaptive-behavior) 섹션을 참조 하세요.

## <a name="anatomy"></a>구조

<b>왼쪽된 탐색</b><br>

![왼쪽된 NavigationView 섹션](images/leftnav-anatomy.png)

<b>상단 탐색</b><br>

![최상위 NavigationView 섹션](images/topnav-anatomy.png)

## <a name="pane"></a>창

창을 통해에 왼쪽 또는 맨 위에 배치할 수는 `PanePosition` 속성입니다.

창 왼쪽 및 위쪽 위치에 대 한 자세한 창 구조는 다음과 같습니다.

<b>왼쪽된 탐색</b><br>

![NavigationView 분석](images/navview-pane-anatomy-vertical.png)

1. 메뉴 버튼
1. 탐색 항목
1. 구분 기호
1. 헤더
1. AutoSuggestBox (선택 사항)
1. 설정 버튼 (선택 사항)

<b>상단 탐색</b><br>

![NavigationView 분석](images/navview-pane-anatomy-horizontal.png)

1. 헤더
1. 탐색 항목
1. 구분 기호
1. AutoSuggestBox (선택 사항)
1. 설정 버튼 (선택 사항)

뒤로 단추는 창의 왼쪽 위 모서리에 나타나지만 NavigationView 콘텐츠를 백 스택에 자동으로 추가 하지 않습니다. 뒤로 탐색 사용을 참조 합니다 [뒤로 탐색](#backwards-navigation) 섹션.

NavigationView 창 포함 될 수도 있습니다.

1. 특정 페이지로 이동 하기 위한 [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)형식의 탐색 항목
2. 구분 기호 [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)숫자 형태로 탐색 항목 그룹화 합니다. 공백으로 구분 기호를 렌더링 하는 0으로 [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) 속성을 설정 합니다.
3. 헤더 [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)형태로 항목 그룹에 레이블을 지정 합니다.
4. 앱 수준 검색을 허용할 선택적 [AutoSuggestBox](auto-suggest-box.md) 입니다.
5. [앱 설정](../app-settings/app-settings-and-data.md)에 대한 선택적 진입점 설정 항목을 숨기려면 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 속성을 사용 합니다.

왼쪽된 창에 포함 되어 있습니다.

6. 창 열기 / 닫기 전환 하려면 메뉴 단추입니다. 큰 앱 창에서 창이 열리면 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 속성을 사용하여 이 단추를 숨기도록 선택할 수 있습니다.

### <a name="pane-footer"></a>창 폴더

[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 속성에 추가되는 경우 창 바닥글의 자유 형식 콘텐츠

:::row:::
    :::column:::
    <b>왼쪽된 탐색</b><br>
    ![폴더의 왼쪽된 탐색 창](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>상단 탐색</b><br>
    ![헤더의 상단 탐색 창](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>창 헤더

자유 형식 콘텐츠 창의 헤더에서 [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) 속성에 추가 하는 경우

:::row:::
    :::column:::
    <b>왼쪽된 탐색</b><br>
    ![헤더의 왼쪽된 탐색 창](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>상단 탐색</b><br>
    ![헤더의 상단 탐색 창](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>창 콘텐츠

[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) 속성에 추가 될 때 창에서 자유 형식의 콘텐츠

:::row:::
    :::column:::
    <b>왼쪽된 탐색</b><br>
    ![사용자 지정 contentleft 탐색 창](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>상단 탐색</b><br>
    ![사용자 지정 콘텐츠 상단 탐색 창](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>시각적 스타일

하드웨어 및 소프트웨어 요구 사항이 충족 되 면 NavigationView는 자동으로 해당 창에 [아크릴 재질](../style/acrylic.md) 및 [강조](../style/reveal.md) 왼쪽된 창에만 사용 합니다.

## <a name="header"></a>헤더

![머리글 영역은 navview 일반 이미지](images/nav-header.png)

머리글 영역은 왼쪽된 창 위치에서 탐색 단추와 세로 방향으로 정렬 되며와 창에서 최상위 창 위치 아래에 있습니다. 높이가 52 픽셀로 고정된가 px 합니다. 선택한 탐색 범주의 페이지 제목을 유지하는 것이 목적입니다. 머리글이 페이지 위쪽에 고정되고 콘텐츠 영역에 대한 스크롤 자르기 지점 역할을 합니다.

NavigationView 디스플레이 최소 모드일 때 머리글이 표시 되어야 합니다. 더 큰 창 너비에 사용되는 다른 모드에서 머리글을 숨기도록 선택할 수 있습니다. 이렇게 하려면 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 속성을 **false**로 설정합니다.

## <a name="content"></a>콘텐츠

![콘텐츠 영역의 navview 일반 이미지](images/nav-content.png)

콘텐츠 영역은 선택한 탐색 범주에 대한 대부분의 정보가 표시되는 곳입니다.

NavigationView가 최소 모드이고 여백이 24px일 경우 콘텐츠 영역에 대해 12px 여백이 적당합니다.

## <a name="adaptive-behavior"></a>적응형 동작

NavigationView는 사용 가능한 화면 공간을 기준으로 디스플레이 모드를 자동으로 변경합니다. 적응형 디스플레이 모드 동작을 사용자 지정할 수도 있습니다.

### <a name="default"></a>Default

NavigationView의 기본 적응형 동작 작은 창 너비에 큰 창 너비에는 확장 된 왼쪽된 창, 중간 창 너비에 아이콘 전용 왼쪽된 탐색 창 및 햄버거 메뉴 단추를 표시 하는 것입니다. 적응형 동작에 대 한 창 크기에 대 한 자세한 내용은 [화면 크기 및 중단점을](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)참조 하세요.

![gif leftnav 기본 적응형 동작](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>최소

두 번째 일반적인 적응 패턴 큰 창 너비와 중간 크기 및 작은 창 너비 모두에 햄버거 메뉴에는 확장 된 왼쪽된 창을 사용 하는 것입니다.

![gif leftnav 적응형 동작 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

이 경우를 사용 하는 것이 좋습니다.

- 작은 창 너비에 콘텐츠를 앱에 대 한 더 많은 공간을 원하는 합니다.
- 아이콘이 있는 탐색 범주를 명확 하 게 나타낼 수 없습니다.

### <a name="compact"></a>컴팩트

세 번째 일반적인 적응 패턴 큰 창 너비와 중간 크기 및 작은 창 너비 모두에 아이콘 전용 왼쪽된 탐색 창에는 확장 된 왼쪽된 창을 사용 하는 것입니다. 이 항목의 좋은 예 메일 앱입니다.

![gif leftnav 적응형 동작 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

이 경우를 사용 하는 것이 좋습니다.

- 것 항상 화면에서 모든 탐색 옵션을 표시 하는 것이 중요 합니다.
- 아이콘이 있는 탐색 범주를 명확 하 게 나타낼 수 있습니다.

### <a name="no-adaptive-behavior"></a>적응형 동작 없음

경우에 따라 수 하지 원하는 적응형 동작을 가져오지 전혀 합니다. 항상 되도록 확장, 항상 컴팩트 또는 항상 최소한의 창을 설정할 수 있습니다.

![gif leftnav 적응형 동작 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>왼쪽된 탐색 위쪽

큰 창 크기와 작은에서 왼쪽된 탐색에서 상단 탐색을 사용 하는 것이 좋습니다 창 크기는 경우:

- 이 설정 앱에서 하나의 범주는 화면에 맞지 않는, 바람직한 수 있도록 왼쪽된 탐색을 축소 하 함께 표시할 중요 최상위 수준 탐색 범주 동일 하 게 집합이 있습니다.
- 작은 창 크기에 최대한 많은 콘텐츠 공간으로 유지 하고자 합니다.

다음 예제를 참조하세요.

![gif 위쪽 또는 왼쪽 탐색 적응형 동작 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>

    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>
</Grid>

```

경우에 따라 앱 상단 창 및 왼쪽된 창에 서로 다른 데이터 바인딩 해야 합니다. 종종 왼쪽된 창에 더 많은 탐색 요소가 포함 됩니다.

다음 예제를 참조하세요.

![gif 위쪽 또는 왼쪽 탐색 적응형 동작 2](images/navigation-top-to-left2.png)

```xaml
<Page >
    <Page.Resources>
        <DataTemplate x:name="navItem_top_temp" x:DataType="models:Item">
            <NavigationViewItem Background= Icon={x:Bind TopIcon}, Content={x:Bind TopContent}, Visibility={x:Bind TopVisibility} />
        </DataTemplate>

        <DataTemplate x:name="navItem_temp" x:DataType="models:Item">
            <NavigationViewItem Icon={x:Bind Icon}, Content={x:Bind Content}, Visibility={x:Bind Visibility} />
        </DataTemplate>
        
        <services:NavViewDataTemplateSelector x:Key="navview_selector" 
              NavItemTemplate="{StaticResource navItem_temp}" 
              NavItemTopTemplate="{StaticResource navItem_top_temp}" 
              NavPaneDisplayMode="{x:Bind NavigationViewControl.PaneDisplayMode}">
        </services:NavViewDataTemplateSelector>
    </Page.Resources>

    <Grid >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavView x:Name='NavigationViewControl' MenuItemsSource={x:Bind items}   
                 PanePosition = "Top" MenuItemTemplateSelector="navview_selector" />
    </Grid>
</Page>

```

```csharp
ObservableCollection<Item> items = new ObservableCollection<Item>();
items.Add(new Item() {
    Content = "Aa",
    TopContent ="A",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Bb",
    TopContent = "B",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Cc",
    TopContent = "C",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});

public class NavViewDataTemplateSelector : DataTemplateSelector
    {
        public DataTemplate NavItemTemplate { get; set; }

        public DataTemplate NavItemTopTemplate { get; set; }    

     public NavigationViewPaneDisplayMode NavPaneDisplayMode { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            Item currItem = item as Item;
            if (NavPaneDisplayMode == NavigationViewPanePosition.Top)
                return NavItemTopTemplate;
            else 
                return NavItemTemplate;
        }   

    }

```

## <a name="interaction"></a>조작

사용자가 창에서 탐색 항목을 탭하면 NavigationView가 해당 항목을 선택된 상태로 표시하고 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 이벤트를 발생시킵니다. 탭을 누르면 새 항목이 선택되는 경우에도 NavigationView가 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 이벤트를 발생시킵니다.

앱은 사용자 상호 작용에 응답하여 적절한 정보로 머리글과 콘텐츠를 업데이트하는 역할을 합니다. 또한 프로그래밍 방식으로 탐색 항목에서 콘텐츠로 [포커스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.FocusState)를 이동하는 것이 좋습니다. 로드에 초기 포커스를 설정하여 사용자 흐름을 간소화하고 포커스가 이동하는 예상 키보드 수를 최소화합니다.

### <a name="tabs"></a>탭

탭 모델 선택 및 포커스 연결 됩니다. 에 전환이 포커스 선택을 이동은 정상적으로 동작 합니다. 에 아래 예에서는 오른쪽 화살표 지정 선택 표시기 디스플레이에서으로 이동 돋보기 합니다. 사용으로 [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus) 속성을 설정 하 여이 얻을 수 있습니다.

![최상위 텍스트 전용 navview의 스크린샷](images/nav-tabs.png)

다음은 XAML 예제는:

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

탭 선택을 변경 하는 경우 콘텐츠를 교체할 False로 설정 FrameNavigationOptions.IsNavigationStackEnabled 사용 하 여 프레임의 [NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType) 메서드를 사용할 수 있으며 NavigateOptions.TransitionInfoOverride는 적절 한 왼쪽-오른쪽으로 설정 애니메이션입니다. 예를 들어, [코드 예제에서](#code-example) 는 아래를 참조 하세요.

기본 스타일을 변경 하려는 경우 NavigationView의 [MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle) 속성을 재정의할 수 있습니다. 또한 다른 데이터 템플릿을 지정 하려면 [MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate) 속성을 설정할 수 있습니다.

## <a name="backwards-navigation"></a>뒤로 탐색

NavigationView에는 다음과 같은 속성에서 사용할 수 있는 뒤로 단추가 기본적으로 포함되어 있습니다.

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)의 기본값은 NavigationViewBackButtonVisible 열거값 및 "자동"입니다. 뒤로 단추를 표시하거나 숨기는 데 사용됩니다. 이 단추가 보이지 않으면 뒤로 단추를 그리기 위한 공간이 축소됩니다.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)는 기본값이 false이고 뒤로 단추 상태를 전환하는 데 사용할 수 있습니다.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)는 사용자가 뒤로 단추를 클릭할 때 발생합니다.
    - 최소/컴팩트 모드에서 NavigationView.Pane이 플라이아웃으로 열릴 때 뒤로 단추를 클릭하면 창이 닫히고 **PaneClosing** 이벤트가 발생합니다.
    - IsBackEnabled가 false이면 이벤트가 발생하지 않습니다.

:::row:::
    :::column:::
    <b>왼쪽된 탐색</b><br>
    ![NavigationView의 뒤로 단추 탐색 왼쪽](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>상단 탐색</b><br>
    ![상단 탐색에 NavigationView 뒤로 단추](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>코드 예제

> [!NOTE]
> NavigationView는 앱의 루트 컨테이너 역할을 합니다. 이 컨트롤은 앱 창의 전체 너비와 높이에 맞게 확장되도록 설계되었습니다.
[CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 및 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 속성을 사용하여 탐색 보기에서 디스플레이 모드가 변경되는 너비를 재정의할 수 있습니다.

다음은 종단 간 예로 상단 탐색 창에서 큰 창 크기와 작은 창 크기의 왼쪽된 탐색 창 NavigationView 통합할 수 있습니다.

이 샘플에서는 자주 새 탐색 범주를 선택 하는 최종 사용자가 기대 하므로 했습니다.

- [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) 속성 설정
- 탐색 스택에 추가 하지 않는 프레임 탐색을 사용 합니다.
- 게임 패드 왼쪽/오른쪽 범퍼 앱의 최상위 수준 탐색 범주를 탐색 하는 경우를 나타내는 데 사용 되는 [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) 속성에서 기본값을 유지 합니다. 기본값은 "WhenSelectionFollowsFocus". 다른 가능한 값은 "항상" 및 "없음"입니다.

또한 뒤로 NavigationView의 뒤로 단추를 사용 하 여 탐색을 구현 하는 방법을 보여 줍니다.

샘플 내용에 대 한 기록을 다음과 같습니다.

![NavigationView 종단 간 샘플](images/nav-code-example.gif)

샘플 코드는 다음과 같습니다.

> [!NOTE]
> [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 사용 하는 경우 도구 키트에 대 한 참조를 추가 해야 합니다. `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`.

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavigationView x:Name="NavView"
                    SelectionFollowsFocus="Enabled"
                    ItemInvoked="NavView_ItemInvoked"
                    IsSettingsVisible="True"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested"
                    Header="Welcome">

            <NavigationView.MenuItems>
                <NavigationViewItem Content="Home" x:Name="home" Tag="home">
                    <NavigationViewItem.Icon>
                        <FontIcon Glyph="&#xE10F;"/>
                    </NavigationViewItem.Icon>
                </NavigationViewItem>
                <NavigationViewItemSeparator/>
                <NavigationViewItemHeader Content="Main pages"/>
                <NavigationViewItem Icon="AllApps" Content="Apps" x:Name="apps" Tag="apps"/>
                <NavigationViewItem Icon="Video" Content="Games" x:Name="games" Tag="games"/>
                <NavigationViewItem Icon="Audio" Content="Music" x:Name="music" Tag="music"/>
            </NavigationView.MenuItems>

            <NavigationView.AutoSuggestBox>
                <AutoSuggestBox x:Name="ASB" QueryIcon="Find"/>
            </NavigationView.AutoSuggestBox>

            <NavigationView.HeaderTemplate>
                <DataTemplate>
                    <Grid Margin="24,10,0,0">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Text="Welcome"/>
                        <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            VerticalAlignment="Top"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                            <AppBarButton Label="Refresh" Icon="Refresh"/>
                            <AppBarButton Label="Import" Icon="Import"/>
                        </CommandBar>
                    </Grid>
                </DataTemplate>
            </NavigationView.HeaderTemplate>

            <Frame x:Name="ContentFrame" Margin="24"/>

        </NavigationView>
    </Grid>
</Page>
```

> [!NOTE]
> [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 사용 하는 경우 도구 키트에 대 한 참조를 추가 해야 합니다. `using MUXC = Microsoft.UI.Xaml.Controls;`.

```csharp
// List of ValueTuple holding the Navigation Tag and the relative Navigation Page 
private readonly IList<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon(Symbol.Folder),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default: you need to specify it
    NavView_Navigate("home");

    // Add keyboard accelerators for backwards navigation
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

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{

    if (args.IsSettingsInvoked)
        ContentFrame.Navigate(typeof(SettingsPage));
    else
    {
        // Getting the Tag from Content (args.InvokedItem is the content of NavigationViewItem)
        var navItemTag = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(i => args.InvokedItem.Equals(i.Content))
            .Tag.ToString();

        NavView_Navigate(navItemTag);
    }
}

private void NavView_Navigate(string navItemTag)
{
    var item = _pages.First(p => p.Tag.Equals(navItemTag));
    ContentFrame.Navigate(item.Page);
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == NavigationViewDisplayMode.Compact ||
        NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag
        NavView.SelectedItem = (NavigationViewItem)NavView.SettingsItem;
    }
    else
    {
        var item = _pages.First(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));
    }
}
```

## <a name="customizing-backgrounds"></a>배경 사용자 지정

NavigationView의 주요 영역 배경을 변경하려면, `Background` 속성을 기본 설정 브러시로 설정하세요.

위쪽, 최소 또는 컴팩트 모드의 NavigationView 되 면 창의 배경이 인 앱 아크릴을 보여 줍니다. 이 동작을 업데이트하거나 창 아크릴의 모양을 사용자 지정하려면 App.xaml에서 두 테마 리소스를 덮어써서 수정합니다.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewTopPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="scroll-content-under-top-pane"></a>상단 창에서 스크롤 콘텐츠

원활한 모양을 + 느낌, 앱은 ScrollViewer를 사용 하는 페이지 이며 탐색 창 맨 위에 배치 하는 경우 권장 상단 탐색 창 아래 콘텐츠 스크롤 필요 합니다.

이를 true로 관련 ScrollViewer에서 [CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds) 속성을 설정 하 여 얻을 수 있습니다.

![navview 스크롤 탐색 창](images/nav-scroll-content.png)

앱에 매우 긴 스크롤 콘텐츠에 있는 경우 상단 탐색 창에 연결 하 고 매끄러운 화면을 형성 하는 고정 헤더 통합을 고려 하는 것이 좋습니다. 

![navview 스크롤 고정 헤더](images/nav-scroll-stickyheader.png)

NavigationView에서 [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) 속성을 설정 하 여이 얻을 수 있습니다. 

경우에 따라 사용자 아래로 스크롤 하는 경우 NavigationView에서 [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) 속성을 false로 설정 하 여 탐색 창, 숨기기는 것이 좋습니다.

![navview 스크롤 숨기기 탐색](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>관련 항목

- [NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [마스터/세부](master-details.md)
- [피벗 컨트롤](tabs-pivot.md)
- [탐색 기본 사항](../basics/navigation-basics.md)
- [UWP용 흐름 디자인 개요](../fluent-design-system/index.md)