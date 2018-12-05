---
title: 컴퍼지션 조정
description: 컴퍼지션 Api를 사용 하 여 UI를 조정, 성능 최적화 및 사용자 설정 및 디바이스 특성을 수용 합니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6252ce3d2e213602250f6c24f8867767accecbe
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8702882"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>효과 및 Windows UI를 사용 하 여 환경을 조정

Windows UI의 차별화 많은 멋진 효과, 애니메이션 및 수단을 제공합니다. 그러나 성능과 대 한 사용자 기대 충족은 여전히 성공적인 응용 프로그램을 만드는 부분이 합니다. 유니버설 Windows 플랫폼 큰, 다양 한 일련의 다른 기능과 특징을 가진 장치를 지원 합니다. 모든 사용자를 위한 포괄적인 경험을 제공 하기 위해 장치에서 응용 프로그램 확장을 맞추고 사용자 기본 설정 준수 해야 합니다. UI 조정 디바이스의 기능을 활용 하 고 재미 있고 포괄적인 사용자 경험을 효율적으로 제공할 수 있습니다.

UI 조정은 광범위 한 범주를 포함 하는 성능, 다음 영역에 대해 아름 다운 UI에 대 한 작업입니다.

- 사용 하 고 효과 대 한 사용자 설정 조정
- 애니메이션에 대 한 사용자 설정을 수용합니다.
- 특정된 하드웨어 기능에 대 한 UI를 최적화합니다.

여기에서는 효과 위의 영역에 시각적 계층을 사용 하 여 애니메이션에 맞게 조정 하는 방법을 살펴보겠습니다 있지만 뛰어난 최종 사용자 환경을 제공 하기 위해 응용 프로그램에 맞게 조정 하는 다른 많은 수단입니다. 지침 문서 다양 한 장치 및 [반응 형 UI를 만들기](/design/layout/responsive-design.md)에 대 한 [UI를 조정](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) 하는 방법에 사용할 수 있습니다.

## <a name="user-effects-settings"></a>사용자 효과 설정

사용자가 다양 한 응용 프로그램 준수 및 적응 하는 이유는 Windows 환경을 사용자 지정할 수 있습니다. 최종 사용자가 제어할 수 있는 하나의 영역이 시스템 전체에서 사용 시간의 표시 효과의 형식을 변경 됩니다.

### <a name="transparency-effects-settings"></a>투명도 효과 설정

켜기/끄기 투명도 효과 회전 하는 이러한 한 효과 설정을 사용자 지정할 수 있습니다. 개인 설정에서 설정 앱에서 찾을 수 있습니다 > 색상, 또는 설정 앱을 통해 > 접근성 > 표시 합니다.

![설정에서 투명도 옵션](images/tailoring-transparency-setting.png)

켜진 경우 투명도 사용 하는 모든 효과 예상 대로 표시 됩니다. 아크릴, HostBackdropBrush, 또는 완전 불투명 되지 않은 모든 사용자 지정 효과 그래프가 적용 됩니다.

꺼져 있으면 아크릴 재질 자동으로 대체 됩니다 단색 하므로 XAML의 아크릴 브러시는 기본적으로이 이벤트를 수신 대기 합니다. 여기에서 적절 하 게 대체 단색 투명 효과 사용 하지 않을 때 계산기 앱 확인할:

![아크릴을 사용한 계산기](images/tailoring-acrylic.png)
![투명도 설정에 응답 하는 아크릴을 사용한 계산기](images/tailoring-acrylic-fallback.png)

그러나 모든 사용자 지정 효과 대 한 응용 프로그램 [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) 속성 또는 [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) 이벤트에 응답 하 없는 투명도 있는 효과 사용 하 여 효과/효과 그래프를 전환 해야 합니다. 이 예는 다음과 같습니다.

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

응용 프로그램은 수신 하 고 애니메이션은 설정 하거나 해제 설정에서 사용자 설정에 따라 [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) 속성에 응답 하는 마찬가지로, > 접근성 > 표시 합니다.

![설정에서 애니메이션 옵션](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>API 기능을 활용

[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) Api 활용 하 여 어떤 컴퍼지션 기능 사용할 수 있는 고성능에 특정 하드웨어를 감지할 수 있으며 최종 사용자 장치에서 성능이 및 멋진 환경을 얻을 수 있도록 디자인을 조정할 수 있습니다. Api는 다양 한 양식 요소에서 크기를 조정 하는 정상적인 효과 구현 하기 위해 하드웨어 시스템 기능을 확인 하는 수단을 제공 합니다. 이렇게 하면 쉽게 적절 하 게 만드는 아름 다운 응용 프로그램 및 원활한 최종 사용자 경험을 조정할 수 있습니다.

이 API는 메서드 및 효과 응용 프로그램 UI에 대 한 결정을 크기 조정을 사용할 수 있는 이벤트 수신기를 제공 합니다. 기능은 시스템 복잡 한 컴퍼지션 및 렌더링 작업을 처리할 수 얼마나 잘 감지를 활용 하는 개발자를 위한 사용이 쉬운 모델의 정보를 반환 합니다.

### <a name="using-composition-capabilities"></a>컴퍼지션 기능을 사용 하 여

아크릴 재질, 재료 알림으로 시나리오 및 하드웨어에 따라 더 많은 성능이 효과 대체 위치 등의 기능에 대 한 CompositionCapabilities 기능을 활용 하 고 이미 합니다.

API는 기존 코드 몇 가지 간단한 단계에 추가할 수 있습니다.

1. 응용 프로그램의 생성자에서 기능 개체를 획득 합니다.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 앱에 대 한 접근 권한 값 변경된 이벤트 수신기를 등록 합니다.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 콘텐츠를 다양 한 기능 수준을 처리 하도록 이벤트 콜백 메서드를 추가 합니다. 이 수 또는 다음 단계를 유사한 되지 않을 수도 있습니다.
1. 효과 사용 하 여 기능 개체를 먼저 확인 합니다. 조건부 검사를 사용 하는 것이 좋습니다. 또는 효과 맞게 조정 하는 방법에 따라 컨트롤 명세서를 전환 합니다.

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

전체 예제 코드는 [Windows UI Github 리 포](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities)에서 찾을 수 있습니다.

## <a name="fast-vs-slow-effects"></a>슬로우 효과 및 빠른

CompositionCapabilities API에서 제공 된 [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) 및 [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) 메서드의 피드백에 따라, 응용 프로그램에서는 선택한의 최적화 된 다른 효과 대 한 비용이 많이 드는 않았거나 지원 되지 않는 효과를 결정할 수 있습니다. 디바이스. 일부 효과 일관 되 게 다른 사용자 보다 더 많은 리소스 집약적 것으로 알려진 제한적으로 사용 해야 하 고 다른 효과 더욱 자유롭게 사용할 수 있습니다. 하지만 모든 효과 주의 해야 때 사용할 연결 및 일부 시나리오 또는 조합으로 애니메이션 효과 그래프의 성능 특성을 변경할 수 있습니다. 다음은 개별 효과 대 한 몇 가지 대략 성능 특성입니다.

- 고성능 영향을 미칠 수 있는 효과 다음과 같습니다 – 가우스 흐림, 그림자 마스크, BackDropBrush, HostBackDropBrush, 및 시각적 계층입니다. 이 저사양 장치 [(기능 수준 9.1 9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)에 대 한 권장 되지 않습니다 되며 고급 장치에서 신중 하 게 사용 해야 합니다.
- 색 매트릭스를 특정 혼합 효과 BlendModes (광도, 색상, 채도 및 색조)를 포함 하는 중간 성능에 미치는 영향을 사용 하 여 효과 추천, SceneLightingEffect, 및 (시나리오에 따라) BorderEffect 합니다. 이러한 효과 저사양 장치에서 특정 시나리오를 사용 하 여 작동할 수 있지만 주의 연결 하 고 애니메이션 효과 주는 경우 사용 해야 합니다. 개 이하로 사용을 제한 하 고만 전환에 애니메이션 효과 주는 것이 좋습니다.
- 다른 모든 효과 낮은 성능 영향을 미칠 및 애니메이션을 적용 하 고 연결 하는 경우 모든 적절 한 시나리오에서 작동 합니다.

## <a name="related-articles"></a>관련 문서

- [UWP 반응 형 디자인 기술](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 장치 조정](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
