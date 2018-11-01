---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 컴퍼지션 브러시
description: 브러시는 해당 출력으로 Visual 영역을 그립니다. 각 브러시의 출력 유형은 서로 다릅니다.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 730d5ae9062fe39533cd615facaf5beaa7d02ffd
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919790"
---
# <a name="composition-brushes"></a>컴퍼지션 브러시
UWP 응용 프로그램에서 화면에 보이는 모든 것은 브러시로 그려 졌 기 때문에 표시 됩니다. 브러시를 사용 하 여 간단 하 고 단색 색상에서 이미지 또는 드로잉을 복잡 한 효과 체인에 이르기까지 콘텐츠로 사용자 인터페이스 (UI) 개체를 그릴 수 있습니다. 여기서는 CompositionBrush 페인팅의 개념을 소개 합니다.

Note, UWP XAML 응용 프로그램을 사용 하 여 작업 하는 경우 [XAML 브러시](/windows/uwp/design/style/brushes) 또는 [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)는 UIElement를 그릴 수도 있습니다. 일반적으로 것이 쉽고 시나리오는 XAML 브러시에서 지원 되는 경우 XAML 브러시를 선택 하는 것이 좋습니다. 예를 들어, 텍스트 또는 이미지를 사용 하 여 도형의 채우기 변경 단추의 색에 애니메이션을 적용 합니다. 반면에 애니메이션된 9 개 모눈 늘이기 애니메이션된 마스크 또는 효과 체인에 페인트 처럼 브러시 XAML에서 지원 되지 않는 작업을 시도 하는 경우 사용할 수 있습니다 CompositionBrush는 [를 사용 하 여 한 UIElement를 그릴 XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

시각적 계층을 사용 하는 경우는 CompositionBrush는 [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)의 영역을 그리는 데 사용 되어야 합니다.

-   [사전 요구 사항](./composition-brushes.md#prerequisites)
-   [CompositionBrush 사용 하 여 페인트](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [단색으로 그리기](./composition-brushes.md#paint-with-a-solid-color)
    -   [선형 그라데이션으로 그리기](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [이미지를 사용 하 여 페인트](./composition-brushes.md#paint-with-an-image)
    -   [사용자 지정 그리기를 사용 하 여 페인트](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [비디오를 사용 하 여 페인트](./composition-brushes.md#paint-with-a-video)
    -   [필터 효과 사용 하 여 페인트](./composition-brushes.md#paint-with-a-filter-effect)
    -   [불투명 마스크를 사용 하 여 CompositionBrush를 사용 하 여 페인트](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [NineGrid 스트레치를 사용 하는 CompositionBrush를 사용 하 여 페인트](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [배경 픽셀을 사용 하 여 페인트](./composition-brushes.md#paint-using-background-pixels)
-   [CompositionBrushes 결합](./composition-brushes.md#combining-compositionbrushes)
-   [XAML 브러시 및 CompositionBrush를 사용 하 여](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [관련 항목](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>사전 요구 사항
이 개요에서는 익숙한 기본 복합 응용 프로그램의 구조와 [시각적 계층 개요](visual-layer.md)에 설명 된 대로

## <a name="paint-with-a-compositionbrush"></a>CompositionBrush 사용 하 여 페인트

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 는 "페인트" 해당 출력으로 영역을 합니다. 각 브러시의 출력 유형은 서로 다릅니다. 일부 브러시는 단색, 그라데이션, 이미지, 사용자 지정 그리기 또는 효과 사용 하 여 다른 사용자와 영역을 그리기. 다른 브러시의 동작을 수정 하는 특수 브러시도 있습니다. 예를 들어, 불투명 마스크 컨트롤은 CompositionBrush에 의해 어떤 영역을 그리는 데 사용할 수 또는 늘이기 영역 그리기는 CompositionBrush에 적용을 제어 하는 9 표를 사용할 수 있습니다. CompositionBrush는 다음 형식 중 하나로 사용할 수 있습니다.

|클래스                                   |세부 정보                                         |에 도입|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |단색으로 영역을 그립니다.                        |Windows10 년 11 월 업데이트 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |[ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface) 프로그램 내용으로 영역을 그립니다.|Windows10 년 11 월 업데이트 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |복합 효과의 내용으로 영역을 그립니다. |Windows10 년 11 월 업데이트 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |불투명 마스크를 사용 하 여 CompositionBrush 사용 하 여 시각적 요소를 그립니다. |Windows10 기념 업데이트 (14393 SDK)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |NineGrid 스트레치를 사용 하는 CompositionBrush를 사용 하 여 영역을 그립니다. |Windows10 기념 업데이트 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|선형 그라데이션으로 영역을 그립니다.                    |Windows 10 작성자 업데이트 (내부 미리 보기 SDK) 포함
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |두 응용 프로그램에서 배경 픽셀 또는 데스크톱에서 응용 프로그램의 창 뒤에 직접 픽셀을 샘플링 하 여 영역을 그립니다. CompositionEffectBrush와 같은 다른 CompositionBrush에 대 한 입력으로 사용 | 10 기념 업데이트가 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>단색으로 그리기

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) 는 단색으로 영역을 그립니다. 다양 한 방법으로 SolidColorBrush의 색을 지정할 수 있습니다. 예를 들어, 알파, 빨강, 파랑 및 녹색 (ARGB) 채널을 지정 하거나 [색](https://docs.microsoft.com/uwp/api/windows.ui.colors) 클래스에서 제공 하는 미리 정의 된 색 중 하나를 사용 하 여 있습니다.

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

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) 는 선형 그라데이션으로 영역을 그립니다. 선형 그라데이션의 그라데이션 축 선을 통해 둘 이상의 색을 혼합합니다. GradientStop 개체를 사용 하 여 변화와 그 위치에 색상을 지정 합니다.

2 정지는 빨간색 및 노란색 색으로 칠해진는 LinearGradientBrush와 SpriteVisual는 다음 그림과 코드를 보여 줍니다.

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

### <a name="paint-with-an-image"></a>이미지를 사용 하 여 페인트

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 에 ICompositionSurface로 렌더링 되는 픽셀을 사용 하 여 영역을 그립니다. 예를 들어, CompositionSurfaceBrush는 [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API를 사용 하 여 전자 ICompositionSurface 화면에 렌더링 된 이미지를 사용 하 여 영역을 그리는 데 사용할 수 있습니다.

SpriteVisual는 LoadedImageSurface를 사용 하는 ICompositionSurface에 렌더링 된는 licorice의 비트맵을 사용 하 여 그린 다음 그림과 코드를 보여 줍니다. CompositionSurfaceBrush의 속성을 늘이고 정렬 시각적 개체의 경계 내에서 비트맵을 사용할 수 있습니다.

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

### <a name="paint-with-a-custom-drawing"></a>사용자 지정 그리기를 사용 하 여 페인트
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 는 [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) (또는 D2D)를 사용 하 여 렌더링 되는 ICompositionSurface 파일에서 픽셀을 사용 하 여 영역을 그리는 데 사용할 수도 있습니다.

Win2D를 사용 하는 SpriteVisual는 ICompositionSurface로 렌더링 된 텍스트를 사용 하 여 그린 다음 코드에서 보여줍니다. 참고 [Win2D NuGet](http://www.nuget.org/packages/Win2D.uwp) 패키지를 프로젝트에 포함 해야 하는 Win2D를 사용 하려면.

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

마찬가지로, CompositionSurfaceBrush는 데도 사용할 수 있습니다 Win2D interop를 사용 하는 SwapChain와 SpriteVisual는 그릴. [이 샘플](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) 에 Win2D는 SpriteVisual에는 swapchain 그리는 데 사용 하는 방법의 예가 나와 있습니다.

### <a name="paint-with-a-video"></a>비디오를 사용 하 여 페인트
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 는 [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 통해 로드 된 비디오를 사용 하 여 렌더링 되는 ICompositionSurface 파일에서 픽셀을 사용 하 여 영역을 그리는 데 사용할 수도 있습니다.

다음 코드는 SpriteVisual는 ICompositionSurface에 로드 된 비디오로 그리는 보여 줍니다.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("http://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
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

### <a name="paint-with-a-filter-effect"></a>필터 효과 사용 하 여 페인트

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) 에 CompositionEffect의 출력을 사용 하 여 영역을 그립니다. 시각적 계층에서 효과 소스 콘텐츠 색, 그라데이션, 이미지, 비디오, swapchains, ui, 지역 또는 비주얼 트리 등의 컬렉션에 적용 된 애니메이션 필터 효과로 생각할 수 있습니다. 소스는 일반적으로 다른 CompositionBrush를 사용 하 여 지정 됩니다.

다음 그림과 코드 그린 고양이 흑백 처리 필터 효과가 적용 된 이미지는 SpriteVisual를 보여 줍니다.

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

CompositionBrushes를 사용 하 여 효과 만드는 방법에 대 한 자세한 내용은 [시각적 계층에서 효과](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects) 참조 하십시오.

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>불투명 마스크가 적용 된 CompositionBrush를 사용 하 여 페인트

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) 는 불투명 마스크가 적용 된 CompositionBrush 사용 하 여 영역을 그립니다. 불투명 마스크의 모든 CompositionBrush CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush, 또는 CompositionNineGridBrush 형식의 수 있습니다. 불투명 마스크는 CompositionSurfaceBrush로 지정 해야 합니다.

다음 그림과 코드는 SpriteVisual를 CompositionMaskBrush로 그리는 보여 줍니다. 마스크는 마스크로 원의 이미지를 사용 하는 원 처럼 마스크는 CompositionLinearGradientBrush입니다.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>NineGrid 스트레치를 사용 하는 CompositionBrush를 사용 하 여 페인트

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) 는 눈금 9 비유를 사용 하 여 확장 하는 CompositionBrush 사용 하 여 영역을 그립니다. 9 개 모눈 비유에서는 중심 보다는 CompositionBrush의 가장자리 및 모서리를 다르게 늘릴 수 있습니다. 9 개 모눈 늘이기의 원본을 모든 유형의 CompositionColorBrush CompositionBrush, CompositionSurfaceBrush, 또는 CompositionEffectBrush 할 수 있습니다.

다음 코드는 SpriteVisual를 CompositionNineGridBrush로 그리는 보여 줍니다. 마스크의 9 눈금을 사용 하 여 확장 하는 CompositionSurfaceBrush입니다.

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

### <a name="paint-using-background-pixels"></a>배경 픽셀을 사용 하 여 페인트

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) 는 영역 뒤 콘텐츠로 영역을 그립니다. CompositionBackdropBrush는 그 자체로 사용 되지 않는 있지만 대신 한 EffectBrush와 같은 다른 CompositionBrush에 대 한 입력으로 사용 됩니다. 예를 들어, CompositionBackdropBrush는 흐림 효과에 대 한 입력을 사용 하 여 얼음된 유리 효과 얻을 수 있습니다.

다음은 CompositionSurfaceBrush와 이미지 위에 서리로 덮인된 유리 오버레이 사용 하 여 이미지를 만드는 작은 시각적 트리를 보여 줍니다. 서리로 덮인된 유리 오버레이 이미지 위에 EffectBrush로 채워진 SpriteVisual를 배치 하 여 만들어집니다. EffectBrush는 흐림 효과에 대 한 입력으로는 CompositionBackdropBrush를 사용합니다.

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

## <a name="combining-compositionbrushes"></a>CompositionBrushes 결합
CompositionBrushes 많은 입력으로 다른 CompositionBrushes를 사용 합니다. 예를 들어, SetSourceParameter 메서드를 사용 하 여 용도 다른 CompositionBrush는 CompositionEffectBrush에 대 한 입력으로 설정 합니다. 아래 표에서 CompositionBrushes의 지원 되는 조합을 설명합니다. 메모는 지원 되지 않는 사용 하 여 예외를 throw 합니다.

<table>
<tbody>
<tr>
<th>Brush</th>
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
<td>NO</td>
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
<td>NO</td>
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
<td>NO</td>
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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>XAML 브러시 및 CompositionBrush를 사용 하 여

다음 표에서 시나리오 및 XAML 또는 복합 브러시 사용 규정 여부는 UIElement 또는 응용 프로그램에 SpriteVisual를 그릴 때의 목록을 제공 합니다. 

> [!NOTE]
> CompositionBrush는 XAML UIElement에 대 한 제안 되는 CompositionBrush는 XamlCompositionBrushBase를 사용 하 여 패키지 된 간주 됩니다.

|시나리오                                                                   | XAML UIElement                                                                                                |컴포지션 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|단색으로 영역 그리기                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|애니메이션된 색으로 영역 그리기                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|정적 그라데이션으로 영역 그리기                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|그라데이션 중지점이 애니메이션된으로 영역 그리기                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|이미지로 영역 그리기                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|웹 페이지를 사용 하 여 영역 그리기                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |해당 없음
|NineGrid 스트레치를 사용 하 여 이미지 영역 그리기                         |[이미지 컨트롤](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|NineGrid 늘이기 애니메이션된으로 영역 그리기                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|swapchain로 영역 그리기                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |W swapchain interop / [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|비디오로 영역 그리기                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) 미디어 interop w /
|사용자 정의 2D 드로잉으로 영역 그리기                                       |Win2D에서 [CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |W Win2D interop / [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|애니메이션이 적용 되지 않은 마스크 영역 그리기                                       |XAML [셰이프](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) 를 사용 하 여 마스크를 정의 하려면   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|애니메이션된 마스크를 사용 하 여 영역 그리기                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|애니메이션된 필터 효과로 영역 그리기                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|배경 픽셀에 적용 된 효과로 영역 그리기        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>관련 항목

[컴포지션 네이티브 Direct2D 및 DirectX interop EndDraw 및 BeginDraw](composition-native-interop.md)

[XamlCompositionBrushBase 사용 하 여 XAML 브러시 interop](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
