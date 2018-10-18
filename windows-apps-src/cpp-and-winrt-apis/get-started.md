---
author: stevewhims
description: C++/WinRT 사용 속도를 높이기 위해 이 항목은 단순한 코드 예제를 안내합니다.
title: C++/WinRT 시작
ms.author: stwhi
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 가져오기, 얻기, 시작
ms.localizationpriority: medium
ms.openlocfilehash: b5954aa8236a9abeee6e5c74a200f77fcccf97e3
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "4966814"
---
# <a name="get-started-with-cwinrt"></a>C++/WinRT 시작
하는 데 속도 사용 하 여 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt),이 항목은 단순한 코드 예제를 안내 합니다.

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 빠른 시작
> [!NOTE]
> C++/WinRT Visual Studio Extension(VSIX)(프로젝트 템플릿 지원과 C++/WinRT MSBuild 속성 및 대상 제공)의 설치 및 사용에 대한 자세한 내용은 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)를 참조하세요.

새 **Windows 콘솔 응용 프로그램(C++/WinRT)** 프로젝트를 만듭니다.

> [!IMPORTANT]
> Visual Studio 2017을 사용 하는 경우 (15.8.0 버전 이상)를 대상으로 하는 Windows SDK 버전 10.0.17134.0(windows (Windows 10, 버전 1803) 한 다음 새로 만든 C + + WinRT 프로젝트 컴파일 오류가 있는 하지 못할 수도 있습니다 "C3861*오류: 'from_abi': 식별자 하지 발견*", 및 기타 오류 *base.h*에서 발생 합니다. 해결 방법은 대상 중 하나는 이후 (자세한 준수) 버전의 Windows SDK 또는 집합 프로젝트 속성 **C/c + +** > **언어** > **적합성 모드: 아니요** (또한 경우 **허용 /-** 프로젝트 속성 **에에서 표시 됩니다 C/C++** > **언어** >  **추가 옵션****명령줄** 삭제).

다음과 같이 `pch.h` 및 `main.cpp`를 편집합니다.

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
...
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

위의 예제에서 짧은 코드 샘플을 하나씩 살펴보고 각 부분에서 해당 내용을 설명하겠습니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
```

포함된 헤더는 SDK의 일부로 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 폴더 안에 있습니다. Visual Studio는 *IncludePath* 매크로에 해당 경로를 포함시킵니다. 헤더에는 C++/WinRT에 프로젝션된 Windows API가 포함됩니다. 즉, 각 Windows 형식에 대해 C++/WinRT는 C++ 친화적인 해당 형식(*프로젝션된 형식*이라고 함을 정의합니다). 프로젝션된 형식은 Windows 형식과 동일한 정규화된 이름을 가지지만 C++ **winrt** 네임스페이스에 배치됩니다. 이러한 형식을 미리 컴파일된 헤더에 포함하면 증분 빌드 시간을 줄일 수 있습니다.

> [!IMPORTANT]
> Windows 네임스페이스의 형식을 사용할 때마다 표시된 것과 같이 해당 네임스페이스와 일치하는 C++/WinRT Windows 네임스페이스 헤더 파일을 추가하세요. *해당* 헤더는 형식의 네임스페이스와 같은 이름을 가집니다. 예를 들어 [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) 런타임 클래스에 대해 C++/WinRT 프로젝션을 사용하려면 `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` 지시문은 선택 사항이지만 편리합니다. 이러한 지시문(**winrt** 네임스페이스의 모든 것에 대해 정규화되지 않은 이름 조회를 허용)에 대해 위해 표시된 패턴은 새로운 프로젝트를 시작하는 경우에 적합하며 C++/WinRT는 프로젝트 내에서 사용 중인 유일한 언어 프로젝션입니다. 반면에 C++/WinRT 코드를 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 및/또는 SDK 응용 프로그램 이진 인터페이스(ABI) 코드(이 모델 중 하나 또는 둘 다에서 포팅하거나 상호 운용)와 혼합하는 경우 [C++/WinRT 와 C++/CX 사이의 상호 운용성](interop-winrt-cx.md), [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md), [C++/WinRT와 ABI 사이의 상호 운용성](interop-winrt-abi.md) 항목을 참조하세요.

```cppwinrt
winrt::init_apartment();
```

**winrt::init_apartment**에 대한 호출은 기본적으로 다중 스레드 아파트에서 COM을 초기화합니다.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

두 개의 개체를 스택 할당합니다. 이 두 개체는 Windows 블로그의 URI와 배포 클라이언트를 나타냅니다. 간단한 전각 문자열 리터럴로 URI 구성합니다(문자열로 작업할 수 있는 다양한 방법은 [C++/WinRT의 문자열 처리](strings.md) 참조).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)는 비동기식 Windows 런타임 함수의 예입니다. 위의 코드 예제는 **RetrieveFeedAsync**에서 비동기 연산 개체를 수신한 후 해당 개체에 대해 **get**을 호출하여 호출 스레드를 차단하고 결과를 기다립니다(이 경우 배포 피드). 동시성에 대한 자세한 내용과 비차단 기법에 대해서는 [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)는 **begin** 및 **end** 함수에서 반환된 반복기(또는 상수, 역방향 및 상수-역방향 변형)로 정의되는 범위입니다. 이에 따라 범위 기반 `for` 문 또는 **std::for_each** 템플릿 함수와 함께 **Items**를 열거할 수 있습니다.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

[**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 개체로서 피드의 제목 텍스트를 가져옵니다(자세한 내용은 [C++/WinRT의 문자열 처리](strings.md) 참조). 그러면 **hstring**이 C++ 표준 라이브러리 문자열과 함께 사용되는 패턴을 반영하는 **c_str** 함수를 통해 출력됩니다.

이와 같이 C++/WinRT는 `syndicationItem.Title().Text()` 같이 클래스와 유사한 최신 C++ 표현식의 사용을 장려합니다. 이는 기존 COM 프로그래밍과 확연히 다를 뿐만 아니라 더욱 완전한 프로그래밍 스타일입니다. 직접 COM을 초기화할 필요가 없으며, COM 포인터로 작업합니다.

HRESULT 반환 코드를 처리할 필요도 없습니다. C++/WinRT가 [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 등의 HRESULT 오류를 예외로 전환하기 때문에 자연스러운 최신 프로그래밍 스타일을 유지합니다. 오류 처리와 코드 샘플에 대한 자세한 내용은 [C++/WinRT를 통한 오류 처리](error-handling.md)를 참조하세요.

## <a name="important-apis"></a>중요 API
* [Syndicationclient:: Retrievefeedasync 메서드](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items 속성](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [hresult-error 구조체](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT를 통한 오류 처리](error-handling.md)
* [C++/WinRT와 C++/CX 사이의 상호 운용성](interop-winrt-cx.md)
* [C++/WinRT와 ABI 사이의 상호 운용성](interop-winrt-abi.md)
* [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md)
* [C++/WinRT의 문자열 처리](strings.md)
