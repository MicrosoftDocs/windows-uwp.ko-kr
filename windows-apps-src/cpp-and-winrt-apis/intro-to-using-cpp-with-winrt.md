---
author: stevewhims
description: C++/WinRT 소개&mdash;Windows 런타임 API용 표준 C++ 언어 프로젝션
title: C++/WinRT 소개
ms.author: stwhi
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션
ms.localizationpriority: medium
ms.openlocfilehash: 968afd6fdad1e7bf6b3c38d929ab79eefa71819a
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832327"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 소개
> [!NOTE]
> **일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.**

Windows SDK는 버전 10.0.17134.0(Windows 10, 버전 1803)에서 도입되기 시작하여 현재는 C++/WinRT까지 포함되어 있습니다.

> [!IMPORTANT]
> C++/WinRT에서 가장 중요하여 반드시 알고 있어야 할 정보는 섹션 [C++/WinRT에 대한 SDK 지원](#sdk-support-for-cwinrt)과 섹션 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](#visual-studio-support-for-cwinrt-and-the-vsix)에 설명되어 있습니다.

C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일에서만 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT에서는 모든 표준과 호환되는 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다.

## <a name="language-projections"></a>언어 프로젝션
Windows 런타임은 구성 요소 개체 모델(COM)을 기반으로 하며, *언어 프로젝션*을 통해 액세스하도록 설계되었습니다. 프로젝션은 COM 세부 정보를 숨기며, 지정된 언어에 더욱 자연스러운 프로그래밍 환경을 제공합니다.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 참조 콘텐츠의 C++/WinRT 언어 프로젝션
[Windows UWP API](https://docs.microsoft.com/uwp/api/)를 검색할 때는 오른쪽 상단에 있는 **언어** 콤보 상자를 클릭한 다음 **C++/WinRT**를 선택하여 C++/WinRT 언어 프로젝션에 표시되는 API 구문 블록을 확인합니다.

## <a name="sdk-support-for-cwinrt"></a>C++/WinRT에 대한 SDK 지원
Windows SDK는 버전 10.0.17134.0(Windows 10, 버전 1803)부터 C++/WinRT 프로젝션의 Windows 네임스페이스 헤더와 도구가 포함됩니다. 한 가지 중요한 도구는 `cppwinrt.exe`입니다. 이 도구는 `.winmd`에서 메타데이터를 C++/WinRT에 표시하는 소스 코드 파일을 생성합니다. Windows 런타임 메타데이터(`.winmd`) 파일은 Windows 런타임 API 표면을 정식으로 설명하는 방법을 제공합니다. `cppwinrt.exe`는 Windows API이든, 혹은 타사 Windows 런타임 구성 요소 API이든 상관없이 Windows 런타임 메타데이터에서 API 표면을 완전히 설명하거나 *표시하는* 표준 C++ 라이브러리를 생성합니다. `Cppwinrt.exe` 또한 자사/타사 API를 사용할 때는 물론이고 구성 요소에서 사용자 고유의 API를 작성하기 위한 개발 워크플로에서도 중요한 역할을 합니다.

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>C++/WinRT에 대한 Visual Studio 지원 및 VSIX
Visual Studio의 C++/WinRT 프로젝트 템플릿과 C++/WinRT MSBuild 속성 및 대상의 경우에는 [C++/WinRT Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix)을 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)에서 다운로드하여 설치하세요.

Visual Studio 2017 버전 15.6 이상과 Windows SDK 버전 10.0.17134.0(Windows 10, 버전 1803)이 필요합니다. 설치가 끝나면 Visual Studio에서 새 프로젝트를 만들거나, 혹은 프로젝트 > PropertyGroup으로 이동해 `<CppWinRTProject>true</CppWinRTProject>` 속성을 `.vcxproj` 파일에 추가하여 기존 프로젝트를 변환할 수도 있습니다. 속성 추가를 마쳤으면 이제 `cppwinrt.exe` 도구 호출을 포함해 프로젝트에 대한 C++/WinRT MSBuild 지원을 가져옵니다.

C++/WinRT는 the C++17 표준의 기능을 사용하기 때문에 프로젝트 속성 **C/C++** > **언어** > **ISO C++17 표준(/std:c++17)** 이 필요합니다. 또한 **적합성 모드: 예(/permissive-)** 를 설정하여 코드가 표준을 더욱 따르도록 할 수도 있습니다.

알고 있어야 할 또 다른 프로젝트 속성은 **C/C++** > **일반** > **경고를 오류로 처리**입니다. 이 속성은 필요에 따라 **예(/WX)** 또는 **아니오(/WX-)** 로 설정하세요. 간혹 `cppwinrt.exe` 도구에서 생성된 소스 파일이 구현체가 추가될 때까지 경고를 생성하는 경우가 있습니다.

이는 VSIX에서 제공되는 Visual Studio 프로젝트 템플릿입니다.

### <a name="windows-console-application-cwinrt"></a>Windows 콘솔 응용 프로그램(C++/WinRT)
콘솔 사용자 인터페이스가 포함된 Windows 데스크톱의 C++/WinRT 클라이언트 응용 프로그램용 프로젝트 템플릿입니다.

### <a name="blank-app-cwinrt"></a>비어 있는 앱(C++/WinRT)
XAML 사용자 인터페이스가 있는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

Visual Studio는 각 XAML 태그 파일 다음에 있는 IDL(`.idl`) 파일에서 구현체와 헤더 스텁을 생성할 목적으로 XAML 컴파일러를 지원합니다. 먼저 IDL 파일에서 앱의 XAML 페이지에서 참조할 로컬 런타임 클래스를 모두 정의한 후 프로젝트를 1회 빌드하여 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 로컬 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

### <a name="core-app-cwinrt"></a>주요 앱(C++/WinRT)
XAML을 사용하지 않는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

대신 C++/WinRT 프로젝션 Windows 네임스페이스 헤더를 Windows.ApplicationModel.Core 네임스페이스에 사용합니다. 빌드와 실행까지 마친 후 빈 공간을 클릭하여 색이 지정된 사각형을 추가한 다음 색이 지정된 사각형을 클릭하여 끌어옵니다.

### <a name="windows-runtime-component-cwinrt"></a>Windows 런타임 구성 요소(C++/WinRT)
일반적으로 유니버설 Windows 플랫폼(UWP)에서 사용되는 구성 요소용 프로젝트 템플릿입니다.

이 템플릿은 `midl.exe` > `cppwinrt.exe` 도구 체인을 나타냅니다. 이 경우 Windows 런타임 메타데이터(`.winmd`)가 IDL에서 생성되고, 그런 다음 구현체와 헤더 스텁이 Windows 런타임 메타데이터에서 생성됩니다.

IDL 파일에서 구성 요소의 런타임 클래스와 기본 인터페이스, 그리고 그 밖에 구현되는 인터페이스를 정의합니다. 프로젝트를 1회 빌드하여 `module.g.cpp`, `module.h.cpp`, 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 구성 요소의 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

빌드된 Windows 런타임 구성 요소 이진 파일과 이진 파일의 `.winmd`를 UWP 앱에 번들로 추가합니다.

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 빠른 시작
새 **Windows 콘솔 응용 프로그램(C++/WinRT)** 프로젝트를 만듭니다. 다음과 같이 `main.cpp`를 편집합니다.

```cppwinrt
// main.cpp

#include "pch.h"
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

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
        hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

추가된 헤더인 `winrt/Windows.Foundation.h`와 `winrt/Windows.Web.Syndication.h`는 SDK에서 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 폴더 안에 있습니다. Visual Studio는 *IncludePath* 매크로에 해당 경로를 포함시킵니다. 헤더에는 C++/WinRT에 프로젝션된 Windows API가 포함됩니다. Windows 네임스페이스의 유형을 사용할 때마다 해당하는 C++/WinRT 프로젝션의 Windows 네임스페이스 헤더를 다음과 같이 추가하세요. `using namespace` 지시문은 선택 사항이지만 편리합니다.

프로젝션된 형식은 모두 C++/WinRT 루트 네임스페이스인 **winrt**에 있습니다. [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)와 Windows SDK 모두 루트 네임스페이스인 **Windows**에 유형을 선언합니다. 이처럼 서로 다른 네임스페이스를 사용하면 사용자가 원하는 대로 C++/CX에서 C++/WinRT로 마이그레이션할 수 있습니다.

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)는 비동기식 Windows 런타임 함수의 예입니다. 위의 코드 예제는 **RetrieveFeedAsync**에서 비동기 연산 개체를 수신한 후 해당 개체에 대해 **get**을 호출하여 호출 스레드를 차단하고 결과를 기다립니다. 동시성에 대한 자세한 내용과 비차단 기법에 대해서는 [C++/WinRT로 동시성 및 비동기 작업](concurrency.md)을 참조하세요.

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)는 **begin** 및 **end** 함수에서 반환된 반복기(또는 상수, 역방향 및 상수-역방향 변형)로 정의되는 범위입니다. 이에 따라 범위 기반 `for` 문 또는 **std::for_each** 템플릿 함수와 함께 **Items**를 열거할 수 있습니다.

그런 다음 코드가 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 개체로서 피드의 제목 텍스트를 가져옵니다([C++/WinRT의 문자열 처리](strings.md) 참조). 그러면 **hstring**이 **c_str**를 통해 출력됩니다. C++ 표준 라이브러리의 문자열을 사용했다면 출력된 모습이 익숙하게 보일 것입니다.

이와 같이 C++/WinRT는 `syndicationItem.Title().Text()` 같이 클래스와 유사한 최신 C++ 표현식의 사용을 장려합니다. 이는 기존 COM 프로그래밍과 확연히 다를 뿐만 아니라 더욱 완전한 프로그래밍 스타일입니다. COM을 명시적으로 초기화하거나(**winrt::init_apartment**가 대신 수행), COM 포인터를 사용하거나, HRESULT 반환 코드를 처리할 필요도 없습니다. C++/WinRT가 HRESULT 오류를 예외로 전환하기 때문에 자연스러운 최신 프로그래밍 스타일을 유지합니다.

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 프로젝션의 사용자 지정 형식
C++/WinRT 프로그래밍에서는 표준 C++ 언어 기능과 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 비롯해 일부 C++ 표준 라이브러리 데이터 형식도 사용할 수 있습니다. 그 밖에도 프로젝션에서 일부 사용자 지정 데이터 형식에 대해서도 알아둘 필요가 있으며, 실제로 사용자 지정 데이터 형식을 사용할 수도 있습니다. 예를 들어, 위의 빠른 시작에서는 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 사용했습니다.

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array)는 임의 시점에서 사용할 가능성이 높은 또 하나의 형식입니다. 하지만 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 같은 형식은 직접 사용할 가능성이 비교적 낮습니다. 혹은 사용하지 않도록 선택할 수도 있는데, 이때는 C++ 표준 라이브러리에 해당하는 형식이 표시되더라도 어떤 코드도 변경할 필요 없습니다.

그 밖에 C++/WinRT 프로젝션의 Windows 네임스페이스 헤더에 대해서 자세히 공부할 경우 마주칠 수 있는 형식들도 있습니다. 그 한 예가 바로 **winrt::param::hstring**입니다. 이러한 형식들은 효율성을 이유로 존재할 뿐이기 때문에 코드에 사용해서는 안 됩니다.

## <a name="important-apis"></a>중요 API
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT의 문자열 처리](strings.md)
* [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
