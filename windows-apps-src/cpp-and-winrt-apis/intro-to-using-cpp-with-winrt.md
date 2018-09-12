---
author: stevewhims
description: C++/WinRT 소개&mdash;Windows 런타임 API용 표준 C++ 언어 프로젝션
title: C++/WinRT 소개
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 소개
ms.localizationpriority: medium
ms.openlocfilehash: 220c5c7395ed9388b02b74e0cbed5b913971bbba
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3932846"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 소개
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT에서는 모든 표준과 호환되는 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다. Windows SDK는 C++/WinRT를 포함하며, 버전 10.0.17134.0(Windows 10, 버전 1803)에서 도입되었습니다.

C + + /winrt는 Microsoft의 권장된 대체는 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 언어 프로젝션 및 [Windows 런타임 c + + 템플릿 라이브러리 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). 전체 목록은 [항목에 대해 C + + WinRT](index.md#topics-about-cwinrt) 와 상호 운용에서 C + 이식 하는 방법에 대 한 정보를 포함 + CX 및 WRL 합니다.

> [!IMPORTANT]
> C++/WinRT에서 가장 중요하여 반드시 알고 있어야 할 두 가지 정보는 섹션 [C++/WinRT에 대한 SDK 지원](#sdk-support-for-cwinrt)과 섹션 [C++/WinRT에 대한 Visual Studio 지원 및 VSIX](#visual-studio-support-for-cwinrt-and-the-vsix)에 설명되어 있습니다.

## <a name="language-projections"></a>언어 프로젝션
Windows 런타임은 구성 요소 개체 모델(COM)을 기반으로 하며, *언어 프로젝션*을 통해 액세스하도록 설계되었습니다. 프로젝션은 COM 세부 정보를 숨기며, 지정된 언어에 더욱 자연스러운 프로그래밍 환경을 제공합니다.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 참조 콘텐츠의 C++/WinRT 언어 프로젝션
[Windows UWP API](https://docs.microsoft.com/uwp/api/)를 검색할 때는 오른쪽 상단에 있는 **언어** 콤보 상자를 클릭한 다음 **C++/WinRT**를 선택하여 C++/WinRT 언어 프로젝션에 표시되는 API 구문 블록을 확인합니다.

## <a name="sdk-support-for-cwinrt"></a>C++/WinRT에 대한 SDK 지원
버전 10.0.17134.0(Windows 10 버전 1803)을 기준으로 Windows SDK는 자사 Windows API(Windows 네임스페이스의 Windows 런타임 API)를 사용하기 위한 헤더 파일 기반 표준 C++ 라이브러리를 포함합니다. C++/WinRT에는 C++/WinRT 코드에서 사용을 위해 메타데이터에 설명된 API를 *프로젝션*하는 헤더 파일 기반 표준 C++ 라이브러리를 생성하기 위해 Windows 런타임 메타데이터(`.winmd`) 파일을 가리킬 수 있는 `cppwinrt.exe` 도구도 제공됩니다. Windows 런타임 메타데이터(`.winmd`) 파일은 Windows 런타임 API 표면을 정식으로 설명하는 방법을 제공합니다. 메타데이터를 `cppwinrt.exe` 가리켜 타사 Windows 런타임 구성 요소에서 구현된 또는 응용 프로그램에서 구현된 모든 런타임 클래스와 함께 사용하기 위한 라이브러리를 생성할 수 있습니다. 자세한 정보는 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

C++/WinRT로 COM 스타일 프로그래밍을 이용하지 않고도 자체 표준 C++를 사용하여 런타임 클래스를 구현할 수도 있습니다. 런타임 클래스의 경우 IDL 파일에 형식만 설명하면 `midl.exe` 및 `cppwinrt.exe`가 상용구 소스 코드 파일 구현을 생성합니다. 또는 C++/WinRT 기본 클래스에서 파생하여 인터페이스를 구현할 수도 있습니다. 자세한 정보는 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>C++/WinRT에 대한 Visual Studio 지원 및 VSIX
Visual Studio의 C++/WinRT 프로젝트 템플릿과 C++/WinRT MSBuild 속성 및 대상의 경우에는 [C++/WinRT Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix)을 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)에서 다운로드하여 설치하세요.

Visual Studio 2017(버전 15.6 이상, 15.7 이상 권장) 및 Windows SDK 버전 10.0.17134.0(Windows 10 버전 1803)이 필요합니다. 설치 이미 하지 않은 경우에 Visual Studio 설치 관리자 내에서 **c + + 유니버설 Windows 플랫폼 도구** 옵션을 설치 해야 합니다. Windows **설정**에서 > **업데이트 \ 및 보안** > **개발자를 위한** **앱 테스트용 로드** 옵션 보다는 **개발자 모드** 옵션을 선택 합니다.

하면 다음 수 및 빌드, 만들거나 열, C + + /winrt Visual Studio에서 프로젝트와 배포 합니다. 또는 추가 하 여 기존 프로젝트를 변환할 수는 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성을 해당 `.vcxproj` 파일.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

속성 추가를 마쳤으면 이제 `cppwinrt.exe` 도구 호출을 포함해 프로젝트에 대한 C++/WinRT MSBuild 지원을 가져옵니다.

때문에 C + + WinRT는 c++17 표준의 기능을 사용, 필요한 프로젝트 속성 **C/c + +** > **언어** > **c + + 언어 표준** > **ISO c++17 표준 (/ (/std:c++17 + + 17)** 입니다. 또한 **적합성 모드: 예(/permissive-)** 를 설정하여 코드가 표준을 더욱 따르도록 할 수도 있습니다.

알고 있어야 할 또 다른 프로젝트 속성은 **C/C++** > **일반** > **경고를 오류로 처리**입니다. 이 속성은 필요에 따라 **예(/WX)** 또는 **아니오(/WX-)** 로 설정하세요. 간혹 `cppwinrt.exe` 도구에서 생성된 소스 파일이 구현체가 추가될 때까지 경고를 생성하는 경우가 있습니다.

VSIX는 C++/WinRT 프로젝션된 형식의 Visual Studio 기본 디버그 시각화(natvis)도 제공하여 C# 디버깅과 유사한 경험을 제공합니다. Natvis는 디버그 빌드에 대해 자동으로 이루어집니다. WINRT_NATVIS 기호를 정의하여 빌드를 릴리스할 수도 있습니다.

VSIX에서 제공되는 Visual Studio 프로젝트 템플릿은 다음과 같습니다.

### <a name="windows-console-application-cwinrt"></a>Windows 콘솔 응용 프로그램(C++/WinRT)
콘솔 사용자 인터페이스가 포함된 Windows 데스크톱의 C++/WinRT 클라이언트 응용 프로그램용 프로젝트 템플릿입니다.

### <a name="blank-app-cwinrt"></a>비어 있는 앱(C++/WinRT)
XAML 사용자 인터페이스가 있는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

Visual Studio는 각 XAML 태그 파일 다음에 있는 IDL(`.idl`) 파일에서 구현체와 헤더 스텁을 생성할 목적으로 XAML 컴파일러를 지원합니다. 먼저 IDL 파일에서 앱의 XAML 페이지에서 참조할 로컬 런타임 클래스를 모두 정의한 후 프로젝트를 1회 빌드하여 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 로컬 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

### <a name="core-app-cwinrt"></a>주요 앱(C++/WinRT)
XAML을 사용하지 않는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

대신 C++/WinRT Windows 네임스페이스 헤더를 Windows.ApplicationModel.Core 네임스페이스에 사용합니다. 빌드와 실행까지 마친 후 빈 공간을 클릭하여 색이 지정된 사각형을 추가한 다음 색이 지정된 사각형을 클릭하여 끌어옵니다.

### <a name="windows-runtime-component-cwinrt"></a>Windows 런타임 구성 요소(C++/WinRT)
일반적으로 유니버설 Windows 플랫폼(UWP)에서 사용되는 구성 요소용 프로젝트 템플릿입니다.

이 템플릿은 `midl.exe` > `cppwinrt.exe` 도구 체인을 나타냅니다. 이 경우 Windows 런타임 메타데이터(`.winmd`)가 IDL에서 생성되고, 그런 다음 구현체와 헤더 스텁이 Windows 런타임 메타데이터에서 생성됩니다.

IDL 파일에서 구성 요소의 런타임 클래스와 기본 인터페이스, 그리고 그 밖에 구현되는 인터페이스를 정의합니다. 프로젝트를 1회 빌드하여 `module.g.cpp`, `module.h.cpp`, 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 구성 요소의 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

빌드된 Windows 런타임 구성 요소 이진 파일과 이진 파일의 `.winmd`를 UWP 앱에 번들로 추가합니다.

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 프로젝션의 사용자 지정 형식
C++/WinRT 프로그래밍에서 일부 C++ 표준 라이브러리 데이터 형식을 포함한 표준 C++ 언어 기능과 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 사용할 수 있습니다. 그 밖에도 프로젝션에서 일부 사용자 지정 데이터 형식에 대해서도 알아둘 필요가 있으며, 실제로 사용자 지정 데이터 형식을 사용할 수도 있습니다. 예를 들어 [C++/WinRT 시작](get-started.md)의 빠른 시작 코드 예제에서 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 사용합니다.

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array)는 임의 시점에서 사용할 가능성이 높은 또 하나의 형식입니다. 하지만 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 같은 형식은 직접 사용할 가능성이 비교적 낮습니다. 혹은 사용하지 않도록 선택할 수도 있는데, 이때는 C++ 표준 라이브러리에 해당하는 형식이 표시되더라도 어떤 코드도 변경할 필요 없습니다.

> [!WARNING]
> 그 밖에 C++/WinRT Windows 네임스페이스 헤더에 대해서 자세히 공부할 경우 마주칠 수 있는 형식들도 있습니다. 예는 **winrt::param::hstring**이지만 컬렉션 예도 있습니다. 이는 오로지 입력 매개 변수의 바인딩을 최적화하기 위해서만 존재하며 크게 성능을 향상하고 관련 표준 C++ 형식 및 컨테이너를 위해 "단지 작동"하는 호출 패턴을 최대한 활용합니다. 이러한 형식은 대부분의 값을 추가하는 경우에 프로젝션에 의해서만 사용됩니다. 우수하게 최적화되었으며 일반적으로 용도로 사용되지 않습니다. 직접 사용하려고 하지 마십시오. `winrt::impl` 네임스페이스도 구현 형식이며 따라서 변경되기 쉬우므로 여기에서 어떤 것도 사용해서는 안 됩니다. 표준 형식이나 [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)의 형식을 계속 사용해야 합니다.

## <a name="important-apis"></a>중요 API
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio Extension(VSIX)](https://aka.ms/cppwinrt/vsix)
* [C++/WinRT 시작](get-started.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT의 문자열 처리](strings.md)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
