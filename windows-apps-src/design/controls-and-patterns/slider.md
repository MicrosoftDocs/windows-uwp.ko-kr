---
author: QuinnRadich
Description: Lets the user set a value in a given range.
title: 슬라이더
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a284625bdfa76fd1fe41948f01e64e1bea3d2644
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4260538"
---
# <a name="sliders"></a>슬라이더

 

슬라이더는 사용자가 트랙을 따라 Thumb 컨트롤을 이동하여 값 범위에서 값을 선택할 수 있도록 하는 컨트롤입니다.

> **중요 API**: [Slider 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), [Value 속성](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx), [ValueChanged 이벤트](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)

![슬라이더 컨트롤](images/controls/slider.png)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

정의된 연속 값(예, 볼륨 또는 밝기) 또는 불연속 값 범위(예, 화면 해상도 설정)를 사용자가 설정할 수 있도록 하려면 슬라이더를 사용합니다.

슬라이더는 숫자 값이 아닌 상대적 수량의 경우 유용한 방법입니다. 예를 들어, 값을 2 또는 5로 설정하는 것이 아니라 오디오 볼륨을 낮게 또는 중간으로 설정하고자 하는 경우가 있습니다.

둘 중 하나를 선택하는 문제에는 슬라이더를 사용하지 마세요. 대신 [토글 스위치](toggles.md)를 사용합니다.

다음은 슬라이더를 사용할지 여부를 결정할 때 고려해야 하는 추가 요소입니다.

-   **설정이 상대 수량처럼 보이나요?** 그렇지 않다면 [라디오 단추](radio-button.md) 또는 [목록 상자](lists.md)를 사용합니다.
-   **설정이 정확하고 알려진 숫자 값인가요?** 그렇다면 숫자 [입력란](text-box.md)을 사용합니다.
-   **설정을 변경하면 어떤 효과가 있는지에 대해 즉각적인 피드백을 받게 되면 사용자에게 이익이 되나요?** 그렇다면 슬라이더를 사용합니다. 예를 들어 사용자는 색상, 채도 및 광도 값의 변경 효과를 즉시 확인함으로써 색상을 더 쉽게 선택할 수 있습니다.
-   **설정에서 값 범위가 4개 이상인가요?** 그렇지 않다면 [라디오 단추](radio-button.md)를 사용합니다.
-   **사용자가 값을 변경할 수 있나요?** 슬라이더는 사용자 조작을 위한 것입니다. 사용자가 값을 변경할 수 없으면 대신 읽기 전용 텍스트를 사용합니다.

슬라이더와 숫자 입력란 중 결정하려 할 때, 다음과 같은 경우에는 숫자 입력란을 사용합니다.

-   화면 공간이 비좁은 경우
-   사용자가 키보드로 입력하는 것을 좋아하는 경우

다음 경우에는 슬라이더를 사용합니다.

-   즉각적인 피드백이 사용자에게 도움이 되는 경우

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Slider">앱을 열고 작동 중인 슬라이더를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Windows Phone에서 볼륨을 제어하는 슬라이더입니다.

![Windows Phone에서 볼륨을 제어하는 슬라이더](images/control-examples/slider-phone.png)

Windows 표시 설정에서 텍스트 크기를 변경하는 슬라이더입니다.

![Windows 표시 설정에서 텍스트 크기를 변경하는 슬라이더](images/control-examples/slider-display-settings.png)

## <a name="create-a-slider"></a>슬라이더 만들기

XAML에서 슬라이더를 만드는 방법은 다음과 같습니다.

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

코드로 슬라이더를 만드는 방법은 다음과 같습니다.

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

[Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx) 속성에서 슬라이더 값을 가져오고 설정합니다. 값 변경에 응답하기 위해 데이터 바인딩을 사용하여 Value 속성에 바인딩하거나 [ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx) 이벤트를 처리할 수 있습니다.

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## <a name="recommendations"></a>권장 사항

-   컨트롤은 사용자가 원하는 값을 쉽게 설정할 수 있는 크기로 만듭니다. 불연속 값이 포함된 설정의 경우 마우스를 사용하여 어떤 값이든 쉽게 선택할 수 있도록 만듭니다. 슬라이더의 끝점이 항상 뷰의 범위 내에 있는지 확인합니다.
-   사용자가 실제로 선택을 하는 동안이나 후에 즉각적인 피드백을 제공합니다. 예를 들어 Windows 볼륨 컨트롤은 사용자가 선택한 오디오 볼륨을 들려줍니다.
-   레이블을 사용하여 값의 범위를 표시합니다. 예외: 슬라이더가 세로 방향이고 맨 위 레이블이 최대값, 높음, 많음 등을 나타낼 경우 의미가 분명하므로 다른 레이블을 생략할 수 있습니다.
-   슬라이더를 사용하지 않을 경우 모든 관련 레이블 또는 시각적 피드백도 해제합니다.
-   흐름 방향 및/또는 슬라이더 방향을 설정할 때 텍스트의 방향을 고려합니다. 언어에 따라 스크립트를 왼쪽에서 오른쪽으로 읽거나, 오른쪽에서 왼쪽으로 읽습니다.
-   슬라이더를 진행률 표시기로 사용하지 마세요.
-   슬라이더 위치 조정 컨트롤의 크기는 기본 크기에서 변경하지 마세요.
-   값 범위가 큰 경우에는 연속 슬라이더를 만들지 마세요. 사용자가 범위 내에서 여러 대표 값 중 하나를 선택하게 될 가능성이 큽니다. 이 경우에는 단계로만 선택이 가능한 값을 사용합니다. 예를 들어 시간 값이 최대 1개월까지 가능하나 사용자는 1분, 1시간, 1일 또는 1개월 중에서만 선택해야 하는 경우 이 4단계만 있는 슬라이더를 만듭니다.

## <a name="additional-usage-guidance"></a>추가 사용법 지침

### <a name="choosing-the-right-layout-horizontal-or-vertical"></a>올바른 레이아웃 선택: 수평 또는 수직

수평 또는 수직으로 슬라이더의 방향을 지정할 수 있습니다. 다음 지침을 사용하여 사용할 레이아웃을 결정합니다.

-   자연스러운 방향을 사용합니다. 예를 들어, 슬라이더가 정상적으로 세로로 표시되는 값(예, 온도)을 표현하는 경우에는 세로 방향을 사용합니다.
-   비디오 앱과 같은 미디어 내에서 검색하는 데 컨트롤이 사용되는 경우에는 수평 방향을 사용합니다.
-   한 방향(가로 또는 세로)으로 이동할 수 있는 페이지에 슬라이더를 사용할 때는 이동 방향과 다른 방향을 슬라이더에 사용합니다. 그렇지 않으면, 페이지를 이동하려 할 때 실수로 슬라이더를 밀어 값이 달라질 수도 있습니다.
-   어떤 방향을 사용할지 잘 모르겠으면 페이지 레이아웃에 가장 잘 맞는 방향을 사용합니다.

### <a name="range-direction"></a>범위 방향

범위 방향은 현재 값에서 최대값으로 슬라이더를 밀어 이동하는 방향입니다.

-   세로 슬라이더의 경우에는 읽기 방향과 관계 없이 슬라이더의 위쪽에 가장 큰 값이 오도록 합니다. 예를 들어, 볼륨 슬라이더의 경우에는 반드시 최대 볼륨 설정이 슬라이더의 위쪽에 오도록 합니다. 다른 종류의 값(예, 요일)인 경우에는 페이지의 읽기 방향을 따릅니다.
-   가로 스타일에서는 왼쪽에서 오른쪽 방향 레이아웃의 경우 낮은 값을 슬라이더의 왼쪽, 오른쪽에서 왼쪽 방향 레이아웃의 경우 낮은 값을 오른쪽에 오도록 합니다.
-   그러나 미디어 검색 막대의 경우 예외적으로 더 작은 값을 항상 슬라이더의 왼쪽에 배치합니다.

### <a name="steps-and-tick-marks"></a>단계 및 눈금

-   슬라이더로 최소값과 최대값 사이의 중간 값도 선택할 수 있도록 하려면 단계 포인트를 사용합니다. 예를 들어 슬라이더를 사용하여 예매할 영화 티켓 수를 지정하는 경우에는 부동 소수점 값을 사용하지 마세요. 단계 값으로 1을 지정하세요.
-   단계(끌기 지점이라고도 함)를 지정하는 경우 마지막 단계가 슬라이더의 최대값에 정렬되어 있는지 확인합니다.
-   사용자에게 중요한 값의 위치를 표시하려면 눈금 표시를 사용합니다. 예를 들어, 확대/축소를 제어하는 슬라이더에는 50%, 100%, 200% 등의 눈금 표시를 설정할 수 있습니다.
-   설정의 근사값을 알 필요가 있을 때는 눈금을 표시합니다.
-   사용자가 컨트롤과 상호 작용하지 않고도 선택한 설정 값을 정확히 알아야 하는 경우에는 눈금과 값 레이블을 표시합니다. 그렇지 않은 경우에는 값 도구 설명을 사용하여 정확한 값을 알 수 있습니다.
-   단계 지점이 분명하지 않은 경우 항상 눈금 표시를 표시합니다. 예를 들어 슬라이더의 너비가 200픽셀이고 슬라이더에 200개의 끌기 지점이 있는 경우에는 사용자가 끌기 동작을 눈치채지 못하므로 눈금 표시를 숨길 수 있습니다. 그러나 끌기 지점이 10개만 있는 경우에는 눈금을 표시합니다.

### <a name="labels"></a>레이블

-   **슬라이더 레이블**

    슬라이더 레이블은 슬라이더의 목적을 나타냅니다.

    -   레이블은 끝에 문장 부호가 오지 않게 합니다(모든 컨트롤 레이블에 대한 규칙임).
    -   대부분의 레이블을 해당 컨트롤 위에 배치하는 형식의 슬라이더인 경우에는 레이블을 슬라이더 위에 놓습니다.
    -   대부분의 레이블을 해당 컨트롤의 측면에 배치하는 형식의 슬라이더인 경우에는 레이블을 측면에 놓습니다.
    -   사용자가 슬라이더를 터치할 때 손가락 때문에 레이블이 안 보일 수 있으므로 레이블을 슬라이더 아래에 배치하지 마세요.
-   **범위 레이블**

    범위 레이블 또는 채우기 레이블은 슬라이더의 최소값과 최대값을 설명합니다.

    -   세로 방향이어서 불필요한 경우가 아니면 슬라이더 범위의 양 끝에 레이블을 지정합니다.
    -   가능하면 한 레이블에 한 단어만 사용합니다.
    -   끝에 문장 부호를 넣지 않습니다.
    -   다음 레이블들은 설명적이어야 하며 나란히 배치해야 합니다. 예: 최대/최소, 많음/적음, 낮음/높음, 작음/큼
-   **값 레이블**

    값 레이블은 슬라이더의 현재 값을 표시합니다.

    -   값 레이블이 필요하면 슬라이더의 아래쪽에 표시합니다.
    -   텍스트는 컨트롤을 기준으로 중앙에 오도록 하고 픽셀 등의 단위를 포함시킵니다.
    -   슬라이더의 위치 조정 컨트롤이 삭제 중에 포함되므로 레이블 또는 다른 시각 효과를 사용하여 현재 값을 다른 방법으로 표시하는 것이 좋습니다. 텍스트 크기를 설정하는 슬라이더는 슬라이더 옆에 있는 올바른 크기의 샘플 텍스트를 렌더링할 수 있습니다.

### <a name="appearance-and-interaction"></a>모양 및 조작

슬라이더는 트랙과 위치 조정 컨트롤로 구성됩니다. 트랙은 입력할 수 있는 값의 범위를 나타내는 막대(다양한 스타일의 눈금을 선택적으로 표시 가능)입니다. 위치 조정 컨트롤은 사용자가 트랙을 탭하거나 앞뒤로 문질러서 위치를 지정할 수 있는 선택기입니다.

슬라이더에는 큰 터치 대상이 있습니다. 터치 접근성을 유지하려면 슬라이더를 디스플레이의 가장자리에서 충분히 멀리 배치해야 합니다.

사용자 지정 슬라이더를 디자인할 경우 사용자에게 모든 필요한 정보를 최대한 복잡하지 않게 제공하는 것이 좋습니다. 사용자가 설정을 이해하기 위해 단위를 알아야 하는 경우 값 레이블을 사용하고, 해당 값을 그래픽으로 나타낼 수 있는 창의적인 방법을 찾습니다. 예를 들어 볼륨을 제어하는 슬라이더는 최소값 쪽에 음파가 없는 스피커 그래픽을 표시하고, 최대값 쪽에 음파가 있는 스피커 그래픽을 표시할 수 있습니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-topics"></a>관련 항목
- [토글 스위치](toggles.md)
- [Slider 클래스](https://msdn.microsoft.com/library/windows/apps/br209614)
