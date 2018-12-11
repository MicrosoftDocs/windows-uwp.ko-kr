---
Description: Describes the landmarks and headings features of accessibility.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: 랜드마크 및 머리글
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d81957c379bd948a50d08b980ff20debc6c223c5
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927387"
---
# <a name="landmarks-and-headings"></a>랜드마크 및 머리글

사용자 인터페이스는 일반적으로 구경 온 사용자가 *모든* 콘텐츠를 느리게 읽지 않고, 자신에게 관심 있는 내용만 빨리 살펴볼 수 있도록 시각적으로 효율적인 방식으로 구성을 합니다. 화면 읽기 프로그램 사용자들에게도 동일한 빨리 살펴보기 기능이 필요합니다. 랜드마크 및 머리글은 보조 기술의 사용자에 대한 효율적인 탐색에 도움이 되는 사용자 인터페이스의 섹션을 정의합니다. 콘텐츠를 랜드마크 및 머리글로 표시해서 화면 읽기 프로그램 사용자에게 구경 온 사용자와 유사한 방식의 콘텐츠 빠르게 살펴보기 옵션을 제공합니다.

화면 읽기 프로그램 사용자들의 빠른 탐색에 사용할 수 있도록 오래 전부터 웹 콘텐츠에 [ARIA 랜드마크](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), [ARIA 머리글](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html), [HTML 머리글](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) 개념이 사용되어 왔습니다. 웹 페이지는 랜드마크와 머리글을 사용해 AT 사용자들이 큰 부분(랜드마크)과 작은 부분(머리글)에 더 빨리 도달하도록 만들어 콘텐츠를 더 유용하게 만듭니다. 구체적으로 화면 읽기 프로그램에는 사용자가 랜드마크와 머리글을 점프할 수 있는(이전/다음 또는 특정 머리글 수준) 명령어가 있습니다. 같은 이유에서 앱에도 랜드마크와 머리글을 고려하는 것이 중요합니다.

랜드마크는 콘텐츠를 검색, 탐색, 메인 콘텐츠 등 여러 범주로 그룹화해 사용할 수 있도록 해줍니다. 그룹화를 하면, AT 사용자는 그룹 사이를 빠르게 탐색할 수 있습니다. 이런 빠른 탐색 덕분에 사용자는 많은 양의 콘텐츠를 건너뛸 수 있습니다. 과거에는 이렇게 많은 콘텐츠를 항목 별로 탐색해야 해서 경험이 나빴을 것입니다. 

예를 들어 탭 패널을 사용하는 경우 이런 탐색 랜드마크를 고려합니다. 검색 편집 상자를 사용하는 경우, 이 검색 랜드마크를 고려하고 메인 콘텐츠를 메인 콘텐츠 랜드마크로 설정하는 방법을 고려합니다. 랜드마크 내부 또는 외부에서 머리글 하위 요소를 논리적인 머리글 수준으로 설정합니다. 

Windos 설정 앱의 **접근성** 페이지를 보겠습니다. 

![Windows 설정 앱의 접근성 페이지](images/EaseOfAccessSettings.png)  

검색 랜드마크로 래핑된 검색 편집 상자가 있습니다. 왼쪽의 탐색 요소는 탐색 랜드마크로, 오른쪽의 메인 콘텐츠는 메인 콘텐츠 랜드마크 내부에 래핑되어 있습니다. 더 살펴보면 탐색 랜드마크 내부에 **접근성**이라는 메인 그룹 머리글이 있습니다. 이는 머리글 수준 1에 해당됩니다. 그 아래에는 하위 옵션인 **시각**, **청각** 등이 있습니다. 이는 머리글 수준 2에 해당됩니다. 이런 식으로 메인 콘텐츠 머리글에 대한 설정을 하고 메인 주제에 대한 설정을 합니다. **디스플레이는** 머리글 수준 1이며, 머리글 수준 2인 하위 그룹은 **모든 항목 크게** 입니다. 

랜드마크와 머리글 없이도 설정 앱을 사용할 수 있지만, 랜드마크와 머리글 때문에 더욱 간편하게 사용할 수 있습니다. 화면 읽기 프로그램 사용자는 자신에게 필요한 그룹(랜드마크)을 빨리 쉽게 찾고, 다시 하위 그룹(머리글)을 찾을 수 있습니다. 

[AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty)을 사용해 UI 요소를 원하는 [랜드마크 유형](https://msdn.microsoft.com/library/windows/desktop/mt759299) 으로 설정합니다. 이 랜드마크 UI 요소는 랜드마크에 적합한 모든 다른 UI 요소들을 포함합니다. 

[AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty)를 사용해 랜드마크의 이름을 지정합니다. 메인이나 탐색형 등 미리 정의된 랜드마크 유형을 선택하는 경우 이 이름을 랜드마크 이름으로 사용합니다. 하지만 랜드마크 유형을 사용자 지정 설정하는 경우, 이 속성을 통해 랜드마크 이름을 지정해야 합니다. 또 이 속성을 사용해 사용자 지정 랜드마크 유형이 아닌 랜드마크의 기본값인 이름을 재정의할 수 있습니다. 

[AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty)를 사용해 UI 요소를 *수준1*부터 *수준9*까지 수준의 머리글로 설정합니다.

