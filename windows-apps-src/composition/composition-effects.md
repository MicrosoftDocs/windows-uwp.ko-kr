---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: 컴퍼지션 효과
description: 개발자는 효과 Api를 사용 하 여 UI가 렌더링 되는 방식을 사용자 지정할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c25bdaa5a0639e35b10a50deb0aa441a01b874d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175597"
---
# <a name="composition-effects"></a>컴퍼지션 효과

애니메이션 효과 [**api를 사용 하면 이미지**](/uwp/api/Windows.UI.Composition) 와 ui에 실시간 효과를 적용 하 여 효과 속성을 수 있습니다. 이 개요에서는 효과를 컴퍼지션 시각적 개체에 적용 하는 데 사용할 수 있는 기능을 실행 합니다.

응용 프로그램의 효과를 설명 하는 개발자에 게 [UWP (유니버설 Windows 플랫폼](../get-started/universal-application-platform-guide.md) ) 일관성을 지원 하기 위해 컴퍼지션 [효과는 Win2D's](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) IGraphicsEffect 인터페이스를 활용 하 여 효과 설명을 사용 합니다.

브러시 효과는 기존 이미지 집합에 효과를 적용 하 여 응용 프로그램의 영역을 그리는 데 사용 됩니다. Windows 10 컴퍼지션 효과 Api는 스프라이트 시각적 개체에 초점을 맞추고 있습니다. SpriteVisual은 색, 이미지 및 효과를 만들 때 유연 하 고 상호 작용 수 있습니다. SpriteVisual는 2D 사각형을 브러시로 채울 수 있는 컴퍼지션 시각적 유형입니다. 시각적 개체는 사각형의 경계를 정의 하 고 브러시는 사각형을 그리는 데 사용 되는 픽셀을 정의 합니다.

효과 브러시는 효과 그래프의 출력에서 가져온 내용이 있는 컴퍼지션 트리 시각적 개체에 사용 됩니다. 효과는 기존 표면/질감을 참조할 수 있지만 다른 컴퍼지션 트리의 출력은 참조할 수 없습니다.

[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)를 사용 하 여 효과 브러시를 사용 하 여 XAML UIElements 적용 될 수도 있습니다.

## <a name="effect-features"></a>효과 기능

- [효과 라이브러리](./composition-effects.md#effect-library)
- [연결 효과](./composition-effects.md#chaining-effects)
- [애니메이션 지원](./composition-effects.md#animation-support)
- [상수와 애니메이션 효과 속성 비교](./composition-effects.md#constant-vs-animated-effect-properties)
- [독립적인 속성이 있는 여러 효과 인스턴스](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>효과 라이브러리

현재 컴퍼지션은 다음과 같은 효과를 지원 합니다.

| 효과               | 설명                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2D 관계 변형  | 이미지에 2D 유사 변형 행렬을 적용 합니다. 효과 [샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects)에서 알파 마스크에 애니메이션 효과를 주는 데이 효과를 사용 했습니다.       |
| 산술 복합 | 유연한 수식을 사용 하 여 두 이미지를 결합 합니다. [샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects)에서 교차 페이드 효과를 만들기 위해 산술 복합을 사용 했습니다. |
| Blend 효과         | 두 이미지를 결합 하는 blend 효과를 만듭니다. 컴퍼지션은 Win2D에서 지원 되는 26 [blend 모드](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm) 의 21 개를 제공 합니다.        |
| 색 원본         | 단색을 포함 하는 이미지를 생성 합니다.                                                                                                                                                                               |
| 복합            | 두 이미지를 결합 합니다. 컴퍼지션은 Win2D에서 지원 되는 13 개의 [복합 모드](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm) 를 모두 제공 합니다.                                              |
| 이 예와             | 이미지의 대비를 늘리거나 줄입니다.                                                                                                                                                                           |
| 노출을             | 이미지의 노출을 늘리거나 줄입니다.                                                                                                                                                                           |
| 회색조            | 이미지를 단색 회색으로 변환 합니다.                                                                                                                                                                                   |
| 감마 전송       | 채널 별 감마 전송 함수를 적용 하 여 이미지 색을 변경 합니다.                                                                                                                                           |
| 색상 회전           | 색상 값을 회전 하 여 이미지의 색을 변경 합니다.                                                                                                                                                                   |
| [               | 이미지 색을 반전 합니다.                                                                                                                                                                                            |
| Saturate             | 이미지의 채도를 변경 합니다.                                                                                                                                                                                         |
| 톤                | 이미지를 세피아 톤으로 변환 합니다.                                                                                                                                                                                          |
| 온도 및 색조 | 이미지의 온도 및/또는 색조를 조정 합니다.                                                                                                                                                                           |

자세한 내용은 [Win2D's 네임](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) 스페이스를 참조 하세요. 컴퍼지션에서 지원 되지 않는 효과는 nocomposition으로 명시 됩니다 \[ \] .

### <a name="chaining-effects"></a>연결 효과

응용 프로그램이 이미지에 여러 효과를 동시에 사용할 수 있도록 효과를 연결할 수 있습니다. 효과 그래프는 하나를 참조할 수 있는 여러 효과를 지원할 수 있습니다. 효과를 설명할 때 효과에 대 한 입력으로 효과를 추가 하기만 하면 됩니다.

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

위의 예제에서는 두 개의 입력이 있는 산술 복합 효과를 설명 합니다. 두 번째 입력에는. 5 채도 속성의 포화 효과가 있습니다.

### <a name="animation-support"></a>애니메이션 지원

효과 속성이 애니메이션을 지원 합니다. 효과 컴파일을 수행 하는 동안 효과 속성에 애니메이션을 적용할 수 있으며 상수로 "구운" 할 수 있는 효과를 지정할 수 있습니다. 애니메이션 효과 속성은 "effect name. 속성 이름" 형식의 문자열을 통해 지정 됩니다. 이러한 속성은 효과의 여러 인스턴스화에 대해 독립적으로 애니메이션을 적용할 수 있습니다.

### <a name="constant-vs-animated-effect-properties"></a>상수 및 애니메이션 효과 속성

효과 컴파일을 수행 하는 동안에는 효과 속성을 동적으로 지정 하거나 "구운 in" 속성으로 지정할 수 있습니다. 동적 속성은 "." 형식의 문자열을 통해 지정 <effect name> 됩니다 <property name> . 동적 속성은 특정 값으로 설정 하거나 컴퍼지션 애니메이션 시스템을 사용 하 여 애니메이션을 적용할 수 있습니다.

위의 효과 설명을 컴파일하는 경우 굽는 채도의 유연성이 0.5 또는 동적으로 설정 하 여 동적으로 설정 하거나 애니메이션을 적용할 수 있습니다.

다음에서 채도 구운를 사용 하 여 효과 컴파일:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

동적 채도를 사용 하 여 효과 컴파일:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

그런 다음 위의 효과의 채도 속성을 정적 값으로 설정 하거나 Expression 또는 ScalarKeyFrame 애니메이션 중 하나를 사용 하 여 애니메이션을 적용할 수 있습니다.

다음과 같이 효과의 채도 속성에 애니메이션 효과를 주는 데 사용 되는 ScalarKeyFrame를 만들 수 있습니다.

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

다음과 같이 효과의 채도 속성에서 애니메이션을 시작 합니다.

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

효과 및 식 사용에 대 한 [AlphaMask 샘플과](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/AlphaMask) 키 프레임을 사용 하 여 애니메이션 효과를 주는 효과 속성은 [Desaturation-애니메이션 샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/Desaturation - Animation) 을 참조 하세요.

### <a name="multiple-effect-instances-with-independent-properties"></a>독립적인 속성이 있는 여러 효과 인스턴스

효과를 컴파일하는 동안 매개 변수를 동적으로 지정 하 여 효과를 적용 하는 인스턴스를 기준으로 매개 변수를 변경할 수 있습니다. 이렇게 하면 두 시각적 개체가 동일한 효과를 사용 하지만 다른 효과 속성으로 렌더링 될 수 있습니다. 자세한 내용은 ColorSource 및 Blend [샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/ColorSource and Blend) 을 참조 하세요.

## <a name="getting-started-with-composition-effects"></a>컴퍼지션 효과 시작

이 빠른 시작 자습서에서는 효과의 기본 기능 중 일부를 활용 하는 방법을 보여 줍니다.

- [Visual Studio 설치](./composition-effects.md#installing-visual-studio)
- [새 프로젝트 만들기](./composition-effects.md#creating-a-new-project)
- [Win2D 설치](./composition-effects.md#installing-win2d)
- [컴퍼지션 기본 설정](./composition-effects.md#setting-your-composition-basics)
- [CompositionSurface 브러시 만들기](./composition-effects.md#creating-a-compositionsurface-brush)
- [효과 생성, 컴파일 및 적용](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>Visual Studio 설치

- 지원 되는 버전의 Visual Studio가 설치 되어 있지 않은 경우 Visual Studio [다운로드 페이지로](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs)이동 합니다.

### <a name="creating-a-new-project"></a>새 프로젝트 만들기

- 파일->새로 만들기->프로젝트 ...로 이동 합니다.
- ' Visual c # ' 선택
- ' 빈 앱 (Windows 유니버설) ' 만들기 (Visual Studio 2015)
- 선택한 프로젝트 이름 입력
- ' 확인 '을 클릭 합니다.

### <a name="installing-win2d"></a>Win2D 설치

Win2D는 Nuget.org 패키지로 릴리스 되었으며 효과를 사용 하려면 먼저 설치 해야 합니다.

두 가지 패키지 버전, 즉 Windows 10 용 패키지 버전과 Windows 8.1 용 패키지가 있습니다. 컴퍼지션 효과의 경우 Windows 10 버전을 사용 합니다.

- 도구 → NuGet 패키지 관리자 → 솔루션에 대 한 NuGet 패키지 관리로 이동 하 여 NuGet 패키지 관리자를 시작 합니다.
- "Win2D"를 검색 하 고 대상 Windows 버전에 적절 한 패키지를 선택 합니다. Windows. UI입니다. 컴퍼지션은 Windows 10 (8.1 아님)을 지원 하 고 Win2D를 선택 합니다.
- 사용권 계약에 동의 합니다.
- ' 닫기 '를 클릭 합니다.

다음 몇 단계에서는 컴퍼지션 API를 사용 하 여이 cat 이미지에 채도 효과를 적용 하 여 모든 채도를 제거 합니다. 이 모델에서는 효과가 생성 된 다음 이미지에 적용 됩니다.

![원본 이미지](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>컴퍼지션 기본 설정

Compositor, root System.windows.media.containervisual>를 설정 하 고 코어 창에 연결 하는 방법에 대 한 예제는 GitHub의 [컴퍼지션 시각적 트리 샘플](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/CompositionImageSample) 을 참조 하세요.

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

### <a name="creating-compiling-and-applying-effects"></a>효과 생성, 컴파일 및 적용

1. 그래픽 효과 만들기

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

1. 컴퍼지션 트리에서 SpriteVisual을 만들고 효과를 적용 합니다.

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. 로드할 이미지 원본을 만듭니다.

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. SpriteVisual 화면 크기 및 브러시

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. 앱 실행 – 결과는 낮춘 고양이 여야 합니다.

![낮춘 이미지](images/composition-cat-desaturated.png)

## <a name="more-information"></a>추가 정보

- [Microsoft-컴퍼지션 GitHub](https://github.com/microsoft/WindowsCompositionSamples)
- [**Windows. UI. 컴퍼지션**](/uwp/api/Windows.UI.Composition)
- [Twitter의 Windows 컴퍼지션 팀](https://twitter.com/wincomposition)
- [컴퍼지션 개요](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [시각적 트리 기본 사항](composition-visual-tree.md)
- [컴퍼지션 브러시](composition-brushes.md)
- [XamlCompositionBrushBase](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [애니메이션 개요](composition-animation.md)
- [BeginDraw 및 EndDraw를 사용한 컴퍼지션 네이티브 DirectX 및 Direct2D 상호 운용성](composition-native-interop.md)