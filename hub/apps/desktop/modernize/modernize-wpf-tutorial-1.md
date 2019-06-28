---
description: 이 자습서에는 UWP XAML 사용자 인터페이스를 추가, MSIX 패키지 만들기 및 WPF 앱에 다른 최신 구성 요소를 통합 하는 방법을 보여 줍니다.
title: Contoso 마이그레이션 expenses 3.NET Core로
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e718de7a22873ccf347e60c661f724ce3abdd2cf
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420130"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>1단계: Contoso 마이그레이션 expenses 3.NET Core로

Contoso Expenses 라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법에 설명 하는 자습서의 첫 번째 부분입니다. 자습서, 필수 구성 요소 및 샘플 앱을 다운로드 하기 위한 지침의 개요를 참조 하세요. [자습서: WPF 앱을 현대화](modernize-wpf-tutorial.md)합니다.
  
자습서의이 부분에서는.NET Framework 4.7.2에서에서 전체 Contoso expenses 마이그레이션할 됩니다 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3)합니다. 이 자습서의이 부분을 시작 하기 전에 해야 다음 수행 합니다.

* [열고 ContosoExpenses 예제를 빌드하려면](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) Visual Studio 2019에 있습니다.
* Visual Studio 2019의 릴리스 버전을 사용 하는 경우에.NET Core SDK의 미리 보기 버전을 사용 하도록 설정 합니다. Visual Studio로 이동 **도구 > 옵션**검색 상자에 "Preview"를 입력 하 고 선택 **.NET Core SDK의 미리 보기를 사용 하 여**입니다. 사용 중인 경우는 [미리 보기 버전의 Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/),.NET Core 미리 보기는 기본적으로 사용 하기 때문에이 옵션을 선택할 필요가 없습니다.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>.NET Core 3로 ContosoExpenses 프로젝트 마이그레이션

이 섹션에서는.NET Core 3에서 Contoso expenses ContosoExpenses 프로젝트를 마이그레이션할 수 있습니다. 과 같은 기존 ContosoExpenses 프로젝트가 아니라.NET Framework 4.7.2는 대신.NET Core 3 대상 파일을 포함 하는 새 프로젝트 파일을 만들어이 수행할 수 있습니다. 이 옵션을 사용 하면 앱의.NET Framework 및.NET Core 버전을 사용 하 여 단일 솔루션을 유지할 수 있습니다.

1. ContosoExpenses.NET Framework 4.7.2 대상으로 현재 프로젝트를 확인 합니다. 솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses** 프로젝트를 선택 **속성**, 있는지 확인 합니다 **대상 프레임 워크** 속성을는  **응용 프로그램** 탭은.NET Framework 4.7.2로 설정 됩니다.

    ![.NET framework 버전 4.7.2 프로젝트](images/wpf-modernize-tutorial/NETFramework472.png)

3. Windows 탐색기에서로 이동 합니다 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** 폴더 라는 새 텍스트 파일을 만들어 **ContosoExpenses.Core.csproj**합니다.

4. 파일을 마우스 오른쪽 단추로 클릭, 선택 **여**, 메모장, Visual Studio Code 또는 Visual Studio와 같은 원하는 텍스트 편집기에서 엽니다.

5. 파일에 다음 텍스트를 복사 하 고 저장 합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 파일을 닫고를 반환 합니다 **ContosoExpenses** Visual Studio에서 솔루션입니다.

7. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses** 솔루션 선택 **-> 기존 프로젝트 추가**합니다. 선택 된 **ContosoExpenses.Core.csproj** 에서 방금 만든 파일을 `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` 솔루션에 추가할 폴더.

합니다 **ContosoExpenses.Core.csproj** 는 다음 요소가 포함 됩니다.

* 합니다 **프로젝트** 요소는 SDK 버전을 지정 **Microsoft.NET.Sdk.WindowsDesktop**합니다. 이 for Windows Desktop.NET 응용 프로그램을 나타내며 WPF 및 Windows Forms 앱에 대 한 구성 요소가 포함 됩니다.
* 합니다 **PropertyGroup** 요소 프로젝트 출력 실행 파일 (DLL 되지)는.NET Core 3을 대상으로 하 고 WPF를 사용 하 여 나타내는 자식 요소가 포함 되어 있습니다. Windows Forms 앱에 대 한 사용을 **UseWinForms** 요소 대신 합니다 **UseWPF** 요소입니다.

> [!NOTE]
> .NET Core 3.0에 도입 된.csproj 형식으로 작업할 경우.csproj와 동일한 폴더에 있는 모든 파일에는 프로젝트의 일부로 간주 됩니다. 따라서 프로젝트에 포함 된 모든 파일을 지정할 필요가 없습니다. 사용자 지정 빌드 작업을 하거나 정의 하려면 해당 파일을 제외 하려면를 지정 해야 합니다.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>.NET Standard로 ContosoExpenses.Data 프로젝트 마이그레이션

합니다 **ContosoExpenses** 솔루션에는 **ContosoExpenses.Data** 모델 및 서비스에 대 한 인터페이스를 포함 하 고.NET 4.7.2를 대상으로 하는 클래스 라이브러리. .NET core 3.0 앱으로.NET Core에서 사용할 수 없는 Api를 사용 하지 않는.NET Framework 라이브러리를 사용할 수 있습니다. 그러나 최상의 현대화 경로.NET standard 라이브러리를 옮기는 것입니다. 이렇게 하면 라이브러리는.NET Core 3.0 앱에서 완벽 하 게 지원 됩니다. 또한 (ASP.NET Core)를 통해 웹 및 모바일 (Xamarin)를 통해 같은 다른 플랫폼도 사용 하 여 라이브러리를 다시 사용할 수 있습니다.

마이그레이션할 합니다 **ContosoExpenses.Data** .NET standard 프로젝트:

1. Visual Studio에서 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Data** 프로젝트를 선택 **프로젝트 언로드**합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 선택한 **ContosoExpenses.Data.csproj 편집**합니다.

2. 프로젝트 파일의 전체 내용을 삭제 합니다.

3. 다음 XML 복사 및 붙여넣고 파일을 저장 합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Data** 프로젝트를 선택 **프로젝트 다시 로드**합니다.

## <a name="configure-nuget-packages-and-dependencies"></a>NuGet 패키지 및 종속성 구성

마이그레이션될 때 합니다 **ContosoExpenses.Core** 하 고 **ContosoExpenses.Data** 프로젝트 이전 섹션에서는 프로젝트에서 NuGet 패키지 참조를 제거 합니다. 이 섹션에서는 이러한 참조를 다시 추가 합니다.

에 대 한 NuGet 패키지를 구성 하는 **ContosoExpenses.Data** 프로젝트:

1.  에 **ContosoExpenses.Data** 프로젝트를 확장 합니다 **종속성** 노드. 유의 합니다 **NuGet** 섹션은 없습니다.

    ![NuGet 패키지](images/wpf-modernize-tutorial/NuGetPackages.png)

    열면 합니다 **Packages.config** 에 **솔루션 탐색기** '이전' NuGet 패키지 참조 전체.NET Framework을 사용한 경우 프로젝트 사용을 찾을 수 있습니다.

    ![패키지 및 종속성](images/wpf-modernize-tutorial/Packages.png)

    내용이 여기에 **Packages.config** 파일입니다. 모든 NuGet 패키지를 대상으로 전체.NET Framework 4.7.2는 알 수 있습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. 에 **ContosoExpenses.Data** 프로젝트를 삭제 합니다 **Packages.config** 파일입니다.

4. 에 **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다 **종속성** 노드 선택 **NuGet 패키지 관리**합니다.

  ![NuGet 패키지 관리...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. 에 **NuGet 패키지 관리자** 창에서 클릭 **찾아보기**합니다. 검색 된 `Bogus` 패키지 하 고 설치 합니다.

    ![가짜 NuGet 패키지](images/wpf-modernize-tutorial/Bogus.png)

6. 검색 된 `LiteDB` 패키지 하 고 설치 합니다.

    ![LiteDB NuGet 패키지](images/wpf-modernize-tutorial/LiteDB.png)

    이러한 NuGet 패키지이 목록을 저장 된 프로젝트를 packages.config 파일에 더 이상 이므로 궁금할 수 없습니다. 참조 된 NuGet 패키지는.csproj 파일에 직접 저장 됩니다. 내용을 확인 하 여이 확인할 수 있습니다 합니다 **ContosoExpenses.Data.csproj** 텍스트 편집기에서 프로젝트 파일입니다. 파일의 끝에 추가한 다음 줄을 찾을 수 있습니다.

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > .NET Framework 4.7.2 프로젝트에서 사용 되는 것과이.NET Core 3 프로젝트에 대 한 동일한 패키지를 설치 하 고 있는지 확인할 수 있습니다. NuGet 패키지는 멀티 타기 팅을 지원 합니다. 라이브러리 작성자가 다양 한 아키텍처 및 플랫폼, 동일한 패키지에 다른 버전의 라이브러리를 포함할 수 있습니다. 이러한 패키지.NET Core 3 프로젝트와 호환 되는 전체.NET Framework 뿐만 아니라.NET Standard 2.0을 지원 합니다. .NET Framework,.NET Core 및.NET Standard 차이점에 대 한 자세한 내용은 참조 하세요. [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)합니다.

에 대 한 NuGet 패키지를 구성 하는 **ContosoExpenses.Core** 프로젝트:

1. 에 **ContosoExpenses.Core** 프로젝트를 열고 합니다 **packages.config** 파일. 현재.NET Framework 4.7.2를 대상으로 하는 다음 참조를 포함 되었는지 확인 합니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    .NET 표준 버전의 다음 단계에서는 해야 합니다 `MvvmLightLibs` 및 `Unity` 패키지 합니다. 다른 두 가지 종속성이 두 라이브러리를 설치할 때 NuGet에서 자동으로 다운로드 합니다.

2. 에 **ContosoExpenses.Core** 프로젝트를 삭제 합니다 **Packages.config** 파일입니다.

3. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **NuGet 패키지 관리**합니다.

4. 에 **NuGet 패키지 관리자** 창에서 클릭 **찾아보기**합니다. 검색 된 `Unity` 패키지 하 고 설치 합니다.

    ![Unity 패키지](images/wpf-modernize-tutorial/UnityPackage.png)

5. 검색 된 `MvvmLightLibsStd10` 패키지 하 고 설치 합니다. 이 버전의.NET Standard 버전 이기는 `MvvmLightLibs` 패키지 있습니다. 이 패키지에 대 한 작성자는.NET Standard 버전의.NET Framework 버전 보다 별도 패키지에서 라이브러리 패키지 하도록 선택 했습니다.

    ! MvvmLightsLibs 패키지[](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. 에 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다 **종속성** 노드 선택 **참조 추가**합니다.

7. 에 **프로젝트 > 솔루션** 범주를 선택한 **ContosoExpenses.Data** 클릭 **확인**합니다.

    ![참조 추가](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>자동으로 생성 된 어셈블리 특성을 사용 하지 않도록 설정

이 시점에, 마이그레이션 프로세스에서 빌드 하려는 경우는 **ContosoExpenses.Core** 프로젝트 몇 가지 오류가 표시 됩니다.

![.NET core 3 빌드에 대 한 새 오류](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

프로젝트 파일에서 어셈블리 정보를.NET Core 3.0 저장소를 사용 하 여 도입 된 새.csproj 형식 때문에이 문제가 발생 하는 것이 아니라 **AssemblyInfo.cs** 파일입니다. 이러한 오류를 해결 하려면이 동작을 사용 하지 않도록 설정 하 고 프로젝트를 계속 사용할 수 있도록 합니다 **AssemblyInfo.cs** 파일입니다.

1. Visual Studio에서 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **프로젝트 언로드**합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 선택한 **ContosoExpenses.Core.csproj 편집**합니다.

1. 다음 요소를 추가 합니다 **PropertyGroup** 섹션 및 파일을 저장 합니다.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    이 요소를 추가한 후 합니다 **PropertyGroup** 섹션이 다음과 같아집니다.

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **프로젝트 다시 로드**합니다.

4. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Data** 프로젝트를 선택 **프로젝트 언로드**합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 선택한 **ContosoExpenses.Data.csproj 편집**합니다.

5. 동일한 항목을 추가 합니다 **PropertyGroup** 섹션 및 파일을 저장 합니다.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    이 요소를 추가한 후 합니다 **PropertyGroup** 섹션이 다음과 같아집니다.

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Data** 프로젝트를 선택 **프로젝트 다시 로드**합니다.

## <a name="add-the-windows-compatibility-pack"></a>Windows 호환 기능 팩 추가

컴파일 하려는 경우는 **ContosoExpenses.Core** 하 고 **ContosoExpenses.Data** 프로젝트를 볼 수는 이제 이전 오류를 해결 하지만 여전히에 일부 오류가 있을  **ContosoExpenses.Data** 다음과 유사한 라이브러리입니다.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

이러한 오류는 변환의 결과 **ContosoExpenses.Data** 를 Linux, Android, iOS를 포함 한 여러 플랫폼에서 실행 하는.NET Standard 라이브러리에는.NET Framework 라이브러리 (Windows에 특정 됨)에서 프로젝트 입니다. **ContosoExpenses.Data** 라는 클래스를 포함 하는 프로젝트 **RegistryService**, 레지스트리, Windows 전용 개념 상호 작용 하는 합니다.

이러한 오류를 해결 하려면 설치 합니다 [Windows 호환성](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 패키지. 이 패키지는.NET Standard 라이브러리에 사용할 대부분의 Windows 관련 Api에 대 한 지원을 제공 합니다. 라이브러리를 더 이상 플랫폼 간 하지만이 패키지를 사용 하 여은 여전히.NET 표준 대상 후 합니다. 

1. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Data** 프로젝트입니다.
2. 선택할 **NuGet 패키지 관리**합니다.
3. 에 **NuGet 패키지 관리자** 창에서 클릭 **찾아보기**합니다. 검색 된 `Microsoft.Windows.Compatibility` 패키지 하 고 설치 합니다. 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 이제 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 컴파일하는 데 다시 시도 합니다 **ContosoExpenses.Data** 프로젝트 및 선택 **빌드**합니다.

이 빌드 프로세스는 오류 없이 완료 됩니다.

## <a name="test-and-debug-the-migration"></a>테스트 및 마이그레이션 디버그

이제 프로젝트에 성공적으로 빌드할 준비가 실행 하 고 테스트 응용 프로그램에서 런타임 오류가 있는지 확인 합니다.

1. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **시작 프로젝트로 설정**합니다.

2. F5 키를 눌러 시작 합니다 **ContosoExpenses.Core** 디버거에서 프로젝트입니다. 다음과 유사한 예외가 표시 됩니다.

    ![Visual Studio에 표시 되는 예외](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    에 대 한 정보를 제거 하는 마이그레이션 시작 부분에.csproj 파일에서 콘텐츠를 삭제 하는 경우이 예외가 발생 되는 **빌드 작업** 이미지 파일에 대 한 합니다. 다음 단계는이 문제를 해결 합니다.

3. 디버거를 중지 합니다.

4. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **프로젝트 언로드**합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 선택한 **ContosoExpenses.Core.csproj 편집**합니다.

5. 닫기 전에 **프로젝트** 요소에 다음 항목을 추가 합니다.

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **프로젝트 다시 로드**합니다.

7. Contoso.ico 앱에 할당 하려면 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트를 선택 **속성**합니다. 열린된 페이지에서 드롭다운 목록에서 클릭 **아이콘이** 선택한 `Images\contoso.ico`합니다.

    ![프로젝트의 속성에서 Contoso 아이콘](images/wpf-modernize-tutorial/ContosoIco.png)

8. **저장**을 클릭합니다.

9. F5 키를 눌러 시작 합니다 **ContosoExpenses.Core** 디버거에서 프로젝트입니다. 앱이 이제 실행 되는지 확인 합니다.

## <a name="next-steps"></a>다음 단계

이 시점에서 자습서를 성공적으로 마이그레이션한 Contoso expenses.NET Core 3입니다. 에 대 한 준비가 [2 부: XAML 제도 사용 하 여 UWP InkCanvas 컨트롤을 추가](modernize-wpf-tutorial-2.md)합니다.

> [!NOTE]
> 고해상도 화면에 있는 경우 앱으로 매우 작은 진행 되는지 확인할 수 있습니다. 자습서의 다음 단계에서이 문제를 해결 합니다.
