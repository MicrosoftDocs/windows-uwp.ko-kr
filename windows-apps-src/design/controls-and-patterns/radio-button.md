---
Description: 라디오 단추를 사용하면 두 가지 이상의 옵션 중 하나를 선택할 수 있습니다.
title: 라디오 단추에 대한 지침
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6705c314d9a70f8b6282841a7f8b1df76c6ef880
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978405"
---
# <a name="radio-buttons"></a>라디오 단추

라디오 단추는 사용자가 상호 배타적이지만 관련이 있는 두 가지 이상의 옵션 중에 하나를 선택할 수 있는 단추입니다. 각 옵션은 하나의 라디오 단추로 표시됩니다.

기본 상태에서는 그룹의 라디오 단추가 선택되지 않습니다. 그러나 사용자가 라디오 단추 옵션을 선택하면 그룹의 선택되지 않은 상태를 사용자가 복원할 수 없습니다.

라디오 단추 그룹의 동작은 여러 항목을 선택 및 선택 취소할 수 있는 [확인란](checkbox.md)과 다릅니다.

![라디오 단추](images/controls/radio-button.png)

**Windows UI 라이브러리 가져오기**

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | **RadioButtons** 컨트롤은 Windows 앱용 최신 컨트롤과 UI 기능을 포함하고 있는 NuGet 패키지인 Windows UI 라이브러리의 일부로 제공됩니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

> **Windows UI 라이브러리 API:** [RadioButtons 클래스](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectionChanged 이벤트](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged), [SelectedItem 속성](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex 속성](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)
>
> **플랫폼 API:** [RadioButton 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Checked 이벤트](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [IsChecked 속성](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

라디오 단추를 사용하여 사용자에게 두 개 이상의 상호 배타적 옵션을 제공합니다.

![라디오 단추 그룹](images/radiobutton_basic.png)

사용자가 모든 옵션을 확인한 후 선택해야 할 때 라디오 단추를 사용합니다. 라디오 단추는 모든 옵션을 똑같이 강조하므로 필요 이상으로 또는 원하는 것 이상으로 옵션에 주의를 끌 수도 있습니다. 사용자가 모든 옵션에 똑같이 주의를 기울여야 하는 경우 외에는 다른 컨트롤을 사용하는 것이 좋습니다. 예를 들어 대부분의 상황에서 대부분의 사용자가 기본 옵션을 사용하는 것이 좋은 경우에는 [드롭다운 목록](combo-box.md)을 대신 사용하세요.

![기본 옵션을 강조 표시하는 데 사용되는 드롭다운 목록](images/combo_box_collapsed.png)

상호 배타적인 옵션이 두 개만 있는 경우 두 옵션을 하나의 [확인란](checkbox.md) 또는 [토글 스위치](toggles.md)로 결합합니다. 예를 들어 "동의함" 라디오 단추와 "동의 안 함" 라디오 단추를 각각 사용하는 대신 "동의함" 확인란을 사용합니다.

![확인란은 이진 선택 항목을 제공하는 좋은 대안입니다.](images/radiobutton_vs_checkbox.png)

사용자가 여러 옵션을 선택할 수 있는 경우에는 [확인란](checkbox.md)을 사용합니다.

![다중 선택을 지원하는 확인란](images/checkbox2.png)

옵션이 고정 단위(10, 20, 30)를 가진 숫자인 경우에는 [슬라이더](slider.md) 컨트롤을 사용합니다.

![단계별 값을 선택하는 데 사용되는 슬라이더](images/controls/slider.png)

옵션이 8개를 초과하는 경우에는 [콤보 상자 또는 목록 상자](combo-box.md)를 사용합니다.

![여러 옵션을 제공하는 데 사용되는 목록 상자](images/combo_box_scroll.png)

> [!NOTE]
> 사용 가능한 옵션이 앱의 현재 컨텍스트에 따라 달라지거나 동적으로 변경될 수 있는 경우에는 단일 선택 [목록 상자](combo-box.md#list-boxes)를 사용합니다.

## <a name="radiobuttons-behavior"></a>RadioButtons 동작

접근성 및 키보드 고급 사용자가 옵션 목록을 보다 빠르고 쉽게 탐색할 수 있도록 키보드 액세스 및 탐색 동작이 [RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041) 그룹에서 최적화되었습니다.

바로 가기 키 및 접근성이 개선되었을 뿐 아니라 방향, 간격 및 여백 설정 자동화를 통해 RadioButton 그룹의 개별 라디오 단추 기본 시각적 레이아웃이 최적화되었습니다. 따라서 [StackPanel](../layout/layout-panels.md#stackpanel) 또는 [Grid](../layout/layout-panels.md#grid) 같은 보다 기본적인 그룹화 컨트롤을 사용할 때 지정할 수 있으므로 이러한 속성을 지정하지 않아도 됩니다.

### <a name="navigating-a-radiobuttons-group"></a>RadioButtons 그룹 탐색

RadioButtons 컨트롤은 다음과 같은 두 가지 상태를 지원합니다.

- 아무것도 선택되지 않은 RadioButton 컨트롤 목록
- 항목 하나가 이미 선택된 RadioButton 컨트롤 목록

다음 두 섹션에서는 두 가지 라디오 단추 포커스 동작을 모두 다룹니다.

#### <a name="item-already-selected"></a>항목이 이미 선택됨

사용자가 라디오 단추를 선택하고 목록을 탭하면 선택된 라디오 단추로 포커스가 이동합니다.

|탭 포커스가 없는 목록 | 초기 탭 포커스가 있는 목록 |
|:--:|:--:|
| ![탭 포커스가 없는 목록](images/radiobutton-selected-item-no-tab-focus.png) | ![초기 탭 포커스가 있는 목록](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="no-item-selected"></a>선택한 항목 없음

라디오 단추를 선택하지 않으면 목록의 첫 번째 라디오 단추에 포커스가 있습니다.

> [!NOTE]
> 초기 탭 탐색에서 탭 포커스를 받는 항목은 선택되지 않습니다.

|탭 포커스가 없는 목록 | 초기 탭 포커스가 있는 목록|
|:--:|:--:|
| ![탭 포커스가 없는 목록](images/radiobutton-no-selected-item-no-tab-focus.png) | ![초기 탭 포커스가 있는 목록](images/radiobutton-no-selected-item-tab-focus.png)|

### <a name="keyboard-navigation"></a>키보드 탐색

라디오 단추 옵션의 행/열이 하나 있고 항목이 이미 탭 포커스를 받은 경우 화살표 키를 누르면 RadioButtons 컨트롤 내의 항목 간에 "내부 탐색"이 수행됩니다. 키보드 탐색 동작에 대한 자세한 내용은 [키보드 상호 작용 - 탐색](../input/keyboard-interactions.md#navigation)을 참조하세요.

RadioButtons 컨트롤의 경우 옵션 목록이 세로 방향으로 정렬된 경우(독점적으로) 위쪽/아래쪽 화살표 키는 항목 간에 이동하고 왼쪽/오른쪽 화살표 키는 아무것도 수행하지 않습니다. 그러나 가로 방향으로 정렬된(독점적으로) 목록에서는 왼쪽/오른쪽 화살표 키와 위쪽/아래쪽 화살표 키 모두 같은 방식으로 항목 간에 이동합니다.

![단일 열/행 RadioButton 그룹의 키보드 탐색 예](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*단일 열/행 RadioButton 그룹의 키보드 탐색 예*

#### <a name="navigating-within-multi-columnrow-layouts"></a>여러 열/행 레이아웃 내부 탐색

항목이 위에서 아래로, 왼쪽에서 오른쪽으로 채워지는 열 중심 배열에서는 포커스가 열의 마지막 항목에 있는 상태에서 아래쪽 화살표 키를 누르면 포커스가 그 다음 열의 첫 번째 항목으로 이동합니다. 이와 똑같은 동작이 역순으로 발생합니다. 포커스가 열의 첫 번째 항목에 있는 상태에서 위쪽 화살표 키를 누르면 포커스가 이전 열의 마지막 항목으로 이동합니다.

![다중 열/행 RadioButton 그룹의 키보드 탐색 예](images/radiobutton-keyboard-navigation-multi-column-row.png)

항목이 왼쪽에서 오른쪽으로, 위에서 아래로 채워지는 행 중심 배열에서는 포커스가 행의 마지막 항목에 있는 상태에서 오른쪽 화살표 키를 누르면 포커스가 그 다음 행의 첫 번째 항목으로 이동합니다. 이와 똑같은 동작이 역순으로 발생합니다. 포커스가 행의 첫 번째 항목에 있는 상태에서 왼쪽 화살표 키를 누르면 포커스가 이전 행의 마지막 항목으로 이동합니다.

##### <a name="wrapping"></a>줄 바꿈

RadioButtons 그룹은 래핑하지 않습니다. 화면 판독기를 사용할 때 경계와 시작/끝을 명확하게 표시할 수 없어 시각 장애가 있는 사용자가 목록을 탐색하기 어렵게 만들기 때문입니다. 또한 RadioButtons 컨트롤은 적절한 수의 항목을 포함해야 하므로 열거형을 지원하지 않습니다([올바른 컨트롤인가요?](#is-this-the-right-control) 참조).

## <a name="selection-follows-focus"></a>선택 항목이 포커스를 따라 이동

키보드를 사용하여 RadioButtons 목록의 항목 간에 이동할 때(항목을 이미 선택한 상태에서) 포커스가 한 항목에서 다음 항목으로 이동하면 새로 포커스를 받은 항목이 선택되고 이전에 포커스가 있던 항목은 선택 취소됩니다.

|키보드 탐색 전 | 키보드 탐색 후|
|:--|:--|
| ![키보드 탐색 전의 포커스 및 선택 예제](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*키보드 탐색 전의 포커스 및 선택 예제* | ![키보드 탐색 후의 포커스 및 선택 예제](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*아래쪽 또는 오른쪽 화살표 키가 포커스를 "3" RadioButton으로 이동하고, "3"을 선택하고, "2"를 선택 취소하는 키보드 탐색 후의 포커스 및 선택 예제입니다.*

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Xbox 게임 패드와 리모컨으로 탐색

Xbox 게임 패드 또는 리모컨을 사용하여 RadioButtons 컨트롤 내에서 탐색하는 경우 "선택 항목이 포커스를 따라 이동" 동작이 비활성화되며, 포커스가 있는 라디오 단추를 선택하려면 "A" 단추를 눌러야 합니다.

## <a name="accessibility-behavior"></a>접근성 동작

다음 표에서는 내레이터가 라디오 단추 그룹을 처리하는 방법과 전달되는 내용(사용자가 설정한 내레이터 세부 설정에 따라 다름)을 자세히 설명합니다.

| 초기 포커스 | 포커스가 선택한 항목으로 이동 |
|:--|:--|
| "그룹 이름" RadioButton 컬렉션, N개 중 x개 선택됨 | RadioButton "이름" 선택됨, N개 중 x개 |
|"그룹 이름" RadioButton 컬렉션, 선택된 항목 없음| RadioButton "이름" 선택되지 않음, N개 중 x개 <br> *(Shift 화살표 키를 사용하여 탐색하는 경우 어떤 선택 항목도 포커스를 따라 이동하지 않음)* |

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/RadioButton">앱을 열고 실제로 작동하는 RadioButton을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="using-the-winui-radiobuttons-control"></a>WinUI RadioButtons 컨트롤 사용

[WinUI](https://github.com/microsoft/microsoft-ui-xaml)를 사용하는 경우 [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 컨트롤을 사용하는 것이 좋습니다.

RadioButtons 컨트롤은 쉽게 설정하여 사용할 수 있으며, 적절하고 예상 가능한 키보드 및 내레이터 동작을 제공합니다.

여기서는 세 가지 옵션을 사용하여 기본 RadioButtons 컨트롤을 선언합니다.

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```

![두 그룹으로 나뉜 라디오 단추](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>여러 열 정의

[MaxColumns 속성](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns)을 지정하여 다중 열 RadioButtons 컨트롤을 선언할 수 있습니다.

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![두 개의 열 그룹으로 나뉜 라디오 단추](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>데이터 바인딩

RadioButtons 컨트롤은 다음 코드 조각처럼 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 속성을 사용하여 데이터 바인딩을 지원합니다.

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radio-button-group"></a>자신만의 라디오 단추 그룹 만들기

> [!Important]
> WinUI RadioButtons 컨트롤을 사용하여 RadioButton 요소를 그룹화하는 것이 좋습니다(이전 버전의 WinUI를 사용하지 않는 경우).

라디오 단추는 그룹으로 작동합니다. 다음 두 가지 방법으로 라디오 단추 컨트롤을 그룹화할 수 있습니다.

- 동일한 부모 컨테이너 내에 배치합니다.
- 각 라디오 단추의 [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 속성을 동일한 값으로 설정합니다.

이 예제에서 첫 번째 라디오 단추 그룹은 동일한 스택 패널에 배치되어 암시적으로 그룹화됩니다. 두 번째 그룹은 두 스택 패널로 나뉘어 있으므로 GroupName별로 명시적으로 그룹화됩니다.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

다음 이미지는 이 라디오 단추 그룹이 어떻게 렌더링되는지 보여줍니다.

![두 그룹으로 나뉜 라디오 단추](images/radio-button-groups.png)

## <a name="radio-button-states"></a>라디오 단추 상태

라디오 단추의 상태는 선택되거나 선택되지 않은 두 가지 상태입니다. 라디오 단추가 선택된 경우 해당 [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 속성은 **true**입니다. 라디오 단추가 선택 취소된 경우 **IsChecked** 속성이 **false**입니다. 같은 그룹에 있는 다른 라디오 단추를 클릭하여 라디오 단추를 선택 취소할 수 있지만, 라디오 단추를 다시 클릭하여 선택 취소할 수는 없습니다. 그러나 IsChecked 속성을 **false**로 설정하여 프로그래밍 방식으로 라디오 단추를 선택 취소할 수 있습니다.

## <a name="recommendations"></a>권장 사항

- 라디오 단추 세트의 용도와 현재 상태가 명확한지 확인합니다.
- 라디오 단추의 텍스트 콘텐츠를 한 줄로 제한합니다.
- 텍스트 콘텐츠가 동적인 경우 단추 크기를 조정하는 방법과 주위의 시각적 모양을 고려하세요.
- 브랜드 지침에 다른 글꼴을 사용하도록 규정되지 않은 경우 기본 글꼴을 사용합니다.
- 라디오 단추 그룹 두 개를 나란히 배치하지 마세요. 두 라디오 단추 그룹이 나란히 배치된 경우 어느 단추가 어느 그룹에 속하는지 확인하는 데 어려움이 있습니다.

### <a name="visuals-to-consider"></a>고려할 시각적 개체

다음 이미지는 RadioButton 그룹에 라디오 단추를 가장 적절하게 배치하는 방법을 보여줍니다.

![라디오 단추 집합](images/radiobutton-layout.png)

![라디오 단추에 대한 간격 지침](images/radiobutton-redline.png)

> [!NOTE]
> WinUI RadioButtons 컨트롤을 사용하는 경우 간격, 여백 및 방향은 이미 최적화되어 있습니다.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목

### <a name="for-designers"></a>디자이너용

- [단추](buttons.md)
- [토글 스위치](toggles.md)
- [확인란](checkbox.md)
- [목록과 콤보 상자](lists.md)
- [슬라이더](slider.md)

### <a name="for-developers-xaml"></a>개발자용(XAML)

- [RadioButton 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
