---
description: C++/WinRT 사용 속도를 높이려면 이 항목에서 안내하는 단순한 코드 예제를 살펴봅니다.
title: C++/WinRT 시작
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 가져오기, 시작하기, 시작
ms.localizationpriority: medium
ms.openlocfilehash: 412f34d21ddb24f637450fdfc71214c360445841
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170197"
---
# <a name="get-started-with-cwinrt"></a>C++/WinRT 시작

[C++/WinRT](./intro-to-using-cpp-with-winrt.md)를 빠르게 사용할 수 있도록, 이 항목에서는 새 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트를 기반으로 하는 간단한 코드 예제를 살펴보겠습니다. 이 항목에서는 [Windows 데스크톱 애플리케이션 프로젝트에 C++/WinRT 지원을 추가](#modify-a-windows-desktop-application-project-to-add-cwinrt-support)하는 방법도 보여 줍니다.

> [!NOTE]
> Visual Studio 및 Windows SDK의 최신 버전을 사용하여 개발하는 것이 좋지만, Visual Studio 2017(버전 15.8.0 이상)을 사용 중이고 Windows SDK 버전 10.0.17134.0(Windows 10 버전, 1803)을 대상으로 하는 경우 새로 만든 C++/WinRT 프로젝트가 컴파일되지 않고 “오류 C3861: ‘from_abi’: 식별자를 찾을 수 없음” 오류와 *base.h*에서 발생하는 기타 오류가 표시될 수 있습니다. 해결 방법으로, 보다 규칙에 맞는 Windows SDK 최신 버전을 대상으로 지정하거나, 프로젝트 속성 **C/C++**  > **언어** > **적합성 모드: 아니요**를 설정합니다. 또는 **추가 옵션** 아래의 프로젝트 속성 **C/C++**  > **언어** > **명령줄**에 **/permissive-** 가 표시되는 경우 삭제합니다.

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 빠른 시작

> [!NOTE]
> &mdash;프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT Visual Studio 확장(VSIX) 및 NuGet 패키지를 설치하고 사용하는 방법을 포함&mdash;하는 C++/WinRT용 Visual Studio 개발 설정에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

새 **Windows 콘솔 애플리케이션(C++/WinRT)** 프로젝트를 만듭니다.

`pch.h` 및 `main.cpp`를 다음과 같이 편집합니다.

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
```

```cppwinrt
// main.cpp
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

위의 간단한 코드 예제를 하나씩 살펴보고 각 파트에서 수행되는 작업을 설명하겠습니다.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

기본 프로젝트 설정을 사용하면 Windows SDK의 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 폴더에 있는 헤더가 포함됩니다. Visual Studio는 *IncludePath* 매크로에 해당 경로를 포함합니다. 그러나 `cppwinrt.exe` 도구를 통해 프로젝트의 *$(GeneratedFilesDir)* 폴더에 동일한 헤더가 생성되기 때문에 Windows SDK에 대한 엄격한 종속성은 없습니다. 다른 곳에서 찾을 수 없거나 프로젝트 설정을 변경하는 경우 해당 폴더에서 헤더가 로드됩니다.

헤더에는 C++/WinRT에 프로젝션된 Windows API가 포함됩니다. 즉, C++/WinRT는 각 Windows 형식에 해당하는 C++에 편리한 형식(‘프로젝션된 형식’이라고 함)을 정의합니다. 프로젝션된 형식은 Windows 형식과 동일한 정규화된 이름을 갖지만 C++ **winrt** 네임스페이스에 배치됩니다. 이러한 형식을 미리 컴파일된 헤더에 포함하면 증분 빌드 시간을 줄일 수 있습니다.

> [!IMPORTANT]
> Windows 네임스페이스의 형식을 사용하려는 경우에는 항상 해당하는 C++/WinRT Windows 네임스페이스 헤더 파일을 위와 같이 `#include`해야 합니다. ‘해당’ 헤더는 형식의 네임스페이스와 동일한 이름을 가진 헤더입니다. 예를 들어 [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) 런타임 클래스에 C++/WinRT 프로젝션을 사용하려면 `winrt/Windows.Foundation.Collections.h` 헤더를 포함합니다.
> 
> 일반적으로 C++/WinRT 프로젝션 헤더가 부모 네임스페이스 헤더 파일을 자동으로 포함합니다. 예를 들어 `winrt/Windows.Foundation.Collections.h`는 `winrt/Windows.Foundation.h`를 포함합니다. 하지만 이 구현 세부 정보는 시간이 지나면 변경되므로 이 동작을 사용하면 안 됩니다. 필요한 헤더를 명시적으로 포함해야 합니다.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` 지시문은 선택 사항이지만 편리합니다. 이러한 지시문에 위와 같은 패턴을 사용하면 **winrt** 네임스페이스에 있는 모든 항목에 대해 한정되지 않은 이름 조회가 가능하므로, 새 프로젝트를 시작 중이며 C++/WinRT가 해당 프로젝트 내에서 사용하는 유일한 언어 프로젝션인 경우에 적합합니다. 반면에 C++/WinRT 코드와 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 및/또는 SDK ABI(애플리케이션 이진 인터페이스) 코드를 함께 사용하는 경우(두 모델 중 하나 또는 둘 다에서 포팅하거나 상호 운용), [C++/WinRT와 C++/CX 간의 상호 운용성](interop-winrt-cx.md), [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md), [C++/WinRT와 ABI 간의 상호 운용성](interop-winrt-abi.md) 항목을 참조하세요.

```cppwinrt
winrt::init_apartment();
```

**winrt::init_apartment** 호출은 기본적으로 다중 스레드 아파트에서 Windows 런타임의 스레드를 초기화합니다. 이 호출에서 COM도 초기화합니다.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

두 개의 개체를 스택 할당합니다. 두 개체는 Windows 블로그의 URI와 배포 클라이언트를 나타냅니다. 간단한 와이드 문자열 리터럴로 URI를 생성합니다(문자열로 작업할 수 있는 더 많은 방법은 [C++/WinRT의 문자열 처리](strings.md) 참조).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)는 비동기 Windows 런타임 함수의 예입니다. 이 코드 예제는 **RetrieveFeedAsync**에서 비동기 작업 개체를 받은 후 해당 개체에서 **get**을 호출하여 호출 스레드를 차단하고 결과(이 경우 배포 피드)를 기다립니다. 동시성에 대한 자세한 내용과 비차단 기술에 대해서는 [C++/WinRT를 통한 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)는 **begin** 및 **end** 함수에서 반환된 반복기(또는 해당 상수, 역방향, 상수-역방향 변형)로 정의된 범위입니다. 따라서 범위 기반 `for` 문 또는 **std::for_each** 템플릿 함수를 사용하여 **Items**를 열거할 수 있습니다. 이처럼 Windows 런타임 컬렉션을 반복할 때마다 `#include <winrt/Windows.Foundation.Collections.h>`가 필요합니다.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

피드의 제목 텍스트를 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 개체로 가져옵니다(자세한 내용은 [C++/WinRT의 문자열 처리](strings.md) 참조). 그러면 C++ 표준 라이브러리 문자열과 함께 사용된 패턴을 반영하는 **hstring**이 **c_str** 함수를 통해 출력됩니다.

이와 같이 C++/WinRT는 클래스와 유사한 최신 C++ 식(예: `syndicationItem.Title().Text()`)의 사용을 장려합니다. 이 기술은 기존 COM 프로그래밍과 확연히 다를 뿐만 아니라 보다 명확한 프로그래밍 스타일입니다. COM을 직접 초기화하거나 COM 포인터를 사용할 필요가 없습니다.

HRESULT 반환 코드를 처리할 필요도 없습니다. C++/WinRT는 자연스러운 최신 프로그래밍 스타일을 위해 [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 등의 오류 HRESULT를 예외로 변환합니다. 오류 처리와 코드 예제에 대한 자세한 내용은 [C++/WinRT를 통한 오류 처리](error-handling.md)를 참조하세요.

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Windows 데스크톱 애플리케이션 프로젝트를 수정하여 C++/WinRT 지원 추가

이 섹션에서는 기존의 Windows 데스크톱 애플리케이션 프로젝트에 C++/WinRT 지원을 추가하는 방법을 보여 줍니다. 기존의 Windows 데스크톱 애플리케이션 프로젝트가 없는 경우 먼저 프로젝트를 만든 후 이러한 단계를 수행할 수 있습니다. 예를 들어 Visual Studio를 열고, **Visual C++** \> **Windows 데스크톱** \> **Windows 데스크톱 애플리케이션** 프로젝트를 만듭니다.

선택적으로 [C++/WinRT VSIX(Visual Studio Extension)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 및 NuGet 패키지를 설치할 수 있습니다. 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

### <a name="set-project-properties"></a>프로젝트 속성 설정

**일반** \> **Windows SDK 버전** 프로젝트 속성으로 차례로 이동한 다음, **모든 구성** 및 **모든 플랫폼**을 선택합니다. **Windows SDK 버전**이 10.0.17134.0(Windows 10, 버전 1803) 이상으로 설정되어 있는지 확인합니다.

[새 프로젝트가 컴파일되지 않는 이유는 무엇인가요?](./faq.md)의 영향을 받지 않는지 확인합니다.

C++/WinRT는 C++17 표준의 기능을 사용하기 때문에 프로젝트 속성 **C/C++**  > **언어** > **C++ 언어 표준**을 ‘ISO C++17 표준(/std:c++17)’으로 설정합니다.

### <a name="the-precompiled-header"></a>미리 컴파일된 헤더

기본 프로젝트 템플릿은 `framework.h` 또는 `stdafx.h`라는 미리 컴파일된 헤더를 자동으로 만듭니다. 헤더 이름을 `pch.h`로 바꿉니다. `stdafx.cpp` 파일이 있는 경우 파일 이름도 `pch.cpp`로 바꿉니다. 프로젝트 속성 **C/C++**  > **미리 컴파일된 헤더** > **미리 컴파일된 헤더**를 *만들기(/Yc)* 로 설정하고 **미리 컴파일된 헤더 파일**을 *pch.h*로 설정합니다.

모든 `#include "framework.h"`(또는 `#include "stdafx.h"`)를 찾아서 `#include "pch.h"`로 바꿉니다.

`pch.h`에 `winrt/base.h`를 포함합니다.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>연결

C++/WinRT 언어 프로젝션은 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 상위 라이브러리에 대한 연결이 필요한 특정 Windows 런타임 프리(비멤버) 함수 및 진입점에 따라 달라집니다. 이 섹션에서는 링커를 충족하는 세 가지 방법을 설명합니다.

첫 번째 옵션은 Visual Studio 프로젝트에 C++/WinRT MSBuild 속성과 대상을 모두 추가하는 것입니다. 이 작업을 수행하려면 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)를 프로젝트에 설치합니다. Visual Studio에서 프로젝트를 열고, **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 차례로 클릭하고, 검색 상자에서 **Microsoft.Windows.CppWinRT**를 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치**를 클릭하여 해당 프로젝트에 대한 패키지를 설치합니다.

프로젝트 연결 설정을 사용하여 `WindowsApp.lib`를 명시적으로 연결할 수도 있습니다. 또는 다음과 같이 소스 코드(예: `pch.h`)에서 이 작업을 수행할 수 있습니다.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

이제 컴파일하고 연결한 다음, 프로젝트에 C++/WinRT 코드(예: 위의 [C++/WinRT 빠른 시작](#a-cwinrt-quick-start) 섹션의 코드와 유사한 코드)를 추가할 수 있습니다.

## <a name="the-three-main-scenarios-for-cwinrt"></a>C++/WinRT에 대한 세 가지 주요 시나리오

C++/WinRT를 사용하여 익숙해지고 이 설명서의 나머지 부분을 살펴보면 다음 섹션에서 설명하는 세 가지 주요 시나리오가 있음을 알 수 있습니다.

### <a name="consuming-windows-runtime-apis-and-types"></a>Windows 런타임 API 및 형식 사용

즉 API를 *사용*하거나 *호출*합니다. 예를 들어 Bluetooth를 통한 통신, 비디오 스트리밍 및 표시, Windows 셸과의 통합 등을 수행하는 API를 호출합니다. C++/WinRT는 이 시나리오 범주를 완벽하고 단호하게 지원합니다. 자세한 내용은 [C++/WinRT를 통한 API 사용](./consume-apis.md)을 참조하세요.

### <a name="authoring-windows-runtime-apis-and-types"></a>Windows 런타임 API 및 형식 작성

즉 API 및 형식을 *생성*합니다. 예를 들어 위의 섹션에서 설명하는 종류의 API, 그래픽 API, 스토리지 및 파일 시스템 API, 네트워킹 API 등을 생성합니다. 자세한 내용은 [C++/WinRT를 통한 API 작성](./author-apis.md)을 참조하세요.

API를 구현하려면 먼저 IDL을 사용하여 해당 API의 모양을 정의해야 하므로 C++/WinRT를 사용하여 API를 작성하는 것이 이러한 API를 사용하는 것보다 더 중요합니다. 이러한 작업은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](./binding-property.md)에서 연습할 수 있습니다.

### <a name="xaml-applications"></a>XAML 애플리케이션

이 시나리오는 애플리케이션 및 컨트롤을 XAML UI 프레임워크에 구축하는 것입니다. XAML 애플리케이션에서 작업하는 것은 사용과 작성의 조합에 해당합니다. 그러나 XAML은 현재 Windows에서 가장 많이 사용되는 UI 프레임워크이며 Windows 런타임에 대한 영향력은 이에 비례하므로 고유한 시나리오 범주가 있습니다.

XAML은 리플렉션을 제공하는 프로그래밍 언어에서 가장 효율적으로 작동합니다. C++/WinRT에서는 XAML 프레임워크와 상호 운용하기 위해 약간의 추가 작업을 수행해야 하는 경우도 있습니다. 이러한 모든 사례는 설명서에서 다룹니다. 적절한 시작 지점은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](./binding-property.md) 및 [C++/WinRT를 통한 XAML 사용자 지정(템플릿 기반) 컨트롤](./xaml-cust-ctrl.md)입니다.

## <a name="important-apis"></a>중요 API
* [SyndicationClient::RetrieveFeedAsync 메서드](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items 속성](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error 구조체](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT를 통한 오류 처리](error-handling.md)
* [C++/WinRT와 C++/CX 간의 상호 운용성](interop-winrt-cx.md)
* [C++/WinRT와 ABI 사이의 상호 운용성](interop-winrt-abi.md)
* [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md)
* [C++/WinRT의 문자열 처리](strings.md)