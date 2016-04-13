---
Windows에서 제공되는 다양한 언어 및 지역 설정을 사용하여 Windows에서 UI 리소스를 선택하고 앱의 UI 요소 형식을 지정하는 방법을 제어합니다.
언어 및 지역 관리
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
언어 및 지역 관리
template: detail.hbs
---

# 언어 및 지역 관리


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**WinJS.Resources 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/br229779)

Windows에서 제공되는 다양한 언어 및 지역 설정을 사용하여 Windows에서 UI 리소스를 선택하고 앱의 UI 요소 형식을 지정하는 방법을 제어합니다.

## <span id="Introduction"> </span> <span id="introduction"> </span> <span id="INTRODUCTION"> </span>소개


언어 및 지역 설정을 관리하는 방법을 설명하는 샘플 앱은 [응용 프로그램 리소스 및 지역화 샘플](http://go.microsoft.com/fwlink/p/?linkid=231501)(영문)을 참조하세요.

Windows 사용자는 제한된 언어 집합에서 언어를 하나만 선택할 필요가 없습니다. 대신 Windows에서 사용할 언어를 지정할 수 있습니다. 이는 Windows가 해당 언어로 번역되어 있지 않은 경우에도 마찬가지입니다. 여러 언어를 지정할 수도 있습니다.

Windows 사용자는 전 세계의 아무 위치나 지정할 수 있습니다. 또한 위치에 상관없이 아무 언어나 지정할 수 있습니다. 위치와 언어가 상호 제한적이지 않습니다. 사용자가 프랑스어를 사용한다고 해서 프랑스에 있는 것을 의미하는 것은 아니고, 프랑스에 있다고 해서 프랑스어를 사용하는 것을 의미하지는 않습니다.

Windows 사용자는 Windows와 완전히 다른 언어로 앱을 실행할 수 있습니다. 예를 들어 영어로 실행 중인 Windows에서 스페인어로 앱을 실행할 수 있습니다.

Windows 스토어 앱의 경우 언어는 [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)로 나타냅니다. Windows 런타임 HTML 및 XAML에서 대부분의 API는 이러한 BCP-47 언어 태그의 문자열 표현을 반환하거나 수락할 수 있습니다. 또한 [IANA 언어 목록](http://go.microsoft.com/fwlink/p/?linkid=227303)(영문)을 참조하세요.

Windows 스토어에서 지원되는 언어 태그 목록을 구체적으로 보려면 [지원되는 언어](https://msdn.microsoft.com/library/windows/apps/jj657969)를 참조하세요.

## <span id="Tasks"> </span> <span id="tasks"> </span> <span id="TASKS"> </span>작업


### <span id="Users_can_set_their_language_preferences."> </span> <span id="users_can_set_their_language_preferences."> </span> <span id="USERS_CAN_SET_THEIR_LANGUAGE_PREFERENCES."> </span>사용자가 언어 선택을 지정할 수 있습니다.

사용자 언어 기본 설정 목록은 사용자의 언어를 사용자가 원하는 순서대로 설명하는 순서가 지정된 언어 목록입니다.

사용자는 **설정** &gt; **시간 및 언어** &gt; **지역 및 언어**에서 목록을 설정합니다. 또는 **제어판** &gt; **시계, 언어 및 지역**을 사용할 수 있습니다.

사용자의 언어 기본 설정 목록에는 여러 언어 및 국가별 또는 다른 특정 언어가 포함될 수 있습니다. 예를 들어 사용자가 fr-CA를 기본 설정했지만 en-GB를 이해할 수도 있습니다.

### <span id="Specify_the_supported_languages_in_the_app_s_manifest."> </span> <span id="specify_the_supported_languages_in_the_app_s_manifest."> </span> <span id="SPECIFY_THE_SUPPORTED_LANGUAGES_IN_THE_APP_S_MANIFEST."> </span>앱의 매니페스트에서 지원되는 언어를 지정합니다.

앱의 매니페스트 파일(보통 Package.appxmanifest)의 [**Resources element**](https://msdn.microsoft.com/library/windows/apps/dn934770)에서 앱의 지원되는 언어 목록을 지정하거나, Visual Studio가 프로젝트에 있는 언어를 기반으로 매니페스트 파일에서 언어 목록을 자동으로 생성합니다. 매니페스트는 지원되는 언어를 적절한 수준으로 정확하게 설명해야 합니다. 매니페스트에 나열된 언어는 Windows 스토어에서 사용자에게 표시되는 언어입니다.

### <span id="Specify_the_default_language."> </span> <span id="specify_the_default_language."> </span> <span id="SPECIFY_THE_DEFAULT_LANGUAGE."> </span>기본 언어를 지정합니다.

Visual Studio에서 package.appxmanifest를 열고 **응용 프로그램** 탭으로 이동한 다음 기본 언어를 응용 프로그램을 작성하는 데 사용 중인 언어로 설정합니다.

앱에서 사용자가 선택한 언어가 지원되지 않는 경우 기본 언어가 사용됩니다. Visual Studio는 런타임에 적절한 자산을 선택할 수 있도록 기본 언어를 사용하여 해당 언어로 표시된 자산에 메타데이터를 추가합니다.

또한 응용 프로그램 언어를 적절하게 설정하려면 기본 언어 속성을 매니페스트의 첫 번째 언어로 설정해야 합니다(아래의 "응용 프로그램 언어 목록을 만듭니다" 단계 설명 참조). 기본 언어의 리소스는 해당 언어(예: en-US/logo.png)로 한정되어야 합니다. 기본 언어는 정규화되지 않은 자산의 암시적 언어를 지정하지 않습니다. 자세한 내용은 [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)을 참조하세요.

### <span id="Qualify_resources_with_their_language."> </span> <span id="qualify_resources_with_their_language."> </span> <span id="QUALIFY_RESOURCES_WITH_THEIR_LANGUAGE."> </span>해당 언어와 관련하여 리소스를 인증합니다.

대상을 신중히 고려하고 대상으로 할 사용자의 언어 및 위치도 고려합니다. 어떤 지역에 사는 많은 사람이 그 지역의 기본 언어를 선호하지 않습니다. 예를 들어 미국에는 스페인어를 기본 언어로 사용하는 수백만 가구가 있습니다.

언어와 관련하여 리소스를 인증할 때에는,

-   언어에 대해 정의된 억제 스크립트 값이 없는 경우 스크립트를 포함합니다. 언어 태그 세부 사항에 대한 자세한 내용은 [IANA 하위 태그 레지스트리](http://go.microsoft.com/fwlink/p/?linkid=227303)를 참조하세요. 예를 들어 zh-Hant, zh-Hant-TW, 또는 zh-Hans를 사용하고 zh-CN 또는 zh-TW는 사용하지 않습니다.
-   언어와 관련한 모든 언어적 내용에 표시합니다. 기본 언어 프로젝트 속성은 표시되지 않은 리소스의 언어(즉, 중립 언어)가 아닙니다. 이렇게 하면 다른 표시된 언어 리소스가 사용자와 일치하지 않을 경우 표시된 언어 리소스 중 선택해야 할 어느 항목이 지정됩니다.

내용의 정확한 표현으로 자산을 표시합니다.

-   Windows는 교차 국가별 변형어(en-US ~ en-GB 등)를 포함하여 복잡한 일치 작업을 수행하므로 응용 프로그램은 언제든지 내용의 정확한 표현으로 자산을 표시하고 Windows에서 각 사용자에게 적절히 맞추도록 할 수 있습니다.
-   Windows 스토어는 응용 프로그램을 보고 있는 사용자에게 매니페스트에 있는 내용을 표시합니다.
-   일부 도구와 기계 번역기와 같은 다른 구성 요소가 데이터 이해에 도움이 되는 사투리 정보와 같은 특정 언어 태그를 찾을 수도 있습니다.
-   특히 여러 변형을 사용할 수 있으면 반드시 세부 사항 전체를 사용하여 자산을 표시하세요. 예를 들어 en-GB와 en-US 모두 해당 지역에서 사용하는 경우 두 언어를 표시하세요.
-   표준 사투리가 하나 있는 언어의 경우에는 지역을 추가할 필요가 없습니다. 일반적인 태그 지정은 ja-JP 대신 ja로 자산을 표시하는 등의 일부 상황에서는 합리적입니다.

때로 일부 리소스의 경우 지역화할 필요가 없는 상황이 있습니다.

-   모든 언어로 표시되는 UI 문자열과 같은 리소스의 경우 이 리소스가 사용하는 적절한 언어로 리소스를 표시하고 이러한 리소스 모두가 기본 언어로 표시되도록 하세요. 중립 리소스(언어로 표시되지 않은 리소스)를 지정할 필요는 없습니다.
-   전체 응용 프로그램의 언어 집합의 하위 집합(부분적인 지역화)으로 제공되는 리소스의 경우에는, 자산 제공 시 사용되는 언어의 집합을 지정하고 이러한 리소스 모두가 기본 언어로 표시되도록 하세요. Windows에서는 선호 순서로 사용자가 말하는 모든 언어를 확인하여 사용자에게 가능한 최상의 언어를 선택합니다. 예를 들어 앱에 스페인어로 된 전체 리소스 집합이 있을 경우 앱의 일부 UI는 카탈로니아어로 지역화되지 않을 수 있습니다. 카탈로니아어를 사용하고 그 다음으로 스페인어를 사용하는 사용자의 경우 카탈로니아어로 사용할 수 없는 리소스는 스페인어로 나타납니다.
-   일부 언어에서 특정 예외 사항이 있고 다른 모든 언어는 공통 리소스에 매핑되는 리소스의 경우 모든 언어에 사용해야 하는 리소스는 미확인 언어 태그 'und'로 표시해야 합니다. Windows는 '\*'와 유사한 방식으로 'und' 언어 태그를 해석합니다. 즉, 일치하는 특정 다른 언어를 찾은 후 일치하는 최상위 응용 프로그램 언어를 찾습니다. 예를 들어 몇 개의 리소스(요소 너비 등)가 핀란드어에 대해 서로 다르지만 나머지 리소스가 모든 언어에 대해 동일할 경우 핀란드어 리소스는 핀란드어 태그로 표시하고 나머지는 'und'로 표시해야 합니다.
-   글꼴이나 텍스트 높이와 같이, 언어 대신 언어의 스크립트를 기반으로 하는 리소스의 경우 미확인 언어 태그를 'und-&lt;script&gt;'와 같이 지정된 스크립트와 함께 사용해야 합니다. 예를 들어 라틴 글꼴의 경우 und-Latn\\fonts.css를 사용하고 키릴 글꼴의 경우 und-Cryl\\fonts.css를 사용합니다.

### <span id="Create_the_application_language_list."> </span> <span id="create_the_application_language_list."> </span> <span id="CREATE_THE_APPLICATION_LANGUAGE_LIST."> </span>응용 프로그램 언어 목록을 만듭니다.

런타임 시, 시스템은 앱이 매니페스트에 대한 지원을 선언하는 사용자 언어 기본 설정을 결정하고 *응용 프로그램 언어 목록*을 만듭니다. 이 목록을 사용하여 응용 프로그램이 사용해야 할 언어를 결정합니다. 이 목록은 앱 및 시스템 리소스, 날짜, 시간, 숫자 및 기타 구성 요소에 대해 사용되는 언어를 결정합니다. 예를 들어 리소스 관리 시스템([**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 및 [**WinJS.Resources 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/br229779))은 응용 프로그램 언어에 따라 UI 리소스를 로드합니다. 또한 [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)은 응용 프로그램 언어 목록을 기반으로 형식을 선택합니다. 응용 프로그램 언어 목록은 [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396)를 통해 사용할 수 있습니다.

리소스에 따라 일치하는 언어를 찾는 것은 어렵습니다. 언어 태그에 대해서는 일치 우선 순위에 영향을 주는 선택적 구성 요소가 많고 실제로 이러한 구성 요소를 접할 수 있으므로 Windows가 일치 작업을 처리하도록 하는 것이 좋습니다.

언어 태그에 있는 선택적 구성 요소의 예는 다음과 같습니다.

-   억제 스크립트 언어용 스크립트. 예를 들어 en-Latn-US는 en-US와 일치합니다.
-   지역. 예를 들어 en-US는 en과 일치합니다.
-   변형어. 예를 들어 de-DE-1996은 de-DE와 일치합니다.
-   -x 및 기타 확장. 예를 들어 en-US-x-Pirate는 en-US와 일치합니다.

또한 언어 태그에 대해 형식 xx 또는 xx-yy가 아니면서 모두 일치하지는 않는 많은 구성 요소가 있습니다.

-   zh-Hant는 zh-Hans와 일치하지 않습니다.

Windows에서는 언어의 일치를 잘 이해할 수 있는 표준 방식으로 우선 순위를 지정합니다. 예를 들어 en-US는 en-US, en, en-GB 등의 우선하는 순서로 일치합니다.

-   Windows는 교차 지역 일치 작업을 수행합니다. 예를 들어 en-US는 en-US와 일치한 다음 en과 일치하고, 그다음 en-\*와 일치합니다.
-   Windows에는 언어에 대한 기본 지역과 같이, 지역 내 선호도 일치를 위한 추가 데이터가 있습니다. 예를 들어 fr-FR은 fr-CA보다 fr-BE와 더 일치합니다.
-   Windows에서의 언어 일치에 대한 향후의 모든 개선 사항은 Windows API 사용 시 무료로 받을 수 있습니다.

목록의 첫 번째 언어에 대한 일치는 목록의 두 번째 언어에 대한 일치보다 앞에 나타나며, 이는 다른 국가별 변형에 대해서도 마찬가지입니다. 예를 들어 응용 프로그램 언어가 en-US인 경우 en-GB에 대한 리소스가 fr-CA 리소스보다 우선적으로 선택되고, en 형식에 대한 리소스가 없는 경우에만 fr-CA에 대한 리소스가 선택됩니다.

응용 프로그램 언어 목록은 사용자의 국가별 변형으로 설정되며, 이는 사용자의 국가별 변형이 앱에 제공된 국가별 변형과 다른 경우에도 마찬가지입니다. 예를 들어 사용자는 en-GB를 사용하지만 앱에서는 en-US를 지원하는 경우 응용 프로그램 언어 목록은 en-GB를 포함합니다. 따라서 날짜, 시간 및 숫자 형식은 사용자의 예상(en-GB)에 가깝게 지정되지만, 언어 일치로 인해 UI 리소스는 여전히 앱에서 지원되는 언어(en-US)로 로드됩니다.

응용 프로그램 언어 목록은 다음 항목으로 구성됩니다.

1.  **(옵션) 기본 언어 재정의** [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398)는 고유의 독립적인 언어 옵션을 사용자에게 제공하는 앱이나 특별한 이유가 있어서 기본 언어 옵션을 재정의해야 하는 앱을 위한 단순 재정의 설정입니다. 자세히 알아보려면 [응용 프로그램 리소스 및 지역화 샘플](http://go.microsoft.com/fwlink/p/?linkid=231501)을 참조하세요.
2.  **앱에서 지원되는 사용자 언어.** 이 목록은 언어 선택 순서대로 나열된 사용자의 언어 선택 목록입니다. 이 목록은 앱의 매니페스트에서 지원되는 언어 목록을 기준으로 필터링됩니다. 앱에서 지원하는 언어 목록별로 사용자 언어를 필터링하면 SDK(소프트웨어 개발 키트), 클래스 라이브러리, 종속 프레임워크 패키지 및 앱 간의 일관성이 유지됩니다.
3.  **1과 2가 비어 있는 경우 앱에서 지원되는 기본 언어 또는 첫 번째 언어.** 사용자가 구사하는 언어가 앱에서 지원되는 언어가 아닌 경우 선택한 응용 프로그램 언어가 앱에서 지원되는 첫 번째 언어입니다.

예제는 아래의 설명 섹션을 참조하세요.

### <span id="Set_the_HTTP_Accept_Language_header."> </span> <span id="set_the_http_accept_language_header."> </span> <span id="SET_THE_HTTP_ACCEPT_LANGUAGE_HEADER."> </span>HTTP Accept Language 헤더를 설정합니다.

일반 웹 요청 및 XHR(XMLHttpRequest)에서 Windows 스토어 앱 및 데스크톱 앱의 HTTP 요청에서는 표준 HTTP Accept-Language 헤더를 사용합니다. 기본적으로 HTTP 헤더는 **설정** &gt; **시간 및 언어** &gt; **국가 및 언어**에서 지정한 대로 사용자의 언어 선택(사용자가 선호하는 순서)으로 지정됩니다. 목록의 각 언어는 언어 중립과 가중치(q)를 포함하도록 확장됩니다. 예를 들어 fr-FR 및 en-US로 구성된 사용자의 언어 목록은 HTTP Accept-Language 헤더가 fr-FR, fr, en-US, en("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3")으로 나타납니다.

### <span id="Use_the_APIs_in_the_Windows.Globalization_namespace."> </span> <span id="use_the_apis_in_the_windows.globalization_namespace."> </span> <span id="USE_THE_APIS_IN_THE_WINDOWS.GLOBALIZATION_NAMESPACE."> </span>Windows.Globalization 네임스페이스의 API를 사용합니다.

일반적으로 [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 네임스페이스의 API 요소는 응용 프로그램 언어 목록을 사용하여 언어를 결정합니다. 일치하는 형식을 가진 언어가 없으면 사용자 로캘이 사용됩니다. 이 로캘은 시스템 시계에 사용된 것과 같습니다. 사용자 로캘은 **설정** &gt; **시간 및 언어** &gt; **지역 및 언어** &gt; **추가 날짜, 시간 및 국가별 설정** &gt; **지역: 날짜, 시간 또는 숫자 형식 변경**에서 사용할 수 있습니다. 또한 **Windows.Globalization** API는 응용 프로그램 언어 목록 대신 사용할 언어 목록을 지정하는 재정의를 수락합니다.

또한 [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)에는 도우미 개체로 제공되는 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 개체가 있습니다. 이 개체를 통해 앱에서 언어에 대한 세부 정보(예: 언어 스크립트, 표시 이름, 기본 이름)를 검사할 수 있습니다.

### <span id="Use_geographic_region_when_appropriate."> </span> <span id="use_geographic_region_when_appropriate."> </span> <span id="USE_GEOGRAPHIC_REGION_WHEN_APPROPRIATE."> </span>필요할 경우 지리적 지역을 사용합니다.

언어 대신 사용자 홈의 지리적 지역 설정을 사용하여 사용자에게 표시할 콘텐츠를 선택해야 합니다. 예를 들어 새 앱은 사용자의 홈 위치에서 콘텐츠를 표시하도록 기본 설정될 수 있습니다. 사용자의 홈 위치는 Windows를 설치할 때 설정되며 이전 작업에서 설명된 대로 Windows UI의 **지역: 날짜, 시간 또는 숫자 형식 변경**에서 사용할 수 있습니다. [
            **Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829)을 사용하여 현재 사용자 홈의 국가별 설정을 검색할 수 있습니다.

또한 [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)에는 도우미 개체로 제공되는 [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795) 개체가 있습니다. 이 개체를 통해 앱에서 특정 지역에 대한 세부 정보(예: 표시 이름, 기본 이름, 사용 중인 통화)를 검색할 수 있습니다.

## <span id="Remarks"> </span> <span id="remarks"> </span> <span id="REMARKS"> </span>설명


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
<th align="left">앱에서 지원되는 언어(매니페스트에 정의됨)</th>
<th align="left">사용자 언어 기본 설정(제어판에서 설정)</th>
<th align="left">앱의 기본 언어 재정의(옵션)</th>
<th align="left">앱 언어</th>
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

 

## <span id="related_topics"> </span>관련 항목


* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [IANA 언어 목록](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [응용 프로그램 리소스 및 지역화 샘플(영문)](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [지원되는 언어](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 





<!--HONumber=Mar16_HO1-->


