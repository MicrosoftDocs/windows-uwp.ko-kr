---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, 다른 최신 구성 요소를 WPF 앱에 통합하는 방법을 보여 줍니다.
title: Contoso Expenses 앱을 .NET Core 3으로 마이그레이션
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "71317101"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>1부: Contoso Expenses 앱을 .NET Core 3으로 마이그레이션

Contoso 지출이라는 샘플 WPF 데스크톱 앱을 현대화하는 방법을 보여주는 자습서의 첫 번째 부분입니다. 자습서의 개요, 필수 조건 및 샘플 앱 다운로드 지침은 [자습서: WPF 앱 현대화](modernize-wpf-tutorial.md)를 참조하세요.
  
이 자습서 부분에서는 전체 Contoso Expenses 앱을 .NET Framework 4.7.2에서 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3)으로 마이그레이션합니다. 이 자습서 부분을 시작하기 전에 Visual Studio 2019에서 [ContosoExpenses 샘플을 열고 빌드](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)해야 합니다.

> [!NOTE]
> .NET Framework에서 .NET Core 3으로 WPF 애플리케이션을 마이그레이션하는 방법에 대한 자세한 내용은 [이 블로그 시리즈](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)를 참조하세요.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>ContosoExpenses 프로젝트를 .NET Core 3으로 마이그레이션

이 섹션에서는 Contoso Expenses 앱의 ContosoExpenses 프로젝트를 .NET Core 3으로 마이그레이션합니다. 먼저 기존 ContosoExpenses 프로젝트와 동일한 파일을 포함하지만 .NET Framework 4.7.2 대신 .NET Core 3을 대상으로 하는 새 프로젝트 파일을 만듭니다. 그러면 .NET Framework 및 .NET Core 버전의 앱을 모두 포함하는 단일 솔루션을 유지 관리할 수 있습니다.

1. ContosoExpenses 프로젝트가 현재 .NET Framework 4.7.2를 대상으로 하는지 확인합니다. 솔루션 탐색기에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택한 다음, **애플리케이션** 탭의 **대상 프레임워크** 속성이 .NET Framework 4.7.2로 설정되어 있는지 확인합니다.

    ![프로젝트의 .NET Framework 버전 4.7.2](images/wpf-modernize-tutorial/NETFramework472.png)

3. Windows 탐색기에서 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** 폴더로 이동한 다음, **ContosoExpenses.Core.csproj**라는 새 텍스트 파일을 만듭니다.

4. 파일을 마우스 오른쪽 단추로 클릭하고 **연결 프로그램**을 선택한 다음, 원하는 텍스트 편집기(예: 메모장, Visual Studio Code 또는 Visual Studio)에서 엽니다.

5. 다음 텍스트를 파일에 복사하고 저장합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. 파일을 닫고 Visual Studio의 **ContosoExpenses** 솔루션으로 돌아갑니다.

7. **ContosoExpenses** 솔루션을 마우스 오른쪽 단추로 클릭하고 **추가 -> 기존 프로젝트**를 선택합니다. `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` 폴더에서 방금 만든 **ContosoExpenses.Core.csproj** 파일을 선택하여 솔루션에 추가합니다.

**ContosoExpenses.Core.csproj**에는 다음 요소가 포함되어 있습니다.

* **Project** 요소는 **Microsoft.NET.Sdk.WindowsDesktop**의 SDK 버전을 지정합니다. 이 버전은 Windows 데스크톱용 .NET 애플리케이션을 가리키며 WPF 및 Windows Forms 앱의 구성 요소를 포함합니다.
* **PropertyGroup** 요소에는 프로젝트 출력이 DLL이 아닌 실행 파일이고, .NET Core 3을 대상으로 하며, WPF를 사용함을 나타내는 자식 요소가 포함되어 있습니다. Windows Forms 앱의 경우 **UseWPF** 요소 대신 **UseWinForms** 요소를 사용합니다.

> [!NOTE]
> .NET Core 3.0에서 도입된 .csproj 형식을 사용하는 경우, .csproj와 동일한 폴더에 있는 모든 파일이 프로젝트의 일부로 간주됩니다. 따라서 프로젝트에 포함된 모든 파일을 지정하지 않아도 됩니다. 사용자 지정 빌드 작업을 정의하거나 제외하려는 파일만 지정해야 합니다.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>ContosoExpenses.Data 프로젝트를 .NET Standard로 마이그레이션

**ContosoExpenses** 솔루션에는 서비스의 모델과 인터페이스를 포함하며 .NET 4.7.2를 대상으로 하는 **ContosoExpenses.Data** 클래스 라이브러리가 포함되어 있습니다. .Net Core 3.0 앱은 .NET Core에서 사용할 수 없는 API를 사용하지 않는 한, .NET Framework 라이브러리를 사용할 수 있습니다. 그러나 가장 좋은 현대화 경로는 라이브러리를 .NET Standard로 이동하는 것입니다. 그러면 .NET Core 3.0 앱에서 라이브러리를 완전히 지원합니다. 또한 웹(ASP.NET Core 사용), 모바일(Xamarin 사용) 등의 다른 플랫폼에서 라이브러리를 재사용할 수 있습니다.

**ContosoExpenses.Data** 프로젝트를 .NET Standard로 마이그레이션하려면 다음을 수행합니다.

1. Visual Studio에서 **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 언로드**를 선택합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭하고 **ContosoExpenses.Data.csproj 편집**을 선택합니다.

2. 프로젝트 파일의 전체 내용을 삭제합니다.

3. 다음 XML을 복사하여 붙여넣고 파일을 저장합니다.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 다시 로드**를 선택합니다.

## <a name="configure-nuget-packages-and-dependencies"></a>NuGet 패키지 및 종속성 구성

이전 섹션에서 **ContosoExpenses.Core** 및 **ContosoExpenses.Data** 프로젝트를 마이그레이션할 때 프로젝트에서 NuGet 패키지 참조를 제거했습니다. 이 섹션에서는 해당 참조를 다시 추가합니다.

**ContosoExpenses.Data** 프로젝트의 NuGet 패키지를 구성하려면 다음을 수행합니다.

1. **ContosoExpenses.Data** 프로젝트에서 **종속성** 노드를 펼칩니다. **NuGet** 섹션이 누락된 것을 확인합니다.

    ![NuGet 패키지](images/wpf-modernize-tutorial/NuGetPackages.png)

    **솔루션 탐색기**에서 **Packages.config**를 열면 전체 .NET Framework를 사용할 때 프로젝트에서 사용된 NuGet 패키지의 ‘이전’ 참조를 찾을 수 있습니다.

    ![종속성 및 패키지](images/wpf-modernize-tutorial/Packages.png)

    **Packages.config** 파일 내용은 다음과 같습니다. 모든 NuGet 패키지는 전체 .NET Framework 4.7.2를 대상으로 하는 것을 확인할 수 있습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. **ContosoExpenses.Data** 프로젝트에서 **Packages.config** 파일을 삭제합니다.

4. **ContosoExpenses.Data** 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

  ![NuGet 패키지 관리...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭합니다. `Bogus` 패키지를 검색하고 안정적인 최신 버전을 설치합니다.

    ![Bogus NuGet 패키지](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB` 패키지를 검색하고 안정적인 최신 버전을 설치합니다.

    ![LiteDB NuGet 패키지](images/wpf-modernize-tutorial/LiteDB.png)

    프로젝트에 더 이상 packages.config 파일이 없으므로 이 NuGet 패키지 목록이 저장되는 위치가 궁금할 수도 있습니다. 참조된 NuGet 패키지는 .csproj 파일에 직접 저장됩니다. 확인하려면 텍스트 편집기에서 **ContosoExpenses.Data.csproj** 프로젝트 파일의 내용을 살펴봅니다. 파일 끝에 다음 줄이 추가된 것을 확인할 수 있습니다.

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > 이 .NET Core 3 프로젝트에 대해 .NET Framework 4.7.2 프로젝트에서 사용한 것과 동일한 패키지를 설치하는 것도 확인할 수 있습니다. NuGet 패키지는 멀티 타기팅을 지원합니다. 라이브러리 작성자는 각기 다른 아키텍처와 플랫폼에 대해 컴파일된 여러 버전의 라이브러리를 동일한 패키지에 포함할 수 있습니다. 해당 패키지는 .NET Core 3 프로젝트와 호환되는 전체 .NET Framework와 .NET Standard 2.0을 지원합니다. .NET Framework, .NET Core 및 .NET Standard의 차이점에 대한 자세한 내용은 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)을 참조하세요.

**ContosoExpenses.Core** 프로젝트의 NuGet 패키지를 구성하려면 다음을 수행합니다.

1. **ContosoExpenses.Core** 프로젝트에서 **packages.config** 파일을 엽니다. 현재, .NET Framework 4.7.2를 대상으로 하는 다음 참조가 포함되어 있습니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    다음 단계에서는 `MvvmLightLibs` 및 `Unity` 패키지의 .NET Standard 버전을 사용하겠습니다. 다른 두 항목은 두 라이브러리를 설치할 때 NuGet에서 자동으로 다운로드하는 종속성입니다.

2. **ContosoExpenses.Core** 프로젝트에서 **Packages.config** 파일을 삭제합니다.

3. **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

4. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭합니다. `Unity` 패키지를 검색하고 안정적인 최신 버전을 설치합니다.

    ![Unity 패키지](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10` 패키지를 검색하고 안정적인 최신 버전을 설치합니다. `MvvmLightLibs` 패키지의 .NET Standard 버전입니다. 이 패키지의 경우, 작성자가 라이브러리의 .NET Standard 버전을 .NET Framework 버전과 별도의 패키지로 패키징하도록 선택했습니다.

    ![MvvmLightsLibs 패키지](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. **ContosoExpenses.Core** 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.

7. **프로젝트 > 솔루션** 범주에서 **ContosoExpenses.Data**를 선택하고 **확인**을 클릭합니다.

    ![참조 추가](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>자동 생성된 어셈블리 특성 사용 안 함

마이그레이션 프로세스의 이 시점에서 **ContosoExpenses.Core** 프로젝트를 빌드하려고 하면 몇 가지 오류가 표시됩니다.

![.NET Core 3 빌드 새 오류](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

이 문제는 .NET Core 3.0에서 도입된 새 .csproj 형식이 **AssemblyInfo.cs** 파일이 아닌 프로젝트 파일에 어셈블리 정보를 저장하기 때문에 발생합니다. 오류를 해결하려면 이 동작을 사용하지 않도록 설정하고 프로젝트에서 **AssemblyInfo.cs** 파일을 계속 사용할 수 있게 합니다.

1. Visual Studio에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 언로드**를 선택합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭하고 **ContosoExpenses.Core.csproj 편집**을 선택합니다.

1. **PropertyGroup** 섹션에 다음 요소를 추가하고 파일을 저장합니다.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    이 요소를 추가한 후의 **PropertyGroup** 섹션은 다음과 같이 표시됩니다.

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 다시 로드**를 선택합니다.

4. **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 언로드**를 선택합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭하고 **ContosoExpenses.Data.csproj 편집**을 선택합니다.

5. **PropertyGroup** 섹션에 같은 항목을 추가하고 파일을 저장합니다.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    이 요소를 추가한 후의 **PropertyGroup** 섹션은 다음과 같이 표시됩니다.

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 다시 로드**를 선택합니다.

## <a name="add-the-windows-compatibility-pack"></a>Windows 호환 기능 팩 추가

이제 **ContosoExpenses.Core** 및 **ContosoExpenses.Data** 프로젝트를 컴파일하려고 하면, 이전 오류는 해결되었지만 **ContosoExpenses.Data** 라이브러리에 여전히 다음과 유사한 몇 가지 오류가 있는 것을 확인할 수 있습니다.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

해당 오류는 **ContosoExpenses.Data** 프로젝트를 .NET Framework 라이브러리(Windows용)에서 .NET Standard 라이브러리(Linux, Android, iOS 등을 비롯한 여러 플랫폼에서 실행 가능)로 변환한 경우에 발생합니다. **ContosoExpenses.Data** 프로젝트에는 Windows 전용 개념인 레지스트리와 상호 작용하는 **RegistryService**라는 클래스가 포함되어 있습니다.

오류를 해결하려면 [Windows 호환성](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 패키지를 설치합니다. 이 패키지는 .NET Standard 라이브러리에서 사용할 수 있는 많은 Windows 관련 API를 지원합니다. 이 패키지를 사용한 후에는 라이브러리가 더 이상 크로스 플랫폼이 아니지만 여전히 .NET Standard를 대상으로 합니다. 

1. **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
2. **NuGet 패키지 관리**를 선택합니다.
3. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭합니다. `Microsoft.Windows.Compatibility` 패키지를 검색하고 안정적인 최신 버전을 설치합니다.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 이제 **ContosoExpenses.Data** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **빌드**를 선택하여 프로젝트를 다시 컴파일합니다.

이번에는 빌드 프로세스가 오류 없이 완료됩니다.

## <a name="test-and-debug-the-migration"></a>마이그레이션 테스트 및 디버그

이제 프로젝트가 성공적으로 빌드되었으므로, 앱을 실행 및 테스트하여 런타임 오류가 있는지 확인할 수 있습니다.

1. **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

2. F5 키를 눌러 디버거에서 **ContosoExpenses.Core** 프로젝트를 시작합니다. 다음과 유사한 예외가 표시됩니다.

    ![Visual Studio에 표시되는 예외](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    이 예외는 마이그레이션 시작 시 .csproj 파일에서 콘텐츠를 삭제할 때 이미지 파일의 **빌드 작업** 정보를 제거했기 때문에 발생합니다. 이 문제는 다음 단계에서 해결하겠습니다.

3. 디버거를 중지합니다.

4. **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 언로드**를 선택합니다. 프로젝트를 다시 마우스 오른쪽 단추로 클릭하고 **ContosoExpenses.Core.csproj 편집**을 선택합니다.

5. 닫는 **Project** 요소 앞에 다음 항목을 추가합니다.

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **프로젝트 다시 로드**를 선택합니다.

7. Contoso.ico를 앱에 할당하려면 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. 열린 페이지에서 **아이콘** 아래의 드롭다운을 클릭하고 `Images\contoso.ico`를 선택합니다.

    ![프로젝트 속성의 Contoso 아이콘](images/wpf-modernize-tutorial/ContosoIco.png)

8. **Save**을 클릭합니다.

9. F5 키를 눌러 디버거에서 **ContosoExpenses.Core** 프로젝트를 시작합니다. 이제 앱이 실행되는지 확인합니다.

## <a name="next-steps"></a>다음 단계

지금까지 자습서를 진행했다면 Contoso Expenses 앱을 .NET Core 3으로 마이그레이션했습니다. 이제 [2부: XAML Islands를 사용하여 UWP InkCanvas 컨트롤 추가](modernize-wpf-tutorial-2.md)를 시작할 준비가 되었습니다.

> [!NOTE]
> 고해상도 화면을 사용하는 경우 앱이 매우 작게 표시될 수도 있습니다. 이 문제는 자습서의 다음 단계에서 해결하겠습니다.
