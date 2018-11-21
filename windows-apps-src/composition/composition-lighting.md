---
author: daneuber
title: 컴퍼지션 조명
description: 3D 동적 조명 응용 프로그램에 추가 하는 컴퍼지션 조명 Api는 사용할 수 있습니다.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c5c7bfcb06eb673b0516cef7882685ebd19ddb97
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7424201"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI에 광원을 사용 하 여

Windows.UI.Composition Api를 사용 하 여 실시간 애니메이션 및 효과 만들 수 있습니다. 컴퍼지션 조명을 2D 응용 프로그램에서 3D 조명 수 있습니다. 이 개요에서는 컴퍼지션 조명 설정, 각 빛을 받아들이지 않는를 시각적 개체를 식별 하 고, 효과 사용 하 여 콘텐츠에 대 한 자료를 정의 하는 방법의 기능을 통해 실행 됩니다.

> [!NOTE]
> [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) 개체를 XAML UIElements를 비추는 [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) 적용 하는 방법을 알아보려면 [XAML 조명](xaml-lighting.md)를 참조 하세요.

컴퍼지션 조명 사용 하면 UI 함으로써 흥미로운 만들 수 있습니다.

- 음악 재생 장면 같은 몰입 형 시나리오를 사용 하도록 설정 하려면 장면의 다른 개체의 밝은 독립적의 변환입니다.
- 함께 이동 하도록 광원을 사용 하 여 개체를 페어링할 수 독립적 장면 Fluent [표시](/design/style/reveal.md) 강조와 같은 시나리오를 사용 하도록 설정의 나머지 부분입니다.
- 재료를 만들고 깊이를 그룹으로 빛과 장면 전체 변환 합니다.

컴퍼지션 조명 세 가지 주요 개념을 지원: **조명**, **대상**및 **SceneLightingEffect**합니다.

## <a name="light"></a>Light

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) 다양 한 조명을 생성 및 좌표 공간에 배치할 수 있습니다. 이 조명은 조명에 의해 켜 집니다로 식별 하고자 하는 시각적 개체를 대상으로 합니다.

### <a name="light-types"></a>조명 유형

| 유형 | 설명 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 광원 표시 되는 비 방향 빛을 방출 하는 모든 작업 장면에 반영 되어 있습니다. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 무한 큰 원거리 광원 단일 방향으로 빛을 방출 하는 합니다. 마찬가지로 태양 합니다. |
| [점 조명](/uwp/api/windows.ui.composition.pointlight) | 모든 방향에서 빛을 방출 하는의 지점 소스입니다. 마찬가지로 전구 합니다. |
| [추천](/uwp/api/windows.ui.composition.spotlight) | 조명의 안쪽 및 바깥쪽 원뿔을 방출 하는 광원 합니다. 같은 플래시 합니다. |

## <a name="targets"></a>대상

때 조명을 대상 시각적 ( [대상](/uwp/api/windows.ui.composition.compositionlight.targets) 목록에 추가), 시각적 및 모든 하위 항목의 인식 되며이 광원에 응답 합니다. 점 광원 방향으로 애니메이션에 반응 점 조명은 소스 트리와 아래의 모든 시각적 개체의 루트에 설정으로 단순할 수 있습니다.

**ExclusionsFromTargets** 대상을 추가 하는 것과 유사한 방식에서 조명 시각적 또는 시각 트리를 제거 하는 기능을 제공 합니다. 자식 트리의 루트로 제외 되는 시각적으로에 결과적으로 켜지 지 않습니다.

### <a name="sample-targets"></a>샘플 (대상)

아래 예제는 CompositionPointLight를 사용 하 여 XAML TextBlock을 대상으로 지정 합니다.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

점 광원의 오프셋에 애니메이션을 추가 하 여 가물 효과 쉽게 수행 됩니다.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

자세한 내용은 WindowUIDevLabs 샘플 교정쇄에서 전체 [텍스트 마다 어른 려 보입니다](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) 샘플을 참조 하세요.

## <a name="restrictions"></a>제한 사항

CompositionLight 하 여 켜져 있는 콘텐츠를 결정할 때 고려해 야 할 몇 가지 요인이 있습니다.

개념 | 세부 정보
--- | ---
**주변 광원** | 장면에 비 주변 광원을 추가 모든 기존 빛 해제 됩니다.  항목 이외의 주변 광원의 대상으로 지정 되지 검게 표시 됩니다.  자연 스러운 방법으로 조명에 의해 대상이 아닌 주변 시각 효과 비추는 다른 조명와 함께에서 주변 광원을 사용 합니다.
**조명 수** | UI를 대상으로 두 모든 주변 비 컴퍼지션 조명 조합에서 사용할 수 있습니다. 주변 광원; 제한 되지 않습니다. 발견 점과 원거리 광원 됩니다.
**수명** | CompositionLight 수명 조건 발생할 수 있습니다 (예: 가비지 수집기는 사용 하기 전에 조명 개체를 재활용 될 수 있습니다).  응용 프로그램 수명을 관리 하는 데 멤버로 조명을 추가 하 여 광원에 대 한 참조를 유지 하는 것이 좋습니다.
**변환** | 조명 노드에 시각적 구조에 [관점 변환을](/design/layout/3-d-perspective-effects.md) 같은 효과 사용 하 여 제대로 그릴 수 있는 UI 위에 배치 되어야 합니다.
**대상 및 좌표 공간** | CoordinateSpace는 시각적 공간은 모든 조명 속성을 설정 해야 합니다. CompositionLight.Targets는 CoordinateSpace 트리 내에 있어야 합니다.

## <a name="lighting-properties"></a>조명 속성

조명 사용 유형에 따라 조명 감쇠 및 공간에 대 한 속성에 있을 수 있습니다. 모든 종류의 빛이 모든 속성을 사용하지는 않습니다.

속성 | 설명
--- | ---
**색상** | 광원의 [색](/uwp/api/windows.ui.color) 입니다. 조명 색상 값 [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) 확산, 주변광, 및 방출 되 고 색상을 정의 하는 Specular에 의해 정의 됩니다. 조명에 RGBA 값을 사용 하는 조명 알파 색 구성 요소 사용 되지 않습니다.
**방향** | 조명의 방향입니다. 빛이 가리키는 방향 해당 [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual 기준으로 지정 됩니다.
**좌표 공간** | 모든 시각적 요소에 암시적 3D 좌표 공간. X 방향이 왼쪽에서 오른쪽입니다. Y 방향으로 위에서 아래로 됩니다. Z 방향으로 평면에서 지점입니다. 이 좌표의 원점이 왼쪽 위 모서리를 시각적 개체의 이며 단위는 장치 독립적인 픽셀 (DIP). 이 좌표에 정의 된 빛의 오프셋입니다.
**안쪽 및 바깥쪽 원뿔** | 스포트라이트는 밝은 안쪽 원뿔과 바깥쪽 원뿔 등 두 부분으로 구성된 원뿔 모양의 빛을 방출합니다. 컴퍼지션 안쪽 및 바깥쪽 원뿔 각도 색을 제어할 수 있습니다.
**오프셋** | Visual의 좌표 공간을 기준으로 광원의 오프셋입니다.

> [!NOTE]
> 여러 개의 조명을 적중 동일한 시각적 또는 광원의 색상 값 1.0 초과 충분히 큰 발생할 때마다 조명 색 채널의 고정 인해 광원의 색상을 변경할 수 있습니다.

### <a name="advanced-lighting-properties"></a>고급 조명 속성

속성 | 설명
--- | ---
**강도** | 조명의 밝기를 제어합니다.
**감쇠** | 감쇠는 범위 속성에서 정의한 최대 거리를 향하는 동안 조명 강도가 어떻게 줄어드는지 제어합니다.  상수, Quadradic 및 선형 감쇠 속성도 사용할 수 있습니다.

## <a name="getting-started-with-lighting"></a>조명 시작

광원을 추가 하려면 다음 일반 단계를 따르세요.

- 만들고 광원을 배치: 조명을 생성 하 고 지정 된 좌표 공간에 배치 합니다.
- 조명 개체를 식별: 관련 시각적 개체에는 빛을 대상으로 합니다.
- [선택 사항] 개별 개체를 정의 조명에 반응: 조명 반사가 SpriteVisual 표시 하기 위한 사용자 지정 하려면 EffectBrush 사용 하 여 사용 하 여 SceneLightingEffect 합니다. 반사 기본값은 광원의 CoordinateSpace 자식의 조명을 지원합니다.  시각적 개체는 SceneLightingEffect와 그리고 해당 visual에 대 한 기본 조명을 덮어씁니다.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) [CompositionLight](/uwp/api/windows.ui.composition.compositionlight)대상의 내용에 적용 된 기본 조명을 수정 하는 데 사용 됩니다.

재료 만들기 [SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 자주 사용 됩니다. SceneLightingEffect 달성 조금 더 복잡 한, 같은 이미지에 반사 속성을 사용 하도록 설정 및/또는 일반 지도 사용 하 여 깊이 효과를 제공 하려는 경우를 사용 하는 효과입니다. SceneLightingEffect 확산 및 반사 시간과 같은 조명 속성을 사용 하 여 UI를 사용자 지정 하는 기능을 제공 합니다. 혼합 하 고 다른 조명 능동형 콘텐츠로 작성을 개별적으로 수 있도록 효과 파이프라인의 나머지 부분을 사용 하 여 조명 효과 정의할 수 있습니다.

> [!NOTE]
> 장면 조명, 그림자를 만들지 않습니다. 2D 렌더링에 집중 하는 효과입니다.  그림자를 포함 하 여 실제 조명 모델을 포함 하는 고려 3D 조명 시나리오에 사용 하지 않습니다.


속성 | 설명
--- | ---
**일반 맵** | NormalMaps 조명 방향으로 일반 가리키는 밝아집니다 하며 조 광 수 떨어져 일반 가리키는 텍스처의 효과 만듭니다. [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) LoadedImageSurface를 사용 하 여 NormalMap 자산을 로드 하는 NormalMap 대상 시각적 사용에 추가 합니다.
**앰비언트** | 주변 속성 전체 색 반사 제어 주로 사용 됩니다.
**반사** | 반사 리플렉션 하이라이트 반짝 거리를 표시 하는 개체를 만듭니다. 반사 리플렉션 수준의 물론 빛의 수준을 제어할 수 있습니다.  이러한 속성은 shinny 금속 또는 거친 용지 같은 재료 효과를 만드는 조작 합니다.
**확산** | 확산된 반사 조명 모든 방향에서 scatters 합니다.
**반사율 모델** | [반사율 모델](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) 을 사용 하면 [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) 및 물리적으로 Blinn Phong 기반 중에서 선택할 수 있습니다.  반사 하이라이트를 압축 한 하려는 경우 물리적으로 Blinn Phong 기반을 선택 합니다.

### <a name="sample-scenelightingeffect"></a>샘플 (SceneLightingEffect)

아래의 예제는 SceneLightingEffect 표준 맵은 추가 하는 방법을 보여 줍니다.

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

## <a name="related-articles"></a>관련 문서

- [시각적 계층의 재료와 조명 만들기](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [조명 개요](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [조명의 수학](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 리포지토리](https://github.com/Microsoft/WindowsUIDevLabs)
