---
title: 유니버설 Windows 플랫폼 (UWP) 앱 명령
description: ICommand 인터페이스) (함께 XamlUICommand 및 StandardUICommand 클래스를 사용 하 여 공유 하 고 다양 한 장치에 관계 없이 컨트롤 형식 및 사용 되는 입력된 형식에 걸쳐 명령을 관리 하는 방법.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 11/01/2018
ms.author: kbridge
ms.openlocfilehash: 32d5005f9965b14d5080344832eb185f0e711689
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646528"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>StandardUICommand, XamlUICommand, 및 ICommand를 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에서 명령 실행

이 항목에서는 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에서 명령을 설명 합니다. 특히 사용 하는 방법을 설명 했습니다 합니다 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 하 고 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (ICommand 인터페이스)와 함께 공유 하 고 다양 한 컨트롤 형식에 관계 없이에서 명령을 관리 클래스 장치 및 입력된 형식이 사용 됩니다.

![공유 명령에 대 한 일반적인 용도 나타내는 다이어그램: '즐겨찾기' 명령을 사용 하 여 여러 UI를 표시 합니다.](images/commanding/generic-commanding.png)

*장치 및 입력된 형식에 관계 없이 다양 한 컨트롤 간 공유 명령*

## <a name="important-apis"></a>중요 API

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 고 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>개요

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

명령 단추를 클릭 하거나 상황에 맞는 메뉴에서 항목을 선택 하면 같은 UI 상호 작용을 통해 직접 호출할 수 있습니다. 또한 호출할 수 있습니다 하지 직접 키보드 액셀러레이터, 제스처, 음성 인식 하는 자동화/내게 필요한 옵션 도구 등 입력된 장치를 통해. 호출 되 면 명령을 처리할 수 있습니다 (편집 컨트롤에서 텍스트 탐색), 컨트롤에서 (뒤로 탐색) 창 또는 응용 프로그램 (종료).

명령 텍스트를 삭제 하거나 작업을 취소 하는 등, 앱 내에서 특정 컨텍스트에 대해 작동할 수 있습니다 또는 상황에 맞는 없는 무료, 예: 오디오 음소거 또는 밝기 조정 될 수 있습니다.

다음 이미지는 두 개의 명령 인터페이스를 보여 줍니다 (을 [CommandBar](app-bars.md) 상황에 맞는 부동 및 [CommandBarFlyout](command-bar-flyout.md)) 같은 명령의 대부분을 공유 하는 합니다.

![명령 인터페이스 예제](images/commanding/command-interface-example.png)

## <a name="command-interactions"></a>명령 상호 작용

다양 한 장치, 입력된 형식 및 명령을 호출 하는 방법을 영향을 주는 UI 화면을 인해 최대한 많은 명령 화면을 통해 명령을 노출 하는 것이 좋습니다. 조합을 포함할 수 있습니다 [안쪽으로 살짝 밀어](swipe.md), [MenuBar](menus.md)를 [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md), 및 기존 [ 상황에 맞는 메뉴](menus.md)합니다.

**중요 한 명령에 대 한 입력 관련 가속기를 사용 합니다.** 입력된 가속기는 사용자를 사용 하는 입력된 장치에 따라 더 빠르게 작업을 수행할 수 있습니다.

다양 한 입력된 형식에 대 한 몇 가지 일반적인 입력된 가속기는 다음과 같습니다.

- **포인터** -열기 (&) 마우스 호버 단추
- **키보드** -바로 가기 (액세스 키 및 액셀러레이터 키)
- **터치** -살짝 밀기
- **터치** -데이터를 새로 고치려면 끌어오기

응용 프로그램의 기능을 보편적으로 액세스할 수 있도록 하려면 입력된 형식 및 사용자 환경을 고려해 야 합니다. 예를 들어, 컬렉션 (특히 사용자 편집 가능한 것)는 일반적으로 다양 한 입력된 장치에 따라 상당히 다르게 수행 되는 특정 명령 포함 합니다.

다음 표에서 몇 가지 일반적인 컬렉션 명령과 해당 명령을 노출 하는 방법을 보여 줍니다.

| 명령          | 입력에 관계없는 | 마우스 가속기 | 키보드 가속기 | 터치 가속기 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 항목 삭제      | 상황에 맞는 메뉴   | 호버 단추      | DEL 키              | 살짝 밀어 삭제   |
| 항목에 플래그 지정        | 상황에 맞는 메뉴   | 호버 단추      | Ctrl+Shift+G         | 살짝 밀어 플래그 지정     |
| 데이터 새로 고침     | 상황에 맞는 메뉴   | 해당 없음               | F5 키               | 당겨서 새로 고침   |
| 즐겨찾기에 항목 추가 | 상황에 맞는 메뉴   | 호버 단추      | F, Ctrl+S            | 살짝 밀어 즐겨찾기에 추가 |

**상황에 맞는 메뉴를 항상 제공** 좋습니다 관련 상황에 맞는 명령에 모든 기존 상황에 맞는 메뉴 또는 CommandBarFlyout를 포함 하 여 모든 입력 형식을 모두 지원 됩니다. 예를 들어, 명령 포인터 가리키기 이벤트 중에 노출 되는, 경우 터치만 장치에서 사용할 수 없습니다.

## <a name="commands-in-uwp-applications"></a>UWP 응용 프로그램에는 명령

공유 하 고 UWP 응용 프로그램에서 명령 환경을 관리할 수는 몇 가지가 있습니다. 코드 숨김 (이 비효율적일 수 있습니다 매우, UI의 복잡성에 따라.)에서 클릭 같은 표준 상호 작용에 대 한 이벤트 처리기를 정의할 수 있습니다., 공유 하는 처리기를 표준 상호 작용에 대 한 이벤트 수신기를 바인딩할 수 있습니다. 또는 컨트롤을 바인딩할 수 있습니다. 명령 논리를 설명 하는 ICommand 구현 속성 명령입니다.

이 항목에서 설명 하는 기능을 바인딩 명령을 사용 하 여 좋습니다 효율적이 고 최소한의 코드 중복을 사용 하 여 명령 화면에서 풍부 하 고 포괄적인 사용자 환경을 제공 합니다 (표준 이벤트 처리에 대 한 개별 이벤트 항목 참조).

공유 명령 리소스를 컨트롤에 바인딩하려면 ICommand 인터페이스를 직접 구현할 수 있습니다 또는 명령 중 하나에서 만들 수는 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 기본 클래스 또는 정의한 플랫폼 명령 중 하나는 [ StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스를 파생 합니다.

- ICommand 인터페이스 ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 하거나 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand))을 만들 수 있습니다 완전히 사용자 지정된 앱에서 다시 사용할 수 있는 명령입니다.
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 도이 기능을 제공 하지만 명령 동작, 바로 가기 키 (선택 키 및 액셀러레이터 키), 아이콘, 레이블 및 설명 등의 기본 제공 명령 집합을 노출 하 여 개발을 간소화 합니다.
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 에서 미리 정의 된 속성을 사용 하 여 표준 플랫폼 명령 집합을 선택할 수 있도록 하 여 작업을 더욱 단순화 합니다.

> [!Important]
> UWP 응용 프로그램 명령에는의 구현 된 [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) (c + +) 또는 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#) 선택한에 따라 인터페이스 언어 프레임 워크입니다.

## <a name="command-experiences-using-the-standarduicommand-class"></a>명령은 StandardUICommand 클래스를 사용 하 여 환경을

파생 된 [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (에서 파생 된 [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) c + + 또는 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) 에 C#), 합니다 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스 아이콘, 키보드 액셀러레이터 및 설명과 같은 미리 정의 된 속성을 사용 하 여 표준 플랫폼 명령 집합을 노출 합니다.

A [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 와 같은 일반적인 명령을 정의 하는 빠르고 일관 된 방법을 제공 `Save` 또는 `Delete`합니다. Execute 및 canExecute 기능을 제공 하면 됩니다.

### <a name="example"></a>예

![StandardUICommand 샘플](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

이 예제에서는 기본을 개선 하는 방법을 알아보겠습니다 [ListView](listview-and-gridview.md) 삭제 된 항목을 통해 구현 되는 명령 합니다 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 다양 한 입력된 형식에 대 한 사용자 환경을 최적화 하는 동안 클래스 사용 하 여를 [MenuBar](menus.md), [안쪽으로 살짝 밀어](swipe.md) 컨트롤을 가리킬 때 단추, 및 [상황에 맞는 메뉴](menus.md)합니다.

> [!NOTE]
> 이 샘플의 일부 Microsoft.UI.Xaml.Controls NuGet 패키지에 필요 합니다 [Microsoft Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)합니다.

**Xaml:**

샘플 UI를 포함 한 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 5 개 항목입니다. 삭제가 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 바인딩되는 [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), 및 [ ContextFlyout 메뉴](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)합니다.

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

**Code-behind**

1. 첫째, 정의 `ListItemData` 는 ListView의 각 ListViewItem에 ICommand를 텍스트 문자열을 포함 하는 클래스입니다.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. MainPage 클래스 정의의 컬렉션 `ListItemData` 에 대 한 개체를 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) 의 합니다 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)합니다. 다음 5 개 항목의 초기 컬렉션을 사용 하 여 채우는 것 (텍스트를 사용 하 여 연결 하 고 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 삭제).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = deleteCommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. 다음으로, 항목 삭제 명령을 구현 ICommand ExecuteRequested 처리기를 정의 합니다.

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

4. 마지막으로 포함 하 여 다양 한 ListView 이벤트에 대 한 처리기 정의 [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)를 [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), 및 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트입니다. 포인터 이벤트 처리기는 각 항목에 대 한 Delete 단추 표시 또는 숨기기 위해 사용 됩니다.

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

## <a name="command-experiences-using-the-xamluicommand-class"></a>명령은 XamlUICommand 클래스를 사용 하 여 환경을

정의 되지 않은 명령을 생성 해야 하는 경우는 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 클래스에 더 많은 명령 모양 제어 하고자 합니다 [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스에서 파생 되는 [ ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 인터페이스를 다양 한 UI (예: 아이콘, 레이블, 설명 및 바로 가기 키) 속성, 메서드 및 UI 및 사용자 지정 명령 동작을 신속 하 게 정의 하는 이벤트를 추가 합니다.

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 아이콘 같은 바인딩 컨트롤을 통해 UI를 지정할 수 있습니다 레이블, 설명 및 바로 가기 키 (둘 다 액세스 키 및 키보드 액셀러레이터)를 개별 속성을 설정 하지 않고 있습니다.

### <a name="example"></a>예

![XamlUICommand 샘플](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

이 예제에서는 이전 삭제 기능을 공유 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 예, 하지만 표시 하는 방법을 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스를 사용 하면 사용자 고유의 글꼴 아이콘을 사용 하 여 사용자 지정 삭제 명령을 정의 레이블 키보드 액셀러레이터 및 설명 합니다. 같은 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 예제에서는 기본 강화 [ListView](listview-and-gridview.md) 삭제를 사용 하 여 항목을 통해 구현 되는 명령 합니다 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 클래스를 최적화 하는 동안는 입력된 형식 사용 하 여 다양 한 사용자 환경을 [MenuBar](menus.md), [안쪽으로 살짝 밀어](swipe.md) 컨트롤을 가리킬 때 단추, 및 [상황에 맞는 메뉴](menus.md).

많은 플랫폼 컨트롤 내부적으로 이전 섹션에서 StandardUICommand 예제 처럼 XamlUICommand 속성을 사용 합니다. 

> [!NOTE]
> 이 샘플의 일부 Microsoft.UI.Xaml.Controls NuGet 패키지에 필요 합니다 [Microsoft Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)합니다.

**Xaml:**

샘플 UI를 포함 한 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 5 개 항목입니다. 사용자 지정 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) Delete 바인딩되는 [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), 및 [ ContextFlyout 메뉴](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)합니다.

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

**Code-behind**

1. 첫째, 정의 `ListItemData` 는 ListView의 각 ListViewItem에 ICommand를 텍스트 문자열을 포함 하는 클래스입니다.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. MainPage 클래스 정의의 컬렉션 `ListItemData` 에 대 한 개체를 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) 의 합니다 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)합니다. 다음 5 개 항목의 초기 컬렉션을 사용 하 여 채우는 것 (텍스트를 사용 하 여 연결 하 고 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)).

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

3. 다음으로, 항목 삭제 명령을 구현 ICommand ExecuteRequested 처리기를 정의 합니다.

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

4. 마지막으로 포함 하 여 다양 한 ListView 이벤트에 대 한 처리기 정의 [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)를 [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), 및 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 이벤트입니다. 포인터 이벤트 처리기는 각 항목에 대 한 Delete 단추 표시 또는 숨기기 위해 사용 됩니다.

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

## <a name="command-experiences-using-the-icommand-interface"></a>ICommand 인터페이스를 사용 하 여 명령 환경

다양 한 일반적인 명령 환경을 위한 기초를 제공 하는 표준 UWP 컨트롤 (단추, 목록, 선택, 일정, 예측 텍스트)입니다. 컨트롤 형식의 전체 목록은 참조 하세요 [컨트롤 및 UWP 앱에 대 한 패턴](index.md)합니다.

가장 기본적인 방법은 구조화 된 명령 환경을 지원 하도록 ICommand 인터페이스 구현의 정의 하는 것 ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) c + + 또는 [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)에 대 한 C#).  이 ICommand 인스턴스 단추와 같은 컨트롤을 바인딩할 수 있습니다.

> [!NOTE]
> 경우에 따라 효율적 메서드 클릭 이벤트와 IsEnabled 속성에 속성에 바인딩할 수 있습니다.

#### <a name="example"></a>예

![명령 인터페이스 예제](images/commanding/icommand.gif)

*ICommand 예제*
 
이 예제에서는 단일 명령 단추를 사용 하 여 호출할 수 있습니다 하는 방법을 설명 클릭, 키보드 액셀러레이터를 및 마우스 휠 회전 합니다.

두 개 사용 하 여 [Listview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), 5 개 항목 및 다른 빈 및 이동 항목을 ListView에서 왼쪽에서 오른쪽의 ListView에 대해 하나씩 두 개의 단추를 사용 하 여 채워진 하나 및 왼쪽 오른쪽에서 이동 하는 것에 대 한 다른 항목을 합니다. 각 단추는 해당 명령에 연결 (ViewModel.MoveRightCommand 및 ViewModel.MoveLeftCommand, 각각), 및 설정 되며 해당 연결 된 ListView의 항목 수에 따라 자동으로 사용 하지 않도록 설정 합니다.

**다음 XAML 코드 예제에 대 한 UI를 정의합니다.**

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
        <Grid Grid.Column="1" Margin="0,0,5,0"
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
                          Opacity="{x:Bind Path=ViewModel.listItemLeft.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton" ToolTipService.ToolTip="Tooltip"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand, Mode=OneWay}">
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
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand, Mode=OneWay}">
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
                          Opacity="{x:Bind Path=ViewModel.listItemRight.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
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

**이전 UI에 대 한 코드 숨김은 다음과 같습니다.**

코드 숨김에서 명령 코드를 포함 하는 보기 모델에 연결 합니다. 또한 마우스 휠도 명령 코드를 연결 하는 입력에 대 한 처리기를 정의 합니다.

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

**뷰 모델에서 코드는 같습니다.**

보기 모델 위치 이며에서는 앱에서 두 개의 명령에 대 한 실행 정보를 정의 하나의 ListView를 채우는 숨기 거 나 각 ListView의 항목 개수에 따라 몇 가지 추가 UI 표시에 대 한 불투명도 값 변환기를 제공 합니다.

```csharp
using System;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.ComponentModel;
using Windows.UI.Xaml;
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
    public class UICommand1ViewModel : INotifyPropertyChanged
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand moveLeftCommand;
        public RelayCommand MoveLeftCommand { get => moveLeftCommand; private set { } }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand moveRightCommand;
        public RelayCommand MoveRightCommand { get => moveRightCommand; private set { } }

        // Item collections
        public ObservableCollection<ListItemData> listItemLeft;
        public ObservableCollection<ListItemData> ListItemLeft { get => listItemLeft; private set { } }
        public ObservableCollection<ListItemData> listItemRight;
        public ObservableCollection<ListItemData> ListItemRight { get => listItemRight; private set { } }

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            moveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            moveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            listItemLeft = new ObservableCollection<ListItemData>();
            listItemRight = new ObservableCollection<ListItemData>();

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
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return listItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return listItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (listItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                listItemRight.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemLeft.RemoveAt(listItemLeft.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (listItemRight.Count > 0)
            {
                listItem = new ListItemData();
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemRight.RemoveAt(listItemRight.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
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
        /// <param name="value">The bool passed in</param>
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

**마지막으로,이 ICommand 인터페이스 구현의 같습니다.**

여기에 구현 하는 명령을 정의 합니다 [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 중계 하 여 다른 개체에 해당 기능 및 인터페이스입니다.

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

유니버설 Windows 플랫폼을 제공 하면 강력 하 고 유연한 명령 시스템 공유 및 명령 컨트롤 형식, 장치 및 입력된 형식을 관리 하는 앱을 빌드합니다.

UWP 앱에 대 한 명령을 만들 때 다음 방법 중 하나를 사용 합니다.

- 수신 대기 하 고 XAML/코드 숨김에서 이벤트를 처리 합니다.
- 클릭 같은 메서드를 처리 하는 이벤트에 바인딩
- 사용자 지정 ICommand 구현을 정의 합니다.
- 미리 정의 된 속성 집합에 대 한 고유한 값을 사용 하 여 XamlUICommand 개체 만들기
- 미리 정의 된 플랫폼 속성 및 값의 집합을 사용 하 여 StandardUICommand 개체 만들기

## <a name="next-steps"></a>다음 단계

보여 주는 전체 예제는 [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) 및 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 구현 참조를 [XAML 컨트롤 갤러리](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) 샘플입니다.

## <a name="see-also"></a>참고 항목

[컨트롤 및 UWP 앱에 대 한 패턴](index.md)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->