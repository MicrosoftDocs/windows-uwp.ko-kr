---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가 하 고, MSIX 개의 패키지를 만들고, 기타 최신 구성 요소를 WPF 앱에 통합 하는 방법을 보여 줍니다.
title: Contoso Expenses 앱을 .NET Core 3으로 마이그레이션
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 97488a913605916c067861b5941d7aa127b00917
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643408"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>1단계: Contoso Expenses 앱을 .NET Core 3으로 마이그레이션

Contoso 지출 이라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법을 보여 주는 자습서의 첫 번째 부분입니다. 샘플 앱을 다운로드 하기 위한 자습서, 필수 구성 요소 및 지침에 대 한 개요를 [보려면 자습서: WPF 앱](modernize-wpf-tutorial.md)을 현대화 합니다.
  
자습서의이 부분에서는 .NET Framework 4.7.2에서 [.Net Core 3](modernize-wpf-tutorial.md#net-core-3)으로 전체 Contoso 지출 앱을 마이그레이션합니다. 자습서의이 부분을 시작 하기 전에 다음을 수행 해야 합니다.

* Visual Studio 2019에서 [ContosoExpenses 샘플을 열고 빌드합니다](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) .
* Visual Studio 2019의 릴리스 버전을 사용 하는 경우 .NET Core SDK 미리 보기 버전을 사용 하도록 설정 합니다. Visual Studio에서 **도구 > 옵션**으로 이동 하 여 검색 상자에 "Preview"를 입력 하 고 **.NET Core SDK 미리 보기 사용**을 선택 합니다. [Visual Studio 2019의 미리 보기 버전](https://visualstudio.microsoft.com/vs/preview/)을 사용 하는 경우 .net Core 미리 보기가 기본적으로 사용 하도록 설정 되어 있으므로이 옵션을 선택 하지 않아도 됩니다.

> [!NOTE]
> .NET Framework에서 .NET Core 3으로 WPF 응용 프로그램을 마이그레이션하는 방법에 대 한 자세한 내용은 [이 블로그 시리즈](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)를 참조 하세요.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>ContosoExpenses 프로젝트를 .NET Core 3으로 마이그레이션

이 섹션에서는 Contoso 지출 앱의 ContosoExpenses 프로젝트를 .NET Core 3으로 마이그레이션합니다. 기존 ContosoExpenses 프로젝트와 동일한 파일을 포함 하는 새 프로젝트 파일을 만들어이 작업을 수행 하지만 .NET Framework 4.7.2 대신 .NET Core 3을 대상으로 합니다. 이를 통해 .NET Framework 및 .NET Core 버전의 앱을 모두 갖춘 단일 솔루션을 유지할 수 있습니다.

1. ContosoExpenses 프로젝트가 현재 .NET Framework 4.7.2를 대상으로 하는지 확인 합니다. 솔루션 탐색기에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택한 다음 **응용 프로그램** 탭의 **대상 프레임 워크** 속성이 .NET Framework 4.7.2로 설정 되어 있는지 확인 합니다.

    ![프로젝트에 대 한 .NET Framework 버전 4.7.2](images/wpf-modernize-tutorial/NETFramework472.png)

3. Windows 탐색기에서 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** 폴더로 이동 하 여 **ContosoExpenses**라는 새 텍스트 파일을 만듭니다.

4. 파일을 마우스 오른쪽 단추로 클릭 하 고 **연결**프로그램을 선택한 다음 사용자가 선택한 텍스트 편집기에서 엽니다 (예: 메모장, Visual Studio Code 또는 Visual Studio).

5. 다음 텍스트를 파일에 복사 하 고 저장 합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 파일을 닫고 Visual Studio에서 **ContosoExpenses** 솔루션으로 돌아갑니다.

7. **ContosoExpenses** 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가-> 기존 프로젝트**를 선택 합니다. `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` 폴더에서 방금 만든 **ContosoExpenses** 파일을 선택 하 여 솔루션에 추가 합니다.

**ContosoExpenses** 에는 다음 요소가 포함 됩니다.

* **Project** 요소는 **Microsoft .net SDK**버전의 sdk 버전을 지정 합니다. 이는 Windows 데스크톱용 .NET 응용 프로그램을 나타내며 WPF 및 Windows Forms apps의 구성 요소를 포함 합니다.
* **PropertyGroup** 요소는 프로젝트 출력이 DLL이 아닌 실행 파일 임을 나타내는 자식 요소를 포함 하 고, .net Core 3을 대상으로 하며, WPF를 사용 합니다. Windows Forms 앱의 경우 **Usewpf** 요소 대신 **UseWinForms** 요소를 사용 합니다.

> [!NOTE]
> .NET Core 3.0에 도입 된 .csproj 형식으로 작업할 때 .csproj와 동일한 폴더에 있는 모든 파일은 프로젝트의 일부로 간주 됩니다. 따라서 프로젝트에 포함 된 모든 파일을 지정할 필요가 없습니다. 사용자 지정 빌드 작업을 정의 하거나 제외 하려는 파일만 지정 해야 합니다.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>ContosoExpenses 프로젝트를 .NET Standard로 마이그레이션

**ContosoExpenses** 솔루션에는 서비스 및 대상 .net 4.7.2에 대 한 모델 및 인터페이스를 포함 하는 **ContosoExpenses** 클래스 라이브러리가 포함 되어 있습니다. .Net Core 3.0 앱은 .NET Core에서 사용할 수 없는 Api를 사용 하지 않는 경우 .NET Framework 라이브러리를 사용할 수 있습니다. 그러나 가장 좋은 현대화 경로는 라이브러리를 .NET Standard 이동 하는 것입니다. 이렇게 하면 .NET Core 3.0 앱에서 라이브러리를 완전히 지원 합니다. 또한 웹 (ASP.NET Core) 및 모바일 (Xamarin을 통해) 등의 다른 플랫폼과 함께 라이브러리를 다시 사용할 수 있습니다.

**ContosoExpenses** 프로젝트를 .NET Standard로 마이그레이션하려면 다음을 수행 합니다.

1. Visual Studio에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 **ContosoExpenses 편집**을 선택 합니다.

2. 프로젝트 파일의 전체 내용을 삭제 합니다.

3. 다음 XML을 복사 하 여 붙여넣고 파일을 저장 합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

## <a name="configure-nuget-packages-and-dependencies"></a>NuGet 패키지 및 종속성 구성

이전 섹션에서 **ContosoExpenses** 및 **ContosoExpenses** 프로젝트를 마이그레이션하면 프로젝트에서 NuGet 패키지 참조가 제거 됩니다. 이 섹션에서는 이러한 참조를 다시 추가 합니다.

**ContosoExpenses** 프로젝트에 대 한 NuGet 패키지를 구성 하려면:

1. **ContosoExpenses** 프로젝트에서 **종속성** 노드를 확장 합니다. **NuGet** 섹션이 누락 되었는지 확인 합니다.

    ![NuGet 패키지](images/wpf-modernize-tutorial/NuGetPackages.png)

    **솔루션 탐색기** 에서 **패키지** 를 열면 전체 .NET Framework를 사용 하는 경우 프로젝트를 사용 하는 NuGet 패키지의 ' 이전 ' 참조가 검색 됩니다.

    ![종속성 및 패키지](images/wpf-modernize-tutorial/Packages.png)

    다음은 **패키지 .config** 파일의 내용입니다. 모든 NuGet 패키지가 전체 .NET Framework 4.7.2를 대상으로 하는 것을 알 수 있습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. **ContosoExpenses** 프로젝트에서 **패키지 .config** 파일을 삭제 합니다.

4. **ContosoExpenses** 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.

  ![NuGet 패키지 관리 ...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭 합니다. `Bogus` 패키지를 검색 하 고 안정적인 최신 버전을 설치 합니다.

    ![가짜 NuGet 패키지](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB` 패키지를 검색 하 고 안정적인 최신 버전을 설치 합니다.

    ![LiteDB NuGet 패키지](images/wpf-modernize-tutorial/LiteDB.png)

    프로젝트에 더 이상 패키지 .config 파일이 없으므로 이러한 NuGet 패키지 목록이 저장 되는 위치를 궁금할 수 있습니다. 참조 된 NuGet 패키지는 .csproj 파일에 직접 저장 됩니다. 텍스트 편집기에서 **ContosoExpenses** 프로젝트 파일의 내용을 확인 하 여이를 확인할 수 있습니다. 파일의 끝에 다음 줄이 추가 된 것을 확인할 수 있습니다.

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 이 .NET Core 3 프로젝트에 대해 4.7.2 프로젝트 .NET Framework에서 사용 하는 것과 동일한 패키지를 설치 하는 것을 확인할 수도 있습니다. NuGet 패키지는 다중 대상 지정을 지원 합니다. 라이브러리 작성자는 다른 아키텍처 및 플랫폼에 대해 컴파일된 동일한 패키지에 서로 다른 버전의 라이브러리를 포함할 수 있습니다. 이러한 패키지는 .NET Core 3 프로젝트와 호환 되는 전체 .NET Framework 및 .NET Standard 2.0를 지원 합니다. .NET Framework, .NET Core 및 .NET Standard의 차이점에 대 한 자세한 내용은 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)를 참조 하세요.

ContosoExpenses 프로젝트에 대 한 NuGet 패키지를 구성 하려면 다음을 수행 **합니다** .

1. **ContosoExpenses** 프로젝트에서 **패키지 .config** 파일을 엽니다. .NET Framework 4.7.2를 대상으로 하는 다음 참조가 현재 포함 되어 있습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    다음 단계에서는 `MvvmLightLibs` 및 `Unity` 패키지 버전을 .NET Standard 합니다. 다른 두 라이브러리는 이러한 두 라이브러리를 설치할 때 NuGet에서 자동으로 다운로드 하는 종속성입니다.

2. **ContosoExpenses** 프로젝트에서 **패키지 .config** 파일을 삭제 합니다.

3. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.

4. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭 합니다. `Unity` 패키지를 검색 하 고 안정적인 최신 버전을 설치 합니다.

    ![Unity 패키지](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10` 패키지를 검색 하 고 안정적인 최신 버전을 설치 합니다. `MvvmLightLibs` 패키지의 .NET Standard 버전입니다. 이 패키지의 경우 작성자는 라이브러리의 .NET Standard 버전을 .NET Framework 버전이 아닌 별도의 패키지에 패키지 하도록 선택 했습니다.

    ![MvvmLightsLibs 패키지](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. **ContosoExpenses** 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다.

7. **프로젝트 > 솔루션** 범주에서 **ContosoExpenses** 를 선택 하 고 **확인**을 클릭 합니다.

    ![참조 추가](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>자동 생성 된 어셈블리 특성 사용 안 함

마이그레이션 프로세스의이 시점에서 **ContosoExpenses** 프로젝트를 빌드하려고 하면 몇 가지 오류가 표시 됩니다.

![.NET Core 3 빌드 새 오류](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

이 문제는 .NET Core 3.0에 도입 된 새 .csproj 형식이 **AssemblyInfo.cs** 파일이 아닌 프로젝트 파일에 어셈블리 정보를 저장 하기 때문에 발생 합니다. 이러한 오류를 해결 하려면이 동작을 사용 하지 않도록 설정 하 고 프로젝트에서 **AssemblyInfo.cs** 파일을 계속 사용할 수 있도록 합니다.

1. Visual Studio에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 **ContosoExpenses 편집**을 선택 합니다.

1. **PropertyGroup** 섹션에 다음 요소를 추가 하 고 파일을 저장 합니다.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    이 요소를 추가한 후에는 **PropertyGroup** 섹션이 다음과 같이 표시 됩니다.

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

4. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 **ContosoExpenses 편집**을 선택 합니다.

5. **PropertyGroup** 섹션에 같은 항목을 추가 하 고 파일을 저장 합니다.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    이 요소를 추가한 후에는 **PropertyGroup** 섹션이 다음과 같이 표시 됩니다.

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

## <a name="add-the-windows-compatibility-pack"></a>Windows 호환 기능 팩 추가

이제 **ContosoExpenses** 및 **ContosoExpenses** 프로젝트를 컴파일하려고 하면 이전 오류가 수정 된 것을 알 수 있지만 **ContosoExpenses** 라이브러리에는 여전히 몇 가지 오류가 있습니다.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

이러한 오류는 Linux, Android, iOS 등을 비롯 한 여러 플랫폼에서 실행할 수 있는 .NET Framework 라이브러리 (Windows 용)에서 .NET Standard 라이브러리로 **ContosoExpenses** 프로젝트를 변환 하는 경우에 발생 합니다. **ContosoExpenses** 프로젝트에는 Windows 전용 개념인 레지스트리와 상호 작용 하는 **RegistryService**라는 클래스가 포함 되어 있습니다.

이러한 오류를 해결 하려면 [Windows 호환성](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 패키지를 설치 합니다. 이 패키지는 .NET Standard 라이브러리에서 사용할 수 있는 많은 Windows 관련 Api에 대 한 지원을 제공 합니다. 라이브러리는이 패키지를 사용한 후 더 이상 플랫폼 간이 아니지만 여전히 .NET Standard 대상으로 합니다. 

1. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.
2. **NuGet 패키지 관리**를 선택 합니다.
3. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭 합니다. `Microsoft.Windows.Compatibility` 패키지를 검색 하 고 안정적인 최신 버전을 설치 합니다.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 이제 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **빌드**를 선택 하 여 프로젝트를 다시 컴파일합니다.

이번에는 빌드 프로세스가 오류 없이 완료 됩니다.

## <a name="test-and-debug-the-migration"></a>마이그레이션 테스트 및 디버그

이제 프로젝트가 성공적으로 빌드 되었으므로 앱을 실행 및 테스트 하 여 런타임 오류가 있는지 확인할 수 있습니다.

1. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다.

2. F5 키를 눌러 디버거에서 **ContosoExpenses** 프로젝트를 시작 합니다. 다음과 유사한 예외가 표시 됩니다.

    ![Visual Studio에 표시 되는 예외](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    이 예외는 마이그레이션이 시작 될 때 .csproj 파일에서 콘텐츠를 삭제 했을 때 이미지 파일에 대 한 **빌드 작업** 에 대 한 정보를 제거 했기 때문에 발생 합니다. 다음 단계는이 문제를 해결 합니다.

3. 디버거를 중지 합니다.

4. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 **ContosoExpenses 편집**을 선택 합니다.

5. **프로젝트** 닫기 요소 앞에 다음 항목을 추가 합니다.

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

7. Contoso .ico를 앱에 할당 하려면 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다. 열린 페이지에서 **아이콘** 아래의 드롭다운을 클릭 하 고를 선택 `Images\contoso.ico`합니다.

    ![프로젝트 속성의 Contoso 아이콘](images/wpf-modernize-tutorial/ContosoIco.png)

8. **저장**을 클릭합니다.

9. F5 키를 눌러 디버거에서 **ContosoExpenses** 프로젝트를 시작 합니다. 이제 앱이 실행 되는지 확인 합니다.

## <a name="next-steps"></a>다음 단계

자습서의이 시점에서 Contoso 지출 앱을 .NET Core 3으로 마이그레이션 했습니다. 이제 2 부를 사용할 [준비가 되었습니다. XAML 아일랜드](modernize-wpf-tutorial-2.md)를 사용 하 여 UWP InkCanvas 컨트롤을 추가 합니다.

> [!NOTE]
> 고해상도 화면이 표시 되 면 앱이 매우 작은 것으로 보일 수 있습니다. 자습서의 다음 단계에서이 문제를 해결 합니다.
