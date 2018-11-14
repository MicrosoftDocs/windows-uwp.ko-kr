---
author: stevewhims
Description: There are several URI (Uniform Resource Identifier) schemes that you can use to refer to files that come from your app's package, your app's data folders, or the cloud. You can also use a URI scheme to refer to strings loaded from your app's Resources Files (.resw).
title: URI 스키마
template: detail.hbs
ms.author: stwhi
ms.date: 10/16/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 75ba42674ca1ea460698fcce6e67bb3528589797
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6251612"
---
# <a name="uri-schemes"></a>URI 스키마

앱 패키지, 앱의 데이터 폴더 또는 클라우드에서 파일을 참조하는 데 사용할 수 있는 몇 가지 URI (Uniform Resource Identifier) 스키마가 있습니다. URI 스키마를 사용하여 앱의 리소스 파일(.resw)에서 로드되는 문자열을 참조할 수 있습니다. 코드, XAML 태그, 앱 패키지 매니페스트, 또는 사용자 타일 및 알림 메시지 템플릿에서 이러한 URI 스키마를 사용할 수 있습니다.

## <a name="common-features-of-the-uri-schemes"></a>URI 스키마의 일반적인 기능

이 항목에 설명된 모든 스키마는 표준화 및 리소스 검색에 대한 일반적인 URI 스키마 규칙을 따릅니다. URI의 일반적인 구문에 대해 [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444)을 참조하세요.

모든 URI 스키마는 [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444)에 따라 계층 구조 부분을 기관과 URI의 경로 구성 요소로 정의합니다.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

즉, URI에는 세 가지 기본 구성 요소가 있습니다. URI *스키마*의 두 슬래시 바로 뒤에 나오는 것은 *기관*이라 불리는 구성 요소입니다(비어 있을 수 있음). 그리고 그 뒤를 바로 따르는 것이 *경로*입니다. URI `http://www.contoso.com/welcome.png`를 예로 들면 스키마는 "`http://`", 기관은 "`www.contoso.com`", 경로는 "`/welcome.png`"입니다. 다른 예로 URI `ms-appx:///logo.png`의 경우 기관 구성 요소는 비어 있으며 기본값을 가집니다.

이 항목에서 언급된 특정 스키마 URI 처리에서 조각 구성 요소는 무시됩니다. 리소스를 검색 및 비교하는 동안 조각 구성 요소는 영향을 미치지 않습니다. 그러나 특정 구현 위의 계층은 보조 리소스를 검색하기 위해 조각을 해석할 수 있습니다.

비교는 모든 IRI 구성 요소의 표준화 이후 바이트 단위로 발생합니다.

## <a name="case-insensitivity-and-normalization"></a>대/소문자 구분 사용 안 함 및 표준화

이 항목에 설명된 모든 URI 스키마는 스키마에 대한 표준화 및 리소스 검색에 대한 일반적인 URI 스키마 규칙(RFC 3986)을 따릅니다. 이러한 URI의 표준화된 형태는 대소문자 및 퍼센트 디코드 RFC 3986 예약되지 않은 문자를 유지합니다.

이 항목에 설명된 모든 URI 스키마의 경우 *스키마*, *기관*, 및 *경로*는 표준 방법으로 대/소문자를 구분하거나 시스템에 의해 대/소문자 구분이 처리됩니다. **참고** 규칙의 예외는 `ms-resource` *기관*이 있으며 대/소문자를 구분합니다.

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx 및 ms-appx-web

`ms-appx` 또는 `ms-appx-web` URI 스키마를 사용하여 앱의 패키지에서 제공되는 파일을 참조할 수 있습니다([앱 패키징](../packaging/index.md) 참조). 앱 패키지의 파일은 일반적으로 정적 이미지, 데이터, 코드 및 레이아웃 파일입니다. `ms-appx-web` 스키마는 웹 구획의 `ms-appx`와 동일한 파일에 액세스합니다. 이에 대한 예와 자세한 내용은 [XAML 태그와 코드에서 이미지 또는 다른 자산 참조](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)를 참조하세요.

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>스키마 이름(ms-appx 및 ms-appx-web)

URI 스키마 이름은 "ms-appx" 또는 "ms-appx-web" 문자열입니다.

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>기관(ms-appx 및 ms-appx-web)

기관은 패키지 매니페스트에 정의된 패키지 ID 이름입니다. 따라서 이는 URI 및 IRI(Internationalized resource identifier) 양식 둘 다에서 패키지 ID 이름에 사용할 수 있는 문자로 제한됩니다. 패키지 이름은 현재 실행 중인 앱 패키지 종속성 그래프의 패키지 중 하나의 이름이어야 합니다.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

기관에 다른 문자가 나타나면 검색 및 비교는 실패합니다. 기관에 대한 기본값은 현재 실행 중인 앱 패키지입니다.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>사용자 정보 및 포트(ms-appx 및 ms-appx-web)

다른 인기 있는 스키마와 달리 `ms-appx` 스키마는 사용자 정보 또는 포트 구성 요소를 정의하지 않습니다. 따라서 "@" and ":"는 유효한 기관 값으로 허용되지 않으며 이것이 포함될 경우 조회가 실패합니다. 다음이 실패합니다.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>경로(ms-appx 및 ms-appx-web)

경로 구성 요소는 일반 RFC 3986 구문과 일치하며 IRI에서 비 ASCII 문자를 지원합니다. 경로 구성 요소는 파일의 논리 또는 실제 파일 경로를 정의합니다. 해당 파일은 기관에서 지정한 앱에 대해 앱 패키지가 설치된 위치와 관련된 폴더에 있습니다.

경로가 실제 경로와 파일 이름을 참조하는 경우 해당 실제 파일 자산이 검색됩니다. 하지만 이러한 실제 파일이 없으면 검색하는 동안 반환되는 실제 리소스는 실행 시 콘텐츠 협상을 사용하여 결정됩니다. 이 결정은 언어, 디스플레이 배율 인수, 테마, 고대비 및 기타 런타임 컨텍스트와 같은 앱, 운영 체제, 사용자 설정을 따릅니다. 예를 들어 검색할 실제 리소스 값을 결정할 때 사용자의 언어, 시스템의 디스플레이 설정, 사용자의 고대비 설정의 조합을 고려할 수 있습니다.

```xml
ms-appx:///images/logo.png
```

위의 URI은 다음 실제 파일 이름의 현재 앱의 패키지 내에서 파일을 실제로 검색할 수도 있습니다.

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

물론 전체 이름을 직접 참조하여 동일한 실제 파일을 검색할 수도 있습니다.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

`ms-appx(-web)`의 경로 구성 요소는 일반 URI와 같이 대/소문자를 구분합니다. 그러나 리소스에 액세스하는 기본 파일 시스템이 대/소문자를 구분하는 경우(예: NTFS) 리소스 검색은 대/소문자를 구분합니다.

URI의 표준화된 형태는 대소문자와 퍼센트 디코드(두자리 16진수가 따르는 "%" 기호) RFC 3986 예약되지 않은 문자를 유지합니다. 문자 "?", "#", "/", "*", '”'(큰따옴표)는 파일 또는 폴더 이름 같은 데이터를 나타내는 경로에 %로 인코딩되어야 합니다. 검색하기 전에 모든 퍼센트로 인코딩된 문자를 디코딩합니다. 따라서 Hello#World.html이라는 파일을 검색하려면 이 URI를 사용합니다.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>쿼리(ms-appx 및 ms-appx-web)

리소스를 검색하는 동안 쿼리 매개 변수는 무시됩니다. 쿼리 매개 변수의 표준화된 형태는 대/소문자를 유지합니다. 비교하는 동안 쿼리 매개 변수는 무시되지 않습니다.

## <a name="ms-appdata"></a>ms-appdata

`ms-appdata` URI 스키마를 사용하여 앱의 로컬, 로밍, 임시 데이터 폴더에서 파일을 참조합니다. 이러한 앱 데이터 폴더에 대한 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)을 참조하세요.

`ms-appdata` URI 스키마는 [ms- appx 및 ms-appx-web](#ms-appx-and-ms-appx-web)이 수행하는 런타임 콘텐츠 협상을 수행하지 않습니다. 그러나 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)의 콘텐츠에 응답하고 URI에 전체 실제 파일 이름을 사용하여 앱 데이터를 로드할 수 있습니다.

### <a name="scheme-name-ms-appdata"></a>스키마 이름(ms-appdata)

URI 스키마 이름은 "ms-appdata" 문자열입니다.

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>기관(ms-appdata)

기관은 패키지 매니페스트에 정의된 패키지 ID 이름입니다. 따라서 이는 URI 및 IRI(Internationalized resource identifier) 양식 둘 다에서 패키지 ID 이름에 사용할 수 있는 문자로 제한됩니다. 패키지 이름은 현재 실행 중인 앱 패키지 이름과 동일해야 합니다.

```xml
ms-appdata://Contoso.MyApp/
```

기관에 다른 문자가 나타나면 검색 및 비교는 실패합니다. 기관에 대한 기본값은 현재 실행 중인 앱 패키지입니다.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>사용자 정보 및 포트(ms-appdata)

다른 인기 있는 스키마와 달리 `ms-appdata` 스키마는 사용자 정보 또는 포트 구성 요소를 정의하지 않습니다. 따라서 "@" and ":"는 유효한 기관 값으로 허용되지 않으며 이것이 포함될 경우 조회가 실패합니다. 다음이 실패합니다.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>경로(ms-appdata)

경로 구성 요소는 일반 RFC 3986 구문과 일치하며 IRI에서 비 ASCII 문자를 지원합니다. [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) 위치 내에는 로컬, 로밍, 임시 상태 저장소에 대해 예약된 세 개의 예약된 폴더가 있습니다. `ms-appdata` 스키마를 사용하여 이러한 위치에 있는 파일 및 폴더에 액세스할 수 있습니다. 경로 구성 요소의 첫 번째 세그먼트는 다음과 같은 방법으로 특정 폴더를 지정해야 합니다. 따라서 ""hier-part"의 "path-empty" 형식은 잘못되었습니다.

로컬 폴더.

```xml
ms-appdata:///local/
```

임시 폴더.

```xml
ms-appdata:///temp/
```

로밍 폴더.

```xml
ms-appdata:///roaming/
```

`ms-appdata`의 경로 구성 요소는 일반 URI와 같이 대/소문자를 구분합니다. 그러나 리소스에 액세스하는 기본 파일 시스템이 대/소문자를 구분하는 경우(예: NTFS) 리소스 검색은 대/소문자를 구분합니다.

URI의 표준화된 형태는 대소문자와 퍼센트 디코드(두자리 16진수가 따르는 "%" 기호) RFC 3986 예약되지 않은 문자를 유지합니다. 문자 "?", "#", "/", "*", '”'(큰따옴표)는 파일 또는 폴더 이름 같은 데이터를 나타내는 경로에 %로 인코딩되어야 합니다. 검색하기 전에 모든 퍼센트로 인코딩된 문자를 디코딩합니다. 따라서 Hello#World.html이라는 로컬 파일을 검색하려면 이 URI를 사용합니다.

```xml
ms-appdata://local/Hello%23World.html
```

리소스 검색과 최상위 수준 경로 세그먼트 식별은 마침표 표준화 후 처리됩니다 (".././b/c"). 따라서 URI는 예약된 폴더 중 하나에서 직접 마침표를 삽입할 수 없습니다. 그러므로 다음 URI는 허용되지 않습니다.

```xml
ms-appdata:///local/../hello/logo.png
```

그러나 이 URI는 허용됩니다(중복임에도 불구하고).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>쿼리(ms-appdata)

리소스를 검색하는 동안 쿼리 매개 변수는 무시됩니다. 쿼리 매개 변수의 표준화된 형태는 대/소문자를 유지합니다. 비교하는 동안 쿼리 매개 변수는 무시되지 않습니다.

## <a name="ms-resource"></a>ms-resource

`ms-resource` URI 스키마를 사용하여 앱의 리소스 파일(.resw)에서 로드되는 문자열을 참조할 수 있습니다. 리소스 파일의 예와 자세한 내용은 [UI와 앱 패키지 매니페스트에 문자열 지역화](localize-strings-ui-manifest.md)를 참조하세요.

### <a name="scheme-name-ms-resource"></a>스키마 이름(ms-resource)

URI 스키마 이름은 "ms-resource" 문자열입니다.

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>기관(ms-resource)

기관은 일반적으로 패키지 매니페스트에 정의된 패키지 ID 이름에 해당하는 PRI(Package Resource Index)에 정의된 최상위 수준 리소스 지도입니다. [앱 패키징](../packaging/index.md)을 참조하세요. 따라서 이는 URI 및 IRI(Internationalized resource identifier) 양식 둘 다에서 패키지 ID 이름에 사용할 수 있는 문자로 제한됩니다. 패키지 이름은 현재 실행 중인 앱 패키지 종속성 그래프의 패키지 중 하나의 이름이어야 합니다.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

기관에 다른 문자가 나타나면 검색 및 비교는 실패합니다. 기관에 대한 기본값은 현재 실행 중인 앱의 대/소문자 구분 패키지 이름입니다.

```xml
ms-resource:///
```

기관은 대/소문자를 구분하고 표준화된 양식은 대/소문자를 유지합니다. 그러나 리소스를 조회는 대소문자를 구분하지 않습니다.

### <a name="user-info-and-port-ms-resource"></a>사용자 정보 및 포트(ms-resource)

다른 인기 있는 스키마와 달리 `ms-resource` 스키마는 사용자 정보 또는 포트 구성 요소를 정의하지 않습니다. 따라서 "@" and ":"는 유효한 기관 값으로 허용되지 않으며 이것이 포함될 경우 조회가 실패합니다. 다음이 실패합니다.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>경로(ms-resource)

경로는 [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) 하위 트리의 계층 구조 위치([리소스 관리 시스템](https://msdn.microsoft.com/library/windows/apps/jj552947) 참조) 및 그 안의 [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live)를 식별합니다. 일반적으로 이는 리소스 파일(.resw)의 파일 이름(확장명 제외)과 그 안의 문자열 리소스의 식별자에 해당합니다.

자세한 내용과 예는 [UI 및 앱 패키지 매니페스트의 문자열 지역화](localize-strings-ui-manifest.md) 및 [언어, 배율, 고대비에 대한 타일 및 알림 메시지](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)를 참조하세요.

`ms-resource`의 경로 구성 요소는 일반 URI와 같이 대/소문자를 구분합니다. 그러나 기본 검색 수행 [CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628) *ignoreCase* 설정 `true`합니다.

URI의 표준화된 형태는 대소문자와 퍼센트 디코드(두자리 16진수가 따르는 "%" 기호) RFC 3986 예약되지 않은 문자를 유지합니다. 문자 "?", "#", "/", "*", '”'(큰따옴표)는 파일 또는 폴더 이름 같은 데이터를 나타내는 경로에 %로 인코딩되어야 합니다. 검색하기 전에 모든 퍼센트로 인코딩된 문자를 디코딩합니다. 따라서 라는 리소스 파일에서 문자열 리소스를 검색 하려면 `Hello#World.resw`을이 URI를 사용 합니다.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>쿼리(ms-resource)

리소스를 검색하는 동안 쿼리 매개 변수는 무시됩니다. 쿼리 매개 변수의 표준화된 형태는 대/소문자를 유지합니다. 비교하는 동안 쿼리 매개 변수는 무시되지 않습니다. 쿼리 명령줄 매개 변수는 대/소문자를 구분하여 비교됩니다.

이 URI 구문 분석 위의 특정 구성 요소 개발자는 적합한 곳에 쿼리 매개 변수를 사용하도록 선택할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [URI(Uniform Resource Identifier): 일반 구문](http://go.microsoft.com/fwlink/p/?LinkId=263444)
* [앱 패키징](../packaging/index.md)
* [XAML 태그와 코드에서 이미지 또는 다른 자산 참조](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)
* [UI와 앱 패키지 매니페스트에 문자열 지역화](localize-strings-ui-manifest.md)
* [리소스 관리 시스템](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [언어, 배율, 고대비에 대한 타일 및 알림 메시지](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)