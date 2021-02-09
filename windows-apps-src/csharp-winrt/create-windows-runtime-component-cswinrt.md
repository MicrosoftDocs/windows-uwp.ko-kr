---
description: 'C #/Winrt를 사용 하 여 Windows 런타임 구성 요소를 작성 하 고 네이티브 응용 프로그램에서 사용'
title: C#/WinRT 구성 요소를 만들고 C++/WinRT에서 사용
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f5157f97163a72ccce1ce9fc3f560fb4e16b1df
ms.sourcegitcommit: 61a874d00991f7ca06466a99a557ef0777bd0f7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989640"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>연습: c #/Winrt 구성 요소를 만들고 c + +/WinRT에서 사용

> [!NOTE]
> 이 문서에서 설명 하는 c #/Winrt 제작 지원은 현재 c #/Winrt 버전 1.1.2 210208.6입니다. 이 릴리스는 초기 피드백 및 평가용 으로만 사용 됩니다.

C #/Winrt를 사용 하면 .NET 5 개발자가 클래스 라이브러리 프로젝트를 사용 하 여 c #에서 자체 Windows 런타임 구성 요소를 작성할 수 있습니다. 작성 된 구성 요소는 기본 데스크톱 응용 프로그램에서 패키지 참조로 사용 하거나 몇 가지 수정 사항이 있는 프로젝트 참조로 사용할 수 있습니다.

이 연습에서는 c #/Winrt를 사용 하 여 간단한 Windows 런타임 구성 요소를 만들고, 구성 요소를 NuGet 패키지로 배포 하 고, c + +/WinRT 콘솔 응용 프로그램에서 구성 요소를 사용 하는 방법을 보여 줍니다. 이 연습에 대 한 샘플 코드 [는 Github에서 찾을 수 있습니다.](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)

런타임 구성 요소를 작성 하는 동안 [이 문서](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 에 설명 된 지침 및 형식 제한 사항을 따릅니다. 내부적으로 구성 요소의 Windows 런타임 형식은 UWP 앱에서 허용 되는 모든 .NET 기능을 사용할 수 있습니다. 자세한 내용은 [UWP 앱 용 .net](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)을 참조 하세요. 외부적으로 형식의 멤버는 매개 변수 및 반환 값에 대 한 Windows 런타임 형식만 노출할 수 있습니다.

> [!NOTE]
> [.Net 형식에 매핑되](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)는 몇 가지 Windows 런타임 형식이 있습니다. 이러한 .NET 형식은 Windows 런타임 구성 요소의 공용 인터페이스에서 사용 될 수 있으며 구성 요소의 사용자에 게 해당 하는 Windows 런타임 형식으로 표시 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

이 연습을 수행 하려면 다음 도구 및 구성 요소가 필요 합니다.

- Visual Studio 2019
- .NET 5.0 SDK
- C + +/winrt [VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 프로젝트 템플릿

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>C #/Winrt를 사용 하 여 간단한 Windows 런타임 구성 요소 만들기

먼저 Visual Studio 2019에서 새 프로젝트를 만듭니다. **클래스 라이브러리 (.Net Core)** 프로젝트 템플릿을 선택 하 고 이름을 **AuthoringDemo** 로 선택 합니다. 프로젝트를 다음과 같이 추가 하 고 수정 해야 합니다.

1. `TargetFramework` **AuthoringDemo** 파일에서를 업데이트 하 고 다음 요소를에 추가 합니다 `PropertyGroup` .

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

    요소에 대해 `TargetFramework` 다음 [대상 프레임 워크 모니커](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)중 하나를 사용할 수 있습니다.
    - **net 5.0-windows 10.0.17763.0**
    - **net 5.0-windows 10.0.18362.0**
    - **net 5.0-windows 10.0.19041.0**

2. 최신 버전의 [c #/Winrt NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/1.1.2-prerelease.210208.6)를 설치 합니다.

    a. 솔루션 탐색기에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리** 를 선택 합니다.

    b. **Microsoft. CsWinRT** NuGet 패키지를 검색 하 고 최신 버전을 설치 합니다. 이 연습에서는 c #/Winrt 버전 1.1.2.210208.6를 사용 합니다.

3. `PropertyGroup`여러 c #/Winrt 속성을 설정 하는 새 요소를 추가 합니다.

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
    </PropertyGroup>
      ```

      이 예제에서 속성에 대 한 자세한 내용은 다음과 같습니다. C #/Winrt 프로젝트 속성의 전체 목록은 [c #/Winrt NuGet 설명서를 참조 하세요.](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - `CsWinRTComponent`속성은 프로젝트가 Windows 런타임 구성 요소 임을 지정 하 여 해당 구성 요소에 대 한 WinMD 파일이 생성 되도록 지정 합니다.
    - `CsWinRTWindowsMetadata`속성은 Windows 메타 데이터에 대 한 소스를 제공 합니다. 버전 1.1.1에 필요 합니다.

4. 라이브러리 **(.cs)** 클래스 파일을 사용 하 여 런타임 클래스를 제작할 수 있습니다. **Class1.cs** 파일을 마우스 오른쪽 단추로 클릭 하 고 이름을 **Example.cs** 로 바꿉니다. 다음 코드를이 파일에 추가 합니다 .이 파일은 public 속성과 메서드를 런타임 클래스에 추가 합니다. 런타임 구성 요소 **공용** 에 노출할 모든 클래스를 표시 해야 합니다.

    ```csharp
    namespace AuthoringDemo
    {
        public sealed class Example
        {
            public int SampleProperty { get; set; }

            public static string SayHello()
            {
                return "Hello from your C# WinRT component";
            }
        }
    }
    ```

5. 이제 프로젝트를 빌드하여 구성 요소에 대 한 메타 데이터 파일을 생성할 수 있습니다. **솔루션 탐색기** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **빌드** 를 클릭 합니다. 생성 된 **파일** 폴더의 **솔루션 탐색기** 및 빌드 출력 폴더에 생성 된 **AuthoringDemo** 파일이 표시 됩니다.

## <a name="generate-a-nuget-package-for-the-component"></a>구성 요소에 대 한 NuGet 패키지를 생성 합니다.

그런 다음 구성 요소에 대 한 NuGet 패키지를 생성 합니다. 패키지를 생성 하는 경우 c #/Winrt는 네이티브 응용 프로그램에서 사용할 수 있도록 패키지의 구성 요소와 호스트 어셈블리를 구성 합니다.

NuGet 패키지를 생성 하는 방법에는 여러 가지가 있습니다.

* 프로젝트를 빌드할 때마다 NuGet 패키지를 생성 하려면 **AuthoringDemo** 프로젝트 파일에 다음 속성을 추가 하 고 프로젝트를 다시 빌드합니다.

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>
    ```

* 또는 **솔루션 탐색기** 에서 **AuthoringDemo** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **팩** 을 선택 하 여 NuGet 패키지를 생성할 수 있습니다.

패키지를 빌드할 때 **빌드** 창에는 NuGet 패키지가 `AuthoringDemo.1.0.0.nupkg` 성공적으로 만들어졌는지 표시 되어야 합니다.

## <a name="consume-the-component-in-cwinrt"></a>C + +/WinRT에서 구성 요소 사용

C #/WinRT 작성 된 Windows 런타임 구성 요소는 몇 가지 수정으로 네이티브 응용 프로그램에서 사용 될 수 있습니다. 다음 단계에서는 네이티브 콘솔 응용 프로그램에서 위의 작성 된 구성 요소를 호출 하는 방법을 보여 줍니다. 

1. 새 **c + +/Winrt 콘솔 응용 프로그램** 프로젝트를 솔루션에 추가 합니다. 이 프로젝트는 다른 솔루션에 포함 될 수도 있습니다 (선택 하는 경우).

    a. **솔루션 탐색기** 에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고  ->  **새 프로젝트** 추가를 클릭 합니다.

    b. **새 프로젝트 추가 대화 상자** 에서 **c + +/Winrt 콘솔 응용 프로그램** 프로젝트 템플릿을 검색 합니다. 템플릿을 선택 하 고 **다음** 을 클릭 합니다.

    다. 새 프로젝트의 이름을 **CppConsoleApp** 하 고 **만들기** 를 클릭 합니다.

2. NuGet 패키지 또는 프로젝트 참조로 AuthoringDemo 구성 요소에 대 한 참조를 추가 합니다.

    - **옵션 1 (패키지 참조)**:  

        a. **CppConsoleApp** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리** 를 선택 합니다. AuthoringDemo NuGet 패키지에 대 한 참조를 추가 하도록 패키지 원본을 구성 해야 할 수 있습니다. 이렇게 하려면 NuGet 패키지 관리자에서 **설정** 아이콘을 클릭 하 고 패키지 원본을 적절 한 경로에 추가 합니다.

        ![NuGet 설정](images/nuget-sources-settings.png)

        b. 패키지 원본을 구성한 후 **AuthoringDemo** 패키지를 검색 하 고 **설치** 를 클릭 합니다.

        ![NuGet 패키지 설치](images/install-authoring-nuget.png)

    - **옵션 2 (프로젝트 참조)**:
        
        a. **CppConsoleApp** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 참조 **추가** 를 선택  ->  합니다. **프로젝트** 노드 아래에서 **AuthoringDemo** 프로젝트에 대 한 참조를 추가 합니다. 이 미리 보기를 통해 **찾아보기** 노드에서 **AuthoringDemo** 에 대 한 파일 참조를 추가 해야 합니다. 생성 된 winmd 파일은 **AuthoringDemo** 프로젝트의 출력 디렉터리에서 찾을 수 있습니다.

        b. 이 미리 보기의 경우에는 **CppConsoleApp** 에 다음 속성 그룹을 추가 해야 합니다. 네이티브 응용 프로그램 프로젝트 파일을 편집 하려면 먼저 **CppConsoleApp** 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드** 를 선택 합니다.

        ```xml
        <PropertyGroup>
            <TargetFrameworkVersion>net5.0</TargetFrameworkVersion>
            <TargetFramework>native</TargetFramework>
            <TargetRuntime>Native</TargetRuntime>
        </PropertyGroup>
        ```

3. 구성 요소를 호스트 하는 것을 지원 하려면 파일 및 매니페스트 파일에 runtimeconfig.js를 추가 해야 합니다. 관리 되는 구성 요소 호스팅에 대 한 자세한 내용은 [이러한 호스팅 문서](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)를 참조 하세요.

    a. 파일에 runtimeconfig.js를 추가 하려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가-> 새 항목** 을 선택 합니다. **텍스트 파일** 템플릿을 검색 하 고WinRT.Host.runtimeconfig.js이름을로 **설정** 합니다. 다음 내용을 붙여넣습니다.

    ```json
    {
        "runtimeOptions": {
            "tfm": "net5.0",
            "rollForward": "LatestMinor",
            "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "5.0.0"
            }
        }
    }
    ```

    항목의 경우 `tfm` DOTNET_ROOT 환경 변수를 사용 하 여 사용자 지정 자체 포함 .net 5 설치를 참조할 수 있습니다.

    b. 매니페스트 파일을 추가 하려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가-> 새 항목** 을 선택 합니다. **텍스트 파일** 템플릿을 검색 하 고 **CppConsoleApp.exe 이름을 .manifest** 로 합니다. 다음 내용을 붙여넣습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
        <assemblyIdentity version="1.0.0.0" name="CppConsoleApp"/>
        <file name="WinRT.Host.dll">
            <activatableClass
                name="AuthoringDemo.Example"
                threadingModel="both"
                xmlns="urn:schemas-microsoft-com:winrt.v1" />
        </file>
    </assembly>
    ```

    패키지 되지 않은 응용 프로그램에는 매니페스트 파일이 필요 합니다. 이 파일에서 위에 표시 된 대로 활성화 가능한 클래스 등록 항목을 사용 하 여 런타임 클래스를 지정 합니다.

4. 프로젝트를 배포할 때 출력에 runtimeconfig.js및 매니페스트 파일을 포함 하도록 프로젝트를 수정 합니다. **WinRT.Host.runtimeconfig.js** 및 **CppConsoleApp.exe .manifest** 파일 모두에 대해 **솔루션 탐색기** 에서 파일을 클릭 하 고 **Content** 속성을 **True** 로 설정 합니다. 예를 들면 다음과 같습니다.

    ![콘텐츠 배포](images/deploy-content.png)

5. 프로젝트의 헤더 파일 아래에서 **.pch** 를 열고 구성 요소를 포함 하는 다음 코드 줄을 추가 합니다.

    ```cpp
    #include <winrt/AuthoringDemo.h>
    ```

6. 프로젝트의 소스 파일 아래에서 **main .cpp** 를 열고 다음 내용으로 바꿉니다.

    ```cpp
    #include "pch.h"
    #include "iostream"

    using namespace winrt;
    using namespace Windows::Foundation;

    int main()
    {
        init_apartment();

        AuthoringDemo::Example ex;
        ex.SampleProperty(42);
        std::wcout << ex.SampleProperty() << std::endl;
        std::wcout << ex.SayHello().c_str() << std::endl;
    }
    ```

7. **CppConsoleApp** 프로젝트를 빌드하고 실행 합니다. 이제 아래 출력을 볼 수 있습니다.

    ![C + +/WinRT 콘솔 출력](images/consume-component-output.png)

## <a name="related-topics"></a>관련 항목

- [예제 코드](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)
- [구성 요소 작성](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [관리 되는 구성 요소 호스팅](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
