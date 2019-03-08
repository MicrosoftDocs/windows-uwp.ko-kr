---
title: 컴퍼지션 그림자
description: 그림자 Api를 통해 UI 콘텐츠를 사용자 지정할 수 있는 동적 그림자를 추가할 수 있습니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630838"
---
# <a name="shadows-in-windows-ui"></a>Windows UI에 그림자

합니다 [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) 클래스에 적용할 수 있는 구성 가능한 그림자를 만드는 방법을 제공을 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 하거나 [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (시각적 개체의 하위 트리). 시각적 계층에서 개체에 대 한 일반적인 경우는 DropShadow의 모든 속성 애니메이션을 적용할 수 CompositionAnimations를 사용 하 여 합니다.

## <a name="basic-drop-shadow"></a>기본 그림자

기본 섀도 만들려면 간단히 새 DropShadow 만들고 시각적 개체에 연결 합니다. 그림자는 기본적으로 사각형입니다. 표준 속성 집합을 조정 하 여 그림자의 모양과 느낌에 사용할 수 있습니다.

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

![기본 DropShadow 함께 사각형 시각적 개체](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>그림자를 형성합니다.

DropShadow 프로그램에 대 한 모양을 정의 하는 방법은 몇 가지가 있습니다.

- **기본값을 사용 하 여** -기본적으로 DropShadow 셰이프 CompositionDropShadowSourcePolicy에 '기본' 모드에 의해 정의 됩니다. SpriteVisual, 기본값은 사각형 마스크는 제공 되지 않는 경우. LayerVisual에 대 한 기본 시각적 개체의 브러시의 알파를 사용 하는 마스크를 상속 하는 것입니다.
- **마스크를 설정** – 설정할 수 있습니다 합니다 [마스크](/uwp/api/windows.ui.composition.dropshadow.mask) 그림자에 대 한 불투명 마스크를 정의 하는 속성입니다.
- **상속 된 항목 마스크를 사용 하도록 지정** – 설정 된 [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) 속성을 사용 하 여 [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)합니다. InheritFromVisualContent 시각적 개체의 브러시의 알파에서 생성 된 마스크를 사용 합니다.

## <a name="masking-to-match-your-content"></a>콘텐츠 일치 하는 마스킹

시각적 개체의 콘텐츠와 일치 시킬에 그림자를 하려는 경우에 섀도 마스크 속성에 대 한 시각적 개체의 브러시를 사용 하거나 자동으로 내용에서 마스크를 상속 하도록 그림자를 설정 합니다. LayerVisual를 사용 하는 경우 그림자는 기본적으로 마스크를 상속 합니다.

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

![마스킹된 그림자를 사용 하 여 연결 된 웹 이미지](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>대체 마스크를 사용 하 여

일부 경우에는 시각적 개체의 콘텐츠가 일치 하지 않는 그림자를 구체화 하는 것이 좋습니다. 이 위해서는 브러시를 사용 하 여 알파를 사용 하 여 마스크 속성을 명시적으로 설정 해야 합니다.

에 아래 예제에서는 두 화면-섀도 마스크에 대 한 하나 및 시각적 콘텐츠를 로드 합니다.

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

![마스킹 원 그림자를 사용 하 여 연결 된 웹 이미지](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>애니메이션 적용

시각적 계층에서 표준 이므로 DropShadow 속성 애니메이션을 적용할 수 컴퍼지션 애니메이션을 사용 합니다. 아래에서는 그림자 흐림 효과 반지름에 애니메이션 효과를 위의 혼합할 샘플에서 코드를 수정 합니다.

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

더 복잡 한 프레임 워크 요소에 그림자를 추가 하려는 경우에 XAML 컴퍼지션 사이의 그림자와의 상호 운용성에 몇 가지가 있습니다.

1. 사용 된 [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) Windows 커뮤니티 도구 키트에서 사용할 수 있습니다. 참조 된 [DropShadowPanel 설명서](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) 사용 방법에 대 한 자세한 내용은 합니다.
1. 시각적 개체를 섀도 호스트로 사용 및 XAML 유인물 시각적 개체에 연결을 만듭니다.
1. 컴퍼지션 샘플 갤러리를 사용 하 여 [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) CompositionShadow 컨트롤 사용자 지정 합니다. 사용량에 대 한 다음 예제를 참조 하세요.

## <a name="performance"></a>성능

시각적 계층에 여러 가지 최적화가 효율적이 고 사용할 수 있는 효과에 그림자를 생성할 설정한 옵션에 따라 비교적 비용이 많이 드는 작업을 수 있습니다. 다음은 다양 한 유형의 그림자에 대 한 높은 수준의 '비용'입니다. 특정 그림자 비용이 많이 들 수 있지만 이러한 계속 될 있습니다 특정 시나리오에서 제한적으로 사용 하기에 적합 합니다.

섀도 특성| 비용
------------- | -------------
사각형    | 낮음
Shadow.Mask      | 높은
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 높은
정적 흐리게 반경이 | 낮음
흐리게 반경이 애니메이션 적용 | 높은

## <a name="additional-resources"></a>추가 리소스

- [Composition DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub Repo](https://github.com/Microsoft/WindowsUIDevLabs)