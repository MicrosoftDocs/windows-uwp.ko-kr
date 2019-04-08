---
title: 컴퍼지션 조명
description: 동적 3D 조명 응용 프로그램에 추가할 컴퍼지션 조명 Api는 사용할 수 있습니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 733ce75942a05482ade88c1510e788f1cbd515d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602208"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI에서 광원을 사용 하 여

Windows.UI.Composition Api 실시간 애니메이션 및 효과 만드는 데 사용할 수 있습니다. 컴퍼지션 조명을 2D 응용 프로그램에서 3D 조명 수 있습니다. 이 개요에서는, 컴퍼지션 광원을 설정 하 고, 각 빛을 수신 하도록 시각적 개체를 식별 하 고, 효과 사용 하 여 콘텐츠에 대 한 자료를 정의 하는 방법의 기능을 통해 실행 됩니다.

> [!NOTE]
> 어떻게 읽을 [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) 개체에 적용 [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) XAML Uielement를을 참조 하세요. [XAML 조명](xaml-lighting.md)합니다.

컴퍼지션 조명 함으로써 UI 흥미로운를 만들 수 있습니다.

- 음악 재생 장면 같은 몰입 형 시나리오를 사용 하도록 설정 하려면 장면에서 다른 개체의 밝은 독립적으로의 변환입니다.
- 함께 이동 하도록 광원을 사용 하 여 개체를 페어링할 수 Fluent 같은 시나리오를 사용 하도록 설정 하려면 장면 나머지 무관 [표시](/windows/uwp/design/style/reveal) 강조 표시 합니다.
- 자료를 만들고 깊이를 그룹으로 전체 장면 고 light의 변환입니다.

컴퍼지션 조명 세 가지 주요 개념을 지원합니다. **Light**, **대상을**, 및 **SceneLightingEffect**합니다.

## <a name="light"></a>밝게

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) 다양 한 조명 만들고 좌표 공간에 배치할 수 있습니다. 세 개의이 표시등 조명에서 활성화 된 것으로 식별 하려는 시각적 개체 대상입니다.

### <a name="light-types"></a>밝은 형식

| 형식 | 설명 |
| --- | --- |
| [Ambientlight를 함께 사용](/uwp/api/windows.ui.composition.ambientlight) | 장면에 있는 모든 항목 표시 되는 비 방향 빛을 내보내는 광원 반영 합니다. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 크기에 제한이 없는 먼 광원을 단일 방향으로 조명을 내보냅니다. Sun 같은. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | 조명을 모든 방향으로 내보내는 광원의 지점 원본입니다. 전구 같은. |
| [추천](/uwp/api/windows.ui.composition.spotlight) | 광원을 광원의 내부 및 외부 콘을 내보냅니다. 같은 손전등 합니다. |

## <a name="targets"></a>대상

광원 시각적 개체를 대상 하는 경우 (추가할 [대상](/uwp/api/windows.ui.composition.compositionlight.targets) 목록)을 인식 하 고이 광원 응답할 시각적 개체 및 모든 하위 항목입니다. 이 지점 광원 방향의 애니메이션에는 PointLight 소스 트리 및 아래의 모든 시각적 개체의 루트에 대응 하는 설정을 만큼 단순할 수 있습니다.

**ExclusionsFromTargets** 대상을 추가와 비슷한 방식으로 시각적 개체의 하위 트리 또는 시각적 개체의 조명을 제거할 수 있습니다. 제외 되는 시각적 개체에서 루 팅 트리에서 자식은 결과적으로 켜져 있지 않습니다.

### <a name="sample-targets"></a>샘플 (대상)

아래 샘플을 CompositionPointLight XAML 텍스트 블록을 대상으로 사용 합니다.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

애니메이션 점 광원의 오프셋을 더하여 가물 효과 쉽게 수행 됩니다.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

전체 참조 [텍스트 Shimmer](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) 자세한 무명 인쇄 업자가 WindowUIDevLabs 샘플에 있는 샘플입니다.

## <a name="restrictions"></a>제한 사항

콘텐츠 확인 켜지 CompositionLight 여 때 고려해 야 할 여러 요인이 있습니다.

개념 | 세부 정보
--- | ---
**한 앰비언트 조명** | 장면에 앰비언트가 아니어야 광원 추가 모든 기존 조명 해제 됩니다.  앰비언트가 아니어야 광원 대상이 아닌 항목 검게 표시 됩니다.  대상으로 지정 하지 광원 자연 스러운 방식으로 주변 시각적 개체를 함께 다른 광원에에서 주변 광원을 사용 합니다.
**조명 수** | UI를 대상으로 모든 조합에서 모든 두 앰비언트가 아니어야 컴퍼지션 광원을 사용할 수 있습니다. 주변 광원; 제한 되지 않습니다. 스폿 지점과 먼 광원 됩니다.
**수명** | CompositionLight 수명 조건이 발생할 수 있습니다 (예: 가비지 수집기 사용 되기 전에 조명 개체를 재활용할 수 있습니다).  응용 프로그램 수명을 관리 하는 데 멤버로 조명을 추가 하 여 조명에 대 한 참조를 유지 하는 것이 좋습니다.
**변환** | 광원 UI와 같은 효과 사용 하는 위의 노드에 배치 해야 합니다 [원근 변환을](/windows/uwp/design/layout/3-d-perspective-effects) 올바르게 그릴 시각적 구조에 있습니다.
**대상 및 좌표 공간** | CoordinateSpace 공간이 visual 모든 조명 속성을 설정 해야 합니다. CompositionLight.Targets는 CoordinateSpace 트리 내에 있어야 합니다.

## <a name="lighting-properties"></a>조명 속성

사용 되는 광원의 형식에 따라 광원 감쇠 및 공간에 대 한 속성을 가질 수 있습니다. 모든 종류의 빛이 모든 속성을 사용하지는 않습니다.

속성 | 설명
--- | ---
**색** | 합니다 [Color](/uwp/api/windows.ui.color) 조명의 합니다. 조명 색 값은 정의한 [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) 확산을 주변광을 내보낸다는 것 색을 정의 하는 반사 합니다. 표시등; RGBA 값을 사용 하는 조명 알파 색 구성 요소 사용 되지 않습니다.
**방향** | 광원의 방향입니다. 밝은 가리키고 있는 방향을 기준으로 지정 된 해당 [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual 합니다.
**좌표 공간** | 모든 시각적 개체에는 암시적 3D 좌표 공간입니다. X 방향은 왼쪽에서 오른쪽입니다. Y 방향 방법은 위쪽에서 아래쪽입니다. Z 방향의 평면에서 점입니다. 이 좌표 원래 점은 시각적 개체의 왼쪽 위 모퉁이 고 단위 장치 독립적 픽셀 (DIP)입니다. 이 좌표에 정의 된 광원의 오프셋입니다.
**내부 및 외부 콘** | 스포트라이트는 밝은 안쪽 원뿔과 바깥쪽 원뿔 등 두 부분으로 구성된 원뿔 모양의 빛을 방출합니다. 컴퍼지션 내부 및 외부 원뿔 각도 및 색 제어할 수 있습니다.
**오프셋** | 시각적 개체의 좌표 공간을 기준으로 광원의 오프셋입니다.

> [!NOTE]
> 여러 개의 조명을 동일한 시각적 개체에 도달할 때 또는 광원의 색 값 1.0 초과 충분히 커지면 때마다 조명의 색의 광원 색 채널을 고정으로 인해 변경할 수 있습니다.

### <a name="advanced-lighting-properties"></a>고급 속성을 조명

속성 | 설명
--- | ---
**강도** | 빛의 밝기를 제어합니다.
**감쇠** | 감쇠는 범위 속성에서 정의한 최대 거리를 향하는 동안 조명 강도가 어떻게 줄어드는지 제어합니다.  상수, 선형 및 Quadradic 감쇠 속성도 사용할 수 있습니다.

## <a name="getting-started-with-lighting"></a>Getting Started with 조명

광원 추가 하려면 다음 일반 단계를 수행 합니다.

- 만들고 전등을 배치 합니다. 광원 생성 하 고 지정 된 좌표 공간에서이 배치 합니다.
- Light 개체를 식별 합니다. 밝은 관련 시각적 개체에서 사용 되는 대상입니다.
- [선택 사항] 개별 개체를 정의 광원에 반응 합니다. SceneLightingEffect는 SpriteVisual 표시 하는 것에 대 한 간단한 리플렉션 사용자 지정 하는 EffectBrush 사용 합니다. 리플렉션 기본값 조명이 광원의 CoordinateSpace의 자식 항목을 지원합니다.  SceneLightingEffect를 사용 하 여 그린 시각적 개체는 해당 시각적 개체에 대 한 기본 조명을 덮어씁니다.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 의 내용에 적용할 기본 조명 수정 되는 [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) 대상인를 [CompositionLight](/uwp/api/windows.ui.composition.compositionlight)합니다.

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 자료를 만들기 위한 자주 사용 됩니다. SceneLightingEffect 이미지의 반사 속성을 사용 하도록 설정 및/또는 법선 맵을 사용 하 여 깊이 감을 제공 같은 더 복잡 한 항목을 달성 하려고 할 때 사용 되는 효과입니다. SceneLightingEffect 확산 및 반사 거리 처럼 조명 속성을 사용 하 여 UI를 사용자 지정 하는 기능을 제공 합니다. 추가 하 고 수 있도록 개별적으로 혼합 콘텐츠를 사용 하 여 다른 조명 반응 compose 효과 파이프라인의 나머지 부분을 사용 하 여 조명 효과 사용자 지정할 수 있습니다.

> [!NOTE]
> 장면의 조명 그림자를 생성 하지 않습니다. 2D 렌더링에 초점을 맞춘 영향을 줄 것입니다.  그림자를 비롯 한 실제 조명 모델을 포함 하는 고려 사항 3D 조명 시나리오에는 사용 하지 않습니다.


속성 | 설명
--- | ---
**법선 맵** | NormalMaps 여기서 광원 쪽으로 일반 가리키는 밝게 되며 일반 가리키는 떨어진 됩니다 서서히 어두워 지기 질감의 효과 만듭니다. NormalMap 대상된 visual 사용에 추가 하는 [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) LoadedImageSurface 사용 하 여 NormalMap 자산을 로드 합니다.
**앰비언트** | 앰비언트 속성 전체 색 리플렉션 제어에 주로 사용 됩니다.
**반사** | 반사광 shiny 표시 하는 개체에 강조 표시를 만듭니다. 반사광의 수준 뿐만 아니라 서 빛 수준을 제어할 수 있습니다.  이러한 속성 shinny 금속 또는 광택 용지와 같은 재질 효과를 만들고 조작 합니다.
**확산** | 확산 된 리플렉션 scatters 모든 방향으로 조명을 합니다.
**반사율 모델** | [반사율 모델](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) 중에서 선택할 수 있습니다 [Blinn 퐁](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) 및 Blinn 퐁 물리적으로 기반으로 합니다.  반사 하이라이트를 압축 한 하려고 할 때 실제로 Blinn 퐁 기반을 선택 합니다.

### <a name="sample-scenelightingeffect"></a>샘플 (SceneLightingEffect)

아래 샘플을 SceneLightingEffect 법선 맵을 추가 하는 방법을 보여 줍니다.

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

- [자료를 만들고 시각적 계층에서 광원](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [조명 개요](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [조명의 수학](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 리포지토리](https://github.com/Microsoft/WindowsUIDevLabs)
