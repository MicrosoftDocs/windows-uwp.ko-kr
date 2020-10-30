---
description: 앱의 패키지, 앱의 데이터 폴더 또는 클라우드에서 제공하는 파일을 참조하는 데 사용할 수 있는 몇 가지 URI(Uniform Resource Identifier) 체계가 있습니다. URI 체계를 사용하여 앱의 리소스 파일(.resw)에서 로드되는 문자열을 참조할 수도 있습니다.
title: URI 체계
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 8806992ebb7f4335ca0a748c1b2bce4a6de39fae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031549"
---
# <a name="uri-schemes"></a>URI 체계

앱의 패키지, 앱의 데이터 폴더 또는 클라우드에서 제공하는 파일을 참조하는 데 사용할 수 있는 몇 가지 URI(Uniform Resource Identifier) 체계가 있습니다. URI 체계를 사용하여 앱의 리소스 파일(.resw)에서 로드되는 문자열을 참조할 수도 있습니다. 코드, XAML 태그, 앱 패키지 매니페스트 또는 타일 및 알림 알림 템플릿에서 이러한 URI 스키마를 사용할 수 있습니다.

## <a name="common-features-of-the-uri-schemes"></a>URI 체계의 일반적인 기능

이 항목에서 설명 하는 모든 구성표는 정규화 및 리소스 검색에 대 한 일반적인 URI 체계 규칙을 따릅니다. URI의 제네릭 구문은 [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) 을 참조 하세요.

모든 URI 체계는 URI의 기관 및 경로 구성 요소로 [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) 당 계층 구조 파트를 정의 합니다.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

즉, 기본적으로 URI에는 세 가지 구성 요소가 있습니다. URI *체계* 의 두 슬래시 바로 다음에는 *인증 기관* 이라고 하는 구성 요소 (비어 있을 수 있음)가 있습니다. 그리고 바로 다음에 오는 *경로* 입니다. URI를 `http://www.contoso.com/welcome.png` 예로 들어, 체계는 ""이 고, 기관은 "" 이며, `http://` `www.contoso.com` 경로는 ""입니다 `/welcome.png` . 또 다른 예는 `ms-appx:///logo.png` 인증 기관 구성 요소가 비어 있고 기본값을 사용 하는 URI입니다.

이 항목에서 설명 하는 Uri의 스키마 별 처리에서는 조각 구성 요소가 무시 됩니다. 리소스를 검색 하 고 비교 하는 동안 조각 구성 요소에는 관계가 없습니다. 그러나 특정 구현 위에 있는 계층은 보조 리소스를 검색 하는 조각을 해석할 수 있습니다.

모든 IRI 구성 요소의 정규화 후 바이트에 대 한 비교가 발생 합니다.

## <a name="case-insensitivity-and-normalization"></a>대/소문자 구분 안 하며 정규화

이 항목에서 설명 하는 모든 URI 체계는 스키마에 대 한 정규화 및 리소스 검색을 위해 일반적인 URI 규칙 (RFC 3986)을 따릅니다. 이러한 Uri의 정규화 된 형식은 대/소문자를 유지 하 고 RFC 3986 예약 되지 않은 문자를 백분율 디코딩합니다.

이 항목에서 설명 하는 모든 URI 스키마 ( *스키마* , *기관* 및 *경로* )는 표준에 따라 대/소문자를 구분 하지 않으며, 다른 하나는 대/소문자를 구분 하지 않는 방식으로 시스템에서 처리 됩니다. **참고** 해당 규칙에 대 한 유일한 예외는 대/소문자를 구분 하는의 *인증 기관* 입니다 `ms-resource` .

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx 및 ms-appx-웹

`ms-appx`또는 `ms-appx-web` URI 체계를 사용 하 여 앱의 패키지에서 제공 되는 파일을 참조 합니다 ( [앱 패키징](../packaging/index.md)참조). 응용 프로그램 패키지의 파일은 일반적으로 정적 이미지, 데이터, 코드 및 레이아웃 파일입니다. `ms-appx-web`체계는 `ms-appx` 웹 구획에서와 동일한 파일에 액세스 합니다. 예제 및 자세한 정보는 [XAML 태그 및 코드에서 이미지 또는 기타 자산 참조](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)를 참조 하세요.

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>구성표 이름 (ms appx 및 ms-appx-웹)

URI 체계 이름은 문자열 "ms appx" 또는 "ms-appx-웹"입니다.

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Authority (ms appx 및 ms-appx-웹)

Authority는 패키지 매니페스트에 정의 된 패키지 id 이름입니다. 따라서 URI 및 IRI (국제화 된 리소스 식별자) 형식은 패키지 id 이름에 허용 되는 문자 집합으로 제한 됩니다. 패키지 이름은 현재 실행 중인 앱의 패키지 종속성 그래프에서 패키지 중 하나의 이름 이어야 합니다.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

다른 문자가 인증 기관에 표시 되 면 검색 및 비교가 실패 합니다. Authority의 기본값은 현재 실행 중인 앱의 패키지입니다.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>사용자 정보 및 포트 (ms appx 및 ms-appx-웹)

`ms-appx`다른 인기 있는 체계와 달리 구성표는 사용자 정보 또는 포트 구성 요소를 정의 하지 않습니다. " @" and " :"은 유효한 인증 기관 값으로 허용 되지 않으므로 포함 된 경우에는 조회가 실패 합니다. 다음의 각 작업이 실패 합니다.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Path (ms appx 및 ms-appx-웹)

경로 구성 요소는 일반 RFC 3986 구문과 일치 하며, Iri에서 ASCII가 아닌 문자를 지원 합니다. Path 구성 요소는 파일의 논리적 또는 물리적 파일 경로를 정의 합니다. 이 파일은 권한으로 지정 된 앱에 대해 설치 된 앱 패키지의 위치와 연결 된 폴더에 있습니다.

경로가 실제 경로와 파일 이름을 참조 하면 실제 파일 자산이 검색 됩니다. 그러나 이러한 물리적 파일이 없는 경우 검색 중에 반환 되는 실제 리소스는 런타임에 콘텐츠 협상을 사용 하 여 결정 됩니다. 이러한 결정은 앱, OS 및 언어, 표시 배율 인수, 테마, 고대비 및 기타 런타임 컨텍스트 등의 사용자 설정에 따라 결정 됩니다. 예를 들어 검색할 실제 리소스 값을 결정할 때 앱 언어, 시스템의 표시 설정 및 사용자의 고대비 설정의 조합을 고려해 볼 수 있습니다.

```xml
ms-appx:///images/logo.png
```

위의 URI는 실제로 다음과 같은 실제 파일 이름을 사용 하 여 현재 앱의 패키지 내에서 파일을 검색할 수 있습니다.

<blockquote>
<pre>
\Images\fr-FR\logo.scale-100_contrast-white.png
</blockquote>
</pre>

물론 전체 이름으로 직접 참조 하 여 동일한 물리적 파일을 검색할 수도 있습니다.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

의 경로 구성 요소는 `ms-appx(-web)` 제네릭 uri와 같이 대/소문자를 구분 합니다. 그러나 리소스에 액세스 하는 기본 파일 시스템에서 대/소문자를 구분 하지 않는 경우 (예: NTFS의 경우) 리소스 검색은 대/소문자를 소문자 합니다.

URI의 정규화 된 형태는 대/소문자를 유지 하 고 백분율 디코딩 ("%" 기호 뒤에 두 자리 16 진수 표시) RFC 3986 예약 되지 않은 문자를 유지 합니다. 문자 "?", "#", "/", "*" 및 ' "' (큰따옴표 문자)는 파일 또는 폴더 이름과 같은 데이터를 나타내기 위해 경로에서 백분율 인코딩 되어야 합니다. 모든% 인코딩된 문자는 검색 전에 디코딩됩니다. 따라서 Hello # World.html 이라는 파일을 검색 하려면이 URI를 사용 합니다.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>쿼리 (ms appx 및 ms-appx-웹)

쿼리 매개 변수는 리소스를 검색 하는 동안 무시 됩니다. 쿼리 매개 변수의 정규화 된 형태는 대/소문자를 유지 합니다. 비교 하는 동안에는 쿼리 매개 변수가 무시 되지 않습니다.

## <a name="ms-appdata"></a>ms-appdata

`ms-appdata`응용 프로그램의 로컬, 로밍 및 임시 데이터 폴더에서 제공 되는 파일을 참조 하려면 URI 체계를 사용 합니다. 이러한 앱 데이터 폴더에 대 한 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)을 참조 하세요.

`ms-appdata`URI 체계는 [ms appx 및 ms-appx 웹](#ms-appx-and-ms-appx-web) 에서 수행 하는 런타임 콘텐츠 협상을 수행 하지 않습니다. 하지만 [QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 의 콘텐츠에 응답 하 고 URI에서 전체 물리적 파일 이름을 사용 하 여 앱 데이터에서 적절 한 자산을 로드할 수 있습니다.

### <a name="scheme-name-ms-appdata"></a>체계 이름 (ms-appdata)

URI 체계 이름은 문자열 "appdata"입니다.

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>기관 (ms-appdata)

Authority는 패키지 매니페스트에 정의 된 패키지 id 이름입니다. 따라서 URI 및 IRI (국제화 된 리소스 식별자) 형식은 패키지 id 이름에 허용 되는 문자 집합으로 제한 됩니다. 패키지 이름은 현재 실행 중인 앱 패키지의 이름 이어야 합니다.

```xml
ms-appdata://Contoso.MyApp/
```

다른 문자가 인증 기관에 표시 되 면 검색 및 비교가 실패 합니다. Authority의 기본값은 현재 실행 중인 앱의 패키지입니다.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>사용자 정보 및 포트 (appdata)

`ms-appdata`다른 인기 있는 체계와 달리 구성표는 사용자 정보 또는 포트 구성 요소를 정의 하지 않습니다. " @" and " :"은 유효한 인증 기관 값으로 허용 되지 않으므로 포함 된 경우에는 조회가 실패 합니다. 다음의 각 작업이 실패 합니다.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Path (ms-appdata)

경로 구성 요소는 일반 RFC 3986 구문과 일치 하며, Iri에서 ASCII가 아닌 문자를 지원 합니다. 로컬, 로밍 및 임시 상태 저장소에 대 한 세 개의 예약 된 폴더가 [있습니다.](/uwp/api/Windows.Storage.ApplicationData?branch=live) `ms-appdata`구성표를 사용 하면 해당 위치의 파일 및 폴더에 액세스할 수 있습니다. 경로 구성 요소의 첫 번째 세그먼트는 다음과 같은 방식으로 특정 폴더를 지정 해야 합니다. 따라서 "hier-part"의 "경로 비어 있음" 형식은 유효 하지 않습니다.

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

의 경로 구성 요소는 `ms-appdata` 제네릭 uri와 같이 대/소문자를 구분 합니다. 그러나 리소스에 액세스 하는 기본 파일 시스템에서 대/소문자를 구분 하지 않는 경우 (예: NTFS의 경우) 리소스 검색은 대/소문자를 소문자 합니다.

URI의 정규화 된 형태는 대/소문자를 유지 하 고 백분율 디코딩 ("%" 기호 뒤에 두 자리 16 진수 표시) RFC 3986 예약 되지 않은 문자를 유지 합니다. 문자 "?", "#", "/", "*" 및 ' "' (큰따옴표 문자)는 파일 또는 폴더 이름과 같은 데이터를 나타내기 위해 경로에서 백분율 인코딩 되어야 합니다. 모든% 인코딩된 문자는 검색 전에 디코딩됩니다. 따라서 Hello # World.html 이라는 로컬 파일을 검색 하려면이 URI를 사용 합니다.

```xml
ms-appdata://local/Hello%23World.html
```

리소스 검색 및 최상위 경로 세그먼트의 id는 점 정규화 이후 처리 됩니다 (".. /./b/c "). 따라서 Uri는 예약 된 폴더 중 하나를 사용할 수 없습니다. 따라서 다음 URI는 허용 되지 않습니다.

```xml
ms-appdata:///local/../hello/logo.png
```

그러나이 URI는 중복 되는 경우에만 허용 됩니다.

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>쿼리 (ms appdata)

쿼리 매개 변수는 리소스를 검색 하는 동안 무시 됩니다. 쿼리 매개 변수의 정규화 된 형태는 대/소문자를 유지 합니다. 비교 하는 동안에는 쿼리 매개 변수가 무시 되지 않습니다.

## <a name="ms-resource"></a>ms-리소스

`ms-resource`URI 체계를 사용 하 여 앱의 리소스 파일 (. resw)에서 로드 된 문자열을 참조 합니다. 리소스 파일에 대 한 예제 및 자세한 정보는 [UI 및 앱 패키지 매니페스트에서 문자열 지역화](localize-strings-ui-manifest.md)를 참조 하세요.

### <a name="scheme-name-ms-resource"></a>스키마 이름 (ms resource)

URI 체계 이름은 문자열 "ms resource"입니다.

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>기관 (ms resource)

이 권한은 일반적으로 패키지 매니페스트에 정의 된 패키지 id 이름에 해당 하는 PRI (패키지 리소스 인덱스)에 정의 된 최상위 리소스 맵입니다. [앱 패키징](../packaging/index.md))을 참조 하세요. 따라서 URI 및 IRI (국제화 된 리소스 식별자) 형식은 패키지 id 이름에 허용 되는 문자 집합으로 제한 됩니다. 패키지 이름은 현재 실행 중인 앱의 패키지 종속성 그래프에서 패키지 중 하나의 이름 이어야 합니다.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

다른 문자가 인증 기관에 표시 되 면 검색 및 비교가 실패 합니다. Authority의 기본값은 현재 실행 중인 앱의 대/소문자를 구분 하는 패키지 이름입니다.

```xml
ms-resource:///
```

기관은 대/소문자를 구분 하며 정규화 된 형식에서 대/소문자를 유지 합니다. 그러나 리소스를 조회 하는 경우에는 대/소문자가 소문자 됩니다.

### <a name="user-info-and-port-ms-resource"></a>사용자 정보 및 포트 (ms resource)

`ms-resource`다른 인기 있는 체계와 달리 구성표는 사용자 정보 또는 포트 구성 요소를 정의 하지 않습니다. " @" and " :"은 유효한 인증 기관 값으로 허용 되지 않으므로 포함 된 경우에는 조회가 실패 합니다. 다음의 각 작업이 실패 합니다.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Path (ms resource)

경로는 [Resourcemap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) 하위 트리 ( [리소스 관리 시스템](/previous-versions/windows/apps/jj552947(v=win.10))참조)와 그 안에 있는 [namedresource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) 의 계층 위치를 식별 합니다. 일반적으로이는 리소스 파일 (. resw)의 파일 이름 (확장명 제외)과 그 안에 포함 된 문자열 리소스의 식별자에 해당 합니다.

예제 및 자세한 내용은 [UI에서 문자열 지역화 및 앱 패키지 매니페스트](localize-strings-ui-manifest.md) 및 [언어, 규모 및 고대비에 대 한 타일 및 알림 알림 지원](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)을 참조 하세요.

의 경로 구성 요소는 `ms-resource` 제네릭 uri와 같이 대/소문자를 구분 합니다. 그러나 기본 검색은 *ignoreCase* 가로 설정 된 [comparestringordinal](/windows/desktop/api/winstring/nf-winstring-windowscomparestringordinal) 을 수행 합니다 `true` .

URI의 정규화 된 형태는 대/소문자를 유지 하 고 백분율 디코딩 ("%" 기호 뒤에 두 자리 16 진수 표시) RFC 3986 예약 되지 않은 문자를 유지 합니다. 문자 "?", "#", "/", "*" 및 ' "' (큰따옴표 문자)는 파일 또는 폴더 이름과 같은 데이터를 나타내기 위해 경로에서 백분율 인코딩 되어야 합니다. 모든% 인코딩된 문자는 검색 전에 디코딩됩니다. 따라서 이라는 리소스 파일에서 문자열 리소스를 검색 하려면 `Hello#World.resw` 이 URI를 사용 합니다.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>쿼리 (ms 리소스)

쿼리 매개 변수는 리소스를 검색 하는 동안 무시 됩니다. 쿼리 매개 변수의 정규화 된 형태는 대/소문자를 유지 합니다. 비교 하는 동안에는 쿼리 매개 변수가 무시 되지 않습니다. 쿼리 매개 변수는 대/소문자를 비교 하 여 소문자 합니다.

이 URI 구문 분석 위에 계층화 된 특정 구성 요소의 개발자는 쿼리 매개 변수를 적절 하 게 사용 하도록 선택할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [URI (Uniform Resource Identifier): 제네릭 구문](https://www.ietf.org/rfc/rfc3986.txt)
* [앱 패키징](../packaging/index.md)
* [XAML 태그 및 코드에서 이미지 또는 기타 자산 참조](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [설정과 기타 앱 데이터의 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)
* [UI 및 앱 패키지 매니페스트의 문자열 지역화](localize-strings-ui-manifest.md)
* [리소스 관리 시스템](/previous-versions/windows/apps/jj552947(v=win.10))
* [언어, 규모 및 고대비에 대 한 타일 및 알림 알림 지원](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
