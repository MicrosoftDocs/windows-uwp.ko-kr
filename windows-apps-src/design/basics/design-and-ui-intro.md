---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: UWP(유니버설 Windows 플랫폼) 앱 디자인 소개(Windows 앱)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817281"
---
#  <a name="introduction-to-uwp-app-design"></a>UWP 앱 디자인 소개

유니버설 Windows 플랫폼(UWP) 디자인 지침은 세련되고 아름다운 앱을 디자인 및 빌드하기 위한 리소스입니다.

규범적인 규칙 목록이 아닙니다. 발전하는 [흐름 디자인 시스템](../fluent-design-system/index.md)과 앱 빌드 커뮤니티의 필요 사항에 맞춰 적응할 수 있도록 고안된 살아있는 문서입니다.

소개에서는 모든 UWP 앱에 포함된 유니버설 디자인 기능에 대한 개요 및 여러 장치에 걸쳐 확장되는 UI(사용자 인터페이스)를 빌드하기 위해 문서를 사용할 수 있는 방법을 설명합니다.

## <a name="video-summary"></a>비디오 요약

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>유효 픽셀 및 크기 조정

UWP 앱은 [UWP 앱을 지원하는 모든 디바이스](../devices/index.md)에서 쉽게 읽고 상호 작용할 수 있도록 컨트롤, 글꼴 및 기타 UI 요소의 크기를 조정합니다.

디바이스에서 앱이 실행될 때 시스템은 화면에 UI 요소가 표시되는 방식을 정규화하는 알고리즘을 사용합니다. 이 크기 조정 알고리즘은 가시거리 및 화면 밀도(인치당 픽셀)를 고려하여 물리적 크기가 아닌 인식되는 크기를 최적화합니다. 크기 조정 알고리즘에 따르면 3m 떨어져 있는 Surface Hub의 24픽셀 글꼴은 몇 인치 떨어져 있는 5인치 휴대폰의 24픽셀 글꼴과 마찬가지로 읽힙니다.

![다양한 디바이스의 가시거리](images/1910808-hig-uap-toolkit-03.png)

크기 조정 시스템의 작동 방식 때문에 UWP 앱을 디자인할 때 실제 물리적 픽셀이 아닌 유효 픽셀로 디자인하게 됩니다. 유효 픽셀(epx)은 가상 측정 단위이며, 화면 밀도와 무관하게 레이아웃 크기와 간격을 표시하는 데 사용됩니다. (지침에서는 epx, ep, px를 번갈아 사용합니다.).

디자인할 때는 픽셀 밀도 및 실제 화면 해상도를 무시해도 됩니다. 대신, 크기 클래스에 대한 유효 해상도(유효 픽셀로 나타낸 해상도)를 고려해서 디자인합니다(자세한 내용은 [화면 크기 및 중단점 문서](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) 참조).

> [!TIP]
> 이미지 편집 프로그램에서 화면 모형을 만드는 경우 DPI를 72로 설정하고 이미지 크기를 목표로 하는 크기 클래스의 유효 해상도로 설정하세요. 크기 클래스 및 유효 해상도 목록은 [화면 크기 및 중단점 문서](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)를 참조하세요.


## <a name="layout"></a>레이아웃

### <a name="margins-spacing-and-positioning"></a>여백, 간격, 위치 
![그리드](images/4epx.png)

시스템은 앱 UI 크기를 확장할 때 4의 배수씩 확장합니다.

즉 **UI 요소의 크기, 여백, 위치는 항상 4의 배수인 epx가 되어야 합니다**. 이렇게 하면 전체 픽셀이 정렬된 최고의 렌더링이 됩니다. 또 UI 요소가 선명해지고, 가장자리가 날카로워집니다. 

텍스트에는 이런 요구 사항이 없습니다. 텍스트의 크기와 위치를 자유롭게 설정할 수 있습니다. 텍스트와 다른 UI 요소를 정렬하는 방법에 대한 자세한 내용은 [UWP Typography Guide](../style/typography.md)를 참조하세요.

![그리드 크기 조정](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>레이아웃 패턴

![일반적인 레이아웃 패턴](images/page-components.png)

사용자 인터페이스는 세 가지 유형의 요소들로 구성되어 있습니다. 
1. **탐색 요소**는 사용자가 표시하려는 콘텐츠를 선택하는 데 도움을 줍니다. [탐색 기본 사항](navigation-basics.md)을 참조하세요.
2. **명령 요소**는 조작, 저장, 콘텐츠 공유 등 작업을 시작합니다. [명령 기본 사항](commanding-basics.md)을 참조하세요.
3. **콘텐츠 요소**는 앱의 콘텐츠를 표시합니다. [콘텐츠 기본 사항](content-basics.md)을 참조하세요.

### <a name="adaptive-behavior"></a>적응형 동작
![전화와 데스크톱의 적응 동작](../controls-and-patterns/images/patterns_masterdetail.png)

앱의 UI를 모든 윈도우 기반 장치에서 읽고 사용할 수 있겠지만, 앱의 UI를 특정 장치와 화면 크기에 맞게 사용자 지정하는 것이 좋습니다. 더 자세한 지침은 [화면 크기 및 중단점](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)과 [반응형 디자인 기술](../layout/responsive-design.md)을 참조하세요.

## <a name="type"></a>입력

UWP 앱은 기본값으로 **Segoe UI** 글꼴을 사용하며, UWP 입력 램프에는 모든 디스플레이 크기에서 가장 효율적으로 접근하기 위해 7가지 입력 체계 클래스가 포함되어 있습니다. 

![입력 램프](images/type-ramp.png)

입력 램프에 대한 세부 정보는 [UWP 입력 체계 가이드](../style/typography.md)를 참조하세요. 앱에서 여러 UWP 입력 램프 수준을 사용하는 방법을 알아보려면 [테마 리소스](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)를 참조하세요.

## <a name="color"></a>색

색은 사용자에게 직관적으로 정보를 전달하는 방법을 제공합니다. 대화형 신호, 사용자 작업에 대한 피드백 제공, 상태 정보 전달, 인터페이스의 시각적 일관성 제공에 사용할 수 있습니다. 

Windows 10은 여러 다양한 셸과 UWP 앱에 적용되는 48가지 색으로 구성된 공통 유니버셜 색상표를 제공합니다. 

![유니버설 windows 색상표](images/colors.png)

시스템은 자동으로 시스템 테마 컬러를 기반으로 UWP 앱에 색을 적용합니다. 설정의 색상표에서 테마 컬러를 선택하면, 해당 색이 시스템 테마의 일부로 표시됩니다. 사용자 기본 설정에 따라 시작, 시작 타일, 작업 표시줄, 제목 표시줄에도 시스템 테마 컬러를 표시할 수 있습니다. 

![설정에서 테마 컬러 선택](images/selectcolor.png)

UWP 앱에서는 공용 컨트롤의 하이퍼링크와 선택한 상태에 시스템 테마 컬러가 반영됩니다.

![컨트롤의 시스템 테마 컬러](images/accentcolor.png)

UWP 앱은 [가벼운 스타일](../controls-and-patterns/xaml-styles.md#lightweight-styling)을 사용하거나, [사용자 지정 컨트롤](../controls-and-patterns/control-templates.md)을 만들어 시스템 테마 컬러가 컨트롤에 표시되는 것을 막을 수 있습니다.

UWP 앱에서 색 사용에 대한 추가 지침은 [색](../style/color.md) 문서를 참조하세요.

### <a name="themes"></a>테마

사용자는 밝은 테마, 어두운 테마, 고대비 테마를 선택할 수 있습니다. 사용자의 테마 기본 설정에 따라 앱의 모양을 변경 하거나 옵트아웃 할 수 있습니다.

밝은 테마는 생산성 작업 및 읽기에 가장 적합합니다.

어두운 테마를 사용하면 동영상이나 이미지가 많은 미디어 중심의 앱과 시나리오에서 대비를 더 또렷이 표시할 수 있습니다. 이러한 시나리오에서는 읽기보다는 영화 시청이 많을 것입니다. 또 주변 조명이 어두울 것입니다.

![밝은 테마와 어두운 테마에 표시된 계산기](images/light-dark.png)

고대비 테마는 더 쉽게 인터페이스를 확인할 수 있는 작은 대비색상표를 사용합니다.
![밝은 테마 및 고대비 검정 테마로 표시된 계산기](../accessibility/images/high-contrast-calculators.png)


앱에서 UWP 색 램프와 테마 사용에 대한 자세한 내용은 [테마 리소스](../controls-and-patterns/xaml-theme-resources.md)를 참조하세요.

## <a name="icons"></a>아이콘
![아이콘](images/icons.png)

아이콘은 우리 일상 생활에 점점 더 많이 사용되고 있는 시각적 언어 유형입니다. 아이콘을 사용하면 시각적으로 설득력 있는 방식으로 개념과 동작을 표현하고, 화면 공간을 절약할 수 있으며, 이는 디지털 라이프를 순항하는 하나의 방편으로도 기능합니다. 

모든 UWP 앱은 [Segoe MDL2 글꼴](../style/segoe-ui-symbol-font.md)의 아이콘을 이용합니다. 이 아이콘은 모든 사람에게 친숙하고 쉽게 식별할 수 있는 기존에 확립된 형태에 토대를 두고 있습니다. 그러나 한 손으로 그릴 수 있다는 느낌이 들도록 현대화되어 있습니다.

자신만의 아이콘을 만들고 싶다면[UWP 앱용 아이콘](../style/icons.md)을 참조하세요.

## <a name="tiles"></a>타일
![시작 메뉴 타일](images/tiles.png)

시작 메뉴에 표시되고, 앱에서 진행되고 있는 일을 간략하게 알 수 있습니다. 그 원동력은 이면의 콘텐츠, 제공되는 인텔리전스 및 기능입니다. 

UWP 앱은 앱의 아이콘과 정체성에 따라 4가지 크기(작은, 중간, 넓은, 큰)로 타일을 사용자 지정할 수 있습니다. UWP 앱의 타일 디자인에 대한 지침은 [타일 및 아이콘 자산에 대한 지침](../shell/tiles-and-notifications/app-assets.md)을 참조하세요.

## <a name="controls"></a>컨트롤
![단추 컨트롤](images/1910808-hig-uap-toolkit-01.png)

UWP는 모든 Windows 기반 장치에서 원활하게 작동되는 공통 컨트롤 세트를 제공합니다. 이러한 컨트롤에는 단추와 텍스트 요소 같은 단순한 컨트롤부터 데이터 및 템플릿 세트에서 목록을 생성할 수 있는 정교한 컨트롤까지 모든 것이 포함되어 있습니다.

UWP 컨트롤은 자동으로 시스템 테마와 테마 컬러를 선택하고, 모든 입력 유형을 지원하며, 모든 장치로 확장이 됩니다. 또 다양하게 사용자 지정을 할 수 있습니다. 예를 들면, 컨트롤의 전경색을 변경하거나, 모양을 완전히 사용자 지정할 수 있습니다. 

여기에서 만들 수 있는 UWP 컨트롤과 패턴의 전체 목록은 [컨트롤 및 패턴 섹션](../controls-and-patterns/index.md)을 참조하세요.

## <a name="input"></a>입력
![입력](../input/images/input-interactions/icons-inputdevices03.png)

UWP 앱은 스마트 조작을 사용합니다. 즉, 클릭이 실제 마우스 클릭인지, 스타일러스인지, 또는 손가락으로 탭한 것인지를 알거나 정의하지 않고 클릭 조작을 디자인할 수 있습니다. 하지만 여기에 더해 [특정 입력 모드와 장치에](../input/input-primer.md) 대해 앱을 디자인 할 수 있습니다.

## <a name="accessibility"></a>접근성
![포괄적인 사용자 디자인](images/inclusive.png)

마지막이지만 아주 중요한 접근성은 앱의 경험을 모든 사용자에게 개방적으로 만드는 것이 중요합니다. 장애가 있는 사용자뿐만 아니라 모든 사람과 관련이 있습니다. 모든 사용자가 포괄적인 사용자 경험에서 혜택을 누릴 수 있습니다. 모든 사용자가 앱을 쉽게 사용할 수 있도록 만드는 방법에 대한  자세한 내용은 [UWP 앱의 유용성](../usability/index.md)을 참조하세요. 시각, 청각, 이동성에 제약이 있는 사용자를 위한 [접근성 기능](../accessibility/accessibility-overview.md) 또한 고려하는 것이 좋습니다. 

처음부터 접근성을 디자인에 포함시키면, 더 적은 노력과 시간으로 [쉽게 사용할 수 있는 앱을](../accessibility/accessibility-in-the-store.md) 만들 수 있습니다.

## <a name="tools-and-design-toolkits"></a>도구 및 디자인 키트
기본적인 디자인 기능에 대해 알아봤습니다. 이제 자신의 UWP 앱 디자인을 시작해 보세요!

디자인 프로세스에 도움을 주는 다양한 도구들을 제공하고 있습니다.

* XD, Illustrator, Photoshop, Framer, Sketch와 추가 디자인 도구 및 글꼴 다운로드는 [디자인 도구 키트 페이지](../downloads/index.md)를 참조하세요. 

* UWP 앱을 실제로 코딩하도록 컴퓨터를 설정하는 방법은 [시작 &gt; 설정](../../get-started/get-set-up.md) 문서를 참조하세요. 

* UWP용 UI 구현 방법에 대한 영감을 얻고 싶다면 종합적인 [샘플 UWP 앱](https://developer.microsoft.com/windows/samples)을 조사해 보세요.

## <a name="next-fluent-design-system"></a>다음: 흐름 디자인 시스템
흐름 디자인(Microsoft의 디자인 시스템)의 기반이 되는 원칙에 대해 학습하고, UWP 앱에 포함시킬 수 있는 더 많은 기능에 대해 알아보고 싶다면 계속해서 [흐름 디자인 시스템](../fluent-design-system/index.md)에 대해 알아보세요.