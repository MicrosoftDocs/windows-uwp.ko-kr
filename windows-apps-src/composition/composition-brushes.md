---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 컴퍼지션 브러시
description: 브러시는 해당 출력으로 Visual 영역을 그립니다. 각 브러시의 출력 유형은 서로 다릅니다.
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0f7bcb745245c896d1fc72c2e324af341b2e5aac
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683866"
---
# <a name="composition-brushes"></a>컴퍼지션 브러시
UWP 응용 프로그램에서 화면에 보이는 모든 것은 브러쉬로 그려졌기 때문에 볼 수 있습니다. 브러쉬를 사용하면 단순한 단색이나 이미지 또는 그림, 복잡한 효과 체인 등 다양한 콘텐츠로 사용자 인터페이스(UI) 개체를 그릴 수 있습니다. 이 항목에서는 CompositionBrush를 이용한 그리기 작업의 개념을 소개합니다.

XAML UWP 앱에서 작업하는 경우 [XAML Brush](/windows/uwp/design/style/brushes) 또는 [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)로 UIElement를 그리기로 선택할 수 있습니다. 일반적으로 XAML Brush가 해당 시나리오를 지원하는 경우 XAML Brush를 선택하는 것이 쉽고 효과적입니다. 예를 들어, 버튼 색상에 애니메이션을 적용하고 텍스트 채우기 또는 이미지 형태를 변경합니다. 반면에 애니메이션이 적용 된 마스크 또는 애니메이션 9 그리드 늘이기 나 효과 체인으로 그리기와 같은 XAML 브러시에서 지원 하지 않는 작업을 수행 하려는 경우 CompositionBrush를 사용 하 여 [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)를 사용 하 여 UIElement를 그릴 수 있습니다.

시각적 계층 작업을 하는 경우 [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) 영역을 그리려면 CompositionBrush를 사용해야 합니다.

-   [필수 구성 요소](./composition-brushes.md#prerequisites)
-   [CompositionBrush를 사용 하 여 그리기](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [단색으로 그리기](./composition-brushes.md#paint-with-a-solid-color)
    -   [선형 그라데이션으로 그리기](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [방사형 그라데이션으로 그리기](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [이미지로 그리기](./composition-brushes.md#paint-with-an-image)
    -   [사용자 지정 그리기로 그리기](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [비디오를 사용 하 여 그리기](./composition-brushes.md#paint-with-a-video)
    -   [필터 효과를 사용 하 여 그리기](./composition-brushes.md#paint-with-a-filter-effect)
    -   [불투명 마스크를 사용 하 여 CompositionBrush으로 그리기](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [NCompositionBrush Grid stretch를 사용 하 여 그림판으로 그리기](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [배경 픽셀을 사용 하 여 그리기](./composition-brushes.md#paint-using-background-pixels)
-   [CompositionBrushes 결합](./composition-brushes.md#combining-compositionbrushes)
-   [XAML 브러시 및 CompositionBrush 사용](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [관련 항목](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>전제 조건
이 개요에서는 [시각적 계층 개요](visual-layer.md)에 설명된 기본 컴퍼지션 응용 프로그램 구조에 익숙하다는 것을 전제로 합니다.

## <a name="paint-with-a-compositionbrush"></a>CompositionBrush로 그리기

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)는 출력으로 영역을 "그립니다". 각 브러시의 출력 유형은 서로 다릅니다. 어떤 브러시는 단색으로 영역을 그리고, 또 어떤 브러시는 그라데이션, 이미지, 사용자 지정 그림 또는 효과를 사용합니다. 다른 브러시의 동작을 수정하는 특수 브러시도 있습니다. 예를 들어 불투명 마스크를 사용하여 CompositionBrush가 어떤 영역을 그릴지 제어하거나, 9개 모눈을 사용하여 영역을 그릴 때 CompositionBrush에 적용되는 확장을 제어할 수 있습니다. CompositionBrush는 다음 유형 중 하나일 수 있습니다.

|클래스                                   |세부 정보                                         |도입된 위치|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |단색으로 영역 그리기                        |Windows 10, 버전 1511 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |[ICompositionSurface](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ICompositionSurface)의 콘텐츠로 영역 그리기|Windows 10, 버전 1511 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |컴퍼지션 효과의 콘텐츠로 영역 그리기 |Windows 10, 버전 1511 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |불투명 마스크와 CompositionBrush로 시각 항목 그리기 |Windows 10, 버전 1607 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |9개 모눈 확장을 이용해 CompositionBrush로 영역 그리기 |Windows 10, 버전 1607 (SDK 14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|선형 그라데이션으로 영역 그리기                    |Windows 10, 버전 1709 (SDK 16299)
|[CompositionRadialGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionradialgradientbrush)|방사형 그라데이션으로 영역을 그립니다.                    |Windows 10, 버전 1903 (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |응용 프로그램 또는 데스크톱의 응용 프로그램 창 바로 뒤에 있는 픽셀에서 배경 픽셀을 샘플링하여 영역을 그립니다. CompositionEffectBrush처럼 다른 CompositionBrush에 대한 입력으로 사용됨 | Windows 10, 버전 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>단색으로 그리기

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)가 단색으로 영역을 그립니다. SolidColorBrush의 색을 지정하는 다양한 방법이 있습니다. 예를 들어 알파, 빨간색, 녹색 및 파란색(ARGB) 채널을 지정하거나 [색](https://docs.microsoft.com/uwp/api/windows.ui.colors) 클래스에서 제공하는 미리 정의된 색상 중 하나를 사용할 수 있습니다.

다음 그림 및 코드에서는 검은색 브러시로 선을 긋고 색상 값이 0x9ACD32인 단색 브러시로 칠한 사각형을 만드는 작은 시각적 트리를 보여 줍니다.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>선형 그라데이션으로 그리기

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)는 선형 그라데이션으로 영역을 그립니다. 선형 그라데이션은 선, 그라데이션 축에 두 가지 이상의 색상을 혼합합니다. GradientStop 개체를 사용하여 그라데이션의 색상과 위치를 지정합니다.

다음 그림과 코드는 빨간색과 노란색을 사용하여 2개 스톱이 있는 LinearGradientBrush로 그린 SpriteVisual을 보여줍니다.

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-radial-gradient"></a>방사형 그라데이션으로 그리기

[CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush) 방사형 그라데이션으로 영역을 그립니다. 방사형 그라데이션은 타원의 중심에서 시작 하 여 타원의 반경에서 끝나는 그라데이션을 사용 하 여 두 개 이상의 색을 혼합 합니다. GradientStop 개체는 그라데이션에서 색 및 해당 위치를 정의 하는 데 사용 됩니다.

다음 그림 및 코드에서는 RadialGradientBrush가 2 인 GradientStops를 사용 하 여 그린 SpriteVisual를 보여 줍니다.

![CompositionRadialGradientBrush](images/radial-gradient-brush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionRadialGradientBrush RGBrush;

_compositor = Window.Current.Compositor;

RGBrush = _compositor.CreateRadialGradientBrush();
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Aquamarine));
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.DeepPink));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = RGBrush;
_gradientVisual.Size = new Vector2(200, 200);
```

### <a name="paint-with-an-image"></a>이미지로 그리기

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)는 ICompositionSurface에 렌더링된 픽셀로 영역을 그립니다. 예를 들어 CompositionSurfaceBrush를 사용하여 ICompositionSurface surface using [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API에 렌더링된 이미지로 영역을 그릴 수 있습니다.

다음 그림과 코드는 LoadedImageSurface를 사용해 ICompositionSurface에 렌더링된 licorice의 비트맵으로 그린 SpriteVisual을 보여줍니다. CompositionSurfaceBrush의 속성을 사용하여 시각의 경계 내에서 비트맵을 확장하고 정렬할 수 있습니다.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>사용자 지정 그림으로 그리기
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)를 사용하면 [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)(또는 D2D)를 사용해 렌더링된 ICompositionSurface의 픽셀로 영역을 그릴 수도 있습니다.

다음 코드는 Win2D를 사용하여 ICompositionSurface에 렌더링된 텍스트 실행으로 그린 SpriteVisual을 보여줍니다. Win2D를 사용하려면 프로젝트에 [Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) 패키지를 포함시켜야 합니다.

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

마찬가지로, CompositionSurfaceBrush를 사용하면 Win2D 상호 운용성을 사용하여 SwapChain으로 SpriteVisual을 그릴 수도 있습니다. [이 샘플](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)은 Win2D를 사용하여 스왑 체인으로 SpriteVisual을 그리는 방법에 대한 예제를 제공합니다.

### <a name="paint-with-a-video"></a>비디오로 그리기
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)를 사용하면 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 통해 로드한 비디오를 사용해 렌더링된 ICompositionSurface의 픽셀로 영역을 그릴 수도 있습니다.

다음 코드는 ICompositionSurface에 로드된 비디오로 그린 SpriteVisual을 보여줍니다.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>필터 효과로 그리기

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)는 CompositionEffect의 출력으로 영역을 그립니다. 시각적 계층의 효과는 색상, 그라데이션, 이미지, 비디오, 스왑 체인, UI 영역 또는 화면 효과의 트리 등, 소스 콘텐츠의 컬렉션에 적용되는 애니메이션 가능한 필터 효과라고 할 수 있습니다. 소스 콘텐츠는 일반적으로 다른 CompositionBrush를 사용하여 지정됩니다.

다음 그림과 코드는 채도 감소 필터 효과가 적용된 고양이 이미지로 그린 SpriteVisual을 보여줍니다.

![CompositionEffectBrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

CompositionBrush를 이용한 효과 만들기에 대한 자세한 내용은 [시각 계층의 효과](https://docs.microsoft.com/windows/uwp/composition/composition-effects)를 참조하세요.

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>불투명 마스크가 적용된 CompositionBrush로 그리기

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)는 불투명 마스크가 적용된 CompositionBrush로 영역을 그립니다. 불투명 마스크의 소스는 CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush, 또는 CompositionNineGridBrush 유형의 모든 CompositionBrush일 수 있습니다. 불투명 마스크는 CompositionSurfaceBrush로 지정되어야 합니다.

다음 그림과 코드는 CompositionMaskBrush로 그린 SpriteVisual을 보여줍니다. 마스크의 소스는 원의 이미지를 마스크로 사용해 원과 비슷하게 보이도록 마스킹된 CompositionLinearGradientBrush입니다.

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>9개 모눈 확장을 이용해 CompositionBrush로 그리기

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)는 9개 모눈 메타포를 이용해 확장되는 CompositionBrush로 영역을 그립니다. 9개 모눈 메타포를 사용하면 CompositionBrush의 가장자리 및 모서리를 중심과 다르게 늘릴 수 있습니다. 9개 모눈 확장의 소스는 CompositionColorBrush, CompositionSurfaceBrush 또는 CompositionEffectBrush 유형의 모든 CompositionBrush일 수 있습니다.

다음 코드는 CompositionNineGridBrush로 그린 SpriteVisual을 보여줍니다. 마스크의 소스는 9개 모눈을 사용하여 확장되는 CompositionSurfaceBrush입니다.

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>배경 픽셀을 사용해 그리기

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)는 영역 뒤의 콘텐츠로 영역을 그립니다. CompositionBackdropBrush는 단독으로 사용되지 않고, EffectBrush 등의 다른 CompositionBrush에 대한 입력으로 사용됩니다. 예를 들어 흐림 효과에 대한 입력으로 CompositionBackdropBrush를 사용하면 불투명한 유리 효과를 얻을 수 있습니다.

다음 코드는 CompositionSurfaceBrush와 이미지 위에 불투명한 유리 오버레이를 사용하여 이미지를 만드는 작은 시각적 트리를 보여줍니다. 불투명한 유리 오버레이는 이미지 위에 EffectBrush가 채워진 SpriteVisual을 배치하여 만듭니다. EffectBrush는 CompositionBackdropBrush를 흐림 효과에 대한 입력으로 사용합니다.

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>CompositionBrush 결합
다수의 CompositionBrush가 다른 CompositionBrush를 입력으로 사용합니다. 예를 들어 SetSourceParameter 메서드를 사용하면 다른 CompositionBrush를 CompositionEffectBrush에 대한 입력으로 설정할 수 있습니다. 아래 표에는 지원되는 CompositionBrush 조합이 나와 있습니다. 지원되지 않는 조합을 사용하면 예외가 발생합니다.

<table>
<tbody>
<tr>
<th>브러시</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>예</td>
<td>예</td>
<td>예</td>
<td>예</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>예</td>
<td>예</td>
<td>예</td>
<td>아니요</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>예</td>
<td>예</td>
<td>예</td>
<td>예</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>아니요</td>
<td>아니요</td>
<td>예</td>
<td>아니요</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>아니요</td>
<td>아니요</td>
<td>아니요</td>
<td>아니요</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>예</td>
<td>예</td>
<td>예</td>
<td>아니요</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>예</td>
<td>아니요</td>
<td>아니요</td>
<td>아니요</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>XAML Brush와 CompositionBrush 사용 비교

다음 표는 응용 프로그램에서 UIElement 또는 SpriteVisual을 그릴 때 XAML 또는 컴퍼지션 브러시 사용이 규정되었는지 여부가 표시된 시나리오 목록입니다. 

> [!NOTE]
> XAML UIElement에 대해 CompositionBrush가 제안된 경우 CompositionBrush가 XamlCompositionBrushBase를 사용하여 패키징된 것으로 간주됩니다.

|시나리오                                                                   | XAML UIElement                                                                                                |컴퍼지션 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|단색으로 영역 그리기                                             |[System.windows.media.solidcolorbrush>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|애니메이션이 적용된 색으로 영역 그리기                                          |[System.windows.media.solidcolorbrush>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|정적 그라데이션으로 영역 그리기                                       |[System.windows.media.lineargradientbrush.startpoint](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|애니메이션 적용된 그라데이션 스톱으로 영역 그리기                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|이미지로 영역 그리기                                                |[ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)                                     |[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|웹 페이지로 영역 그리기                                               |[WebViewBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)                                   |해당 없음
|9개 모눈 확장을 이용해 이미지로 영역 그리기                         |[이미지 컨트롤](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|애니메이션 적용된 9개 모눈 확장으로 영역 그리기                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|스왑 체인으로 영역 그리기                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |스왑 체인 상호 운용성 포함 [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|비디오로 영역 그리기                                                 |[MediaElement](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback)                                                                                                  |미디어 상호 운용성 포함 [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|사용자 지정 2D 그림으로 영역 그리기                                       |Win2D의 [CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |Win2D 상호 운용성 포함 [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|애니메이션이 적용되지 않은 마스크로 영역 그리기                                       |XAML[셰이프](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)을 사용하여 마스크 정의   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|애니메이션이 적용된 마스크로 영역 그리기                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|애니메이션이 적용된 필터 효과로 영역 그리기                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|배경 픽셀에 적용된 효과로 영역 그리기        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>관련 항목

[BeginDraw 및 EndDraw를 사용 하 여 네이티브 DirectX 및 Direct2D interop 컴퍼지션](composition-native-interop.md)

[XamlCompositionBrushBase를 사용 하는 XAML 브러시 interop](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
