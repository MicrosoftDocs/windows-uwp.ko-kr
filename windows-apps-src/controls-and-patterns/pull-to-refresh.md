---
author: Jwmsft
Description: "목록 보기에서 당겨서 새로 고침 패턴을 사용합니다."
title: "당겨서 새로 고침"
label: Pull-to-refresh
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.openlocfilehash: 51a8c9a2e4618e054374308918a74cf2095119ef
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2017
---
# <a name="pull-to-refresh"></a>당겨서 새로 고침

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

당겨서 새로 고침 패턴을 사용하면 데이터 목록을 터치하고 아래로 당겨서 더 많은 데이터를 검색할 수 있습니다. 당겨서 새로 고침은 모바일 앱에서 널리 사용되지만 터치 스크린이 있는 디바이스에서 유용합니다. [조작 이벤트](../input-and-devices/touch-interactions.md#manipulation-events)를 처리하여 앱에서 당겨서 새로 고침을 구현할 수 있습니다.

> **중요 API**: [ListView 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [GridView 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

[당겨서 새로 고침 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620635)은 이 패턴을 지원하는 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 컨트롤을 확장하는 방법을 보여 줍니다. 이 문서에서는 이 샘플을 사용하여 당겨서 새로 고침을 구현하는 주요 사항을 설명합니다.

![당겨서 새로 고침 샘플](images/ptr-phone-1.png)

## <a name="is-this-the-right-pattern"></a>올바른 패턴인가요?

정기적으로 새로 고치려는 데이터 목록이나 그리드가 있고 앱이 터치 우선 모바일 디바이스에서 실행될 수 있는 경우 당겨서 새로 고침 패턴을 사용합니다.

## <a name="implement-pull-to-refresh"></a>당겨서 새로 고침 구현

당겨서 새로 고침을 구현하려면 사용자가 목록을 아래로 당길 때 조작 이벤트를 처리하고, 시각적 피드백을 제공하고, 데이터를 새로 고쳐야 합니다. 여기서는 [당겨서 새로 고침 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620635)에서 이 작업을 수행하는 방법에 대해 알아봅니다. 여기에 모든 코드가 표시되지 않으므로 GitHub의 코드를 보거나 샘플을 다운로드해야 합니다.

당겨서 새로 고침 샘플은 **ListView** 컨트롤을 확장하는 `RefreshableListView`라는 사용자 지정 컨트롤을 만듭니다. 이 컨트롤은 시각적 피드백을 제공하는 새로 고침 표시기를 추가하고 목록 보기의 내부 스크롤 뷰어의 조작 이벤트를 처리합니다. 또한 목록을 끌어오는 경우와 데이터를 새로 고쳐야 하는 경우를 알리는 두 이벤트를 추가합니다. RefreshableListView는 데이터를 새로 고쳐야 하는 알림만 제공합니다. 데이터를 업데이트하려면 앱에서 이벤트를 처리해야 하고 해당 코드는 모든 앱에 따라 다릅니다.

RefreshableListView는 새로 고침을 요청하는 시기 및 새로 고침 표시기가 사라지는 방법을 결정하는 ‘자동 새로 고침’ 모드를 제공합니다. 자동 새로 고침을 켜거나 끌 수 있습니다.
- 꺼짐: `PullThreshold`를 초과했을 때 목록이 해제되는 경우에만 새로 고침을 요청합니다. 사용자가 스크롤러를 놓을 때 표시기가 사라지는 애니메이션 효과가 주어집니다. 휴대폰에서 사용할 수 있는 경우 상태 표시줄 표시기가 표시됩니다.
- 켜짐: `PullThreshold`를 초과하자마자 해제 여부에 상관없이 새로 고침을 요청합니다. 표시기는 새 데이터를 검색할 때까지 표시되며 새 데이터가 검색되면 사라지는 애니메이션 효과가 주어집니다. **Deferral**은 데이터 가져오기가 완료되면 앱에 알리는 데 사용됩니다.

> **참고**&nbsp;&nbsp;샘플의 코드는 [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)에 적용될 수도 있습니다. GridView를 수정하려면 ListView 대신 GridView에서 사용자 지정 클래스를 파생하고 기본 GridView 템플릿을 수정합니다.

## <a name="add-a-refresh-indicator"></a>새로 고침 표시기 추가

사용자에게 시각적 피드백을 제공하여 앱에서 당겨서 새로 고침 기능을 사용할 수 있다고 알립니다. RefreshableListView에는 XAML에서 시각적 표시기를 설정할 수 있는 `RefreshIndicatorContent` 속성이 있습니다. `RefreshIndicatorContent`를 설정하지 않는 경우 대체될 기본 텍스트 표시기도 포함합니다.

다음은 새로 고침 표시기에 대한 권장 지침입니다.

![새로 고침 표시기 검토](images/ptr-redlines-1.png)

**목록 보기 템플릿 수정**

당겨서 새로 고침 샘플에서 `RefreshableListView` 컨트롤 템플릿은 새로 고침 표시기를 추가하여 표준 **ListView** 템플릿을 수정합니다. 새로 고침 표시기는 목록 항목을 보여 주는 부분인 [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx) 위의 [Grid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx)에 배치됩니다.

> **참고**&nbsp;&nbsp;`DefaultRefreshIndicatorContent` 텍스트 상자는 `RefreshIndicatorContent` 속성이 설정되지 않은 경우에만 표시되는 텍스트 대체 표시기를 제공합니다.

다음은 기본 ListView 템플릿에서 수정된 컨트롤 템플릿의 일부입니다.

**XAML**
```xaml
<!-- Styles/Styles.xaml -->
<Grid x:Name="ScrollerContent" VerticalAlignment="Top">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Border x:Name="RefreshIndicator" VerticalAlignment="Top" Grid.Row="1">
        <Grid>
            <TextBlock x:Name="DefaultRefreshIndicatorContent" HorizontalAlignment="Center" 
                       Foreground="White" FontSize="20" Margin="20, 35, 20, 20"/>
            <ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}"></ContentPresenter>
        </Grid>
    </Border>
    <ItemsPresenter FooterTransitions="{TemplateBinding FooterTransitions}" 
                    FooterTemplate="{TemplateBinding FooterTemplate}" 
                    Footer="{TemplateBinding Footer}" 
                    HeaderTemplate="{TemplateBinding HeaderTemplate}" 
                    Header="{TemplateBinding Header}" 
                    HeaderTransitions="{TemplateBinding HeaderTransitions}" 
                    Padding="{TemplateBinding Padding}"
                    Grid.Row="1"
                    x:Name="ItemsPresenter"/>
</Grid>
```

**XAML에서 콘텐츠 설정**

XAML에서 목록 보기에 대한 새로 고침 표시기의 콘텐츠를 설정합니다. 새로 고침 표시기의 [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx)(`<ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}">`)에 의해 설정한 XAML 콘텐츠가 표시됩니다. 이 콘텐츠를 설정하지 않으면 기본 텍스트 표시기가 대신 표시됩니다.

**XAML**
```xaml
<!-- MainPage.xaml -->
<c:RefreshableListView
    <!-- ... See sample for removed code. -->
    AutoRefresh="{x:Bind Path=UseAutoRefresh, Mode=OneWay}"
    ItemsSource="{x:Bind Items}"
    PullProgressChanged="listView_PullProgressChanged"
    RefreshRequested="listView_RefreshRequested">

    <c:RefreshableListView.RefreshIndicatorContent>
        <Grid Height="100" Background="Transparent">
            <FontIcon
                Margin="0,0,0,30"
                HorizontalAlignment="Center"
                VerticalAlignment="Bottom"
                FontFamily="Segoe MDL2 Assets"
                FontSize="20"
                Glyph="&#xE72C;"
                RenderTransformOrigin="0.5,0.5">
                <FontIcon.RenderTransform>
                    <RotateTransform x:Name="SpinnerTransform" Angle="0" />
                </FontIcon.RenderTransform>
            </FontIcon>
        </Grid>
    </c:RefreshableListView.RefreshIndicatorContent>
    
    <!-- ... See sample for removed code. -->

</c:RefreshableListView>
```

**회전자에 애니메이션 효과 주기**

목록을 아래로 당기면 RefreshableListView의 `PullProgressChanged` 이벤트가 발생합니다. 앱에서 이 이벤트를 처리하여 새로 고침 표시기를 제어합니다. 샘플에서는 이 스토리보드가 표시기의 [RotateTransform](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.aspx)에 애니메이션 효과를 주고 새로 고침 표시기를 회전하는 작업부터 시작합니다. 

**XAML**
```xaml
<!-- MainPage.xaml -->
<Storyboard x:Name="SpinnerStoryboard">
    <DoubleAnimation
        Duration="00:00:00.5"
        FillBehavior="HoldEnd"
        From="0"
        RepeatBehavior="Forever"
        Storyboard.TargetName="SpinnerTransform"
        Storyboard.TargetProperty="Angle"
        To="360" />
</Storyboard>
```

## <a name="handle-scroll-viewer-manipulation-events"></a>스크롤 뷰어 조작 이벤트 처리

목록 보기 컨트롤 템플릿에는 사용자가 목록 항목을 스크롤할 수 있도록 해 주는 기본 제공 [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)가 포함되어 있습니다. 당겨서 새로 고침을 구현하려면 기본 제공 스크롤 뷰어의 조작 이벤트뿐만 아니라 여러 관련 이벤트를 처리해야 합니다. 조작 이벤트에 대한 자세한 내용은 [터치 조작](../input-and-devices/touch-interactions.md)을 참조하세요.

** OnApplyTemplate**

이벤트 처리기를 추가하고 나중에 코드에서 호출할 수 있도록 스크롤 뷰어 및 기타 템플릿 부분에 액세스하려면 [OnApplyTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.onapplytemplate.aspx) 메서드를 재정의해야 합니다. 코드에서 나중에 사용하도록 저장할 수 있는 컨트롤 템플릿의 명명된 부분에 대한 참조를 가져오려면 OnApplyTemplate 템플릿에서 [GetTemplateChild](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.gettemplatechild.aspx)를 호출합니다.

샘플에서는 템플릿 부분을 저장하는 데 사용된 변수를 개인 변수 영역에서 선언합니다. OnApplyTemplate 메서드에서 검색된 후 [DirectManipulationStarted](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationstarted.aspx), [DirectManipulationCompleted](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationcompleted.aspx), [ViewChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.viewchanged.aspx) 및 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 이벤트의 이벤트 처리기가 추가됩니다.

**DirectManipulationStarted**

당겨서 새로 고침 작업을 시작하려면 사용자가 아래로 당기기 시작할 때 콘텐츠가 스크롤 뷰어의 맨 위로 스크롤되어야 합니다. 그러지 않으면 사용자가 목록에서 위로 이동하기 위해 끌어오는 것으로 간주됩니다. 이 처리기의 코드는 조작이 스크롤 뷰어 맨 위의 콘텐츠로 시작되었는지 여부를 확인하고 목록의 결과를 새로 고칠 수 있습니다. 컨트롤의 '새로 고칠 수 있는' 상태가 적절하게 설정됩니다. 

컨트롤을 새로 고칠 수 있으면 애니메이션에 대한 이벤트 처리기도 추가됩니다.

**DirectManipulationCompleted**

사용자가 목록 당기기를 멈추면 조작하는 동안 이 처리기의 코드가 새로 고침 활성화 여부를 확인합니다. 새로 고침이 활성화된 경우 `RefreshRequested` 이벤트가 발생하고 `RefreshCommand` 명령이 실행됩니다.

애니메이션에 대한 이벤트 처리기도 제거됩니다.

`AutoRefresh` 속성 값에 따라 목록에 즉시 애니메이션 효과를 다시 주거나 새로 고침이 완료될 때까지 기다린 후 다시 애니메이션 효과를 줄 수도 있습니다. [Deferral](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 개체가 새로 고침 완료 표시에 사용됩니다. 이때 새로 고침 표시기 UI가 숨겨집니다.

DirectManipulationCompleted 이벤트 처리기의 이 부분에서 `RefreshRequested` 이벤트가 발생하고 필요한 경우 Deferral을 가져옵니다.

**C#**
```csharp
if (this.RefreshRequested != null)
{
    RefreshRequestedEventArgs refreshRequestedEventArgs = new RefreshRequestedEventArgs(
        this.AutoRefresh ? new DeferralCompletedHandler(RefreshCompleted) : null);
    this.RefreshRequested(this, refreshRequestedEventArgs);
    if (this.AutoRefresh)
    {
        m_scrollerContent.ManipulationMode = ManipulationModes.None;
        if (!refreshRequestedEventArgs.WasDeferralRetrieved)
        {
            // The Deferral object was not retrieved in the event handler.
            // Animate the content up right away.
            this.RefreshCompleted();
        }
    }
}
```

**ViewChanged**

다음 두 경우 ViewChanged 이벤트 처리기에서 처리됩니다.

첫째, 스크롤 뷰어 확대/축소로 인해 보기가 변경된 경우 컨트롤의 '새로 고칠 수 있는' 상태가 취소됩니다.

둘째, 콘텐츠가 자동 새로 고침의 마지막에 애니메이션을 종료한 경우 안쪽 여백 사각형이 숨겨지고 스크롤 뷰어를 사용한 터치 조작을 다시 사용할 수 있으며 [VerticalOffset](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticaloffset.aspx)은 0으로 설정됩니다.

**PointerPressed**

당겨서 새로 고침은 터치 조작으로 목록을 당길 때에만 발생합니다. PointerPressed 이벤트 처리기에서 코드는 이벤트를 발생시킨 포인터의 종류를 확인하고 터치 포인터였는지 여부를 나타내는 변수(`m_pointerPressed`)를 설정합니다. 이 변수는 DirectManipulationStarted 처리기에 사용됩니다. 포인터가 터치 포인터가 아닌 경우 아무 작업도 수행되지 않고 DirectManipulationStarted 처리기가 반환됩니다.

## <a name="add-pull-and-refresh-events"></a>끌어오기 및 새로 고침 이벤트 추가

'RefreshableListView'는 데이터를 새로 고치고 새로 고침 표시기를 관리할 수 있도록 앱에서 처리할 수 있는 2개의 이벤트를 추가합니다.

이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)를 참조하세요.

**RefreshRequested**

'RefreshRequested' 이벤트는 사용자가 새로 고칠 목록을 끌어온 앱에 알립니다. 이 이벤트를 처리하여 새 데이터를 가져오고 목록을 업데이트합니다.

다음은 샘플의 이벤트 처리기입니다. 목록 보기의 `AutoRefresh` 속성을 확인하여 **true**인 경우 Deferral을 가져오는 것을 알 수 있습니다. Deferral을 사용하면 새로 고침이 완료될 때까지 새로 고침 표시기가 중지되거나 숨겨지지 않습니다.

**C#**
```csharp
private async void listView_RefreshRequested(object sender, RefreshableListView.RefreshRequestedEventArgs e)
{
    using (Deferral deferral = listView.AutoRefresh ? e.GetDeferral() : null)
    {
        await FetchAndInsertItemsAsync(_rand.Next(1, 5));

        if (SpinnerStoryboard.GetCurrentState() != Windows.UI.Xaml.Media.Animation.ClockState.Stopped)
        {
            SpinnerStoryboard.Stop();
        }
    }
}
```

**PullProgressChanged**

샘플에서는 앱에서 새로 고침 표시기에 대한 콘텐츠를 제공하고 제어합니다. 'PullProgressChanged' 이벤트는 새로 고침 표시기를 시작, 중지 및 다시 설정할 수 있도록 목록을 끌어올 때 앱에 알립니다. 

## <a name="composition-animations"></a>컴퍼지션 애니메이션

기본적으로 스크롤 막대가 맨 위에 도달하면 스크롤 뷰어의 콘텐츠는 중지됩니다. 사용자가 계속해서 목록을 아래로 당기면 시각적 계층에 액세스하고 목록 콘텐츠에 애니메이션 효과를 줘야 합니다. 샘플에서는 이를 위해 [컴퍼지션 애니메이션](https://msdn.microsoft.com/windows/uwp/composition/composition-animation), 특히 [식 애니메이션](https://msdn.microsoft.com/windows/uwp/composition/composition-animation#expression-animations)을 사용합니다.

샘플에서 이 작업은 주로 `CompositionTarget_Rendering` 이벤트 처리기 및 `UpdateCompositionAnimations` 메서드에서 수행됩니다.

## <a name="related-articles"></a>관련 문서

- [컨트롤 스타일 지정](styling-controls.md)
- [터치 조작](../input-and-devices/touch-interactions.md)
- [목록 보기 및 그리드 보기](listview-and-gridview.md)
- [목록 보기 항목 템플릿](listview-item-templates.md)
- [식 애니메이션](https://msdn.microsoft.com/windows/uwp/composition/composition-animation#expression-animations)