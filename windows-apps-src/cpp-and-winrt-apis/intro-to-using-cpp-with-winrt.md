---
description: C++/WinRT 소개&mdash;Windows 런타임 API용 표준 C++ 언어 프로젝션
title: C++/WinRT 소개
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 소개
ms.localizationpriority: medium
ms.openlocfilehash: 883463f291864016ebc32f2d510936452c931366
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649698"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 소개
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT는 Windows 런타임(WinRT) API용 최신 표준 C++17 언어 프로젝션으로서 헤더 파일 기반 라이브러리로 구현되며, 오늘날 Windows API에 대해 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT에서는 모든 표준과 호환되는 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다. Windows SDK는 C++/WinRT를 포함하며, 버전 10.0.17134.0(Windows 10, 버전 1803)에서 도입되었습니다.

C + + /cli WinRT는 Microsoft의 권장 되는 대체는 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 언어 프로젝션 및 [Windows 런타임 c + + 템플릿 라이브러리 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)합니다. 전체 목록은 [C +에 대 한 항목 + WinRT](index.md#topics-about-cwinrt) 와 상호 운용 및에서 C + 이식에 대 한 정보를 포함 합니다. + CX 및 WRL 합니다.

> [!IMPORTANT]
> 두 가지는 가장 중요 한 C + + 알아야 할 WinRT 섹션에 설명 되어 있습니다 [SDK 지원 C + + WinRT](#sdk-support-for-cwinrt) 및 [Visual Studio 지원 C + + /cli WinRT, XAML, VSIX 확장 및 NuGet 패키지](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>언어 프로젝션
Windows 런타임은 구성 요소 개체 모델(COM)을 기반으로 하며, *언어 프로젝션*을 통해 액세스하도록 설계되었습니다. 프로젝션은 COM 세부 정보를 숨기며, 지정된 언어에 더욱 자연스러운 프로그래밍 환경을 제공합니다.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 참조 콘텐츠의 C++/WinRT 언어 프로젝션
[Windows UWP API](https://docs.microsoft.com/uwp/api/)를 검색할 때는 오른쪽 상단에 있는 **언어** 콤보 상자를 클릭한 다음 **C++/WinRT**를 선택하여 C++/WinRT 언어 프로젝션에 표시되는 API 구문 블록을 확인합니다.

## <a name="sdk-support-for-cwinrt"></a>C++/WinRT에 대한 SDK 지원
버전 10.0.17134.0(Windows 10 버전 1803)을 기준으로 Windows SDK는 자사 Windows API(Windows 네임스페이스의 Windows 런타임 API)를 사용하기 위한 헤더 파일 기반 표준 C++ 라이브러리를 포함합니다. C++/WinRT에는 C++/WinRT 코드에서 사용을 위해 메타데이터에 설명된 API를 *프로젝션*하는 헤더 파일 기반 표준 C++ 라이브러리를 생성하기 위해 Windows 런타임 메타데이터(`.winmd`) 파일을 가리킬 수 있는 `cppwinrt.exe` 도구도 제공됩니다. Windows 런타임 메타데이터(`.winmd`) 파일은 Windows 런타임 API 표면을 정식으로 설명하는 방법을 제공합니다. 메타데이터를 `cppwinrt.exe` 가리켜 타사 Windows 런타임 구성 요소에서 구현된 또는 응용 프로그램에서 구현된 모든 런타임 클래스와 함께 사용하기 위한 라이브러리를 생성할 수 있습니다. 자세한 정보는 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

C++/WinRT로 COM 스타일 프로그래밍을 이용하지 않고도 자체 표준 C++를 사용하여 런타임 클래스를 구현할 수도 있습니다. 런타임 클래스의 경우 IDL 파일에 형식만 설명하면 `midl.exe` 및 `cppwinrt.exe`가 상용구 소스 코드 파일 구현을 생성합니다. 또는 C++/WinRT 기본 클래스에서 파생하여 인터페이스를 구현할 수도 있습니다. 자세한 정보는 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio 지원 C + + /cli WinRT, XAML, VSIX 확장 및 NuGet 패키지
Visual Studio 지원에 대 한 Windows SDK 대상 버전 (Windows 10, 버전 1803) 10.0.17134.0 외에도 해야 Visual Studio 2017 (이상 버전 15.6; 15.7 이상 권장), 또는 Visual Studio 2019 합니다. 설치 해야 하지 않은 이미 설치한 경우에 **c + + 유니버설 Windows 플랫폼 도구** Visual Studio 설치 관리자 내에서 옵션입니다. 및 Windows **설정을** > **업데이트 \& 보안** > **개발자를 위한**을 선택 합니다 **개발자 모드** 옵션 대신 **테스트용으로 앱 로드** 옵션입니다.

최신 버전 다운로드 및 설치 해야 합니다 [C + + WinRT Visual Studio 확장 (VSIX)](https://aka.ms/cppwinrt/vsix) 에서 합니다 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)합니다.

- VSIX 확장을 사용 하면 C + + /cli WinRT 프로젝트 및 항목 템플릿을 Visual Studio에서 C +를 사용 하 여 시작할 수 있도록 + WinRT 개발 합니다.
- 또한 하면 Visual Studio 기본 디버그 시각화 (natvis) C + + /cli WinRT 형식 프로젝션 유사한 환경을 제공 C# 디버깅 합니다. Natvis는 디버그 빌드에 대해 자동으로 이루어집니다. WINRT_NATVIS 기호를 정의하여 빌드를 릴리스할 수도 있습니다.

Visual Studio 프로젝트 템플릿 C + + /cli WinRT는 다음과 같습니다. 만들 때 새 C + + /cli WinRT 프로젝트는 VSIX 확장을 설치한 경우 새 C +의 최신 버전을 사용 하 여 + WinRT 프로젝트를 자동으로 설치 합니다 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)합니다. 합니다 **Microsoft.Windows.CppWinRT** NuGet 패키지는 제공 C + + WinRT 빌드 지원 (MSBuild 속성 및 대상)의 경우 이식 가능한 프로젝트를 개발 컴퓨터와 빌드 에이전트 (기반이 NuGet 패키지만 및 VSIX 확장 설치 됩니다).

> [!IMPORTANT]
> 프로젝트 된 사용 하 여 만든 (또는 사용 하려면 업그레이드) 하는 경우 이전 VSIX 확장의 버전을 1.0.190128.4, 보다 확인할 [이전 버전의 VSIX 확장](#earlier-versions-of-the-vsix-extension)합니다. 해당 섹션 VSIX 확장의 최신 버전을 사용 하도록 업그레이드를 알아야 하는 프로젝트의 구성에 대 한 중요 한 정보를 포함 합니다.

때문에 C + + /cli WinRT는 C + + 17 표준 기능이, 프로젝트 속성 필요할 **C/c + +** > **언어** > **c + + 언어 표준**  >  **ISO C + + 17 표준을 (/ /std: c + + 17)** 합니다. 설정할 수도 있습니다 **준수 모드: 예 (허용 /-)**, 표준 규격 코드를 추가로 제한 하는 합니다.

알고 있어야 할 또 다른 프로젝트 속성은 **C/C++** > **일반** > **경고를 오류로 처리**입니다. 이 속성은 필요에 따라 **예(/WX)** 또는 **아니오(/WX-)** 로 설정하세요. 간혹 `cppwinrt.exe` 도구에서 생성된 소스 파일이 구현체가 추가될 때까지 경고를 생성하는 경우가 있습니다.

체제 위에 설명 된 대로 최대 설정 수 및 빌드를 만들거나 열 수, C + + /cli WinRT Visual Studio에서 프로젝트를 배포 합니다.

또는 수동으로 설치 하 여 기존 프로젝트를 변환할 수 있습니다 합니다 **Microsoft.Windows.CppWinRT** NuGet 패키지. 후 설치 (또는 업데이트) 최신 버전의 VSIX 확장을 Visual Studio에서 기존 프로젝트를 열고, 클릭 **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 입력 하거나 붙여 넣습니다 **Microsoft.Windows.CppWinRT** 검색 상자에서 검색 결과에서 항목을 선택 하 고 클릭 **설치** 해당 프로젝트에 대 한 패키지를 설치 합니다. 패키지를 추가한 후 얻게 C + + /cli WinRT MSBuild 지원 호출을 비롯 한 프로젝트를 `cppwinrt.exe` 도구.

C +를 사용 하는 프로젝트를 식별할 수 + 있는지에 따라 WinRT MSBuild 지원 합니다 **Microsoft.Windows.CppWinRT** 프로젝트 내에 설치 된 NuGet 패키지.

VSIX 확장에서 제공 되는 Visual Studio 프로젝트 템플릿을 다음과 같습니다.

### <a name="windows-console-application-cwinrt"></a>Windows 콘솔 응용 프로그램(C++/WinRT)
콘솔 사용자 인터페이스가 포함된 Windows 데스크톱의 C++/WinRT 클라이언트 응용 프로그램용 프로젝트 템플릿입니다.

### <a name="blank-app-cwinrt"></a>비어 있는 앱(C++/WinRT)
XAML 사용자 인터페이스가 있는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

Visual Studio는 각 XAML 태그 파일 다음에 있는 IDL(`.idl`) 파일에서 구현체와 헤더 스텁을 생성할 목적으로 XAML 컴파일러를 지원합니다. 먼저 IDL 파일에서 앱의 XAML 페이지에서 참조할 로컬 런타임 클래스를 모두 정의한 후 프로젝트를 1회 빌드하여 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 로컬 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

Visual Studio의 XAML 디자인 화면의 지원 C + + /cli WinRT가 대응 하는 C#입니다. 한 가지 예외는 합니다 **이벤트** 탭의 **속성** 창입니다. 사용 하 여는 C# 프로젝트, 이벤트 처리기를 추가 하려면 해당 탭을 사용할 수 있습니다 C + + /cli WinRT 프로젝트는 나타나지 않습니다. 하지만 [C + 대리자를 사용 하 여 이벤트를 처리 + WinRT](handle-events.md) 이벤트 처리기 코드를 추가 하는 방법에 대 한 정보에 대 한 합니다.

### <a name="core-app-cwinrt"></a>주요 앱(C++/WinRT)
XAML을 사용하지 않는 유니버설 Windows 플랫폼(UWP) 앱용 프로젝트 템플릿입니다.

대신 C++/WinRT Windows 네임스페이스 헤더를 Windows.ApplicationModel.Core 네임스페이스에 사용합니다. 빌드와 실행까지 마친 후 빈 공간을 클릭하여 색이 지정된 사각형을 추가한 다음 색이 지정된 사각형을 클릭하여 끌어옵니다.

### <a name="windows-runtime-component-cwinrt"></a>Windows 런타임 구성 요소(C++/WinRT)
일반적으로 유니버설 Windows 플랫폼(UWP)에서 사용되는 구성 요소용 프로젝트 템플릿입니다.

이 템플릿은 `midl.exe` > `cppwinrt.exe` 도구 체인을 나타냅니다. 이 경우 Windows 런타임 메타데이터(`.winmd`)가 IDL에서 생성되고, 그런 다음 구현체와 헤더 스텁이 Windows 런타임 메타데이터에서 생성됩니다.

IDL 파일에서 구성 요소의 런타임 클래스와 기본 인터페이스, 그리고 그 밖에 구현되는 인터페이스를 정의합니다. 프로젝트를 1회 빌드하여 `module.g.cpp`, `module.h.cpp`, 구현체 템플릿을 `Generated Files`에, 그리고 스텁 유형 정의를 `Generated Files\sources`에 생성합니다. 그런 다음 생성된 스텁 유형 정의를 참조에 사용하여 구성 요소의 런타임 클래스를 구현합니다. 런타임 클래스는 클래스 고유의 IDL 파일에 선언하는 것이 좋습니다.

빌드된 Windows 런타임 구성 요소 이진 파일과 이진 파일의 `.winmd`를 UWP 앱에 번들로 추가합니다.

## <a name="earlier-versions-of-the-vsix-extension"></a>VSIX 확장의 이전 버전
설치 하는 것이 좋습니다 (또는 업데이트)의 최신 버전의 [VSIX 확장](https://aka.ms/cppwinrt/vsix)합니다. 기본적으로 업데이트 되도록 구성 됩니다. 이렇게 하면 VSIX 확장 1.0.190128.4,이 섹션에서는 보다 이전 버전을 사용 하 여 만든 프로젝트에 있는 경우 새 버전을 사용 하려면 해당 프로젝트를 업그레이드 하는 방법에 대 한 중요 한 정보를 포함 합니다. 업데이트 하지 않으면 다음 여전히 있습니다 정보를이 단원의 유용 합니다.

측면에서 Windows SDK 및 Visual Studio 버전 및 Visual Studio 구성에서 정보를 지원 합니다 [Visual Studio 지원 C + + /cli WinRT, XAML, VSIX 확장 및 NuGet 패키지](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 위의 섹션 앞에 적용 됩니다 VSIX 확장의 버전입니다. 동작에 대 한 중요 한 차이점을 설명 하는 아래 정보 및 구성을 사용 하 여 만든 (또는 사용 하려면 업그레이드) 프로젝트 이전 버전입니다.

### <a name="created-earlier-than-101810022"></a>1.0.181002.2 보다 먼저 생성
이전 보다 1.0.181002.2, C + VSIX 확장의 버전을 사용 하 여 프로젝트를 만든 경우 + WinRT 빌드 지원 VSIX 확장의 버전으로 빌드된 합니다. 프로젝트에는 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성에서 설정 된 `.vcxproj` 파일.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

수동으로 설치 하 여 프로젝트를 업그레이드할 수는 **Microsoft.Windows.CppWinRT** NuGet 패키지. 후 설치 (또는 업그레이드) 최신 버전의 VSIX 확장을 Visual Studio에서 프로젝트를 열고, 클릭 **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 입력 하거나 붙여 넣습니다 **Microsoft.Windows.CppWinRT** 검색 상자에서 검색 결과에서 항목을 선택 하 고 클릭 **설치** 프로젝트에 대 한 패키지를 설치 합니다.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>사용 하 여 만든 (또는로 업그레이드) 1.0.181002.2 사이의 1.0.190128.3
1.0.181002.2 사이의 경계가 1.0.190128.3 VSIX 확장의 버전을 사용 하 여 프로젝트를 만든 경우 해당 **Microsoft.Windows.CppWinRT** 프로젝트에서 자동으로 프로젝트에 설치 된 NuGet 패키지 템플릿입니다. 이 범위에서 VSIX 확장의 버전을 사용 하 여 이전 프로젝트를 업그레이드도 될 수 있습니다. 수행한 경우, 한 다음&mdash;빌드 지원 여전히이 범위에서 VSIX 확장의 버전도 되었으므로&mdash;업그레이드 된 프로젝트 있거나 없을 수 있습니다 합니다 **Microsoft.Windows.CppWinRT** NuGet 패키지 설치 합니다.

프로젝트를 업그레이드 하려면 이전 섹션의 지침에 따라 및 프로젝트에 있는지 확인 합니다 **Microsoft.Windows.CppWinRT** NuGet 패키지를 설치 합니다.

### <a name="invalid-upgrade-configurations"></a>잘못 된 업그레이드 구성
VSIX 확장의 최신 버전에서는 유효 하지 않은 빌드하도록 하려면 프로젝트에 대 한는 `<CppWinRTEnabled>true</CppWinRTEnabled>` 도 없는 경우 속성을 **Microsoft.Windows.CppWinRT** NuGet 패키지가 설치 합니다. 이 구성 사용 하 여 프로젝트 빌드 오류 메시지를 생성 합니다. "C + + /cli WinRT VSIX 프로젝트 빌드 지원을 제공 하지 않습니다.  Microsoft.Windows.CppWinRT Nuget 패키지에 프로젝트 참조를 추가 하십시오. "

위에서 설명한 것 처럼, C + + /cli WinRT 프로젝트는 이제 NuGet 패키지를 설치 해야 합니다.

이후를 `<CppWinRTEnabled>` 요소는 이제 사용 되지 않는, 필요에 따라 편집할 수 있습니다에 `.vcxproj`, 및 요소를 삭제 합니다. 반드시 필요한 것 이지만 옵션입니다.

또한 경우에 `.vcxproj` 포함 `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, C + 요구 하지 않고 빌드할 수 있도록 제거할 수 있습니다 + WinRT VSIX 확장을 설치 합니다.

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 프로젝션의 사용자 지정 형식
C + + WinRT 프로그래밍, 표준 c + + 언어 기능을 사용할 수 있습니다 및 [표준 c + + 데이터 형식 및 C + + /cli WinRT](std-cpp-data-types.md)&mdash;c + + 표준 라이브러리 데이터 형식도 포함 합니다. 그 밖에도 프로젝션에서 일부 사용자 지정 데이터 형식에 대해서도 알아둘 필요가 있으며, 실제로 사용자 지정 데이터 형식을 사용할 수도 있습니다. 예를 들어 [C++/WinRT 시작](get-started.md)의 빠른 시작 코드 예제에서 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 사용합니다.

[**winrt::com_array** ](/uwp/cpp-ref-for-winrt/com-array) 일부 지점에서 사용 하는 일을 할 수 있는 다른 형식입니다. 하지만 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 같은 형식은 직접 사용할 가능성이 비교적 낮습니다. 혹은 사용하지 않도록 선택할 수도 있는데, 이때는 C++ 표준 라이브러리에 해당하는 형식이 표시되더라도 어떤 코드도 변경할 필요 없습니다.

> [!WARNING]
> 그 밖에 C++/WinRT Windows 네임스페이스 헤더에 대해서 자세히 공부할 경우 마주칠 수 있는 형식들도 있습니다. 예는 **winrt::param::hstring**이지만 컬렉션 예도 있습니다. 이는 오로지 입력 매개 변수의 바인딩을 최적화하기 위해서만 존재하며 크게 성능을 향상하고 관련 표준 C++ 형식 및 컨테이너를 위해 "단지 작동"하는 호출 패턴을 최대한 활용합니다. 이러한 형식은 대부분의 값을 추가하는 경우에 프로젝션에 의해서만 사용됩니다. 우수하게 최적화되었으며 일반적으로 용도로 사용되지 않습니다. 직접 사용하려고 하지 마십시오. `winrt::impl` 네임스페이스도 구현 형식이며 따라서 변경되기 쉬우므로 여기에서 어떤 것도 사용해서는 안 됩니다. 표준 형식이나 [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)의 형식을 계속 사용해야 합니다.

## <a name="important-apis"></a>중요 API
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C + + /cli WinRT Visual Studio 확장 (VSIX)](https://aka.ms/cppwinrt/vsix)
* [C++/WinRT 시작](get-started.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)
* [문자열 처리 C + + /cli WinRT](strings.md)
* [Windows UWP Api](https://docs.microsoft.com/uwp/api/)
