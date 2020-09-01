---
title: 컴퍼지션 그림자
description: 섀도 Api를 사용 하면 UI 콘텐츠에 동적으로 사용자 지정 가능한 그림자를 추가할 수 있습니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166427"
---
# <a name="shadows-in-windows-ui"></a>Windows UI의 그림자

[Dropshadow](/uwp/api/Windows.UI.Composition.DropShadow) 클래스는 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 또는 [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (시각적 개체의 하위 트리)에 적용할 수 있는 구성 가능한 그림자를 만드는 방법을 제공 합니다. 시각적 계층의 개체에 대 한 일반적인 경우와 마찬가지로 CompositionAnimations를 사용 하 여 DropShadow의 모든 속성에 애니메이션 효과를 적용할 수 있습니다.

## <a name="basic-drop-shadow"></a>기본 그림자

기본 그림자를 만들려면 새 DropShadow를 만들어 시각적 개체에 연결 하기만 하면 됩니다. 그림자는 기본적으로 사각형입니다. 표준 속성 집합을 사용 하 여 그림자의 모양과 느낌을 조정할 수 있습니다.

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![기본 DropShadow를 사용 하는 사각형 시각적 개체](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>그림자 모양 지정

DropShadow 셰이프를 정의 하는 몇 가지 방법이 있습니다.

- **기본값 사용** -기본적으로 dropshadow 셰이프는 CompositionDropShadowSourcePolicy의 ' 기본 ' 모드로 정의 됩니다. SpriteVisual의 경우 마스크가 제공 되지 않으면 기본값은 사각형입니다. LayerVisual의 경우 기본값은 시각적 개체 브러시의 알파를 사용 하 여 마스크를 상속 하는 것입니다.
- **마스크 설정** – [마스크](/uwp/api/windows.ui.composition.dropshadow.mask) 속성을 설정 하 여 그림자에 대 한 불투명 마스크를 정의할 수 있습니다.
- **상속 된 마스크를 사용 하도록 지정** – [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)를 사용 하도록 [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) 속성을 설정 합니다. InheritFromVisualContent는 시각적 개체의 브러시 알파에서 생성 된 마스크를 사용 합니다.

## <a name="masking-to-match-your-content"></a>콘텐츠와 일치 하도록 마스킹

그림자를 시각적 개체의 콘텐츠와 일치 시키려는 경우에는 그림자 마스크 속성에 대해 시각적 개체의 브러시를 사용 하거나 콘텐츠에서 마스크를 자동으로 상속 하도록 그림자를 설정할 수 있습니다. LayerVisual를 사용 하는 경우 섀도는 기본적으로 마스크를 상속 합니다.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![마스킹된 그림자를 사용 하는 연결 된 웹 이미지](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>대체 마스크 사용

경우에 따라 시각적 개체의 콘텐츠와 일치 하지 않도록 그림자 모양을 지정할 수 있습니다. 이러한 효과를 얻으려면 알파를 사용 하는 브러시를 사용 하 여 Mask 속성을 명시적으로 설정 해야 합니다.

아래 예제에서는 시각적 콘텐츠에 대 한 화면과 그림자 마스크에 대 한 두 개의 표면을 로드 합니다.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![원 마스킹된 그림자가 있는 연결 된 웹 이미지](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>주기

시각적 계층에서 표준 그대로 사용 하 여 DropShadow 속성은 컴퍼지션 애니메이션을 사용 하 여 애니메이션을 적용할 수 있습니다. 아래에서는 위의 sprinkles 샘플에서 코드를 수정 하 여 그림자에 대 한 흐리게 반경을 애니메이션 합니다.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>XAML의 그림자

더 복잡 한 프레임 워크 요소에 그림자를 추가 하려는 경우에는 XAML과 컴퍼지션 사이에 그림자를 사용 하는 몇 가지 방법이 있습니다.

1. Windows 커뮤니티 도구 키트에서 제공 되는 [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) 을 사용 합니다. 사용 방법에 대 한 자세한 내용은 [DropShadowPanel 설명서](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) 를 참조 하세요.
1. XAML 유인물 시각적 개체에 연결 & 그림자 호스트로 사용할 시각적 개체를 만듭니다.
1. 컴퍼지션 샘플 갤러리의 [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) custom CompositionShadow 컨트롤을 사용 합니다. 사용법은 여기에서 예제를 참조 하세요.

## <a name="performance"></a>성능

시각적 계층에는 효과를 효율적으로 사용할 수 있도록 최적화 된 많은 최적화가 있지만, 그림자 생성은 설정 하는 옵션에 따라 상대적으로 비용이 많이 드는 작업이 될 수 있습니다. 다음은 다양 한 유형의 섀도에 대 한 높은 수준의 ' 비용 '입니다. 특정 그림자는 비용이 많이 들 수 있지만 특정 시나리오에서 사용 하는 것이 적합할 수도 있습니다.

그림자 특징| 비용
------------- | -------------
사각형    | 낮음
그림자. Mask      | 높음
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 높음
정적 흐림 반경 | 낮음
흐림 효과 반경 애니메이션 | 높음

## <a name="additional-resources"></a>추가 리소스

- [컴퍼지션 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 리포지토리](https://github.com/microsoft/WindowsCompositionSamples)