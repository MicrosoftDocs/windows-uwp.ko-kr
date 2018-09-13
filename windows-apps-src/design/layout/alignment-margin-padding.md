---
author: QuinnRadich
Description: Use alignment, margin, and padding properties to arrange the layout of elements on a page.
title: 레이아웃 맞춤, 여백 및 안쪽 여백
ms.author: quradic
ms.date: 03/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a45e89c63ec12cb7b77997eac741ebedc415c54
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3957181"
---
# <a name="alignment-margin-padding"></a>맞춤, 여백, 안쪽 여백

UWP 앱에서는 대부분 사용자 인터페이스(UI) 요소가 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 클래스에서 상속됩니다. 모든 FrameworkElement에는 레이아웃 동작에 영향을 미치는 차원, 맞춤, 여백, 안쪽 여백 속성이 있습니다. 다음은 앱의 UI를 쉽게 이해할 뿐만 아니라 어떤 상황에서도 간단하게 사용할 수 있도록 레이아웃 속성을 사용하는 방법에 대한 간략한 지침입니다.

## <a name="dimensions-height-width"></a>차원(Height, Width)
적절한 크기 조정은 모든 콘텐츠를 명확하고, 쉽게 이해하는 데 효과적입니다. 사용자가 기본 콘텐츠를 확인하기 위해 스크롤하거나 확대/축소할 필요도 없습니다.

![차원을 나타내는 다이어그램](images/dimensions.svg)

- [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 및 [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)는 요소의 크기를 지정합니다. 기본 값은 수학적 표현으로 NaN(Not A Number)입니다. [유효 픽셀](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)로 측정된 고정 값을 사용하거나, 혹은 **자동** 또는 [가변 크기 조정](layout-panels.md#grid)을 유동적 동작에 사용할 수 있습니다.

- [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 및 [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)는 런타임 시 요소의 크기를 나타내는 읽기 전용 속성입니다. 유동 레이아웃이 커지거나 줄어드는 경우에는 [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 이벤트의 값도 바뀝니다. 단, [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)은 ActualHeight 값과 ActualWidth 값을 바꾸지 못합니다.

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) 및 [**MinHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight)는 유동 크기 조정을 허용하면서 요소의 크기를 제한하는 값을 지정합니다.

- [**FontSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) 및 기타 텍스트 속성은 텍스트 요소의 레이아웃 크기를 제어합니다. 텍스트 요소가 차원을 명시적으로 선언하지 않은 경우에는 ActualWidth와 ActualHeight를 계산합니다. 

## <a name="alignment"></a>맞춤
맞춤은 UI가 균형 있게 정돈되어 단정하게 보이도록 할 뿐만 아니라 시각적 계층 및 관계를 구성하는 데도 사용됩니다.

![맞춤을 나타낸 다이어그램](images/alignment.svg)

- [**HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 및 [**VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)는 해당 부모 컨테이너 내에서 요소의 위치가 결정되는 방법을 지정합니다.
    - **HorizontalAlignment**에 대한 값은 **Left**, **Center**, **Right** 및 **Stretch**입니다.
    - **VerticalAlignment**에 대한 값은 **Top**, **Center**, **Bottom** 및 **Stretch**입니다.

- **Stretch**는 두 속성의 기본 값이며, 요소가 부모 컨테이너에 제공된 모든 공간을 채웁니다. 실수인 Height 및 Width 값을 지정하면 Stretch 값이 취소되고, 대신 Center 값으로 작용합니다. Button과 같은 일부 컨트롤은 기본 Stretch 값을 해당하는 기본 스타일로 재정의합니다.

- [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) 및 [**VerticalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment)는 컨테이너 내에서 자식 요소의 위치가 결정되는 방법을 지정합니다.

- 맞춤은 레이아웃 패널 내에서 클리핑에 영향을 미칠 수 있습니다. 예를 들어, `HorizontalAlignment="Left"`를 지정했을 때 콘텐츠가 ActualWidth보다 크면 요소의 오른쪽이 잘립니다.

- 텍스트 요소는 [**TextAlignment**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.textalignment) 속성을 사용합니다. 일반적으로 기본 값인 왼쪽 맞춤을 권장합니다. 텍스트 스타일 지정에 대한 자세한 내용은 [입력 체계](../style/typography.md)를 참조하세요.

## <a name="margin-and-padding"></a>여백 및 안쪽 여백
여백 및 안쪽 여백 속성은 UI 밀도가 너무 높거나 낮지 않도록 유지하는 동시에 펜이나 터치 같은 일부 입력 장치를 더욱 쉽게 사용할 수 있도록 하는 효과가 있습니다. 다음은 컨테이너와 컨테이너 내용의 여백과 안쪽 여백을 표시하는 일러스트레이션입니다.

![XAML 여백 및 안쪽 여백 다이어그램](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>여백
[**여백**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)은 요소 주위의 빈 공간 크기를 제어합니다. 여백은 ActualHeight 및 ActualWidth에 픽셀을 추가하지 않으며, 적중 횟수 테스트 및 소싱 입력 이벤트와 관련해서 요소의 일부로 간주되지도 않습니다.

- 여백 값은 동일하거나 서로 다를 수 있습니다. `Margin="20"`일 때는 왼쪽, 위, 오른쪽, 아래 방향으로 20픽셀의 여백이 요소에 동일하게 적용됩니다. `Margin="0,10,5,25"`일 때는 각각 왼쪽, 위, 오른쪽 및 아래 방향으로 값이 적용됩니다. 

- 여백은 가산됩니다. 두 요소가 모두 10픽셀의 균일한 여백을 지정하면서 동시에 임의 방향으로 인접한 피어인 경우에는 요소 사이의 거리가 20픽셀입니다.

- 음수 여백이 허용됩니다. 그러나 음수 여백을 사용하면 잘리거나 피어가 과도하게 그려지는 경우가 많이 발생할 수 있으므로 음수 여백 사용은 일반적인 기법이 아닙니다.

- 여백 값은 결국에는 제약을 받기 마련입니다. 다시 말해서 컨테이너가 요소를 자르거나 제한할 수 있기 때문에 여백을 지정할 때는 주의가 필요합니다. 여백 값은 요소가 렌더링되지 못하는 원인이 되기도 합니다. 즉, 여백을 적용했을 때 요소의 차원이 0이 되는 경우가 발생할 수 있습니다.

### <a name="padding"></a>안쪽 여백
[**안쪽 여백**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding)은 요소의 내부 테두리와 자식 콘텐츠 또는 요소 사이의 간격을 제어합니다. 양수 안쪽 여백 값은 해당 요소의 콘텐츠 영역을 줄입니다. 

여백과 달리 안쪽 여백은 FrameworkElement 속성이 아닙니다. 다음과 같이 각각 자신의 안쪽 여백 속성을 정의하는 클래스가 몇 가지 있습니다.

-   [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding): 모든 [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls) 파생 클래스를 상속합니다. 모든 컨트롤에 콘텐츠가 있는 것은 아니므로 이러한 컨트롤은 속성을 설정해도 아무 효과가 없습니다. 컨트롤에 테두리가 있는 경우 해당 테두리 안에 안쪽 여백이 적용됩니다.
-   [**Border.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding): [**BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) 및 [**Child**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child) 요소로 만들어진 직사각형 선 사이의 공간을 정의합니다.
-   [**ItemsPresenter.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding): 항목 컨트롤의 항목 모양에 영향을 주며 각 항목 주위에 지정한 안쪽 여백을 배치합니다.
-   [**TextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) 및 [**RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding): 텍스트 요소의 텍스트 주위에 경계 상자를 확장합니다. 이러한 텍스트 요소는 **배경**이 없기 때문에 육안으로 확인하기 어려울 수 있습니다. 이때는 [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block) 컨테이너에서 [**여백**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) 설정을 사용하세요.

둘 중 어떠한 경우가 되었든 요소 역시 여백 속성을 갖습니다. 여백과 안쪽 여백을 모두 적용하는 경우 외부 컨테이너와 내부 콘텐츠 사이의 거리는 여백과 안쪽 여백의 합계라는 점에서 가산됩니다.

### <a name="example"></a>예
실제 컨트롤에서 여백 및 안쪽 여백의 효과를 살펴보겠습니다. 다음은 기본 Margin과 Padding 값이 0인 Grid 내에 있는 TextBox입니다.

![여백 및 안쪽 여백이 0인 TextBox](images/xaml-layout-textbox-no-margins-padding.svg)

다음은 이 XAML에 나와 있는 것처럼 TextBox에서 Margin과 Padding 값을 가진 동일한 TextBox와 Grid입니다.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="24,16"/>
</Grid>
```

![양수 여백 값과 안쪽 여백 값을 가진 TextBox](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>스타일 리소스
하나의 컨트롤에서 각 속성 값을 개별적으로 설정할 필요가 없습니다. 일반적으로 속성 값을 [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 리소스로 그룹화하고 Style을 컨트롤에 적용하는 것이 더 효율적입니다. 이는 동일한 속성 값을 많은 컨트롤에 적용해야 하는 경우 특히 그렇습니다. 스타일을 사용하는 방법에 대한 자세한 내용은 [XAML 스타일](../controls-and-patterns/xaml-styles.md)을 참조하세요.

## <a name="general-recommendations"></a>일반 권장 사항
- 측정 값을 일부 주요 요소에만 적용하고 나머지 요소에는 유동 레이아웃 동작을 사용하세요. 이렇게 하면 창 너비가 바뀔 때 [반응형 UI](responsive-design.md)가 가능해집니다.

- 측정 값을 사용하는 경우에는 **모든 차원, 여백 및 안쪽 여백이 4 epx 단위로 증가해야 합니다**. UWP가 [유효 픽셀 및 크기 조정](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)을 사용하여 모든 장치 및 화면 크기에서 앱을 쉽게 알아볼 수 있도록 하는 경우에는 UI 크기를 4의 배수로 조정합니다. 4 단위로 값을 높여 사용하면 전체 픽셀에 맞게 조정되어 최상의 렌더링을 얻을 수 있습니다.

- 창 너비가 작을 때는(640픽셀 미만) 12epx 여백을, 그리고 창 너비가 클 때는 24epx 여백을 권장합니다.

![권장 여백](images/12-gutter.svg)

## <a name="related-topics"></a>관련 항목
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
