---
title: 모퉁이 반경
description: 둥근 모퉁이 원칙, 디자인 방법 및 사용자 지정 옵션에 대해 알아봅니다.
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp, 모퉁이 반경, 둥근 모양
ms.openlocfilehash: a83473b5ad836633bc195aa2b5afe87fa092e0ee
ms.sourcegitcommit: 3c3730e968fba89b21459390735614cd4c9d9c67
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320426"
---
# <a name="corner-radius"></a>모퉁이 반경

[Windows UI 라이브러리](/uwp/toolkits/winui/)(WinUI) 버전 2.2부터 많은 컨트롤의 기본 스타일이 둥근 모퉁이를 사용하도록 업데이트되었습니다. 이러한 새 스타일은 따뜻함과 신뢰감을 주기 위한 것으로, 사용자가 UI를 시각적으로 처리하기 쉽습니다.

다음은 두 개의 Button 컨트롤입니다. 첫 번째는 모퉁이가 둥글지 않고, 두 번째는 새로운 둥근 모퉁이 스타일을 사용합니다.

![둥근 모퉁이가 있는 단추와 없는 단추](images/rounded-corner/my-button.png)

WinUI 2.2 이상을 위한 NuGet 패키지를 설치하는 경우 WinUI 컨트롤 및 플랫폼 컨트롤 모두에 대해 새로운 기본 스타일이 설치됩니다. 이러한 스타일은 앱에서 WinUI 2.2를 사용할 때 자동으로 사용됩니다. 새 스타일을 사용하기 위해 수행해야 하는 추가 작업은 없습니다. 하지만 이 작업을 수행해야 하는 경우 둥근 모퉁이를 사용자 지정하는 방법이 이 문서의 뒷부분에 나와 있습니다.

> [!IMPORTANT]
> 일부 컨트롤은 플랫폼([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)) 및 WinUI([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2))에서 모두 사용할 수 있습니다(예: **TreeView** 또는 **ColorPicker**). 앱에서 WinUI를 사용하는 경우 컨트롤의 WinUI 버전을 사용해야 합니다. WinUI와 함께 사용하는 경우 모퉁이를 둥글게 조정하는 작업은 플랫폼 버전에서 일관되지 않게 적용될 수 있습니다.

> **중요 API**: [Control.CornerRadius property](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>기본 컨트롤 디자인

둥근 모퉁이 스타일을 사용하는 컨트롤에는 사각형 요소, 플라이아웃 요소 및 막대 요소의 세 가지 영역이 있습니다.

### <a name="corners-of-rectangle-ui-elements"></a>사각형 UI 요소의 모퉁이

- 이러한 UI 요소에는 사용자에게 화면에서 항상 표시되는 단추와 같은 기본 컨트롤이 포함됩니다.
- 이러한 UI 요소에 사용되는 기본 반경 값은 **2px**입니다.

![둥근 모퉁이가 강조 표시된 단추](images/rounded-corner/button.png)

**컨트롤**

- AutoSuggestBox
- 단추
  - ContentDialog 단추
- CalendarDatePicker
- CheckBox
  - TreeView 다중 선택 확인란
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>플라이아웃 및 오버레이 UI 요소의 모퉁이

- 이러한 요소는 MenuFlyout과 같이 일시적으로 화면에 나타나는 일시적 UI 요소이거나 TabView 탭과 같은 다른 UI를 오버레이하는 요소일 수 있습니다.
- 이러한 UI 요소에 사용되는 기본 반경 값은 **4px**입니다.

![플라이아웃 예제](images/rounded-corner/flyout.png)

**컨트롤**

- CommandBarFlyout
- ContentDialog
- 플라이아웃
- MenuFlyout 비교
- TabView 탭
- TeachingTip
- ToolTip
- 플라이아웃 부분(열려 있는 경우)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>막대 요소

- 이러한 UI 요소는 막대나 선처럼 모양이 지정됩니다(예: ProgressBar).
- 여기서 사용하는 기본 반경 값은 **2px**입니다.

![진행률 표시줄 예제](images/rounded-corner/bars.png)

**컨트롤**

- NavigationView 선택 표시기
- Pivot 선택 표시기
- ProgressBar
- ScrollBar(`IndicatorMode=TouchIndicator` 경우)
- 슬라이더
  - ColorPicker 색 슬라이더
  - MediaTransportControls 검색 표시줄 슬라이더

## <a name="customization-options"></a>사용자 지정 옵션

제공되는 기본 모퉁이 반경 값은 고정되어 있지 않으며, 몇 가지 방법으로 모퉁이의 둥근 정도를 쉽게 수정할 수 있습니다. 이렇게 하려면 두 개의 글로벌 리소스를 통하거나, 원하는 사용자 지정 세분성 수준에 따라 컨트롤에서 직접 [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) 속성을 통해 수행할 수 있습니다.

### <a name="when-not-to-round"></a>둥글게 하지 않는 경우

어떤 인스턴스는 컨트롤의 모퉁이를 둥글게 하지 않아야 합니다. 이 경우 기본적으로 둥글게 되어 있지 않습니다.

- SplitButton의 두 부분처럼 컨테이너 내에 있는 여러 UI 요소가 서로 닿아있는 경우입니다. 닿은 곳에 공간이 없어야 합니다.

![SplitButton](images/rounded-corner/split-button-2.png)

- 컨트롤이 ScrollViewer의 일부이기도 한 ScrollBar 컨테이너의 일부인 ScrollBar의 막대같이 다른 컨테이너 내에 있는 경우입니다.

![ScrollBar](images/rounded-corner/scrollbar.png)

- 플라이아웃 UI 요소의 한 쪽이 플라이아웃을 호출하는 UI에 연결된 경우입니다.

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>키보드 포커스 사각형 및 그림자

기본 디자인은 키보드 포커스 사각형 또는 컨트롤 그림자의 모퉁이를 둥글게 하는 특별한 작업을 수행하지 않습니다. 더 높은 모퉁이 반경 값을 사용하면 기능적으로 중단되지는 않습니다. 그러나 더 큰 값으로 인한 원치 않는 시각적 결함을 방지하기 위해 이 사실을 알고 있는 것이 좋습니다.

다음은 어떻게 모퉁이 반경이 크면 그림자가 바람직하지 않은 것처럼 보일 수 있는지에 관한 예제입니다.

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>둥근 모퉁이와 성능

둥근 모퉁이를 자연스럽게 렌더링하기 위해서는 사각 모퉁이를 렌더링하는 데 비해 더 많은 그리기 성능이 사용됩니다. 기본 모퉁이 반경 값을 선택할 때 디자인 원칙뿐 아니라 앱에서 사용하는 경우 기본 컨트롤이 제대로 작동하도록 하는 데에도 주의를 기울였습니다.

이 컨텍스트에서 앱 성능에 대해 고려할 때 주로 페이지 로드 시간과 앱 시작 시간을 고려해야 합니다. 더 큰 UI 화면에서 모퉁이가 둥글면 성능에 더 큰 영향을 준다는 점을 고려하세요. 전체 화면 앱 UI에서는 둥근 모퉁이 그리기를 피하세요. ContentDialog와 같이 UI가 잠깐 표시되고 페이지가 로드된 후에는 별 문제가 되지 않습니다.

### <a name="page-or-app-wide-cornerradius-changes"></a>페이지 또는 앱 전체 CornerRadius 변경 사항

모든 컨트롤의 모퉁이 반경을 제어하는 두 가지 앱 리소스가 있습니다.

- `ControlCornerRadius` - 기본값은 2px입니다.
- `OverlayCornerRadius` - 기본값은 4px입니다.

어느 범위에서든 이러한 리소스의 값을 재정의하면 그에 따라 해당 범위 내의 모든 컨트롤에 영향을 미칩니다.

즉, 원형을 적용할 수 있는 모든 컨트롤의 원형을 변경하려면 다음과 같이 새 CornerRadius 값을 사용하여 두 리소스를 앱 수준에서 정의할 수 있습니다.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">4</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
            </ResourceDictionary>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

또는 페이지나 컨테이너 수준과 같이 특정 범위 내에서 모든 컨트롤의 원형을 변경하려는 경우에는 다음과 같이 유사한 패턴을 따를 수 있습니다.

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> `OverlayCornerRadius` 리소스가 적용되기 위해서는 앱 수준에서 정의되어야 합니다.
>
>팝업과 플라이아웃은 시각적 트리(Visual Tree)의 루트 요소에서 동적으로 생성되므로 이들이 사용하는 모든 리소스도 그곳에서 정의되어야 하기 때문입니다. 그렇지 않으면, 범위를 벗어나게 됩니다.

### <a name="per-control-cornerradius-changes"></a>컨트롤 기준 CornerRadius 변경 사항

컨트롤의 선택 번호에 대해서만 원형을 변경하려는 경우 컨트롤의 [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) 속성을 직접 수정할 수 있습니다.

|Default | 수정된 속성 |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

모든 컨트롤의 모퉁이가 수정된 `CornerRadius` 속성에 응답하는 것은 아닙니다. 모퉁이를 둥글게 만들려는 컨트롤이 실제로 예상한 대로 `CornerRadius` 속성에 응답하도록 하려면 먼저 `ControlCornerRadius` 또는 `OverlayCornerRadius` 글로벌 리소스가 해당 컨트롤에 영향을 미치는지 확인합니다. 영향을 미치지 않는 경우 둥글게 만들려는 컨트롤에 모퉁이가 있는지 확인합니다. 대부분의 컨트롤은 실제 에지를 렌더링하지 않으므로 `CornerRadius` 속성을 제대로 사용할 수 없습니다.

### <a name="basing-custom-styles-on-winui"></a>WinUI 기반 사용자 지정 스타일

스타일에 올바른 `BasedOn` 특성을 지정하여 WinUI 둥근 모서리 스타일을 기준으로 사용자 지정 스타일을 만들 수 있습니다. 예를 들어, WinUI 단추 스타일을 기준으로 사용자 지정 단추 스타일을 만들려면 다음을 수행합니다.

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

일반적으로 WinUI 컨트롤 스타일은 일관된 명명 규칙 "DefaultXYZStyle"을 따릅니다. 여기서 "XYZ"는 컨트롤의 이름입니다. 전체 내용을 참조하려면 WinUI 리포지토리에서 XAML 파일을 찾아볼 수 있습니다.
