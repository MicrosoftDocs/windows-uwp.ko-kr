---
author: Jwmsft
Description: "화면 공간을 절약하는 동시에 최상위 탐색을 배치하는 컨트롤입니다."
title: "탐색 보기"
ms.assetid: 
label: Navigation view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 98ab8a95288e5a0225a2185f7ce037e16209d173
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2017
---
# <a name="navigation-view"></a>탐색 보기

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

탐색 보기는 축소 가능한 탐색 메뉴를 통해 앱의 최상위 영역에 대한 일반 수직 레이아웃을 제공합니다. 이 컨트롤은 탐색 창, 햄버거 메뉴, 패턴을 구현하도록 설계되었으며 여러 창 크기에 맞춰 레이아웃을 자동으로 조정합니다.

> **중요 API**: [NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [NavigationViewItem 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [NavigationViewDisplayMode 열거](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![NavigationView의 예](images/navview_wireframe.png)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

NavigationView는 다음에 대해 제대로 작동합니다.

-  유형이 비슷한 최상위 수준 탐색 항목이 여러 개인 앱. 예를 들면 미식축구, 야구, 농구, 축구 등과 같은 범주가 있는 스포츠 앱입니다.
-  앱 전체에서 일관된 탐색 환경 제공. 창에는 작업이 아닌 탐색 요소만 포함해야 합니다.
-  최상위 수준 탐색 범주의 중간에서 높은 쪽(5-10)
-  더 작은 창의 화면 공간을 유지합니다.

탐색 보기는 사용할 수 있는 여러 탐색 요소 중 하나일 뿐입니다. 탐색 패턴 및 다른 탐색 요소에 대해 자세히 알아보려면 [UWP(유니버설 Windows 플랫폼) 앱용 탐색 디자인 기본 사항](../layout/navigation-basics.md)을 참조하세요.

SplitView 및 ListView를 사용하여 탐색 창 패턴을 구성하는 방법의 코드 샘플이 필요하면 GitHub에서 [XAML 탐색 솔루션](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/XamlNavigation)을 다운로드하세요.

## <a name="navigationview-parts"></a>NavigationView 요소
컨트롤은 크게 왼쪽의 탐색용 창과 오른쪽의 머리글 및 콘텐츠의 세 섹션으로 나뉩니다.

![NavigationView 섹션](images/navview_sections.png)

### <a name="pane"></a>창

탐색 창에 다음을 포함할 수 있습니다.

- 특정 페이지로 이동하기 위한 [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)
- 탐색 항목 그룹화를 위한 [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)
- 항목 그룹에 레이블을 지정하기 위한 [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 형태의 머리글
- [앱 설정](../app-settings/app-settings-and-data.md)에 대한 선택적 진입점 설정 항목을 숨기려면 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsSettingsVisible) 속성을 사용
- [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_PaneFooter) 속성에 추가되는 경우 창 바닥글의 자유 형식 콘텐츠

기본 제공 탐색("햄버거") 단추로 사용자가 창을 열고 닫을 수 있습니다. 큰 앱 창에서 창이 열리면 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsPaneToggleButtonVisible) 속성을 사용하여 이 단추를 숨기도록 선택할 수 있습니다.

### <a name="header"></a>머리글

머리글 영역은 탐색 단추와 세로 방향으로 정렬되며 높이가 고정되어 있습니다. 선택한 탐색 범주의 페이지 제목을 유지하는 것이 목적입니다. 머리글이 페이지 위쪽에 고정되고 콘텐츠 영역에 대한 스크롤 자르기 지점 역할을 합니다.

NavigationView가 최소 모드일 때 머리글이 표시되어야 합니다. 더 큰 창 너비에 사용되는 다른 모드에서 머리글을 숨기도록 선택할 수 있습니다. 이렇게 하려면 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_AlwaysShowHeader) 속성을 **false**로 설정합니다.

### <a name="content"></a>콘텐츠

콘텐츠 영역은 선택한 탐색 범주에 대한 대부분의 정보가 표시되는 곳입니다. 여기에는 하나 이상의 요소가 포함될 수 있으며 [피벗](tabs-pivot.md)과 같은 추가 하위 수준 탐색에 유용합니다.

NavigationView가 최소 모드이고 여백이 24px일 경우 콘텐츠 쪽에 12px 여백이 적당합니다.

## <a name="visual-style"></a>시각적 스타일

<div class="microsoft-internal-note">
[UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav?t=Fluent%20Design%20System%7CControls%20%26%20Patterns%7CNavigationView)에서 이 컨트롤에 대한 검토를 사용할 수 있습니다.<br/><br/>
</div>

탐색 항목은 선택됨, 비활성화됨, 포인터 가리키기, 눌림 및 중요 시각적 상태를 지원합니다.

![NavigationView 항목 상태: 비활성화됨, 포인터 가리키기, 눌림, 중요](images/navview_item-states.png)

하드웨어 및 소프트웨어 요구 사항이 충족되면 NavigationView 창에 자동으로 새 [아크릴 재질](../style/acrylic.md) 및 [강조 표시](../style/reveal.md)가 사용됩니다.


## <a name="navigationview-modes"></a>NavigationView 모드
NavigationView 창을 열거나 닫을 수 있으며 세 가지 표시 모드 옵션이 있습니다.
-  **최소** 필요에 따라 창이 표시되고 숨겨지는 동안 햄버거 단추만 유지됩니다.
-  **컴팩트** 창이 항상 전체 너비로 열 수 있는 좁은 조각으로 나타납니다.
-  **확장** 창이 콘텐츠 옆에 열려 있습니다. 햄버거 단추를 활성화하여 창을 닫으면 창의 너비가 좁은 은색으로 변합니다.

기본적으로 시스템에서 자동으로 컨트롤에 사용 가능한 화면 공간을 기준으로 최적의 디스플레이 모드를 선택합니다. 이 설정을 재정의할 수 있습니다. 자세한 내용은 다음 섹션을 참조하세요.

### <a name="minimal"></a>최소

![닫힌 창과 열린 창이 표시된 최소 모드의 NavigationView](images/navview_minimal.png)

-  닫혀 있을 때는 창이 기본적으로 숨겨지고 탐색 단추만 표시됩니다.
-  화면 공간을 절약하는 요청 시 탐색을 제공합니다. 휴대폰 및 패블릿에서 실행하는 앱에 적합합니다.
-  탐색 단추를 누르면 머리글이나 콘텐츠 위에 오버레이를 그리는 창이 열리고 닫힙니다. 콘텐츠가 재배치되지 않습니다.
-  열려 있으면 창이 투명하며 선택, 뒤로 단추 누름, 창 바깥쪽 탭 등의 빠른 해제 제스처로 닫을 수 있습니다.
-  창의 오버레이가 열리면 선택한 항목이 표시됩니다.
-  요구 사항이 충족되면 열려 있는 창의 배경이 [앱 내 아크릴](../style/acrylic.md#acrylic-blend-types)입니다.
-  기본적으로 전체 너비가 640px 이하일 경우 NavigationView는 최소 모드입니다.

### <a name="compact"></a>컴팩트

![닫힌 창과 열린 창이 표시된 컴팩트 모드의 NavigationView](images/navview_compact.png)

-  닫혀 있을 때는 아이콘과 탐색 단추만 표시하는 창의 수직 조각을 볼 수 있습니다.
-  화면 공간을 작게 사용하는 동안 선택한 위치를 일부 보여 줍니다.
-  이 모드는 태블릿과 [3m 환경](../input-and-devices/designing-for-tv.md)과 같은 중간 화면에 더 적합합니다.
-  탐색 단추를 누르면 머리글이나 콘텐츠 위에 오버레이를 그리는 창이 열리고 닫힙니다. 콘텐츠가 재배치되지 않습니다.
-  머리글은 불필요하며 콘텐츠에 더 많은 세로 공간을 제공하기 위해 숨길 수 있습니다.
-  선택된 항목은 시각적 표시기로 사용자가 탐색 트리 내에 있는 위치를 강조 표시합니다.
-  요구 사항이 충족되면 창의 배경이 [앱 내 아크릴](../style/acrylic.md#acrylic-blend-types)입니다.
-  기본적으로 전체 너비가 641px에서 1007px 사이일 경우 NavigationView는 컴팩트 모드입니다.

### <a name="expanded"></a>확장

![열린 창이 표시된 확장 모드의 NavigationView](images/navview_expanded.png)

-  기본적으로 창은 열려 있습니다. 이 모드는 큰 화면에 더욱 적합합니다.
-  창이 사용 가능한 공간 내에서 재배치되는 머리글 및 콘텐츠와 함께 나란히 그려집니다.
-  탐색 단추를 사용하여 창이 닫혀 있는 경우 창은 머리글 및 콘텐츠와 나란히 좁은 은색으로 표시됩니다.
-  머리글은 불필요하며 콘텐츠에 더 많은 세로 공간을 제공하기 위해 숨길 수 있습니다.
-  선택된 항목은 시각적 표시기로 사용자가 탐색 트리 내에 있는 위치를 강조 표시합니다.
-  요구 사항이 충족되면 창의 배경이 [백그라운드 아크릴](../style/acrylic.md#acrylic-blend-types)을 사용하여 그려집니다.
-  기본적으로 전체 너비가 1007px보다 넓을 경우 NavigationView는 확장 모드입니다.

## <a name="overriding-the-default-adaptive-behavior"></a>기본 적응형 동작 재정의

사용 가능한 화면 공간을 기준으로 탐색 보기가 자동으로 표시 모드를 변경합니다.
[!NOTE] NavigationView는 앱의 루트 컨테이너 역할을 합니다. 이 컨트롤은 앱 창의 전체 너비와 높이에 맞게 확장되도록 설계되었습니다.
[CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_CompactModeThresholdWidth) 및 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ExpandedModeThresholdWidth) 속성을 사용하여 탐색 보기가 디스플레이 모드를 변경하는 너비를 재정의할 수 있습니다. 표시 모드 동작을 사용자 지정하려는 경우를 설명하는 다음 시나리오를 고려하세요.

-  **자주 탐색** 사용자가 앱 영역 사이를 다소 자주 탐색할 경우로 예상되는 경우 보기의 창을 더 좁은 창 너비로 유지하는 것이 좋습니다. 노래/앨범/아티스트 탐색 영역이 있는 음악 앱은 280px 창을 선택할 수 있으며 앱 창이 560px보다 넓을 경우 확장 모드로 유지됩니다.
```xaml
<NavigationView OpenPaneLength=”280” CompactModeThresholdWidth="560" ExpandedModeThresholdWidth=”560”/>
```
-    **Rare navigation** 사용자가 앱 영역 사이를 매우 드물게 탐색할 경우로 예상되는 경우 창을 더 넓은 창 너비로 숨겨 놓는 것이 좋습니다. 앱이 1080p 디스플레이에서 최대화될 때도 여러 레이아웃의 계산기 앱은 최소 모드로 유지할 수 있습니다.
```xaml
<NavigationView CompactModeThresholdWidth=”1920” ExpandedModeThresholdWidth=”1920”/>
```
-    **아이콘 명확성** 앱의 탐색 영역이 의미 있는 아이콘을 표시하기에 충분하지 않은 경우 컴팩트 모드를 사용하지 않는 것이 좋습니다. 컬렉션/앨범/폴더 탐색 영역이 있는 이미지 보기 앱은 NavigationView를 좁은 너비와 중간 너비에서는 최소 모드로 표시하고 넓은 너비에서는 확장 모드로 표시할 수 있습니다.
```xaml
<NavigationView CompactModeThresholdWidth=”1008”/>
```

## <a name="interaction"></a>조작

사용자가 창에서 탐색 범주를 탭하면 NavigationView가 해당 항목을 선택된 상태로 표시하고 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ItemInvoked) 이벤트를 발생시킵니다. 탭을 누르면 새 항목이 선택되는 경우에도 NavigationView가 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_SelectionChanged) 이벤트를 발생시킵니다. 앱은 사용자 상호 작용에 응답하여 적절한 정보로 머리글과 콘텐츠를 업데이트하는 역할을 합니다. 또한 프로그래밍 방식으로 탐색 항목에서 콘텐츠로 포커스를 이동하는 것이 좋습니다. 로드에 초기 포커스를 설정하여 사용자 흐름을 간소화하고 포커스가 이동하는 예상 키보드 수를 최소화합니다.

## <a name="how-to-use-navigationview"></a>NavigationView를 사용하는 방법

다음은 앱에 NavigationView를 통합하는 방법의 간단한 예입니다.

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <NavigationView x:Name="NavView"
                    ItemInvoked="NavView_ItemInvoked"
                    Loaded="NavView_Loaded">
    <!-- Load NavigationViewItems in NavView_Loaded. -->

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Margin="12,0"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                        <AppBarButton Label="Refresh" Icon="Refresh"/>
                        <AppBarButton Label="Import" Icon="Import"/>
                    </CommandBar>
                </Grid>
            </DataTemplate>
        </NavigationView.HeaderTemplate>

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame">
            <Frame.ContentTransitions>
                <TransitionCollection>
                    <NavigationThemeTransition/>
                </TransitionCollection>
            </Frame.ContentTransitions>
        </Frame>

    </NavigationView>
</Page>
```

```csharp
private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Apps", Icon = new SymbolIcon(Symbol.AllApps), Tag = "apps" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Games", Icon = new SymbolIcon(Symbol.Video), Tag = "games" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Music", Icon = new SymbolIcon(Symbol.Audio), Tag = "music" });
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    foreach (NavigationViewItem item in NavView.MenuItems)
    {
        if (item.Tag.ToString() == "play")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        switch ((args.InvokedItem as NavigationViewItem).Tag)
        {
          case "apps":
              ContentFrame.Navigate(typeof(AppsPage));
              break;

          case "games":
              ContentFrame.Navigate(typeof(GamesPage));
              break;

          case "music":
              ContentFrame.Navigate(typeof(MusicPage));
              break;

          case "content":
              ContentFrame.Navigate(typeof(MyContentPage));
              break;
        }
    }
}
```

## <a name="navigation"></a>탐색

NavigationView는 자동으로 앱의 제목 표시줄에 뒤로 단추를 표시하거나 뒤로 스택에 콘텐츠를 추가하지 않습니다. 컨트롤은 소프트웨어 또는 하드웨어 뒤로 단추 누름에 자동으로 응답하지 않습니다. 이 항목에 대한 자세한 내용과 앱에 대한 탐색 지원을 추가하는 방법은 [기록 및 뒤로 탐색](../layout/navigation-history-and-backwards-navigation.md) 섹션을 참조하세요.


## <a name="related-topics"></a>관련 항목

* [NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
* [마스터/세부](master-details.md)
* [피벗 컨트롤](tabs-pivot.md)
* [탐색 기본 사항](../layout/navigation-basics.md)
 

 
