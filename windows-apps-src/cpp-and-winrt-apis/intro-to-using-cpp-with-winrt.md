---
description: C++/WinRT 소개&mdash;Windows 런타임 API용 표준 C++ 언어 프로젝션
title: C++/WinRT 소개
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 소개
ms.localizationpriority: medium
ms.openlocfilehash: 2c7334711debf87d8834213af39ba384166404e1
ms.sourcegitcommit: 2d2483819957619b6de21b678caf887f3b1342af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042376"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 소개
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT에서는 모든 표준과 호환되는 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다. Windows SDK는 C++/WinRT를 포함하며, 버전 10.0.17134.0(Windows 10, 버전 1803)에서 도입되었습니다.

C + + /winrt는 Microsoft의 권장된 대체 합니다 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 언어 프로젝션 및 [Windows 런타임 c + + 템플릿 라이브러리 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). 전체 목록은 [항목 C + + WinRT](index.md#topics-about-cwinrt) 와 상호 작용 및에서 포트, C + 모두에 대 한 정보를 포함 + CX 및 WRL 합니다.

> [!IMPORTANT]
> C +의 가장 중요 한 부분 중 두 가지 + 고려해 야 할 WinRT 섹션에 설명 되어 [SDK 지원 C + + WinRT](#sdk-support-for-cwinrt) 및 [Visual Studio 지원 C + + WinRT, XAML, VSIX 확장과 NuGet 패키지](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.

## <a name="language-projections"></a>언어 프로젝션
Windows 런타임은 구성 요소 개체 모델(COM)을 기반으로 하며, *언어 프로젝션*을 통해 액세스하도록 설계되었습니다. 프로젝션은 COM 세부 정보를 숨기며, 지정된 언어에 더욱 자연스러운 프로그래밍 환경을 제공합니다.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 참조 콘텐츠의 C++/WinRT 언어 프로젝션
[Windows UWP API](https://docs.microsoft.com/uwp/api/)를 검색할 때는 오른쪽 상단에 있는 **언어** 콤보 상자를 클릭한 다음 **C++/WinRT**를 선택하여 C++/WinRT 언어 프로젝션에 표시되는 API 구문 블록을 확인합니다.

## <a name="sdk-support-for-cwinrt"></a>C++/WinRT에 대한 SDK 지원
버전 10.0.17134.0(Windows 10 버전 1803)을 기준으로 Windows SDK는 자사 Windows API(Windows 네임스페이스의 Windows 런타임 API)를 사용하기 위한 헤더 파일 기반 표준 C++ 라이브러리를 포함합니다. C++/WinRT에는 C++/WinRT 코드에서 사용을 위해 메타데이터에 설명된 API를 *프로젝션*하는 헤더 파일 기반 표준 C++ 라이브러리를 생성하기 위해 Windows 런타임 메타데이터(`.winmd`) 파일을 가리킬 수 있는 `cppwinrt.exe` 도구도 제공됩니다. Windows 런타임 메타데이터(`.winmd`) 파일은 Windows 런타임 API 표면을 정식으로 설명하는 방법을 제공합니다. 메타데이터를 `cppwinrt.exe` 가리켜 타사 Windows 런타임 구성 요소에서 구현된 또는 응용 프로그램에서 구현된 모든 런타임 클래스와 함께 사용하기 위한 라이브러리를 생성할 수 있습니다. 자세한 정보는 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

C++/WinRT로 COM 스타일 프로그래밍을 이용하지 않고도 자체 표준 C++를 사용하여 런타임 클래스를 구현할 수도 있습니다. 런타임 클래스의 경우 IDL 파일에 형식만 설명하면 `midl.exe` 및 `cppwinrt.exe`가 상용구 소스 코드 파일 구현을 생성합니다. 또는 C++/WinRT 기본 클래스에서 파생하여 인터페이스를 구현할 수도 있습니다. 자세한 정보는 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio 지원 C + + WinRT, XAML, VSIX 확장과 NuGet 패키지
Visual Studio 지원에 대 한는 최소 Windows SDK 대상 버전인 10.0.17134.0 (Windows 10, 버전 1803) 외에도 필요 Visual Studio 2017 (최소 버전 15.6 이상, 15.7 이상 권장), 또는 Visual Studio 2019 합니다. 설치 이미 하지 않은 경우 Visual Studio 설치 관리자 내에서 **c + + 유니버설 Windows 플랫폼 도구** 옵션을 설치 해야 합니다. Windows **설정**에서 > **\& 보안 업데이트** > **개발자를 위한** **앱 테스트용 로드** 옵션 보다는 **개발자 모드** 옵션을 선택 합니다.

다운로드 하 고 최신 버전을 설치 해야 합니다 [C + + WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) [Visual Studio Marketplace](https://marketplace.visualstudio.com/)에서.

- VSIX 확장을 사용 하면 C + + /winrt 프로젝트 템플릿과 항목 템플릿과 Visual Studio에서 C + 차별화할 수 있도록 + WinRT 개발 합니다.
- 또한 하면 Visual Studio 기본 디버그 시각화 (natvis) C + + /winrt 프로젝션 된 형식의; C# 디버깅과 유사한 경험을 제공 합니다. Natvis는 디버그 빌드에 대해 자동으로 이루어집니다. WINRT_NATVIS 기호를 정의하여 빌드를 릴리스할 수도 있습니다.

Visual Studio 프로젝트 템플릿은 C + + WinRT는 다음과 같습니다. 만들 때 새 C + + WinRT 프로젝트 최신 버전의 설치 된 VSIX 확장 새로운 C + + WinRT 프로젝트 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)를 자동으로 설치 합니다. **Microsoft.Windows.CppWinRT** NuGet 패키지를 제공 C + + WinRT 빌드 (MSBuild 속성 및 대상)를 지원 프로젝트 빌드 에이전트는 개발 컴퓨터와 휴대용 만들기 (기반이 NuGet 패키지 및 VSIX 확장 설치 됨).

> [!IMPORTANT]
> 사용 하 여 만든 (또는 작업할 업그레이드)는 프로젝트에 있는 경우 이전 VSIX 확장의 버전 보다 1.0.190128.4, 그러면 [VSIX 확장의 이전 버전](#earlier-versions-of-the-vsix-extension)입니다. 해당 섹션 VSIX 확장의 최신 버전을 사용 하 여 업그레이드 하기 전에 알아야 할 해야 하는 프로젝트의 구성에 대 한 중요 한 정보를 포함 합니다.

때문에 C + + 필요한 프로젝트 속성 **C/c + +**, C + + 17 표준의 기능을 사용 하 여 WinRT > **언어** > **c + + 언어 표준** > **ISO C + + 17 표준 (/ /std: + + 17)**. 또한 **적합성 모드: 예(/permissive-)** 를 설정하여 코드가 표준을 더욱 따르도록 할 수도 있습니다.

알고 있어야 할 또 다른 프로젝트 속성은 **C/C++** > **일반** > **경고를 오류로 처리**입니다. 이 속성은 필요에 따라 **예(/WX)** 또는 **아니오(/WX-)** 로 설정하세요. 간혹 `cppwinrt.exe` 도구에서 생성된 소스 파일이 구현체가 추가될 때까지 경고를 생성하는 경우가 있습니다.

위에서 설명한 대로 최대 설정 시스템을 사용 하 여 수 및 빌드, 만들거나 열 수, C + + /winrt Visual Studio에서 프로젝트와 배포 합니다.

또는 수동으로 **Microsoft.Windows.CppWinRT** NuGet 패키지를 설치 하 여 기존 프로젝트를 변환할 수 있습니다. 후 설치 (또는 업데이트) 최신 버전의 VSIX 확장을 Visual Studio에서 기존 프로젝트를 열고, **프로젝트**를 클릭 \> **NuGet 패키지 관리...**  \>  **찾아보기**입력 또는 **Microsoft.Windows.CppWinRT** 검색 상자에 붙여 넣을, 검색 결과에서 항목을 선택 하 고 다음 해당 프로젝트에 대 한 패키지를 설치 하려면 **설치** 를 클릭 합니다. 패키지를 추가 하 고 나면 받게 C + + /winrt MSBuild 지원을 호출을 포함 해 프로젝트에 대 한는 `cppwinrt.exe` 도구입니다.

C +를 사용 하는 프로젝트를 식별할 수 + /winrt MSBuild 지원을 프로젝트 내에 설치 된 **Microsoft.Windows.CppWinRT** NuGet 패키지의 존재 여부에 따라 합니다.

VSIX 확장에서 제공 하는 Visual Studio 프로젝트 템플릿은 다음과 같습니다.

### <a name="windows-console-application-cwinrt"></a>Windows 콘솔 응용 프로그램(C++/WinRT)
콘솔 사용자 인터페이스가 포함된 Windows 데스크톱의 C++/WinRT 클라이언트 응용 프로그램용 프로젝트 템플릿입니다.

### <a name="blank-app-cwinrt"></a>비어 있는 앱(C++/WinRT)
XAML 사용자 인터페이스가 있는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

Visual Studio는 각 XAML 태그 파일 다음에 있는 IDL(`.idl`) 파일에서 구현체와 헤더 스텁을 생성할 목적으로 XAML 컴파일러를 지원합니다. 먼저 IDL 파일에서 앱의 XAML 페이지에서 참조할 로컬 런타임 클래스를 모두 정의한 후 프로젝트를 1회 빌드하여 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 로컬 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

Visual Studio의 XAML 디자인 화면 지원 C + + /winrt는 C#와 가까운 합니다. **속성** 창의 **이벤트** 탭은 예외가입니다. C# 프로젝트를 사용 하 여 이벤트 처리기를 추가 하려면 해당 탭을 사용할 수 있습니다. C + + WinRT 프로젝트에 해당 기능 나타나지 않습니다. 표시 [C +의 대리자를 사용 하 여 이벤트를 처리 + WinRT](handle-events.md) 이벤트 처리기 코드를 추가 하는 방법에 대 한 내용은 합니다.

### <a name="core-app-cwinrt"></a>주요 앱(C++/WinRT)
XAML을 사용하지 않는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

대신 C++/WinRT Windows 네임스페이스 헤더를 Windows.ApplicationModel.Core 네임스페이스에 사용합니다. 빌드와 실행까지 마친 후 빈 공간을 클릭하여 색이 지정된 사각형을 추가한 다음 색이 지정된 사각형을 클릭하여 끌어옵니다.

### <a name="windows-runtime-component-cwinrt"></a>Windows 런타임 구성 요소(C++/WinRT)
일반적으로 유니버설 Windows 플랫폼(UWP)에서 사용되는 구성 요소용 프로젝트 템플릿입니다.

이 템플릿은 `midl.exe` > `cppwinrt.exe` 도구 체인을 나타냅니다. 이 경우 Windows 런타임 메타데이터(`.winmd`)가 IDL에서 생성되고, 그런 다음 구현체와 헤더 스텁이 Windows 런타임 메타데이터에서 생성됩니다.

IDL 파일에서 구성 요소의 런타임 클래스와 기본 인터페이스, 그리고 그 밖에 구현되는 인터페이스를 정의합니다. 프로젝트를 1회 빌드하여 `module.g.cpp`, `module.h.cpp`, 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 구성 요소의 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

빌드된 Windows 런타임 구성 요소 이진 파일과 이진 파일의 `.winmd`를 UWP 앱에 번들로 추가합니다.

## <a name="earlier-versions-of-the-vsix-extension"></a>VSIX 확장의 이전 버전
설치 하는 것이 좋습니다 (또는 업데이트) [VSIX 확장](https://aka.ms/cppwinrt/vsix)의 최신 버전입니다. 기본적으로 자체적으로 업데이트 하도록 구성 되어 있습니다. 이렇게 하면 VSIX 확장 이전의 1.0.190128.4, 다음이 섹션의 버전을 사용 하 여 만든 프로젝트에 있는 경우 새 버전으로 작동 하도록 해당 프로젝트를 업그레이드 하는 방법에 대 한 중요 한 정보를 포함 합니다. 업데이트 하지 않으면 다음 여전히 찾을 수 정보가이 섹션에 유용 합니다.

측면에서 Windows SDK와 Visual Studio 버전 및 Visual Studio 구성 정보를 지원 합니다 [Visual Studio 지원 C + + WinRT, XAML, VSIX 확장과 NuGet 패키지](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 위 섹션 VSIX의 이전 버전에 적용 됩니다. 확장 합니다. 아래의 정보 동작에 대 한 중요 한 차이점을 설명 하 고의 구성을 사용 하 여 만든 (또는 작업할 업그레이드 된) 프로젝트 이전 버전입니다.

### <a name="created-earlier-than-101810022"></a>1.0.181002.2 보다 앞에서 만든
이전의 1.0.181002.2를 다음 C + VSIX 확장의 버전을 사용 하 여 프로젝트를 만든 경우 + WinRT 빌드 지원이 VSIX 확장의 해당 버전에 기본 제공 되었습니다. 프로젝트에는 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성는 `.vcxproj` 파일.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

수동으로 **Microsoft.Windows.CppWinRT** NuGet 패키지를 설치 하 여 프로젝트를 업그레이드할 수 있습니다. 후 설치 (또는 업그레이드를) 최신 버전의 VSIX 확장을 Visual Studio에서 프로젝트를 열고, **프로젝트**를 클릭 \> **NuGet 패키지 관리...**  \>  **찾아보기**, 입력 또는 **Microsoft.Windows.CppWinRT** 검색 상자에 붙여 넣을, 검색 결과에서 항목을 선택 및 프로젝트에 대 한 패키지를 설치 하려면 **설치** 를 차례로 클릭 합니다. 그런 다음 편집에 `.vcxproj` , 파일을 제거 합니다 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성입니다.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>사용 하 여 만든 (또는 업그레이드) 1.0.181002.2 사이의 1.0.190128.3
1.0.181002.2 사이의 1.0.190128.3 VSIX 확장의 버전을 사용 하 여 프로젝트를 만든 경우 포함 한 다음 **Microsoft.Windows.CppWinRT** NuGet 패키지가 설치 된 프로젝트에 자동으로 프로젝트 템플릿에 합니다. 이 범위에 대 한 VSIX 확장의 버전을 사용 하기 위해 이전 프로젝트를 업그레이드할 수도 있습니다. 만약 다음&mdash;빌드 지원이이 범위에 대 한 VSIX 확장의 버전에 여전히 존재도 이후에&mdash;업그레이드 된 프로젝트 수도 **Microsoft.Windows.CppWinRT** NuGet 패키지를 설치 하지 않은 합니다.

프로젝트를 업그레이드 하려면 이전 섹션의 지침에 따라 하 고 프로젝트 **Microsoft.Windows.CppWinRT** NuGet 패키지를 설치 않았는지 확인 합니다. 그런 다음도 제거는 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성입니다.

### <a name="invalid-upgrade-configurations"></a>잘못 된 업그레이드 구성
VSIX 확장의 최신 버전을 사용 하 여 유효 하지 않은 할 프로젝트는 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성 또한 **Microsoft.Windows.CppWinRT** NuGet 패키지를 설치 되어 있지 않을 경우. 이 구성 사용 하 여 프로젝트 빌드 오류 메시지를 생성 "C + + /winrt VSIX는 더 이상 프로젝트 빌드 지원을 제공 합니다.  Microsoft.Windows.CppWinRT Nuget 패키지에 대 한 프로젝트 참조를 추가 하세요. "

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 프로젝션의 사용자 지정 형식
C + + /winrt 프로그래밍에서는 표준 c + + 언어 기능을 사용할 수 및 [표준 c + + 데이터 형식 및 C + + WinRT](std-cpp-data-types.md)&mdash;일부 c + + 표준 라이브러리 데이터 형식이 포함 됩니다. 그 밖에도 프로젝션에서 일부 사용자 지정 데이터 형식에 대해서도 알아둘 필요가 있으며, 실제로 사용자 지정 데이터 형식을 사용할 수도 있습니다. 예를 들어 [C++/WinRT 시작](get-started.md)의 빠른 시작 코드 예제에서 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 사용합니다.

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
