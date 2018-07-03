---
author: serenaz
Description: Control that provides top-level app navigation with an automatically adapting, collapsible left navigation menu
title: 탐색 보기
ms.assetid: ''
label: Navigation view
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: vasriram
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c7817bf7ff60a52ea48c988bdebd6d4d2eeacdb7
ms.sourcegitcommit: 618741673a26bd718962d4b8f859e632879f9d61
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "1992152"
---
# <a name="navigation-view"></a>탐색 보기

탐색 보기 컨트롤은 앱의 최상위 수준 탐색을 위해 축소 가능한 탐색 메뉴를 제공합니다. 이 컨트롤은 탐색 창(햄버거 메뉴) 패턴을 구현하며 여러 창 크기에 맞춰 창의 디스플레이 모드를 자동으로 조정합니다.

> **중요 API**: [NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [NavigationViewItem 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [NavigationViewDisplayMode 열거](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![NavigationView의 예](images/navview_wireframe.png)

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev010/player]

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

NavigationView는 다음에서 제대로 작동합니다.

-  유형이 비슷한 최상위 수준 탐색 항목이 여러 개인 앱 (예를 들면 미식축구, 야구, 농구, 축구와 같은 범주가 있는 스포츠 앱)
-  최상위 수준 탐색 범주의 중간에서 높은 쪽(5-10)
-  일관된 탐색 환경 제공 창에는 작업이 아닌 탐색 요소만 포함해야 합니다.
-  더 작은 창의 화면 공간을 유지합니다.

NavigationView는 사용할 수 있는 몇 가지 탐색 요소 중 하나일 뿐입니다. 다른 탐색 패턴 및 요소에 대한 자세한 내용은 [탐색 디자인 기초](../basics/navigation-basics.md)를 참조하세요.

NavigationView 컨트롤에는 간단한 탐색 창 패턴을 구현하는 동작들이 다수 포함되어 있습니다. 탐색을 위해 NavigationView에서 지원되지 않는 보다 복잡한 동작이 필요할 경우에는 [마스터/세부 정보](master-details.md) 패턴을 대신 고려할 수 있습니다.

## <a name="examples"></a>예
<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/NavigationView">앱을 열고 작동 중인 NavigationView를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="navigationview-sections"></a>NavigationView 섹션

![NavigationView 섹션](images/navview_sections.png)

### <a name="pane"></a>창

기본 제공 탐색("햄버거") 단추로 사용자가 창을 열고 닫을 수 있습니다. 큰 앱 창에서 창이 열리면 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 속성을 사용하여 이 단추를 숨기도록 선택할 수 있습니다. 햄버거 옆에 있는 텍스트 레이블은 [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 속성입니다.

기본 제공되는 뒤로 단추는 창의 왼쪽 위 모서리에 표시됩니다. NavigationView 컨트롤은 자동으로 백 스택에 콘텐츠를 추가하지 않지만, 뒤로 탐색을 사용하고 싶으면 [뒤로 탐색](#backwards-navigation) 섹션을 참조하세요.

NavigationView 창에는 다음이 포함될 수도 있습니다.

- 특정 페이지로 이동하기 위한 [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)
- 탐색 항목 그룹화를 위한 [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)
- 항목 그룹에 레이블을 지정하기 위한 [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 형태의 머리글
- 앱 수준 검색을 허용할 [AutoSuggestBox](auto-suggest-box.md) 옵션
- [앱 설정](../app-settings/app-settings-and-data.md)에 대한 선택적 진입점 설정 항목을 숨기려면 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 속성을 사용
- [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 속성에 추가되는 경우 창 바닥글의 자유 형식 콘텐츠

#### <a name="visual-style"></a>시각적 스타일

NavigationView 항목은 선택됨, 비활성화됨, 포인터 가리키기, 누름, 포커스가 있는 시각적 상태를 지원합니다.

![NavigationView 항목 상태: 비활성화됨, 포인터 가리키기, 눌림, 중요](images/navview_item-states.png)

하드웨어 및 소프트웨어 요구 사항이 충족되면 NavigationView 창에 자동으로 새 [아크릴 재질](../style/acrylic.md) 및 [강조 표시](../style/reveal.md)가 사용됩니다.

### <a name="header"></a>머리글

머리글 영역은 탐색 단추와 세로 방향으로 정렬되며 높이가 52 픽셀로 고정되어 있습니다. 선택한 탐색 범주의 페이지 제목을 유지하는 것이 목적입니다. 머리글이 페이지 위쪽에 고정되고 콘텐츠 영역에 대한 스크롤 자르기 지점 역할을 합니다.

NavigationView가 최소 모드일 때 머리글이 표시되어야 합니다. 더 큰 창 너비에 사용되는 다른 모드에서 머리글을 숨기도록 선택할 수 있습니다. 이렇게 하려면 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 속성을 **false**로 설정합니다.

### <a name="content"></a>콘텐츠

콘텐츠 영역은 선택한 탐색 범주에 대한 대부분의 정보가 표시되는 곳입니다. 

NavigationView가 최소 모드이고 여백이 24px일 경우 콘텐츠 영역에 대해 12px 여백이 적당합니다.

## <a name="navigationview-display-modes"></a>NavigationView 디스플레이 모드
NavigationView 창을 열거나 닫을 수 있으며 세 가지 디스플레이 모드 옵션이 있습니다.
-  **최소** 필요에 따라 창이 표시되고 숨겨지는 동안 햄버거 단추만 유지됩니다.
-  **컴팩트** 창이 항상 전체 너비로 열 수 있는 좁은 조각으로 나타납니다.
-  **확장** 창이 콘텐츠 옆에 열려 있습니다. 햄버거 단추를 활성화하여 창을 닫으면 창의 너비가 좁은 은색으로 변합니다.

기본적으로 시스템에서 자동으로 컨트롤에 사용 가능한 화면 공간을 기준으로 최적의 디스플레이 모드를 선택합니다 (이 설정을 [재정의](#overriding-the-default-adaptive-behavior)하는 것이 가능).

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
-  실제 면적이 작은 화면에서 선택한 위치를 몇 가지 방법으로 나타낼 수 있습니다.
-  이 모드는 태블릿과 [3m 환경](../devices/designing-for-tv.md)과 같은 중간 화면에 더 적합합니다.
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

### <a name="overriding-the-default-adaptive-behavior"></a>기본 적응형 동작 재정의

NavigationView는 사용 가능한 화면 공간을 기준으로 디스플레이 모드를 자동으로 변경합니다.

> [!NOTE]
> NavigationView는 앱의 루트 컨테이너 역할을 합니다. 이 컨트롤은 앱 창의 전체 너비와 높이에 맞게 확장되도록 설계되었습니다.
[CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 및 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 속성을 사용하여 탐색 보기에서 디스플레이 모드가 변경되는 너비를 재정의할 수 있습니다.

표시 모드 동작을 사용자 지정하려는 경우를 설명하는 다음 시나리오를 고려하세요.

- **자주 탐색** 사용자가 앱 영역 사이를 다소 자주 탐색할 경우로 예상되는 경우 보기의 창을 더 좁은 창 너비로 유지하는 것이 좋습니다. 노래/앨범/아티스트 탐색 영역이 있는 음악 앱은 280px 창을 선택할 수 있으며 앱 창이 560px보다 넓을 경우 확장 모드로 유지됩니다.
```xaml
<NavigationView OpenPaneLength="280" CompactModeThresholdWidth="560" ExpandedModeThresholdWidth="560"/>
```

- **Rare navigation** 사용자가 앱 영역 사이를 매우 드물게 탐색할 경우로 예상되는 경우 창을 더 넓은 창 너비로 숨겨 놓는 것이 좋습니다. 앱이 1080p 디스플레이에서 최대화될 때도 여러 레이아웃의 계산기 앱은 최소 모드로 유지할 수 있습니다.
```xaml
<NavigationView CompactModeThresholdWidth="1920" ExpandedModeThresholdWidth="1920"/>
```

- **아이콘 명확성** 앱의 탐색 영역이 의미 있는 아이콘을 표시하기에 충분하지 않은 경우 컴팩트 모드를 사용하지 않는 것이 좋습니다. 컬렉션/앨범/폴더 탐색 영역이 있는 이미지 보기 앱은 NavigationView를 좁은 너비와 중간 너비에서는 최소 모드로 표시하고 넓은 너비에서는 확장 모드로 표시할 수 있습니다.
```xaml
<NavigationView CompactModeThresholdWidth="1008"/>
```

## <a name="interaction"></a>조작

사용자가 창에서 탐색 항목을 탭하면 NavigationView가 해당 항목을 선택된 상태로 표시하고 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 이벤트를 발생시킵니다. 탭을 누르면 새 항목이 선택되는 경우에도 NavigationView가 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 이벤트를 발생시킵니다. 

앱은 사용자 상호 작용에 응답하여 적절한 정보로 머리글과 콘텐츠를 업데이트하는 역할을 합니다. 또한 프로그래밍 방식으로 탐색 항목에서 콘텐츠로 [포커스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.FocusState)를 이동하는 것이 좋습니다. 로드에 초기 포커스를 설정하여 사용자 흐름을 간소화하고 포커스가 이동하는 예상 키보드 수를 최소화합니다.

## <a name="backwards-navigation"></a>뒤로 탐색
NavigationView에는 다음과 같은 속성에서 사용할 수 있는 뒤로 단추가 기본적으로 포함되어 있습니다.
- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)의 기본값은 NavigationViewBackButtonVisible 열거값 및 "자동"입니다. 뒤로 단추를 표시하거나 숨기는 데 사용됩니다. 이 단추가 보이지 않으면 뒤로 단추를 그리기 위한 공간이 축소됩니다.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)는 기본값이 false이고 뒤로 단추 상태를 전환하는 데 사용할 수 있습니다.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)는 사용자가 뒤로 단추를 클릭할 때 발생합니다.
    - 최소/컴팩트 모드에서 NavigationView.Pane이 플라이아웃으로 열릴 때 뒤로 단추를 클릭하면 창이 닫히고 **PaneClosing** 이벤트가 발생합니다.
    - IsBackEnabled가 false이면 이벤트가 발생하지 않습니다.

![NavigationView 뒤로 단추](../basics/images/back-nav/NavView.png)

## <a name="code-example"></a>코드 예제

다음은 앱에 NavigationView를 통합하는 방법의 간단한 예입니다. 

NavigationView의 뒤로 단추를 통해 뒤로 탐색을 실행하는 방법이 나와 있습니다. NavigationView의 뒤로 탐색 속성을 사용하려면 [Windows 10 Insider Preview(v10.0.17110.0에 도입)](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)가 필요합니다.

또한 `x:Uid`로 탐색 항목 콘텐츠 문자열을 지역화하는 방법을 보여줍니다. 지역화에 대한 자세한 내용은 [UI에서 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)를 참조하세요.

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
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested">

        <NavigationView.MenuItems>
            <NavigationViewItem x:Uid="HomeNavItem" Content="Home" Tag="home">
                <NavigationViewItem.Icon>
                    <FontIcon Glyph="&#xE10F;"/>
                </NavigationViewItem.Icon>
            </NavigationViewItem>
            <NavigationViewItemSeparator/>
            <NavigationViewItemHeader Content="Main pages"/>
            <NavigationViewItem x:Uid="AppsNavItem" Icon="AllApps" Content="Apps" Tag="apps"/>
            <NavigationViewItem x:Uid="GamesNavItem" Icon="Video" Content="Games" Tag="games"/>
            <NavigationViewItem x:Uid="MusicNavItem" Icon="Audio" Content="Music" Tag="music"/>
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

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame" Margin="24">
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
    // you can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
    { Content = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    // set the initial SelectedItem 
    foreach (NavigationViewItemBase item in NavView.MenuItems)
    {
        if (item is NavigationViewItem && item.Tag.ToString() == "home")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
            
    ContentFrame.Navigated += On_Navigated;

    // add keyboard accelerators for backwards navigation
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
    
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{  
    if (args.IsSettingsInvoked)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        // find NavigationViewItem with Content that equals InvokedItem
        var item = sender.MenuItems.OfType<NavigationViewItem>().First(x => (string)x.Content == (string)args.InvokedItem);
        NavView_Navigate(item as NavigationViewItem);
    }
}

private void NavView_Navigate(NavigationViewItem item)
{
    switch (item.Tag)
    {
        case "home":
            ContentFrame.Navigate(typeof(HomePage));
            break;

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
    bool navigated = false;

    // don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen && (NavView.DisplayMode == NavigationViewDisplayMode.Compact || NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
    {
        return false;
    }
    else
    {
        if (ContentFrame.CanGoBack)
        {
            ContentFrame.GoBack();
            navigated = true;
        }
    }
    return navigated;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        NavView.SelectedItem = NavView.SettingsItem as NavigationViewItem;
    }
    else 
    {
        Dictionary<Type, string> lookup = new Dictionary<Type, string>()
        {
            {typeof(HomePage), "home"},
            {typeof(AppsPage), "apps"},
            {typeof(GamesPage), "games"},
            {typeof(MusicPage), "music"},
            {typeof(MyContentPage), "content"}    
        };

        String stringTag = lookup[ContentFrame.SourcePageType];

        // set the new SelectedItem  
        foreach (NavigationViewItemBase item in NavView.MenuItems)
        {
            if (item is NavigationViewItem && item.Tag.Equals(stringTag))
            {
                item.IsSelected = true;
                break;
            }
        }        
    }
}
```

## <a name="customizing-backgrounds"></a>배경 사용자 지정

NavigationView의 주요 영역 배경을 변경하려면, `Background` 속성을 기본 설정 브러시로 설정하세요.

NavigationView가 최소 또는 컴팩트 모드이고 배경 아크릴이 확장 모드인 경우, 창의 배경은 앱 내 아크릴을 표시합니다. 이 동작을 업데이트하거나 창 아크릴의 모양을 사용자 지정하려면 App.xaml에서 두 테마 리소스를 덮어써서 수정합니다.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="extending-your-app-into-the-title-bar"></a>앱을 제목 표시줄로 확장

앱의 창 내에서 물 흐르듯 자연스럽게 보이도록 하려면 NavigationView와 그 아크릴 창을 앱의 제목 표시줄 영역으로 확장하는 것이 좋습니다. 이렇게 하면 제목 표시줄, 단색 NavigationView 콘텐츠 및 NavigationView 창의 아크릴에 의해 시각적으로 보기 좋지 않은 모양이 만들어지는 것을 방지할 수 있습니다.

이렇게 하려면 App.xaml.cs에 다음 코드를 추가합니다.

```csharp
//draw into the title bar
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;

//remove the solid-colored backgrounds behind the caption controls and system back button
var viewTitleBar = ApplicationView.GetForCurrentView().TitleBar;
viewTitleBar.ButtonBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonForegroundColor = (Color)Resources["SystemBaseHighColor"];
```

제목 표시줄에 그리기를 하면 앱의 제목이 숨겨지는 부작용이 있습니다. 사용자에게 도움이 되도록 자체 TextBlock을 추가하여 제목을 복원합니다. NavigationView가 포함된 루트 페이지에 다음 태그를 추가합니다.

```xaml
<Grid>
    <TextBlock x:Name="AppTitle"
        xmlns:appmodel="using:Windows.ApplicationModel"
        Text="{x:Bind appmodel:Package.Current.DisplayName}"
        Style="{StaticResource CaptionTextBlockStyle}"
        IsHitTestVisible="False"
        Canvas.ZIndex="1"/>
    

    <NavigationView Canvas.ZIndex="0" ... />

</Grid>
```

또한 뒤로 단추의 가시성에 따라 AppTitle의 여백을 조정해야 합니다. 앱이 FullScreenMode에 있으면 TitleBar가 공간을 예약한 경우라도 뒤로 화살표에 대한 간격을 제거해야 합니다.

```csharp
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
Window.Current.SetTitleBar(AppTitle);
coreTitleBar.ExtendViewIntoTitleBar = true;

void UpdateAppTitle()
{
    var full = (ApplicationView.GetForCurrentView().IsFullScreenMode);
    var left = 12 + (full ? 0 : CoreApplication.GetCurrentView().TitleBar.SystemOverlayLeftInset);
    AppTitle.Margin = new Thickness(left, 8, 0, 0);
}

Window.Current.CoreWindow.SizeChanged += (s, e) => UpdateAppTitle();
coreTitleBar.LayoutMetricsChanged += (s, e) => UpdateAppTitle();
```

제목 표시줄의 사용자 지정에 대한 자세한 내용은 [제목 표시줄 사용자 지정](../shell/title-bar.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목

- [NavigationView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [마스터/세부](master-details.md)
- [피벗 컨트롤](tabs-pivot.md)
- [탐색 기본 사항](../basics/navigation-basics.md)
- [UWP용 흐름 디자인 개요](../fluent-design-system/index.md)

