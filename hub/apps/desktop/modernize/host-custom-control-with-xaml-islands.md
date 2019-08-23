---
description: 이 문서에서는 XAML 아일랜드를 사용 하 여 WPF 앱에서 사용자 지정 UWP 컨트롤을 호스트 하는 방법을 보여 줍니다.
title: XAML 아일랜드를 사용 하 여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스팅
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml 제도, 사용자 지정 컨트롤, 사용자 컨트롤, 호스트 컨트롤
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9f8d9d56d8a05be7d22ea52c9b45908c246fc250
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643767"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>XAML 아일랜드를 사용 하 여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스팅

이 문서에서는 Windows Community Toolkit의 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용 하 여 .net Core 3을 대상으로 하는 WPF 앱에서 사용자 지정 UWP 컨트롤을 호스트 하는 방법을 보여 줍니다. 사용자 지정 컨트롤은 여러 개의 자사 UWP 컨트롤을 포함 하며 UWP 컨트롤 중 하나의 속성을 WPF 앱의 문자열에 바인딩합니다.

이 문서는 WPF 앱에서이 작업을 수행 하는 방법을 보여 주지만 Windows Forms 앱의 프로세스와 비슷합니다. WPF 및 Windows Forms apps에서 UWP 컨트롤을 호스트 하는 방법에 대 한 개요는 [이 문서](xaml-islands.md#wpf-and-windows-forms-applications)를 참조 하세요.

## <a name="overview"></a>개요

WPF 앱에서 사용자 지정 UWP 컨트롤을 호스트 하려면 다음 구성 요소가 필요 합니다. 이 문서에서는 이러한 각 구성 요소를 만드는 방법에 대 한 지침을 제공 합니다.

* **WPF 앱에 대 한 프로젝트 및 소스 코드**입니다. [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용 하 여 사용자 지정 UWP 컨트롤을 호스트 하는 것은 WPF 및 .net Core 3을 대상으로 하는 앱 Windows Forms 에서만 지원 됩니다. 이 시나리오는 .NET Framework를 대상으로 하는 앱에서 지원 되지 않습니다.

* **사용자 지정 UWP 컨트롤**입니다. 앱을 사용 하 여 컴파일할 수 있도록 호스트 하려는 사용자 지정 UWP 컨트롤에 대 한 소스 코드가 필요 합니다. 일반적으로 사용자 지정 컨트롤은 WPF (또는 Windows Forms) 프로젝트와 동일한 솔루션에서 참조 하는 UWP 클래스 라이브러리 프로젝트에서 정의 됩니다.

* **XamlApplication 개체를 정의 하는 UWP 앱 프로젝트**입니다. WPF (또는 Windows Forms) 프로젝트에는 Windows 커뮤니티 도구 키트에서 제공 하 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 는 클래스의 인스턴스에 대 한 액세스 권한이 있어야 합니다. 이 개체는 응용 프로그램의 현재 디렉터리에 있는 어셈블리의 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로 작동 합니다. 이 작업을 수행 하는 권장 방법은 WPF Windows Forms (유니버설 Windows) 프로젝트와 동일한 솔루션에 **빈 앱 (유니버설 Windows)** 프로젝트를 추가 하 고이 프로젝트에서 `App` 기본 클래스를 수정 하는 것입니다.
  > [!NOTE]
  > 솔루션에는 개체를 `XamlApplication` 정의 하는 프로젝트가 하나만 포함 될 수 있습니다. 앱의 모든 사용자 지정 UWP 컨트롤은 같은 `XamlApplication` 개체를 공유 합니다. 개체를 `XamlApplication` 정의 하는 프로젝트에는 XAML 아일랜드에서 uwp 컨트롤을 호스트 하는 데 사용 되는 다른 모든 UWP 라이브러리 및 프로젝트에 대 한 참조가 포함 되어야 합니다.

## <a name="create-a-wpf-project"></a>WPF 프로젝트 만들기

시작 하기 전에 다음 지침에 따라 WPF 프로젝트를 만들고 XAML 아일랜드를 호스팅하도록 구성 합니다. 기존 WPF 프로젝트가 있는 경우 프로젝트에 대 한 이러한 단계 및 코드 예제를 적용할 수 있습니다.

> [!NOTE]
> .NET Framework를 대상으로 하는 기존 프로젝트가 있는 경우 프로젝트를 .NET Core 3으로 마이그레이션해야 합니다. 자세한 내용은 [이 블로그 시리즈](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)를 참조 하세요.

1. 아직 수행 하지 않은 경우 사용 가능한 최신 미리 보기 버전의 [.Net Core 3 PREVIEW SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)를 설치 합니다.

2. Visual Studio 2019에서 새 **WPF 앱 (.Net Core)** 프로젝트를 만듭니다.

3. [패키지 참조](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) 를 사용할 수 있는지 확인 합니다.

    1. Visual Studio에서 **도구-> NuGet 패키지 관리자-> 패키지 관리자 설정**을 클릭 합니다.
    2. **기본 패키지 관리 형식**에 대해 **PackageReference** 이 선택 되어 있는지 확인 합니다.

4. **솔루션 탐색기** 에서 WPF 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.

5. **NuGet 패키지 관리자** 창에서 **시험판 포함** 이 선택 되어 있는지 확인 합니다.

6. **찾아보기** 탭을 선택 하 고 6.0.0 [호스트](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) 패키지 (버전 v-preview7 이상)를 검색 한 다음 패키지를 설치 합니다. 이 패키지는 다른 관련 NuGet 패키지를 포함 하 여 UWP 컨트롤을 호스트 하기 위해 **Windowsxamlhost** 컨트롤을 사용 하는 데 필요한 모든 것을 제공 합니다.
    > [!NOTE]
    > Windows Forms 앱은 6.0.0 [호스트](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) 패키지 (버전 v-preview7 이상)를 사용 해야 합니다.

7. X86 또는 x64와 같은 특정 플랫폼을 대상으로 하도록 솔루션을 구성 합니다. 사용자 지정 UWP 컨트롤은 **모든 CPU**를 대상으로 하는 프로젝트에서 지원 되지 않습니다.

    1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **속성** -> **구성 속성** -> **Configuration Manager**을 선택 합니다. 
    2. **활성 솔루션 플랫폼**에서 **새로 만들기**를 선택 합니다. 
    3. **새 솔루션 플랫폼** 대화 상자에서 **x64** 또는 **x 86** 을 선택 하 고 **확인**을 누릅니다. 
    4. 열기 대화 상자를 닫습니다.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>UWP 앱 프로젝트에서 XamlApplication 개체 만들기

다음으로, WPF 프로젝트와 동일한 솔루션에 UWP 앱 프로젝트를 추가 합니다. Windows 커뮤니티 도구 키트에서 `App` 제공 하는 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 클래스에서 파생 되도록이 프로젝트의 기본 클래스를 수정 합니다. WPF 앱의 **windowsxamlhost** 개체는 사용자 지정 UWP `XamlApplication` 컨트롤을 호스트 하는 데이 개체가 필요 합니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고**새 프로젝트** **추가** -> 를 선택 합니다.
2. 솔루션에 **빈 앱(Universal Windows)** 프로젝트 추가 대상 버전 및 최소 버전이 모두 **Windows 10 버전 1903** 이상으로 설정 되어 있는지 확인 합니다.
3. UWP 앱 프로젝트에서 [Microsoft 6.0.0 application](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 패키지 (버전 v-preview7 이상)를 설치 합니다.
4. App.xaml 파일 을 열고이 파일의 내용을 다음 xaml로 바꿉니다. 을 `MyUWPApp` UWP 앱 프로젝트의 네임 스페이스로 바꿉니다.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. **App.xaml.cs** 파일을 열고이 파일의 내용을 다음 코드로 바꿉니다. 을 `MyUWPApp` UWP 앱 프로젝트의 네임 스페이스로 바꿉니다.

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. UWP 앱 프로젝트에서 **Mainpage .xaml** 파일을 삭제 합니다.
7. UWP 앱 프로젝트를 빌드합니다.
8. WPF 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭 하 고 UWP 앱 프로젝트에 대 한 참조를 추가 합니다.

## <a name="create-a-custom-uwp-control"></a>사용자 지정 UWP 컨트롤 만들기

WPF 앱에서 사용자 지정 UWP 컨트롤을 호스팅하려면 앱을 사용 하 여 컴파일할 수 있도록 컨트롤에 대 한 소스 코드가 있어야 합니다. 일반적으로 사용자 지정 컨트롤은 간편한 이식성을 위해 UWP 클래스 라이브러리 프로젝트에서 정의 됩니다.

이 섹션에서는 새 클래스 라이브러리 프로젝트에서 간단한 사용자 지정 UWP 컨트롤을 정의 합니다. 또는 이전 섹션에서 만든 UWP 앱 프로젝트에서 사용자 지정 UWP 컨트롤을 정의할 수 있습니다. 그러나 이러한 단계에서는 설명 목적으로 별도의 클래스 라이브러리 프로젝트에서이 작업을 수행 합니다 .이는 일반적으로 사용자 지정 컨트롤을 이식성에 맞게 구현 하는 방법입니다. 

사용자 지정 컨트롤이 이미 있는 경우 여기에 표시 된 컨트롤 대신 사용할 수 있습니다. 그러나 다음 단계에 표시 된 것 처럼 컨트롤을 포함 하는 프로젝트를 구성 해야 합니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고**새 프로젝트** **추가** -> 를 선택 합니다.
2. **클래스 라이브러리 (유니버설 Windows)** 프로젝트를 솔루션에 추가 합니다. 대상 버전 및 최소 버전이 모두 **Windows 10 버전 1903** 이상으로 설정 되어 있는지 확인 합니다.
3. 프로젝트 파일을 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다. 프로젝트 파일을 다시 마우스 오른쪽 단추로 클릭 하 고 **편집**을 선택 합니다.
4. 닫기 `</Project>` 요소 앞에 다음 XML을 추가 하 여 여러 속성을 사용 하지 않도록 설정한 다음 프로젝트 파일을 저장 합니다. WPF (또는 Windows Forms) 앱에서 사용자 지정 UWP 컨트롤을 호스트 하려면 이러한 속성을 사용 하도록 설정 해야 합니다.

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. 프로젝트 파일을 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.
6. 기본 **Class1.cs** 파일을 삭제 하 고 프로젝트에 새 **사용자 정의 컨트롤** 항목을 추가 합니다.
7. 사용자 정의 컨트롤에 대 한 XAML 파일에서 다음 `StackPanel` 을 기본값 `Grid`의 자식으로 추가 합니다. 이 예제에서는 컨트롤 ``TextBlock`` 을 추가한 다음 해당 컨트롤 ``Text`` ``XamlIslandMessage`` 의 특성을 필드에 바인딩합니다.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. 사용자 정의 컨트롤에 대 한 코드 숨겨진 파일에서 아래와 같이 `XamlIslandMessage` 사용자 정의 컨트롤 클래스에 필드를 추가 합니다.

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. UWP 클래스 라이브러리 프로젝트를 빌드합니다.
10. WPF 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭 하 고 UWP 클래스 라이브러리 프로젝트에 대 한 참조를 추가 합니다.
11. 이전에 구성한 UWP 앱 프로젝트에서 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 UWP 클래스 라이브러리 프로젝트에 대 한 참조를 추가 합니다.
12. 전체 솔루션을 다시 빌드하고 모든 프로젝트가 성공적으로 빌드 되었는지 확인 합니다.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>WPF 앱에서 사용자 지정 UWP 컨트롤 호스트

1. **솔루션 탐색기**에서 WPF 프로젝트를 확장 하 고 사용자 지정 컨트롤을 호스트 하려는 mainwindow.xaml 파일 또는 다른 창을 엽니다.
2. XAML 파일에서 다음 네임 스페이스 선언을 `<Window>` 요소에 추가 합니다.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 동일한 파일에서 다음 컨트롤을 `<Grid>` 요소에 추가 합니다. 특성을 `InitialTypeName` UWP 클래스 라이브러리 프로젝트에 있는 사용자 정의 컨트롤의 정규화 된 이름으로 변경 합니다.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 코드 숨겨진 파일을 열고 `Window` 클래스에 다음 코드를 추가 합니다. 이 코드는 UWP `ChildChanged` 사용자 지정 컨트롤의 ``XamlIslandMessage`` 필드 값 `WPFMessage` 을 WPF 앱의 필드 값에 할당 하는 이벤트 처리기를 정의 합니다. UWP `UWPClassLibrary.MyUserControl` 클래스 라이브러리 프로젝트에서 사용자 정의 컨트롤의 정규화 된 이름으로 변경 합니다.

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. 앱을 빌드 및 실행 하 고 UWP 사용자 정의 컨트롤이 예상 대로 표시 되는지 확인 합니다.

## <a name="package-the-app"></a>앱 패키지

필요에 따라 배포를 위해 [Msix 패키지](https://docs.microsoft.com/windows/msix) 에 WPF 앱을 패키지할 수 있습니다. MSIX은 Windows 용 최신 앱 패키징 기술 이며 MSI, APPX, App-v 및 ClickOnce 설치 기술의 조합을 기반으로 합니다.

다음 지침에서는 Visual Studio 2019의 [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 사용 하 여 msix 패키지의 솔루션에 있는 모든 구성 요소를 패키지 하는 방법을 보여 줍니다. 이러한 단계는 MSIX 패키지에서 WPF 앱을 패키징하는 경우에만 필요 합니다. 이러한 단계에는 현재 사용자 지정 UWP 컨트롤을 호스트 하는 시나리오와 관련 된 몇 가지 해결 방법이 포함 되어 있습니다.

1. 새 [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 솔루션에 추가 합니다. 프로젝트를 만들 때 **Windows 10, 버전 1903 (10.0;)을 선택 합니다. 빌드 18362)** 는 **대상 버전** 및 **최소 버전**모두에 해당 합니다.

2. 패키징 프로젝트에서 **응용 프로그램** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다. 프로젝트 목록에서 솔루션의 WPF 프로젝트를 선택 하 고 **확인**을 클릭 합니다.

3. 패키징 프로젝트 파일을 편집 합니다. 이러한 변경은 현재 .NET Core 3을 대상으로 하 고 XAML 아일랜드를 호스트 하는 WPF 앱을 패키징하는 데 필요 합니다.

    1. 솔루션 탐색기에서 패키징 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 파일 편집**을 선택 합니다.
    2. 파일에서 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 요소를 찾습니다. 이 요소를 다음 XML로 바꿉니다. 이러한 변경은 현재 .NET Core 3을 대상으로 하 고 UWP 컨트롤을 호스트 하는 WPF 앱을 패키징하는 데 필요 합니다.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject>
                </SourceProject>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. 프로젝트 파일을 저장하고 닫습니다.

4. 올바른 기본 시작 화면 이미지를 참조 하도록 패키지 매니페스트를 편집 합니다. 이 해결 방법은 현재 사용자 지정 UWP 컨트롤을 호스트 하는 WPF 앱을 패키징하는 데 필요 합니다.

    1. 패키징 프로젝트에서 **appxmanifest.xml** 파일을 마우스 오른쪽 단추로 클릭 하 고 **코드 보기**를 클릭 합니다.
    2. 파일에서 다음 요소를 찾습니다.

        ```<uap:SplashScreen Image="Images\SplashScreen.png" />```

    3. 이 요소를 다음으로 변경 합니다.

        ```<uap:SplashScreen Image="Images\SplashScreen.scale-200.png" />```

    4. **Appxmanifest.xml** 파일을 저장 하 고 닫습니다.

5. WPF 프로젝트 파일을 편집 합니다. 이러한 변경 내용은 현재 사용자 지정 UWP 컨트롤을 호스트 하는 WPF 앱을 패키징하는 데 필요 합니다.

    1. 솔루션 탐색기에서 WPF 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드**를 선택 합니다.
    2. WPF 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **편집**을 선택 합니다.
    3. 파일에서 마지막 `</PropertyGroup>` 닫는 태그를 찾아 해당 태그 바로 뒤에 다음 XML을 추가 합니다.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. 프로젝트 파일을 저장하고 닫습니다.
    5. WPF 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 다시 로드**를 선택 합니다.

6. 패키징 프로젝트를 빌드하고 실행 합니다. WPF가 실행 되 고 UWP 사용자 지정 컨트롤이 예상 대로 표시 되는지 확인 합니다.

## <a name="related-topics"></a>관련 항목

* [데스크톱 응용 프로그램의 UWP 컨트롤](xaml-islands.md)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)