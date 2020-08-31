---
description: 내게 필요한 옵션의 랜드마크 및 제목 기능을 설명 합니다.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: 랜드마크 및 머리글
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 942c24e8f5c7c521502ee5a9f9eb7175bf04b94f
ms.sourcegitcommit: 2a1ceeacf5cdadc803bad83dc3ceb57a16ce79a3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89067515"
---
# <a name="landmarks-and-headings"></a>랜드마크 및 머리글

사용자 인터페이스는 일반적으로 sighted 사용자가 *모든* 콘텐츠를 읽기 위해 느리지 않고도 관심 있는 항목을 신속 하 게 skim 수 있도록 하는 시각적 효율적인 방식으로 구성 됩니다. 화면 판독기 사용자에 게는 동일한 skimming 기능이 있어야 합니다. 랜드마크 및 머리글은 보조 기술 ()의 사용자를 보다 효율적으로 탐색 하는 데 도움이 되는 사용자 인터페이스 섹션을 정의 합니다. 콘텐츠를 랜드마크 및 제목에 표시 하면 화면 읽기 프로그램 사용자에 게 sighted 사용자의 방식과 유사한 콘텐츠를 skim 하는 옵션이 제공 됩니다.

웹 콘텐츠에서 [aria 랜드마크](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), [Aria 머리글](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html)및 [HTML 머리글](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) 의 개념을 사용 하 여 화면 판독기 사용자가 빠르게 탐색할 수 있습니다. 웹 페이지는 랜드마크 및 머리글을 활용 하 여 사용자가 대량 청크 (랜드마크) 및 더 작은 청크 (제목)로 신속 하 게 이동할 수 있도록 하 여 콘텐츠를 더 사용할 수 있도록 합니다. 특히 화면 읽기 권한자에는 사용자가 랜드마크 사이를 이동 하 여 제목 간을 이동할 수 있는 명령이 있습니다 (다음/이전 또는 특정 제목 수준). 동일한 이유로 앱에서 랜드마크 및 머리글을 고려 하는 것이 중요 합니다.

랜드마크를 사용 하면 콘텐츠를 검색, 탐색, 주 콘텐츠 등의 다양 한 범주로 그룹화 할 수 있습니다. 그룹화 한 후에는 사용자가 그룹 간에 신속 하 게 이동할 수 있습니다. 이 빠른 탐색을 통해 사용자는 이전에 항목을 통해 항목을 탐색 하는 데 충분 하지 않은 잠재적으로 많은 양의 콘텐츠를 건너뛸 수 있습니다.

예를 들어 탭 패널을 사용 하는 경우이 탐색 랜드마크를 고려 합니다. 검색 편집 상자를 사용 하는 경우이 검색 랜드마크를 고려 하 고 기본 콘텐츠를 주 콘텐츠 랜드마크로 설정 하는 것이 좋습니다. 랜드마크 내에서 또는 랜드마크 외부에 있든 관계 없이 하위 요소를 논리 제목 수준의 머리글로 설정 하는 것이 좋습니다.

Windows 설정 앱의 **접근성** 페이지를 참조 하세요.

![Windows 설정 앱의 접근성 페이지](images/EaseOfAccessSettings.png)  

검색 랜드마크 내에 래핑된 검색 편집 상자가 있습니다. 왼쪽의 탐색 요소는 탐색 랜드마크 안에 래핑됩니다. 오른쪽의 기본 콘텐츠는 주 콘텐츠 랜드마크 내에서 래핑됩니다. 이를 추가로 수행 하는 경우 탐색 랜드마크 내에서 제목 수준 1 인 **액세스 용이성** 이라는 주 그룹 머리글이 있습니다. 아래에는 **Vison**, **청각**등의 하위 옵션이 있습니다. 제목 수준 2가 있습니다. 기본 제목 (표시)을 제목 수준 1로 설정 하 고 제목 수준 2로 **모든 항목** 을 **표시**합니다.

설정 앱은 랜드마크 및 제목 없이 액세스할 수 있지만,이를 통해 더 사용할 수 있게 됩니다. 화면 읽기 프로그램 사용자는 필요한 그룹 (이정표)에 빠르고 쉽게 액세스할 수 있으며 하위 그룹 (제목)으로 신속 하 게 이동할 수 있습니다.

[LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) 를 사용 하 여 UI 요소를 원하는 [랜드마크 유형](https://docs.microsoft.com/windows/desktop/WinAuto/landmark-type-identifiers) 으로 설정 합니다. 이 랜드마크 UI 요소는 해당 랜드마크에 적합 한 다른 모든 UI 요소를 캡슐화 합니다.

[LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) 를 사용 하 여 특히 이정표의 이름을로 합니다. 주 또는 탐색 등의 미리 정의 된 랜드마크 유형을 선택 하는 경우 이러한 이름이 랜드마크 이름에 사용 됩니다. 그러나 랜드마크 형식을 custom으로 설정 하는 경우에는이 속성을 통해 이정표의 이름을 지정 해야 합니다. 이 속성을 사용 하 여 사용자 지정이 아닌 랜드마크 형식에서 기본 이름을 재정의할 수도 있습니다.

[HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) 를 사용 하 여 UI 요소를 *Level1* 에서 *Level9*까지 특정 수준의 머리글로 설정 합니다.

## <a name="examples"></a>예

Windows 데스크톱 앱에서 일반적인 프로그래밍 가능성 문제를 해결 하는 방법을 보여 주는 다양 한 코드 샘플은 [windows 데스크톱 앱에서 일반적인 프로그래밍 가능성 문제를 해결 하기 위한 코드 샘플](https://docs.microsoft.com/accessibility-tools-docs/)을 참조 하세요.

이러한 코드 샘플은 UI에서 많은 내게 필요한 옵션 문제를 해결 하는 데 도움이 될 수 있는[ Windows 용 내게 필요한 옵션 지원](https://github.com/microsoft/accessibility-insights-windows)기능에서 직접 참조 됩니다.
