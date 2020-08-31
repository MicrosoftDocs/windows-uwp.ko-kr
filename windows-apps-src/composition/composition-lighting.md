---
title: 컴퍼지션 조명
description: 컴퍼지션 조명 Api를 사용 하 여 응용 프로그램에 동적 3D 조명을 추가할 수 있습니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154707"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI에서 조명 사용

Windows. u i > Api를 사용 하면 실시간 애니메이션 및 효과를 만들 수 있습니다. 컴퍼지션 조명을 통해 2D 응용 프로그램에서 3D 조명을 사용할 수 있습니다. 이 개요에서는 컴퍼지션 광원을 설정 하 고, 각 조명을 받는 시각적 개체를 식별 하 고, 효과를 사용 하 여 콘텐츠에 대 한 자료를 정의 하는 방법의 기능을 실행 합니다.

> [!NOTE]
> [Xamllight](/uwp/api/windows.ui.xaml.media.xamllight) 개체가 [COMPOSITIONLIGHTS](/uwp/api/Windows.UI.Composition.CompositionLight) 을 조명 xaml UIElements에 적용 하는 방법을 알아보려면 [xaml 조명](xaml-lighting.md)을 참조 하세요.

컴퍼지션 조명을 사용 하면 다음을 허용 하 여 흥미로운 UI를 만들 수 있습니다.

- 장면에 있는 다른 개체와 독립적인 광원을 변환 하 여 음악 재생 장면 같은 몰입 형 시나리오를 지원 합니다.
- [흐름 표시](../design/style/reveal.md) 강조 표시와 같은 시나리오를 사용할 수 있도록 개체를 조명을 연결 하 여 장면의 나머지 부분과 독립적으로 이동 하는 기능입니다.
- 조명 및 전체 장면을 그룹으로 변환 하 여 재질 및 깊이를 만듭니다.

컴퍼지션 조명은 **광원**, **대상**및 **SceneLightingEffect**의 세 가지 주요 개념을 지원 합니다.

## <a name="light"></a>밝음

[CompositionLight](/uwp/api/windows.ui.composition.compositionlight) 를 사용 하면 다양 한 조명을 만들어 좌표 공간에 배치할 수 있습니다. 이러한 광원은 조명에서 lit로 식별 하려는 시각적 개체를 대상으로 합니다.

### <a name="light-types"></a>광원 유형

| 유형 | Description |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 장면의 모든 항목에 반영 되는 비 방향성 조명을 내보내는 광원입니다. |
| [Dist앤틸리스 라이트](/uwp/api/windows.ui.composition.distantlight) | 단일 방향으로 조명을 내보내는 무한 멀리 떨어져 있는 광원입니다. Sun과 비슷합니다. |
| [유사](/uwp/api/windows.ui.composition.pointlight) | 조명을 모든 방향으로 내보내는 점 원본입니다. 전구와 같습니다. |
| [스포트라이트](/uwp/api/windows.ui.composition.spotlight) | 광원의 내부 및 외부 원추를 내보내는 광원입니다. 손전등와 같습니다. |

## <a name="targets"></a>대상

조명이 시각적 개체 (대상 목록에 추가)를 대상 [으로 하는](/uwp/api/windows.ui.composition.compositionlight.targets) 경우 시각적 개체와 모든 해당 하위 항목은이 광원을 인식 하 고이 광원에 응답 합니다. 트리의 루트에서 PointLight 소스를 설정 하는 것 처럼 간단 하 고 아래 모든 시각적 개체는 점 조명 방향의 애니메이션에 반응 합니다.

**ExclusionsFromTargets** 를 사용 하면 대상을 추가 하는 것과 비슷한 방식으로 시각적 개체의 하위 트리 또는 시각적 개체의 조명을 제거할 수 있습니다. 제외 되는 시각적 개체를 기반으로 하는 트리의 자식은 결과로 lit 않습니다.

### <a name="sample-targets"></a>샘플 (대상)

아래 샘플에서는 CompositionPointLight를 사용 하 여 XAML TextBlock을 대상으로 합니다.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

시점 광원의 오프셋에 애니메이션을 추가 하 여 shimmering 효과를 쉽게 달성할 수 있습니다.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

자세히 알아보려면 WindowUIDevLabs 샘플 교정쇄에서 complete [Text Shimmer](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) 샘플을 참조 하세요.

## <a name="restrictions"></a>제한 사항

CompositionLight에 의해 발생 하는 콘텐츠를 결정할 때 고려해 야 할 몇 가지 요소가 있습니다.

개념 | 세부 정보
--- | ---
**주변 광원** | 장면에 비 앰비언트 조명을 추가 하면 기존 조명이 모두 꺼집니다.  비 앰비언트 조명의 대상이 아닌 항목은 검은색으로 표시 됩니다.  광원의 대상으로 지정 되지 않은 주변 시각적 개체를 자연스럽 게 표시 하려면 다른 조명과 함께 주변 광원을 사용 합니다.
**광원 수** | 모든 조합에서 두 개의 비 앰비언트 컴퍼지션 광원을 사용 하 여 UI를 대상으로 할 수 있습니다. 주변 광원은 제한 되지 않습니다. 지점, 점 및 원거리 광원은입니다.
**수명** | CompositionLight 수명 조건이 발생할 수 있습니다 (예: 가비지 수집기는 사용 하기 전에 라이트 개체를 재활용할 수 있음).  응용 프로그램의 수명을 관리 하는 데 도움이 되도록 조명을 구성원으로 추가 하 여 광원에 대 한 참조를 유지 하는 것이 좋습니다.
**변환** | 시각적 구조에서 [원근감 변환과](../design/layout/3-d-perspective-effects.md) 같은 효과를 사용 하 여 올바르게 그려야 하는 UI 위의 노드에 조명을 두어야 합니다.
**대상 및 좌표 공간** | CoordinateSpace은 모든 광원 속성을 설정 해야 하는 표시 영역입니다. CompositionLight는 CoordinateSpace 트리 내에 있어야 합니다.

## <a name="lighting-properties"></a>조명 속성

사용 되는 광원 유형에 따라 광원에는 감쇠 및 공간에 대 한 속성이 포함 될 수 있습니다. 모든 종류의 표시등이 모든 속성을 사용 하는 것은 아닙니다.

속성 | Description
--- | ---
**색상** | 조명의 [색](/uwp/api/windows.ui.color) 입니다. 조명 색 값은 내보내지는 색을 정의 하는 [D3D](../graphics-concepts/light-properties.md) 확산, 앰비언트 및 반사로 정의 됩니다. 조명이 광원에 대해 RGBA 값을 사용 합니다. 알파 색 구성 요소는 사용 되지 않습니다.
**Direction** | 광원의 방향입니다. 조명이 가리키는 방향이 [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) 시각적 개체를 기준으로 지정 됩니다.
**좌표 공간** | 모든 시각적 개체에는 암시적 3D 좌표 공간이 있습니다. X 방향이 왼쪽에서 오른쪽입니다. Y 방향은 위쪽에서 아래쪽입니다. Z 방향은 평면의 점입니다. 이 좌표의 원래 점은 시각적 개체의 왼쪽 위 모퉁이가 고 단위는 DIP (장치 독립적 픽셀)입니다. 이 좌표에 정의 된 조명의 오프셋입니다.
**내부 및 외부 원추** | 집중은 밝은 내부 원뿔과 외부 원뿔의 두 부분으로 구성 된 원뿔을 내보냅니다. 컴퍼지션을 사용 하면 내부 및 외부 원뿔 각도와 색을 제어할 수 있습니다.
**Offset** | 좌표 공간 시각적 개체를 기준으로 하는 광원의 오프셋입니다.

> [!NOTE]
> 여러 조명이 동일한 시각적 개체에 도달 하거나, 밝은 색 값이 1.0을 초과 하기에 충분히 커질 때마다 조명의 색 채널이 고정 때문에 조명의 색이 변경 될 수 있습니다.

### <a name="advanced-lighting-properties"></a>고급 조명 속성

속성 | Description
--- | ---
**농도가** | 조명의 밝기를 제어 합니다.
**감쇠** | 감쇠는 조명의 강도가 range 속성에 지정 된 최대 거리를 향해 감소 하는 방식을 제어 합니다.  상수, Quadradic 및 선형 감쇠 속성을 사용할 수 있습니다.

## <a name="getting-started-with-lighting"></a>조명 시작

조명을 추가 하려면 다음과 같은 일반적인 단계를 따르세요.

- 광원 만들기 및 두기: 조명을 만들고 지정 된 좌표 공간에 넣습니다.
- 개체 식별: 관련 시각적 개체에서 광원을 대상으로 지정 합니다.
- 필드 개별 개체가 광원에 반응 하는 방식을 정의 합니다. SceneLightingEffect를 EffectBrush와 함께 사용 하 여 SpriteVisual를 표시 하는 빛 반사를 사용자 지정할 수 있습니다. 리플렉션 기본값은 광원의 CoordinateSpace 자식 조명을 지원 합니다.  SceneLightingEffect로 그린 시각적 개체는 해당 시각적 개체에 대 한 기본 조명을 덮어씁니다.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 은 [CompositionLight](/uwp/api/windows.ui.composition.compositionlight)의 대상 [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) 내용에 적용 되는 기본 조명을 수정 하는 데 사용 됩니다.

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 은 자료 생성에 자주 사용 됩니다. SceneLightingEffect는 이미지에 반사 속성을 사용 하도록 설정 하는 것과 같은 복잡 한 작업을 수행할 때 사용 되는 효과 이며 표준 지도를 사용 하 여 깊이의 효과를 제공 합니다. SceneLightingEffect는 반사 및 확산 양과 같은 조명 속성을 사용 하 여 UI를 사용자 지정 하는 기능을 제공 합니다. 효과 파이프라인의 나머지 부분을 사용 하 여 조명 효과를 추가로 사용자 지정 하 여 사용자가 콘텐츠를 서로 다른 조명 반응 혼합할 수 있습니다.

> [!NOTE]
> 장면 조명이 그림자를 생성 하지 않습니다. 2D 렌더링에 초점을 맞춘 효과입니다.  그림자를 비롯 한 실제 조명 모델을 포함 하는 3D 조명 시나리오를 고려 하지 않습니다.


속성 | Description
--- | ---
**표준 맵** | 법선 맵은 빛을 향한 법선이 더 밝아지고 일반적인 포인팅이 dimmer 되는 질감 효과를 만듭니다. 대상 시각적 개체에 NormalMap을 추가 하려면 LoadedImageSurface를 사용 하 여 [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 를 사용 하 여 normalmap 자산을 로드 합니다.
**앰비언트** | 앰비언트 속성은 일반적으로 전체 색 반사를 제어 하는 데 사용 됩니다.
**반사** | 반사 반사는 개체에 대 한 하이라이트를 만들어 개체가 빛나는 것으로 표시 되도록 합니다. 빛의 수준 뿐만 아니라 반사 리플렉션의 수준도 제어할 수 있습니다.  이러한 속성을 조작 하 여 shinny 귀금속 또는 광택 용지와 같은 재질 효과를 만듭니다.
**확산** | 확산 된 리플렉션에서는 모든 방향으로 조명을 scatters.
**Reflectance 모델** | [Reflectance 모델](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) 을 사용 하면 [Blinn 퐁](/visualstudio/designers/how-to-create-a-basic-phong-shader) 와 물리적으로 기반 Blinn 퐁 중에서 선택할 수 있습니다.  압축 된 반사 하이라이트를 사용 하려는 경우 물리적으로 Blinn 퐁를 선택 합니다.

### <a name="sample-scenelightingeffect"></a>샘플 (SceneLightingEffect)

아래 샘플에서는 SceneLightingEffect에 법선 지도를 추가 하는 방법을 보여 줍니다.

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>관련된 문서

- [시각적 계층에서 재질 및 조명 만들기](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [조명 개요](../graphics-concepts/lighting-overview.md)
- [CompositionCapabilities API](/uwp/api/windows.ui.composition.compositioncapabilities)
- [광원의 수학](../graphics-concepts/mathematics-of-lighting.md)
- [SceneLightingEffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 리포지토리](https://github.com/microsoft/WindowsCompositionSamples)