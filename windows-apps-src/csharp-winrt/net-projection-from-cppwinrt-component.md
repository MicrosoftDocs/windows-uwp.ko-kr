---
description: '이 연습에서는 c #/Winrt를 사용 하 여 c + +/Vb 구성 요소에 대 한 .NET 5 프로젝션을 생성 하는 방법을 보여 줍니다.'
title: C + +/WinRT 구성 요소에서 .NET 5 프로젝션을 생성 하 고 NuGet을 배포 하는 연습
ms.date: 11/12/2020
ms.topic: article
keywords: 'windows 10, c #, winrt, cswinrt, 프로젝션'
ms.localizationpriority: medium
ms.openlocfilehash: 57bc5c49d47dacee910cd3d80964f797633ef587
ms.sourcegitcommit: 6da85cc75c02a5a7417966abddc8824ac87fb619
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964736"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>연습: C++/WinRT 구성 요소에서 .NET 5 프로젝션 생성 및 NuGet 배포

이 연습에서는 [c #/winrt](index.md) 를 사용 하 여 c + +/winrt 구성 요소에 대 한 .net 5 프로젝션을 생성 하 고, 연결 된 NuGet 패키지를 만들고, .Net 5 c # 콘솔 응용 프로그램에서 NuGet 패키지를 참조 하는 방법을 보여 줍니다.

[여기](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)에서이 연습에 대 한 전체 샘플을 GitHub에서 다운로드할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 연습 및 해당 샘플에는 다음과 같은 도구 및 구성 요소가 필요 합니다.

- 유니버설 Windows 플랫폼 개발 워크 로드가 설치 된 [Visual Studio 16.8](https://visualstudio.microsoft.com/downloads/) 이상 유니버설 Windows 플랫폼 개발에 **대 한 설치 세부 정보** 에서  >   **c + + (v14x) 유니버설 Windows 플랫폼 도구** 옵션을 확인 합니다.
- [.Net 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0).
- C + +/winrt 프로젝트 템플릿에 대 한 [c + +/WINRT VSIX 확장](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) .

## <a name="create-a-simple-cwinrt-runtime-component"></a>간단한 c + +/WinRT 런타임 구성 요소 만들기

이 연습을 수행 하려면 먼저 .NET 5 프로젝션을 만들 c + +/WinRT 구성 요소가 있어야 합니다. 이 연습 [에서는 GitHub의](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample/SimpleMathComponent)관련 샘플에서 **SimpleMathComponent** 프로젝트를 사용 합니다. 이는 [c + +/WINRT VSIX 확장](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 사용 하 여 만든 **Windows 런타임 구성 요소 (c + +/winrt)** 프로젝트입니다. 개발 컴퓨터에 프로젝트를 복사한 후 Visual Studio 2019 Preview에서 솔루션을 엽니다.

이 프로젝트의 코드는 아래 헤더 파일에 표시 된 기본 수학 연산에 대 한 기능을 제공 합니다. 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

C + +/WinRT 구성 요소를 만들고 winmd 파일을 생성 하는 방법에 대 한 자세한 내용은 [c + +/WinRT를 사용 하는 Windows 런타임 구성 요소](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)를 참조 하세요.

> [!NOTE]
> 구성 요소에서 [IInspectable:: GetRuntimeClassName](/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) 를 구현 하는 경우 유효한 WinRT 클래스 이름을 반환 **해야** 합니다. C #/Winrt는 interop에 클래스 이름 문자열을 사용 하므로 잘못 된 런타임 클래스 이름으로 인해 **InvalidCastException** 이 발생 합니다.

## <a name="add-a-projection-project-to-the-component-solution"></a>구성 요소 솔루션에 프로젝션 프로젝트 추가

리포지토리에서 샘플을 복제 한 경우 먼저 **SimpleMathProjection** 프로젝트를 삭제 하 여 연습 단계별 지침을 따르세요.

1. 새 **클래스 라이브러리 (.Net Core)** 프로젝트를 솔루션에 추가 합니다.

    1. **솔루션 탐색기** 에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고  ->  **새 프로젝트** 추가를 클릭 합니다.
    2. **새 프로젝트 추가 대화 상자** 에서 **클래스 라이브러리 (.net Core)** 프로젝트 템플릿을 검색 합니다. 템플릿을 선택 하 고 **다음** 을 클릭 합니다.
    3. 새 프로젝트의 이름을 **SimpleMathProjection** 하 고 **만들기** 를 클릭 합니다.

2. 프로젝트에서 빈 **Class1.cs** 파일을 삭제 합니다.

3. [C #/Winrt NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT)를 설치 합니다.

    1. **솔루션 탐색기** 에서 **SimpleMathProjection** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리** 를 선택 합니다. 
    2. **Microsoft. CsWinRT** NuGet 패키지를 검색 하 고 최신 버전을 설치 합니다.

4. **SimpleMathComponent** 프로젝트에 프로젝트 참조를 추가 합니다. **솔루션 탐색기** 에서 **SimpleMathProjection** 프로젝트 아래의 **종속성** 노드를 마우스 오른쪽 단추로 클릭 하 고, **프로젝트 참조 추가** 를 선택 하 고, **SimpleMathComponent** 프로젝트를 선택 합니다.

이러한 단계를 수행 하면 **솔루션 탐색기** 다음과 같이 표시 됩니다.

![프로젝션 프로젝트 종속성을 보여 주는 솔루션 탐색기](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>프로젝트 파일을 편집 하 여 c #/Winrt를 실행 합니다.

**cswinrt.exe** 를 호출 하 고 프로젝션 어셈블리를 생성 하기 전에 프로젝션 프로젝트에 대 한 프로젝트 파일을 편집 해야 합니다.

1. **솔루션 탐색기** 에서 **SimpleMathProjection** 노드를 두 번 클릭 하 여 편집기에서 프로젝트 파일을 엽니다.

2. Windows SDK를 `TargetFramework` 참조 하도록 요소를 업데이트 합니다. 이렇게 하면 interop 및 프로젝션 지원에 필요한 어셈블리 종속성 추가 됩니다. 이 샘플은이 연습을 통해 최신 Windows 10 릴리스를 대상으로 합니다. **net 5.0-Windows 10.0.19041.0** (SDK 버전 2004 라고도 함).

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. `PropertyGroup`여러 **cswinrt** 속성을 설정 하는 새 요소를 추가 합니다.

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    이 예의 설정에 대 한 자세한 내용은 다음과 같습니다.

    - `CsWinRTIncludes`속성은 프로젝트에 사용할 네임 스페이스를 지정 합니다.
    - `CsWinRTGeneratedFilesDir`속성은 프로젝션에서 파일이 생성 되는 출력 디렉터리를 설정 합니다 .이 디렉터리는 원본에서 빌드할 때 다음 섹션에서 설정 합니다.

4. 이 연습에서 최신 c #/Winrt 버전은 Windows 메타 데이터를 지정 해야 할 수 있습니다. 다음 중 하나를 사용 하 여 제공할 수 있습니다.

    - NuGet 패키지 참조 ( [예:).]( https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts/)

      ```xml
      <ItemGroup>
        <PackageReference Include="Microsoft.Windows.SDK.Contracts" Version="10.0.19041.1" />
      </ItemGroup>
      ```

    - 또 다른 옵션은 `CsWinRTWindowsMetadata` 3 단계에서의에 다음 속성을 추가 하는 것입니다 `PropertyGroup` .

      ```xml
      <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
      ```

5. **SimpleMathProjection** 파일을 저장 하 고 닫습니다.

## <a name="build-projects-out-of-source"></a>소스에서 프로젝트 빌드

[관련 샘플](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)에서 빌드는 **디렉터리. build. props** 파일을 사용 하 여 구성 됩니다. **SimpleMathComponent** 및 **SimpleMathProjection** 프로젝트를 빌드할 때 생성 된 파일은 솔루션 수준의 *_build* 폴더에 표시 됩니다. 소스 외부에서 빌드하도록 프로젝트를 구성 하려면 아래에 있는 **build. props** 파일을 솔루션 파일을 포함 하는 디렉터리에 복사 합니다.

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

이 단계는 프로젝션을 생성 하는 데 필요 하지 않지만, 동일한 디렉터리의 두 프로젝트에서 빌드 파일을 생성 하 고 빌드 정리 작업을 더 쉽게 만드는 방법으로 간단 하 게 만들 수 있습니다. 소스에서 빌드하지 않는 경우 **SimpleMathComponent** 및 interop 어셈블리 **SimpleMathComponent.dll** 모두 해당 하는 프로젝트 폴더의 다른 디렉터리에 생성 됩니다. 이러한 파일은 모두 아래 **SimpleMathProjection** 에서 참조 되므로 경로를 적절 하 게 변경 해야 합니다.

## <a name="create-a-nuget-package-from-the-projection"></a>프로젝션에서 NuGet 패키지 만들기

Interop 어셈블리를 배포 하 고 사용 하기 위해 몇 가지 추가 프로젝트 속성을 추가 하 여 솔루션을 빌드할 때 NuGet 패키지를 자동으로 만들 수 있습니다. .NET 5.0 대상의 경우 패키지에는 interop 어셈블리, 구현 어셈블리 및 필요한 c #/Winrt 런타임 어셈블리에 대 한 c #/Winrt NuGet 패키지에 대 한 종속성이 포함 되어야 합니다 ( **WinRT.Runtime.dll**.

1. NuGet 사양 (. nuspec) 파일을 **SimpleMathProjection** 프로젝트에 추가 합니다.

    1. **솔루션 탐색기** 에서 **SimpleMathProjection** 노드를 마우스 오른쪽 단추로 클릭 하 고 ,  ->  **새 폴더** 추가를 선택 하 고, 폴더 이름을 **nuget** 로 합니다. 
    2. **Nuget** 폴더를 마우스 오른쪽 단추로 클릭 하 고   ->  **새 항목** 추가를 선택한 다음 XML 파일을 선택 하 고 이름을 **SimpleMathProjection. nuspec** 로 선택 합니다. 

2. **SimpleMathProjection** 에 다음을 추가 하 여 패키지를 자동으로 생성 합니다. 이러한 속성은 `NuspecFile` NuGet 패키지를 생성 하는 및 디렉터리를 지정 합니다.

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example NuGet spec for distributing the interop assembly from the C++/WinRT component. Note that for .NET 5.0 targets, under the `dependencies` node there is a dependency on CsWinRT, and under the `files` node **SimpleMathProjection.dll** is specified instead of **SimpleMathComponent.winmd** for the target `lib\net5.0\SimpleMathProjection.dll`. This behavior is new in .NET 5.0 and enabled by C#/WinRT. The implementation assembly, **SimpleMathComponent.dll**, must also be deployed for .NET 5.0 targets. 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support netcore3, uap, net46+, net5, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>프로젝션 및 NuGet 패키지를 생성 하는 솔루션 빌드

이제 솔루션을 빌드할 수 있습니다. 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **솔루션 빌드** 를 선택 합니다. 그러면 먼저 구성 요소 프로젝트와 프로젝션 프로젝트가 빌드됩니다. Interop **.cs** 파일 및 어셈블리는 구성 요소 프로젝트의 메타 데이터 파일 외에도 출력 디렉터리에 생성 됩니다. 또한 **nuget** 폴더에 생성 된 Nuget 패키지 **simplemathcomponent 0.1.0** 를 볼 수 있습니다.

![프로젝션 생성을 보여 주는 솔루션 탐색기](images/projection-generated-files.png)

## <a name="reference-the-nuget-package-in-a-c-net-50-console-application"></a>C # .NET 5.0 콘솔 응용 프로그램에서 NuGet 패키지 참조

투영 된 **SimpleMathComponent** 을 사용 하기 위해 응용 프로그램에서 새로 만든 NuGet 패키지에 대 한 참조를 간단히 추가할 수 있습니다. 다음 단계에서는 별도의 솔루션에서 간단한 콘솔 앱을 만들어이 작업을 수행 하는 방법을 보여 줍니다.

1. **콘솔 앱 (.Net Core)** 프로젝트를 사용 하 여 새 솔루션을 만듭니다.

    1. Visual Studio에서 **파일** -> **새로 만들기** -> **프로젝트** 를 선택합니다.
    2. **새 프로젝트 추가 대화 상자** 에서 **콘솔 앱 (.net Core)** 프로젝트 템플릿을 검색 합니다. 템플릿을 선택 하 고 **다음** 을 클릭 합니다.
    3. 새 프로젝트의 이름을 **SampleConsoleApp** 하 고 **만들기** 를 클릭 합니다. 새 솔루션에서이 프로젝트를 만들면 **SimpleMathComponent** NuGet 패키지를 별도로 복원할 수 있습니다.

2. **솔루션 탐색기** 에서 **SampleConsoleApp** 노드를 두 번 클릭 하 여 **SampleConsoleApp** 프로젝트 파일을 열고 다음 예제와 같이 대상 프레임 워크 모니커 및 플랫폼 구성을 업데이트 합니다.

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. **SimpleMathComponent** NuGet 패키지를 **SampleConsoleApp** 프로젝트에 추가 합니다. MSIX 패키지에 패키지 되지 않은 앱에 필요한 [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) NuGet 패키지도 필요 합니다. 프로젝트를 빌드할 때 **SimpleMathComponent** nuget을 복원 하려면 `RestoreSources` 구성 요소 솔루션의 **nuget** 폴더에 대 한 경로와 함께 속성을 사용 하면 됩니다.

    ```xml
    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
      <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
      <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    이 연습에서는 **SimpleMathComponent** 에 대 한 NuGet 복원 경로가 두 솔루션 파일이 동일한 디렉터리에 있다고 가정 합니다. 또는 [로컬 NuGet 패키지 피드](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) 를 솔루션에 추가할 수 있습니다.

4. **SimpleMathComponent** 에서 제공 하는 기능을 사용 하도록 **Program.cs** 파일을 편집 합니다.

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. 콘솔 앱을 빌드하고 실행 합니다. 아래 출력이 표시 됩니다.

    ![콘솔 NET5 출력](images/console-output.png)

## <a name="resources"></a>리소스

- [이 연습에 대 한 전체 코드 샘플](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)
