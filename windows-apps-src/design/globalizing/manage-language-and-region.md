---
Description: 이 항목에서는 사용자 프로필 언어 목록, 앱 매니페스트 언어 목록 및 앱 런타임 언어 목록 이라는 용어를 정의 합니다. 이 항목 및이 기능 영역의 다른 항목에서 이러한 용어를 사용할 예정 이므로 무엇을 의미 하는지 알아 두어야 합니다.
title: 사용자 프로필 언어 및 앱 매니페스트 언어 이해
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 9998436b106acce6a9223140e66d2633c2210a54
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493358"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>사용자 프로필 언어 및 앱 매니페스트 언어 이해
Windows 사용자는 **설정**  >  **시간 & 언어**  >  **지역 & 언어** 를 사용 하 여 기본 표시 언어의 순서가 지정 된 목록 또는 단일 기본 표시 언어만 구성할 수 있습니다. 언어에는 지역 변형이 있을 수 있습니다. 예를 들어 스페인에서 스페인어로, 스페인어는 멕시코로, 스페인어는 미국의 음성으로, 스페인어는 미국에서 음성으로 선택할 수 있습니다.

또한 **설정**  >  **시간 &** 언어  >  **지역 &** 언어 이지만 언어와는 별도로, 전 세계에서 사용자의 위치 (지역 이라고 함)를 지정할 수 있습니다. 표시 언어 (및 지역 변형) 설정은 지역 설정의 determiner이 아니며 그 반대의 경우도 마찬가지입니다. 예를 들어 사용자가 현재 프랑스에 살고 있지만 기본 Español (México)의 기본 설정 된 Windows 표시 언어를 선택할 수 있습니다.

Windows 앱의 경우 언어는 [BCP-47 언어 태그로](https://tools.ietf.org/html/bcp47)표시 됩니다. 예를 들어, BCP-47 언어 태그 "en-us"는 **설정**의 영어 (미국)에 해당 합니다. 적절 한 Windows 런타임 Api는 BCP-47 언어 태그의 문자열 표현을 허용 하 고 반환 합니다.

또한 [IANA language 하위 태그 레지스트리](https://www.iana.org/assignments/language-subtag-registry)를 참조 하세요.

다음 세 섹션에서는 "사용자 프로필 언어 목록", "앱 매니페스트 언어 목록" 및 "app runtime language list" 라는 용어를 정의 합니다. 이 항목 및이 기능 영역의 다른 항목에서 이러한 용어를 사용할 예정 이므로 무엇을 의미 하는지 알아 두어야 합니다.

## <a name="user-profile-language-list"></a>사용자 프로필 언어 목록
사용자 프로필 언어 목록은 **설정**  >  **시간 & 언어**  >  **지역 & 언어**  >  **언어**에 따라 사용자가 구성한 목록의 이름입니다. 코드에서는 [**GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) 속성을 사용 하 여 사용자 프로필 언어 목록에 문자열의 읽기 전용 목록으로 액세스할 수 있습니다. 여기서 각 문자열은 "en-us" 또는 "ja-jp"와 같은 단일 [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47) 입니다.

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>응용 프로그램 매니페스트 언어 목록
앱 매니페스트 언어 목록은 앱이 지원을 선언 하거나 선언 하는 언어 목록입니다. 이 목록은 지역화를 위한 개발 수명 주기를 통해 앱이 진행 됨에 따라 증가 합니다.

이 목록은 컴파일 시간에 결정 되지만, 발생 하는 방식을 정확 하 게 제어 하는 두 가지 옵션이 있습니다. 한 가지 옵션은 Visual Studio에서 프로젝트의 파일에 있는 목록을 확인할 수 있도록 하는 것입니다. 이렇게 하려면 먼저 앱 패키지 매니페스트 소스 파일 ()의 **응용 프로그램 탭에서 응용 프로그램** 의 **기본 언어** 를 설정 `Package.appxmanifest` 합니다. 그런 다음 동일한 파일이이 구성을 포함 하는지 확인 합니다 (기본적으로 수행 됨).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Visual Studio에서 빌드된 응용 프로그램 패키지 매니페스트 파일 ( `AppxManifest.xml` )을 생성할 때마다 `Resource` 소스 파일의 단일 요소를 프로젝트에서 찾을 수 있는 모든 언어 한정자의 공용 구조체로 확장 합니다 ( [언어, 크기 조정, 고대비 및 기타 한정자를 위해 리소스를 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)하는 방법 참조). 예를 들어 지역화를 시작 하 고 폴더 또는 파일 이름에 "en-us", "ja-jp" 및 "fr-fr"이 포함 된 문자열, 이미지 및/또는 파일 리소스가 있는 경우 빌드된 파일에는 `AppxManifest.xml` 다음이 포함 됩니다. 목록의 첫 번째 항목은 사용자가 설정한 기본 언어입니다.

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

다른 옵션은 `<Resource>` 앱 패키지 매니페스트 소스 파일 ()의 단일 "x 생성" 요소를 `Package.appxmanifest` 확장 된 요소 목록으로 바꾸는 것입니다 `<Resource>` (기본 언어를 먼저 나열 하기 위해 주의). 이 옵션은 사용자에 게 더 많은 유지 관리 작업이 필요 하지만 사용자 지정 빌드 시스템을 사용 하는 경우 적절 한 옵션 일 수 있습니다.

시작 하기 위해 앱 매니페스트 언어 목록에는 한 가지 언어만 포함 됩니다. 아마도 en-us입니다. 그러나 궁극적으로 &mdash; 매니페스트를 수동으로 구성 하거나 &mdash; 목록에서 확장 되는 프로젝트에 번역 된 리소스를 추가 하는 경우입니다.

앱이 Microsoft Store에 있는 경우 응용 프로그램 매니페스트 언어 목록의 언어는 고객에 게 표시 되는 언어입니다. Microsoft Store에서 특별히 지 원하는 BCP 47 언어 태그 목록은 [지원 되는 언어](../../publish/supported-languages.md)를 참조 하세요.

코드에서 [**Applicationlanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) 속성을 사용 하 여 응용 프로그램 매니페스트 언어 목록에 문자열의 읽기 전용 목록으로 액세스할 수 있습니다. 여기서 각 문자열은 단일 BCP-47 언어 태그입니다.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>앱 런타임 언어 목록
관심 있는 세 번째 언어 목록은 방금 설명한 두 목록 간의 교집합입니다. 런타임에 응용 프로그램에서 지 원하는 언어 목록 (앱 매니페스트 언어 목록)은 사용자가 기본 설정 (사용자 프로필 언어 목록)을 선언한 언어 목록과 비교 됩니다. 앱 런타임 언어 목록이이 교차 (교집합이 비어 있지 않은 경우) 또는 앱의 기본 언어 (교집합이 비어 있는 경우)로 설정 됩니다.

구체적으로 말하면 앱 런타임 언어 목록은 이러한 항목으로 구성 됩니다.

1.  **(선택 사항) 주 언어 재정의**입니다. [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) 은 사용자에 게 독립적인 언어를 선택 하거나 기본 언어 선택 항목을 재정의 하는 몇 가지 강력한 이유가 있는 앱에 대 한 간단한 재정의 설정입니다. 자세히 알아보려면 [응용 프로그램 리소스 및 지역화 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208))을 참조 하세요.
2.  **앱에서 지 원하는 사용자의 언어**입니다. 앱 매니페스트 언어 목록으로 필터링 된 사용자 프로필 언어 목록입니다. 앱에서 지 원하는 언어에 따라 사용자 언어를 필터링 하면 Sdk (소프트웨어 개발 키트), 클래스 라이브러리, 종속 프레임 워크 패키지 및 앱 간에 일관성을 유지 합니다.
3.  **1과 2가 비어 있으면 앱에서 지 원하는 기본 또는 첫 번째 언어**입니다. 사용자 프로필 언어 목록에 앱이 지 원하는 언어가 포함 되지 않은 경우 앱 런타임 언어가 앱에서 지 원하는 첫 번째 언어입니다.

코드에서 [QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 속성을 사용 하 여 세미콜론으로 구분 된 BCP-47 언어 태그 목록을 포함 하는 문자열 형식으로 앱 런타임 언어 목록에 액세스할 수 있습니다.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

각각 단일 BCP-47 언어 태그를 포함 하는 문자열의 읽기 전용 목록으로 액세스할 수도 있습니다. 이 작업을 수행 하는 데는 [**Resourcecontext. 언어**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) 속성 또는 [**applicationlanguages**](/uwp/api/windows.globalization.applicationlanguages.Languages) 를 사용할 수 있습니다.

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

앱 런타임 언어 목록에는 Windows에서 앱에 대해 로드 하는 리소스와 날짜, 시간, 숫자 및 기타 구성 요소의 형식을 지정 하는 데 사용 되는 언어도 결정 됩니다. [날짜/시간/숫자 형식 전역화를](use-global-ready-formats.md)참조 하세요.

**참고** 사용자 프로필 언어와 앱 매니페스트 언어가 서로 다른 지역 변형 인 경우 사용자의 지역 변형이 앱 런타임 언어로 사용 됩니다. 예를 들어 사용자가 5GB를 선호 하 고 앱에서 en-us를 지 원하는 경우 앱 런타임 언어는 en-us입니다. 이렇게 하면 날짜, 시간 및 숫자가 사용자의 기대 (en-us)에 더 가깝게 서식 지정 되지만, 지역화 된 리소스는 앱의 지원 언어 (en-us)에 계속 로드 됩니다 (언어 일치로 인해).

## <a name="qualify-resource-files-with-their-language"></a>리소스 파일을 해당 언어로 한정
리소스 파일 이름 또는 해당 폴더를 언어 리소스 한정자로 바꿉니다. 리소스 한정자에 대 한 자세한 내용은 [언어, 크기 조정, 고대비 및 기타 한정자에 대 한 리소스를 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)하는 방법을 참조 하세요. 리소스 파일은 이미지 (또는 다른 자산) 이거나 텍스트 문자열을 포함 하는 *resw* 와 같은 리소스 컨테이너 파일 일 수 있습니다.

**참고** 앱의 기본 언어에 포함 된 리소스도 언어 한정자를 지정 해야 합니다. 예를 들어, 앱의 기본 언어가 영어 (미국) 인 경우 자산을로 한정 `\Assets\Images\en-US\logo.png` 합니다.

- Windows는 en-us 및 en-us와 같은 지역 변형을 포함 하 여 복잡 한 일치를 수행 합니다. 따라서 지역 하위 태그를 적절 하 게 포함 합니다. [리소스 관리 시스템이 언어 태그와 일치 하는 방법을](../../app-resources/how-rms-matches-lang-tags.md)확인 합니다.
- 언어에 대해 정의 된 표시 안 함-스크립트 값이 없는 경우 한정자에서 언어 스크립트 하위 태그를 지정 합니다. 예를 들어 zh-cn 또는 zh-cn를 사용 하는 대신 zh-cn-Zh-hant, zh-cn-Zh-hant-zh-cn-Hans를 사용 합니다 (자세한 내용은 [IANA 언어 하위 태그 레지스트리](https://www.iana.org/assignments/language-subtag-registry)참조).
- 표준 언어가 하나인 언어의 경우 지역 한정자를 포함할 필요가 없습니다. 예를 들어 ja-jp 대신 ja-jp를 사용 합니다.
- 컴퓨터 변환기와 같은 일부 도구 및 기타 구성 요소는 지역별 언어 정보와 같은 특정 언어 태그를 찾을 수 있으므로 데이터를 이해 하는 데 도움이 됩니다.

### <a name="not-all-resources-need-to-be-localized"></a>모든 리소스를 지역화 해야 하는 것은 아닙니다.

모든 리소스에 대해 지역화가 필요 하지 않을 수 있습니다.

- 최소한 모든 리소스가 기본 언어로 존재 하는지 확인 합니다.
- 일부 리소스의 하위 집합은 밀접 하 게 관련 된 언어 (부분 지역화)에 충분 합니다. 예를 들어 앱이 스페인어로 전체 리소스 집합을 포함 하는 경우 앱의 UI를 모두 카탈로니아어로 지역화 하지 않을 수 있습니다. 카탈로니아어와 스페인어를 말하는 사용자의 경우 카탈로니아어에서 사용할 수 없는 리소스는 스페인어로 표시 됩니다.
- 일부 리소스는 특정 언어에 대 한 예외가 필요할 수 있으며, 대부분의 다른 리소스는 공통 리소스에 매핑됩니다. 이 경우 ' und ' 언어 태그가 결정 되지 않은 모든 언어에 사용할 리소스를 표시 합니다. Windows에서는 ' und ' 언어 태그를 와일드 카드 (' '와 유사 \* )로 해석 합니다 .이는 다른 특정 일치 항목 후에 최상위 앱 언어와 일치 합니다. 예를 들어, 일부 리소스가 핀란드어에 대해 다르지만 나머지 리소스는 모든 언어에 대해 동일 하 게 표시 되는 경우 핀란드어 리소스는 핀란드어 언어 태그로 표시 되어야 하 고 나머지는 ' und '로 표시 되어야 합니다.
- 텍스트의 글꼴 또는 높이와 같은 언어 스크립트를 기반으로 하는 리소스의 경우 지정 된 스크립트와 함께 결정 되지 않은 언어 태그를 사용 합니다. ' &lt; und &gt; '. 예를 들어 라틴어 글꼴의 경우 및를 사용 하 여 `und-Latn\\fonts.css` 키릴 자모 글꼴을 사용 `und-Cryl\\fonts.css` 합니다.

## <a name="set-the-http-accept-language-request-header"></a>HTTP Accept 언어 요청 헤더 설정
호출 하는 웹 서비스의 지역화 범위가 앱과 동일한 지 여부를 고려 합니다. 일반적인 웹 요청의 Windows 앱에서 수행 된 HTTP 요청과 XMLHttpRequest (XHR)는 표준 HTTP Accept 언어 요청 헤더를 사용 합니다. 기본적으로 HTTP 헤더는 사용자 프로필 언어 목록으로 설정 됩니다. 목록의 각 언어는 언어의 neutrals 및 가중치 (q)를 포함 하도록 추가로 확장 됩니다. 예를 들어, fr-fr 및 en-us의 사용자 언어 목록에서는 fr-fr, fr, en-us, en ("fr-fr, fr; q = 0.8, en-us; q = 0.5, en; q = 0.3")의 HTTP Accept 언어 요청 헤더가 생성 됩니다. 그러나 날씨 앱 (예:)이 프랑스어 (프랑스)로 UI를 표시 하지만 기본 설정 목록에서 사용자의 최상위 언어가 독일어 인 경우 앱 내에서 일관성을 유지 하기 위해 서비스에서 프랑스어 (프랑스)를 명시적으로 요청 해야 합니다.

## <a name="apis-in-the-windowsglobalization-namespace"></a>Windows 세계화 네임 스페이스의 Api
일반적으로 [**Windows 세계화**](/uwp/api/windows.globalization?branch=live) 네임 스페이스의 api는 앱 런타임 언어 목록을 사용 하 여 언어를 결정 합니다. 일치 하는 형식이 없는 언어가 있으면 사용자 로캘이 사용 됩니다. 이는 시스템 클록에 사용 되는 로캘과 동일 합니다. 사용자 로캘은 **설정**  >  **시간 & 언어**  >  **지역 & 언어**  >  **추가 날짜, 시간, & 국가별 설정**  >  **영역: 날짜, 시간 또는 숫자 형식 변경**에서 사용할 수 있습니다. 또한 **Windows 세계화** api에는 앱 런타임 언어 목록 대신 사용할 언어 목록을 지정 하는 재정의가 있습니다.

[**Language 클래스를**](/uwp/api/windows.globalization.language?branch=live) 사용 하 여 언어 스크립트, 표시 이름 및 네이티브 이름과 같은 특정 언어에 대 한 세부 정보를 검사할 수 있습니다.

## <a name="use-geographic-region-when-appropriate"></a>해당 하는 경우 지리적 지역 사용
**설정**  >  **시간 & 언어**  >  **지역 & 언어**  >  **국가 또는 지역**에서 사용자는 전 세계의 위치를 지정할 수 있습니다. 사용자에 게 표시할 콘텐츠를 선택 하기 위해 언어 대신이 설정을 사용할 수 있습니다. 예를 들어 뉴스 앱은 기본적으로이 지역의 콘텐츠를 표시 합니다.

코드에서 [**GlobalizationPreferences. HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) 속성을 사용 하 여이 설정에 액세스할 수 있습니다.

[**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) 클래스를 사용 하 여 표시 이름, 네이티브 이름 및 사용 중인 통화와 같은 특정 영역에 대 한 세부 정보를 검사할 수 있습니다.

## <a name="examples"></a>예제
다음 표에는 다양 한 언어 및 지역 설정에서 사용자가 앱의 UI에 표시 되는 사항의 예가 포함 되어 있습니다.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">응용 프로그램 매니페스트 언어 목록</th>
<th align="left">사용자 프로필 언어 목록</th>
<th align="left">앱의 주 언어 재정의 (선택 사항)</th>
<th align="left">앱 런타임 언어 목록</th>
<th align="left">앱에서 사용자에 게 표시 되는 내용</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">영어 (GB) (기본값); 독일어 (독일)</td>
<td align="left">영어(영국)</td>
<td align="left">없음</td>
<td align="left">영어(영국)</td>
<td align="left">UI: 영어 (GB)<br>날짜/시간/숫자: 영어 (GB)</td>
</tr>
<tr>
<td align="left">독일어 (독일) (기본값); 프랑스어 (프랑스); 이탈리아어 (이탈리아)</td>
<td align="left">프랑스어 (오스트리아)</td>
<td align="left">없음</td>
<td align="left">프랑스어 (오스트리아)</td>
<td align="left">UI: 프랑스어 (프랑스) (프랑스어 (오스트리아)에서 대체)<br>날짜/시간/번호: 프랑스어 (오스트리아)</td>
</tr>
<tr>
<td align="left">영어 (미국) (기본값); 프랑스어 (프랑스); 영어 (GB)</td>
<td align="left">영어 (캐나다); 프랑스어 (캐나다)</td>
<td align="left">없음</td>
<td align="left">영어 (캐나다); 프랑스어 (캐나다)</td>
<td align="left">UI: 영어 (미국) (영어 (캐나다)에서 대체)<br>날짜/시간/번호: 영어 (캐나다)</td>
</tr>
<tr>
<td align="left">스페인어 (스페인) (기본값); 스페인어 (멕시코); 스페인어 (라틴 아메리카); 포르투갈어 (브라질)</td>
<td align="left">영어(미국)</td>
<td align="left">없음</td>
<td align="left">스페인어(스페인)</td>
<td align="left">UI: 스페인어 (스페인) (영어에 대 한 대체 (fallback)를 사용할 수 없으므로 기본값 사용)<br>날짜/시간/숫자 스페인어 (스페인)</td>
</tr>
<tr>
<td align="left">카탈로니아어 (기본값); 스페인어 (스페인); 프랑스어 (프랑스)</td>
<td align="left">카탈로니아어 프랑스어 (프랑스)</td>
<td align="left">없음</td>
<td align="left">카탈로니아어 프랑스어 (프랑스)</td>
<td align="left">UI: 대부분의 모든 문자열이 카탈로니아어에 있지 않기 때문에 대부분 카탈로니아어 및 일부 프랑스어 (프랑스)<br>날짜/시간/숫자: 카탈로니아어</td>
</tr>
<tr>
<td align="left">영어 (GB) (기본값); 프랑스어 (프랑스); 독일어 (독일)</td>
<td align="left">독일어 (독일); 영어 (GB)</td>
<td align="left">영어 (GB) (앱 UI에서 사용자가 선택 함)</td>
<td align="left">영어 (GB); 독일어 (독일)</td>
<td align="left">UI: 영어 (GB) (언어 재정의)<br>날짜/시간/숫자 영어 (GB)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> Microsoft에서 사용 하는 표준 국가/지역 코드 목록은 [공식 국가/지역 목록을](/windows/uwp/publish/supported-languages)참조 하세요.

## <a name="important-apis"></a>중요 API
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext. 언어](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages. 언어](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [언어](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>관련 항목
* [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)
* [IANA 언어 하위 태그 레지스트리](https://www.iana.org/assignments/language-subtag-registry)
* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [지원되는 언어](../../publish/supported-languages.md)
* [날짜/시간/숫자 형식 세계화](use-global-ready-formats.md)
* [리소스 관리 시스템에서 언어 태그를 일치시키는 방법](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>샘플
* [응용 프로그램 리소스 및 지역화 샘플](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
