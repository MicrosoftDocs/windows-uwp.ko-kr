---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: 컴퍼지션 효과
description: 효과 API를 통해 개발자가 UI를 렌더링하는 방식을 사용자 지정할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b07ab7fa6b65e16f39d9e2a77a677d33d3c70254
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360506"
---
# <a name="composition-effects"></a>컴퍼지션 효과

[  **Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) API를 사용하면 애니메이션 효과를 줄 수 있는 효과 속성을 사용하여 이미지 및 UI에 실시간 효과를 적용할 수 있습니다. 이 개요에서는 컴퍼지션 시각적 개체에 효과를 적용할 수 있는 전체 기능을 실행할 것입니다.

응용 프로그램에서 효과를 설명하는 개발자를 위해 [UWP(유니버설 Windows 플랫폼)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp)를 일관적으로 지원하기 위해, 컴퍼지션 효과에서는 Win2D의 IGraphicsEffect 인터페이스를 활용하여 [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) 네임스페이스를 통해 효과 설명을 사용합니다.

브러시 효과는 기존 이미지에 효과를 적용하여 응용 프로그램 영역을 그리는 데 사용됩니다. Windows 10 컴퍼지션 효과 API는 스트라이프 시각적 개체에 중점을 둡니다. SpriteVisual은 색, 이미지, 효과 생성에서 상호 작용하며 유연성을 제공합니다. SpriteVisual는 브러시를 사용하여 2D 사각형을 채울 수 있는 컴퍼지션 시각적 형식입니다. 시각적 개체는 사각형의 경계를 정의하고 브러시는 사각형을 그리는 데 사용되는 픽셀을 정의합니다.

효과 브러시는 효과 그래프 출력에서 내용을 가져오는 컴퍼지션 트리 시각적 개체에 사용됩니다. 효과는 기존 표면/텍스처를 참조할 수 있지만 다른 컴퍼지션 트리의 출력은 참조할 수 없습니다.

[  **XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)와 효과 브러시를 사용하여 XAML UIElements에도 효과를 적용할 수 있습니다.

## <a name="effect-features"></a>효과 기능

- [효과 라이브러리](./composition-effects.md#effect-library)
- [효과 체인](./composition-effects.md#chaining-effects)
- [애니메이션 지원](./composition-effects.md#animation-support)
- [상수 및 애니메이션된 적용 속성](./composition-effects.md#constant-vs-animated-effect-properties)
- [독립적인 속성을 사용 하 여 여러 효과 인스턴스](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>효과 라이브러리

현재 컴퍼지션은 다음과 같은 효과를 지원합니다.

| 영향               | 설명                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2D 아핀 변형  | 2D 아핀 변형 매트릭스를 이미지에 적용합니다. 효과 [샘플](https://go.microsoft.com/fwlink/?LinkId=785341)에서 이 효과를 사용하여 알파 마스크에 애니메이션 효과를 주었습니다.       |
| 산술 합성 | 유연한 수식을 사용하여 두 개의 이미지를 결합합니다. [샘플](https://go.microsoft.com/fwlink/?LinkId=785341)에서 산술 합성을 사용하여 크로스페이드 효과를 만들었습니다. |
| 혼합 효과         | 두 개의 이미지를 결합하는 혼합 효과를 만듭니다. 컴퍼지션은 Win2D에서 지원되는 26개의 [혼합 모드](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm) 중에서 21개를 제공합니다.        |
| 색 소싱         | 단색을 포함하는 이미지를 생성합니다.                                                                                                                                                                               |
| 합성            | 두 개의 이미지를 결합합니다. 컴퍼지션은 Win2D에서 지원되는 13개의 [합성 모드](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm)를 모두 제공합니다.                                              |
| 이 예와             | 이미지의 대비를 늘리거나 줄입니다.                                                                                                                                                                           |
| Exposure             | 이미지의 노출을 늘리거나 줄입니다.                                                                                                                                                                           |
| 회색조            | 이미지를 단색형 회색으로 변환합니다.                                                                                                                                                                                   |
| 감마 전달       | 채널당 감마 전달 함수를 적용하여 이미지의 색을 변경합니다.                                                                                                                                           |
| 색상 회전           | 색상 값을 회전하여 이미지의 색을 변경합니다.                                                                                                                                                                   |
| 반전               | 이미지의 색을 반전합니다.                                                                                                                                                                                            |
| 채도             | 이미지의 채도를 변경합니다.                                                                                                                                                                                         |
| 세피아                | 이미지를 세피아 톤으로 변환합니다.                                                                                                                                                                                          |
| 온도 및 색조 | 이미지의 온도 및/또는 색조를 조정합니다.                                                                                                                                                                           |

자세한 내용은 Win2D의 [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) 네임스페이스를 참조하세요. 컴퍼지션에서 지원 되지 않습니다 효과 이라고 명시 \[NoComposition\]합니다.

### <a name="chaining-effects"></a>효과 연결

효과를 연결하여 응용 프로그램이 이미지에서 여러 개의 효과를 동시에 사용하도록 할 수 있습니다. 효과 그래프에서는 여러 개의 효과를 지원하며 효과를 하나씩 차례로 참조할 수 있습니다. 효과를 설명할 때 효과에 대한 입력으로 효과를 추가하면 됩니다.

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0
}
```

위 예제에서는 두 개의 입력이 있는 산술 합성 효과에 대해 설명합니다. 두 번째 입력은 .5 채도 속성을 통해 채도 효과가 있습니다.

### <a name="animation-support"></a>애니메이션 지원

효과 속성은 애니메이션을 지원하며, 효과를 컴파일하는 동안 애니메이션 효과를 줄 수 있는 효과 속성과 상수로 "적용"할 수 있는 효과 속성을 지정할 수 있습니다. 애니메이션 효과를 줄 수 있는 속성은 "효과 이름.속성 이름" 형식의 문자열을 통해 지정됩니다. 이러한 속성은 효과의 여러 인스턴스화에서 독립적으로 애니메이션 효과를 줄 수 있습니다.

### <a name="constant-vs-animated-effect-properties"></a>상수 및 애니메이션 효과 속성

효과를 컴파일하는 동안 효과 속성을 동적으로 지정하거나, 상수로 "지정된" 속성으로 지정할 수 있습니다. 동적 속성은 “<effect name>.<property name>” 형식의 문자열을 통해 지정됩니다. 동적 속성을 특정 값으로 설정하거나, 컴퍼지션 애니메이션 시스템을 통해 애니메이션 효과를 줄 수 있습니다.

위의 효과 설명을 컴파일할 때 채도를 0.5로 지정할 수도 있고, 동적으로 만들어 동적으로 설정하거나 애니메이션 효과를 줄 수 있습니다.

지정된 채도로 효과 컴파일:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

동적 채도를 사용하여 효과 컴파일:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

위 효과의 채도 속성을 정적 값으로 설정하거나, 식 또는 ScalarKeyFrame 애니메이션을 사용하여 애니메이션 효과를 줄 수 있습니다.

아래와 같이 효과의 채도 속성에 애니메이션 효과를 줄 ScalarKeyFrame을 만들 수 있습니다.

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

아래와 같이 효과의 채도 속성에서 애니메이션을 시작합니다.

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

키 프레임으로 애니메이션 효과를 준 효과 속성에 대해서는 [채도 감소 - 애니메이션 샘플](https://go.microsoft.com/fwlink/?LinkId=785342)을 참조하고, 효과 및 식 사용에 대해서는 [AlphaMask 샘플](https://go.microsoft.com/fwlink/?LinkId=785343)을 참조하세요.

### <a name="multiple-effect-instances-with-independent-properties"></a>독립 속성이 있는 여러 효과 인스턴스

효과를 컴파일하는 동안 매개 변수가 동적이어야 한다고 지정하면 해당 매개 변수를 효과 인스턴스에서 개별적으로 변경할 수 있습니다. 그러면 두 시각적 개체가 동일한 효과를 사용하지만 다른 효과 속성으로 렌더링됩니다. 자세한 내용은 색 소싱 및 혼합 [샘플](https://go.microsoft.com/fwlink/?LinkId=785344)을 참조하세요.

## <a name="getting-started-with-composition-effects"></a>컴퍼지션 효과 시작하기

이 빠른 시작 자습서는 효과의 몇 가지 기본적인 기능의 사용 방법을 보여줍니다.

- [Visual Studio 설치](./composition-effects.md#installing-visual-studio)
- [새 프로젝트 만들기](./composition-effects.md#creating-a-new-project)
- [Win2D 설치](./composition-effects.md#installing-win2d)
- [컴퍼지션 사항을 설정합니다.](./composition-effects.md#setting-your-composition-basics)
- [CompositionSurface 브러시 만들기](./composition-effects.md#creating-a-compositionsurface-brush)
- [만들기, 컴파일 및 효과 적용](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>Visual Studio 설치

- 지원되는 버전의 Visual Studio가 설치되어 있지 않으면 Visual Studio 다운로드 페이지([여기](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx))로 이동하세요.

### <a name="creating-a-new-project"></a>새 프로젝트 만들기

- 파일-&gt;새로 만들기-&gt;프로젝트...로 이동합니다.
- 'Visual C#'을 선택합니다.
- '빈 앱(Windows 유니버설)' (Visual Studio 2015)를 만듭니다.
- 선택한 프로젝트 이름을 입력합니다.
- '확인'을 클릭합니다.

### <a name="installing-win2d"></a>Win2D 설치

Win2D는 Nuget.org 패키지로 출시되며 Win2D가 설치되어야 효과를 사용할 수 있습니다.

두 가지 버전의 패키지가 있으며 각각 Windows 10용과 Windows 8.1용으로 사용됩니다. 컴퍼지션 효과에는 Windows 10 버전을 사용합니다.

- 도구 → NuGet 패키지 관리자 → 솔루션용 NuGet 패키지 관리로 이동하여 NuGet 패키지 관리자를 시작합니다.
- "Win2D"를 검색하고 대상 버전의 Windows에 적절한 패키지를 선택합니다. Windows.UI. Composition은 Windows 10(8.1이 아님)을 지원하기 때문에 Win2D.uwp를 선택합니다.
- 사용권 계약에 동의합니다.
- '닫기'를 클릭합니다.

다음 몇 가지 단계에서는 컴퍼지션 API를 사용하여 이 고양이 이미지에 채도 효과를 적용하여 모든 채도를 제거합니다. 이 모델에서는 효과가 생성된 다음 이미지에 적용됩니다.

![원본 이미지](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>컴퍼지션 기본 사항 설정

루트 ContainerVisual, Windows.UI.Composition 작성자를 설정하여 핵심 창에 연결하는 방법의 예를 보려면 GitHub의 [컴퍼지션 시각적 트리 샘플](https://go.microsoft.com/fwlink/?LinkId=785345)을 참조하세요.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### <a name="creating-a-compositionsurface-brush"></a>CompositionSurface 브러시 만들기

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush);
```

### <a name="creating-compiling-and-applying-effects"></a>효과 만들기, 컴파일 및 적용

1. 그래픽 효과를 만듭니다.

    ```cs
    var graphicsEffect = new SaturationEffect
    {
      Saturation = 0.0f,
      Source = new CompositionEffectSourceParameter("mySource")
    };
    ```

1. 효과를 컴파일하고 효과 브러시를 만듭니다.

    ```cs
    var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

    var catEffect = effectFactory.CreateBrush();
    catEffect.SetSourceParameter("mySource", surfaceBrush);
    ```

1. 컴퍼지션 트리에서 SpriteVisual을 만들고 효과를 적용합니다.

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. 로드할 이미지 소스를 만듭니다.

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. SpriteVisual에서 면의 크기를 지정하고 브러시합니다.

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. 앱을 실행합니다. 결과로 나타나는 고양이 이미지는 흐릿해야 합니다.

![흐릿한 이미지](images/composition-cat-desaturated.png)

## <a name="more-information"></a>추가 정보

- [Microsoft-컴퍼지션 GitHub](https://github.com/Microsoft/composition)
- [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
- [Twitter의 Windows 컴퍼지션 팀](https://twitter.com/wincomposition)
- [컴퍼지션 개요](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [시각적 트리 기본 사항](composition-visual-tree.md)
- [컴퍼지션 브러시](composition-brushes.md)
- [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [애니메이션 개요](composition-animation.md)
- [컴퍼지션 네이티브 DirectX 및 Direct2D와의 상호 운용 BeginDraw 및 EndDraw](composition-native-interop.md)
