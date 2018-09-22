---
author: Jwmsft
Description: Provides a list by function of some of the controls that you can use in your apps.
title: 기능별 컨트롤
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0840bab2e039ec55ea4070f8dad39c0ae4e74bbc
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4123574"
---
# <a name="controls-by-function"></a>기능별 컨트롤

Windows용 XAML UI 프레임워크는 UI 개발을 지원하는 광범위한 컨트롤 라이브러리를 제공합니다. 이러한 컨트롤에는 시각적으로 표시되는 컨트롤도 있고, 이미지와 미디어 같이 다른 컨트롤이나 콘텐츠의 컨테이너 역할을 하는 컨트롤도 있습니다. 

[XAML UI 기본 사항 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619992)을 다운로드해도 작동 중인 다양한 Windows UI 컨트롤을 볼 수 있습니다.

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 된 경우 여기를 클릭 <a href="xamlcontrolsgallery:/item/NavigationView">앱을 열고 중인 NavigationView를 참조 하세요.</a> </p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


다음은 앱에서 사용할 수 있는 일반적인 XAML 컨트롤의 기능별 목록입니다.

## <a name="appbars-and-commands"></a>앱 바 및 명령

### <a name="app-bar"></a>앱 바
응용 프로그램 관련 명령을 표시하는 도구 모음입니다. 명령 모음을 참조하세요.

참조: [AppBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.aspx) 

### <a name="app-bar-button"></a>앱 바 단추
앱 바 스타일을 사용하여 명령을 표시하는 단추입니다.

![앱 바 단추 아이콘](images/controls/app-bar-buttons.png) 

참조: [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [SymbolIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.symbolicon.aspx), [BitmapIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.bitmapicon.aspx), [FontIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.fonticon.aspx), [PathIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pathicon.aspx) 

디자인 및 방법: [앱 바 및 명령 모음 컨트롤 가이드](app-bars.md) 

샘플 코드: [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-separator"></a>앱 바 구분 기호
명령 모음의 명령 그룹을 시각적으로 구분합니다.

참조: [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 

샘플 코드: [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-toggle-button"></a>앱 바 토글 단추
명령 모음에서 명령을 표시하거나 숨기는 단추입니다.

참조: [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 

샘플 코드: [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="command-bar"></a>명령 모음
앱 바 단추 요소의 크기 조정을 처리하는 특수한 앱 바입니다.

![명령 모음 컨트롤](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
참조: [CommandBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 

디자인 및 방법: [앱 바 및 명령 모음 컨트롤 가이드](app-bars.md)

샘플 코드: [XAML 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="buttons"></a>단추

### <a name="button"></a>Button
사용자 입력에 응답하고 **Click** 이벤트를 발생시키는 컨트롤입니다.

![표준 단추](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

참조: [Button](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx) 

디자인 및 방법: [단추 컨트롤 가이드](buttons.md) 

### <a name="hyperlink"></a>Hyperlink
하이퍼링크 단추를 참조하세요.

### <a name="hyperlink-button"></a>하이퍼링크 단추
마크업 텍스트로 표시되고 브라우저에서 지정된 URI를 여는 단추입니다.

![하이퍼링크 단추](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="http://www.microsoft.com"/>
```

참조: [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hyperlinkbutton.aspx) 

디자인 및 방법: [하이퍼링크 컨트롤 가이드](hyperlinks.md)

### <a name="repeat-button"></a>반복 단추
사용자가 눌렀다가 놓을 때까지 반복해서 **Click** 이벤트를 발생시키는 컨트롤입니다. 

![반복 단추 컨트롤](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

참조: [RepeatButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 

디자인 및 방법: [단추 컨트롤 가이드](buttons.md) 

## <a name="collectiondata-controls"></a>모음/데이터 컨트롤

### <a name="flip-view"></a>대칭 이동 뷰
사용자가 한 번에 하나씩 차례로 전환할 수 있는 항목 컬렉션을 제공하는 컨트롤입니다.

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

참조: [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx) 

디자인 및 방법: [대칭 이동 뷰 컨트롤 가이드](flipview.md) 

### <a name="grid-view"></a>그리드 뷰
세로로 스크롤할 수 있는 행과 열 형태로 항목 컬렉션을 제공하는 컨트롤입니다.

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

참조: [GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 

디자인 및 방법: [목록](lists.md) 

샘플 코드: [ListView 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619900)

### <a name="items-control"></a>항목 컨트롤
데이터 템플릿을 통해 지정된 UI에 항목 컬렉션을 제공하는 컨트롤입니다. 

```xaml
<ItemsControl/>
```

참조: [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) 

### <a name="list-view"></a>목록 뷰
가로로 스크롤할 수 있는 목록 형태로 항목 컬렉션을 제공하는 컨트롤입니다.

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

참조: [ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 

디자인 및 방법: [목록](lists.md) 

샘플 코드: [ListView 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619900)

## <a name="date-and-time-controls"></a>날짜 및 시간 컨트롤

### <a name="calendar-date-picker"></a>달력 날짜 선택
드롭다운 달력 표시를 사용하여 날짜를 선택할 수 있는 컨트롤입니다.

![일정 보기가 열려 있는 일정 날짜 선택 기능입니다.](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

참조: [CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx) 

디자인 및 방법: [일정, 날짜 및 시간 컨트롤](date-and-time.md)
 
### <a name="calendar-view"></a>달력 보기
단일 날짜 또는 여러 날짜를 선택할 수 있는 구성 가능한 달력 표시입니다.

```xaml
<CalendarView/>
```

참조: [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) 

디자인 및 방법: [일정, 날짜 및 시간 컨트롤](date-and-time.md) 

### <a name="date-picker"></a>날짜 선택기
사용자가 날짜를 선택할 수 있는 컨트롤입니다.

![날짜 선택 컨트롤](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

참조: [DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx) 

디자인 및 방법: [일정, 날짜 및 시간 컨트롤](date-and-time.md)
 
### <a name="time-picker"></a>시간 선택기
사용자가 시간 값을 설정할 수 있는 컨트롤입니다.

![TimePicker 컨트롤](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

참조: [TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx) 

디자인 및 방법: [일정, 날짜 및 시간 컨트롤](date-and-time.md)

## <a name="flyouts"></a>플라이아웃

### <a name="context-menu"></a>상황에 맞는 메뉴
메뉴 플라이아웃 및 팝업 메뉴를 참조하세요.

### <a name="flyout"></a>플라이아웃
사용자의 조작이 필요한 메시지를 표시합니다. 대화 상자와 달리 플라이아웃은 별도의 창을 만들지 않으며 다른 사용자 조작을 차단하지 않습니다.

![플라이아웃 컨트롤](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

참조: [Flyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flyout.aspx) 

디자인 및 방법: [플라이 아웃](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>메뉴 플라이아웃
사용자 현재 수행 중인 작업과 관련된 명령 또는 옵션 목록을 일시적으로 표시합니다.

![메뉴 플라이아웃 컨트롤](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

참조: [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyout.aspx), [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 

디자인 및 방법: [메뉴 및 상황에 맞는 메뉴](menus.md) 

샘플 코드: [XAML 상황에 맞는 메뉴 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620021)

### <a name="popup-menu"></a>팝업 메뉴
지정한 명령을 표시하는 사용자 지정 메뉴입니다.

참조: [PopupMenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.popups.popupmenu.aspx) 

디자인 및 방법: [대화 상자](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>도구 설명
요소에 대한 정보를 표시하는 팝업 창입니다. 
 
![도구 설명 컨트롤](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

참조: [ToolTip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltip.aspx), [ToolTipService](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltipservice.aspx) 

디자인 및 방법: 도구 설명에 대한 지침 

## <a name="images"></a>이미지

### <a name="image"></a>이미지
이미지를 제공하는 컨트롤입니다.

```xaml
<Image Source="Assets/Logo.png" />
```

참조: [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 

설계 및 방법: [Image 및 ImageBrush](images-imagebrushes.md) 

샘플 코드: [XAML 이미지 샘플](http://go.microsoft.com/fwlink/p/?linkid=226867)

## <a name="graphics-and-ink"></a>그래픽 및 잉크

### <a name="inkcanvas"></a>InkCanvas
잉크 스트로크를 받아 표시하는 컨트롤입니다.

```xaml
<InkCanvas/>
```

참조: [InkCanvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.inkcanvas.aspx) 

### <a name="shapes"></a>셰이프
줄임표, 사각형, 선. 베지어 패스 등과 같이 표시할 수 있는 다양한 유지 모드 그래픽 개체입니다.

![다각형](images/controls/shapes-polygon.png) 
![경로](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

참조: [Shapes](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.aspx) 

방법: [셰이프 그리기](../../graphics/drawing-shapes.md) 

샘플 코드: [XAML 벡터 기반 그리기 샘플](http://go.microsoft.com/fwlink/p/?linkid=226866)

## <a name="layout-controls"></a>레이아웃 컨트롤

### <a name="border"></a>테두리
다른 개체 주변에 테두리, 배경 또는 둘 다를 그리는 컨테이너 컨트롤입니다.

![2개 직사각형 둘레의 테두리](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

참조: [Border](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.border.aspx)

### <a name="canvas"></a>캔버스
캔버스의 왼쪽 위 모서리에 상대적인 자식 요소의 절대 위치 지정을 지원하는 레이아웃 패널입니다.
 
![캔버스 레이아웃 패널](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

참조: [Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)
 
### <a name="grid"></a>그리드
자식 요소에 대한 열과 행 형태의 배열을 지원하는 레이아웃 패널입니다.

![그리드 레이아웃 패널](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

참조: [Grid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)
 
### <a name="panning-scroll-viewer"></a>이동 스크롤 뷰어
스크롤 뷰어를 참조하세요.

### <a name="relativepanel"></a>RelativePanel
서로 또는 부모 패널과 관련하여 자식 개체를 배치하고 정렬할 수 있는 패널입니다.

![상대 패널 레이아웃 패널](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

참조: [RelativePanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)

### <a name="scroll-bar"></a>스크롤 막대
스크롤 뷰어를 참조하세요. (ScrollBar는 ScrollViewer의 요소입니다. 일반적으로 독립 실행형 컨트롤로 사용하지 않습니다.

참조: [ScrollBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.scrollbar.aspx)
 
### <a name="scroll-viewer"></a>스크롤 뷰어
사용자가 내용을 이동 및 확대/축소할 수 있도록 하는 컨테이너 컨트롤입니다.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

참조: [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx)

디자인 및 방법: [스크롤 및 이동 컨트롤 가이드](scroll-controls.md) 

샘플 코드: [XAML 스크롤, 이동 및 확대/축소 샘플](http://go.microsoft.com/fwlink/p/?linkid=238577)

### <a name="stack-panel"></a>스택 패널
가로 또는 세로 방향으로 지정된 단일 행에 자식 요소를 배열하는 레이아웃 패널입니다.

![스택 패널 레이아웃 컨트롤](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

참조: [StackPanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
자식 요소에 대한 열과 행 형태의 배열을 지원하는 레이아웃 패널입니다. 각 자식 요소를 여러 행과 열에 걸쳐 표시할 수 있습니다.

![가변 크기 랩 그리드 레이아웃 패널](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

참조: [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)

### <a name="viewbox"></a>Viewbox
내용의 크기를 지정된 크기로 조정된 컨테이너 컨트롤입니다.

![Viewbox 컨트롤](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

참조: [Viewbox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.viewbox.aspx)
 
### <a name="zooming-scroll-viewer"></a>확대/축소 스크롤 뷰어
스크롤 뷰어를 참조하세요.

## <a name="media-controls"></a>미디어 컨트롤

### <a name="audio"></a>오디오
미디어 요소 참조

### <a name="media-element"></a>미디어 요소
오디오 및 비디오 콘텐츠를 재생하는 컨트롤입니다.

```xaml
<MediaElement x:Name="myMediaElement"/>
```

참조: [MediaElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediaelement.aspx) 

디자인 및 방법: [미디어 요소 컨트롤 가이드](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
MediaElement에 대한 재생 컨트롤을 제공하는 컨트롤입니다.

![전송 컨트롤이 포함된 미디어 요소](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

참조: [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 

디자인 및 방법: [미디어 요소 컨트롤 가이드](media-playback.md) 

샘플 코드: [시스템 미디어 전송 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620023)

### <a name="video"></a>비디오
미디어 요소를 참조하세요.

## <a name="navigation"></a>탐색

### <a name="navigationview"></a>NavigationView

조정할 수 있도록 컨테이너 및 왼쪽된 탐색 창, 상단 탐색 및 탭 패턴을 구현 하는 유연한 탐색 모델입니다.

참조: [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

디자인 및 방법: [NavigationView 컨트롤 가이드](navigationview.md)

### <a name="splitview"></a>분할 보기

기본 콘텐츠에 대한 보기 하나와 일반적으로 탐색 메뉴에 사용되는 다른 보기 이렇게 두 개의 보기가 포함된 컨테이너 컨트롤입니다.

![분할 보기 컨트롤](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

참조: [SplitView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 

디자인 및 방법: [분할 보기 컨트롤 가이드](split-view.md)

### <a name="web-view"></a>웹 뷰

웹 콘텐츠를 호스트하는 컨테이너 컨트롤입니다.

```xaml
<WebView x:Name="webView1" Source="http://dev.windows.com" 
         Height="400" Width="800"/>
```

참조: [WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 

디자인 및 방법: 웹 보기에 대한 지침 

샘플 코드: [XAML WebView 컨트롤 샘플](http://go.microsoft.com/fwlink/p/?linkid=238582)

### <a name="semantic-zoom"></a>시맨틱 줌

사용자가 항목 컬렉션의 두 가지 뷰 사이에서 확대/축소할 수 있도록 해 주는 컨테이너 컨트롤입니다.

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

참조: [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.aspx) 

디자인 및 방법: [시맨틱 줌 컨트롤 가이드](semantic-zoom.md)

샘플 코드: [XAML GridView 그룹화 및 SemanticZoom 샘플](http://go.microsoft.com/fwlink/p/?linkid=226564)

## <a name="progress-controls"></a>진행률 컨트롤

### <a name="progress-bar"></a>진행률 표시줄
막대를 표시하여 진행 상황을 나타내는 컨트롤입니다.

![진행률 표시줄 컨트롤](images/controls/progress-bar-determinate.png)

특정 값을 표시하는 진행률 표시줄입니다.

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![확정되지 않은 진행률 표시줄 컨트롤](images/controls/progress-bar-indeterminate.png)

확정되지 않은 진행 상황을 표시하는 진행률 표시줄입니다.

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

참조: [ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx) 

디자인 및 방법: [진행률 컨트롤 가이드](progress-controls.md) 

### <a name="progress-ring"></a>진행률 링
링 표시로 확정되지 않은 진행률을 나타내는 컨트롤입니다. 

![진행률 링 컨트롤](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

참조: [ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx) 

디자인 및 방법: [진행률 컨트롤 가이드](progress-controls.md) 

## <a name="text-controls"></a>텍스트 컨트롤

### <a name="auto-suggest-box"></a>자동 제안 상자
사용자가 입력할 때 제안된 텍스트를 제공하는 텍스트 입력 상자입니다.

![검색을 위한 자동 제안 상자](images/controls/auto-suggest-box.png) 

참조: [AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)

디자인 및 방법: [텍스트 컨트롤](text-controls.md), [자동 제안 상자 컨트롤 가이드](auto-suggest-box.md)

샘플 코드: [AutoSuggestBox 마이그레이션 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619996)

### <a name="multi-line-text-box"></a>여러 줄 입력란
텍스트 상자를 참조하세요.

### <a name="password-box"></a>암호 상자
암호를 입력하기 위한 컨트롤입니다.

 ![암호 상자](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

참조: [PasswordBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx) 

디자인 및 방법: [텍스트 컨트롤](text-controls.md), [암호 상자 컨트롤 가이드](password-box.md) 

샘플 코드: [XAML 텍스트 표시 샘플](http://go.microsoft.com/fwlink/p/?linkid=238579), [XAML 텍스트 편집 샘플](http://go.microsoft.com/fwlink/p/?linkid=251417)

### <a name="rich-edit-box"></a>서식 있는 편집 상자
사용자가 서식 있는 텍스트, 하이퍼링크, 이미지 등의 콘텐츠가 포함된 서식 있는 텍스트 문서를 편집할 수 있는 컨트롤입니다.

```xaml
<RichEditBox />
```

참조: [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 

디자인 및 방법: [텍스트 컨트롤](text-controls.md), [서식 있는 편집 상자 컨트롤 가이드](rich-edit-box.md)

샘플 코드: [XAML 텍스트 샘플](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="search-box"></a>검색 상자
자동 제안 상자를 참조하세요.

### <a name="single-line-text-box"></a>한 줄 입력란
텍스트 상자를 참조하세요.

### <a name="static-textparagraph"></a>정적 텍스트/단락
텍스트 블록을 참조하세요.

### <a name="text-block"></a>텍스트 블록
텍스트를 표시하는 컨트롤입니다.

![텍스트 블록 컨트롤](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

참조: [TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx) 

디자인 및 방법: [텍스트 컨트롤](text-controls.md), [텍스트 블록 컨트롤 가이드](text-block.md), [서식 있는 텍스트 블록 컨트롤 가이드](rich-text-block.md)

샘플 코드: [XAML 텍스트 샘플](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="text-box"></a>입력란
한 줄 또는 여러 줄 일반 텍스트 필드입니다.

![텍스트 상자 컨트롤](images/controls/text-box.png) 

```xaml
<TextBox x:Name="textBox1" Text="I am a TextBox" 
         TextChanged="TextBox_TextChanged"/>
```

참조: [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 

디자인 및 방법: [텍스트 컨트롤](text-controls.md), [텍스트 상자 컨트롤 가이드](text-box.md) 

샘플 코드: [XAML 텍스트 샘플](http://go.microsoft.com/fwlink/p/?linkid=238578)

## <a name="selection-controls"></a>선택 컨트롤

### <a name="check-box"></a>확인란
사용자가 선택하거나 선택 취소할 수 있는 컨트롤입니다.

![확인란의 세 가지 상태](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

참조: [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx) 

디자인 및 방법: [확인란 컨트롤 가이드](checkbox.md) 

### <a name="combo-box"></a>콤보 상자
사용자가 선택할 수 있는 항목의 드롭다운 목록입니다.

![콤보 상자 열기](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

참조: [ComboBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.combobox.aspx) 

디자인 및 방법: [목록](lists.md) 

### <a name="list-box"></a>목록 상자
사용자가 선택할 수 있는 인라인 항목 목록을 제공하는 컨트롤입니다. 

![목록 상자 컨트롤](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ListBox>
```

참조: [ListBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listbox.aspx) 

디자인 및 방법: [목록](lists.md) 

### <a name="radio-button"></a>라디오 단추
사용자가 옵션 그룹에서 하나의 옵션을 선택할 수 있도록 해 주는 컨트롤입니다. 라디오 단추가 그룹화되면 함께 사용할 수 없습니다.

![라디오 단추 컨트롤](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

참조: [RadioButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.radiobutton.aspx) 

디자인 및 방법: [라디오 단추 컨트롤 가이드](radio-button.md)
 
### <a name="slider"></a>슬라이더
사용자가 트랙을 따라 Thumb 컨트롤을 이동하여 값 범위에서 값을 선택할 수 있도록 하는 컨트롤입니다.

![슬라이더 컨트롤](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

참조: [Slider](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 

디자인 및 방법: [슬라이더 컨트롤 가이드](slider.md) 

### <a name="toggle-button"></a>토글 단추
두 가지 상태 간에 전환할 수 있는 단추입니다.

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

참조: [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)

디자인 및 방법: [토글 컨트롤 가이드](toggles.md) 

### <a name="toggle-switch"></a>토글 스위치
두 가지 상태에서 전환할 수 있는 스위치입니다.

![토글 스위치 컨트롤](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

참조: [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.toggleswitch.aspx) 

디자인 및 방법: [토글 컨트롤 가이드](toggles.md) 
