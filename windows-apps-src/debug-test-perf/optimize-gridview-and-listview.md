---
ms.assetid: 26DF15E8-2C05-4174-A714-7DF2E8273D32
title: ListView 및 GridView UI 최적화
description: UI 가상화, 요소 감소, 항목에 대한 점진적 업데이트를 통해 ListView 및 GridView의 성능과 시작 시간을 개선합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73da4a2a590c5f1d860bb480c6d81b01e5e93819
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210079"
---
# <a name="listview-and-gridview-ui-optimization"></a>ListView 및 GridView UI 최적화


**참고**   자세한 내용은 //build/ 세션 [사용자가 GridView 및 ListView에서 많은 데이터를 조작할 때의 획기적인 성능 향상](https://channel9.msdn.com/events/build/2013/3-158)을 참조하세요.

UI 가상화, 요소 감소, 항목에 대한 점진적 업데이트를 통해 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)의 성능과 시작 시간을 개선합니다. 데이터 가상화 기술은 [ListView 및 GridView 데이터 가상화](listview-and-gridview-data-optimization.md)를 참조하세요.

## <a name="two-key-factors-in-collection-performance"></a>컬렉션 성능의 두 가지 주요 요소

컬렉션 조작은 일반적인 시나리오입니다. 사진 뷰어에는 사진 컬렉션이 있고, 판독기에는 기사/책/스토리 컬렉션이 있으며, 쇼핑 앱에는 제품 컬렉션이 있습니다. 이 항목에서는 컬렉션 조작에 효율적인 앱을 만들기 위해 수행할 수 있는 작업을 보여 줍니다.

컬렉션의 경우 두 가지 주요 성능 요소가 있습니다. 하나는 UI 스레드가 항목을 만드는 데 걸리는 시간이며 다른 하나는 원시 데이터 집합 및 해당 데이터를 렌더링하는 데 사용되는 UI 요소 모두에 사용되는 메모리입니다.

원활한 이동/스크롤을 위해서는 UI 스레드가 항목을 인스턴스화, 데이터 바인딩 및 배치하는 작업을 효율적이고 지능적으로 수행해야 합니다.

## <a name="ui-virtualization"></a>UI 가상화

UI 가상화는 성능을 개선할 수 있는 가장 중요한 기능입니다. 이는 항목을 나타내는 UI 요소가 필요에 따라 만들어짐을 의미합니다. 1000개 항목의 컬렉션에 바인딩된 항목 컨트롤의 경우 동시에 모든 항목에 대한 UI를 만드는 것은 리소스 낭비입니다. 왜냐하면 이들을 동시에 모두 표시할 수 없기 때문입니다. [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)(및 기타 표준 [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 파생 컨트롤)는 UI 가상화를 수행합니다. 항목이 보기에 가까이(몇 페이지 밖) 스크롤되면 프레임워크가 항목에 대한 UI를 생성하고 이를 캐시합니다. 항목이 다시 표시될 것 같지 않은 경우 프레임워크는 메모리를 회수합니다.

사용자 지정 항목 패널 템플릿([**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) 참조)을 제공하는 경우 [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) 또는 [**ItemsStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel)과 같은 가상화 패널을 사용해야 합니다. [  **VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid), [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) 또는 [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)을 사용하는 경우에는 가상화할 수 없습니다. 또한 다음 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 이벤트는 [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) 또는 [**ItemsStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel)을 사용하는 경우에만 발생합니다. [**ChoosingGroupHeaderContainer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer), [**ChoosingItemContainer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) 및 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging).

프레임워크가 표시될 수 있는 요소를 만들어야 하므로 뷰포트 개념이 UI 가상화에 중요합니다. 일반적으로 [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)의 뷰포트는 논리적 컨트롤 크기입니다. 예를 들어 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)의 뷰포트는 **ListView** 요소의 너비 및 높이입니다. 일부 패널에서는 자동 크기 조정 행 또는 열을 사용하여 자식 요소(예: [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 및 [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid))에 무제한 공간을 허용합니다. 가상화된 **ItemsControl**이 이와 같은 패널에 배치된 경우 모든 항목을 표시할 수 있는 공간을 차지하므로 가상화에 실패합니다. 이 경우 **ItemsControl**에서 너비와 높이를 설정하여 가상화를 복원합니다.

## <a name="element-reduction-per-item"></a>항목별 요소 감소

항목을 렌더링하는 데 사용되는 UI 요소 수를 적절한 최소값으로 유지합니다.

항목 컨트롤이 처음 표시되면 전체 항목의 뷰포트를 렌더링하는 데 필요한 모든 요소가 만들어집니다. 또한 항목이 뷰포트에 근접하면 프레임워크가 바인딩된 데이터 개체를 사용하여 캐시된 항목 템플릿에서 UI 요소를 업데이트합니다. 템플릿 내의 태그 복잡성을 최소화하면 UI 스레드에서 소요되는 메모리 및 시간이 줄어들어 특히 이동/스크롤하는 동안 응답성이 향상됩니다. 이러한 템플릿은 항목 템플릿([**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 참조)과 [**ListViewItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) 또는 [**GridViewItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem)(항목 컨트롤 템플릿 또는 [**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle))의 컨트롤 템플릿입니다. 요소 수를 조금만 줄여도 표시되는 항목 수가 배로 늘어납니다.

요소 감소의 예제는 [XAML 태그 최적화](optimize-xaml-loading.md)를 참조하세요.

[  **ListViewItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) 및 [**GridViewItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem)의 기본 컨트롤 템플릿은 [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) 요소를 포함합니다. 이 프리젠터는 포커스, 선택 항목 및 다른 시각적 상태에 대한 복잡한 시각 요소를 표시하는 최적화된 단일 요소입니다. 사용자 지정 항목 컨트롤 템플릿([**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle))이 이미 있는 경우 또는 나중에 항목 컨트롤 템플릿의 복사본을 편집하려는 경우 **ListViewItemPresenter**를 사용하는 것이 좋습니다. 이 요소는 대부분의 경우 성능과 사용자 지정성 간에 최적화된 균형을 제공합니다. 프리젠터는 해당 속성을 설정하여 사용자 지정합니다. 예를 들어 다음은 항목을 선택할 때 기본적으로 표시되는 확인란을 제거하고 선택한 항목의 배경색을 주황색으로 변경하는 태그입니다.

```xml
...
<ListView>
 ...
 <ListView.ItemContainerStyle>
 <Style TargetType="ListViewItem">
 <Setter Property="Template">
 <Setter.Value>
 <ControlTemplate TargetType="ListViewItem">
 <ListViewItemPresenter SelectionCheckMarkVisualEnabled="False" SelectedBackground="Orange"/>
 </ControlTemplate>
 </Setter.Value>
 </Setter>
 </Style>
 </ListView.ItemContainerStyle>
</ListView>
<!-- ... -->
```

[  **SelectionCheckMarkVisualEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) 및 [**SelectedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedbackground)와 유사한 자체 설명 이름을 가진 약 25개의 속성이 있습니다. 프리젠터 형식이 사용 사례에 맞게 사용자 지정할 수 없는 것으로 증명된 경우 대신 `ListViewItemExpanded` 또는 `GridViewItemExpanded` 컨트롤 템플릿의 복사본을 편집할 수 있습니다. 이러한 템플릿은 `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`에서 찾을 수 있습니다. 이러한 템플릿을 사용하면 사용자 지정성이 증가하는 대신 성능이 저하됩니다.

<span id="update-items-incrementally"/>

## <a name="update-listview-and-gridview-items-progressively"></a>점진적으로 ListView 및 GridView 항목 업데이트

데이터 가상화를 사용하는 경우 로드 중인 항목에 대한 임시 UI 요소를 렌더링하도록 컨트롤을 구성하여 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 및 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)의 응답성을 높게 유지할 수 있습니다. 임시 요소는 데이터가 로드되면서 점점 실제 UI로 대체됩니다.

또한 데이터를 로드하는 위치(로컬 디스크, 네트워크 또는 클라우드)에 상관없이 사용자는 원활한 이동/스크롤을 유지하면서 각 항목을 완전한 충실도로 렌더링할 수 없을 정도로 빠르게 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 또는 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)를 이동/스크롤할 수 있습니다. 원활한 이동/스크롤을 유지하기 위해 자리 표시자를 사용하는 것 외에 여러 단계에서 항목을 렌더링할 수 있습니다.

이러한 기술의 예는 종종 사진 보기 앱에서 볼 수 있습니다. 일부 이미지가 로드 및 표시되지 않은 경우에도 사용자는 계속 이동/스크롤하면서 컬렉션을 조작할 수 있습니다. 또는 "영화" 항목의 경우 첫 번째 단계에서는 제목, 두 번째 단계에서는 평점, 세 번째 단계에서는 포스터 이미지를 표시할 수 있습니다. 사용자에게는 각 항목에 대한 가장 중요한 데이터가 최대한 빨리 표시되므로 한 번에 작업을 수행할 수 있습니다. 그런 다음 시간이 남으면 중요하지 않은 정보가 채워집니다. 다음은 이러한 기술을 구현하는 데 사용할 수 있는 플랫폼 기능입니다.

### <a name="placeholders"></a>자리 표시자

임시 자리 표시자 시각 요소 기능은 기본적으로 켜져 있으며 [**ShowsScrollingPlaceholders**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 속성을 통해 제어됩니다. 빠른 이동/스크롤 중에 이 기능은 원활함을 유지하면서 완전히 표시할 항목이 아직 더 있음을 알려 주는 시각적 힌트를 사용자에게 제공합니다. 아래 기법 중 하나를 사용하는 경우 시스템이 자리 표시자를 렌더링하지 않게 하려면 **ShowsScrollingPlaceholders**를 false로 설정할 수 있습니다.

**x:Phase를 사용한 점진적 데이터 템플릿 업데이트**

[{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 바인딩과 함께 [x:Phase 특성](https://docs.microsoft.com/windows/uwp/xaml-platform/x-phase-attribute)을 사용하여 점진적 데이터 템플릿 업데이트를 구현하는 방법은 다음과 같습니다.

1.  다음은 바인딩 원본의 모양입니다(바인딩할 데이터 원본임).

    ```csharp
    namespace LotsOfItems
    {
        public class ExampleItem
        {
            public string Title { get; set; }
            public string Subtitle { get; set; }
            public string Description { get; set; }
        }

        public class ExampleItemViewModel
        {
            private ObservableCollection<ExampleItem> exampleItems = new ObservableCollection<ExampleItem>();
            public ObservableCollection<ExampleItem> ExampleItems { get { return this.exampleItems; } }

            public ExampleItemViewModel()
            {
                for (int i = 1; i < 150000; i++)
                {
                    this.exampleItems.Add(new ExampleItem(){
                        Title = "Title: " + i.ToString(),
                        Subtitle = "Sub: " + i.ToString(),
                        Description = "Desc: " + i.ToString()
                    });
                }
            }
        }
    }
    ```
2.  다음은 `DeferMainPage.xaml`에 포함된 태그입니다. 그리드 보기에는 **MyItem** 클래스의 **Title**, **Subtitle** 및 **Description** 속성에 바인딩된 요소와 함께 항목 템플릿이 포함됩니다. **x:Phase**는 기본적으로 0으로 설정됩니다. 즉, 항목이 처음에는 제목만 표시되도록 렌더링됩니다. 그런 다음 자막 요소가 데이터 바인딩되고 모든 단계가 처리될 때까지 모든 항목에 대해 표시됩니다.
    ```xml
    <Page
        x:Class="LotsOfItems.DeferMainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Text="{x:Bind Subtitle}" x:Phase="1"/>
                            <TextBlock Text="{x:Bind Description}" x:Phase="2"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```

3.  이제 앱을 시작하고 그리드 보기를 통해 신속하게 이동/스크롤하면 각각의 새 항목이 화면에 표시될 때 먼저 어두운 회색의 직사각형([**ShowsScrollingPlaceholders**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 속성이 기본적으로 **true**로 설정되었기 때문)으로 렌더링된 다음 제목, 자막 및 설명이 차례로 표시됩니다.

**ContainerContentChanging을 사용한 점진적 데이터 템플릿 업데이트**

[  **ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트에 대한 일반적인 전략은 **Opacity**을 사용하여 즉시 표시하지 않아도 되는 요소를 숨기는 것입니다. 요소가 재활용될 때는 이전 값을 유지하므로 새 데이터 항목에서 이러한 값을 업데이트할 때까지 이러한 요소를 숨길 수 있습니다. 이벤트 인수에서 **Phase** 속성을 사용하여 업데이트 및 표시할 이벤트를 결정합니다. 추가 단계가 필요한 경우 콜백을 등록합니다.

1.  **x:Phase**에 대해 동일한 바인딩 소스를 사용합니다.

2.  다음은 `MainPage.xaml`에 포함된 태그입니다. 그리드 보기는 처리기를 해당 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트에 선언하고 **MyItem** 클래스의 **Title**, **Subtitle** 및 **Description** 속성을 표시하는 데 사용되는 요소와 함께 항목 템플릿을 포함합니다. **ContainerContentChanging**을 사용할 경우의 성능 이점을 극대화하기 위해 태그에서 바인딩을 사용하지 않고 대신 값을 프로그래밍 방식으로 할당합니다. 단, 제목을 표시하는 요소는 예외입니다. 이 요소는 0단계에서 고려합니다.
    ```xml
    <Page
        x:Class="LotsOfItems.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}" ContainerContentChanging="GridView_ContainerContentChanging">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Opacity="0"/>
                            <TextBlock Opacity="0"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```
3.  마지막으로, 다음은 [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) 이벤트 처리기의 구현입니다. 이 코드는 **RecordingViewModel** 형식의 속성을 **MainPage**에 추가하여 태그 페이지를 나타내는 클래스에서 바인딩 소스 클래스를 노출하는 방법을 보여 줍니다. 데이터 템플릿에 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 바인딩이 없는 경우 처리기의 첫 번째 단계에서 처리될 때 이벤트 인수 개체를 표시하여 항목에 데이터 컨텍스트를 설정하지 않아도 된다는 힌트를 제공합니다.
    ```csharp
    namespace LotsOfItems
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new ExampleItemViewModel();
            }

            public ExampleItemViewModel ViewModel { get; set; }

            // Display each item incrementally to improve performance.
            private void GridView_ContainerContentChanging(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 0)
                {
                    throw new System.Exception("We should be in phase 0, but we are not.");
                }

                // It's phase 0, so this item's title will already be bound and displayed.

                args.RegisterUpdateCallback(this.ShowSubtitle);

                args.Handled = true;
            }

            private void ShowSubtitle(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 1)
                {
                    throw new System.Exception("We should be in phase 1, but we are not.");
                }

                // It's phase 1, so show this item's subtitle.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[1] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Subtitle;
                textBlock.Opacity = 1;

                args.RegisterUpdateCallback(this.ShowDescription);
            }

            private void ShowDescription(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 2)
                {
                    throw new System.Exception("We should be in phase 2, but we are not.");
                }

                // It's phase 2, so show this item's description.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[2] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Description;
                textBlock.Opacity = 1;
            }
        }
    }
    ```

4.  이제 앱을 실행하고 그리드 보기를 통해 신속하게 이동/스크롤하면 **x:Phase**에 대해 동일한 동작이 표시됩니다.

## <a name="container-recycling-with-heterogeneous-collections"></a>다른 유형의 컬렉션에서 컨테이너 재생

일부 애플리케이션에서는 컬렉션 내의 다른 항목 유형에 대해 다른 UI가 있어야 합니다. 이로 인해 가상화 패널에서 항목을 표시하는 데 사용되는 시각적 요소를 재사용/재활용하는 것이 불가능한 상황이 생길 수 있습니다. 이동 중 항목에 대한 시각적 요소를 다시 만들면 가상화에서 제공하는 성능 향상의 많은 이점이 취소됩니다. 그러나 조금만 계획하면 가상화 패널에서 요소를 재사용할 수 있습니다. 개발자는 시나리오에 따라 두 가지 옵션, 즉 [**ChoosingItemContainer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer)이벤트 또는 항목 템플릿 선택기의 옵션이 있습니다. **ChoosingItemContainer**응 성능이 더 우수한 접근 방식입니다.

**ChoosingItemContainer 이벤트**

[**ChoosingItemContainer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer)는 시작 또는 재활용 중에 새 항목이 필요할 때마다 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)/[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)에 항목(**ListViewItem**/**GridViewItem**)을 제공할 수 있는 이벤트입니다. 컨테이너가 표시하는 데이터 항목 형식에 따라 컨테이너를 만들 수 있습니다(아래 예제에 표시됨). **ChoosingItemContainer**는 다른 항목에 대해 다른 데이터 템플릿을 사용하는 성능이 우수한 방법입니다. 컨테이너 캐싱은 **ChoosingItemContainer**를 사용하여 얻을 수 있는 기능입니다. 예를 들어 5개의 다른 템플릿이 있고 한 템플릿이 다른 템플릿보다 더 자주 발생하는 경우 ChoosingItemContainer를 통해 필요한 비율로 항목을 만들 수 있을 뿐만 아니라 재활용을 위해 적절한 요소 수를 캐시하여 사용할 수 있도록 유지할 수 있습니다. [**ChoosingGroupHeaderContainer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer)는 그룹 헤더에 대해 동일한 기능을 제공합니다.

```csharp
// Example shows how to use ChoosingItemContainer to return the correct
// DataTemplate when one is available. This example shows how to return different 
// data templates based on the type of FileItem. Available ListViewItems are kept
// in two separate lists based on the type of DataTemplate needed.
private void ListView_ChoosingItemContainer
    (ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    // Determines type of FileItem from the item passed in.
    bool special = args.Item is DifferentFileItem;

    // Uses the Tag property to keep track of whether a particular ListViewItem's 
    // datatemplate should be a simple or a special one.
    string tag = special ? "specialFiles" : "simpleFiles";

    // Based on the type of datatemplate needed return the correct list of 
    // ListViewItems, this could have also been handled with a hash table. These 
    // two lists are being used to keep track of ItemContainers that can be reused.
    List<UIElement> relevantStorage = special ? specialFileItemTrees : simpleFileItemTrees;

    // args.ItemContainer is used to indicate whether the ListView is proposing an 
    // ItemContainer (ListViewItem) to use. If args.Itemcontainer, then there was a 
    // recycled ItemContainer available to be reused.
    if (args.ItemContainer != null)
    {
        // The Tag is being used to determine whether this is a special file or 
        // a simple file.
        if (args.ItemContainer.Tag.Equals(tag))
        {
            // Great: the system suggested a container that is actually going to 
            // work well.
        }
        else
        {
            // the ItemContainer's datatemplate does not match the needed 
            // datatemplate.
            args.ItemContainer = null;
        }
    }

    if (args.ItemContainer == null)
    {
        // see if we can fetch from the correct list.
        if (relevantStorage.Count > 0)
        {
            args.ItemContainer = relevantStorage[0] as SelectorItem;
        }
        else
        {
            // there aren't any (recycled) ItemContainers available. So a new one 
            // needs to be created.
            ListViewItem item = new ListViewItem();
            item.ContentTemplate = this.Resources[tag] as DataTemplate;
            item.Tag = tag;
            args.ItemContainer = item;
        }
    }
}
```

**항목 템플릿 선택기**

항목 템플릿 선택기([**DataTemplateSelector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DataTemplateSelector))를 사용하면 런타임 시 표시되는 데이터 항목의 형식에 따라 앱에서 다른 항목 템플릿을 반환할 수 있습니다. 그러면 개발은 더 생산적이 되지만 모든 항목 템플릿을 모든 데이터 항목에 재사용할 수 있는 것은 아니기 때문에 UI 가상화는 더 어려워질 수 있습니다.

항목(**ListViewItem**/**GridViewItem**)을 재활용하는 경우 프레임워크는 재활용 큐(재활용 큐는 데이터를 표시하는 데 현재 사용되지 않는 항목의 캐시임)에서 사용할 수 있는 항목에 현재 데이터 항목에서 원하는 항목과 일치하는 항목 템플릿이 있는지 여부를 결정해야 합니다. 재활용 큐에 적절한 항목 템플릿을 가진 항목이 없는 경우 새 항목이 만들어지고 적절한 항목 템플릿이 이를 위해 인스턴스화됩니다. 반면에 재활용 큐에 적절한 항목 템플릿을 가진 항목이 포함된 경우에는 해당 항목이 재활용 큐에서 제거되고 현재 데이터 항목에 사용됩니다. 항목 템플릿 선택기는 소수의 항목 템플릿만 사용하고 다른 항목 템플릿을 사용하는 항목 컬렉션 전체에서 균등 배포가 있는 상황에서 작동합니다.

다른 항목 템플릿을 사용하는 균등하지 않은 항목 배포가 있는 경우 이동 중 새 항목 템플릿이 만들어져야 하며 그렇게 되면 가상화에서 제공되는 많은 이점을 사용할 수 없습니다. 게다가 항목 템플릿 선택기는 특정 컨테이너를 현재 데이터 항목에 재사용할 수 있는 여부를 평가할 때 다섯 가지의 가능한 후보만 고려합니다. 따라서 앱에서 사용하기 전에 데이터가 항목 템플릿 선택기에서 사용하기에 적절한지 여부를 주의 깊게 고려해야 합니다. 컬렉션이 대부분 같은 유형인 경우에는 선택기가 거의 또는 항상 같은 유형을 반환합니다. 이러한 동질성에 대한 드문 예외에 치러야 할 대가를 인식하고 [**ChoosingItemContainer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer)(또는 두 개의 항목 컨트롤)를 사용하는 것이 바람직한지 고려합니다.

 

