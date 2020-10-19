---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: 브러시 사용
description: Brush 개체는 그릴 개체가 UI에 표시되도록 셰이프, 텍스트 및 컨트롤 일부의 내부나 윤곽선을 그리는 데 사용됩니다.
ms.date: 04/28/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1bf42e75ed8bb6d22fe8d4829aa6df32fa130230
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860045"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>브러시를 사용하여 배경, 전경 및 윤곽선 그리기

애플리케이션 UI에 표시되도록 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 개체를 사용하여 XAML 셰이프, 텍스트 및 컨트롤의 내부와 윤곽선을 그립니다.

> **중요 API**:  [Brush 클래스](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>브러시 소개

앱 캔버스에 표시되는 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape), 텍스트 또는 [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control)의 일부를 그리려면 **Shape**의 [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) 속성 또는 **Control**의 [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) 및 [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) 속성을 **Brush** 값으로 설정합니다.

브러시의 여러 유형은 다음과 같습니다. 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>단색 브러시

[**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)는 빨간색 또는 파란색 같은 단일 [**Color**](/uwp/api/Windows.UI.Color)로 영역을 칠합니다. 가장 기본적인 브러시입니다. XAML에는 **SolidColorBrush** 및 이 클래스에서 지정하는 색을 정의하는 세 가지 방법, 즉, 미리 정의된 색 이름, 16진수 색 값, 속성 요소 구문이 있습니다.

### <a name="predefined-color-names"></a>미리 정의된 색 이름

[**Yellow**](/uwp/api/windows.ui.colors.yellow), [**Magenta**](/uwp/api/windows.ui.colors.magenta) 등의 미리 정의된 색 이름을 사용할 수 있습니다. 이름이 지정된 256개 색을 사용할 수 있습니다. XAML 파서가 올바른 색 채널을 통해 색 이름을 [**Color**](/uwp/api/Windows.UI.Color) 구조로 변환합니다. 이름이 지정된 256개 색은 CSS3(CSS 스타일시트, 수준 3) 사양의 *X11* 색 이름을 기반으로 하므로 이전에 웹 개발 또는 디자인 경험이 있는 경우 이름이 지정된 색 목록을 이미 알고 있을 수도 있습니다.

다음은 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) 속성을 미리 정의된 색 [**Red**](/uwp/api/windows.ui.colors.red)로 설정하는 예입니다.

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![렌더링된 SolidColorBrush](images/brushes-solidcolorbrush.jpg)

*사각형에 적용된 SolidColorBrush*

XAML 대신 코드를 사용하여 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)를 정의하는 경우 이름이 지정된 각 색을 [**Colors**](/uwp/api/windows.ui.colors) 클래스의 정적 속성 값으로 사용할 수 있습니다. 예를 들어 **SolidColorBrush**의 [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) 값을 선언하여 이름이 지정된 색 "Orchid"를 나타내려면 **Color** 값을 정적 값 [**Colors.Orchid**](/uwp/api/windows.ui.colors.orchid)로 설정합니다.

### <a name="hexadecimal-color-values"></a>16진수 색 값

16진수 형식 문자열을 사용하여 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)에 대해 정밀한 24비트 색 값과 8비트 알파 채널을 선언할 수 있습니다. 0에서 F 범위의 두 문자는 각 구성 요소 값을 정의하며 16진수 문자열의 구성 요소 값 순서는 알파 채널(불투명도), 빨간색 채널, 녹색 채널 및 파란색 채널(**ARGB**)입니다. 예를 들어 16진수 값 “\#FFFF0000”은 완전히 불투명한 빨간색입니다(알파=“FF”, 빨간색=“FF”, 녹색=“00”, 파란색=“00”).

다음 XAML 예제에서는 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)의 [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) 속성을 16진수 값 “\#FFFF0000”으로 설정하고 이름이 지정된 색 [**Colors.Red**](/uwp/api/windows.ui.colors.red)를 사용하여 동일한 결과를 제공합니다.

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>속성 요소 구문

속성 요소 구문을 사용하여 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)를 정의할 수 있습니다. 이 구문은 이전 방법보다 더 자세하지만 [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity)와 같은 요소의 추가 속성 값을 지정할 수 있습니다. 속성 요소 구문을 포함하여 XAML 구문에 대한 자세한 내용은 [XAML 개요](../../xaml-platform/xaml-overview.md) 및 [XAML 구문 가이드](../../xaml-platform/xaml-syntax-guide.md)를 참조하세요.

이전 예제에서 만드는 브러시는 가장 일반적인 경우에 UI 정의를 단순하게 유지하는 데 도움이 되는 의도적인 XAML 언어 줄임의 일부로 암시적이고 자동으로 만들어집니다. 다음 예제에서는 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 만들고 [**Rectangle.Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) 속성의 요소 값으로 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)를 명시적으로 만듭니다. **SolidColorBrush**의 [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color)는 [**Blue**](/uwp/api/windows.ui.colors.blue)로 설정되어 있고 [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity)는 0.5로 설정되어 있습니다.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>선형 그라데이션 브러시

[**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)는 선을 따라 정의된 그라데이션으로 영역을 칠합니다. 이 선을 *그라데이션 축*이라고 합니다. [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) 개체를 사용하여 그라데이션 축을 따라 그라데이션의 색과 위치를 지정합니다. 기본적으로 그라데이션 축은 브러시가 칠하는 영역의 왼쪽 위에서 오른쪽 아래로 실행되어 대각선 음영을 생성합니다.

[**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop)은 그라데이션 브러시의 기본 구성 요소입니다. 그라데이션 중지점은 칠하는 영역에 브러시를 적용할 때 그라데이션 축의 [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset)에 표시되는 브러시 [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color)를 지정합니다.

그라데이션 중지점의 [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) 속성은 그라데이션 중지점의 색을 지정합니다. 미리 정의된 색 이름을 사용하거나 16진수 **ARGB** 값을 지정하여 색을 설정할 수 있습니다.

[**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop)의 [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) 속성은 그라데이션 축에서 각 **GradientStop**의 위치를 지정합니다. **Offset**은 0에서 1까지의 **double**입니다. **Offset**이 0이면 그라데이션 축의 시작 부분, 즉 [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) 근처에 **GradientStop**이 배치됩니다. **Offset**이 1이면 [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint)에 **GradientStop**이 배치됩니다. 유용한 [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)에는 최소한 두 개의 **GradientStop** 값이 있어야 합니다. 이 경우 각 **GradientStop**은 다른 [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color)를 지정해야 하며 0에서 1 사이의 다른 **Offset**을 가져야 합니다.

다음 예제에서는 4가지 색의 선형 그라데이션을 만들고 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 칠하는 데 사용합니다.

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

그라데이션 중지점 사이에 있는 각 점의 색은 두 개의 경계 그라데이션 중지점에 지정된 색의 조합으로 선형으로 보간됩니다. 다음 이미지에서는 이전 예제의 그라데이션 중지점을 강조 표시하고 있습니다. 원은 그라데이션 중지점의 위치를 나타내고 점선은 그라데이션 축을 나타냅니다.

![다이어그램의 왼쪽 위 모서리에서 시작하여 다이어그램의 오른쪽 아래 모서리에 도달할 때까지 오른쪽 아래로 기울어지면서 1~4의 그라데이션 중지점을 보여주는 다이어그램입니다.](images/linear-gradients-stops.png)

*두 경계 그라데이션 중지점에서 지정된 색의 조합*

[**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) 및 [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) 속성을 `(0,0)` 및 `(1,1)` 시작 기본값과 다른 값으로 설정하여 그라데이션 중지점이 배치되는 선을 변경할 수 있습니다. **StartPoint** 및 **EndPoint** 좌표 값을 변경하여 가로 또는 세로 그라데이션을 만들거나, 그라데이션 방향을 반대로 하거나, 칠해진 전체 영역보다 적은 범위에 적용하기 위해 그라데이션 범위를 좁힐 수 있습니다. 그라데이션 범위를 좁히려면 **StartPoint** 및/또는 **EndPoint**의 값을 0에서 1 사이의 값으로 설정합니다. 예를 들어 브러시 왼쪽 절반에서 모두 페이드하고 오른쪽은 마지막 [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) 색으로 고정되는 가로 그라데이션을 만들려는 경우 **StartPoint**를 `(0,0)`으로, **EndPoint**를 `(0.5,0)`으로 각각 지정합니다.

### <a name="use-tools-to-make-gradients"></a>도구를 사용하여 그라데이션 만들기

이제 선형 그라데이션의 작동 방식을 알았으므로 Visual Studio 또는 Blend를 사용하여 이러한 그라데이션을 보다 쉽게 만들 수 있습니다. 그라데이션을 만들려면 디자인 화면 또는 XAML 뷰에서 그라데이션을 적용할 개체를 선택합니다. **브러시**를 펼치고 **선형 그라데이션** 탭을 선택합니다.

![Visual Studio에서 선형 그라데이션 만들기](images/tool-gradient-brush-1.png)

*Visual Studio에서 선형 그라데이션 만들기*

이제 그라데이션 중지점의 색을 변경하고 아래쪽에 있는 막대를 사용하여 해당 위치를 이동할 수 있습니다. 또한 막대를 클릭하여 새 그라데이션 중지점을 추가하거나 막대에서 중지점을 끌어 그라데이션 중지점을 제거할 수 있습니다(다음 스크린샷 참조).

![그라데이션 중지점을 제어하는 속성 창의 아래쪽에 있는 막대](images/tool-gradient-brush-2.png)

*그라데이션 설정 슬라이더*

## <a name="radial-gradient-brushes"></a>방사형 그라데이션 브러시

[**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)는 [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) 및 [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) 속성으로 정의된 타원 안에 그려집니다. 그라데이션의 색은 타원의 중심에서 시작하고 반지름에서 끝납니다.

방사형 그라데이션의 색은 [**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops) 컬렉션 속성에 추가된 색 중지점으로 정의됩니다. 각 그라데이션 중지점은 그라데이션을 따라 색과 오프셋을 지정합니다.

그라데이션 원점은 기본적으로 중심으로 설정되며, [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) 속성을 사용하는 오프셋일 수 있습니다.

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode)는 [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx), [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) 및 [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin)에서 상대 좌표 또는 절대 좌표를 나타내는지 여부를 정의합니다.

[**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode)가 `RelativeToBoundingBox`로 설정되면 세 속성의 X 및 Y 값이 요소 범위를 기준으로 처리됩니다. 여기서 `(0,0)`은 왼쪽 위를 나타내고, `(1,1)`은 [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) 및 [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) 속성에 대한 요소 범위의 오른쪽 아래를 나타내며, `(0,0)`은 [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) 속성의 중심을 나타냅니다.

[**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode)가 `Absolute`로 설정되면 세 속성의 X 및 Y 값이 요소 범위 내에서 절대 좌표로 처리됩니다.

다음 예제에서는 4가지 색의 선형 그라데이션을 만들고 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)을 칠하는 데 사용합니다.

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

그라데이션 중지점 사이에 있는 각 점의 색은 두 경계 그라데이션 중지점으로 지정된 색의 조합으로 방사형으로 보간됩니다. 다음 이미지에서는 이전 예제의 그라데이션 중지점을 강조 표시하고 있습니다. 

![방사형 그라데이션의 스크린샷입니다.](images/radial-gradient.png)

*그라데이션 중지점*

## <a name="image-brushes"></a>이미지 브러시

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)는 이미지를 사용하여 영역을 그립니다. 그릴 이미지는 이미지 파일 원본에서 제공됩니다. 로드할 이미지의 경로를 사용하여 [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) 속성을 설정합니다. 일반적으로 이미지 원본은 앱 리소스의 일부인 **Content** 항목에서 제공됩니다.

기본적으로 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)는 칠해진 영역을 완전히 채우도록 이미지를 확장하며, 칠해진 영역의 가로 세로 비율이 해당 이미지와 다른 경우 이미지를 변형할 수도 있습니다. [**Stretch**](/uwp/api/windows.ui.xaml.media.tilebrush.stretch) 속성을 기본값 **Fill**에서 변경하고 **None**, **Uniform** 또는 **UniformToFill**로 설정하여 이 동작을 변경할 수 있습니다.

다음 예제에서는 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)를 만들고 [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource)를 licorice.jpg라는 이미지로 설정합니다. 이 이미지는 앱에 리소스로 포함되어야 합니다. 그런 다음, **ImageBrush**가 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 셰이프로 정의된 영역을 칠합니다.

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![렌더링된 ImageBrush.](images/brushes-imagebrush.jpg)

*렌더링된 ImageBrush*

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 및 [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image)는 모두 URI(Uniform Resource Identifier)를 사용하여 이미지 원본 파일을 참조합니다. 이 경우 이미지 원본 파일은 가능한 여러 이미지 형식을 사용합니다. 이러한 이미지 원본 파일은 URI로 지정됩니다. 이미지 원본, 사용 가능한 이미지 형식을 지정하고 앱에 패키징하는 방법에 대한 자세한 내용은 [Image 및 ImageBrush](../controls-and-patterns/images-imagebrushes.md)를 참조하세요.

## <a name="brushes-and-text"></a>브러시 및 텍스트

브러시를 사용하여 텍스트 요소에 렌더링 특성을 적용할 수도 있습니다. 예를 들어 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)의 [**Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) 속성은 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)를 사용합니다. 여기에 설명된 모든 브러시를 텍스트에 적용할 수 있습니다. 그러나 배경에 적용되는 브러시를 사용하는 경우 배경으로 인해 텍스트를 읽을 수 없게 될 수 있으므로 텍스트에 적용되는 브러시에 주의해야 합니다. 텍스트 요소를 주로 장식용으로 사용하려는 경우가 아니라면 대개의 경우 가독성을 위해 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)를 사용하시기 바랍니다.

단색을 사용하는 경우에도 선택한 텍스트 색이 텍스트 레이아웃 컨테이너의 배경색과 충분히 대비되는지 확인하세요. 텍스트 전경과 텍스트 컨테이너 배경 사이의 대비 수준은 접근성과 관련된 고려 사항입니다.

## <a name="webviewbrush"></a>WebViewBrush

[**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)는 일반적으로 [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) 컨트롤에 표시되는 콘텐츠에 액세스할 수 있는 특수 유형의 브러시입니다. 사각형 **WebView** 컨트롤 영역에 콘텐츠를 렌더링하는 대신 **WebViewBrush**는 렌더 화면에 대한 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 유형 속성이 있는 다른 요소에 해당 콘텐츠를 그립니다. **WebViewBrush**는 모든 브러시 시나리오에 적합하지는 않지만 **WebView** 전환에 유용합니다. 자세한 내용은 [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)를 참조하세요.

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)는 [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush)를 사용하여 XAML UI 요소를 그리는 사용자 지정 브러시를 만드는 데 사용되는 기본 클래스입니다.

이렇게 하면 [**시각적 계층 개요**](../../composition/visual-layer.md)에 설명된 대로 Windows.UI.Xaml과 Windows.UI.Composition 계층 간의 “드롭다운” 상호 운용이 가능합니다. 

사용자 지정 브러시를 만들려면 XamlCompositionBrushBase를 상속하는 새 클래스를 만들고 필요한 메서드를 구현합니다.

예를 들어 이 기능을 사용하여 [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)를 통해 XAML UIElement에 [**효과**](/uwp/composition/composition-effects)를 적용할 수 있습니다. 효과에는 [**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight)로 조명을 받을 때 XAML UIElement의 반사 속성을 제어하는 [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect), **GaussianBlurEffect** 등이 있습니다.

코드 예제는 [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)를 참조하세요.

## <a name="brushes-as-xaml-resources"></a>XAML 리소스로 브러시

XAML 리소스 사전에서 임의 브러시를 키 입력 XAML 리소스로 선언할 수 있습니다. 이렇게 하면 UI의 여러 요소에 적용할 때 동일한 브러시 값을 쉽게 복제할 수 있습니다. 그런 다음, 브러시 값을 공유하고 XAML에서 브러시 리소스를 [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension) 사용으로 참조하는 모든 경우에 적용할 수 있습니다. 공유 브러시를 참조하는 XAML 컨트롤 템플릿이 있고 컨트롤 템플릿 자체가 키 입력 XAML 리소스인 경우도 여기에 포함됩니다.

## <a name="brushes-in-code"></a>코드의 브러시

코드를 사용하여 브러시를 정의하는 것보다 XAML을 사용하여 브러시를 지정하는 것이 훨씬 더 일반적입니다. 왜냐하면 브러시는 보통 XAML 리소스로 정의되고 브러시 값은 디자인 도구에서 출력되거나 XAML UI 정의의 일부로 출력되는 경우가 자주 있기 때문입니다. 코드를 사용하여 브러시를 정의하는 특별한 경우에도 모든 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 유형을 코드 인스턴스화에 사용할 수 있습니다.

코드에서 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)를 만들려면 [**Color**](/uwp/api/Windows.UI.Color) 매개 변수를 사용하는 생성자를 사용합니다. 다음과 같이 [**Colors**](/uwp/api/windows.ui.colors) 클래스의 정적 속성인 값을 전달합니다.

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

[**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) 및 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)의 경우 기본 생성자를 사용한 다음, 다른 API를 호출한 후 해당 브러시를 UI 속성에 사용합니다.

-   코드를 사용하여 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)를 정의하는 경우 [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty)에 [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)(URI 아님)가 필요합니다. 소스가 스트림이면 [**SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) 메서드를 사용하여 값을 초기화합니다. 원본이 **ms-appx** 또는 **ms-resource** 구성표를 사용하는 앱에 콘텐츠를 포함하는 URI이면 URI를 사용하는 [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) 생성자를 사용합니다. 또한 이미지 소스를 검색하거나 디코딩하는 데 타이밍 문제가 있는 경우 [**ImageOpened**](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) 이벤트 처리를 고려할 수 있으며, 이미지 소스를 사용할 수 있을 때까지 표시할 대체 콘텐츠가 필요할 수 있습니다.
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)의 경우 최근에 [**SourceName**](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) 속성을 다시 설정하거나 [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView)의 콘텐츠가 코드와 함께 변경된 경우 [**Redraw**](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw)를 호출해야 할 수 있습니다.

코드 예제는 [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 및 [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)를 참조하세요.
 

 