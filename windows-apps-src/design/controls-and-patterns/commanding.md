---
title: UWP(유니버설 Windows 플랫폼) 앱의 명령
description: 현재 사용하는 디바이스 및 입력 형식에 관계없이, XamlUICommand 및 StandardUICommand 클래스를 ICommand 인터페이스와 함께 사용하여 다양한 컨트롤 형식에서 명령을 공유하고 관리할 수 있습니다.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 07/23/2019
ms.openlocfilehash: 338cae7b6238c3c773f409322600c8bee8c193f5
ms.sourcegitcommit: 401c8ecaf74eee247f1ed0093028cc6558b4a605
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68446375"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>StandardUICommand, XamlUICommand 및 ICommand를 사용하는 UWP(유니버설 Windows 플랫폼) 앱의 명령

이 토픽에서는 UWP(유니버설 Windows 플랫폼) 애플리케이션의 명령에 대해 설명합니다. 특히, 사용하는 디바이스 및 입력 형식에 관계없이 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 및 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스를 ICommand 인터페이스와 함께 사용하여 다양한 컨트롤 형식에서 명령을 공유하고 관리하는 방법을 살펴보겠습니다.

![공유 명령의 일반적인 용도를 나타내는 다이어그램: '즐겨찾기' 명령이 있는 다중 UI 화면](images/commanding/generic-commanding.png)

*디바이스 및 입력 형식에 관계 없이 다양한 컨트롤 간에 명령 공유*

## <a name="important-apis"></a>중요 API

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 및 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>개요

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

단추를 클릭하거나 상황에 맞는 메뉴에서 항목을 선택하는 등의 UI 상호 작용을 통해 명령을 직접 호출할 수 있습니다. 바로 가기 키, 제스처, 음성 인식, 자동화/접근성 도구 등의 입력 디바이스를 통해 간접적으로 호출할 수도 있습니다. 호출한 후에는 컨트롤(편집 컨트롤의 텍스트 탐색), 창(뒤로 탐색) 또는 애플리케이션(종료)을 통해 명령을 처리할 수 있습니다.

명령은 텍스트를 삭제하거나 작업을 취소하는 등의 경우처럼 앱 내의 특정 상황에서 작동할 수도 있고, 오디오 음소거나 밝기 조정처럼 상황에 관계 없이 작동할 수도 있습니다.

다음 이미지에서는 동일한 명령 중 일부를 공유하는 두 개의 명령 인터페이스([CommandBar](app-bars.md) 및 움직이는 상황별 [CommandBarFlyout](command-bar-flyout.md))를 보여 줍니다.

![Microsoft 사진의 명령 모음](images/control-examples/command-bar-photos.png)<br>*Microsoft 사진의 명령 모음*

![Microsoft 사진 갤러리의 상황에 맞는 메뉴](images/ContextMenu_example.png)<br>*Microsoft 사진 갤러리의 상황에 맞는 메뉴*

## <a name="command-interactions"></a>명령 상호 작용

다양한 디바이스, 입력 형식 및 UI 화면이 명령 호출 방법에 영향을 미칠 수 있기 때문에 최대한 많은 명령 화면을 통해 명령을 노출하는 것이 좋습니다. 여기에는 [살짝 밀기](swipe.md), [MenuBar](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md) 및 전통적인 [ 상황에 맞는 메뉴](menus.md) 조합이 포함될 수 있습니다.

**중요한 명령에는 입력 관련 가속기를 사용합니다.** 입력 가속기는 사용자가 사용하는 입력 디바이스에 따라 작업을 더 빠르게 수행할 수 있게 도와줍니다.

다음은 다양한 입력 형식에 제공되는 일반적인 입력 가속기입니다.

- **포인터** - 마우스 및 펜 호버 단추
- **키보드** - 바로 가기(액세스 키 및 가속기 키)
- **터치** - 살짝 밀기
- **터치** - 당겨서 데이터 새로 고침

애플리케이션의 기능을 보편적으로 사용할 수 있도록 입력 형식과 사용자 환경을 고려해야 합니다. 예를 들어 컬렉션(특히 사용자가 편집 가능한 컬렉션)에는 일반적으로 입력 디바이스에 따라 전혀 다른 방법으로 수행되는 다양한 명령이 포함됩니다.

다음 표는 몇 가지 일반적인 컬렉션 명령과 이러한 명령을 노출하는 방법을 보여줍니다.

| 명령          | 입력과 무관 | 마우스 가속기 | 바로 가기 키 | 터치 가속기 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 항목 삭제      | 상황에 맞는 메뉴   | 호버 단추      | DEL 키              | 살짝 밀어 삭제   |
| 항목에 플래그 지정        | 상황에 맞는 메뉴   | 호버 단추      | Ctrl+Shift+G         | 살짝 밀어 플래그 지정     |
| 데이터 새로 고침     | 상황에 맞는 메뉴   | 해당 없음               | F5 키               | 당겨서 새로 고침   |
| 즐겨찾기에 항목 추가 | 상황에 맞는 메뉴   | 호버 단추      | F, Ctrl+S            | 살짝 밀어 즐겨찾기에 추가 |

**상황에 맞는 메뉴를 항상 제공** 관련된 모든 상황에 맞는 명령을 기존 상황에 맞는 메뉴 또는 CommandBarFlyout에 포함하는 것이 좋습니다. 둘 다 모든 입력 형식을 지원합니다. 예를 들어 명령이 포인터 가리키기 이벤트 중에만 노출되는 경우에는 터치 전용 디바이스에 사용할 수 없습니다.

## <a name="commands-in-uwp-applications"></a>UWP 애플리케이션의 명령

UWP 애플리케이션에서 명령 환경을 공유하고 관리할 수 있는 몇 가지 방법이 있습니다. 클릭 같은 표준 상호 작용에 대한 이벤트 처리기를 코드 숨김에 정의하거나(이 방법은 UI의 복잡성에 따라 매우 비효율적일 수 있음), 표준 상호 작용에 대한 이벤트 수신기를 바인딩하거나, 컨트롤의 Command 속성을 명령 논리를 설명하는 ICommand 구현에 바인딩할 수 있습니다.

코드 중복을 최소화하면서 효율적으로 명령 화면에 풍부하고 포괄적인 사용자 환경을 제공하려면 이 토픽에서 설명하는 명령 바인딩 기능을 사용하는 것이 좋습니다(표준 이벤트 처리는 개별 이벤트 토픽 참조).

컨트롤을 공유 명령 리소스에 바인딩하려면 ICommand 인터페이스를 직접 구현하거나, [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 기본 클래스 또는 [ StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 파생 클래스가 정의하는 플랫폼 명령 중 하나로 명령을 빌드하면 됩니다.

- ICommand 인터페이스([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 또는 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand))를 사용하면 완전히 사용자 지정된 재사용 가능한 명령을 앱에 만들 수 있습니다.
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)도 이 기능을 제공하지만, 명령 동작, 바로 가기 키(액세스 키 및 가속기 키), 아이콘, 레이블, 설명 등의 기본 제공 명령 속성을 노출하여 개발을 간소화합니다.
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)는 속성이 미리 정의되어 있는 표준 플랫폼 명령 세트에서 선택할 수 있으므로 더욱 간단합니다.

> [!Important]
> UWP 애플리케이션에서는 선택하는 언어 프레임워크에 따라 명령이 [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)(C++) 또는 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)(C#) 인터페이스로 구현됩니다.

## <a name="command-experiences-using-the-standarduicommand-class"></a>StandardUICommand 클래스를 사용하는 명령 환경

[XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)에서 파생되는(C++는 [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)에서 파생되고, C#은 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)에서 파생) [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스는 아이콘, 키보드 바로 가기 키, 설명 같은 속성이 미리 정의되어 있는 표준 플랫폼 명령 세트를 노출합니다.

[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)는 `Save` 또는 `Delete`처럼 일반적인 명령을 빠르고 일관적으로 정의할 수 있는 방법을 제공합니다. 개발자는 execute 및 canExecute 함수만 제공하면 됩니다.

### <a name="example"></a>예

![StandardUICommand 샘플](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| 이 예제의 코드 다운로드 |
| -------------------- |
| [UWP 명령 샘플(StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

이 예제에서는 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스를 통해 구현된 항목 삭제 명령으로 기본 [ListView](listview-and-gridview.md)를 개선하는 방법과 [MenuBar](menus.md), [살짝 밀기](swipe.md) 컨트롤, 호버 단추 및 [상황에 맞는 메뉴](menus.md)를 사용하여 다양한 입력 형식에 맞게 사용자 환경을 최적화하는 방법을 보여줍니다.

> [!NOTE]
> 이 샘플에는 [Microsoft Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)의 일부인 Microsoft.UI.Xaml.Controls NuGet 패키지가 필요합니다.

**Xaml:**

샘플 UI에는 5개 항목의 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)가 포함되어 있습니다. Delete [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)는 [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 및 [ ContextFlyout 메뉴](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)에 바인딩됩니다.

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**코드 숨김**

1. 먼저, ListView의 각 ListViewItem에 대한 텍스트 문자열과 ICommand를 포함하는 `ListItemData` 클래스를 정의합니다.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. MainPage [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)의 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)에 대한 `ListItemData` 개체 컬렉션을 정의합니다. 그런 다음, 처음 5개 항목의 컬렉션으로 채웁니다(텍스트 및 연결된 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) Delete를 사용하여).

```csharp
/// <summary>
/// ListView item collection.
/// </summary>
ObservableCollection<ListItemData> collection = 
    new ObservableCollection<ListItemData>();

/// <summary>
/// Handler for the layout Grid control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    // Create the standard Delete command.
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(
            new ListItemData {
                Text = "List item " + i.ToString(),
                Command = deleteCommand });
    }
}

/// <summary>
/// Handler for the ListView control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    // Populate the ListView with the item collection.
    listView.ItemsSource = collection;
}
```

3. 다음으로, ICommand ExecuteRequested 처리기를 정의하고 항목 삭제 명령을 구현합니다.

``` csharp
/// <summary>
/// Handler for the Delete command.
/// </summary>
/// <param name="sender">Source of the command event</param>
/// <param name="e">Event args for the command event</param>
private void DeleteCommand_ExecuteRequested(
    XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    // If possible, remove specfied item from collection.
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 마지막으로, [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) 및 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트를 포함하여 다양한 ListView 이벤트에 대한 처리기를 정의합니다. 포인터 이벤트 처리기는 각 항목의 삭제 단추를 표시하거나 숨기는 데 사용됩니다.

```csharp
/// <summary>
/// Handler for the ListView selection changed event.
/// </summary>
/// <param name="sender">Source of the selection changed event</param>
/// <param name="e">Event args for the selection changed event</param>
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

/// <summary>
/// Handler for the pointer entered event.
/// Displays the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer entered event</param>
/// <param name="e">Event args for the pointer entered event</param>
private void ListViewSwipeContainer_PointerEntered(
    object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(
            sender as Control, "HoverButtonsShown", true);
    }
}

/// <summary>
/// Handler for the pointer exited event.
/// Hides the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer exited event</param>
/// <param name="e">Event args for the pointer exited event</param>

private void ListViewSwipeContainer_PointerExited(
    object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(
        sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>XamlUICommand 클래스를 사용하는 명령 환경

[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스에 정의되지 않은 명령을 만들어야 하거나 명령의 모양을 보다 정교하게 제어하려는 경우 [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스는 [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 인터페이스에서 파생되어 UI 및 사용자 지정 명령의 동작을 신속하게 정의할 수 있는 다양한 UI 속성(예: 아이콘, 레이블, 설명, 바로 가기 키), 메서드 및 이벤트를 추가합니다.

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)를 사용하면 개별 속성을 설정하지 않고 아이콘, 레이블, 설명, 바로 가기 키(액세스 키 및 키보드 가속기 둘 다) 같은 컨트롤 바인딩을 통해 UI를 지정할 수 있습니다.

### <a name="example"></a>예

![XamlUICommand 샘플](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| 이 예제의 코드 다운로드 |
| -------------------- |
| [UWP 명령 샘플(XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

이 예제에서는 이전 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 예제의 삭제 기능을 공유하지만, [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스를 사용하여 개발자 고유의 글꼴, 아이콘, 레이블, 키보드 가속기 및 설명으로 사용자 지정 삭제 명령을 정의하는 방법을 보여줍니다. [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 예제와 마찬가지로, [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스를 통해 구현된 항목 삭제 명령으로 기본 [ListView](listview-and-gridview.md)를 개선하고 [MenuBar](menus.md), [살짝 밀기](swipe.md) 컨트롤, 호버 단추 및 [상황에 맞는 메뉴](menus.md)를 사용하여 다양한 입력 형식에 맞게 사용자 환경을 최적화합니다.

이전 섹션의 StandardUICommand 예제와 마찬가지로, 많은 플랫폼 컨트롤이 XamlUICommand 속성을 내부적으로 사용합니다. 

> [!NOTE]
> 이 샘플에는 [Microsoft Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)의 일부인 Microsoft.UI.Xaml.Controls NuGet 패키지가 필요합니다.

**Xaml:**

샘플 UI에는 5개 항목의 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)가 포함되어 있습니다. 사용자 지정 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 삭제는 [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 및 [ContextFlyout 메뉴](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)에 바인딩됩니다.

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" ExecuteRequested="DeleteCommand_ExecuteRequested" Description="Custom XamlUICommand" Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**코드 숨김**

1. 먼저, ListView의 각 ListViewItem에 대한 텍스트 문자열과 ICommand를 포함하는 `ListItemData` 클래스를 정의합니다.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. MainPage [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)의 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)에 대한 `ListItemData` 개체 컬렉션을 정의합니다. 그런 다음, 처음 5개 항목의 컬렉션으로 채웁니다(텍스트 및 연결된 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)를 사용하여).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 다음으로, ICommand ExecuteRequested 처리기를 정의하고 항목 삭제 명령을 구현합니다.

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. 마지막으로, [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) 및 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트를 포함하여 다양한 ListView 이벤트에 대한 처리기를 정의합니다. 포인터 이벤트 처리기는 각 항목의 삭제 단추를 표시하거나 숨기는 데 사용됩니다.

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>ICommand 인터페이스를 사용하는 명령 환경

표준 UWP 컨트롤(단추, 목록, 선택, 일정, 예측 텍스트)은 여러 일반 명령 환경을 위한 기초를 제공합니다. 컨트롤 형식의 전체 목록은 [UWP 앱 컨트롤 및 패턴](index.md)을 참조하세요.

체계적인 명령 환경을 지원하는 가장 기본적인 방법은 ICommand 인터페이스 구현을 정의하는 것입니다(C++는 [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand), C#은 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)).  이 ICommand 인스턴스를 단추 같은 컨트롤에 바인딩할 수 있습니다.

> [!NOTE]
> 경우에 따라 메서드를 Click 이벤트에 바인딩하고 속성을 IsEnabled 속성에 바인딩하는 것이 효과적일 수 있습니다.

#### <a name="example"></a>예

![명령 인터페이스 예제](images/commanding/icommand.gif)

*ICommand 예제*

| 이 예제의 코드 다운로드 |
| -------------------- |
| [UWP 명령 샘플(ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

이 기본 예제에서는 단추 클릭, 바로 가기 키, 마우스 휠 회전을 통해 단일 명령을 호출하는 방법을 보여줍니다.

[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)를 두 개 사용합니다. 하나는 5개 항목과 다른 빈 항목 그리고 단추 2개로 채워져 있고 왼쪽에 있는 ListView의 항목을 오른쪽에 있는 ListView로 이동하는 데 사용되며, 다른 하나는 오른쪽에서 왼쪽으로 항목을 이동하는 데 사용됩니다. 각 단추는 해당 명령에 바인딩되고(각각 ViewModel.MoveRightCommand 및 ViewModel.MoveLeftCommand), 연결된 ListView의 항목 수에 따라 자동으로 설정/해제됩니다.

**다음 XAML 코드는 이 예제의 UI를 정의합니다.**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,0,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.ListItemLeft.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.ListItemRight.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**다음은 이전 UI의 코드 숨김입니다.**

코드 숨김에서 명령 코드를 포함하는 보기 모델을 연결합니다. 또한 마우스 휠의 입력을 처리하는 처리기를 정의합니다. 이 처리기 역시 명령 코드를 연결합니다.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**다음은 보기 모델의 코드입니다.**

보기 모델에서 앱의 두 명령에 대한 실행 세부 정보를 정의하고, 한 ListView를 채우고, 각 ListView의 항목 수에 따라 추가 UI를 숨기거나 표시하는 불투명도 값 변환기를 제공합니다.

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand MoveLeftCommand { get; private set; }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand MoveRightCommand { get; private set; }

        // Item collections
        public ObservableCollection<ListItemData> ListItemLeft { get; } = new ObservableCollection<ListItemData>();
        public ObservableCollection<ListItemData> ListItemRight { get; } = new ObservableCollection<ListItemData>();

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            MoveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            MoveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItem.ListItemText = "Item " + (ListItemLeft.Count + 1).ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.Add(listItem);
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return ListItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return ListItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (ListItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                ListItemRight.Add(listItem);
                listItem.ListItemText = "Item " + ListItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.RemoveAt(ListItemLeft.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (ListItemRight.Count > 0)
            {
                listItem = new ListItemData();
                ListItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + ListItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemRight.RemoveAt(ListItemRight.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// Views subscribe to this event to get notified of property updates.
        /// </summary>
        public event PropertyChangedEventHandler PropertyChanged;

        /// <summary>
        /// Notify subscribers of updates to the named property
        /// </summary>
        /// <param name="propertyName">The full, case-sensitive, name of a property.</param>
        protected void NotifyPropertyChanged(string propertyName)
        {
            PropertyChangedEventHandler handler = this.PropertyChanged;
            if (handler != null)
            {
                PropertyChangedEventArgs args = new PropertyChangedEventArgs(propertyName);
                handler(this, args);
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The count passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**마지막으로, 다음은 ICommand 인터페이스의 구현입니다.**

여기서는 [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 인터페이스를 구현하고 해당 기능을 다른 개체에 전달하는 명령을 정의합니다.

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>요약

유니버설 Windows 플랫폼은 컨트롤 형식, 디바이스 및 입력 형식에 관계 없이 명령을 공유하고 관리하는 앱을 빌드할 수 있는 강력하고 유연한 명령 시스템을 제공합니다.

UWP 앱의 명령을 만들 때 다음 방법을 사용합니다.

- XAML/코드 숨김의 이벤트를 수신 대기하고 처리합니다.
- Click 같은 이벤트 처리 메서드에 바인딩
- 개발자 고유의 ICommand 구현을 정의
- 미리 정의된 속성 세트에 개발자 고유의 값을 사용하여 XamlUICommand 개체 만들기
- 미리 정의된 플랫폼 속성 및 값 세트를 사용하여 StandardUICommand 개체 만들기

## <a name="next-steps"></a>다음 단계

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 및 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 구현을 보여주는 전체 예제는 [XAML 컨트롤 갤러리](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) 샘플을 참조하세요.

## <a name="see-also"></a>참고 항목

[UWP 앱 컨트롤 및 패턴](index.md)

### <a name="samples"></a>샘플

#### <a name="topic-samples"></a>토픽 샘플

- [UWP 명령 샘플(StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [UWP 명령 샘플(XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [UWP 명령 샘플(ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>기타 샘플

- [유니버설 Windows 플랫폼 샘플(C# 및 C++)](https://go.microsoft.com/fwlink/?linkid=832713)
- [XAML 컨트롤 갤러리](https://github.com/Microsoft/Xaml-Controls-Gallery)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->
