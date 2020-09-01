---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: 시각적 계층
description: Windows .xaml API는 프레임 워크 계층 (XAML)과 그래픽 계층 (DirectX) 간의 컴퍼지션 계층에 대 한 액세스를 제공 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3cdd7c17bc0d419a7449b366b620a4d5654cccc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160727"
---
# <a name="visual-layer"></a>시각적 계층

시각적 계층은 그래픽, 효과 및 애니메이션에 대 한 고성능, 유지 모드 API를 제공 하며, Windows 장치에서 모든 UI의 기반이 됩니다.선언적 방식으로 UI를 정의 하 고 시각적 계층은 그래픽 하드웨어 가속을 사용 하 여 응용 프로그램의 UI 스레드와 상관 없이 부드러운, 결함 없는 방식으로 콘텐츠, 효과 및 애니메이션이 렌더링 되도록 합니다.

주목할 만한 주요 사항은 다음과 같습니다.

* 친숙 한 WinRT Api
* 추가 동적 UI 및 상호 작용을 위한 설계
* 디자인 도구에 맞춰진 개념
* 갑작스러운 성능 절벽을 사용 하지 않는 선형 확장성

Windows UWP 앱은 UI 프레임 워크 중 하나를 통해 시각적 계층을 이미 사용 하 고 있습니다. 매우 적은 노력으로 사용자 지정 렌더링, 효과 및 애니메이션에 대 한 시각적 계층을 직접 활용할 수도 있습니다.

![UI 프레임 워크 계층화: 프레임 워크 계층 (Windows. .XAML)은 그래픽 계층 (DirectX)을 기반으로 하는 시각적 계층 (Windows. x a t e)을 기반으로 합니다.](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>시각적 계층에는 무엇이 있나요?

시각적 계층의 기본 함수는 다음과 같습니다.

1. **콘텐츠**: 사용자 지정 그리기 콘텐츠의 경량 합성
1. **효과**: 효과를 적용 하 고 연결 하 고 사용자 지정할 수 있는 실시간 UI 효과 시스템
1. **애니메이션**: 표현, UI 스레드와 독립적으로 실행 되는 프레임 워크 독립적인 애니메이션

### <a name="content"></a>Content

콘텐츠는 시각적 개체를 사용 하 여 애니메이션 및 효과 시스템에서 호스트 되 고 변환 되며 사용할 수 있게 됩니다. 클래스 계층 구조의 기본 [**클래스는 compositor**](/uwp/api/Windows.UI.Composition.Visual) 의 시각적 상태에 대 한 앱 프로세스의 경량 스레드 agile 프록시입니다. 시각적 개체의 하위 클래스에는 자식에서 콘텐츠를 포함 하 고 단색, 사용자 지정 그리기 내용 또는 시각적 효과를 사용 하 여 그릴 수 있는 시각적 개체 및 [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) 의 트리를 만들 수 있도록 하는  [**system.windows.media.containervisual>**](/uwp/api/Windows.UI.Composition.ContainerVisual) 가 포함 되어 있습니다. 이러한 시각적 개체는 2D UI에 대 한 시각적 트리 구조를 구성 하 고 가장 눈에 띄는 XAML FrameworkElements를 다시 만듭니다.

자세한 내용은 [컴퍼지션 시각적](composition-visual-tree.md) 개요를 참조 하세요.

### <a name="effects"></a>효과

시각적 계층의 효과 시스템을 사용 하면 시각적 개체 또는 시각적 개체 트리에 필터 및 투명도 효과 체인을 적용할 수 있습니다. 이는 이미지 및 미디어 효과와 혼동 하지 않는 UI 효과 시스템입니다. 효과는 애니메이션 시스템과 함께 작동 하 여 사용자가 UI 스레드와 독립적으로 렌더링 되는 효과 속성의 부드러운 동적 애니메이션을 구현할 수 있도록 합니다. 시각적 계층의 효과는 맞춤형 및 대화형 환경을 구성 하기 위해 결합 하 고 애니메이션을 적용할 수 있는 독창적인 빌딩 블록을 제공 합니다.

시각적 계층은 애니메이션 효과 효과 체인 외에도 애니메이션 효과 광원에 응답 하 여 시각적 개체에서 재질 속성을 모방 하도록 허용 하는 조명 모델을 지원 합니다. 시각적 개체는 그림자를 캐스팅할 수도 있습니다. 조명 및 그림자를 결합 하 여 깊이와 현실감의 인식을 만들 수 있습니다.

자세한 내용은 [컴퍼지션 효과](composition-effects.md) 개요를 참조 하세요.

### <a name="animations"></a>애니메이션

시각적 계층의 애니메이션 시스템을 사용 하면 시각적 개체를 이동 하 고 효과를 적용 하 고 변환, 클립 및 기타 속성을 확인할 수 있습니다.이러한 시스템은 성능 향상을 고려 하 여 처음부터 설계 된 프레임 워크와 무관 한 시스템입니다.이는 부드러운 및 확장성을 보장 하기 위해 UI 스레드와 독립적으로 실행 됩니다.친숙 한 키 프레임 애니메이션을 사용 하 여 시간에 따른 속성 변경을 구동 하는 반면, 사용자 입력을 비롯 한 다양 한 속성 간의 수학적 관계를 설정 하 여 원활한 조율 환경을 직접 만들 수 있습니다.

자세한 내용은 [컴퍼지션 애니메이션](composition-animation.md) 개요를 참조 하세요.

### <a name="working-with-your-xaml-uwp-app"></a>XAML UWP 앱 사용

XAML 프레임 워크에서 만든 시각적 개체를 가져오고 [**ElementCompositionPreview**](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 의 클래스를 사용 하 여 표시 되는 FrameworkElement를 백업할 수 [**있습니다.**](/uwp/api/Windows.UI.Xaml.Hosting) 프레임 워크에서 사용자가 만든 시각적 개체는 사용자 지정에 몇 가지 제한이 있습니다. 이는 프레임 워크가 오프셋, 변환 및 수명을 관리 하기 때문입니다. 그러나 사용자 고유의 시각적 개체를 만들어 ElementCompositionPreview를 통해 기존 XAML 요소에 연결 하거나 시각적 트리 구조의 어딘가에 있는 기존 System.windows.media.containervisual>에 추가할 수 있습니다.

자세한 내용은 [XAML을 사용 하 여 시각적 계층 사용](using-the-visual-layer-with-xaml.md) 개요를 참조 하세요.

### <a name="working-with-your-desktop-app"></a>데스크톱 앱 작업

시각적 계층을 사용 하 여 WPF, Windows Forms 및 c + + Win32 데스크톱 응용 프로그램의 모양, 느낌 및 기능을 향상 시킬 수 있습니다. 시각적 계층을 사용 하 고 나머지 UI는 기존 프레임 워크에 유지 하기 위해 콘텐츠의 아일랜드를 마이그레이션할 수 있습니다. 즉, 기존 코드베이스를 광범위하게 변경할 필요 없이 애플리케이션 UI를 획기적으로 업데이트하고 개선할 수 있습니다.

자세한 내용은 [시각적 계층을 사용하여 데스크톱 앱 현대화](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [**API에 대 한 전체 참조 설명서**](/uwp/api/Windows.UI.Composition)
* [Windowsuidevlabs GitHub](https://github.com/microsoft/WindowsCompositionSamples) 의 고급 UI 및 컴퍼지션 샘플
* [Windows. ui&gt 샘플 갤러리](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Twitter 피드 ](https://twitter.com/windowsui)
* 이 API에 대 한 Kenny Kerr의 MSDN 문서 읽기: [그래픽 및 애니메이션-Windows 컴퍼지션 넘기기 10](/archive/msdn-magazine/2015/windows-10-special-issue/graphics-and-animation-windows-composition-turns-10)