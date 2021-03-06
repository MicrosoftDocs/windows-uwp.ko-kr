---
description: C#/WinRT는 C# 코드에 대한 WinRT 프로젝션 지원을 제공하는 도구 세트입니다.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, UWP, 표준, C#, WinRT, cswinrt, 프로젝션
ms.localizationpriority: medium
ms.openlocfilehash: 0704a7e9c731c6f60c59615b964b51e0ded242c2
ms.sourcegitcommit: 1022e8819e75484ca0cd94f8baf4f4d11900e0e3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98206092"
---
# <a name="cwinrt"></a>C#/WinRT

C#/WinRT는 C# 언어에 대한 WinRT(Windows 런타임) 프로젝션 지원을 제공하는 NuGet 패키지 도구 키트입니다. *프로젝션* 은 대상 언어에 대해 자연스럽고 친숙한 방식으로 WinRT API를 프로그래밍할 수 있는 변환 계층(예: interop 어셈블리)입니다. 예를 들어 C#/WinRT 프로젝션은 C#과 WinRT 인터페이스 간의 interop 세부 정보를 숨기고, 해당 .NET(예: 문자열, URI, 공통 값 형식 및 제네릭 컬렉션)에 대한 많은 WinRT 형식의 매핑을 제공합니다.

C#/WinRT는 현재 WinRT 형식을 사용할 수 있도록 지원하며, 최신 버전을 통해 WinRT Interop 어셈블리를 [만들고](#create-an-interop-assembly) [참조](#reference-an-interop-assembly)할 수 있습니다. C#/WinRT의 이후 릴리스에서는 C#에서 WinRT 형식을 작성하기 위한 지원이 추가됩니다.

C#/WinRT에 대한 자세한 내용은 [C#/WinRT GitHub 리포지토리](https://aka.ms/cswinrt/repo)를 참조하세요.

## <a name="motivation-for-cwinrt"></a>C#/WinRT에 대한 동기 부여

[.NET Core](/dotnet/core/)는 .NET 플랫폼의 중심이고 .NET 5는 최신 주요 릴리스입니다. 디바이스, 클라우드 및 IoT 애플리케이션을 구축하는 데 사용할 수 있는 오픈 소스 플랫폼 간 런타임입니다.

이전 버전의 .NET Framework 및 .NET Core에는 Windows 고유의 기술인 WinRT에 대한 지식이 기본적으로 제공되었습니다. .NET 5의 이식성 및 효율성 목표를 지원하기 위해 .NET 컴파일러 및 런타임에서 WinRT 프로젝션 지원을 제거하고 C#/WinRT 도구 키트로 이동했습니다. C#/WinRT의 목표는 이전 버전의 C# 컴파일러 및 .NET 런타임에서 제공하는 기본 제공 WinRT 지원을 패리티에 제공하는 것입니다. 자세한 내용은 [Windows 런타임 형식의 .NET 매핑](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)을 참조하세요.

C#/WinRT는 WinUI 3.0도 지원합니다. 이 릴리스의 WinUI는 운영 체제에서 네이티브 Microsoft UI 컨트롤 및 기능을 제거합니다. 이를 통해 앱 개발자는 Windows 10 버전 1803 이상 릴리스에서 최신 컨트롤과 시각적 개체를 사용할 수 있습니다.

마지막으로 C#/WinRT는 일반적인 도구 키트이며, C# 컴파일러 또는 .NET 런타임에서 WinRT에 대한 기본 제공 지원을 사용할 수 없는 다른 시나리오를 지원하기 위한 것입니다.

## <a name="create-an-interop-assembly"></a>interop 어셈블리 만들기

WinRT API는 Windows 메타데이터(*.winmd) 파일에 정의되어 있습니다. C#/WinRT NuGet 패키지([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/))에는 Windows 메타데이터 파일을 처리하고 .NET 5.0 C# 코드를 생성하는 데 사용할 수 있는 C#/WinRT 컴파일러인 **cswinrt.exe** 가 포함되어 있습니다. [C++/WinRT](../cpp-and-winrt-apis/index.md)에서 C++ 언어 프로젝션에 대한 헤더를 생성하는 방법과 비슷하게 이러한 원본 파일을 interop 어셈블리로 컴파일할 수 있습니다. 그런 다음, C#/WinRT 런타임 어셈블리와 함께 애플리케이션에서 참조할 C#/WinRT interop 어셈블리를 배포할 수 있습니다.

Interop 어셈블리를 NuGet 패키지로 만들어 배포하는 방법을 보여주는 연습은 [연습: C++/WinRT 구성 요소에서 .NET 5 프로젝션 생성 및 NuGet 업데이트](net-projection-from-cppwinrt-component.md)를 참조하세요.

### <a name="invoke-cswinrtexe"></a>cswinrt.exe 호출

프로젝트에서 cswinrt.exe를 호출하려면 최신 [C#/WinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)를 설치합니다. 그런 다음, **C# 클래스 라이브러리(.NET Core)** 프로젝트에서 C#/WinRT 관련 프로젝트 속성을 설정하여 interop 어셈블리를 생성할 수 있습니다. 다음 프로젝트 조각에서는 **cswinrt** 를 간단히 호출하여 Contoso 네임스페이스의 형식에 대한 프로젝션 원본을 생성하는 방법을 보여 줍니다. 그런 다음, 이러한 원본이 프로젝트 빌드에 포함됩니다.

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

또한 이 프로젝트에서는 NuGet 패키지, 프로젝트 참조 또는 직접 참조를 통해 프로젝션하려는 CsWinRT NuGet 패키지 및 프로젝트별 .winmd 파일도 참조해야 합니다. 기본적으로 **Windows** 및 **Microsoft** 네임스페이스는 프로젝션되지 않습니다. CsWinRT 프로젝트 속성의 전체 목록은 [CsWinRT NuGet 설명서](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)를 참조하세요.

### <a name="distribute-the-interop-assembly"></a>interop 어셈블리 배포

Interop 어셈블리는 일반적으로 필요한 C#/WinRT 런타임 어셈블리인 **WinRT.Runtime.dll** 의 C#/WinRT NuGet 패키지에 대한 종속성과 함께 NuGet 패키지로 배포됩니다.

.NET 5.0 애플리케이션에 대해 올바른 버전의 C#/WinRT 런타임이 배포되었는지 확인하려면 C#/WinRT NuGet 패키지에 대한 종속성과 함께 .nuspec 파일에 `targetFramework` 조건을 포함합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> .NET 5.0에 대한 대상 프레임워크 모니커가 ".NETCoreApp5.0"에서 "net5.0"으로 이동합니다.

## <a name="reference-an-interop-assembly"></a>interop 어셈블리 참조

일반적으로 C#/WinRT interop 어셈블리는 애플리케이션 프로젝트에서 참조됩니다. 그러나 중간 interop 어셈블리에서 차례로 참조될 수도 있습니다. 예를 들어 WinUI interop 어셈블리는 Windows SDK interop 어셈블리를 참조합니다.

프로젝션된 C#/WinRT 형식을 사용하려면 적절한 C#/WinRT interop NuGet 패키지에 대한 참조를 프로젝트에 추가합니다. 이렇게 하면 interop 어셈블리와 C#/WinRT 런타임 어셈블리가 모두 프로젝트에 추가됩니다.

공식적인 interop 어셈블리를 사용하지 않고 타사 WinRT 구성 요소를 배포하는 경우 애플리케이션 프로젝트에서 [interop 어셈블리를 만드는 절차](#create-an-interop-assembly)에 따라 자체의 프라이빗 프로젝션 원본을 생성할 수 있습니다. 이 방법은 프로세스 내에서 동일한 형식의 충돌하는 프로젝션을 생성할 수 있으므로 사용하지 않는 것이 좋습니다. [의미 체계 버전 관리](https://semver.org) 체계를 따르는 NuGet 패키징은 이를 방지하도록 설계되었습니다. 공식적인 타사 interop 어셈블리가 선호됩니다.

### <a name="winrt-type-activation"></a>WinRT 형식 활성화

C#/WinRT는 [Win2D](https://www.nuget.org/packages/Win2D.uwp/)와 같은 타사 구성 요소뿐만 아니라 운영 체제에서 호스팅하는 WinRT 형식도 활성화할 수 있도록 지원합니다. 데스크톱 애플리케이션의 타사 구성 요소 활성화에 대한 지원은 Windows 10 버전 1903 이상에서 사용할 수 있는 [등록 무료 WinRT 활성화](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/)를 통해 사용하도록 설정됩니다. 구성 요소가 UWP 앱을 대상으로 하도록 빌드된 경우 [VCRT 전달자](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) 패키지를 사용해야 할 수도 있습니다.

C#/WinRT는 위에서 설명한 대로 Windows에서 형식을 활성화하지 못하는 경우에도 활성화 대체 경로를 제공합니다. 이 경우 C#/WinRT는 정규화된 형식 이름에 따라 네이티브 구현 DLL을 찾아서 요소를 점진적으로 제거하려고 시도합니다. 예를 들어 대체 논리는 다음 모듈에서 Contoso.Controls.Widget 형식을 순서대로 활성화하려고 시도합니다.

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT는 [LoadLibrary 대체 검색 순서](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications)를 사용하여 구현 DLL을 찾습니다. 이 대체 동작을 사용하는 앱은 구현 DLL을 앱 모듈과 함께 패키지해야 합니다.

## <a name="common-errors-and-troubleshooting"></a>일반 오류 및 문제 해결

- 오류: "Windows 메타데이터가 제공되거나 검색되지 않았습니다."

  `<CsWinRTWindowsMetadata>` 프로젝트 속성을 사용하여 Windows 메타데이터를 지정할 수 있습니다. 예를 들면 다음과 같습니다.
  ```xml
  <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
  ```
  
- 오류 CS0246: 'Windows' 유형 또는 네임스페이스 이름을 찾을 수 없습니다. using 지시문 또는 어셈블리 참조가 누락되었습니까?와 같은 오류가 표시되는 경우

  이 오류를 해결하려면 특정 Windows 버전을 대상으로 하는 `<TargetFramework>` 속성을 편집합니다. 예를 들면 다음과 같습니다.
  ```xml
  <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
  ```
  `<TargetFramework>` 속성을 지정하는 방법에 대한 자세한 내용은 [Windows 런타임 API 호출](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)에 대한 문서를 참조하세요.


### <a name="net-sdk-versioning-errors"></a>.NET SDK 버전 관리 오류

해당 종속성보다 이전의 .NET SDK 버전으로 빌드된 프로젝트에서 다음 오류 또는 경고가 발생할 수 있습니다.

| 오류 또는 경고 메시지 | 이유 |
|--------------------------|--------|
| 경고 MSB3277: 해결할 수 없는 WinRT.Runtime 또는 Microsoft.Windows.SDK.NET의 서로 다른 버전 간의 충돌이 발견되었습니다. | 이 빌드 경고는 API 표면에서 Windows SDK 유형을 노출하는 라이브러리를 참조할 때 발생합니다. |
| [오류 CS1705](/dotnet/csharp/language-reference/compiler-messages/cs1705): 어셈블리 'AssemblyName1'은 참조된 어셈블리 'AssemblyName2'보다 높은 버전의 'TypeName'을 사용합니다. | 이 빌드 컴파일러 오류는 라이브러리에서 공개된 Windows SDK 유형을 참조하고 사용할 때 발생합니다. |
| System.IO.FileLoadException | 이 런타임 오류는 Windows SDK 유형을 표시하지 않는 라이브러리에서 특정 API를 호출할 때 발생할 수 있습니다. |

이러한 오류를 해결하려면 .NET SDK를 최신 버전으로 업데이트합니다. 이렇게 하면 애플리케이션에서 사용하는 런타임 및 Windows SDK 어셈블리 버전이 모든 종속성과 호환됩니다. 이러한 오류는 .NET 5 SDK에 대한 조기 서비스/기능 업데이트 시 발생할 수 있습니다. 런타임 수정 시 어셈블리 버전을 업데이트해야 할 수 있기 때문입니다.

## <a name="known-issues"></a>알려진 문제

알려진 문제 및 주요 변경 내용은 [C#/WinRT GitHub 리포지토리](https://aka.ms/cswinrt/repo)에 설명되어 있습니다.

C#/WinRT NuGet 패키지, cswinrt.exe 컴파일러 또는 생성된 프로젝션 원본에서 기능 문제가 발생하는 경우 [C#/WinRT 문제 페이지](https://github.com/microsoft/CsWinRT/issues)를 통해 문제를 제출하세요.

## <a name="additional-resources"></a>추가 리소스

* [C#/WinRT GitHub 리포지토리](https://aka.ms/cswinrt/repo)
