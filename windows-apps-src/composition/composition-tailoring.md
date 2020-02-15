---
title: 컴퍼지션 조정
description: 컴퍼지션 Api를 사용 하 여 UI를 조정 하 고, 성능을 최적화 하 고, 사용자 설정 및 장치 특성을 수용할 수 있습니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 95a7355241f9ba4cc7b4bb743b78ac09169d65d9
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213680"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Windows UI를 사용 하 여 효과 & 환경을 조정 합니다.

Windows UI는 차별화를 위한 다양 한 효과, 애니메이션 및 수단을 제공 합니다. 그러나 성능 및 사용자 지정 가능성에 대 한 사용자 기대치를 충족 하는 것은 여전히 성공적인 응용 프로그램을 만드는 데 필요한 부분입니다. 유니버설 Windows 플랫폼는 다양 한 기능 및 기능이 있는 광범위 한 장치 패밀리를 지원 합니다. 모든 사용자에 대 한 포함 환경을 제공 하기 위해 응용 프로그램을 장치 간에 확장 하 고 사용자 기본 설정을 준수 하는지 확인 해야 합니다. UI를 조정 하면 장치의 기능을 활용 하 고 뛰어난 사용자 환경을 보장 하는 효율적인 방법을 제공할 수 있습니다.

UI를 조정 하는 것은 다음 영역과 관련 하 여 성능, 멋진 UI에 대 한 작업을 포괄 하는 포괄적인 범주입니다.

- 효과에 대 한 사용자 설정 준수 및 적응
- 애니메이션에 대 한 사용자 설정 수용
- 지정 된 하드웨어 기능에 대 한 UI 최적화

위의 영역에서 시각적 계층을 사용 하 여 효과와 애니메이션을 조정 하는 방법을 설명 하지만, 응용 프로그램을 조정 하 여 뛰어난 최종 사용자 환경을 보장 하는 다른 여러 가지 방법이 있습니다. 다양 한 장치에 대 한 [ui](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) 를 조정 하 고 [응답성이 뛰어난 ui를 만드는](/windows/uwp/design/layout/responsive-design)방법에 대 한 지침 문서를 사용할 수 있습니다.

## <a name="user-effects-settings"></a>사용자 효과 설정

사용자는 응용 프로그램에서 준수 해야 하는 다양 한 이유로 Windows 환경을 사용자 지정할 수 있습니다. 사용자가 제어할 수 있는 한 가지 영역은 시스템 전체에서 사용 되는 효과의 유형을 변경 하는 것입니다.

### <a name="transparency-effects-settings"></a>투명도 효과 설정

사용자가 사용자 지정할 수 있는 효과 중 하나는 투명도 효과를 설정/해제 하는 것입니다. 이러한 설정은 설정 앱의 개인 설정 > 색 아래 또는 설정 앱을 통해 볼 수 있습니다. > 액세스 편의성 > 표시 됩니다.

![설정의 투명도 옵션](images/tailoring-transparency-setting.png)

이 옵션이 설정 되 면 투명도를 사용 하는 모든 효과가 예상 대로 표시 됩니다. 이는 아크릴, HostBackdropBrush 또는 완전히 불투명 하지 않은 모든 사용자 지정 효과 그래프에 적용 됩니다.

이 기능을 해제 하면 XAML의 아크릴 브러시가 기본적으로이 이벤트를 수신 하기 때문에 아크릴 재질이 단색으로 자동 대체 됩니다. 여기에서 투명도 효과를 사용 하지 않을 때 계산기 앱이 단색으로 적절 하 게 대체 하는 것을 볼 수 있습니다.

아크릴](images/tailoring-acrylic.png)
![계산기를 사용 하 여 계산기 ![투명도 설정에 응답](images/tailoring-acrylic-fallback.png)

그러나 사용자 지정 효과의 경우 응용 프로그램은 [AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) 속성 또는 [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) 이벤트에 응답 하 고 투명도가 없는 효과를 사용 하도록 효과/효과 그래프를 전환 해야 합니다. 이에 대 한 예는 다음과 같습니다.

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>애니메이션 설정

마찬가지로 응용 프로그램은 [AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) 속성에 대 한 응답을 수신 하 고 응답 하 여 설정의 사용자 설정에 따라 애니메이션을 설정 하거나 해제 하 여 > 디스플레이의 접근성을 > 합니다.

![설정의 애니메이션 옵션](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>기능 API 활용

[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) api를 활용 하 여 제공 된 하드웨어에서 사용할 수 있는 컴퍼지션 기능을 검색 하 고, 최종 사용자가 모든 장치에서 성능 및 멋진 경험을 얻을 수 있도록 디자인을 조정할 수 있습니다. Api는 다양 한 폼 팩터를 통해 정상 효과 크기 조정을 구현 하기 위해 하드웨어 시스템 기능을 확인 하는 수단을 제공 합니다. 이렇게 하면 응용 프로그램을 적절 하 게 적절 하 게 조정 하 여 멋진 최종 사용자 환경을 만들 수 있습니다.

이 API는 응용 프로그램 UI에 대 한 효과 확장 결정을 내리는 데 사용할 수 있는 메서드 및 이벤트 수신기를 제공 합니다. 이 기능은 시스템에서 복잡 한 컴퍼지션 및 렌더링 작업을 얼마나 잘 처리할 수 있는지 검색 한 다음 개발자가 활용할 수 있도록 사용 하기 쉬운 모델로 정보를 반환 합니다.

### <a name="using-composition-capabilities"></a>컴퍼지션 기능 사용

CompositionCapabilities 기능은 아크릴 material과 같은 기능을 위해 이미 활용 되 고 있으며,이는 시나리오 및 하드웨어에 따라 자료가 보다 뛰어난 효과로 대체 됩니다.

API는 몇 가지 간단한 단계를 통해 기존 코드에 추가할 수 있습니다.

1. 응용 프로그램의 생성자에서 기능 개체를 가져옵니다.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 앱에 대 한 기능 변경 이벤트 수신기를 등록 합니다.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 이벤트 콜백 메서드에 콘텐츠를 추가 하 여 다양 한 기능 수준을 처리 합니다. 이는 아래의 다음 단계와 유사할 수도 있고 그렇지 않을 수도 있습니다.
1. 효과를 사용 하는 경우 먼저 기능 개체를 확인 합니다. 효과를 조정 하는 방법에 따라 조건부 검사 또는 switch 제어 문을 사용 하는 것이 좋습니다.

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

전체 예제 코드는 [WINDOWS UI Github 리포지토리](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)에서 찾을 수 있습니다.

## <a name="fast-vs-slow-effects"></a>고속 및 저속 효과

CompositionCapabilities API에서 제공 된 [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) 및 [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) 메서드의 피드백에 따라 응용 프로그램은 장치에 최적화 된 다른 효과를 위해 비용이 많이 들고 지원 되지 않는 효과를 교환 하도록 결정할 수 있습니다. 일부 효과는 지속적으로 다른 리소스 보다 더 많은 리소스를 사용 하는 것으로 알려져 있으며, 다른 효과는 더 자유롭게 사용할 수 있습니다. 그러나 모든 효과의 경우 일부 시나리오 또는 조합의 경우 효과 그래프의 성능 특성이 변경 될 수 있으므로 연결 하 고 애니메이션을 적용 하는 경우 주의 해야 합니다. 다음은 개별 효과의 성능 특징에 대 한 몇 가지 규칙입니다.

- 높은 성능에 영향을 주는 것으로 알려진 효과는 가우시안 흐림, 그림자 마스크, BackDropBrush, HostBackDropBrush 및 레이어 시각적 개체와 같습니다. 이는 낮은 종료 장치 [(기능 수준 9.1-9.3)](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)에는 권장 되지 않으며, 높은 최종 장치에서 신중 하 게 사용 해야 합니다.
- 중간 성능 영향을 주는 효과는 색 매트릭스, 특정 Blend 효과 BlendModes (광도, 색, 채도 및 색상), 스포트라이트, SceneLightingEffect 및 (시나리오에 따라) BorderEffect입니다. 이러한 영향은 낮은 종료 장치의 특정 시나리오에서 작동할 수 있지만 연결 및 애니메이션을 적용 하는 경우 주의 해야 합니다. 두 개 이하로 사용을 제한 하 고 전환에만 애니메이션 효과를 주는 것이 좋습니다.
- 다른 모든 효과는 성능에 영향을 주지 않으며, 애니메이션을 적용 하 고 연결 하는 경우 모든 적당 한 시나리오에서 작동 합니다.

## <a name="related-articles"></a>관련 문서

- [UWP 응답성 디자인 기술](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 장치 조정](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
