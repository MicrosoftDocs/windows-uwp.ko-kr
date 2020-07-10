---
description: Fluent 디자인 및 이를 앱에 통합하는 방법을 알아봅니다.
title: Windows용 Fluent 디자인 시스템
keywords: UWP 앱 레이아웃, 유니버설 Windows 플랫폼, 앱 디자인, 인터페이스, Fluent 디자인 시스템
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 7d340b3db12aef964a65860855acc0f0722cda64
ms.sourcegitcommit: 6cdba316bdbd85a2429259ebfb59ff94440e234a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85882935"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Windows 앱 작성자용 Fluent 디자인 시스템

![Fluent 디자인 헤더](images/fluent/fluentdesign-app-header.png)

## <a name="introduction"></a>소개

Fluent 디자인 시스템은 아름답고 공감할 수 있는 적응형 사용자 인터페이스를 만드는 시스템입니다.

## <a name="principles"></a>원칙

**적응형: 각 디바이스에서 자연스럽게 느낄 수 있는 Fluent 환경**

Fluent 환경은 환경에 맞게 조정됩니다. Fluent 환경은 태블릿, 데스크톱 PC 및 Xbox에서 편안함을 느낄 수 있을 뿐만 아니라 혼합 현실 헤드셋에도 훌륭하게 작동합니다. PC용 추가 모니터와 같은 하드웨어가 추가되면 Fluent 환경에서 이 하드웨어를 활용합니다.

**공감: 직관적이고 강력한 Fluent 환경**

Fluent 환경은 동작이나 의도에 맞게 조정되어 요구 사항을 인식하고 예상합니다. 지구의 반대편에 있거나 지금 바로 옆에 있는 사람인지 여부에 관계없이 모든 사람과 아이디어를 하나로 결합합니다.

**아름다움: 매력적이고 몰입도가 높은 Fluent 환경**

Fluent 환경은 실제 세계의 요소를 통합하여 기본 작업을 활용합니다. 조명, 그림자, 움직임, 깊이 및 질감을 사용하여 직관적이고 본능적으로 느낄 수 있는 방식으로 정보를 구성합니다.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>UWP를 사용하여 앱에 Fluent 디자인 적용

![Fluent 디자인 로고](images/fluent/fluentdesign_header.png)

디자인 지침에서는 Fluent 디자인 원칙을 앱에 적용하는 방법을 설명합니다. 어떤 종류의 앱인가요? 지침의 많은 부분을 모든 플랫폼에 적용할 수 있지만, Fluent 디자인을 지원하는 UWP(유니버설 Windows 플랫폼)를 만들었습니다.

Fluent 디자인 기능은 UWP에 기본적으로 제공됩니다. 이러한 기능 중 일부(예: 유효 픽셀 또는 범용 입력 시스템)는 자동입니다. 해당 기능을 활용하기 위해 추가 코드를 작성할 필요가 없습니다. 아크릴과 같은 다른 기능은 선택 사항입니다. 이 경우 코드를 작성하여 앱에 추가합니다.

> Fluent 디자인 기능을 사용하여 기존 WPF 또는 Windows 애플리케이션의 모양, 느낌 및 기능을 향상시킬 수 있도록 UWP 컨트롤을 데스크톱에 가져옵니다. 자세한 내용은 [WPF 및 Windows Forms 애플리케이션에서 UWP 컨트롤 호스팅](/windows/uwp/xaml-platform/xaml-host-controls)을 참조하세요.

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

Fluent 디자인 문서에는 디자인 지침 외에도 디자인을 수행하는 코드를 작성하는 방법이 나와 있습니다. UWP는 사용자 인터페이스를 더 쉽게 만들 수 있는 표시(markup) 기반 언어인 XAML을 사용합니다. 예를 들면 다음과 같습니다.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![XAML 예제](images/fluent/xaml-example.png)


> UWP 개발을 처음 접하는 경우 [UWP 시작 페이지](/windows/uwp/get-started)를 확인하세요.

## <a name="find-a-natural-fit"></a>자연스러운 맞춤형 환경 확인

다양한 디바이스에서 앱을 자연스럽게 느끼도록 하려면 어떻게 해야 할까요? 각각의 특정 디바이스를 염두에 두고 설계한 것처럼 느끼게 하는 것입니다. 다양한 화면 크기에 맞게 조정되는 UI 레이아웃은 낭비되는 공간이 없으므로(혼잡하지도 않음) 마치 해당 디바이스를 위해 설계된 것처럼 자연스러운 환경을 느낄 수 있습니다.

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**적합한 중단점에 따른 디자인**

개별 화면 크기에 맞춰 설계하지 않고 몇 가지 주요 너비(“중단점”이라고도 함)를 중심으로 설계할 경우 디자인 및 코드가 크게 간소화될 뿐만 아니라 소형부터 대형 화면에서도 앱을 제대로 표시할 수 있습니다.

[화면 크기 및 중단점에 대한 알아보기](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**반응형 레이아웃 만들기**

앱을 자연스럽게 보이게 하려면 다양한 화면 크기와 디바이스로 레이아웃을 조정해야 합니다. XAML에서 자동 크기 조정, 레이아웃 패널, 시각적 상태 및 별도의 UI 정의를 사용하여 반응형 UI를 만들 수 있습니다.

[반응형 디자인 알아보기](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**디바이스 스펙트럼에 대한 디자인**

UWP 앱은 광범위한 Windows 기반 디바이스에서 실행될 수 있습니다. 어떤 디바이스를 사용할 수 있는지, 개발 이유가 무엇인지, 그리고 사용자와 앱이 어떻게 상호 작용하는지에 대해 이해하는 데 유용합니다.

[UWP 디바이스에 대해 알아보기](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**적절한 입력에 최적화**

UWP 앱은 일반 마우스, 키보드, 펜 및 터치 조작을 자동으로 지원합니다. 사용자가 따로 해야 할 것이 없습니다. 하지만 펜이나 Surface Dial 같은 특정 입력에 최적화된 지원 방식을 통해 앱을 개선할 수 있습니다.

[입력 및 상호 작용에 대해 알아보기](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>직관적 환경 만들기

사용자가 요구하는 방식으로 작동할 때 직관적인 환경이라고 느낍니다. 설정된 컨트롤과 패턴을 사용하고 접근성과 세계화를 지원하는 플랫폼을 활용하면 사용자가 생산성을 높일 수 있는 손쉬운 환경을 만들 수 있습니다.

적절한 시기에 올바른 작업을 수행할 때 공감을 느끼게 됩니다.

Fluent 환경은 컨트롤과 패턴을 일관적으로 사용하므로 사용자가 예상한 방식으로 작동합니다. Fluent 환경은 광범위한 실제 권한이 있는 사용자가 액세스할 수 있으며, 전 세계 사람들이 사용할 수 있도록 세계화 기능을 통합합니다.

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**적합한 탐색 제공**

올바른 앱 구조 및 탐색 구성 요소를 사용하여 간편한 환경을 만듭니다.

[탐색에 대한 자세한 정보](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**대화형**

사용자가 앱과 상호 작용할 수 있는 단추, 명령 모음, 바로 가기 키 및 상황에 맞는 메뉴는 정적 환경을 동적으로 변경하는 도구입니다.

[명령에 대해 알아보기](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**적합한 작업 컨트롤 사용**

컨트롤은 사용자 인터페이스의 구성 요소입니다. 따라서 적합한 컨트롤을 사용할 경우 사용자가 기대하는 대로 작동하는 사용자 인터페이스를 만들 수 있습니다. UWP는 간단한 단추부터 강력한 데이터 제어 기능에 이르기까지 45가지가 넘는 컨트롤을 제공합니다.

[UWP 컨트롤 알아보기](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![포함 이미지](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**포괄적** 올바르게 디자인된 앱은 장애인들에게도 접근성을 보장합니다. 일부 코딩만 추가하면 자신의 앱을 전 세계 사용자와 서로 공유할 수 있습니다.

[유용성 알아보기](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>매력적이고 몰입도가 높은 환경

Fluent 디자인은 화려한 효과와 관련된 것이 아닙니다. 사람의 뇌가 정보를 효율적으로 처리할 수 있도록 프로그래밍된 경험을 모방하므로 실제로 사용자 환경을 향상시키는 물리적 효과를 통합합니다.

## <a name="use-light"></a>광선 사용

광선은 주의를 끌 수 있는 한 가지 방법입니다. 분위기와 공간 감성을 창출하고 정보를 조명하는 실용적인 도구입니다.

광선을 UWP 앱에 추가하는 방법은 다음과 같습니다.

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**강조 표시**

[강조 표시](/windows/uwp/design/style/reveal)를 사용하여 대화형 요소가 돋보이도록 합니다. 빛으로 숨겨진 테두리를 표시함으로써 사용자가 상호 작용할 수 있는 요소를 비춥니다. 목록 보기나 그리드 보기 같은 일부 컨트롤에서는 표시가 자동으로 활성화됩니다. 다른 컨트롤에서는 사전 설정된 강조 표시 스타일을 적용하여 활성화할 수 있습니다.
:::row-end:::

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**포커스 표시**

[포커스 표시](/windows/uwp/design/style/reveal-focus)는 조명을 사용하여 현재 포커스가 맞춰진 요소에 집중할 수 있도록 합니다.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>깊이 감각 만들기

현재 3차원 세계에 살고 있습니다. 의도적으로 깊이를 UI에 적용하여 평면적인 2차원 인터페이스를 더 효율적인 것으로 변환합니다. 즉 시각적 계층 구조를 만들어 정보와 개념을 효율적으로 나타냅니다. 계층화된 물리적 환경 내에서 사물이 서로 관계되는 방식을 새롭게 창조할 수 있습니다.

깊이를 UWP 앱에 추가하는 방법은 다음과 같습니다.

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**시차**

[시차](/windows/uwp/design/motion/parallax)는 전경의 항목이 배경의 항목보다 빠르게 이동하는 것처럼 보이도록 하여 깊이감을 형성합니다.
:::row-end:::

## <a name="incorporate-motion"></a>움직임 통합

움직임 디자인을 영화처럼 생각할 수 있습니다. 매끄러운 전환은 스토리에 대한 집중도를 높이고 생동감 있는 경험을 선사합니다. 이러한 느낌을 디자인에 도입하여 영화처럼 편리하게 한 작업에서 다음 작업으로 이어갈 수 있습니다.

움직임을 UWP 앱에 추가하는 방법은 다음과 같습니다.

:::row:::
    :::column:::
        ![연속성 gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**연결된 애니메이션**

[연결된 애니메이션](/windows/uwp/design/motion/connected-animation)은 장면들이 매끄럽게 전환되도록 하여, 사용자가 맥락을 이해하는 데 도움을 줍니다.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>올바른 소재를 사용하여 빌드

실제 세계에서 둘러싸고 있는 사물은 감각적이고 활력을 줍니다. 즉 구부러지고, 늘어나고, 튀어 오르고, 부서지고, 미끄러집니다. 이러한 물질적 특성은 디지털 환경으로 변환되어 사람들이 디자인에 손을 뻗고 만질 수 있게 합니다.

소재를 UWP 앱에 추가하는 방법은 다음과 같습니다.

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**아크릴**

[아크릴](/windows/uwp/design/style/acrylic)은 반투명한 재질로, 사용자가 콘텐츠 레이어를 확인하고 UI 요소의 계층 구조를 설정할 수 있습니다.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>디자인 도구 키트 및 코드 샘플

Fluent 디자인을 사용하여 나만의 앱을 만들려고 하는가요? Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer 및 Sketch용 도구 키트는 디자인을 바로 시작하는 데 도움이 되며, 코드 샘플은 빠른 코딩에 유용합니다.

:::row:::
    :::column:::
        ![fpo 이미지](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**디자인 도구 키트 및 샘플 페이지**

자세한 내용은 [디자인 도구 키트 및 샘플 페이지](/windows/uwp/design/downloads/)를 참조하세요.
:::row-end:::









