---
description: 이 토픽에서는 C++/WinRT 앱에 간단한 C# 구성 요소를 추가하는 과정을 안내합니다.
title: C++/WinRT 앱에서 사용할 C# Windows 런타임 구성 요소 작성
ms.date: 12/30/2020
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, C#
ms.localizationpriority: medium
ms.openlocfilehash: 6c6d766eb757241d6d3f38c425538c62fd057316
ms.sourcegitcommit: b0cedbb65d00e6241a9350c2ee1b1e89b090a2d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101086950"
---
# <a name="authoring-a-c-windows-runtime-component-for-use-from-a-cwinrt-app"></a>C++/WinRT 앱에서 사용할 C# Windows 런타임 구성 요소 작성

이 토픽에서는 C++/WinRT 프로젝트에 간단한 C# 구성 요소를 추가하는 과정을 안내합니다.

Visual Studio를 사용하면 간편하게 사용자 지정 Windows 런타임 형식을 작성하고 C# 또는 Visual Basic으로 작성된 WRC(Windows 런타임 구성 요소) 프로젝트 내에 배포한 다음, C++ 애플리케이션 프로젝트에서 해당 WRC를 참조하고 해당 애플리케이션에서 사용자 지정 형식을 사용할 수 있습니다.

내부적으로 Windows 런타임 형식은 UWP 애플리케이션에 허용되는 모든 .NET 기능을 사용할 수 있습니다.

> [!NOTE]
> 자세한 내용은 [C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 및 [UWP 앱용 .NET 개요](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)를 참조하세요.

외부적으로 형식의 멤버는 매개 변수 및 반환 값에 대한 형식만 공개할 수 있습니다. 솔루션을 빌드할 때 Visual Studio는 .NET WRC 프로젝트를 빌드한 다음, Windows 메타데이터(.winmd) 파일을 만드는 빌드 단계를 실행합니다. 이는 Visual Studio가 앱에 포함시키는 WRC(Windows 런타임 구성 요소)입니다.

> [!NOTE]
> .NET은 기본 데이터 형식이나 컬렉션 형식처럼 널리 사용되는 일부 .NET 형식을 해당 Windows 런타임 형식에 자동으로 매핑합니다. 이러한 .NET 형식은 Windows 런타임 구성 요소의 공용 인터페이스에 사용할 수 있으며, 구성 요소 사용자에게 해당 Windows 런타임 형식으로 표시됩니다. [C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건:

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-blank-app"></a>비어 있는 앱 만들기

Visual Studio에서 **비어 있는 앱(C++/WinRT)** 프로젝트 템플릿을 사용하여 새 프로젝트를 만듭니다. **(유니버설 Windows)** 템플릿이 아닌 **(C++/WinRT)** 템플릿을 사용하고 있는지 확인합니다.

폴더 구조가 연습과 일치하도록 새 프로젝트의 이름을 *CppToCSharpWinRT* 로 설정합니다.

## <a name="add-a-c-windows-runtime-component-to-the-solution"></a>솔루션에 C# Windows 런타임 구성 요소 추가

Visual Studio에서 다음과 같이 구성 요소 프로젝트를 만듭니다. 먼저 솔루션 탐색기에서 *CppToCSharpWinRT* 솔루션의 바로 가기 메뉴를 열고 **추가** 를 선택한 다음, **새 프로젝트** 를 선택하여 새 C# 프로젝트를 솔루션에 추가합니다. **새 프로젝트 추가** 대화 상자의 **설치된 템플릿** 섹션에서 **Visual C#** 을 선택하고 **Windows**, **유니버설** 을 차례로 선택합니다. **Windows 런타임 구성 요소(유니버설 Windows)** 템플릿을 선택하고 프로젝트 이름으로 **SampleComponent** 를 입력합니다. 

> [!NOTE]
> **새 유니버설 Windows 플랫폼 프로젝트** 대화 상자에서 최소 버전으로 **Windows 10 크리에이터스 업데이트(10.0, 빌드 15063)** 를 선택합니다. 자세한 내용은 아래의 [애플리케이션 최소 버전](#application-minimum-version) 섹션을 참조하세요.

## <a name="add-the-c-getmystring-method"></a>C# GetMyString 메서드 추가

*SampleComponent* 프로젝트에서 클래스 이름을 *Class1* 에서 *Example* 로 변경합니다. 그런 다음, 두 개의 간단한 멤버, 즉, 프라이빗 `int` 필드와 *GetMyString* 이라는 인스턴스 메서드를 이 클래스에 추가합니다.

> ```csharp
>     public sealed class Example
>     {
>         int MyNumber;
> 
>         public string GetMyString()
>         {
>             return $"This is call #: {++MyNumber}";
>         }
>     }
> ```

> [!NOTE]
> 기본적으로 이 클래스는 **public sealed** 로 표시됩니다. 구성 요소에서 공개하는 모든 Windows 런타임 클래스를 **봉인** 해야 합니다.

> [!NOTE]
> 선택 사항: 새로 추가된 멤버에 IntelliSense를 사용하도록 설정하려면 솔루션 탐색기에서 *SampleComponent* 프로젝트의 바로 가기 메뉴를 열고 **빌드** 를 선택합니다.

## <a name="reference-the-c-samplecomponent-from-the-cpptocsharpwinrt-project"></a>CppToCSharpWinRT 프로젝트에서 C# SampleComponent 참조

솔루션 탐색기의 C++/WinRT 프로젝트에서 **참조** 의 바로 가기 메뉴를 연 다음, **참조 추가** 를 선택하여 **참조 추가** 대화 상자를 엽니다. **프로젝트** 를 선택한 다음 **솔루션** 을 선택합니다. *SampleComponent* 프로젝트의 확인란을 선택하고 **확인** 을 선택하여 참조를 추가합니다.

> [!NOTE]
> 선택 사항: C++/WinRT 프로젝트에 IntelliSense를 사용하도록 설정하려면 솔루션 탐색기에서 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 열고 **빌드** 를 선택합니다.

## <a name="edit-mainpageh"></a>MainPage.h 편집

*CppToCSharpWinRT* 프로젝트에서 `MainPage.h`를 열고 두 항목을 추가합니다.  먼저 `#include` 문의 끝에 `#include "winrt/SampleComponent.h"`를 추가한 다음, `MainPage` 구조체에 `winrt::SampleComponent::Example` 필드를 추가합니다.

```cppwinrt
// MainPage.h
...
#include "winrt/SampleComponent.h"

namespace winrt::CppToCSharpWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        winrt::SampleComponent::Example myExample;
...
    };
}
```

> [!NOTE]
> Visual Studio에서 `MainPage.xaml` 아래에 `MainPage.h`가 나열됩니다.

## <a name="edit-mainpagecpp"></a>MainPage.cpp 편집

C# 메서드 `GetMyString`을 호출하도록 `MainPage.cpp`에서 `Mainpage::ClickHandler` 구현을 변경합니다.

```cppwinrt
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    //myButton().Content(box_value(L"Clicked"));

    hstring myString = myExample.GetMyString();

    myButton().Content(box_value(myString));
}
```
## <a name="run-the-project"></a>프로젝트 실행

이제 프로젝트를 빌드하고 실행할 수 있습니다. 단추를 클릭할 때마다 단추의 숫자가 증가합니다.

![C# 구성 요소를 호출하는 C++/WinRT Windows의 스크린샷](images/csharp-cpp-sample.png)

> [!TIP]
> Visual Studio에서 다음과 같이 구성 요소 프로젝트를 만듭니다. 먼저 솔루션 탐색기에서 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 열고 **속성** 을 선택한 다음, **구성 속성** 에서 **디버깅** 을 선택합니다. C#(관리) 및 C++(네이티브) 코드를 모두 디버그하려면 디버거 형식을 *관리 및 네이티브* 로 설정합니다.
> ![C++ 디버깅 속성](images/cpp-debugging-properties.png)

## <a name="application-minimum-version"></a>애플리케이션 최소 버전

C# 프로젝트의 [**애플리케이션 최소**](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) 버전은 애플리케이션을 컴파일하는 데 사용되는 .NET 버전을 제어합니다. 예를 들어 **Windows 10 Fall Creators Update(10.0, 빌드 16299)** 이상을 선택하면 .NET Standard 2.0 및 Windows ARM64 프로세서를 지원할 수 있습니다. 

> [!TIP]
> .NET Standard 2.0 또는 ARM64 지원이 필요 없는 경우에는 추가 빌드 구성을 방지할 수 있도록 16299보다 낮은 **애플리케이션 최소** 버전을 사용하는 것이 좋습니다.

## <a name="configure-for-windows-10-fall-creators-update-100-build-16299"></a>Windows 10 Fall Creators Update(10.0, 빌드 16299) 구성

C++/WinRT 프로젝트에서 참조하는 C# 프로젝트에서 .NET Standard 2.0 또는 Windows ARM64를 지원하려면 다음 단계를 수행합니다. 

Visual Studio에서 솔루션 탐색기로 이동하여 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 엽니다.  **속성** 을 선택하고 유니버설 Windows 앱 최소 버전을 **Windows 10 Fall Creators Update(10.0, 빌드 16299)** 이상으로 설정합니다. *SampleComponent* 프로젝트에 대해서도 동일한 작업을 수행합니다.

Visual Studio에서 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 열고 **프로젝트 언로드** 를 선택하여 텍스트 편집기에서 `CppToCSharpWinRT.vcxproj`를 엽니다. 

다음 XML을 복사하여 `CPPWinRTCSharpV2.vcxproj`의 첫 번째 `PropertyGroup`에 붙여넣습니다. 

```xml
   <!-- Start Custom .NET Native properties -->
   <DotNetNativeVersion>2.2.9-rel-29512-01</DotNetNativeVersion>
   <DotNetNativeSharedLibary>2.2.8-rel-29512-01</DotNetNativeSharedLibary>
   <UWPCoreRuntimeSdkVersion>2.2.11</UWPCoreRuntimeSdkVersion>
   <!--<NugetPath>$(USERPROFILE)\.nuget\packages</NugetPath>-->
   <NugetPath>$(ProgramFiles)\Microsoft SDKs\UWPNuGetPackages</NugetPath>
   <!-- End Custom .NET Native properties -->
```

`DotNetNativeVersion`, `DotNetNativeSharedLibary` 및 `UWPCoreRuntimeSdkVersion`의 값은 Visual Studio 버전에 따라 다를 수 있습니다.  올바른 값으로 설정하려면 `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages`를 열고 아래 표에 나와 있는 각 값의 하위 디렉터리를 확인합니다.  예를 들어 `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` 디렉터리는 그 아래에 `2.2.9-rel-29512-01`이라는 디렉터리가 있습니다.

> | MSBuild 변수 | 디렉터리 | 예제
> |-| - | -
> | DotNetNativeVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` | `2.2.9-rel-29512-01`
> | DotNetNativeSharedLibary | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.SharedLibrary` | `2.2.8-rel-29512-01`
> | UWPCoreRuntimeSdkVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.UWPCoreRuntimeSdk` | `2.2.11`

다음으로 첫 번째 `PropertyGroup` 바로 뒤에 다음 내용을 변경 없이 그대로 추가합니다.

```xml
  <!-- Start Custom .NET Native targets -->
  <!-- Import all of the .NET Native / CoreCLR props at the beginning of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.props" />
  <!-- End Custom .NET Native targets -->
```

프로젝트 파일의 끝부분에서 닫는 `Project` 태그 바로 앞에 다음 내용을 변경 없이 그대로 추가합니다.

```xml
  <!-- Import all of the .NET Native / CoreCLR targets at the end of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.targets" />
  <!-- End Custom .NET Native targets -->
```

Visual Studio에서 프로젝트 파일을 다시 로드합니다. 다시 로드하려면 Visual Studio 솔루션 탐색기에서 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 열고 **프로젝트 다시 로드** 를 선택합니다.

## <a name="building-for-net-native"></a>.NET 네이티브용 빌드

.NET 네이티브를 대상으로 빌드된 C# 구성 요소를 사용하여 애플리케이션을 빌드하고 테스트하는 것이 좋습니다. Visual Studio에서 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 열고 **프로젝트 언로드** 를 선택하여 텍스트 편집기에서 `CppToCSharpWinRT.vcxproj`를 엽니다. 

그런 다음, C++ 프로젝트 파일의 릴리스 및 ARM64 구성에서 `UseDotNetNativeToolchain` 속성을 `true`로 설정합니다.

Visual Studio 솔루션 탐색기에서 *CppToCSharpWinRT* 프로젝트의 바로 가기 메뉴를 열고 **프로젝트 다시 로드** 를 선택합니다. 

```xml
  <PropertyGroup Condition="'$(Configuration)'=='Release'" Label="Configuration">
...
    <UseDotNetNativeToolchain>true</UseDotNetNativeToolchain>
  </PropertyGroup>
  <PropertyGroup Condition="$(Platform)'='ARM64'" Label="Configuration">
    <UseDotNetNativeToolchain Condition="'$(UseDotNetNativeToolchain)'==''">true</UseDotNetNativeToolchain>
  </PropertyGroup>
```

## <a name="related-topics"></a>관련 항목
* [C# 및 Visual Basic이 포함된 Windows 런타임 구성 요소](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [C++/WinRT를 사용한 Windows 런타임 구성 요소](/windows-apps-src/winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)