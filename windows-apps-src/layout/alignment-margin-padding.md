---
author: Jwmsft
Description: "페이지의 요소 레이아웃에 영향을 주는 맞춤, 여백 및 안쪽 여백을 사용합니다."
title: "UWP(유니버설 Windows 플랫폼) 앱의 맞춤, 여백 및 안쪽 여백"
ms.assetid: 9412ABD4-3674-4865-B07D-64C7C26E4842
label: Alignment, margin, and padding
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: aba82e911a0641378378bee66d0b07e665ac4b5a
ms.lasthandoff: 02/07/2017

---
# <a name="alignment-margin-and-padding"></a>맞춤, 여백 및 안쪽 여백

차원 속성(너비, 높이 및 제약 조건) 외에도 요소에는 요소가 레이아웃 단계를 통과하고 UI에 렌더링될 때 레이아웃 동작에 영향을 주는 맞춤, 여백 및 안쪽 여백 속성이 있을 수도 있습니다. 상황에 따라 때로는 값이 사용되고 때로는 무시되도록 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 개체의 위치를 지정할 때 일반적인 논리 흐름을 사용하는 맞춤, 여백, 안쪽 여백 및 차원 속성 간의 관계가 있습니다.

## <a name="alignment-properties"></a>맞춤 속성

[**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 및 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749) 속성은 부모 요소에 할당된 레이아웃 공간 내에서 자식 요소의 위치를 지정하는 방법을 설명합니다. 이러한 속성을 함께 사용하면 컨테이너에 대한 레이아웃 논리에 따라 컨테이너(패널 또는 컨트롤) 내에서 자식 요소의 위치가 지정될 수 있습니다. 맞춤 속성은 원하는 레이아웃을 적응형 레이아웃 컨테이너로 암시하므로 기본적으로 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 자식에 설정되고 다른 **FrameworkElement** 컨테이너 부모에서 해석됩니다. 맞춤 값은 요소를 방향의 두 가장자리 중 하나에 맞출지 또는 가운데에 맞출지 지정할 수 있습니다. 그러나 두 맞춤 속성의 기본값은 모두 **Stretch**입니다. **Stretch** 맞춤을 사용하면 요소가 레이아웃에 제공된 공간을 채웁니다. **Stretch**가 기본값이므로 명시적 측정이 없거나 레이아웃의 측정 단계에서 제공된 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 값이 없는 경우 적응형 레이아웃을 더 쉽게 사용할 수 있습니다. 이 기본값을 사용하면 각 컨테이너의 크기를 조정할 때까지 명시적 높이/너비가 컨테이너에 맞지 않아서 잘릴 위험이 없습니다.

> [!NOTE]
> 일반적인 레이아웃 원칙으로, 특정 주요 요소에만 측정을 적용하고 다른 요소에는 적응형 레이아웃 동작을 사용하는 것이 가장 좋습니다. 그러면 사용자가 언제든지 가능한 작업인 최상위 앱 창의 크기를 조정할 때 유연한 레이아웃 동작이 제공됩니다.

 
[**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 및 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 값이 있거나 적응 컨테이너 내에서 클리핑이 발생하는 경우 맞춤 값으로 **Stretch**를 설정해도 레이아웃이 해당 컨테이너의 동작으로 제어됩니다. 패널에서 **Stretch** 값이 **Height** 및 **Width**로 제거된 경우 값이 **Center**인 것처럼 동작합니다.

기본 또는 계산된 높이 및 너비 값이 없는 경우 이러한 차원 값은 수학적으로 **NaN**(숫자가 아님)입니다. 요소는 레이아웃 컨테이너에서 차원을 제공할 때까지 기다립니다. 레이아웃을 실행하면 **Stretch** 맞춤이 사용된 요소에 대한 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 및 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 속성의 값이 지정됩니다. 예를 들어 앱 창 크기 조정 등의 레이아웃 관련 변경으로 인해 다른 레이아웃 주기가 발생하는 경우 적응 동작이 다시 실행될 수 있도록 자식 요소의 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 및 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)에는 **NaN** 값이 유지됩니다.

일반적으로 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 등의 텍스트 요소에는 명시적으로 선언된 너비가 없지만 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)로 쿼리할 수 있는 계산된 너비가 있으며, 해당 너비도 **Stretch** 맞춤을 취소합니다. [**FontSize**](https://msdn.microsoft.com/library/windows/apps/br209657) 속성 및 기타 텍스트 속성과 텍스트 자체는 이미 의도한 레이아웃 크기를 암시합니다. 일반적으로 텍스트를 늘리려고 하지 않습니다.) 컨트롤 내의 콘텐츠로 사용된 텍스트도 동일한 효과가 있습니다. 표시해야 하는 텍스트가 있으면 **ActualWidth**가 계산되며, 원하는 너비와 크기가 컨테이너 컨트롤에도 전달됩니다. 또한 텍스트 요소에는 줄당 글꼴 크기, 줄 바꿈 및 기타 텍스트 속성에 따른 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)가 있습니다.

[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 등의 패널에는 이미 다른 레이아웃 논리(행 및 열 정의, 그릴 셀을 나타내기 위해 요소에 설정되는 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 등의 연결된 속성)가 있습니다. 이 경우 맞춤 속성은 해당 셀의 영역 내에서 콘텐츠를 맞추는 방법에 영향을 주지만 셀 구조와 크기 조정은 **Grid**에서 설정으로 제어됩니다.

항목 컨트롤에서 항목의 기본 형식이 데이터인 항목을 표시하는 경우도 있습니다. 이 경우 [**ItemsPresenter**](https://msdn.microsoft.com/library/windows/apps/br242843)가 사용됩니다. 데이터 자체는 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 파생 형식이 아니지만 **ItemsPresenter**가 파생 형식이므로 프리젠터에 대해 [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 및 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)를 설정할 수 있으며, 해당 맞춤은 항목 컨트롤에 표시되는 데이터 항목에 적용됩니다.

맞춤 속성은 부모 레이아웃 컨테이너 차원에 사용 가능한 추가 공간이 있는 경우에만 적용할 수 있습니다. 레이아웃 컨테이너에서 이미 콘텐츠가 잘리는 경우 맞춤은 클리핑이 적용되는 요소 영역에 영향을 줄 수 있습니다. 예를 들어 `HorizontalAlignment="Left"`를 설정하면 요소의 오른쪽 크기가 잘립니다.

## <a name="margin"></a>여백

[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 속성은 레이아웃 상황에서 요소와 해당 피어 사이의 거리 및 요소가 포함된 컨테이너의 콘텐츠 영역과 요소 사이의 거리를 설명합니다. 차원이 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 및 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)인 경계 상자나 직사각형으로 요소를 간주하는 경우 **Margin** 레이아웃은 해당 직사각형 외부에 적용되고 **ActualHeight** 및 **ActualWidth**에 픽셀을 추가하지 않습니다. 또한 여백은 적중 횟수 테스트 및 소싱 입력 이벤트와 관련해서 요소의 일부로 간주되지 않습니다.

일반 레이아웃 동작에서 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 값의 구성 요소는 마지막에 제약되며, [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 및 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)가 이미 0까지 제약된 후에만 제약됩니다. 따라서 컨테이너에 의해 요소가 이미 잘리거나 제약되는 경우에는 여백에 주의하세요. 그렇지 않으면 여백이 적용된 후 차원 중 하나가 0으로 제약되기 때문에 여백으로 인해 요소가 렌더링되지 않은 것처럼 보일 수 있습니다.

`Margin="20"` 등의 구문을 사용하여 여백 값을 균일하게 지정할 수 있습니다. 이 구문을 사용하면 20픽셀의 균일한 여백이 요소에 적용되며 왼쪽, 위쪽, 오른쪽 및 아래쪽 여백이 모두 20픽셀입니다. 여백 값은 각 값이 순서대로 왼쪽, 위쪽, 오른쪽 및 아래쪽에 적용할 개별 여백을 설명하는 4개의 개별 값 형식을 사용할 수도 있습니다. 예를 들면 `Margin="0,10,5,25"`입니다. [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 속성의 기본 형식은 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 구조체이며 [**Left**](https://msdn.microsoft.com/library/windows/apps/hh673893), [**Top**](https://msdn.microsoft.com/library/windows/apps/hh673840), [**Right**](https://msdn.microsoft.com/library/windows/apps/hh673881) 및 [**Bottom**](https://msdn.microsoft.com/library/windows/apps/hh673775) 값을 개별 **Double** 값으로 갖는 속성이 있습니다.

여백은 가산됩니다. 예를 들어 두 요소가 각각 10픽셀의 균일한 여백을 지정하며 임의 방향으로 인접한 피어인 경우 요소 사이의 거리는 20픽셀입니다.

음수 여백이 허용됩니다. 그러나 음수 여백을 사용하면 잘리거나 피어가 과도하게 그려지는 경우가 많이 발생할 수 있으므로 음수 여백 사용은 일반적인 기법이 아닙니다.

[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 속성을 제대로 사용하면 요소의 렌더링 위치와 인접 요소 및 자식의 렌더링 위치를 세부적으로 제어할 수 있습니다. Visual Studio의 XAML 디자이너 내에서 요소 끌기를 사용하여 요소 위치를 지정하는 경우 수정된 XAML은 일반적으로 위치 변경을 XAML로 다시 직렬화하는 데 사용된 해당 요소의 **Margin** 값을 포함합니다.

[**Paragraph**](https://msdn.microsoft.com/library/windows/apps/br244379)의 기본 클래스인 [**Block**](https://msdn.microsoft.com/library/windows/apps/br244503) 클래스에도 [**Margin**](https://msdn.microsoft.com/library/windows/apps/jj191725) 속성이 있습니다. 일반적으로 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565) 또는 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 개체인 부모 컨테이너 내에서 **Paragraph**의 위치를 지정하는 방법 및 [**RichTextBlock.Blocks**](https://msdn.microsoft.com/library/windows/apps/br244347) 컬렉션의 다른 **Block** 피어를 기준으로 둘 이상 단락의 위치를 지정하는 방법에 유사한 영향을 줍니다.

## <a name="padding"></a>여백

**Padding** 속성은 요소와 요소에 포함된 자식 요소 또는 콘텐츠 사이의 거리를 설명합니다. 둘 이상의 자식을 허용하는 요소인 경우 콘텐츠는 모든 콘텐츠를 포함하는 단일 경계 상자로 처리됩니다. 예를 들어 두 개의 항목을 포함하는 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803)이 있는 경우 항목을 포함하는 경계 상자 주위에 [**Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)이 적용됩니다. 해당 컨테이너에 대한 측정 및 정렬 단계 계산을 수행할 때는 **Padding**이 사용 가능한 크기에서 차감되고, 컨테이너 자체가 컨테이너를 포함하는 항목의 레이아웃 단계를 통과할 때는 원하는 크기 값에 포함됩니다. [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)과 달리 **Padding**은 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)의 속성이 아니며, 실제로 각각 고유한 **Padding** 속성을 정의하는 여러 클래스가 있습니다.

-   [**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459): 모든 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 파생 클래스를 상속합니다. 모든 컨트롤에 콘텐츠가 있는 것은 아니므로 일부 컨트롤(예: [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/dn279268))은 속성을 설정해도 아무 효과가 없습니다. 컨트롤에 테두리가 있는 경우([**Control.BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399) 참조) 해당 테두리 안에 안쪽 여백이 적용됩니다.
-   [**Border.Padding**](https://msdn.microsoft.com/library/windows/apps/br209263): [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209256)/[**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209254) 및 [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) 요소로 만들어진 직사각형 선 사이의 공간을 정의합니다.
-   [**ItemsPresenter.Padding**](https://msdn.microsoft.com/library/windows/apps/hh968021): 항목 컨트롤의 항목에 대해 생성된 화면 효과의 모양에 영향을 주며 각 항목 주위에 지정한 안쪽 여백을 배치합니다.
-   [**TextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br209673) 및 [**RichTextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br227596): 텍스트 요소의 텍스트 주위에 경계 상자를 확장합니다. 이러한 텍스트 요소에는 **Background**가 없으므로 텍스트 안쪽 여백과 텍스트 요소 컨테이너에서 적용된 기타 레이아웃 동작을 시각적으로 구분하기 어려울 수 있습니다. 따라서 텍스트 요소 안쪽 여백은 거의 사용되지 않으며, 포함된 [**Block**](https://msdn.microsoft.com/library/windows/apps/jj191725) 컨테이너([**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br244379)의 경우)의 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br227565) 설정을 사용하는 것이 더 일반적입니다.

각 경우의 동일한 요소에 **Margin** 속성도 있습니다. 여백과 안쪽 여백을 모두 적용하는 경우 외부 컨테이너와 내부 콘텐츠 사이의 거리는 여백과 안쪽 여백의 합계라는 점에서 가산됩니다. 콘텐츠, 요소 또는 컨테이너에 각기 다른 배경 값을 적용한 경우 여백이 끝나고 안쪽 여백이 시작되는 지점이 렌더링 시 표시될 수 있습니다.

## <a name="dimensions-height-width"></a>차원(Height, Width)

[**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208718)의 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208751) 및 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208706) 속성은 레이아웃 단계가 수행될 때 맞춤, 여백 및 안쪽 여백 속성이 동작하는 방식에 영향을 주는 경우가 많습니다. 특히, 실수 **Height** 및 **Width** 값은 **Stretch** 맞춤을 취소하며, 레이아웃 측정 단계에서 설정된 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 값의 가능한 구성 요소로 승격됩니다. **Height** 및 **Width**에는 제약 조건 속성이 있습니다. **Height** 값은 [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/br208731) 및 [**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/br208726)로 제약될 수 있고, **Width** 값은 [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/br208733) 및 [**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/br208728)로 제약될 수 있습니다. 또한 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 및 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)는 레이아웃 단계가 완료된 후 유효한 값만 포함하는 계산된 읽기 전용 속성입니다. 차원과 제약 조건 또는 계산된 속성이 상호 관련된 방식에 대한 자세한 내용은 [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 및 [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751)의 설명을 참조하세요.

## <a name="related-topics"></a>관련 항목

**참조**

* [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718)
* [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751)
* [**FrameworkElement.HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720)
* [**FrameworkElement.VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
* [**FrameworkElement.Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)
* [**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)

