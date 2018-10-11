---
author: mijacobs
Description: The universal design features included in every UWP app help you build apps that scale beautifully across a range of devices.
title: UWP(유니버설 Windows 플랫폼) 앱 디자인 소개(Windows 앱)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: mijacobs
ms.date: 05/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 602a0af685e812f5c65f94d07297cac9fc411923
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4533694"
---
# <a name="introduction-to-uwp-app-design"></a>UWP 앱 디자인 소개

![샘플 조명 앱](images/introUWP-header.jpg)

유니버설 Windows 플랫폼(UWP) 디자인 지침은 세련되고 아름다운 앱을 디자인 및 빌드하기 위한 리소스입니다.

규범적인 규칙 목록이 아닙니다. 발전하는 [흐름 디자인 시스템](../fluent-design-system/index.md)과 앱 빌드 커뮤니티의 필요 사항에 맞춰 적응할 수 있도록 고안된 살아있는 문서입니다.

소개에서는 모든 UWP 앱에 포함된 유니버설 디자인 기능에 대한 개요 및 여러 장치에 걸쳐 확장되는 UI(사용자 인터페이스)를 빌드하기 위해 문서를 사용할 수 있는 방법을 설명합니다.

## <a name="effective-pixels-and-scaling"></a>유효 픽셀 및 크기 조정

UWP 앱은 TV에서 태블릿 또는 PC에서 모든 [Windows 10 장치](../devices/index.md)에서 실행 합니다. 다양 한 장치 및 화면 크기에서 멋지게는 UI를 디자인 하는 어떻게 수 있나요?

![다양한 디바이스에서 동일한 앱](images/universal-image-1.jpg)

UWP는 쉽게 읽고 모든 장치와 화면 크기에서 상호 작용할 수 있도록 UI 요소를 자동으로 조정 하 여는 데 도움이 됩니다.

디바이스에서 앱이 실행될 때 시스템은 화면에 UI 요소가 표시되는 방식을 정규화하는 알고리즘을 사용합니다. 이 크기 조정 알고리즘은 가시거리 및 화면 밀도(인치당 픽셀)를 고려하여 물리적 크기가 아닌 인식되는 크기를 최적화합니다. 크기 조정 알고리즘에 따르면 3m 떨어져 있는 Surface Hub의 24픽셀 글꼴은 몇 인치 떨어져 있는 5인치 휴대폰의 24픽셀 글꼴과 마찬가지로 읽힙니다.

![다양한 디바이스의 가시거리](images/scaling-chart.png)

크기 조정 시스템의 작동 방식 때문에 UWP 앱을 디자인할 때 실제 물리적 픽셀이 아닌 유효 픽셀로 디자인하게 됩니다. 유효 픽셀(epx)은 가상 측정 단위이며, 화면 밀도와 무관하게 레이아웃 크기와 간격을 표시하는 데 사용됩니다. (지침에서는 epx, ep, px를 번갈아 사용합니다.).

디자인할 때는 픽셀 밀도 및 실제 화면 해상도를 무시해도 됩니다. 대신, 크기 클래스에 대한 유효 해상도(유효 픽셀로 나타낸 해상도)를 고려해서 디자인합니다(자세한 내용은 [화면 크기 및 중단점 문서](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) 참조).

> [!TIP]
> 이미지 편집 프로그램에서 화면 모형을 만드는 경우 DPI를 72로 설정하고 이미지 크기를 목표로 하는 크기 클래스의 유효 해상도로 설정하세요. 크기 클래스 및 유효 해상도 목록은 [화면 크기 및 중단점 문서](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)를 참조하세요.

### <a name="multiples-of-four"></a>4의 배수씩 확장

:::row:::
    :::column span:::
        The sizes, margins, and positions of UI elements should always be in **multiples of 4 epx** in your UWP apps.

        UWP scales across a range of devices with scaling plateaus of 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350%, and 400%. The base unit is 4 because it's the only integer that can be scaled by non-whole numbers (e.g. 4*1.5 = 6). Using multiples of four aligns all UI elements with whole pixels and ensures UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>레이아웃

UWP 앱은 모든 장치에 자동으로 조정되므로 모든 장치에 대한 UWP 앱 디자인은 같은 구조를 가집니다. UWP 앱의 UI의 처음부터 시작해 봅시다.

### <a name="windows-frames-and-pages"></a>창, 프레임 및 페이지

:::row:::
    :::column:::
        When a UWP app is launched on any Windows 10 device, it launches in a [Window](/uwp/api/Windows.UI.Xaml.Controls.Window) with a [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame), which can navigate between [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) instances.
    :::column-end:::
    :::column:::
        ![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        You can think of your app's UI as a collection of pages. It's up to you to decide what should go on each page, and the relationships between pages.

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>페이지 레이아웃

페이지는 어떤 모습이어야 하나요? 대부분의 페이지는 앱의 페이지 내에서 사용자가 쉽게 이동할 수 있도록 일관성을 제공하기 위해 공통 구조를 따릅니다. 페이지에는 일반적으로 세 가지 유형의 UI 요소가 포함됩니다.

- [탐색](navigation-basics.md) 요소는 사용자가 표시하려는 콘텐츠를 선택하는 데 도움을 줍니다.
- [명령](commanding-basics.md) 요소는 조작, 저장, 콘텐츠 공유 등 작업을 시작합니다.
- [콘텐츠](content-basics.md) 요소는 앱의 콘텐츠를 표시합니다.

![일반적인 레이아웃 패턴](../layout/images/page-components.svg)

일반적인 UWP 앱 패턴을 구현하는 방법에 대한 자세한 내용은 [페이지 레이아웃](../layout/page-layout.md) 문서를 참조하세요.

Visual Studio에서 [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master)를 사용하여 앱의 레이아웃을 시작할 수도 있습니다.

## <a name="controls"></a>컨트롤

UWP의 디자인 플랫폼은 모든 Windows 기반 장치에서 제대로 작동하도록 보장되고 [흐름 디자인 시스템](../fluent-design-system/index.md) 원칙을 준수하는 공용 컨트롤의 집합을 제공합니다. 이러한 컨트롤에는 단추와 텍스트 요소 같은 단순한 컨트롤부터 데이터 및 템플릿 세트에서 목록을 생성할 수 있는 정교한 컨트롤까지 모든 것이 포함되어 있습니다.

![UWP 컨트롤](../style/images/color/windows-controls.svg)

여기에서 만들 수 있는 UWP 컨트롤과 패턴의 전체 목록은 [컨트롤 및 패턴 섹션](../controls-and-patterns/index.md)을 참조하세요.

## <a name="style"></a>스타일

공용 컨트롤은 자동으로 시스템 테마와 테마 컬러를 선택하고, 모든 입력 유형을 지원하며, 모든 장치로 확장이 됩니다. 공감하는 아름다운 적응형 공용 컨트롤은 그런 방식으로 흐름 디자인 시스템을 반영합니다. 공용 컨트롤은 기본 스타일에서 빛, 움직임, 깊이를 사용하므로 이를 사용하여 앱에 흐름 디자인 시스템을 통합합니다.

공용 컨트롤은 다양하게 사용자 지정할 수 있습니다. 예를 들면, 컨트롤의 전경색을 변경하거나, 모양을 완전히 사용자 지정할 수 있습니다. 컨트롤에서 기본 스타일을 재정의하려면 [경량 스타일](../controls-and-patterns/xaml-styles.md#lightweight-styling)을 사용하거나 XAML에서 [사용자 지정 컨트롤](../controls-and-patterns/control-templates.md)을 만듭니다.

![테마 컬러 gif](images/intro-style.gif)

## <a name="shell"></a>셸

:::row:::
    :::column:::
        Your UWP app will interact with the broader Windows experience with tiles and notifications in the Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md).

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>입력

:::row:::
    :::column:::
        UWP apps rely on smart interactions. You can design around a click interaction without having to know or define whether the click comes from a mouse, a stylus, or a tap of a finger. However, you can also design your apps for [specific input modes](../input/input-primer.md).
    :::column-end:::
    :::column:::
        ![inputs](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>장치

![장치](../layout/images/size-classes.svg)

마찬가지로, UWP는 자동으로 앱을 다른 디바이스로 확장하지만 [특정 디바이스에 대한 UWP 앱을 최적화](../devices/index.md)할 수도 있습니다.

## <a name="usability"></a>사용 편의성

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

마지막이지만 아주 중요한 사용 편의성은 앱의 경험을 모든 사용자에게 개방적으로 만드는 것이 중요합니다. 모든 사용자가 포괄적인 사용자 경험에서 혜택을 누릴 수 있습니다. 모든 사용자가 앱을 쉽게 사용할 수 있도록 만드는 방법에 대한 자세한 내용은 [UWP 앱의 유용성](../usability/index.md)을 참조하세요.

국제 고객을 대상으로 설계하는 경우 [세계화 및 지역화](../globalizing/globalizing-portal.md)를 참조하세요.

시각, 청각, 이동성에 제약이 있는 사용자를 위한 [접근성 기능](../accessibility/accessibility-overview.md) 또한 고려하는 것이 좋습니다. 처음부터 접근성을 디자인에 포함시키면, 더 적은 노력과 시간으로 [쉽게 사용할 수 있는 앱을](../accessibility/accessibility-in-the-store.md) 만들 수 있습니다.

## <a name="tools-and-design-toolkits"></a>도구 및 디자인 키트

기본적인 디자인 기능에 대해 알아봤습니다. 이제 자신의 UWP 앱 디자인을 시작해 보세요!

디자인 프로세스에 도움을 주는 다양한 도구들을 제공하고 있습니다.

- XD, Illustrator, Photoshop, Framer, Sketch와 추가 디자인 도구 및 글꼴 다운로드는 [디자인 도구 키트 페이지](../downloads/index.md)를 참조하세요.

- UWP 앱을 실제로 코딩하도록 컴퓨터를 설정하는 방법은 [시작 &gt; 설정](../../get-started/get-set-up.md) 문서를 참조하세요.

- UWP용 UI 구현 방법에 대한 영감을 얻고 싶다면 종합적인 [샘플 UWP 앱](https://developer.microsoft.com/windows/samples)을 조사해 보세요.

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>다음: 흐름 디자인 시스템

흐름 디자인(Microsoft의 디자인 시스템)의 기반이 되는 원칙에 대해 학습하고, UWP 앱에 포함시킬 수 있는 더 많은 기능에 대해 알아보고 싶다면 계속해서 [흐름 디자인 시스템](../fluent-design-system/index.md)에 대해 알아보세요.

## <a name="related-articles"></a>관련 문서

- [UWP 앱이란 무엇인가요?](../../get-started/universal-application-platform-guide.md)
- [흐름 디자인 시스템](../fluent-design-system/index.md)
- [XAML 플랫폼 개요](../../xaml-platform/index.md)
