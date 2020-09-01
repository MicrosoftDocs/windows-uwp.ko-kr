---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 컴퍼지션 브러시
description: 브러시는 해당 출력을 사용 하 여 시각적 개체의 영역을 그립니다. 다른 브러시는 다른 유형의 출력 합니다.
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e3c0454b5596490631f32c3481675f3c70d4eb76
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175647"
---
# <a name="composition-brushes"></a>컴퍼지션 브러시
UWP 응용 프로그램에서 화면에 표시 되는 모든 항목은 브러시로 그리기 때문에 표시 됩니다. 브러시를 사용 하면 단순, 단색, 이미지 또는 그리기에서 복잡 한 효과 체인까지 콘텐츠가 포함 된 UI (사용자 인터페이스) 개체를 그릴 수 있습니다. 이 항목에서는 CompositionBrush를 사용 하 여 그리는 개념을 소개 합니다.

XAML UWP 앱으로 작업 하는 경우 [Xaml 브러시](../design/style/brushes.md) 또는 [CompositionBrush](/uwp/api/Windows.UI.Composition.CompositionBrush)를 사용 하 여 UIElement를 그리게 선택할 수 있습니다. 일반적으로 XAML 브러시에서 시나리오를 지 원하는 경우에는 XAML 브러시를 선택 하는 것이 좋습니다. 예를 들어 단추 색에 애니메이션 효과를 적용 하 여 텍스트 또는 모양의 채우기를 이미지로 변경 합니다. 반면에 애니메이션이 적용 된 마스크 또는 애니메이션 9 그리드 늘이기 나 효과 체인으로 그리기와 같은 XAML 브러시에서 지원 하지 않는 작업을 수행 하려는 경우 CompositionBrush를 사용 하 여 [XamlCompositionBrushBase](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)를 사용 하 여 UIElement를 그릴 수 있습니다.

시각적 계층에서 작업 하는 경우 CompositionBrush를 사용 하 여 [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual)영역을 그려야 합니다.

-   [전제 조건](./composition-brushes.md#prerequisites)
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

## <a name="prerequisites"></a>필수 구성 요소
이 개요에서는 [비주얼 계층 개요](visual-layer.md)에 설명 된 대로 기본 컴퍼지션 응용 프로그램의 구조를 잘 알고 있다고 가정 합니다.

## <a name="paint-with-a-compositionbrush"></a>CompositionBrush를 사용 하 여 그리기

[CompositionBrush](/uwp/api/Windows.UI.Composition.CompositionBrush) 는 출력을 사용 하 여 영역을 "그립니다". 다른 브러시는 다른 유형의 출력 합니다. 일부 브러시는 단색으로 영역을 페인트 하 고 다른 색은 그라데이션, 이미지, 사용자 지정 그리기 또는 효과가 있습니다. 다른 브러시의 동작을 수정 하는 특수화 된 브러시도 있습니다. 예를 들어 불투명 마스크를 사용 하 여 CompositionBrush에서 칠하는 영역을 제어 하거나, 9 개 그리드를 사용 하 여 영역을 그릴 때 CompositionBrush에 적용 되는 스트레치를 제어할 수 있습니다. CompositionBrush는 다음 형식 중 하나일 수 있습니다.

|클래스                                   |세부 정보                                         |에 도입 됨|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |단색으로 영역을 그립니다.                        |Windows 10, 버전 1511 (SDK 10586)|
|[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |[ICompositionSurface](/uwp/api/Windows.UI.Composition.ICompositionSurface) 의 콘텐츠로 영역을 그립니다.|Windows 10, 버전 1511 (SDK 10586)|
|[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |컴퍼지션 효과의 내용으로 영역을 그립니다. |Windows 10, 버전 1511 (SDK 10586)|
|[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |불투명 마스크를 사용 하 여 CompositionBrush를 사용 하 여 시각적 개체를 그립니다. |Windows 10, 버전 1607 (SDK 14393)
|[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |NCompositionBrush Grid stretch를 사용 하 여 영역을 그립니다. |Windows 10, 버전 1607 (SDK 14393)
|[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)|선형 그라데이션으로 영역을 그립니다.                    |Windows 10, 버전 1709 (SDK 16299)
|[CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush)|방사형 그라데이션으로 영역을 그립니다.                    |Windows 10, 버전 1903 (Insider Preview SDK)
|[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |응용 프로그램의 배경 픽셀과 데스크톱의 응용 프로그램 창 바로 뒤의 픽셀을 샘플링 하 여 영역을 그립니다. CompositionEffectBrush와 같은 다른 CompositionBrush에 대 한 입력으로 사용 됩니다. | Windows 10, 버전 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>단색으로 그리기

[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush) 단색으로 영역을 그립니다. System.windows.media.solidcolorbrush> 색을 지정 하는 방법에는 여러 가지가 있습니다. 예를 들어 알파, 빨강, 파랑 및 녹색 (ARGB) 채널을 지정 하거나 [colors](/uwp/api/windows.ui.colors) 클래스에서 제공 하는 미리 정의 된 색 중 하나를 사용할 수 있습니다.

다음 그림 및 코드는 검정 색 브러시로 스트로크 하 고 색 값이 0x9ACD32 인 단색 브러시로 칠하는 사각형을 만드는 작은 시각적 트리를 보여 줍니다.

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

[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush) 는 선형 그라데이션으로 영역을 그립니다. 선형 그라데이션은 선 전체에 걸쳐 그라데이션 축을 혼합 하 여 두 개 이상의 색을 혼합 합니다. GradientStop 개체를 사용 하 여 그라데이션의 색과 위치를 지정 합니다.

다음 그림 및 코드는 빨강 및 노랑 색을 사용 하 여 2를 중지 한 System.windows.media.lineargradientbrush.startpoint로 그린 SpriteVisual을 보여 줍니다.

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

[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 는 ICompositionSurface에 렌더링 된 픽셀을 사용 하 여 영역을 그립니다. 예를 들어 CompositionSurfaceBrush를 사용 하 여 [LoadedImageSurface](/uwp/api/windows.ui.xaml.media.loadedimagesurface) API를 사용 하 여 ICompositionSurface 화면에 이미지를 렌더링 하는 영역을 그릴 수 있습니다.

다음 그림 및 코드는 LoadedImageSurface를 사용 하 여 ICompositionSurface에 렌더링 된 licorice의 비트맵을 사용 하 여 그린 SpriteVisual를 보여 줍니다. CompositionSurfaceBrush의 속성을 사용 하 여 시각적 개체의 범위 내에서 비트맵을 확장 하 고 맞출 수 있습니다.

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

### <a name="paint-with-a-custom-drawing"></a>사용자 지정 그리기로 그리기
[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 를 사용 하 여 [WIN2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (또는 D2D)를 사용 하 여 렌더링 된 ICompositionSurface의 픽셀을 사용 하 여 영역을 그릴 수도 있습니다.

다음 코드는 Win2D를 사용 하 여 ICompositionSurface에 렌더링 된 텍스트 실행으로 그린 SpriteVisual를 보여 줍니다. Win2D를 사용 하려면 [Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) 패키지를 프로젝트에 포함 해야 합니다.

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

마찬가지로 CompositionSurfaceBrush를 사용 하 여 Win2D interop를 사용 하는이 swapchain present로 SpriteVisual를 그릴 수도 있습니다. [이 샘플](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) 에서는 Win2D를 사용 하 여이 swapchain present를 사용 하 여 SpriteVisual를 그리는 방법에 대 한 예제를 제공 합니다.

### <a name="paint-with-a-video"></a>비디오를 사용 하 여 그리기
[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 를 사용 하 여 [MediaPlayer](/uwp/api/Windows.Media.Playback.MediaPlayer) 클래스를 통해 로드 된 비디오를 사용 하 여 렌더링 된 ICompositionSurface의 픽셀을 사용 하 여 영역을 그릴 수도 있습니다.

다음 코드는 ICompositionSurface에 비디오를 로드 하 여 그린 SpriteVisual를 보여 줍니다.

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

### <a name="paint-with-a-filter-effect"></a>필터 효과를 사용 하 여 그리기

[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) 는 CompositionEffect의 출력으로 영역을 그립니다. 시각적 계층의 효과는 색, 그라데이션, 이미지, 비디오, swapchains, UI 영역 또는 시각적 개체 트리와 같은 소스 콘텐츠 컬렉션에 적용 되는 애니메이션 효과 필터 효과로 간주할 수 있습니다. 원본 콘텐츠는 일반적으로 다른 CompositionBrush을 사용 하 여 지정 됩니다.

다음 그림 및 코드는 desaturation 필터 효과를 적용 하는 cat 이미지를 사용 하 여 그린 SpriteVisual를 보여 줍니다.

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

CompositionBrushes를 사용 하 여 효과를 만드는 방법에 대 한 자세한 내용은 [시각적 계층의 효과](./composition-effects.md) 를 참조 하세요.

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>불투명 마스크가 적용 된 CompositionBrush를 사용 하 여 그리기

[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush) 은 불투명도 마스크가 적용 된 CompositionBrush로 영역을 그립니다. 불투명 마스크의 원본은 CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush 또는 CompositionNineGridBrush 형식의 CompositionBrush 수 있습니다. 불투명 마스크는 CompositionSurfaceBrush로 지정 해야 합니다.

다음 그림 및 코드에서는 CompositionMaskBrush로 그린 SpriteVisual를 보여 줍니다. 마스크의 원본은 원 이미지를 마스크로 사용 하 여 원과 같이 표시 되도록 마스크 된 CompositionLinearGradientBrush입니다.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>NCompositionBrush Grid stretch를 사용 하 여 그림판으로 그리기

[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) 는 9 개 그리드 메타포를 사용 하 여 늘어난 CompositionBrush 영역을 그립니다. 9 개 그리드 비유를 사용 하면 CompositionBrush의 가장자리와 모퉁이를 중심으로 다르게 확장할 수 있습니다. 9-그리드 확장의 원본은 CompositionColorBrush, CompositionSurfaceBrush 또는 CompositionEffectBrush 형식의 CompositionBrush 수 있습니다.

다음 코드에서는 CompositionNineGridBrush로 그린 SpriteVisual를 보여 줍니다. 마스크의 소스는 9 개 그리드를 사용 하 여 늘어나는 CompositionSurfaceBrush.

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

### <a name="paint-using-background-pixels"></a>배경 픽셀을 사용 하 여 그리기

[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) 영역 뒤에 콘텐츠가 있는 영역을 그립니다. CompositionBackdropBrush는 자체적으로 사용 되지 않고 대신 EffectBrush와 같은 다른 CompositionBrush의 입력으로 사용 됩니다. 예를 들어, 흐림 효과에 대 한 입력으로 CompositionBackdropBrush를 사용 하 여 frosted 유리 효과를 달성할 수 있습니다.

다음 코드에서는 CompositionSurfaceBrush를 사용 하 여 이미지를 만들고 이미지 위의 frosted 유리 오버레이를 만드는 작은 시각적 트리를 보여 줍니다. Frosted 투명 효과는 이미지 위에 EffectBrush로 채워진 SpriteVisual을 배치 하 여 만듭니다. EffectBrush는 흐리게 효과에 대 한 입력으로 CompositionBackdropBrush를 사용 합니다.

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
많은 CompositionBrushes 다른 CompositionBrushes를 입력으로 사용 합니다. 예를 들어 SetSourceParameter 메서드를 사용 하 여 다른 CompositionBrush을 CompositionEffectBrush에 대 한 입력으로 설정할 수 있습니다. 아래 표에서는 지원 되는 CompositionBrushes 조합을 간략하게 설명 합니다. 지원 되지 않는 조합을 사용 하면 예외가 throw 됩니다.

<table>
<tbody>
<tr>
<th>브러시</th>
<th>EffectBrush 매개 변수 ()</th>
<th>MaskBrush</th>
<th>MaskBrush</th>
<th>N의 Gridbrush 소스</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
</tr>
<tr>
<td>CompositionLinear<br />Gradientbrush가 아니며</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>아니요</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>아니요</td>
<td>아니요</td>
<td>YES</td>
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
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>아니요</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>YES</td>
<td>아니요</td>
<td>아니요</td>
<td>아니요</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>XAML 브러시 및 CompositionBrush 사용

다음 표에서는 응용 프로그램에서 UIElement 또는 SpriteVisual를 그릴 때 XAML 또는 컴퍼지션 브러시 사용이 지정 되었는지 여부를 보여 줍니다. 

> [!NOTE]
> XAML UIElement에 대해 CompositionBrush을 제안 하는 경우 CompositionBrush가 XamlCompositionBrushBase를 사용 하 여 패키지 되는 것으로 간주 됩니다.

|시나리오                                                                   | XAML UIElement                                                                                                |컴퍼지션 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|단색으로 영역 그리기                                             |[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|애니메이션 된 색으로 영역 그리기                                          |[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|정적 그라데이션으로 영역 그리기                                       |[LinearGradientBrush](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)                            |[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|애니메이션 그라데이션 중지점으로 영역 그리기                                 |[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|이미지로 영역 그리기                                                |[ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)                                     |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|웹 페이지를 사용 하 여 영역 그리기                                               |[WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)                                   |해당 없음
|Narea Grid stretch를 사용 하 여 이미지로 영역 그리기                         |[이미지 컨트롤](/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|애니메이션 된 N이상 그리드 스트레치로 영역 그리기                               |[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|이 swapchain present를 사용 하 여 영역 그리기                                             |[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) w/이 swapchain present interop
|비디오를 사용 하 여 영역 그리기                                                 |[MediaElement](../design/controls-and-patterns/media-playback.md)                                                                                                  |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) w/미디어 interop
|사용자 지정 2D drawing으로 영역 그리기                                       |Win2D의 [CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) w/Win2D interop
|애니메이션이 적용 되지 않은 마스크로 영역 그리기                                       |XAML [셰이프](../design/controls-and-patterns/shapes.md) 를 사용 하 여 마스크 정의   |[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|애니메이션 마스크를 사용 하 여 영역 그리기                                        |[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|애니메이션 적용 필터 효과를 사용 하 여 영역 그리기                               |[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|배경 픽셀에 효과를 적용 하 여 영역 그리기        |[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>관련 항목

[BeginDraw 및 EndDraw를 사용 하 여 네이티브 DirectX 및 Direct2D interop 컴퍼지션](composition-native-interop.md)

[XamlCompositionBrushBase를 사용 하는 XAML 브러시 interop](../design/style/brushes.md#xamlcompositionbrushbase)