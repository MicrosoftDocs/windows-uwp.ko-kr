---
Description: Use the pull-to-refresh control to get new content into a list.
title: 당겨서 새로 고침
label: Pull-to-refresh
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2efd091d90a856e45d76c0b1357f30417812160a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194805"
---
# <a name="pull-to-refresh"></a>당겨서 새로 고침

당겨서 새로 고침을 사용하면 터치로 데이터 목록을 아래로 당겨서 더 많은 데이터를 검색할 수 있습니다. 당겨서 새로 고침은 터치 스크린이 장착된 디바이스에서 널리 사용되고 있습니다. 여기 나온 API를 사용하여 앱에서 당겨서 새로 고침을 구현할 수 있습니다.

> **중요 API**: [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![당겨서 새로 고침 gif](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

정기적으로 새로 고침을 수행하고 싶은 데이터 목록이나 그리드가 있을 때는 당겨서 새로 고침을 사용합니다. 그러면 앱이 터치 우선 디바이스에서 실행될 것입니다.

또한 [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)를 사용하여 새로 고침 버튼 같은 다른 방법을 통해 호출이 가능하도록 일관된 새로 고침 환경을 만들 수 있습니다.

## <a name="refresh-controls"></a>새로 고침 컨트롤

당겨서 새로 고침은 2개의 컨트롤로 활성화됩니다.

- **RefreshContainer** - 당겨서 새로 고침 환경에 래퍼를 제공하는 ContentControl입니다. 이는 터치 조작을 처리하고 내부 새로 고침 시각화 도우미의 상태를 관리합니다.
- **RefreshVisualizer** - 다음 섹션에서 설명할 새로 고침 시각화를 캡슐화합니다.

주 컨트롤은 **RefreshContainer**이고, 사용자는 새로 고침을 트리거하기 위해 가져온 콘텐츠를 중심으로 이 컨트롤을 래퍼로서 배치합니다. RefreshContainer는 터치 방식으로만 작동하므로 터치 인터페이스가 없는 사용자가 사용할 수 있는 새로 고침 단추가 있습니다. 새로 고침 단추를 앱의 적정 위치(명령 모음이나 새로 고침 중인 표면과 가까운 위치)에 위치시킬 수 있습니다.

## <a name="refresh-visualization"></a>새로 고침 시각화

새로 고침 시각화는 새로 고침이 발생하는 때를 알리는 데 사용되는 순환 진행률 스피너로서, 개시 후 새로 고침의 진행 상태를 나타냅니다. 새로 고침 시각화 도우미의 상태는 5가지입니다.

 사용자가 새로 고침을 시작하기 위해 목록에서 끌어 내려야 하는 거리를 _임계값_이라고 합니다. 시각화 도우미 [상태](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State)는 이 임계값과 관련된 끌어오기 상태에 의해 결정됩니다. 가능한 값들은 [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate) 열거형에 포함되어 있습니다.

### <a name="idle"></a>유휴

시각화 도우미의 기본 상태는 **유휴** 상태입니다. 사용자는 터치를 통해 RefreshContainer와 상호 작용을 하지 않으며, 진행 중에는 새로 고침이 수행되지 않습니다.

새로 고침 시각화 도우미는 눈에 보이는 증거가 없습니다.

### <a name="interacting"></a>상호 작용 중

사용자가 PullDirection 속성에서 지정된 방향으로 목록을 끌어오면 임계값에 도달하기 전에 시각화 도우미가 **상호 작용 중** 상태에 있습니다.

- 이 상태에 있는 동안 사용자가 컨트롤을 해제하면 해당 컨트롤이 **유휴** 상태로 돌아갑니다.

    ![임계값에 도달하기 전 당겨서 새로 고침](images/ptr-prethreshold.png)

    시각적으로 아이콘은 비활성 상태로 표시됩니다(불투명도 60%). 또한 이 아이콘은 스크롤 조작을 통해 1회 완전히 회전합니다.

- 사용자가 임계값을 초과한 목록을 끌어 오면 시각화 도우미가 **상호 작용 중**에서 **보류 중**으로 상태가 전환됩니다.

    ![임계값 도달 시 당겨서 새로 고침](images/ptr-atthreshold.png)

    시각적으로 이 아이콘은 100% 불투명도와 펄스로 최대 150% 크기의 펄스로 전환되고, 전환 중에 다시 100% 크기로 돌아갑니다.

### <a name="pending"></a>보류 중

사용자가 임계값을 초과한 목록을 끌어오면 시각화 도우미의 상태가 **보류 중**이 됩니다.

- 사용자가 해제 없이 임계값을 초과한 목록을 끌어 오면 **상호 작용 중** 상태로 돌아갑니다.
- 사용자가 목록을 해제하면 새로 고침 요청이 시작되면서 상태가 **새로 고침 중**으로 전환됩니다.

![임계값에 도달한 후 당겨서 새로 고침](images/ptr-postthreshold.png)

시각적으로 아이콘의 크기와 불투명도가 모두 100%입니다. 이 상태에서 아이콘은 스크롤 조작 시 계속해서 아래로 이동하지만, 더 이상 회전은 하지 않습니다.

### <a name="refreshing"></a>새로 고침 중

사용자가 임계값을 초과한 시각화 도우미를 해제하면 상태가 **새로 고침 중**이 됩니다.

이 상태에 들어가면 **RefreshRequested** 이벤트가 발생 합니다. 이는 앱의 콘텐츠 새로 고침이 시작된다는 것을 알리는 신호입니다 이벤트 인수([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs))에는 [지연](/uwp/api/windows.foundation.deferral) 개체가 포함되어 있는데, 이 개체는 이벤트 처리기에서 관리해야 합니다. 그런 다음, 새로 고침을 수행하기 위한 코드가 완료되면 지연을 완료로 표시해야 합니다.

새로 고침이 완료되면 시각화 도우미가 **유휴** 상태로 돌아갑니다.

시각적으로 아이콘은 임계값 위치로 다시 돌아오고 새로 고침 시간 동안 회전합니다. 이러한 회전은 새로 고침의 진행율을 표시하는 데 사용되고, 수신 콘텐츠에 대한 애니메이션으로 대체됩니다.

### <a name="peeking"></a>보기 중

사용자가 새로 고침이 허용되지 않는 시작 위치에서 새로 고침 방향으로 목록을 끌어오면 시각화 도우미는 **보기 중** 상태가 됩니다. 일반적으로 사용자가 끌어오기를 시작할 때 ScrollViewer가 위치 0에 있지 않은 경우에 이런 상황이 발생합니다.

- 이 상태에 있는 동안 사용자가 컨트롤을 해제하면 해당 컨트롤이 **유휴** 상태로 돌아갑니다.

## <a name="pull-direction"></a>끌어오기 방향

기본적으로 사용자는 새로 고침을 시작하기 위해 위에서 아래로 목록을 끌어옵니다. 목록이나 그리드가 다른 방향을 향하고 있으면 이에 맞춰 새로 고침 컨테이너의 끌어오기 방향을 변경해야 합니다.

[PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) 속성은 **BottomToTop**, **TopToBottom**, **RightToLeft**, **LeftToRight** 같은 [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection) 값 중 하나를 가져옵니다.

끌어오기 방향이 변경되면 시각화 도우미의 진행율 스피너의 시작 부분이 자동으로 회전되어 끌어오기 방향에 적합한 위치에서 화살표가 시작됩니다. 필요할 경우 [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) 속성을 변경하여 자동 동작을 재정의할 수 있습니다. 대부분의 경우에는 기본값을 **자동**으로 그대로 두는 것이 좋습니다.

## <a name="implement-pull-to-refresh"></a>당겨서 새로 고침 구현

당겨서 새로 고침을 기능을 목록에 추가하려면 몇 가지 단계만 거치면 됩니다.

1. **RefreshContainer** 컨트롤에서 목록을 래핑합니다.
1. 콘텐츠 새로 고침을 위해 **RefreshRequested** 이벤트를 처리합니다.
1. 원할 경우 **RequestRefresh**를 호출하여(예: 단추 클릭을 통해) 새로 고침을 시작합니다.

> [!NOTE]
> 사용자가 자체적으로 RefreshVisualizer를 시작할 수 있습니다. 하지만 터치를 지원하지 않는 시나리오라 하더라도 RefreshContainer에 콘텐츠를 래핑하고 RefreshContainer.Visualizer 속성에서 제공되는 RefreshVisualizer를 사용하는 것이 좋습니다. 이 문서에서는 시각화 도우미를 항상 새로 고침 컨테이너에서 얻는다고 가정합니다.

> 또한 편의 상 새로 고침 컨테이너의 RequestRefresh 및 RefreshRequested 멤버를 사용합니다. `refreshContainer.RequestRefresh()` 이는 `refreshContainer.Visualizer.RequestRefresh()`에 해당하며, 이로 인해 RefreshContainer.RefreshRequested 이벤트와 RefreshVisualizer.RefreshRequested 이벤트가 모두 발생합니다.

### <a name="request-a-refresh"></a>새로 고침 요청

새로 고침 컨테이너는 사용자가 터치를 통해 콘텐츠를 새로 고침할 수 있도록 터치 조작을 처리합니다. 새로 고침 단추나 음성 컨트롤과 같이 터치가 지원되지 않는 인터페이스에는 다른 어포던스를 제공하는 것이 좋습니다.

새로 고침을 시작하려면 [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh) 메서드를 호출합니다.

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

RequestRefresh를 호출하면 시각화 도우미 상태가 **유휴**에서 **새로 고침 중**으로 직접 전환됩니다.

### <a name="handle-a-refresh-request"></a>새로 고침 요청 처리

필요에 따라 새 콘텐츠를 가져오려면 RefreshRequested 이벤트를 처리합니다. 이벤트 처리기에서 새로 고침 콘텐츠를 다운로드하려면 사용자 앱과 관련된 코드가 필요합니다.

이벤트 인수([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs))에는 [지연](/uwp/api/windows.foundation.deferral) 개체가 포함되어 있습니다. 이벤트 처리기에서 이러한 지연을 관리합니다. 그런 다음, 새로 고침을 수행하기 위한 코드가 완료되면 지연을 완료로 표시합니다.

```csharp
// See the Examples section for the full code.
private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
{
    // Respond to a request by performing a refresh and using the deferral object.
    using (var RefreshCompletionDeferral = args.GetDeferral())
    {
        // Do some async operation to refresh the content

         await FetchAndInsertItemsAsync(3);

        // The 'using' statement ensures the deferral is marked as complete.
        // Otherwise, you'd call
        // RefreshCompletionDeferral.Complete();
        // RefreshCompletionDeferral.Dispose();
    }
}
```

### <a name="respond-to-state-changes"></a>상태 변경에 응답

필요한 경우, 시각화 도우미의 상태의 변화에 응답할 수 있습니다. 예를 들어, 새로 고침 요청이 여러 개 발생하는 것을 막기 위해 시각화 도우미가 새로 고침 중인 상태에서 새로 고침 단추를 비활성화할 수 있습니다.

```csharp
// See the Examples section for the full code.
private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
{
    // Respond to visualizer state changes.
    // Disable the refresh button if the visualizer is refreshing.
    if (args.NewState == RefreshVisualizerState.Refreshing)
    {
        RefreshButton.IsEnabled = false;
    }
    else
    {
        RefreshButton.IsEnabled = true;
    }
}
```

## <a name="examples"></a>예

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>RefreshContainer에서 ScrollViewer 사용

이 예제는 스크롤 뷰어에서 당겨서 새로 고침을 사용하는 방법을 보여줍니다.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>ListView에 당겨서 새로 고침 추가

이 예제는 목록 보기에서 당겨서 새로 고침을 사용하는 방법을 보여줍니다.

```xaml
<StackPanel Margin="0,40" Width="280">
    <CommandBar OverflowButtonVisibility="Collapsed">
        <AppBarButton x:Name="RefreshButton" Click="RefreshButtonClick"
                      Icon="Refresh" Label="Refresh"/>
        <CommandBar.Content>
            <TextBlock Text="List of items" 
                       Style="{StaticResource TitleTextBlockStyle}"
                       Margin="12,8"/>
        </CommandBar.Content>
    </CommandBar>

    <RefreshContainer x:Name="RefreshContainer">
        <ListView x:Name="ListView1" Height="400">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <Grid Height="80">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <TextBlock Text="{x:Bind Path=Header}"
                                   Style="{StaticResource SubtitleTextBlockStyle}"
                                   Grid.Row="0"/>
                        <TextBlock Text="{x:Bind Path=Date}"
                                   Style="{StaticResource CaptionTextBlockStyle}"
                                   Grid.Row="1"/>
                        <TextBlock Text="{x:Bind Path=Body}"
                                   Style="{StaticResource BodyTextBlockStyle}"
                                   Grid.Row="2"
                                   Margin="0,4,0,0" />
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RefreshContainer>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<ListItemData> Items { get; set; } 
        = new ObservableCollection<ListItemData>();

    public MainPage()
    {
        this.InitializeComponent();

        Loaded += MainPage_Loaded;
        ListView1.ItemsSource = Items;
    }

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Loaded -= MainPage_Loaded;
        RefreshContainer.RefreshRequested += RefreshContainer_RefreshRequested;
        RefreshContainer.Visualizer.RefreshStateChanged += Visualizer_RefreshStateChanged;

        // Add some initial content to the list.
        await FetchAndInsertItemsAsync(2);
    }

    private void RefreshButtonClick(object sender, RoutedEventArgs e)
    {
        RefreshContainer.RequestRefresh();
    }

    private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
    {
        // Respond to a request by performing a refresh and using the deferral object.
        using (var RefreshCompletionDeferral = args.GetDeferral())
        {
            // Do some async operation to refresh the content

            await FetchAndInsertItemsAsync(3);

            // The 'using' statement ensures the deferral is marked as complete.
            // Otherwise, you'd call
            // RefreshCompletionDeferral.Complete();
            // RefreshCompletionDeferral.Dispose();
        }
    }

    private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
    {
        // Respond to visualizer state changes.
        // Disable the refresh button if the visualizer is refreshing.
        if (args.NewState == RefreshVisualizerState.Refreshing)
        {
            RefreshButton.IsEnabled = false;
        }
        else
        {
            RefreshButton.IsEnabled = true;
        }
    }

    // App specific code to get fresh data.
    private async Task FetchAndInsertItemsAsync(int updateCount)
    {
        for (int i = 0; i < updateCount; ++i)
        {
            // Simulate delay while we go fetch new items.
            await Task.Delay(1000);
            Items.Insert(0, GetNextItem());
        }
    }

    private ListItemData GetNextItem()
    {
        return new ListItemData()
        {
            Header = "Header " + DateTime.Now.Second.ToString(),
            Date = DateTime.Now.ToLongDateString(),
            Body = DateTime.Now.ToLongTimeString()
        };
    }
}

public class ListItemData
{
    public string Header { get; set; }
    public string Date { get; set; }
    public string Body { get; set; }
}
```

## <a name="related-articles"></a>관련 문서

- [터치 조작](../input/touch-interactions.md)
- [목록 보기 및 그리드 보기](listview-and-gridview.md)
- [항목 컨테이너 및 템플릿](item-containers-templates.md)
- [식 애니메이션](../../composition/composition-animation.md)
