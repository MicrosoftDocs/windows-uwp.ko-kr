---
Description: 단추를 사용하면 즉각적인 작업을 트리거할 수 있습니다.
title: 단추
label: Buttons
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f8d4172389fc2778fda4e335a29b3bae7d90fd0
ms.sourcegitcommit: 5fcd3a595efd3686009505602c34e10163fd0aa5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67558758"
---
# <a name="buttons"></a>단추

단추를 사용하면 즉각적인 작업을 트리거할 수 있습니다. 일부 단추는 탐색, 반복 작업, 메뉴 표시 등의 특정 작업에 맞게 전문화되어 있습니다.

![단추 예제](images/controls/button.png)

[XAML(Extensible Application Markup Language)](../../xaml-platform/xaml-overview.md) 프레임워크는 표준 단추 컨트롤뿐 아니라 여러 특수 단추 컨트롤도 제공합니다.

컨트롤 | 설명
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | 즉시 작업을 시작하는 단추입니다. [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트 또는 [Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) 바인딩에 사용할 수 있습니다.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | 누르고 있는 동안 계속해서 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트를 발생시키는 단추입니다.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | 하이퍼링크처럼 스타일이 지정되는 단추로, 탐색에 사용됩니다. 하이퍼링크에 대한 자세한 내용은 [하이퍼링크](hyperlinks.md)를 참조하세요.
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | 연결된 플라이아웃을 여는 펼침 단추가 있는 단추입니다.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | 두 면이 있는 단추입니다. 한 쪽은 작업을 시작하고, 다른 쪽은 메뉴를 엽니다.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | 두 면이 있는 토글 단추입니다. 한쪽은 켜기/끄기를 전환하고, 다른 쪽은 메뉴를 엽니다.

| **Windows UI 라이브러리 가져오기** |
| - |
| **DropDownButton**, **SplitButton** 및 **ToggleSplitButton**은 UWP 앱용 새 컨트롤과 UI 기능을 포함하고 있는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

| **플랫폼 API** | **Windows UI 라이브러리 API** |
| - | - |
| [Click 이벤트](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)<br/> [Command 속성](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [DropDownButton 클래스](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)<br/> [SplitButton 클래스](/uwp/api/microsoft.ui.xaml.controls.splitbutton)<br/> [ToggleSplitButton 클래스](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

**Button** 컨트롤을 사용하면 사용자가 양식 제출 같은 즉각적인 작업을 시작할 수 있습니다.

다른 페이지를 탐색하는 작업인 경우에는 **Button** 컨트롤을 사용하지 말고 [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) 컨트롤을 사용합니다. 하이퍼링크에 대한 자세한 내용은 [하이퍼링크](hyperlinks.md)를 참조하세요.

> [!IMPORTANT]
> 마법사 탐색인 경우에는 *뒤로* 및 *다음*이라는 레이블이 붙은 단추를 사용합니다. 역방향 탐색 또는 상위 수준 탐색에는 [뒤로 단추](../basics/navigation-history-and-backwards-navigation.md)를 사용합니다.

사용자가 작업을 반복적으로 트리거하려는 경우에는 **RepeatButton** 컨트롤을 사용합니다. 예를 들어 카운터 값을 늘리거나 줄이려면 **RepeatButton** 컨트롤을 사용합니다.

단추에 더 많은 옵션을 포함하는 플라이아웃이 있으면 **DropDownButton** 컨트롤을 사용합니다. 기본 펼침 단추는 단추에 플라이아웃이 포함되어 있다는 것을 시각적으로 나타냅니다.

사용자가 즉각적인 작업을 시작하거나 추가 옵션에서 독립적으로 선택할 수 있게 하려면 **SplitButton** 컨트롤을 사용합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML Controls Gallery</strong>가 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Button">앱을 열고 작동 중인 단추를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

이 예에서는 위치 액세스를 요청하는 대화 상자에서 **허용** 및 **차단**의 두 가지 단추를 사용합니다.

![대화 상자에서 사용되는 단추의 예제](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>단추 만들기

이 예제에서는 클릭에 응답하는 단추를 보여 줍니다.

XAML에서 단추를 만듭니다.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

또는 코드에서 단추를 만듭니다.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

[Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트를 처리합니다.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>단추 조작

손가락 또는 스타일러스로 **Button** 컨트롤을 탭하거나 포인터가 그 위에 있을 때 마우스 왼쪽 단추를 누르면 [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트가 발생합니다. 단추에 키보드 포커스가 있는 경우 Enter 키 또는 스페이스바를 눌러도 **Click** 이벤트가 발생합니다.

단추에는 **Click** 동작이 있기 때문에 일반적으로 **Button** 개체에서 하위 수준의 [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 이벤트를 처리할 수 없습니다. 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)를 참조하세요.

[ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode) 속성을 변경하여 단추가 **Click** 이벤트를 발생시키는 방법을 변경할 수 있습니다. **ClickMode**의 기본값은 **Release**이지만, 단추의 **ClickMode** 값을 **Hover** 또는 **Press**로 설정할 수도 있습니다. **ClickMode**가 **Hover**인 경우 키보드 또는 터치를 통해 **Click** 이벤트를 발생시킬 수는 없습니다.


### <a name="button-content"></a>단추 콘텐츠

**Button**은 [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) 클래스의 콘텐츠 컨트롤입니다. 단추의 XAML 콘텐츠 속성은 [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)이며 XAML에 대해 다음과 같은 구문을 가능하게 합니다. `<Button>A button's content</Button>`. 어떠한 개체라도 단추 콘텐츠로 설정할 수 있습니다. 콘텐츠가 [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 개체인 경우 단추에서 렌더링됩니다. 콘텐츠가 다른 유형의 개체인 경우 해당 문자열 표현이 단추에 표시됩니다.

단추의 콘텐츠는 일반적으로 텍스트입니다. 해당 텍스트를 디자인할 때는 다음 권장 사항을 사용합니다.

-  단추가 수행하는 작업을 명확히 설명해 주는 간결하고 구체적이며 설명적인 텍스트를 사용합니다. 일반적으로 단추 텍스트는 한 단어로 된 동사입니다.

-  브랜드 지침에 다른 글꼴을 사용하도록 규정되어 있지 않은 경우 기본 글꼴을 사용하세요.

-  텍스트가 짧은 경우에는 120px의 최소 단추 너비를 사용하여 명령 단추가 좁아지지 않도록 합니다.

- 텍스트가 긴 경우에는 텍스트의 최대 길이를 26자로 제한하여 명령 단추가 넓어지지 않도록 합니다.

-  단추의 텍스트 콘텐츠가 동적인 경우에는(예: [지역화](../globalizing/globalizing-portal.md)) 단추 크기를 조정하는 방법과 주위의 컨트롤에 나타날 변화를 고려하세요.

<table>
<tr>
<td> <b>해결할 문제:</b><br> 오버플로 텍스트가 포함된 단추 </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>옵션 1:</b><br> 단추 너비를 늘리고, 단추를 중첩하고, 텍스트 길이가 26자 이상이면 줄바꿈 합니다. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>옵션 2:</b><br> 단추 높이를 늘리고 텍스트를 줄바꿈 합니다. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

단추의 모양을 만드는 시각 효과를 사용자 지정할 수도 있습니다. 예를 들어 텍스트를 아이콘으로 바꾸거나 아이콘과 텍스트를 함께 사용할 수 있습니다.

여기서는 이미지와 텍스트가 포함된 **StackPanel**이 단추의 콘텐츠로 설정되어 있습니다.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

단추는 다음과 같습니다.

![이미지 및 텍스트 콘텐츠가 포함된 단추](images/button-orange.png)

## <a name="create-a-repeat-button"></a>반복 단추 만들기

[RepeatButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) 컨트롤은 사용자가 눌렀다가 놓을 때까지 반복해서 [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 이벤트를 발생시키는 단추입니다. [Delay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay) 속성을 설정하여 **RepeatButton** 컨트롤이 클릭 동작 반복을 시작하기 전에 눌러진 후 대기해야 하는 시간을 지정합니다. [Interval](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval) 속성을 설정하여 클릭 동작의 반복 간 시간을 지정합니다. 두 속성에 대한 시간은 밀리초로 지정됩니다.

다음 예제에서는 두 **RepeatButton** 컨트롤(해당 **Click** 이벤트는 텍스트 블록에 표시되는 값을 늘리고 줄이는 데 사용함)을 보여 줍니다.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>드롭다운 단추 만들기

> **DropDownButton**에는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/) 또는 Windows 10, 버전 1809(SDK 17763) 이상이 필요합니다. 최신 SDK를 다운로드하려면 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)를 참조하고 이전 SDK를 다운로드하려면 [Windows SDK 및 에뮬레이터 아카이브](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)를 참조하세요.

[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton)은 더 많은 옵션을 포함하는 연결된 플라이아웃이 있는 시각적 표시기 형태의 갈매기형 펼침 단추를 표시합니다. 플라이아웃을 사용하는 표준 **Button** 컨트롤과 동작은 동일하고, 모양만 다릅니다.

드롭다운 단추는 **Click** 이벤트를 상속하지만 일반적으로 사용되지 않습니다. 대신, **Flyout** 속성을 사용하여 플라이아웃을 연결하고 플라이아웃의 메뉴 옵션을 사용하여 작업을 호출합니다. 단추를 클릭하면 플라이아웃이 자동으로 열립니다.
단추를 원하는 대로 배치하려면 플라이아웃의 [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) 속성을 지정해야 합니다. 기본 배치 알고리즘으로는 상황에 따라 의도한 대로 배치하지 못할 수 있습니다.

> [!TIP]
> 플라이아웃에 대한 자세한 내용은 [메뉴 및 상황에 맞는 메뉴](menus.md)를 참조하세요.

### <a name="example---drop-down-button"></a>예제 - 드롭다운 단추

이 예제에서는 **RichEditBox** 컨트롤의 단락 맞춤 명령이 포함된 플라이아웃을 사용하여 드롭다운 단추를 만드는 방법을 보여줍니다. 자세한 정보 및 코드는 [서식 있는 편집 상자](rich-edit-box.md)를 참조하세요.

![맞춤 명령을 사용하는 드롭다운 단추](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>분할 단추 만들기

 > [!IMPORTANT]
 > **SplitButton**에는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/) 또는 Windows 10, 버전 1809(SDK 17763) 이상이 필요합니다. 최신 SDK를 다운로드하려면 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)를 참조하고 이전 SDK를 다운로드하려면 [Windows SDK 및 에뮬레이터 아카이브](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)를 참조하세요.

[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) 컨트롤은 개별적으로 호출할 수 있는 두 부분으로 구성됩니다. 한 부분은 표준 단추처럼 동작하며 즉각적인 작업을 호출합니다. 다른 부분은 사용자가 선택할 수 있는 추가 옵션을 포함하는 플라이아웃을 호출합니다.

> [!NOTE]
> 터치로 호출하면 분할 단추가 드롭다운 단추처럼 동작하고, 단추의 양쪽 절반이 플라이아웃을 호출합니다. 또 다른 입력 방법으로, 사용자는 단추의 절반을 개별적으로 호출할 수 있습니다.

분할 단추의 일반적인 동작은 다음과 같습니다.

- 사용자가 단추 부분을 클릭하면 **Click** 이벤트를 처리하여 현재 드롭다운 목록에서 선택된 옵션을 호출합니다.

- 드롭다운이 열려 있으면 드롭다운의 항목 호출을 처리하여 현재 선택된 옵션을 변경하고 호출합니다. 터치를 사용할 때에는 단추 **Click** 이벤트가 발생하지 않으므로 플라이아웃 항목을 호출해야 합니다.

> [!TIP]
> 여러 가지 방법으로 드롭다운에 항목을 배치하고 호출을 처리할 수 있습니다. **ListView** 또는 **GridView**를 사용하는 경우 한 가지 방법은 **SelectionChanged** 이벤트를 처리하는 것입니다. 이렇게 하는 경우 [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus)를 **false**로 설정합니다. 그러면 사용자는 변경할 때마다 항목을 호출하지 않고 키보드를 사용하여 옵션을 탐색할 수 있습니다.

### <a name="example---split-button"></a>예제 - 분할 단추

이 예제에서는 **RichEditBox** 컨트롤에서 선택한 텍스트의 전경색을 변경하는 데 사용되는 분할 단추를 만드는 방법을 보여줍니다. 자세한 정보 및 코드는 [서식 있는 편집 상자](rich-edit-box.md)를 참조하세요.
분할 단추의 플라이아웃은 [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) 속성의 기본값으로 [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode)를 사용합니다. 이 값을 재정의할 수 없습니다.

![전경색을 선택할 수 있는 분할 단추](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}"
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>설정/해제 분할 단추 만들기

> [!NOTE]
> **ToggleSplitButton**에는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/) 또는 Windows 10, 버전 1809(SDK 17763) 이상이 필요합니다. 최신 SDK를 다운로드하려면 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)를 참조하고 이전 SDK를 다운로드하려면 [Windows SDK 및 에뮬레이터 아카이브](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)를 참조하세요.

[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) 컨트롤은 개별적으로 호출할 수 있는 두 부분으로 구성됩니다. 한 부분은 끄거나 켤 수 있는 토글 단추처럼 동작합니다. 다른 부분은 사용자가 선택할 수 있는 추가 옵션을 포함하는 플라이아웃을 호출합니다.

설정/해제 분할 단추는 일반적으로 기능에 사용자가 선택할 수 있는 여러 옵션이 있을 때 기능을 활성화 또는 비활성화하는 데 사용됩니다. 예를 들어 문서 편집기에서 목록을 켜고 끄는 데 사용할 수 있습니다. 반면 드롭다운은 목록의 스타일을 선택하는 데 사용됩니다.

> [!NOTE]
> 터치로 호출하면 설정/해제 분할 단추가 드롭다운 단추로 작동합니다. 또 다른 입력 방법으로, 사용자는 단추의 절반을 개별적으로 전환하고 호출할 수 있습니다. 터치를 사용하면 단추의 두 절반이 플라이아웃을 호출합니다. 따라서 플라이아웃 콘텐츠에 단추를 켜고 끄는 옵션을 포함해야 합니다.


### <a name="differences-with-togglebutton"></a>ToggleButton과의 차이점

[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton)과는 다르게, **ToggleSplitButton**은 확정되지 않은 상태가 없습니다. 따라서 다음과 같은 차이점을 기억해야 합니다.

- **ToggleSplitButton**에는 **IsThreeState** 속성 또는 **Indeterminate** 이벤트가 없습니다.
- [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) 속성은 **Nullable<bool>** 이 아닌 부울입니다.
- **ToggleSplitButton**에는 [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) 이벤트만 있고, 별도의 **Checked** 및 **Unchecked** 이벤트는 없습니다.


### <a name="example---toggle-split-button"></a>예제 - 설정/해제 분할 단추

다음 예제는 **RichEditBox** 컨트롤에서 설정/해제 분할 단추를 사용하여 목록 서식 지정을 켜고 끄는 방법과 목록 스타일을 변경하는 방법을 보여줍니다. 자세한 정보 및 코드는 [서식 있는 편집 상자](rich-edit-box.md)를 참조하세요.
설정/해제 분할 단추의 플라이아웃은 [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) 속성의 기본값으로 [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode)를 사용합니다. 이 값을 재정의할 수 없습니다.

![목록 스타일을 선택하는 설정/해제 분할 단추](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout>
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged"
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>권장 사항

- 단추의 목적과 상태가 사용자에게 명확하게 전달되는지 확인합니다.

- 확인 대화 상자처럼 동일한 결정에 대한 단추가 여러 개 있는 경우에는 다음과 같은 순서로 커밋 단추를 표시합니다. 여기서 [그렇게 함] 및 [그렇게 하지 않음]은 기본 지침에 대한 구체적인 응답입니다.
  - 확인/[그렇게 함]/예
    - [그렇게 하지 않음]/아니요
    - Cancel

- 사용자에게 단추를 한 번에 한 개 또는 두 개만 표시합니다(예: **수락** 및 **취소**). 사용자에게 더 많은 작업을 표시해야 하는 경우 사용자가 하나의 명령 단추로 작업을 선택하여 트리거할 수 있도록 [확인란](checkbox.md) 또는 [라디오 단추](radio-button.md)를 사용하는 것이 좋습니다.

- 앱 내의 여러 페이지에서 작업을 사용할 수 있어야 하는 경우 단추를 여러 페이지에 복제하는 대신 [하단 앱 바](app-bars.md)를 사용하는 것이 좋습니다.


### <a name="recommended-single-button-layout"></a>권장하는 단일 단추 레이아웃

레이아웃에 단추가 하나만 필요한 경우 컨테이너 컨텍스트에 따라 왼쪽 또는 오른쪽에 맞춰야 합니다.

  - 단추가 하나만 있는 대화 상자의 경우 단추를 **오른쪽에 맞춰야** 합니다. 대화 상자에 단추가 하나만 있는 경우 단추가 안전한 비파괴 작업을 수행하는지 확인합니다. [ContentDialog](dialogs.md)를 사용하고 단일 단추를 지정하면 자동으로 오른쪽에 맞춰집니다.

    ![대화 상자 내의 단추](images/pushbutton_doc_dialog.png)

  - 단추가 알림 메시지, 플라이아웃 또는 목록 보기 항목과 같은 컨테이너 UI 내에 나타나는 경우 컨테이너 내에서 단추를 **오른쪽에 맞춰야**합니다.

    ![컨테이너 내의 단추](images/pushbutton_doc_container.png)

  - 설정 페이지의 아래쪽에 있는 **적용** 단추와 같이 단일 단추가 있는 페이지에서는 단추를 **왼쪽에 맞춰야** 합니다. 그러면 단추가 나머지 페이지 콘텐츠와 맞춰집니다.

    ![페이지의 단추](images/pushbutton_doc_page.png)


## <a name="back-buttons"></a>뒤로 단추

뒤로 단추는 뒤로 스택 또는 사용자의 탐색 기록을 통해 뒤로 탐색할 수 있게 하는 시스템 제공 UI 요소입니다. 뒤로 단추를 직접 만들지 않아도 되지만 좋은 뒤로 탐색 환경을 사용하려면 일부 작업을 수행해야 할 수 있습니다. 자세한 내용은 [UWP 앱에 대한 탐색 기록 및 뒤로 탐색](../basics/navigation-history-and-backwards-navigation.md)을 참조하세요.


## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): 이 샘플에서는 모든 XAML 컨트롤을 대화형 형식으로 보여줍니다.


## <a name="related-articles"></a>관련 문서

- [Button 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)
- [라디오 단추](radio-button.md)
- [확인란](checkbox.md)
- [토글 스위치](toggles.md)
