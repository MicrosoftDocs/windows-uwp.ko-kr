---
description: Windows 런타임 API용 표준 C++ 언어 프로젝션인 C++/WinRT에 대한 소개입니다.
title: C++/WinRT 소개
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 소개
ms.localizationpriority: medium
ms.openlocfilehash: fd267f96ca6931252ab3130d363447ae79820108
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209138"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 소개
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT를 사용하면 모든 표준 호환 C++17 컴파일러를 통해 Windows 런타임 API를 작성하고 사용할 수 있습니다. Windows SDK는 C++/WinRT를 포함하며, 10.0.17134.0 버전(Windows 10 버전 1803)에서 도입되었습니다.

C++/WinRT는 Microsoft에서 추천하는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 언어 프로젝션 및 [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)용 대체 언어입니다. [C++/WinRT 관련 항목](index.md#topics-about-cwinrt)의 전체 목록에는 C++/CX 및 WRL과 상호 운용하고 이 언어에서 이식하는 방법에 대한 정보가 모두 포함됩니다.

> [!IMPORTANT]
> 알아 두어야 할 가장 중요한 몇 가지 C++/WinRT 정보는 [C++/WinRT에 대한 SDK 지원](#sdk-support-for-cwinrt) 섹션과 [C++/WinRT, XAML, VSIX 확장 및 NuGet 패키지에 대한 Visual Studio 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 섹션에 설명되어 있습니다.

## <a name="language-projections"></a>언어 프로젝션
Windows 런타임은 COM(구성 요소 개체 모델) API를 기반으로 하며, ‘언어 프로젝션’을 통해 액세스하도록 설계되었습니다.  프로젝션은 COM 세부 정보를 숨기며, 지정된 언어에 더욱 자연스러운 프로그래밍 환경을 제공합니다.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 참조 콘텐츠의 C++/WinRT 언어 프로젝션
[Windows UWP API](https://docs.microsoft.com/uwp/api/)를 검색할 때는 오른쪽 위에 있는 **언어** 콤보 상자를 클릭하고 **C++/WinRT**를 선택하여 C++/WinRT 언어 프로젝션에 표시되는 API 구문 블록을 확인합니다.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>C++/WinRT, XAML, VSIX 확장 및 NuGet 패키지에 대한 Visual Studio 지원
Visual Studio 지원의 경우 Visual Studio 2019 또는 Visual Studio 2017(최소 15.6 버전, 15.7 이상 권장)이 필요합니다. Visual Studio Installer 내에서 **유니버설 Windows 플랫폼 개발** 워크로드를 설치합니다. **설치 세부 정보** > **유니버설 Windows 플랫폼 개발**에서 **C++(v14x) 유니버설 Windows 플랫폼 도구** 옵션을 확인합니다(아직 확인하지 않은 경우). 또한 Windows **설정** > **업데이트 \& 보안** > **개발자용**에서 **사이드로드 앱** 옵션 대신에 **개발자 모드** 옵션을 선택합니다.

Visual Studio 및 Windows SDK의 최신 버전을 사용하여 개발하는 것이 좋지만 10.0.17763.0(Windows 10 버전 1809) 이전에 Windows SDK와 함께 제공된 C++/WinRT 버전을 사용하는 경우, 위에서 언급한 Windows 네임스페이스 헤더를 사용하려면 10.0.17134.0(Windows 10 버전 1803)의 프로젝트에서 최소한의 Windows SDK 대상 버전이 필요합니다.

[Visual Studio Marketplace](https://marketplace.visualstudio.com/)에서 [C++/WinRT Visual Studio 확장(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)의 최신 버전을 다운로드하여 설치하려고 합니다.

- VSIX 확장에서 제공하는 Visual Studio의 C++/WinRT 프로젝트 및 항목 템플릿을 사용하여 C++/WinRT 개발을 시작할 수 있습니다.
- 또한 함께 제공되는 C++/WinRT 프로젝션된 형식의 Visual Studio 기본 디버그 시각화(natvis)를 통해 C# 디버깅과 유사한 환경이 구현됩니다. Natvis는 디버그 빌드에서 자동으로 실행됩니다. WINRT_NATVIS 기호를 정의하여 빌드를 릴리스할 수도 있습니다.

C++/WinRT에 대한 Visual Studio 프로젝트 템플릿은 아래 섹션에서 설명합니다. 최신 버전의 VSIX 확장이 설치된 C++/WinRT 프로젝트를 새로 만들면 이 프로젝트에서 자동으로 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)를 설치합니다. **Microsoft.Windows.CppWinRT** NuGet 패키지는 C++/WinRT 빌드 지원(MSBuild 속성 및 대상)을 제공하므로, VSIX 확장은 설치되지 않고 NuGet 패키지만 설치된 개발 머신과 빌드 에이전트 사이에 프로젝트를 이식할 수 있습니다.

또는 **Microsoft.Windows.CppWinRT** NuGet 패키지를 수동으로 설치하여 기존 프로젝트를 변환할 수 있습니다. 최신 버전의 VSIX 확장을 설치하거나 이 버전으로 업데이트한 후 Visual Studio에서 기존 프로젝트를 열고, **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 차례로 클릭하고, 검색 상자에서 **Microsoft.Windows.CppWinRT**를 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치**를 클릭하여 해당 프로젝트에 대한 패키지를 설치합니다. 패키지 추가를 마쳤으면 이제 `cppwinrt.exe` 도구 호출을 포함해 프로젝트에 대한 C++/WinRT MSBuild 지원을 가져옵니다.

> [!IMPORTANT]
> 1\.0.190128.4보다 이전 버전의 VSIX 확장을 사용하여 생성되거나 이 버전에서 작동하도록 업그레이드된 프로젝트가 있는 경우에는 [이전 버전의 VSIX 확장](#earlier-versions-of-the-vsix-extension)을 참조하세요. 이 섹션에는 최신 버전의 VSIX 확장을 사용하도록 업그레이드하기 위해 알고 있어야 하는 프로젝트 구성에 대한 중요한 정보가 포함됩니다.

- C++/WinRT는 C++17 표준의 기능을 사용하므로 NuGet 패키지는 Visual Studio에서 프로젝트 속성 **C/C++**  > **언어** > **C++ 언어 표준** > **ISO C++17 표준(/std:c++17)** 을 설정합니다.
- [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file) 컴파일러 옵션도 추가합니다.
- `co_await`를 사용하도록 설정하기 위해 [/await](/cpp/build/reference/await-enable-coroutine-support) 컴파일러 옵션을 추가합니다.
- C++/WinRT codegen을 내보내도록 XAML 컴파일러에 지시합니다.
- 또한 표준을 준수하도록 코드를 추가로 제한하는 **준수 모드: 예(/permissive-)** 를 설정하려고 합니다.
- 알고 있어야 할 또 다른 프로젝트 속성은 **C/C++**  > **일반** > **경고를 오류로 처리**입니다. 이 속성은 필요에 따라 **예(/WX)** 또는 **아니요(/WX-)** 로 설정하세요. 간혹 `cppwinrt.exe` 도구에서 생성된 소스 파일은 구현이 추가될 때까지 경고를 생성하는 경우가 있습니다.

위에서 설명한 대로 시스템을 설정하면 Visual Studio에서 C ++/WinRT 프로젝트를 만들고 빌드하거나 열고 배포할 수 있습니다.

버전 2.0의 경우 **Microsoft.Windows.CppWinRT** NuGet 패키지에는 `cppwinrt.exe` 도구가 포함되어 있습니다. C++/WinRT 코드에서의 사용을 위해 메타데이터에 설명된 API를 ‘프로젝션’하는 헤더 파일 기반 표준 C++ 라이브러리를 생성하기 위해 Windows 런타임 메타데이터(`.winmd`) 파일을 `cppwinrt.exe` 도구로 가리킬 수 있습니다.  Windows 런타임 메타데이터(`.winmd`) 파일을 사용하면 Windows 런타임 API 표면을 정규화하여 설명할 수 있습니다. 메타데이터에서 `cppwinrt.exe`를 가리켜 협력사 또는 타사 Windows 런타임 구성 요소에서 구현되거나 자체 애플리케이션에서 구현된 모든 런타임 클래스와 함께 사용할 라이브러리를 생성할 수 있습니다. 자세한 내용은 [C++/WinRT를 통한 API 사용](consume-apis.md)을 참조하세요.

C++/WinRT로 COM 스타일 프로그래밍을 이용하지 않고도 자체 표준 C++를 사용하여 런타임 클래스를 구현할 수도 있습니다. 런타임 클래스의 경우 IDL 파일에 형식만 설명하면 `midl.exe` 및 `cppwinrt.exe`가 상용구 원본 코드 파일 구현을 생성합니다. 또는 C++/WinRT 기본 클래스에서 파생하여 인터페이스를 구현할 수도 있습니다. 자세한 내용은 [C++/WinRT를 통한 API 작성](author-apis.md)을 참조하세요.

프로젝트 속성을 통해 설정된 `cppwinrt.exe` 도구에 대한 사용자 지정 옵션 목록은 Microsoft.Windows.CppWinRT NuGet 패키지 [추가 정보](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing)를 참조하세요.

프로젝트에 설치된 **Microsoft.Windows.CppWinRT** NuGet 패키지가 있는 경우 C++/WinRT MSBuild 지원을 사용하는 프로젝트를 식별할 수 있습니다.

VSIX 확장에서 제공되는 Visual Studio 프로젝트 템플릿은 다음과 같습니다.

### <a name="blank-app-cwinrt"></a>비어 있는 앱(C++/WinRT)
XAML 사용자 인터페이스가 있는 UWP(유니버설 Windows 플랫폼) 앱용 프로젝트 템플릿입니다.

Visual Studio는 각 XAML 태그 파일 뒤에 있는 IDL(Interface Definition Language)(`.idl`) 파일에서 구현과 헤더 스텁을 생성할 목적으로 XAML 컴파일러를 지원합니다. 먼저 IDL 파일에서 앱의 XAML 페이지에서 참조할 로컬 런타임 클래스를 모두 정의한 후 프로젝트를 한 번 빌드하여 구현 템플릿을 `Generated Files`에서 생성하고 스텁 형식 정의를 `Generated Files\sources`에서 생성합니다. 그런 다음, 이러한 스텁 형식 정의를 참조에 사용하여 로컬 런타임 클래스를 구현합니다. [런타임 클래스를 Midl 파일(.idl)로 팩터링](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)을 참조하세요.

C++/WinRT에 대한 Visual Studio 2019의 XAML 디자인 화면은 C#의 패리티에 가깝습니다. Visual Studio 2019에서 **속성** 창의 **이벤트** 탭을 사용하여 C++/WinRT 프로젝트에 이벤트 처리기를 추가할 수 있습니다. 수동으로 코드에 이벤트 처리기를 추가할 수도 있습니다. 자세한 내용은 [C++/WinRT에서 대리자를 사용하여 이벤트 처리](handle-events.md)를 참조하세요.

### <a name="core-app-cwinrt"></a>주요 앱(C++/WinRT)
XAML을 사용하지 않는 UWP(유니버설 Windows 플랫폼) 앱용 프로젝트 템플릿입니다.

대신 C++/WinRT Windows 네임스페이스 헤더를 Windows.ApplicationModel.Core 네임스페이스에 사용합니다. 빌드와 실행까지 마친 후 빈 공간을 클릭하여 색이 지정된 사각형을 추가한 다음, 색이 지정된 사각형을 클릭하여 끌어옵니다.

### <a name="windows-console-application-cwinrt"></a>Windows 콘솔 애플리케이션(C++/WinRT)
콘솔 사용자 인터페이스가 포함된 Windows 데스크톱의 C++/WinRT 클라이언트 애플리케이션용 프로젝트 템플릿입니다.

### <a name="windows-desktop-application-cwinrt"></a>Windows 데스크톱 애플리케이션(C++/WinRT)
Windows 데스크톱의 C++/WinRT 클라이언트 애플리케이션용 프로젝트 템플릿입니다. 여기에는 Win32 **MessageBox** 내의 Windows 런타임 [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri)가 표시됩니다.

### <a name="windows-runtime-component-cwinrt"></a>Windows 런타임 구성 요소(C++/WinRT)
일반적으로 UWP(유니버설 Windows 플랫폼)에서 사용되는 구성 요소용 프로젝트 템플릿입니다.

이 템플릿은 `midl.exe` > `cppwinrt.exe` 도구 체인을 나타냅니다. 이 경우 Windows 런타임 메타데이터(`.winmd`)가 IDL에서 생성된 다음, 구현과 헤더 스텁이 Windows 런타임 메타데이터에서 생성됩니다.

IDL 파일에서 구성 요소의 런타임 클래스와 기본 인터페이스, 그리고 그 밖에 구현되는 인터페이스를 정의합니다. 프로젝트를 1회 빌드하여 `module.g.cpp`, `module.h.cpp`, `Generated Files`의 구현 템플릿 및 `Generated Files\sources`의 스텁 형식 정의를 생성합니다. 그런 다음, 생성된 스텁 형식 정의를 참조에 사용하여 구성 요소의 런타임 클래스를 구현합니다. [런타임 클래스를 Midl 파일(.idl)로 팩터링](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)을 참조하세요.

빌드된 Windows 런타임 구성 요소 이진 파일 및 이진 파일의 `.winmd`와 함께 이 두 항목을 사용하는 UWP 앱을 번들로 제공합니다.

## <a name="earlier-versions-of-the-vsix-extension"></a>이전 버전의 VSIX 확장
[VSIX 확장](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)의 최신 버전을 설치하거나 이 버전으로 업데이트하는 것이 좋습니다. 기본적으로 이 확장은 자동으로 업데이트되도록 구성됩니다. 1\.0.190128.4보다 이전 버전의 VSIX 확장을 사용하여 만든 프로젝트가 있을 때 이 작업을 수행하는 경우, 이 섹션에서는 해당 프로젝트가 새 버전에서 작동하도록 업그레이드하는 방법에 대한 중요한 정보를 제공합니다. 업데이트하지 않는 경우에도 이 섹션의 정보가 유용하다는 것을 알게 됩니다.

지원되는 Windows SDK 및 Visual Studio 버전과 Visual Studio 구성의 측면에서 위의 [C++/WinRT, XAML, VSIX 확장 및 NuGet 패키지에 대한 Visual Studio 지원](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 섹션에 있는 정보는 이전 버전의 VSIX 확장에 적용됩니다. 아래 정보는 이전 버전을 사용하여 만들거나 이전 버전에서 작동하도록 업그레이드된 프로젝트의 동작 및 구성에 관한 중요한 차이점을 설명합니다.

### <a name="created-earlier-than-101810022"></a>1\.0.181002.2보다 이전에 생성됨
프로젝트가 1.0.181002.2보다 이전 버전의 VSIX 확장을 사용하여 생성된 경우에는 VSIX 확장 버전에 C++/WinRT 빌드 지원이 기본적으로 제공됩니다. 프로젝트의 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성이 `.vcxproj` 파일에 설정되어 있습니다.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

**Microsoft.Windows.CppWinRT** NuGet 패키지를 수동으로 설치하여 프로젝트를 업그레이드할 수 있습니다. 최신 버전의 VSIX 확장을 설치하거나 이 버전으로 업그레이드한 후 Visual Studio에서 프로젝트를 열고, **프로젝트**\>**NuGet 패키지 관리...** \> **찾아보기**를 차례로 클릭하고, 검색 상자에서 **Microsoft.Windows.CppWinRT**를 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치**를 클릭하여 프로젝트에 대한 패키지를 설치합니다.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>1\.0.181002.2-1.0.190128.3 버전으로 생성되거나 해당 버전으로 업그레이드됨
프로젝트가 1.0.181002.2-1.0.190128.3(포함) VSIX 확장 버전을 사용하여 생성된 경우 **Microsoft.Windows.CppWinRT** NuGet 패키지는 프로젝트 템플릿을 통해 자동으로 프로젝트에 설치되었습니다. 이 범위에 있는 VSIX 확장 버전을 사용하도록 이전 프로젝트를 업그레이드했을 수도 있습니다. 이 경우 이 범위의 VSIX 확장 버전에 빌드 지원이 계속 포함되므로 업그레이드된 프로젝트에는 **Microsoft.Windows.CppWinRT** NuGet 패키지가 설치되거나 설치되지 않았을 수 있습니다.

프로젝트를 업그레이드하려면 이전 섹션의 지침에 따라 프로젝트에 **Microsoft.Windows.CppWinRT** NuGet 패키지가 설치되어 있는지 확인합니다.

### <a name="invalid-upgrade-configurations"></a>잘못된 업그레이드 구성
최신 버전의 VSIX 확장을 사용하는 경우 프로젝트에 **Microsoft.Windows.CppWinRT** NuGet 패키지가 설치되어 있지 않으면 프로젝트에 `<CppWinRTEnabled>true</CppWinRTEnabled>` 속성이 포함될 수 없습니다. 이 구성을 사용하는 프로젝트에서 빌드 오류 메시지 “C++/WinRT VSIX에서 더 이상 프로젝트 빌드 지원을 제공하지 않습니다.  Microsoft.Windows.CppWinRT NuGet 패키지에 프로젝트 참조를 추가하세요.”를 생성합니다.

위에서 언급한 대로 C++/WinRT 프로젝트에는 NuGet 패키지가 설치되어 있어야 합니다.

이제 `<CppWinRTEnabled>` 요소는 사용되지 않으므로 선택적으로 `.vcxproj`를 편집하고 요소를 삭제할 수 있습니다. 반드시 필요한 것이 아니라 옵션입니다.

또한 `.vcxproj`에 `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`가 포함되어 있는 경우 C++/WinRT VSIX 확장을 설치하지 않고도 빌드할 수 있도록 제거할 수 있습니다.

## <a name="sdk-support-for-cwinrt"></a>C++/WinRT에 대한 SDK 지원
이 지원은 이제 호환성을 위해서만 제공되지만 버전 10.0.17134.0(Windows 10 버전 1803)을 기준으로 Windows SDK는 자사 Windows API(Windows 네임스페이스의 Windows 런타임 API)를 사용하기 위한 헤더 파일 기반 표준 C++ 라이브러리를 포함합니다. 해당 헤더는 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 폴더에 있습니다. Windows SDK 버전 10.0.17763.0(Windows 10 버전 1809)에서는 이 헤더가 프로젝트의 *$(GeneratedFilesDir)* 폴더에 생성됩니다.

호환성을 위해 Windows SDK도 `cppwinrt.exe` 도구와 함께 제공됩니다. 그러나 **Microsoft.Windows.CppWinRT** NuGet 패키지에 포함된 최신 버전의 `cppwinrt.exe`를 설치하고 사용하는 것이 좋습니다. 해당 패키지와 `cppwinrt.exe`는 위의 섹션에서 설명합니다.

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 프로젝션의 사용자 지정 형식
C++/WinRT 프로그래밍에서 일부 C++ 표준 라이브러리 데이터 형식을 포함한 표준 C++ 언어 기능과 [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)를 사용할 수 있습니다. 그 밖에도 프로젝션에서 일부 사용자 지정 데이터 형식에 대해서도 알아둘 필요가 있으며, 실제로 사용자 지정 데이터 형식을 사용할 수도 있습니다. 예를 들어 [C++/WinRT 시작](get-started.md)의 빠른 시작 코드 예제에서 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)을 사용합니다.

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array)는 임의 시점에서 사용할 가능성이 높은 또 하나의 형식입니다. 하지만 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 같은 형식은 직접 사용할 가능성이 비교적 낮습니다. 혹은 사용하지 않도록 선택할 수도 있는데, 이때는 C++ 표준 라이브러리에 해당하는 형식이 표시되더라도 어떤 코드도 변경할 필요가 없습니다.

> [!WARNING]
> 그 밖에 C++/WinRT Windows 네임스페이스 헤더에 대해서 자세히 공부할 경우 마주칠 수 있는 형식들도 있습니다. 예제로는 **winrt::param::hstring**이 있지만 컬렉션 예제도 있습니다. 이는 오로지 입력 매개 변수의 바인딩을 최적화하기 위해서만 존재하며 크게 성능을 향상하고 관련 표준 C++ 형식 및 컨테이너를 위해 “단지 작동”하는 호출 패턴을 최대한 활용합니다. 이 형식은 대부분의 값을 추가하는 경우에 프로젝션에 의해서만 사용됩니다. 우수하게 최적화되었으며 일반적인 용도로 사용되지 않습니다. 직접 사용하려고 하지 마세요. 이 형식은 구현 형식이며 변경되기 쉬우므로 `winrt::impl` 네임스페이스에서 어떤 것도 사용해서는 안 됩니다. 표준 형식이나 [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)의 형식을 계속 사용해야 합니다.
>
> [매개 변수를 ABI 경계로 전달](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi)도 참조하세요.

## <a name="important-apis"></a>중요 API
* [winrt::hstring 구조체](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 네임스페이스](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 확장(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)
* [C++/WinRT 시작](get-started.md)
* [표준 C++ 데이터 형식 및 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT의 문자열 처리](strings.md)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
