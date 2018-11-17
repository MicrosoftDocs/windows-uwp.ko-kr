---
description: 흐름 디자인은 무엇이고, 어떻게 앱에 통합하는지 알아봅니다.
title: Windows 용 fluent 디자인 시스템
author: mijacobs
keywords: uwp 앱 레이아웃, 유니버설 windows 플랫폼, 앱 디자인, 인터페이스, 흐름 디자인 시스템
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c61eb71a82234a1339295536140121d80f83a033
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166567"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Fluent 디자인 시스템에 대 한 Windows 앱 작성자

![Fluent 디자인 헤더](images/fluentdesign-app-header.jpg)

## <a name="introduction"></a>소개

Fluent 디자인 시스템에는 적응형 공감 다우 며 멋진 사용자 인터페이스는 시스템 이기도 합니다.

## <a name="principles"></a>원칙

**적응형: 각 장치마다 Fluent 환경을 자연스럽게 느낄 수 있다는 것을 말합니다.**

이러한 Fluent 환경이 각 장치의 환경에 적용됩니다. Fluent 환경은 태블릿, 데스크톱 PC 및 Xbox에서 편안한 느낌-도 작동 제대로 혼합 현실 헤드셋에서. PC 모니터 같은 하드웨어를 추가하면 Fluent 환경이 추가된 하드웨어를 이용합니다.

**공감: Fluent 환경은 직관적이고 강력합니다.**

Fluent 환경은 동작이나 의도에 반응하여 필요한 것을 이해하고 예상합니다. 지구 반대편에 있는 사람이든, 혹은 지금 바로 옆에 있는 사람이든 상관없이 모든 사람과 아이디어를 하나로 결합합니다.

**아름다움: Fluent 환경은 매력적이고 몰입도가 높습니다.**

Fluent 환경은 실제 세상의 요소를 통합하여 근본적인 것을 활용합니다. 조명, 그림자, 동작, 깊이, 질감 등을 사용하여 직관적이고 본능적으로 느낄 수 있도록 정보를 구성합니다.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>UWP 사용 하 여 앱에 흐름 디자인을 적용

![흐름 디자인 로고](images/fluentdesign_header.png)

디자인 지침에는 앱에 흐름 디자인 원칙을 적용 하는 방법을 설명 합니다. 어떤 유형의 앱? 모든 플랫폼에 적용할 수 다양 한 지침을 하는 동안 UWP (유니버설 Windows 플랫폼) 흐름 디자인을 지원 하기 위해 만들었습니다.

흐름 디자인은 UWP에 기본적으로 적용됩니다. 이러한 기능 중에서 유효 픽셀이나 범용 입력 시스템 같은 일부 기능은 자동입니다. 이 기능을 활용하기 위해 추가 코드를 작성할 필요가 없습니다. 아크릴 같은 다른 기능은 선택 사항입니다. 즉 추가할 코드를 직접 작성하여 앱에 추가하면 됩니다.

> 기존 WPF 또는 흐름 디자인 기능이 있는 Windows 응용 프로그램의 모양과 느낌, 기능을 향상시킬 수 있도록 데스크톱에 UWP 컨트롤을 포함했습니다. 자세한 내용은 [WPF 및 Windows Forms 응용 프로그램의 호스트 UWP 컨트롤](/windows/uwp/xaml-platform/xaml-host-controls)을 참조 하세요.

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

디자인 지침 뿐만 아니라 Fluent 디자인 기사도 보여 줍니다 발생할 디자인 하는 코드를 작성 하는 방법. UWP XAML 사용자 인터페이스를 만드는 쉽게 태그 기반 언어를 사용 합니다. 예를 들면 다음과 같습니다.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="24,16"/>
</Grid>
```

![](images/xaml-example.png)


> UWP 개발을 처음 접하는 경우 확인 하는 [UWP 페이지 시작](https://developer.microsoft.com/windows/apps/getstarted)하세요.

## <a name="find-a-natural-fit"></a>자연스러운 경험 찾기

다양한 장치에서 앱이 자연스럽게 보이도록 하려면 어떻게 해야 할까요? 처음부터 특정 장치를 염두에 두고 설계한 것처럼 느끼게 하면 됩니다. 화면 크기에 따라 조정되어 낭비하는 공간이 없고(혼잡하지도 않은) UI 레이아웃에서는 마치 특정 장치를 위해 설계된 것처럼 자연스러운 경험을 느낄 수 있습니다.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>직관적인 확인

직관적인 경험은 사용자가 기대 하는 방식으로 동작 하는 경우. 구성된 컨트롤 및 패턴을 사용하는 동시에 접근성과 세계화를 지원하는 플랫폼 기능을 이용할 때 비로소 사용자 생산성을 손쉽게 높일 수 있는 환경이 구현됩니다.

공감대의 표현은 적절한 시점에 올바른 일을 하는 것입니다.

Fluent 환경은 컨트롤과 패턴을 일관적으로 사용하기 때문에 사용자가 예상하는 대로 동작합니다. 또한 광범위한 실제 권한이 있는 사용자라면 누구나 액세스가 가능할 뿐만 아니라 세계화 기능까지 결합되어 전 세계 누구나 사용할 수 있습니다.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>매력적이고 몰입도 높은 환경

흐름 디자인은 현란한 효과에 관한 것이 아닙니다. 사람의 뇌가 정보를 효율적으로 처리할 수 있도록 프로그래밍되는 것처럼 경험을 에뮬레이션하기 때문에 실제로 사용자 경험을 강화할 수 있는 효과가 있습니다.

## <a name="use-light"></a>조명 사용

조명은 이목을 집중시키는 방법 중 하나입니다. 또한 분위기와 공간감을 형성하고 정보를 강조하는 실용적인 도구입니다.

조명을 UWP 앱에 추가:

:::row:::
    :::column:::
        ![fpo image](../style/images/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](../style/reveal.md) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](../style/images/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](../style/reveal-focus.md) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>깊이감 추가하기

우리는 3차원 세계에 살고 있습니다. UI에 의도적으로 깊이를 적용함으로써 평면적인 2차원 인터페이스를 입체적인 형태, 즉 시각적 계층 구조를 형성하여 효율적으로 정보와 개념을 제공하는 형태로 바꿉니다. 계층화된 물리적 환경 내에서 사물이 서로 관계 맺는 방식을 새롭게 창조할 수 있습니다.

깊이를 UWP 앱에 추가:

:::row:::
    :::column:::
        ![fpo image](../motion/images/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](../motion/parallax.md) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>동작 통합

동영상 같은 동작 디자인이라고 볼 수 있습니다. 매끄러운 전환은 스토리에 대한 집중도를 높이고 생동감 있는 경험을 선사합니다. 이런 느낌을 디자인에 구현하여, 영화를 보듯 편안하게 여러 작업을 이어갈 수 있도록 했습니다.

동작을 UWP 앱에 추가:

:::row:::
    :::column:::
        ![continuity gif](images/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](../motion/connected-animation.md) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>올바른 재료를 사용하는 디자인

현실 세계에서 우리를 둘러싼 사물은 감각적이고 생기가 있습니다. 구부러지고, 늘어나고, 튀어 오르고, 부서지고, 미끄러집니다. 이러한 사물의 속성을 디지털 환경에 구현하여, 사람들이 디자인에 손을 뻗어 만지고 싶어하도록 만듭니다.

재료를 UWP 앱에 추가:

:::row:::
    :::column:::
        ![fpo image](../style/images/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](../style/acrylic.md) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>디자인 도구 키트 및 코드 샘플

흐름 디자인을 사용해 나만의 앱을 만들어보고 싶으세요? Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer, Sketch 등의 도구 키트가 디자인을 바로 시작할 수 있도록 지원할 뿐만 아니라 코드 샘플은 빠른 코딩에 매우 유용합니다.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









