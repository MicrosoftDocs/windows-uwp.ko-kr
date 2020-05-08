---
Description: 다양 한 언어 및 문화권 구성을 사용 하는 시스템에서 적절 하 게 작동 하는 방식으로 앱을 디자인 하 고 개발 합니다.
Search.Refinement.TopicID: 180
title: 세계화 지침
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: f08c8178781c82e8961fd180d4b75912359b4da9
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730270"
---
# <a name="guidelines-for-globalization"></a>세계화 지침

다양 한 언어 및 문화권 구성을 사용 하는 시스템에서 적절 하 게 작동 하는 방식으로 앱을 디자인 하 고 개발 합니다. [**세계화**](/uwp/api/Windows.Globalization?branch=live) api를 사용 하 여 데이터 서식 지정 그리고 언어, 지역, 문자 분류, 시스템 작성, 날짜/시간 형식 지정, 숫자, 통화, 가중치 및 정렬 규칙에 대 한 코드의 가정을 방지 합니다.

| 권장 | Description |
| ------------- | ----------- |
| 문자열을 조작 하 고 비교할 때 문화권을 고려해 야 합니다. | 예를 들어 문자열을 비교 하기 전에 문자열의 대/소문자를 변경 하지 마십시오. [문자열 사용에 대 한 권장 사항을](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)참조 하세요. |
| 문자열 및 기타 데이터를 정렬 (정렬) 할 때 항상 사전순으로 수행 된다고 가정 하지 마십시오. | 라틴어 스크립트를 사용 하지 않는 언어의 경우 데이터 정렬은 발음 또는 펜 스트로크 수와 같은 요소를 기준으로 합니다. 라틴어 스크립트를 사용 하는 언어에도 항상 영문자 정렬을 사용 하지는 않습니다. 예를 들어 일부 문화권에서 전화 번호부는 사전순으로 정렬 되지 않을 수 있습니다. Windows에서 사용자의 정렬을 처리할 수 있지만 사용자 고유의 정렬 알고리즘을 만드는 경우 대상 시장에서 사용 되는 정렬 방법을 고려해 야 합니다. |
| 숫자, 날짜, 시간, 주소 및 전화 번호를 적절 하 게 지정 합니다. | 이러한 형식은 문화권, 지역, 언어 및 시장에 따라 다릅니다. 이러한 데이터를 표시 하는 경우 [**전역화**](/uwp/api/Windows.Globalization?branch=live) api를 사용 하 여 특정 대상 그룹에 적합 한 형식을 가져옵니다. [날짜/시간/숫자 형식 전역화를](use-global-ready-formats.md)참조 하세요. 제품군과 지정 된 이름이 표시 되는 순서와 주소 형식이 다를 수 있습니다. 표준 날짜, 시간 및 숫자 표시를 사용 합니다. 표준 날짜 및 시간 선택 컨트롤을 사용 합니다. 표준 주소 정보를 사용 합니다. |
| 국가별 측정 단위와 통화를 지원 합니다. | 가장 많이 사용 되는 메트릭 시스템과 왕정 system은 서로 다른 국가에서 사용 되는 단위와 배율이 서로 다릅니다. 길이, 온도 및 영역과 같은 측정치를 처리 하는 경우 올바른 시스템 측정을 지원 해야 합니다. [**GeographicRegion CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) 속성을 사용 하 여 지역에서 사용 중인 통화 집합을 가져옵니다. |
| 문자 인코딩에 유니코드를 사용 합니다. | 기본적으로 Microsoft Visual Studio는 모든 문서에 유니코드 문자 인코딩을 사용 합니다. 다른 편집기를 사용 하는 경우에는 해당 유니코드 문자 인코딩에 원본 파일을 저장 해야 합니다. 모든 Windows 런타임 Api는 u t f-16으로 인코딩된 문자열을 반환 합니다. |
| 국가별 용지 크기를 지원 합니다. | 가장 일반적인 용지 크기는 국가 마다 다르며, 인쇄와 같이 용지 크기에 의존 하는 기능을 포함 하는 경우 일반적인 국제 크기를 지원 하 고 테스트 해야 합니다. |
| 키보드 또는 IME의 언어를 기록 합니다. | 앱에서 사용자에 게 텍스트 입력을 요청 하는 경우 현재 사용 하도록 설정 된 키보드 레이아웃 또는 IME (입력기)의 언어 태그를 기록 합니다. 이렇게 하면 입력이 나중에 표시 될 때 적절 한 서식 지정을 사용 하 여 사용자에 게 표시 됩니다. [**CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) 속성을 사용 하 여 현재 입력 언어를 가져옵니다. |
| 언어를 사용 하 여 사용자의 지역을 가정 하지 마세요. 그리고 지역을 사용 하 여 사용자의 언어를 가정 하지 마세요. | 언어와 지역은 별도의 개념입니다. 사용자는 영국에서 말한 것 처럼 영어의 특정 지역 변형을 사용할 수 있지만 사용자는 완전히 다른 국가 또는 지역에 있을 수 있습니다. 앱에 사용자 언어 (예: UI 텍스트의 경우) 또는 지역 (예: 라이선스)에 대 한 지식이 필요한 지 여부를 고려 합니다. 자세한 내용은 [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)를 참조 하세요. |
| 언어 태그를 비교 하는 규칙은 trivial이 아닙니다. | [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47) 는 복잡 합니다. 언어 태그를 비교할 때 스크립트 정보 일치, 레거시 태그 및 여러 지역 변형 관련 문제를 비롯 한 여러 가지 문제가 있습니다. Windows의 리소스 관리 시스템이 일치를 처리 합니다. 모든 언어에서 리소스 집합을 지정할 수 있으며, 시스템은 사용자 및 앱에 대해 적절 한 리소스를 선택 합니다. [앱 리소스 및 리소스 관리 시스템](../../app-resources/index.md) 및 [리소스 관리 시스템이 언어 태그와 일치 하는 방법](../../app-resources/how-rms-matches-lang-tags.md)을 참조 하세요. |
| 레이블 및 텍스트 입력 컨트롤에 대해 다양 한 텍스트 길이와 글꼴 크기를 수용 하도록 UI를 디자인 합니다. | 다른 언어로 변환 된 문자열은 길이가 크게 달라질 수 있으므로 UI 컨트롤이 해당 콘텐츠에 동적으로 크기를 조정 해야 합니다. 다른 언어의 공통 문자는 일반적으로 영어에서 사용 되는 것 (예: 있습니다 또는 Ņ)의 위 또는 아래 표시를 포함 합니다. 표준 글꼴 크기 및 선 높이를 사용 하 여 적절 한 세로 공간을 제공 합니다. 다른 언어에 대 한 글꼴은 읽기를 유지 하기 위해 더 큰 최소 글꼴 크기를 요구할 수 있습니다. [Windows. 세계화. Fonts](/uwp/api/windows.globalization.fonts?branch=live) 네임 스페이스의 클래스를 참조 하세요. |
| 읽기 순서의 미러링을 지원 합니다. | 텍스트 맞춤 및 읽기 순서는 영어와 같이 왼쪽에서 오른쪽으로, 오른쪽에서 왼쪽 (예: 아랍어 또는 히브리어)로 지정할 수 있습니다. 제품을 자체의 다른 읽기 순서를 사용 하는 언어로 지역화 하는 경우 UI 요소의 레이아웃이 미러링을 지원 해야 합니다. 뒤로 단추, UI 전환 효과 및 이미지와 같은 항목도 미러링 해야 할 수 있습니다. 자세한 내용은 [레이아웃 및 글꼴 조정 및 RTL 지원](adjust-layout-and-fonts--and-support-rtl.md)을 참조 하세요. |
| 텍스트와 글꼴을 올바르게 표시 합니다. | 이상적인 글꼴, 글꼴 크기 및 텍스트 방향은 시장 마다 다릅니다. 자세한 내용은 [**레이아웃 및 글꼴 조정 및 RTL**](adjust-layout-and-fonts--and-support-rtl.md) 및 [국제 글꼴](loc-international-fonts.md)지원을 참조 하세요. |

## <a name="important-apis"></a>중요 API
 
* [전역화](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>관련 항목

* [문자열 사용에 대한 권장 사항](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [날짜/시간/숫자 형식 세계화](use-global-ready-formats.md)
* [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)
* [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)
* [앱 리소스 및 리소스 관리 시스템](../../app-resources/index.md)
* [리소스 관리 시스템에서 언어 태그를 일치시키는 방법](../../app-resources/how-rms-matches-lang-tags.md)
* [레이아웃 및 글꼴 조정, RTL 지원](adjust-layout-and-fonts--and-support-rtl.md)
* [국가별 글꼴](loc-international-fonts.md)
* [앱을 현지화 가능하게 만들기](prepare-your-app-for-localization.md)

## <a name="samples"></a>샘플

* [세계화 기본 설정 샘플](https://code.msdn.microsoft.com/windowsapps/Globalization-preferences-6654eb36)
