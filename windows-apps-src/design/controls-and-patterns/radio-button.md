---
description: 라디오 단추를 사용하여 사용자가 상호 배타적이지만 관련이 있는 두 가지 이상의 옵션 중에 하나를 선택하도록 하는 방법을 알아보세요.
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
ms.openlocfilehash: 4953b5adc1953bac83b90271b4042e3b9f13c3f2
ms.sourcegitcommit: 083ddf840ab42bb48b4892fc2876ecbf698e481b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89615526"
---
# <a name="radio-buttons"></a>라디오 단추

옵션 단추라고도 하는 라디오 단추는 사용자가 상호 배타적이지만 관련이 있는 두 가지 이상의 옵션 중에 하나를 선택할 수 있는 단추입니다. 라디오 단추는 항상 그룹에서 사용되며 각 옵션은 그룹에서 하나의 라디오 단추로 표시됩니다.

기본 상태에서는 RadioButtons 그룹의 라디오 단추가 선택되지 않습니다. 즉, 모든 라디오 단추가 선택 취소됩니다. 그러나 사용자가 라디오 단추를 선택한 후에는 그룹을 초기의 지워진 상태로 복원하기 위해 이 단추를 선택 취소할 수 없습니다.

RadioButtons 그룹의 단일 동작은 여러 항목을 선택 및 선택 취소하거나 지울 수 있는 [확인란](checkbox.md)과 구별됩니다.

:::image type="content" source="images/controls/radio-button.png" alt-text="라디오 단추가 하나 선택된 RadioButtons 그룹의 예":::

**Windows UI 라이브러리 가져오기**

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | RadioButtons 컨트롤은 Windows 앱용 최신 컨트롤과 UI 기능을 포함하고 있는 NuGet 패키지인 Windows UI 라이브러리의 일부로 제공됩니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요. |

> **Windows UI 라이브러리 API**: [RadioButtons 클래스](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectedItem 속성](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex 속성](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex), [SelectionChanged 이벤트](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **플랫폼 API**: [RadioButton 클래스](/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [IsChecked 속성](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked), [Checked 이벤트](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **RadioButtons 및 RadioButton**
>
>라디오 단추 그룹을 만드는 방법에는 두 가지가 있습니다.
>
>- WinUI 2.3부터 **[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)** 컨트롤을 사용하는 것이 좋습니다. 이 컨트롤은 레이아웃을 간소화하고, 키보드 탐색 및 접근성을 처리하며, 데이터 원본에 대한 바인딩을 지원합니다.
>- 개별 **[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)** 컨트롤의 그룹을 사용할 수 있습니다. 앱에서 WinUI 2.3 이상을 사용하지 않는 경우 이 방법이 유일한 옵션입니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

사용자가 둘 이상의 상호 배타적 옵션 중에서 선택할 수 있도록 하려면 라디오 단추를 사용합니다.

:::image type="content" source="images/radiobutton_basic.png" alt-text="라디오 단추가 하나 선택된 RadioButtons 그룹":::

사용자가 모든 옵션을 확인한 후 옵션을 선택해야 하는 경우에는 라디오 단추를 사용합니다. 라디오 단추는 모든 옵션을 똑같이 강조하므로 일부 옵션이 필요 이상으로 또는 원하는 이상으로 사용자의 주의를 끌 수도 있습니다.

모든 옵션이 똑같이 사용자의 주의를 끌어야 하는 경우 외에는 다른 컨트롤을 사용하는 것이 좋습니다. 예를 들어 대부분의 사용자에게 가장 적합한 단일 옵션을 추천하려면 [콤보 상자](combo-box.md)를 사용하여 가장 적합한 옵션을 기본 옵션으로 표시합니다.

:::image type="content" source="images/combo_box_collapsed.png" alt-text="기본 옵션을 표시하는 콤보 상자":::

설정/해제 또는 예/아니요와 같이 단일 이진 선택 항목으로 명확하게 표현할 수 있는 두 가지 옵션만 있는 경우 이러한 선택 항목을 단일 [확인란](checkbox.md) 또는 [토글 스위치](toggles.md) 컨트롤로 결합합니다. 예를 들어 "동의함" 라디오 단추와 "동의 안 함" 라디오 단추를 각각 사용하는 대신 "동의함" 확인란 하나만 사용합니다.

단일 이진 선택 항목에는 두 개의 라디오 단추를 사용하지 않습니다.

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="이진 선택 항목을 표시하는 두 개의 라디오 단추":::

대신 확인란을 사용합니다.

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="확인란은 이진 선택 항목을 제공하는 좋은 대안입니다.":::

사용자가 여러 옵션을 선택할 수 있는 경우에는 [확인란](checkbox.md)을 사용합니다.

:::image type="content" source="images/checkbox2.png" alt-text="확인란 지원 다중 선택":::

사용자 옵션이 값 범위 내에 있는 경우(예: *10, 20, 30, ... 100*) [슬라이더](slider.md) 컨트롤을 사용합니다.

:::image type="content" source="images/controls/slider.png" alt-text="값 범위 내의 값 하나를 표시하는 슬라이더 컨트롤":::

8개가 넘는 옵션이 있는 경우 [콤보 상자](combo-box.md)를 사용합니다.

:::image type="content" source="images/combo_box_scroll.png" alt-text="여러 옵션을 표시하는 목록 상자":::

사용 가능한 옵션이 앱의 현재 컨텍스트에 따라 달라지거나 동적으로 변경될 수 있는 경우에는 목록 컨트롤을 사용합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 <a href="xamlcontrolsgallery:/item/RadioButton">앱을 열고 RadioButtons 컨트롤이 실제로 작동하는 모습을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="winui-radiobuttons-overview"></a>WinUI RadioButtons 개요

키보드 액세스 및 탐색 동작은 [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 컨트롤에서 최적화되었습니다. 이러한 향상된 기능을 통해 내게 필요한 옵션 및 키보드 전원 사용자가 옵션 목록을 보다 빠르고 쉽게 탐색할 수 있습니다.

이러한 기능 향상 외에도 방향, 간격 및 여백 설정 자동화를 통해 RadioButtons 그룹의 개별 라디오 단추 기본 시각적 레이아웃이 최적화되었습니다. 따라서 [StackPanel](../layout/layout-panels.md#stackpanel) 또는 [Grid](../layout/layout-panels.md#grid) 같은 보다 기본적인 그룹화 컨트롤을 사용할 때 지정할 수 있으므로 이러한 속성을 지정하지 않아도 됩니다.

### <a name="navigating-a-radiobuttons-group"></a>RadioButtons 그룹 탐색

`RadioButtons` 컨트롤에는 키보드 사용자가 목록을 더 빠르고 쉽게 탐색할 수 있도록 지원하는 특별한 탐색 동작이 있습니다.

#### <a name="keyboard-focus"></a>키보드 포커스

`RadioButtons` 컨트롤에서 지원하는 두 가지 상태는 다음과 같습니다.

- 라디오 단추가 선택되지 않음
- 라디오 단추 하나가 선택됨

다음 섹션에서는 각 상태에 대한 컨트롤의 포커스 동작을 설명합니다.

##### <a name="no-radio-button-is-selected"></a>라디오 단추가 선택되지 않음

라디오 단추를 선택하지 않으면 목록의 첫 번째 라디오 단추에 포커스가 있습니다.

> [!NOTE]
> 초기 탭 탐색에서 탭 포커스를 받는 항목은 선택되지 않습니다.

|탭 포커스가 없는 목록 | 초기 탭 포커스가 있는 목록|
|:--:|:--:|
| ![탭 포커스가 없는 목록 및 선택한 항목이 없음](images/radiobutton-no-selected-item-no-tab-focus.png) | ![초기 탭 포커스가 있는 목록 및 선택한 항목이 없음](images/radiobutton-no-selected-item-tab-focus.png)|

##### <a name="one-radio-button-is-selected"></a>라디오 단추 하나가 선택됨

사용자가 라디오 단추가 이미 선택된 목록을 탭하면 선택된 라디오 단추에서 포커스를 갖습니다.

|탭 포커스가 없는 목록 | 초기 탭 포커스가 있는 목록 |
|:--:|:--:|
| ![탭 포커스가 없는 목록 및 선택한 항목](images/radiobutton-selected-item-no-tab-focus.png) | ![초기 탭 포커스가 있는 목록 및 선택한 항목](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="keyboard-navigation"></a>키보드 탐색

일반적인 키보드 탐색 동작에 대한 자세한 내용은 [키보드 조작 - 탐색](../input/keyboard-interactions.md#navigation)을 참조하세요.

포커스가 이미 `RadioButtons` 그룹의 항목에 있는 경우 사용자는 화살표 키를 그룹 내 항목 간의 "내부 탐색"에 사용할 수 있습니다. 위쪽 및 아래쪽 화살표 키는 XAML 태그에서 정의한 대로 "다음" 또는 "이전" 논리 항목으로 이동합니다. 왼쪽 및 오른쪽 화살표 키는 공간적으로 이동합니다.

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>단일 열 또는 단일 행 레이아웃 내 탐색

단일 열 또는 단일 행 레이아웃에서 키보드 탐색을 수행하면 다음과 같은 동작이 발생합니다.

|단일 열 | 단일 행|
|:--|:--|
| ![단일 열 RadioButtons 그룹에 대한 키보드 탐색의 예](images/radiobutton-keyboard-navigation-single-column.png)</br>위쪽 및 아래쪽 화살표 키는 항목 간에 이동합니다.</br>왼쪽 및 오른쪽 화살표 키는 아무 작업도 수행하지 않습니다. | ![단일 행 RadioButtons 그룹에 대한 키보드 탐색의 예](images/radiobutton-keyboard-navigation-single-row.png)<br/>왼쪽 및 위쪽 화살표 키는 이전 항목으로 이동하고, 오른쪽 및 아래쪽 화살표 키는 다음 항목으로 이동합니다. |

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>다중 열, 다중 행 레이아웃 내 탐색

다중 열, 다중 행 그리드 레이아웃에서 키보드 탐색을 수행하면 다음과 같은 동작이 발생합니다.

|왼쪽/오른쪽 화살표 키| 위쪽/아래쪽 화살표 키 |
|:--|:--|
| ![다중 열/행 RadioButtons 그룹에 대한 가로 키보드 탐색의 예](images/radiobutton-keyboard-navigation-multi-column-row-1.png)</br>왼쪽 및 오른쪽 화살표 키는 포커스를 한 행의 항목 간에 가로로 이동합니다. | ![다중 열/행 RadioButtons 그룹에 대한 세로 키보드 탐색의 예](images/radiobutton-keyboard-navigation-multi-column-row-2.png)<br/>위쪽 및 아래쪽 화살표 키는 포커스를 열의 항목 간에 세로로 이동합니다. |
| ![포커스가 열의 마지막 항목에 있는 가로 키보드 탐색의 예](images/radiobutton-keyboard-navigation-multi-column-row-3.png)</br> 포커스가 열의 마지막 항목에 있고 오른쪽 또는 왼쪽 화살표 키를 누르면 포커스가 다음 또는 이전 열의 마지막 항목으로 이동합니다(있는 경우). | ![포커스가 열의 마지막 항목에 있는 세로 키보드 탐색의 예](images/radiobutton-keyboard-navigation-multi-column-row-4.png)<br/>포커스가 열의 마지막 항목에 있고 아래쪽 화살표 키를 누르면 포커스가 다음 열의 첫 번째 항목으로 이동합니다(있는 경우). 포커스가 열의 첫 번째 항목에 있고 위쪽 화살표 키를 누르면 포커스가 이전 열의 마지막 항목으로 이동합니다(있는 경우). |

자세한 내용은 [키보드 조작](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items)을 참조하세요.

###### <a name="wrapping"></a>줄 바꿈

RadioButtons 그룹은 포커스를 첫 번째 행 또는 열에서 마지막 항목으로 또는 마지막 행 또는 열에서 첫 번째 항목으로 줄 바꿈하지 않습니다. 사용자가 화면 판독기를 사용할 때 경계와 시작 및 끝을 명확하게 표시할 수 없어 시각 장애가 있는 사용자가 목록을 탐색하기 어렵게 만들기 때문입니다.

또한 `RadioButtons` 컨트롤은 적절한 수의 항목을 포함하기 위한 컨트롤이므로 열거형을 지원하지 않습니다([올바른 컨트롤인가요?](#is-this-the-right-control) 참조).

### <a name="selection-follows-focus"></a>선택 항목이 포커스를 따라 이동

키보드를 사용하여 `RadioButtons` 그룹의 항목 간에 탐색할 때 포커스가 한 항목에서 다음 항목으로 이동하면 포커스가 새로 있는 항목이 선택되고 이전에 포커스가 있던 항목은 지워집니다.

|키보드 탐색 전 | 키보드 탐색 후|
|:--|:--|
| ![키보드 탐색 전의 포커스 및 선택 예제](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*키보드 탐색 전의 포커스 및 선택 예제* | ![키보드 탐색 후의 포커스 및 선택 예제](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*키보드 탐색 후의 포커스 및 선택 항목에 대한 예 - 아래쪽 화살표 키를 통해 포커스가 라디오 단추 "3"으로 이동하여 이를 선택하고, 라디오 단추 2를 지움* |

Ctrl+화살표 키를 사용하여 선택 항목을 변경하지 않고 포커스를 이동할 수 있습니다. 포커스가 이동되면 스페이스바를 사용하여 현재 포커스가 있는 항목을 선택할 수 있습니다.

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Xbox 게임 패드와 리모컨으로 탐색

Xbox 게임 패드 또는 리모컨을 사용하여 라디오 단추 간에 이동하는 경우, "선택 항목이 포커스를 따라 이동" 동작이 사용하지 않도록 설정되며 사용자는 "A" 단추를 눌러 현재 포커스가 있는 라디오 단추를 선택해야 합니다.

## <a name="accessibility-behavior"></a>접근성 동작

다음 표에서는 내레이터에서 `RadioButtons` 그룹을 처리하는 방법과 알림 내용을 설명합니다. 이 동작은 사용자가 내레이터 세부 사항을 설정하는 방법에 따라 달라집니다.

| 작업 | 내레이터 알림 |
|:--|:--|
| 포커스가 선택한 항목으로 이동 | "_name_, RadioButton, 선택됨, _N_ 중 _x_" |
|포커스가 선택하지 않은 항목으로 이동<br> *(Ctrl+화살표 키 또는 Xbox 게임 패드를 사용하여 탐색하는 경우<br> 선택 항목이 포커스를 따르지 않음을 나타냅니다.)* | "_name_, RadioButton, 선택되지 않음, _N_ 중 _x_"  |

> [!NOTE]
> 내레이터에서 각 항목에 대해 알리는 _**name**_ 값은 항목에 사용할 수 있는 경우 [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty) 연결된 속성의 값입니다. 그렇지 않으면 항목의 [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0) 메서드에서 반환된 값입니다.
>
> _**x**_ 는 현재 항목의 번호입니다. _**N**_ 은 그룹의 총 항목 수입니다.

## <a name="create-a-winui-radiobuttons-group"></a>WinUI RadioButtons 그룹 만들기

`RadioButtons` 컨트롤은 [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol)과 비슷한 콘텐츠 모델을 사용합니다. 즉 다음을 수행할 수 있습니다.

- 항목을 [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items) 컬렉션에 직접 추가하거나 데이터를 해당 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 속성에 바인딩하여 채웁니다.
- [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) 또는 [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) 속성을 사용하여 선택한 옵션을 가져오고 설정합니다.
- 옵션이 선택될 때 작업을 수행하도록 [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) 이벤트를 처리합니다.

> [!TIP]
> 이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이를 [Page](/uwp/api/windows.ui.xaml.controls.page) 요소(`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`)에 추가했습니다.
>
>코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 이 **using** 문(`using muxc = Microsoft.UI.Xaml.Controls;`)을 파일 맨 위에 추가했습니다.

여기서는 세 가지 옵션이 있는 간단한 `RadioButtons` 컨트롤을 선언합니다. [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header) 속성은 레이블을 그룹에 제공하도록 설정되고, `SelectedIndex` 속성은 기본 옵션을 제공하도록 설정됩니다.

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

결과는 다음과 같습니다.

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="세 개의 라디오 단추로 구성된 그룹":::

사용자가 옵션을 선택할 때 작업을 수행하도록 [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) 이벤트를 처리합니다. 여기서는 "ExampleBorder"(`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`)라는 [Border](/uwp/api/windows.ui.xaml.controls.border) 요소의 배경색을 변경합니다.

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 속성에서 선택한 항목을 가져올 수도 있습니다. 하나의 선택한 항목만 인덱스 0에 있으므로 선택한 항목을 `string colorName = e.AddedItems[0] as string;`과 같이 가져올 수 있습니다.

### <a name="selection-states"></a>선택 상태

라디오 단추의 상태는 선택됨 및 선택 취소됨 두 가지입니다. `RadioButtons` 그룹에서 옵션이 선택되면 [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) 속성에서 해당 값을 가져오고 [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) 속성에서 컬렉션의 해당 위치를 가져올 수 있습니다. 사용자가 동일한 그룹의 다른 라디오 단추를 선택하면 라디오 단추의 선택이 취소되지만, 같은 라디오 단추를 다시 클릭하면 선택 취소되지 않습니다. 그러나 `SelectedItem = null` 또는 `SelectedIndex = -1`로 설정하여 라디오 단추 그룹을 프로그래밍 방식으로 지울 수 있습니다. (`SelectedIndex`를 `Items` 컬렉션의 범위를 벗어난 값으로 설정하면 선택되지 않습니다.)

### <a name="radiobuttons-content"></a>RadioButtons 콘텐츠

이전 예제에서는 `RadioButtons` 컨트롤을 단순 문자열로 채웠습니다. 컨트롤에서 라디오 단추를 제공하고 문자열을 각 단추의 레이블로 사용했습니다.

그러나 `RadioButtons` 컨트롤은 개체로 채울 수 있습니다. 일반적으로 개체에서 텍스트 레이블로 사용할 수 있는 문자열 표현을 제공하려고 합니다. 경우에 따라 텍스트 대신 이미지가 적절할 수도 있습니다.

여기서는 [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon) 요소를 사용하여 컨트롤을 채웁니다.

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="기호 아이콘이 있는 그룹 라디오 단추":::

개별 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 컨트롤을 사용하여 `RadioButtons` 항목을 채울 수도 있습니다. 이는 나중에 설명할 특별한 사례입니다. [RadioButtons 그룹의 RadioButton 컨트롤]()을 참조하세요.

개체를 사용할 수 있다는 이점은 `RadioButtons` 컨트롤을 데이터 모델의 사용자 지정 유형에 바인딩할 수 있다는 것입니다. 다음 섹션에서는 이에 대해 설명합니다.

### <a name="data-binding"></a>데이터 바인딩

`RadioButtons` 컨트롤은 해당 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 속성에 대한 데이터 바인딩을 지원합니다. 다음 예제에서는 컨트롤을 사용자 지정 데이터 원본에 바인딩하는 방법을 보여 줍니다. 이 예제의 모양 및 기능은 이전 배경색 예제와 동일하지만, 여기서는 색 브러시가 `SelectionChanged` 이벤트 처리기에서 만들어지는 대신 데이터 모델에 저장됩니다.

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>RadioButtons 그룹의 RadioButton 컨트롤

개별 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 컨트롤을 사용하여 `RadioButtons` 항목을 채울 수 있습니다. 이렇게 하면 `AutomationProperties.Name`과 같은 특정 속성에 액세스할 수 있습니다. 또는 기존 `RadioButton` 코드가 있지만 `RadioButtons`의 레이아웃 및 탐색을 활용하려고 할 수 있습니다.

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

`RadioButtons` 그룹에서 `RadioButton` 컨트롤을 사용하면 `RadioButtons` 컨트롤에서 `RadioButton`을 표시하는 방법을 인식하고 있으므로 두 개의 선택 항목 원이 표시되지 않습니다.

그러나 알아야 할 몇 가지 동작이 있습니다. 충돌을 방지하기 위해 개별 컨트롤 또는 `RadioButtons`에서 상태와 이벤트를 처리하는 것이 좋습니다.

다음 표에서는 두 컨트롤의 관련 이벤트 및 속성을 보여 줍니다.

|RadioButton  |RadioButtons  |
|---------|---------|
|[Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked), [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

`Checked` 또는 `Unchecked`와 같은 개별 `RadioButton`에 대한 이벤트를 처리하고 `RadioButtons.SelectionChanged` 이벤트도 처리하는 경우 두 이벤트가 모두 발생합니다. `RadioButton` 이벤트가 먼저 발생한 다음, `RadioButtons.SelectionChanged` 이벤트가 발생합니다 이로 인해 충돌이 발생할 수 있습니다.

`IsChecked`, `SelectedItem` 및 `SelectedIndex` 속성은 동기화된 상태로 유지됩니다. 한 속성을 변경하면 다른 두 속성이 업데이트됩니다.

[RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname) 속성은 무시됩니다. `RadioButtons` 컨트롤에서 그룹을 만듭니다.

### <a name="defining-multiple-columns"></a>여러 열 정의

기본적으로 `RadioButtons` 컨트롤은 단일 열에서 라디오 단추를 세로로 정렬합니다. 컨트롤에서 라디오 단추를 여러 열로 정렬하도록 하려면 [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) 속성을 설정할 수 있습니다. (이렇게 하면 항목이 위쪽에서 아래쪽으로 채워진 다음, 왼쪽에서 오른쪽으로 채워지는 열 우선 순서로 배치됩니다.)

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="두 개의 3열 그룹으로 나뉜 라디오 단추":::

> [!TIP]
> 항목이 단일 가로 행으로 정렬되도록 하려면 `MaxColumns`를 그룹의 항목 수와 동일하게 설정합니다.

## <a name="create-your-own-radiobutton-group"></a>사용자 고유의 RadioButton 그룹 만들기

> [!Important]
> 이전 버전의 WinUI를 사용하지 않는 경우 WinUI `RadioButtons` 컨트롤을 사용하여 `RadioButton` 요소를 그룹화하는 것이 좋습니다.

라디오 단추는 그룹으로 작동합니다. 개별 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 컨트롤은 두 가지 방법 중 하나를 사용하여 그룹화할 수 있습니다.

- 동일한 부모 컨테이너 내에 배치합니다.
- 각 라디오 단추의 [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 속성을 동일한 값으로 설정합니다.

이 예제에서 첫 번째 라디오 단추 그룹은 동일한 스택 패널에 배치되어 암시적으로 그룹화됩니다. 두 번째 그룹은 두 개의 스택 패널로 나뉘어 있으므로 `GroupName`을 사용하여 명시적으로 단일 그룹으로 그룹화합니다.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

이러한 두 그룹의 `RadioButton` 컨트롤은 다음과 같습니다.

:::image type="content" source="images/radio-button-groups.png" alt-text="두 그룹으로 나뉜 라디오 단추":::

### <a name="radio-button-states"></a>라디오 단추 상태

라디오 단추의 상태는 선택됨 및 선택 취소됨 두 가지입니다. 라디오 단추가 선택되면 해당 [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 속성은 `true`입니다. 라디오 단추가 선택 취소되면 해당 `IsChecked` 속성은 `false`입니다. 사용자가 동일한 그룹의 다른 라디오 단추를 선택하면 라디오 단추의 선택이 취소되지만, 같은 라디오 단추를 다시 클릭하면 선택 취소되지 않습니다. 그러나 해당 `IsChecked` 속성을 `false`로 설정하여 라디오 단추를 프로그래밍 방식으로 지울 수 있습니다.

### <a name="visuals-to-consider"></a>고려할 시각적 개체

개별 `RadioButton` 컨트롤의 기본 간격은 `RadioButtons` 그룹에서 제공하는 간격과 다릅니다. `RadioButtons` 간격을 개별 `RadioButton` 컨트롤에 적용하려면 여기에 나와 있는 대로 `Margin` 값으로 `0,0,7,3`을 사용합니다.

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

다음 이미지에서는 그룹에서 기본 설정된 라디오 단추 간격을 보여 줍니다.

:::image type="content" source="images/radiobutton-layout.png" alt-text="세로 방향으로 정렬된 라디오 단추 세트를 보여주는 이미지":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="라디오 단추의 간격 지침을 보여주는 이미지":::

> [!NOTE]
> WinUI RadioButtons 컨트롤을 사용하는 경우 간격, 여백 및 방향이 이미 최적화되어 있습니다.

## <a name="recommendations"></a>권장 사항

- 라디오 단추 세트의 용도와 현재 상태가 명시적인지 확인합니다.
- 라디오 단추의 텍스트 레이블을 한 줄로 제한합니다.
- 텍스트 레이블이 동적인 경우 단추 크기를 자동으로 조정하는 방법과 주변의 시각적 개체에 미치는 영향을 고려하세요.
- 브랜드 지침에 다른 글꼴을 사용하도록 규정되지 않은 경우 기본 글꼴을 사용합니다.
- RadioButtons 그룹 두 개를 나란히 배치하지 마세요. RadioButtons 그룹 두 개가 나란히 배치되면 어느 단추가 어느 그룹에 속하는지 사용자가 확인하는 데 어려움이 있습니다.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- 모든 XAML 컨트롤을 대화형 형식으로 가져오려면 [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery)을 참조하세요.

## <a name="related-topics"></a>관련 항목

- [단추](buttons.md)
- [토글 스위치](toggles.md)
- [확인란](checkbox.md)
- [목록과 콤보 상자](lists.md)
- [슬라이더](slider.md)
- [RadioButtons 클래스](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [RadioButton 클래스](/uwp/api/windows.ui.xaml.controls.radiobutton)
