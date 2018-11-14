---
author: stevewhims
Description: This topic defines the terms user profile language list, app manifest language list, and app runtime language list. We'll be using these terms in this topic and other topics in this feature area, so it's important to know what they mean.
title: 사용자 프로필 언어와 앱 매니페스트 언어 이해
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.author: stwhi
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 2215231b21700fc17b08c2149316f9a59f8d1f04
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266496"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>사용자 프로필 언어와 앱 매니페스트 언어 이해
Windows 사용자는 **설정** > **시간 및 언어** > **지역 및 언어**를 사용하여 기본 설정 표시 언어의 정렬된 목록 또는 기본 설정 표시 언어 하나를 구성할 수 있습니다. 언어에는 지역별 변수가 포함될 수 있습니다. 예를 들어 스페인에서 사용하는 스페인어, 멕시코에서 사용하는 스페인어, 미국에서 사용하는 스페인어 및 기타 스페인어를 선택할 수 있습니다.

또한 사용자는 **설정** > **시간 및 언어** > **지역 및 언어**에서 또는 언어에서 별도로 위치(또는 지역)를 지정할 수 있습니다. 표시 언어(및 지역 변수) 설정은 지역 설정의 결정자가 아니며, 반대의 경우도 마찬가지입니다. 예를 들어 사용자가 현재 프랑스에 거주하고 있지만 기본 설정 Windows 표시 언어로 스페인어(멕시코)를 선택할 수 있습니다.

UWP 앱의 경우 언어는 [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)로 나타냅니다. 예를 들어, BCP-47 언어 태그 "en-US"는 **설정**에서 영어(미국)에 해당합니다. 적절한 UWP API는 BCP-47 언어 태그의 문자열 표현을 수락하고 반환합니다.

[IANA 언어 하위 태그 레지스트리](http://go.microsoft.com/fwlink/p/?linkid=227303)도 참조하세요.

다음 3개 섹션에서는 "사용자 프로필 언어 목록", "앱 매니페스트 언어 목록" 및 "앱 런타임 언어 목록"이라는 용어를 정의합니다. 이 항목 및 이 기능 영역의 다른 항목에서 이러한 용어를 사용할 예정이므로 이 용어의 의미를 알아 두는 것이 중요합니다.

## <a name="user-profile-language-list"></a>사용자 프로필 언어 목록
사용자 프로필 언어 목록은 사용자가 **설정** > **시간 및 언어** > **지역 및 언어** > **언어**에서 구성한 목록 이름입니다. 코드에서 [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) 속성을 사용하여 읽기 전용 문자열 목록으로 사용자 프로필 언어 목록에 액세스할 수 있습니다. 각 문자열은 "en-US" 또는 "ja-JP"와 같이 단일 [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)입니다.

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>앱 매니페스트 언어 목록
앱 매니페스트 언어 목록은 앱에서 지원을 선언하거나 선언할 언어 목록입니다. 이 목록은 개발 주기 동안 앱이 진행됨에 따라 지역화 과정까지 확장될 수 있습니다.

목록은 컴파일할 때 결정되지만 어떻게 나타날지 제어할 수 있는 두 가지 옵션이 있습니다. 한 가지 방법은 Visual Studio가 사용자의 프로젝트 파일에서 목록을 결정하는 것입니다. 그렇게 하려면 먼저 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest`)의 **응용 프로그램** 탭에 있는 **기본 언어**를 설정합니다. 그런 다음 동일한 파일이 이 구성을 포함하는지 확인합니다(기본적으로 이렇게 포함됨).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Visual Studio에서 작성된 앱 패키지 매니페스트 파일(`AppxManifest.xml`)을 만들 때마다 소스 파일의 단일 `Resource` 요소를 프로젝트에서 찾는 모든 언어 한정자 집합으로 확장합니다([언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md) 참조). 예를 들어 지역화를 시작했고 폴더 또는 파일 이름에 en-US", "ja-JP" 및 "fr-FR"이 포함된 문자열, 이미지 및/또는 파일 리소스가 있는 경우 작성된`AppxManifest.xml` 파일에는 다음이 포함됩니다(목록의 첫 번째 항목은 설정한 기본 언어임).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

나머지 옵션은 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest`)의 "x-generate" `<Resource>` 요소 하나를 `<Resource>` 요소의 확장된 목록으로 교체하는 것입니다(기본 언어를 먼저 나열함에 주의할 것). 이 옵션에는 유지 관리할 사항이 더 많지만 사용자 지정 빌드 시스템을 사용할 경우 적합한 옵션일 수 있습니다.

시작하려면 앱 매니페스트 언어 목록에 한 가지 언어만 포함되어야 합니다. 아마도 en-US일 것입니다. 하지만 결국 매니페스트를 수동으로 구성하거나 번역된 리소스를 프로젝트에 추가함에 따라 해당 목록은 길어질 것입니다.

해당 앱이 Microsoft Store에 있는 경우 앱 매니페스트 언어 목록의 언어는 고객에게 표시되는 언어입니다. Microsoft Store에서 지원되는 BCP-47 언어 태그 목록을 보려면 [지원되는 언어](../../publish/supported-languages.md)를 참조하세요.

코드에서 [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) 속성을 사용하여 읽기 전용 문자열 목록으로 앱 매니페스트 언어 목록에 액세스할 수 있습니다. 각 문자열은 단일 BCP-47 언어 태그입니다.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>앱 런타임 언어 목록
세 번째 언어 관심 목록은 방금 설명한 두 목록 사이에 교차됩니다. 런타임 시 앱에서 지원을 선언한 언어 목록(앱 매니페스트 언어 목록)은 사용자가 기본 설정으로 선언한 언어 목록(사용자 프로필 언어 목록)과 비교됩니다. 앱 런타임 언어 목록은 이 교차 부분으로 설정되거나(교차 부분이 비어 있지 않는 경우) 앱의 기본 언어로 설정됩니다(교차 부분이 비어 있는 경우).

특히, 앱 런타임 언어 목록은 다음 항목으로 구성됩니다.

1.  **(옵션) 기본 언어 재정의**. [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)는 사용자에게 고유한 언어를 선택할 수 있는 앱 또는 기본 언어 선택을 재정의해야 하는 뚜렷한 이유가 있는 앱에 대한 간단한 재정의 설정입니다. 자세히 알아보려면 [응용 프로그램 리소스 및 지역화 샘플](http://go.microsoft.com/fwlink/p/?linkid=231501)을 참조하세요.
2.  **앱에서 지원되는 사용자 언어**. 앱 매니페스트 언어 목록에서 필터링된 사용자 프로필 언어 목록입니다. 앱에서 지원하는 언어 목록별로 사용자 언어를 필터링하면 SDK(소프트웨어 개발 키트), 클래스 라이브러리, 종속 프레임워크 패키지 및 앱 간의 일관성이 유지됩니다.
3.  **1과 2가 비어 있는 경우 앱에서 지원되는 기본 언어 또는 첫 번째 언어**. 사용자 프로필 언어 목록에 앱에서 지원되는 언어가 포함되어 있지 않은 경우 앱 런타임 언어가 앱에서 지원되는 첫 번째 언어입니다.

코드에서 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 속성을 사용하여 세미콜론으로 구분된 BCP-47 언어 태그 목록을 포함하는 문자열 형태로 앱 런타임 언어 목록에 액세스할 수 있습니다.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

읽기 전용 문자열 목록으로도 액세스할 수 있습니다. 각 목록에는 단일 BCP-47 언어 태그가 들어 있습니다. [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) 속성 또는 [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) 속성을 사용하여 이를 수행할 수 있습니다.

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

앱 런타임 언어 목록은 Windows에서 앱에 로드하는 리소스와 날짜, 시간, 숫자 및 기타 구성 요소의 형식을 지정하는 데 사용되는 언어를 결정합니다. [날짜/시간/숫자 형식 세계화](use-global-ready-formats.md)를 참조하세요.

**참고** 사용자 프로필 언어 및 앱 매니페스트 언어의 지역 변수가 서로의 지역 변수인 경우 사용자의 지역 변수가 앱 런타임 언어로 사용됩니다. 예를 들어 사용자가 en-GB를 선호하고 앱에서 en-US를 지원하는 경우 앱 런타임 언어는 en-GB입니다. 따라서 날짜, 시간 및 숫자 형식은 사용자의 예상(en-GB)에 가깝게 지정되지만, 언어 일치로 인해 지역화된 리소스는 여전히 앱에서 지원되는 언어(en-US)로 로드됩니다.

## <a name="qualify-resource-files-with-their-language"></a>해당 언어와 관련하여 리소스 파일을 인증합니다.
언어 리소스 한정자를 사용하여 리소스 파일 또는 폴더의 이름을 지정합니다. 리소스 한정자에 대한 자세한 내용은 [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)을 참조하세요. 리소스 파일은 단일 이미지 또는 기타 자산 파일이거나 문자열 리소스를 포함하는 리소스 파일(.resw)과 같은 컨테이너 리소스 파일일 수 있습니다.

**참고** 앱 기본 언어의 리소스도 해당 언어로 정규화되어야 합니다. 예를 들어 앱의 기본 언어가 영어(미국)인 경우 en-US 리소스도 `\Assets\Images\en-US\logo.png`와 같이 정규화합니다. 

- Windows는 복합 일치(예: en-US 및 en-GB와 같은 지역 변수 포함)를 수행합니다 따라서 지역 하위 태그를 적절하게 포함하거나 제외합니다. [리소스 관리 시스템이 언어 태그를 일치하는 방법](../../app-resources/how-rms-matches-lang-tags.md)을 참조하세요.
- 언어에 대해 정의된 억제 스크립트 값이 없는 경우 스크립트를 포함합니다. 언어 태그 세부 사항은 [IANA 언어 하위 태그 레지스트리](http://go.microsoft.com/fwlink/p/?linkid=227303)를 참조하세요. 예를 들어 zh-Hant, zh-Hant-TW, 또는 zh-Hans를 사용하고 zh-CN 또는 zh-TW는 사용하지 않습니다.
- 표준 사투리가 하나 있는 언어의 경우에는 지역을 추가할 필요가 없습니다. 일반적인 태그 지정은 ja-JP 대신 ja로 자산을 표시하는 등의 일부 상황에서는 합리적입니다.
- 일부 도구와 기계 번역기와 같은 다른 구성 요소가 데이터 이해에 도움이 되는 사투리 정보와 같은 특정 언어 태그를 찾을 수도 있습니다.

일부 리소스의 경우 지역화할 필요가 없는 상황이 있습니다.

- 모든 언어에 포함된 UI 문자열과 같은 리소스의 경우 해당 언어로 표시합니다. 이러한 모든 문자열은 기본 언어로 존재하는지 확인합니다.
- 전체 앱의 언어 집합의 하위 집합(부분적인 지역화)으로 제공되는 리소스의 경우에는, 자산 제공 시 사용되는 언어의 집합을 지정하고 이러한 리소스 모두가 기본 언어로 표시되도록 하세요. 예를 들어 앱에 스페인어로 된 전체 리소스 집합이 있을 경우 앱의 일부 UI는 카탈로니아어로 지역화되지 않을 수 있습니다. 카탈로니아어를 사용하고 그 다음으로 스페인어를 사용하는 사용자의 경우 카탈로니아어로 사용할 수 없는 리소스는 스페인어로 나타납니다.
- 일부 언어에서 특정 예외 사항이 있지만 다른 모든 언어는 공통 리소스에 매핑되는 리소스의 경우 모든 언어에 사용해야 하는 리소스는 미확인 언어 태그 'und'로 표시해야 합니다. Windows는 '\*'와 유사하게 와일드카드로 'und' 언어 태그를 해석합니다. 즉, 일치하는 특정 다른 언어를 찾은 후 일치하는 최상위 앱 언어를 찾습니다. 예를 들어 몇 개의 리소스가 핀란드어에 대해 서로 다르지만 나머지 리소스가 모든 언어에 대해 동일할 경우 핀란드어 리소스는 핀란드어 태그로 표시하고 나머지는 'und'로 표시해야 합니다.
- 글꼴이나 텍스트 높이와 같이, 언어 대신 언어의 스크립트를 기반으로 하는 리소스의 경우 미확인 언어 태그를 'und-&lt;script&gt;'와 같이 지정된 스크립트와 함께 사용해야 합니다. 예를 들어, 라틴 글꼴은 `und-Latn\\fonts.css`를 사용하고 키릴 글꼴은 `und-Cryl\\fonts.css`를 사용합니다.

## <a name="set-the-http-accept-language-request-header"></a>HTTP Accept Language 요청 헤더 설정
호출하는 웹 서비스의 지역화 정도가 앱에서 호출하는 것과 동일한지 고려합니다. 일반 웹 요청 및 XHR(XMLHttpRequest)에서 UWP 앱 및 데스크톱 앱의 HTTP 요청에서는 표준 HTTP Accept-Language 요청 헤더를 사용합니다. 기본적으로 HTTP 헤더는 사용자 프로필 언어 목록으로 설정됩니다. 목록의 각 언어는 언어 중립과 가중치(q)를 포함하도록 확장됩니다. 예를 들어 fr-FR 및 en-US로 구성된 사용자의 언어 목록은 HTTP Accept-Language 요청 헤더가 fr-FR, fr, en-US, en("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3")으로 나타납니다. 그러나 예를 들어 날씨 앱의 UI가 프랑스어(프랑스)로 표시되지만 사용자의 기본 설정 목록에 독일어가 최상위 언어로 표시되는 경우 해당 앱에서 일관되게 표시되도록 서비스에서 프랑스어(프랑스)를 명시적으로 요청해야 합니다.

## <a name="apis-in-the-windowsglobalization-namespace"></a>Windows.Globalization 네임스페이스의 API
일반적으로 [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) 네임스페이스의 API는 앱 런타임 언어 목록을 사용하여 언어를 결정합니다. 일치하는 형식을 가진 언어가 없으면 사용자 로캘이 사용됩니다. 이 로캘은 시스템 시계에 사용된 것과 같습니다. 사용자 로캘은 **설정** > **시간 및 언어** > **지역 및 언어** > **추가 날짜, 시간 및 국가별 설정** > **지역: 날짜, 시간 또는 숫자 형식 변경**에서 사용할 수 있습니다. 또한 **Windows.Globalization** API는 응용 앱 런타임 언어 목록 대신 사용할 언어 목록을 지정하는 재정의를 수락합니다.

[**Language**](/uwp/api/windows.globalization.language?branch=live) 클래스를 사용하여 특정 언어에 대한 세부 정보(예: 언어 스크립트, 표시 이름, 기본 이름)를 검사할 수 있습니다.

## <a name="use-geographic-region-when-appropriate"></a>필요할 경우 지리적 지역 사용
**설정** > **시간 및 언어** > **지역 및 언어** > **국가 또는 지역**에서 사용자는 전 세계 위치 중 자신의 위치를 지정할 수 있습니다. 언어 대신 이 설정을 사용하여 사용자에게 표시할 콘텐츠를 선택해야 합니다. 예를 들어, 뉴스 앱에 해당 지역의 콘텐츠가 표시되도록 기본 설정할 수 있습니다.

코드에서 [**GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) 속성을 사용하여 이 설정에 액세스할 수 있습니다.

[**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) 클래스를 사용하여 특정 지역에 대한 세부 정보(예: 표시 이름, 기본 이름, 사용 중인 통화)를 검사할 수 있습니다.

## <a name="examples"></a>예
다음 표에는 다양한 언어 및 국가별 설정에 따라 앱의 UI로 표시되는 항목의 예가 나와 있습니다.

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
<th align="left">앱 매니페스트 언어 목록</th>
<th align="left">사용자 프로필 언어 목록</th>
<th align="left">앱의 기본 언어 재정의(옵션)</th>
<th align="left">앱 런타임 언어 목록</th>
<th align="left">앱에 표시되는 항목</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">영어(GB)(기본값); 독일어(독일)</td>
<td align="left">영어(GB)</td>
<td align="left">없음</td>
<td align="left">영어(GB)</td>
<td align="left">UI: 영어(GB)<br>날짜/시간/숫자: 영어(GB)</td>
</tr>
<tr>
<td align="left">독일어(독일)(기본값); 프랑스어(프랑스); 이탈리아어(이탈리아)</td>
<td align="left">프랑스어(오스트리아)</td>
<td align="left">없음</td>
<td align="left">프랑스어(오스트리아)</td>
<td align="left">UI: 프랑스어(프랑스) (프랑스어(오스트리아)에서 대체)<br>날짜/시간/숫자: 프랑스어(오스트리아)</td>
</tr>
<tr>
<td align="left">영어(미국)(기본값); 프랑스어(프랑스); 영어(GB)</td>
<td align="left">영어(캐나다); 프랑스어(캐나다)</td>
<td align="left">없음</td>
<td align="left">영어(캐나다); 프랑스어(캐나다)</td>
<td align="left">UI: 영어(US)(영어(캐나다)에서 대체)<br>날짜/시간/숫자: 영어(캐나다)</td>
</tr>
<tr>
<td align="left">스페인어(스페인)(기본값); 스페인어(멕시코); 스페인어(라틴 아메리카); 포르투갈어(브라질)</td>
<td align="left">영어(미국)</td>
<td align="left">없음</td>
<td align="left">스페인어(스페인)</td>
<td align="left">UI: 스페인어(스페인)(영어에 대해 사용 가능한 대체 언어가 없으므로 기본값 사용)<br>날짜/시간/숫자 스페인어(스페인)</td>
</tr>
<tr>
<td align="left">카탈로니아어(기본값); 스페인어(스페인); 프랑스어(프랑스)</td>
<td align="left">카탈로니아어; 프랑스어(프랑스)</td>
<td align="left">없음</td>
<td align="left">카탈로니아어; 프랑스어(프랑스)</td>
<td align="left">UI: 대부분 카탈로니아어 및 일부 프랑스어(프랑스)(일부 문자열이 카탈로니아어로 되어 있지 않음)<br>날짜/시간/숫자: 카탈로니아어</td>
</tr>
<tr>
<td align="left">영어(GB)(기본값); 프랑스어(프랑스); 독일어(독일)</td>
<td align="left">독일어(독일); 영어(GB)</td>
<td align="left">영어(GB)(앱의 UI에서 사용자가 선택)</td>
<td align="left">영어(GB); 독일어(독일)</td>
<td align="left">UI: 영어(GB)(언어 재정의)<br>날짜/시간/숫자 영어(GB)</td>
</tr>
</tbody>
</table>

## <a name="important-apis"></a>중요 API
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [언어](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>관련 항목
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [IANA 언어 하위 태그 레지스트리](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [지원되는 언어](../../publish/supported-languages.md)
* [날짜/시간 숫자 형식 세계화](use-global-ready-formats.md)
* [리소스 관리 시스템이 언어 태그를 일치하는 방법](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>샘플
* [응용 프로그램 리소스 및 지역화 샘플(영문)](http://go.microsoft.com/fwlink/p/?linkid=231501)
