---
Description: Design and develop your app in such a way that it functions appropriately on systems with different language and culture configurations.
Search.Refinement.TopicID: 180
title: 세계화 지침
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 2e2dc5186c028aa8f20c2cc1d697f1749b4f1765
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8880951"
---
# <a name="guidelines-for-globalization"></a>세계화 지침

다른 언어 및 문화 구성을 가진 시스템에서 올바르게 작동하는 방식으로 앱을 디자인하고 개발합니다. [**세계화**](/uwp/api/Windows.Globalization?branch=live) API를 사용하여 데이터를 포맷하고, 언어, 지역, 문자 분류, 쓰기 시스템, 날짜/시간 형식, 숫자, 통화, 무게에 관한 코드 및 정렬 규칙에서 가정을 피합니다.

| 권장 사항 | 설명 |
| ------------- | ----------- |
| 문자열을 조정하고 비교할 때 문화를 고려합니다. | 예를 들어 문자열을 비교하기 전에 변경하지 않습니다. [문자열 사용에 대한 권장 사항](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)을 참조하세요. |
| 문자열 및 기타 데이터를 정렬할 때 항상 사전순으로 수행된다고 가정하지 마세요. | 라틴어 스크립트를 사용하지 않는 언어의 경우 발음 또는 펜 스트로크 수와 같은 요소를 기반으로 데이터가 정렬됩니다. 라틴어 스크립트를 사용하는 언어도 항상 알파벳 순으로 정렬되는 것은 아닙니다. 예를 들어 일부 국가의 전화 번호부는 알파벳 순으로 정렬되지 않습니다. Windows에서 자동으로 정렬할 수 있지만, 개발자가 정렬 알고리즘을 직접 만들 경우 대상 시장에서 사용되는 정렬 방법을 고려해야 합니다. |
| 올바른 숫자, 날짜, 시간, 주소 및 전화 번호 형식을 사용하세요. | 이러한 형식은 문화, 지역, 언어 및 시장 간에 다릅니다. 이러한 데이터를 표시하는 경우 [**세계화**](/uwp/api/Windows.Globalization?branch=live) API를 사용하여 특정 대상에 맞는 형식을 사용합니다. [날짜/시간/숫자 형식 세계화](use-global-ready-formats.md)를 참조하세요. 성과 이름이 표시되는 순서, 주소 형식도 다를 수 있습니다. 표준 날짜, 시간 및 숫자 표시를 사용합니다. 표준 날짜 및 시간 선택 컨트롤을 사용합니다. 표준 주소 정보를 사용합니다. |
| 국가별 측정 단위 및 통화를 지원합니다. | 사용되는 단위와 배율은 국가마다 다르지만, 미터법과 영국식 시스템이 가장 일반적으로 사용됩니다. 길이, 온도, 면적 등을 측정할 경우 올바른 시스템 측정 기능을 지원해야 합니다. [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) 속성을 사용하여 특정 지역에서 사용하는 통화 모음을 가져옵니다. |
| 문자 인코딩에 대해 유니코드를 사용합니다. | 기본적으로 Microsoft Visual Studio에서는 모든 문서에 대해 유니코드 문자 인코딩을 사용합니다. 다른 편집기를 사용하는 경우 원본 파일을 적절한 유니코드 문자 인코딩으로 저장해야 합니다. 모든 UWP API는 UTF-16 인코딩 문자열을 반환합니다. |
| 국제 용지 크기를 지원합니다. | 가장 일반적인 용지 크기는 국가마다 다르므로, 인쇄와 같이 용지 크기에 따라 달라지는 기능이 포함되어 있는 경우 일반적인 국가별 크기를 지원하는지 테스트하여 확인해야 합니다. |
| 키보드 또는 IME의 언어를 기록합니다. | 앱에서 사용자에게 텍스트 입력을 묻는 경우 현재 활성화된 키보드 레이아웃 또는 IME(입력기)의 언어 태그를 기록합니다. 그러면 나중에 입력 내용을 사용자에게 표시할 때 적절한 서식으로 표시됩니다. [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) 속성을 사용하여 현재 입력 언어를 가져옵니다. |
| 언어로 사용자의 지역을 가정하거나 지역으로 사용자의 언어를 가정하지 마세요. | 언어 및 지역은 서로 다른 개념입니다. 사용자가 특정 지역의 변형된 언어(예: 영어[영국] en-GB)로 말하지만, 완전히 다른 국가 또는 지역에 있을 수 있습니다. 앱에 사용자의 언어(예: UI 텍스트용) 또는 지역(예: 라이선스 발급용) 정보가 필요한지 여부를 고려하세요. 자세한 내용은 [사용자 프로필 언어와 앱 매니페스트 언어 이해](manage-language-and-region.md)를 참조하세요. |
| 언어 태그를 비교하는 규칙은 간단하지 않습니다. | [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)는 복잡합니다. 언어 태그를 비교할 경우 많은 문제(예: 스크립트 정보, 레거시 태그 및 여러 지역별 변형의 일치 문제)가 있습니다. Windows의 리소스 관리 시스템에서 일치 작업을 자동으로 처리합니다. 리소스 집합을 원하는 언어로 지정할 수 있습니다. 그러면 시스템에서 사용자와 앱에 적절한 언어를 자동으로 선택합니다. [앱 리소스 및 리소스 관리 시스템](../../app-resources/index.md) 및 [리소스 관리 시스템과 언어 태그를 일치하는 방법](../../app-resources/how-rms-matches-lang-tags.md)을 참조하세요. |
| 레이블 및 텍스트 입력 컨트롤에 다른 텍스트 길이 및 글꼴 크기를 사용할 수 있도록 UI를 디자인합니다. | 다른 언어로 번역된 문자열은 길이가 상당히 다를 수 있으므로 UI 컨트롤을 콘텐츠에 맞게 동적으로 크기를 조정해야 합니다. 다른 언어의 공통 문자에는 영어에 일반적으로 사용되는 위 또는 아래 표시가 포함될 수 있습니다(예: Å 또는 Ņ). 표준 글꼴 크기와 선 높이를 사용하여 충분한 세로 공간을 제공합니다. 다른 언어의 글꼴은 가독성 유지를 위해 최소 글꼴 크기가 더 커야 할 수 있습니다. [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live) 네임스페이스의 클래스를 참조하세요. |
| 읽는 순서 미러링을 지원합니다. | 텍스트 정렬 및 읽기 순서는 영어의 경우 왼쪽에서 오른쪽으로 아랍어 또는 히브리어의 경우 RTL(오른쪽에서 왼쪽으로) 방식이 사용됩니다. 제품을 다른 읽기 순서를 사용하는 언어로 지역화할 경우 UI 요소의 레이아웃이 미러링을 지원해야 합니다. 항목(예: 뒤로 단추), UI 전환 효과 및 이미지를 미러링해야 할 수도 있습니다. 자세한 내용은 [레이아웃 및 글꼴 조정, RTL 지원](adjust-layout-and-fonts--and-support-rtl.md)을 참조하세요. |
| 텍스트와 글꼴을 올바르게 표시합니다. | 적합한 글꼴, 글꼴 크기 및 텍스트 방향은 시장마다 다릅니다. 자세한 내용은 [**레이아웃 및 글꼴 조정, RTL 지원**](adjust-layout-and-fonts--and-support-rtl.md) 및 [국가별 글꼴](loc-international-fonts.md)을 참조하세요. |

## <a name="important-apis"></a>중요 API
 
* [세계화](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>관련 항목

* [문자열 사용에 대한 권장 사항](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [날짜/시간 숫자 형식 세계화](use-global-ready-formats.md)
* [사용자 프로필 언어와 앱 매니페스트 언어 이해](manage-language-and-region.md)
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [앱 리소스 및 리소스 관리 시스템](../../app-resources/index.md)
* [리소스 관리 시스템이 언어 태그를 일치하는 방법](../../app-resources/how-rms-matches-lang-tags.md)
* [레이아웃 및 글꼴 조정, RTL 지원](adjust-layout-and-fonts--and-support-rtl.md)
* [국가별 글꼴](loc-international-fonts.md)
* [자신의 앱을 현지화 가능하도록 만들 수 있습니다.](prepare-your-app-for-localization.md)

## <a name="samples"></a>샘플

* [세계화 기본 설정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231608)
