---
author: daneuber
title: 컴퍼지션 그림자
description: 그림자 Api UI 콘텐츠를 사용자 지정 가능한 그림자를 추가할 수 있습니다.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84e12d6c3e25a18902aaa55011949dd5b5ff97ca
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4119243"
---
# <a name="shadows-in-windows-ui"></a>Windows UI의 그림자

[DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) 클래스 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 또는 [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (하위 트리 시각적 개체)에 적용할 수 있는 구성 가능한 그림자를 만드는 방법을 제공 합니다. 시각적 계층의 개체에 대 한 일반적인 그대로 CompositionAnimations를 사용 하 여 DropShadow의 모든 속성 애니메이션 수 있습니다.

## <a name="basic-drop-shadow"></a>기본 그림자

기본 그림자를 만들려면 새 DropShadow 만들기 하 고 사용자 시각 연결 합니다. 그림자는 기본적으로 사각형입니다. 표준 속성 집합에 그림자의 모양과 느낌을 들을 사용할 수 있습니다.

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

![기본 DropShadow로 사각형 시각적](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>그림자 모양

DropShadow에 대 한 모양을 정의 하는 몇 가지가 있습니다.

- **기본값 사용** -DropShadow 셰이프 기본적으로 CompositionDropShadowSourcePolicy에서 '기본' 모드에 의해 정의 됩니다. SpriteVisual, 기본 않는 사각형 마스크 제공 됩니다. LayerVisual, 기본값은 상속 알파 시각적 개체의 브러시를 사용 하 여 마스크입니다.
- **마스크 설정** – 그림자 불투명 마스크를 정의 하려면 [마스크](/uwp/api/windows.ui.composition.dropshadow.mask) 속성을 설정할 수 있습니다.
- **상속 된 마스크를 사용 하 여 지정** – [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)를 사용 하 여 [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) 속성을 설정 합니다. InheritFromVisualContent 시각적 개체의 브러시의 알파에서 생성 된 마스크를 사용 합니다.

## <a name="masking-to-match-your-content"></a>콘텐츠에 맞게 마스킹

시각적 개체의 내용과 일치 하 여 그림자를 원하는 경우 그림자 마스크 속성에 대 한 시각적 개체의 브러시를 사용 하거나 콘텐츠에서 마스크를 자동으로 상속 그림자를 설정 합니다. LayerVisual를 사용 하 여, 그림자 기본적으로 마스크를 상속 합니다.

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

![마스크 그림자를 사용 하 여 연결 된 웹 이미지](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>대체 마스크를 사용 하 여

경우에 따라 시각적 개체의 콘텐츠를 일치 하지 않을 수 있도록 그림자를 구체화 하는 것이 좋습니다. 이 효과 얻기 위해 알파를 사용 하 여 브러시를 사용 하 여 마스크 속성을 명시적으로 설정 해야 합니다.

에 아래 예에서는 시각적 콘텐츠 및 그림자 마스크에 대해 하나씩 두 표면을 로드 합니다.

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

![원으로 연결 된 웹 이미지 마스킹 그림자](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>애니메이션 효과 주기

시각적 계층의 표준 그대로 컴퍼지션 애니메이션을 사용 하 여 DropShadow 속성 애니메이션 수 있습니다. 아래 뿌리기 위 샘플에서 흐림 반경 그림자에 애니메이션 효과를 주는 코드를 수정 했습니다.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>XAML에서 그림자

그림자 더 복잡 한 프레임 워크 요소를 추가 하려는 경우에 XAML과 컴퍼지션 간의 그림자를 사용 하 여 상호 운용성 몇 가지가 있습니다.

1. 사용 하 여 [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) Windows 커뮤니티 도구 키트에서 사용할 수 있습니다. 사용 하는 방법에 대 한 자세한 내용은 [DropShadowPanel 설명서](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) 를 참조 하세요.
1. 그림자 호스트로 사용 하 여 &는 XAML 핸드 아웃 시각적 개체에 연결 하는 시각적 개체를 만듭니다.
1. 컴퍼지션 샘플 갤러리 [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) 사용자 지정 CompositionShadow 컨트롤을 사용 합니다. 다음 사용 예제를 참조 하세요.

## <a name="performance"></a>성능

시각적 계층에 여러 최적화 효율적이 고 사용할 수 있는 효과에 그림자 생성 어떤 옵션을 설정에 따라 상대적으로 비용이 많이 드는 작업 될 수 있습니다. 다음은 다양 한 유형의 그림자에 대 한 높은 수준 '비용'입니다. 참고는 특정 그림자 비용이 많이 드는 수도 있지만 경우에 따라서는 여전히 특정 시나리오에서 자주 사용 하 여 적절할 수 있습니다.

그림자 특성| 비용
------------- | -------------
사각형    | 낮음
Shadow.Mask      | 높음
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 높음
정적 흐림 반경 | 낮음
Radius 흐림 애니메이션 효과 주기 | 높음

## <a name="additional-resources"></a>추가 리소스

- [컴퍼지션 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 리포지토리](https://github.com/Microsoft/WindowsUIDevLabs)