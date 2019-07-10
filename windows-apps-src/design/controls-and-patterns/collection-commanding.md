---
description: 이 문서에서는 상황에 맞는 명령을 사용하여 모든 입력 형식에 대한 최상의 환경을 제공하는 방식으로 이러한 종류의 작업을 구현하는 방법을 보여줍니다.
title: 상황에 맞는 명령
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1d520f811c9929721bfcb9d1c83fbff6a4891091
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63801188"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>컬렉션 및 목록에 대한 상황에 맞는 명령



많은 앱에는 사용자가 조작할 수 있는 목록, 표 및 트리 형식의 콘텐츠 컬렉션이 있습니다. 예를 들어 사용자가 항목을 삭제하거나, 항목의 이름을 바꾸거나, 항목에 플래그를 지정하거나, 항목을 새로 고칠 수 있습니다. 이 문서에서는 상황에 맞는 명령을 사용하여 모든 입력 형식에 대한 최상의 환경을 제공하는 방식으로 이러한 종류의 작업을 구현하는 방법을 보여줍니다.  

> **중요 API**: [ICommand 인터페이스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand), [UIElement.ContextFlyout 속성](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), [INotifyPropertyChanged 인터페이스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![다양한 입력을 사용하여 즐겨찾기 명령 수행](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>모든 입력 형식에 대한 명령 만들기

사용자가 [다양한 디바이스와 입력](../devices/index.md)을 사용하여 UWP 앱과 상호 작용할 수 있으므로, 앱은 입력에 관계 없는 상황에 맞는 메뉴와 입력과 관련된 가속기를 통해 명령을 노출해야 합니다. 둘 다 포함하면 사용자가 입력 또는 디바이스 유형에 관계 없이 콘텐츠에 대한 명령을 신속하게 호출할 수 있습니다.

다음 표에는 몇 가지 일반적인 컬렉션 명령과 이러한 명령을 노출하는 방법이 나와 있습니다. 

| 명령          | 입력과 무관 | 마우스 가속기 | 바로 가기 키 | 터치 가속기 |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| 항목 삭제      | 상황에 맞는 메뉴   | 호버 단추      | DEL 키              | 살짝 밀어 삭제   |
| 항목에 플래그 지정        | 상황에 맞는 메뉴   | 호버 단추      | Ctrl+Shift+G         | 살짝 밀어 플래그 지정     |
| 데이터 새로 고침     | 상황에 맞는 메뉴   | 해당 없음               | F5 키               | 당겨서 새로 고침   |
| 즐겨찾기에 항목 추가 | 상황에 맞는 메뉴   | 호버 단추      | F, Ctrl+S            | 살짝 밀어 즐겨찾기에 추가 |


* **일반적으로 항목의 [상황에 맞는 메뉴](menus.md)에서 해당 항목에 대한 모든 명령을 사용할 수 있게 만들어야 합니다.** 상황에 맞는 메뉴는 입력 형식에 관계 없이 사용자가 액세스할 수 있으며 사용자가 수행할 수 있는 상황에 맞는 메뉴를 모두 포함해야 합니다.

* **자주 액세스하는 명령의 경우 입력 가속기 사용을 고려하세요.** 입력 가속기는 사용자가 입력 디바이스를 기반으로 빠르게 작업을 수행할 수 있도록 도와줍니다. 다음과 같은 입력 가속기가 있습니다.
    - 살짝 밀어 작업 수행(터치 가속기)
    - 당겨서 데이터 새로 고침(터치 가속기)
    - 바로 가기 키(키보드 가속기)
    - 액세스 키(바로 가기 키)
    - 마우스 및 펜 호버 단추(포인터 가속기)

> [!NOTE]
> 사용자가 모든 유형의 디바이스에서 모든 명령에 액세스할 수 있어야 합니다. 예를 들어 앱의 명령이 호버 단추 포인터 가속기를 통해서만 노출되는 경우 터치 사용자가 해당 명령에 액세스할 수 없습니다. 최소한 상황에 맞는 메뉴를 사용하여 모든 명령에 대한 액세스를 제공해야 합니다.  

## <a name="example-the-podcastobject-data-model"></a>예제: PodcastObject 데이터 모델

추천 명령을 보여주기 위해 이 문서에서는 팟캐스트 앱의 팟캐스트 목록을 만듭니다. 예제 코드에서는 사용자가 목록의 특정 팟캐스트를 "즐겨찾기에 추가"할 수 있도록 설정하는 방법을 보여줍니다.

다음은 이 문서에서 작업할 팟캐스트 개체에 대한 정의입니다. 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

사용자가 IsFavorite 속성을 전환할 때 속성 변경에 응답하도록 PodcastObject가 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)를 구현함을 확인할 수 있습니다.

## <a name="defining-commands-with-the-icommand-interface"></a>ICommand 인터페이스로 명령 정의

[ICommand 인터페이스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)로 여러 입력 형식에 사용 가능한 명령을 정의할 수 있습니다. 예를 들어 사용자가 Delete 키를 누르는 경우와 상황에 맞는 메뉴에서 "삭제"를 마우스 오른쪽 단추로 클릭하는 경우에 대해 각각 하나씩 총 두 가지 이벤트 처리기의 삭제 명령에 똑같은 코드를 작성하는 대신, [ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)로 삭제 논리를 한 번에 구현한 다음, 여러 입력 형식에 사용할 수 있습니다.

"즐겨찾기에 추가" 작업을 나타내는 ICommand를 정의해야 합니다. 명령의 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 메서드를 사용하여 즐겨찾기에 팟캐스트를 추가하겠습니다. CommandParameter 속성을 사용하여 바인딩할 수 있는 명령의 매개 변수를 통해 실행 명령에 특정 팟캐스트가 제공됩니다.

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

여러 컬렉션과 요소에 동일한 명령을 사용하려면 페이지 또는 앱에 명령을 리소스로 저장하면 됩니다.

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

명령을 실행하려면 해당 [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) 메서드를 호출합니다.

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>UserControl을 만들어 다양한 입력에 응답

항목 목록이 있고 각 항목이 여러 입력에 응답해야 하는 경우 항목에 대한 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)을 정의하고 이를 통해 항목의 상황에 맞는 메뉴와 이벤트 처리기를 정의하여 코드를 단순화할 수 있습니다. 

Visual Studio에서 UserControl을 만들려면 다음을 수행합니다.
1. 솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭합니다. 상황에 맞는 메뉴가 나타납니다.
2. **추가 > 새 항목...** 을 선택합니다. <br />**새 항목 추가** 대화 상자가 나타납니다. 
3. 항목 목록에서 UserControl을 선택합니다. 원하는 이름을 지정하고 **추가**를 클릭합니다. Visual Studio에서 자동으로 스텁 UserControl을 생성합니다. 

팟캐스트 예에서는 팟캐스트를 "즐겨찾기에 추가"하는 다양한 방식을 노출하는 목록에 각 팟캐스트가 표시됩니다. 사용자는 다음 작업을 수행하여 팟캐스트를 "즐겨찾기에 추가"할 수 있습니다.
- 상황에 맞는 메뉴 호출
- 키보드 바로 가기 수행
- 호버 단추 표시
- 살짝 밀기 제스처 수행

이러한 동작을 캡슐화하고 FavoriteCommand를 사용하기 위해 이름이 "PodcastUserControl"인 새 [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl)을 생성하여 목록에 팟캐스트를 표시하겠습니다.

PodcastUserControl은 PodcastObject의 필드를 TextBlock으로 표시하고 다양한 사용자 상호 작용에 응답합니다. 이 문서에서는 PodcastUserControl을 참조하고 확장하겠습니다.

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

PodcastUserControl은 PodcastObject에 대한 참조를 DependencyProperty로 유지합니다. 이를 통해 PodcastUserControl에 PodcastObject를 바인딩할 수 있습니다.

PodcastObject를 몇 개 생성한 후 ListView에 PodcastObject를 바인딩하여 팟캐스트 목록을 만들 수 있습니다. PodcastUserControl 개체는 PodcastObject의 시각화를 설명하므로 ListView의 ItemTemplate을 사용하여 설정됩니다.

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>상황에 맞는 메뉴 만들기

상황에 맞는 메뉴는 사용자 요청에 따라 명령 또는 옵션 목록을 표시합니다. 상황에 맞는 메뉴는 연결된 요소와 관련된 상황에 맞는 명령을 제공하며, 해당 항목에 특정한 보조 작업용으로 예약되어 있습니다.

![항목에 상황에 맞는 메뉴 표시](images/ContextualCommand_RightClick.png)

사용자는 다음과 같은 "상황에 맞는 작업"을 사용하여 상황에 맞는 메뉴를 호출할 수 있습니다.

| 입력    | 상황에 맞는 작업                          |
| -------- | --------------------------------------- |
| 마우스    | 마우스 오른쪽 단추로 클릭                             |
| 키보드 | Shift+F10, 메뉴 단추                  |
| 터치    | 항목 길게 누르기                      |
| 펜      | 펜 단추 누르기, 항목 길게 누르기 |
| 게임 패드  | 메뉴 버튼                             |

**사용자가 입력 형식에 관계 없이 상황에 맞는 메뉴를 열 수 있으므로, 상황에 맞는 메뉴에는 목록 항목에 사용할 수 있는 상황에 맞는 명령이 모두 들어 있어야 합니다.**

### <a name="contextflyout"></a>ContextFlyout

UIElement 클래스에서 정의한 [ContextFlyout 속성](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout)을 사용하면 모든 입력 형식에서 작동하는 상황에 맞는 메뉴를 쉽게 만들 수 있습니다. MenuFlyout을 사용하여 상황에 맞는 메뉴를 나타내는 플라이아웃을 제공하면 사용자가 위에 정의된 "상황에 맞는 작업"을 수행할 때 항목에 해당하는 MenuFlyout이 표시됩니다.

PodcastUserControl에 ContextFlyout을 추가하겠습니다. ContextFlyout으로 지정된 MenuFlyout에는 팟캐스트를 즐겨찾기에 추가하기 위한 단일 항목이 들어 있습니다. 이 MenuFlyoutItem은 PodcastObject에 CommandParamter가 바인딩된 상태로 위에 정의된 favoriteCommand를 사용합니다.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

이제 [ContextRequested 이벤트](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested)를 사용하여 상황에 맞는 작업에 응답할 수도 있습니다. ContextFlyout이 지정된 경우 ContextRequested 이벤트가 실행되지 않습니다.

## <a name="creating-input-accelerators"></a>입력 가속기 만들기

컬렉션의 각 항목에 상황에 맞는 명령을 모두 포함하는 상황에 맞는 메뉴가 있어야 하지만, 사용자가 더 적은 수의 자주 수행하는 명령 세트를 빠르게 수행하게 할 수 있습니다. 예를 들어 메일 앱에는 상황에 맞는 메뉴에 나타나는 회신, 보관, 폴더로 이동, 플래그 설정 및 삭제와 같은 보조 명령이 있을 수 있지만, 가장 많이 사용하는 명령은 삭제와 플래그 지정입니다. 가장 많이 사용하는 명령을 식별한 후 입력 기반의 가속기를 사용하여 이러한 명령을 사용자가 더 쉽게 수행하게 할 수 있습니다.

팟캐스트 앱에서 자주 사용하는 명령은 "즐겨찾기에 추가"입니다.

### <a name="keyboard-accelerators"></a>바로 가기 키

#### <a name="shortcuts-and-direct-key-handling"></a>바로 가기 및 직접 키 처리

![Ctrl+F를 눌러 작업 수행](images/ContextualCommand_Keyboard.png)

콘텐츠 유형에 따라 작업을 수행해야 하는 특정 키 조합을 식별할 수 있습니다. 예를 들어 이메일 앱에서 DEL 키를 사용하여 선택된 이메일을 삭제할 수 있습니다. 팟캐스트 앱에서 Ctrl+S 또는 F 키는 나중에 팟캐스트를 즐겨찾기에 추가할 수 있습니다. 일부 명령은 항목을 삭제하는 DEL처럼 잘 알려진 바로 가기를 갖고 있지만, 다른 명령은 앱이나 도메인과 관련된 바로 가기를 갖고 있습니다. 가능하면 잘 알려진 바로 가기를 사용하거나 도구 설명에 미리 알림 텍스트를 제공하여 사용자에게 바로 가기 명령에 대해 알려주세요.

사용자가 [KeyDown](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent) 이벤트를 사용하여 키를 누를 때 앱에서 응답할 수 있습니다. 일반적으로 사용자는 키를 놓을 때까지 기다리지 않고 키를 처음 누를 때 앱이 응답할 것으로 기대합니다.

이 예제에서는 PodcastUserControl에 KeyDown 처리기를 추가하여 사용자가 Ctrl+S 또는 F를 누를 때 즐겨찾기에 팟캐스트를 추가하는 방법을 안내합니다. 전과 동일한 명령이 사용됩니다.

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>마우스 가속기

![항목을 마우스로 가리켜서 단추 표시](images/ContextualCommand_HovertoReveal.png)

사용자는 마우스 오른쪽 단추를 누르면 표시되는 상황에 맞는 메뉴에 익숙하지만, 한 번의 마우스 클릭만으로 일반적인 명령을 수행하게 할 수 있습니다. 이렇게 하려면 컬렉션 항목의 캔버스에 전용 단추를 포함하면 됩니다. 사용자가 마우스를 사용하여 빠르게 작업을 수행하고 시각적 방해물을 최소화할 수 있도록 사용자가 특정 목록 항목 내에 포인터를 둘 때만 이러한 단추를 표시하도록 선택할 수 있습니다.

이 예제에서는 PodcastUserControl에 직접 정의된 단추로 즐겨찾기에 추가 명령을 표시합니다. 이 예제의 단추는 이전 명령과 동일한 FavoriteCommand 명령을 사용합니다. 이 단추의 표시 여부를 전환하려면 VisualStateManager를 사용하여 포인터가 컨트롤에 들어가고 나갈 때 시각적 상태를 전환하면 됩니다.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

마우스가 항목에 들어가고 나갈 때 호버 단추가 나타나고 사라져야 합니다. 마우스 이벤트에 응답하려면 PodcastUserControl에 [PointerEntered](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) 및 [PointerExited](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) 이벤트를 사용합니다.

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

호버 상태에 표시되는 단추는 포인터 입력 형식을 통해서만 액세스할 수 있습니다. 이러한 단추는 포인터 입력으로 제한되기 때문에 포인터 입력 최적화를 위해 단추 아이콘 주위의 여백을 최소화하거나 제거하도록 선택할 수 있습니다. 이렇게 하기로 선택하는 경우 펜 및 마우스로 사용할 수 있도록 단추 공간이 20x20px 이상이어야 합니다.

### <a name="touch-accelerators"></a>터치 가속기

#### <a name="swipe"></a>살짝 밀기

![항목을 살짝 밀어 명령 표시](images/ContextualCommand_Swipe.png)

살짝 밀기 명령은 터치 디바이스의 사용자가 터치를 사용하여 일반적인 보조 작업을 수행할 수 있게 하는 터치 가속기입니다. 살짝 밀기를 통해 터치 사용자는 살짝 밀어 삭제나 살짝 밀어 호출과 같은 일반적인 작업을 사용하여 빠르고 자연스럽게 콘텐츠와 상호 작용할 수 있습니다. 자세한 내용은 [swipe commanding(살짝 밀기 명령)](swipe.md) 문서를 참조하세요.

살짝 밀기를 컬렉션에 통합하려면 다음과 같은 두 가지 구성 요소가 필요합니다. 하나는 명령을 호스트하는 SwipeItems이고, 다른 하나는 항목을 래핑하고 살짝 밀기 상호 작용을 허용하는 SwipeControl입니다.

SwipeItems는 PodcastUserControl에서 리소스로 정의할 수 있습니다. 이 예제에서는 SwipeItems에 즐겨찾기에 항목을 추가하는 명령이 포함되어 있습니다.

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

SwipeControl은 항목을 줄 바꿈하고 사용자가 살짝 밀기 제스처를 사용하여 항목을 조작할 수 있게 합니다. SwipeControl에 SwipeItems에 대한 참조가 해당 RightItems로 포함되어 있습니다. 사용자가 오른쪽에서 왼쪽으로 살짝 밀면 즐겨찾기에 추가 항목이 나타납니다.

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

사용자가 살짝 밀어 즐겨찾기에 추가 명령을 호출하면 Invoked 메서드가 호출됩니다.

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>당겨서 새로 고침

당겨서 새로 고침을 사용하면 데이터 컬렉션을 터치하고 아래로 당겨서 더 많은 데이터를 검색할 수 있습니다. 자세한 내용은 [당겨서 새로 고침](pull-to-refresh.md) 문서를 참조하세요.

### <a name="pen-accelerators"></a>펜 가속기

펜 입력 형식을 사용하면 포인터로 정밀하게 입력할 수 있습니다. 사용자는 펜 기반 가속기를 사용하여 상황에 맞는 메뉴를 여는 등의 일반적인 작업을 수행할 수 있습니다. 상황에 맞는 메뉴를 열려면 사용자는 펜 단추를 누른 상태에서 화면을 탭하거나 콘텐츠를 길게 누르면 됩니다. 또한 사용자는 펜으로 콘텐츠를 가리켜서 도구 설명 표시와 같은 UI를 더 깊게 이해하거나 마우스와 비슷한 보조 호버 작업을 표시할 수 있습니다.

펜 입력에 맞게 앱을 최적화하려면 [펜 및 스타일러스 조작](../input/pen-and-stylus-interactions.md) 문서를 참조하세요.


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

* 사용자가 모든 유형의 UWP 디바이스에서 모든 명령에 액세스할 수 있게 하세요.
* 컬렉션 항목에 사용 가능한 모든 명령에 대한 액세스를 제공하는 상황에 맞는 메뉴를 포함하세요. 
* 자주 사용하는 명령에 대한 입력 가속기를 제공하세요. 
* [ICommand 인터페이스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)를 사용하여 명령을 구현하세요. 

## <a name="related-topics"></a>관련 항목
* [ICommand 인터페이스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [메뉴 및 상황에 맞는 메뉴](menus.md)
* [살짝 밀기](swipe.md)
* [당겨서 새로 고침](pull-to-refresh.md)
* [펜 및 스타일러스 조작](../input/pen-and-stylus-interactions.md)
* [게임 패드 및 Xbox에 맞게 앱 조정](../devices/designing-for-tv.md)
