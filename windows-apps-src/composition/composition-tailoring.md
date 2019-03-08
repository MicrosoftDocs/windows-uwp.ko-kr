---
title: 컴퍼지션에 맞게 구성
description: 컴퍼지션 Api를 사용 하 여 UI를 조정, 성능 최적화를 위해 및 사용자 설정 및 장치 특성을 수용할 수 있도록 합니다.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644418"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>효과 및 Windows UI를 사용 하 여 환경을 조정 합니다.

Windows UI 차이점에 대 한 많은 멋진 효과, 애니메이션 및 수단을 제공합니다. 그러나 성능 및 사용자 지정 기능에 대 한 사용자 기대를 충족은 여전히 성공적인 응용 프로그램 만들기의 필수 구성입니다. 유니버설 Windows 플랫폼에는 대규모의 다양 한 제품군의 다양 한 기능 및 기능을 포함 하는 장치를 지원 합니다. 모든 사용자에 게에 포함 된 환경을 제공 하기 위해 장치에서 응용 프로그램 확장을 확인 하 고 사용자 기본 설정을 적용 해야 합니다. UI를 조정 하면 장치의 기능을 활용 하 고 편리한 및 포괄 사용자 환경을 보장 하는 효율적인 방법을 제공할 수 있습니다.

UI 작업은 성능이 다음과 관련 하 여 멋진 UI에 대 한 작업을 포괄 하는 광범위 한 범주:

- 존중 및 인정 효과 대 한 사용자 설정 조정
- 애니메이션에 대 한 사용자 설정을 수용
- 특정된 하드웨어 기능에 대 한 UI를 최적화

여기서은 효과 시각적 계층 위의 영역에서 사용 하 여 애니메이션을 조정 하는 방법을 다룰 것도 있습니다 뛰어난 최종 사용자 환경을 보장 하려면 응용 프로그램에 맞게 다른 여러 방법. 지침 문서를 제공 하는 방법은 [UI를 조정할](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) 다양 한 장치에 대 한 및 [응답성이 뛰어난 UI를 만드는](/windows/uwp/design/layout/responsive-design)합니다.

## <a name="user-effects-settings"></a>사용자 설정 효과

사용자가 다양 한 이유로 응용 프로그램에서는 준수를 적용할 수 있는 해당 Windows 환경을 사용자 지정할 수 있습니다. 최종 사용자가 제어할 수 있습니다 하나 영역이 시스템 전체에서 사용 되는 효과 표시 유형을 변경 됩니다.

### <a name="transparency-effects-settings"></a>투명 효과 설정

켜기/끄기 투명 효과 회전 하는 이러한 적용 설정 중 하나는 사용자 지정 합니다. 개인 설정 아래에서 [설정] 앱에서 찾을 수 있습니다 > 색 또는 설정 앱을 통해 > 접근성 > 표시 합니다.

![투명도 옵션 설정](images/tailoring-transparency-setting.png)

켜져 있을 때 투명도 사용 하는 영향을 주지는 예상 대로 표시 됩니다. 이 못합니다, HostBackdropBrush, 또는 완전 불투명 하지 않은 모든 사용자 지정 효과 그래프에 적용 됩니다.

해제 된 경우 아크릴 자료 자동으로 대체 됩니다 단색 XAML의 아크릴 브러시에 기본적으로이 이벤트를 수신 대기 하기 때문에. 여기에 적절 하 게 대체 단색 투명 효과나 설정 되어 있지 않으면 계산기 앱이 표시 됩니다.

![못합니다를 사용 하 여 계산기](images/tailoring-acrylic.png)
![못합니다 투명도 설정에 응답을 사용 하 여 계산기](images/tailoring-acrylic-fallback.png)

그러나 사용자 지정 효과 응용 프로그램에 대 한에 응답 해야 합니다 [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) 속성 또는 [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) 이벤트 및 스위치 아웃 효과/효과 투명도 없이 있는 효과 사용 하는 그래프입니다. 이 예는 다음과 같습니다.

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

응용 프로그램은 수신 대기 하 고 응답할 마찬가지로 합니다 [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) 애니메이션 되도록 속성을 설정 하거나 해제 설정에서 사용자 설정에 따라 > 접근성 > 표시 합니다.

![설정에서 애니메이션 옵션](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>기능 API

활용 하 여 합니다 [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) Api는 컴퍼지션 경우 특정 하드웨어에서 사용할 수 있습니다 하 고 성능이 뛰어난 기능 감지를 위해 최종 사용자가 멋진 경험을 고성능에서 디자인을 조정 장치입니다. Api는 다양 한 폼 요소 크기 조정 하는 정상적인 효과 구현 하기 위해 하드웨어 시스템 기능을 확인 하는 방법을 제공 합니다. 이 맞게 적절 하 게 멋진를 만들려는 응용 프로그램 및 원활 하 게 최종 사용자 환경을 쉽게 있습니다.

이 API는 메서드와 UI 응용 프로그램에 대 한 결정을 확장 하는 효과 사용할 수 있는 이벤트 수신기를 제공 합니다. 기능 복잡 한 컴퍼지션 및 렌더링 작업을 시스템이 처리할 수는 얼마나 잘 감지 하 고 활용 하는 개발자를 위한 사용이 간편한 모델에서 정보를 반환 합니다.

### <a name="using-composition-capabilities"></a>컴퍼지션 기능을 사용 하 여

CompositionCapabilities 기능이 이미 있는 자료 대체 시나리오 및 하드웨어에 따라 자세한 성능이 뛰어난 효과를 아크릴 자료와 같은 기능에 대 한 활용 되 고 있습니다.

API는 몇 가지 간단한 단계로의 기존 코드를 추가할 수 있습니다.

1. 응용 프로그램의 생성자에서 기능 개체를 획득 합니다.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 앱에 대 한 기능 변경된 이벤트 수신기를 등록 합니다.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 콘텐츠를 다양 한 기능 수준을 처리 하는 이벤트 콜백 메서드를 추가 합니다. 이 있고 아래 다음 단계와 마찬가지로 되지 않을 수 있습니다.
1. 효과 사용 하는 경우 먼저 기능 개체를 확인 합니다. 조건부 검사를 사용 하는 것이 좋습니다 또는 제어문 효과 맞게 원하는 방법에 따라 전환 합니다.

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

전체 예제 코드에서 확인할 수 있습니다 합니다 [Windows UI Github 리포지토리](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities)합니다.

## <a name="fast-vs-slow-effects"></a>느린 효과 빠른 비교

제공 된 피드백에 따라 [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) 하 고 [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) CompositionCapabilities api에서 메서드, 응용 프로그램에 대 한 비용이 많이 드는 또는 지원 되지 않는 효과 교환 하려면 결정할 수 있습니다 기타 효과 자신이 선택한 장치에 최적화 된 합니다. 몇 가지 다른 항목 보다 더 많은 리소스 집약적 일관 되 게 것으로 알려져 제한적으로 쓰일 수 및 기타 효과 더 자유롭게 사용할 수 있습니다. 그러나 모든 효과 대해 주의 때 사용할 연결 하 고 몇 가지 시나리오 또는 조합으로 애니메이션 효과 그래프의 성능 특성을 변경 될 수 있습니다. 다음은 개별 효과 대 한 몇 가지 경험에 따르면 성능 특징:

- 효과 높은 성능에 영향으로 알려져 되며 다음과 같이 – 가우스 흐리게 표시, 섀도 마스크, BackDropBrush, HostBackDropBrush를 시각적 계층입니다. 이러한 최소값 장치에 대 한 권장 되지 않습니다 [(기능 수준 9.1-9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx), 하이엔드 장치의 주의 해 서 사용 해야 합니다.
- 색 매트릭스를 특정 Blend 효과 BlendModes (명도, 색상, 채도 및 Hue)를 포함 하는 중간 성능에 영향을 효과 추천, SceneLightingEffect, 및 (시나리오에 따라) BorderEffect 합니다. 이러한 효과 최소값 장치의 특정 시나리오에 사용할 수 있지만 연결 하 고 애니메이션 효과 적용 하는 경우 주의 사용 해야 합니다. 두 개 이하로 제한 하는 사용 하 고만 전환에 애니메이션을 적용 하는 것이 좋습니다.
- 다른 모든 작업이 낮은 성능에 영향 및 애니메이션 및 연결 하는 경우 모든 적절 한 시나리오에서 작동 합니다.

## <a name="related-articles"></a>관련 문서

- [UWP 반응 형 디자인 기술](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 장치 구성](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
