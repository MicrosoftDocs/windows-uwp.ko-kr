---
description: 이 문서에서는 XAML Islands를 사용하여 WPF 앱에서 사용자 지정 WinRT XAML 컨트롤을 호스팅하는 방법을 보여 줍니다.
title: XAML Islands를 사용하여 WPF 앱에서 사용자 지정 WinRT XAML 컨트롤 호스팅
ms.date: 10/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, 사용자 지정 컨트롤, 사용자 정의 컨트롤, 호스트 컨트롤
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: cdfdf9b7396943e3ee5345249f38a35d48beb128
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253621"
---
# <a name="host-a-custom-winrt-xaml-control-in-a-wpf-app-using-xaml-islands"></a>XAML Islands를 사용하여 WPF 앱에서 사용자 지정 WinRT XAML 컨트롤 호스팅

이 문서에서는 Windows 커뮤니티 도구 키트에서 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 .NET Core 3.1을 대상으로 하는 WPF 앱에서 사용자 지정 WinRT XAML 컨트롤을 호스팅하는 방법을 보여 줍니다. 사용자 지정 컨트롤은 Windows SDK의 여러 자사 컨트롤을 포함하고, WinRT XAML 컨트롤 중 하나의 속성을 WPF 앱의 문자열에 바인딩합니다. 또한 이 문서에서는 [WinUI 라이브러리](/uwp/toolkits/winui/)에서 컨트롤을 호스팅하는 방법도 보여 줍니다.

이 문서는 WPF 앱에서 이 작업을 수행하는 방법을 보여 주지만 Windows Forms 앱의 프로세스와 비슷합니다. WPF 및 Windows Forms 앱에서 WinRT XAML 컨트롤을 호스팅하는 방법에 대한 개요는 [이 문서](xaml-islands.md#wpf-and-windows-forms-applications)를 참조하세요.

## <a name="required-components"></a>필수 구성 요소

WPF(또는 Windows Forms) 앱에서 사용자 지정 WinRT XAML 컨트롤을 호스팅하려면 솔루션에 다음 구성 요소가 필요합니다. 이 문서에서는 이러한 각 구성 요소를 만드는 방법에 대한 지침을 제공합니다.

* **앱에 대한 프로젝트 및 소스 코드**. [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 사용자 지정 컨트롤을 호스팅하는 것은 .NET Core 3.x를 대상으로 하는 앱에서만 지원됩니다. 이 시나리오는 .NET Framework를 대상으로 하는 앱에서 지원되지 않습니다.

* **사용자 지정 WinRT XAML 컨트롤**. 앱을 사용하여 컴파일할 수 있도록 호스팅하려는 사용자 지정 컨트롤에 대한 소스 코드가 있어야 합니다. 일반적으로 사용자 지정 컨트롤은 WPF 또는 Windows Forms 프로젝트와 동일한 솔루션에서 참조하는 UWP 클래스 라이브러리 프로젝트에서 정의됩니다.

* **XamlApplication에서 파생되는 루트 Application 클래스를 정의하는 UWP 앱 프로젝트**. WPF 또는 Windows Forms 프로젝트는 사용자 지정 UWP XAML 컨트롤을 검색하고 로드할 수 있도록 Windows 커뮤니티 도구 키트에서 제공하는 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스의 인스턴스에 액세스할 수 있어야 합니다. 이 작업을 수행하는 권장 방법은 WPF 또는 Windows Forms 앱에 대한 솔루션의 일부인 별도의 UWP 앱 프로젝트에서 이 개체를 정의하는 것입니다. 

    > [!NOTE]
    > 솔루션은 `XamlApplication` 개체를 정의하는 프로젝트를 하나만 포함할 수 있습니다. 앱의 모든 사용자 지정 WinRT XAML 컨트롤은 동일한 `XamlApplication` 개체를 공유합니다. `XamlApplication` 개체를 정의하는 프로젝트에는 XAML Island에서 컨트롤에 호스팅하는 데 사용되는 다른 모든 WinRT 라이브러리 및 프로젝트에 대한 참조가 포함되어야 합니다.

## <a name="create-a-wpf-project"></a>WPF 프로젝트 만들기

시작하기 전에 다음 지침에 따라 WPF 프로젝트를 만들고 XAML Islands를 호스트하도록 구성합니다. 기존 WPF 프로젝트가 있는 경우 프로젝트에 대해 이러한 단계 및 코드 예제를 적용할 수 있습니다.

> [!NOTE]
> .NET Framework를 대상으로 하는 기존 프로젝트가 있는 경우 프로젝트를 .NET Core 3.1로 마이그레이션해야 합니다. 자세한 내용은 [이 블로그 시리즈](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)를 참조하세요.

1. 아직 설치하지 않은 경우 최신 버전의 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/current)를 설치해야 합니다.

2. Visual Studio 2019에서 새 **WPF 앱(.NET Core)** 프로젝트를 만듭니다.

3. [패키지 참조](/nuget/consume-packages/package-references-in-project-files)를 사용하도록 설정했는지 확인합니다.

    1. Visual Studio에서 **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 설정**을 클릭합니다.
    2. **기본 패키지 관리 형식**에 대해 **PackageReference**를 선택했는지 확인합니다.

4. **솔루션 탐색기**에서 WPF 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

5. **찾아보기** 탭을 선택하고, [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) 패키지를 검색하여 안정적인 최신 버전을 설치합니다. 이 패키지는 다른 관련 NuGet 패키지를 포함하여 **WindowsXamlHost** 컨트롤을 통해 WinRT XAML 컨트롤을 호스팅하는 데 필요한 모든 요소를 제공합니다.
    > [!NOTE]
    > Windows Forms 앱은 [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) 패키지를 사용해야 합니다.

6. x86 또는 x64와 같은 특정 플랫폼을 대상으로 하도록 솔루션을 구성합니다. 사용자 지정 WinRT XAML 컨트롤은 **모든 CPU**를 대상으로 하는 프로젝트에서 지원되지 않습니다.

    1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 **속성** -> **구성 속성** -> **Configuration Manager**를 선택합니다.
    2. **활성 솔루션 플랫폼**에서 **새로 만들기**를 선택합니다. 
    3. **새 솔루션 플랫폼** 대화 상자에서 **x64** 또는 **x86**을 선택하고 **확인**을 누릅니다. 
    4. 열기 대화 상자를 닫습니다.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>UWP 앱 프로젝트에서 XamlApplication 클래스 정의

그런 다음, UWP 앱 프로젝트를 솔루션에 추가하고 이 프로젝트의 기본 `App` 클래스를 수정하여 Windows 커뮤니티 도구 키트에서 제공하는 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스에서 파생하도록 합니다. 이 클래스는 [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider) 인터페이스를 지원합니다. 이 인터페이스를 통해 앱은 런타임 시 애플리케이션의 현재 디렉터리에 있는 어셈블리의 사용자 지정 UWP XAML 컨트롤에 대한 메타데이터를 검색하고 로드할 수 있습니다. 이 클래스는 또한 현재 스레드에 대한 UWP XAML 프레임워크를 초기화합니다. 

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 프로젝트**를 선택합니다.
2. 솔루션에 **빈 앱(유니버설 Windows)** 프로젝트를 추가합니다. 대상 버전 및 최소 버전이 모두 **Windows 10 버전 1903(빌드 18362)** 이상 릴리스로 설정되어 있는지 확인합니다.
3. UWP 앱 프로젝트에서 [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 패키지(안정적인 최신 버전)를 설치합니다.
4. **App.xaml** 파일을 열고 이 파일의 내용을 다음 XAML로 바꿉니다. `MyUWPApp`을 UWP 앱 프로젝트의 네임스페이스로 바꿉니다.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. **App.xaml.cs** 파일을 열고 이 파일의 내용을 다음 코드로 바꿉니다. `MyUWPApp`을 UWP 앱 프로젝트의 네임스페이스로 바꿉니다.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. UWP 앱 프로젝트에서 **MainPage.xaml** 파일을 삭제합니다.
7. UWP 앱 프로젝트를 정리한 후 빌드합니다.

## <a name="add-a-reference-to-the-uwp-project-in-your-wpf-project"></a>WPF 프로젝트에서 UWP 프로젝트에 대한 참조 추가

1. WPF 프로젝트 파일에서 호환되는 프레임워크 버전을 지정합니다. 

    1. **솔루션 탐색기**에서 WPF 프로젝트 노드를 두 번 클릭하여 편집기에서 프로젝트 파일을 엽니다.
    2. 첫 번째 **PropertyGroup** 요소에서 다음 자식 요소를 추가합니다. 필요에 따라 UWP 프로젝트의 대상 및 최소 OS 빌드와 일치하도록 값의 `19041` 부분을 변경합니다.

        ```xml
        <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        ```

        완료되면 **PropertyGroup** 요소가 다음과 같이 표시됩니다.

        ```xml
        <PropertyGroup>
            <OutputType>WinExe</OutputType>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UseWPF>true</UseWPF>
            <Platforms>AnyCPU;x64</Platforms>
            <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        </PropertyGroup>
        ```

2. **솔루션 탐색기**의 WPF 프로젝트 아래에서 마우스 오른쪽 단추로 **종속성** 노드를 클릭하고, UWP 앱 프로젝트에 대한 참조를 추가합니다.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>WPF 앱의 진입점에서 XamlApplication 개체 인스턴스화

다음으로, WPF 앱의 진입점에 코드를 추가하여 UWP 프로젝트에서 방금 정의한 `App` 클래스(현재 `XamlApplication`에서 파생되는 클래스)의 인스턴스를 만듭니다.

1. WPF 프로젝트에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 항목**을 선택하고 **클래스**를 선택합니다. 클래스 이름을 **Program**으로 지정하고 **추가**를 클릭합니다.

2. 생성된 `Program` 클래스를 다음 코드로 바꾸고 파일을 저장합니다. `MyUWPApp`을 UWP 앱 프로젝트의 네임스페이스로 바꾸고 `MyWPFApp`을 WPF 앱 프로젝트의 네임스페이스로 바꿉니다.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

4. 속성의 **애플리케이션** 탭에서 **시작 개체** 드롭다운을 클릭하고 이전 단계에서 추가한 `Program` 클래스의 정규화된 이름을 선택합니다. 
    > [!NOTE]
    > 기본적으로 WPF 프로젝트는 수정하지 않으려는 생성된 코드 파일에서 `Main` 진입점 함수를 정의합니다. 이 단계에서는 프로젝트에 대한 진입점을 새 `Program` 클래스의 `Main` 메서드로 변경하여 앱의 시작 프로세스 초기에 가능한 한 빨리 실행되는 코드를 추가할 수 있습니다. 

5. 프로젝트 속성에 대한 변경 내용을 저장합니다.

## <a name="create-a-custom-winrt-xaml-control"></a>사용자 지정 WinRT XAML 컨트롤 만들기

WPF 앱에서 사용자 지정 WinRT XAML 컨트롤을 호스팅하려면 앱을 사용하여 컴파일할 수 있도록 컨트롤에 대한 소스 코드가 있어야 합니다. 일반적으로 사용자 지정 컨트롤은 간편한 이식성을 위해 UWP 클래스 라이브러리 프로젝트에서 정의됩니다.

이 섹션에서는 새 클래스 라이브러리 프로젝트에서 간단한 사용자 지정 컨트롤을 정의합니다. 또는 이전 섹션에서 만든 UWP 앱 프로젝트에서 사용자 지정 UWP 컨트롤을 정의할 수 있습니다. 그러나 이러한 단계에서는 설명 목적으로 별도의 클래스 라이브러리 프로젝트에서 이 작업을 수행합니다. 이 방식이 일반적으로 사용자 지정 컨트롤을 이식성에 맞게 구현하는 방법이기 때문입니다.

사용자 지정 컨트롤이 이미 있는 경우 여기에 표시된 컨트롤 대신 사용할 수 있습니다. 그러나 다음 단계에 표시된 것처럼 컨트롤을 포함하는 프로젝트를 구성해야 합니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 프로젝트**를 선택합니다.
2. 솔루션에 **클래스 라이브러리(유니버설 Windows)** 프로젝트를 추가합니다. 대상 버전과 최소 버전이 모두 UWP 프로젝트와 동일한 대상 및 최소 OS 빌드로 설정되어 있는지 확인합니다.
3. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **프로젝트 언로드**를 선택합니다. 프로젝트 파일을 다시 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다.
4. 닫는 `</Project>` 요소 앞에 다음 XML을 추가하여 여러 속성을 사용하지 않도록 설정한 다음, 프로젝트 파일을 저장합니다. WPF(또는 Windows Forms) 앱에서 사용자 지정 컨트롤을 호스팅하려면 이러한 속성을 사용하도록 설정해야 합니다.

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **프로젝트 다시 로드**를 선택합니다.
6. 기본 **Class1.cs** 파일을 삭제하고 프로젝트에 새 **사용자 정의 컨트롤** 항목을 추가합니다.
7. 사용자 정의 컨트롤에 대한 XAML 파일에서 다음 `StackPanel`을 기본 `Grid`의 자식으로 추가합니다. 이 예제에서는 ``TextBlock`` 컨트롤을 추가한 다음, 해당 컨트롤의 ``Text`` 특성을 ``XamlIslandMessage`` 필드에 바인딩합니다.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom WinRT XAML control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. 사용자 정의 컨트롤에 대한 코드 숨김 파일에서 아래와 같이 사용자 정의 컨트롤 클래스에 `XamlIslandMessage` 필드를 추가합니다.

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
10. WPF 프로젝트에서 **종속성** 노드를 마우스 오른쪽 단추로 클릭하고 UWP 클래스 라이브러리 프로젝트에 대한 참조를 추가합니다.
11. 이전에 구성한 UWP 앱 프로젝트에서 **참조** 노드를 마우스 오른쪽 단추로 클릭하고 UWP 클래스 라이브러리 프로젝트에 대한 참조를 추가합니다.
12. 전체 솔루션을 다시 빌드하고 모든 프로젝트가 성공적으로 빌드되었는지 확인합니다.

## <a name="host-the-custom-winrt-xaml-control-in-your-wpf-app"></a>WPF 앱에서 사용자 지정 WinRT XAML 컨트롤 호스팅

1. **솔루션 탐색기**에서 WPF 프로젝트를 확장하고 사용자 지정 컨트롤을 호스트하려는 Mainwindow.xaml 파일 또는 다른 창을 엽니다.
2. XAML 파일에서 `<Window>` 요소에 다음 네임스페이스 선언을 추가합니다.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. 동일한 파일에서 `<Grid>` 요소에 다음 컨트롤을 추가합니다. `InitialTypeName` 특성을 UWP 클래스 라이브러리 프로젝트에 있는 사용자 정의 컨트롤의 정규화된 이름으로 변경합니다.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. 코드 숨김 파일을 열고 `Window` 클래스에 다음 코드를 추가합니다. 이 코드는 UWP 사용자 정의 컨트롤의 ``XamlIslandMessage`` 필드 값을 WPF 앱의 `WPFMessage` 필드 값에 할당하는 `ChildChanged` 이벤트 처리기를 정의합니다. `UWPClassLibrary.MyUserControl`을 UWP 클래스 라이브러리 프로젝트에 있는 사용자 정의 컨트롤의 정규화된 이름으로 변경합니다.

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

6. 앱을 빌드 및 실행하고 UWP 사용자 정의 컨트롤이 예상대로 표시되는지 확인합니다.

## <a name="add-a-control-from-the-winui-2x-library-to-the-custom-control"></a>WinUI 2.x 라이브러리의 컨트롤을 사용자 지정 컨트롤에 추가

일반적으로 WinRT XAML 컨트롤은 Windows 10 OS의 일부로 릴리스되었으며 개발자가 Windows SDK를 통해 사용할 수 있게 되었습니다. [WinUI 라이브러리](/uwp/toolkits/winui/)는 Windows SDK에서 WinRT XAML 컨트롤의 업데이트된 버전이 Windows SDK 릴리스에 연결되지 않은 NuGet 패키지에 배포되는 대체 방법입니다. 이 라이브러리에는 Windows SDK 및 기본 UWP 플랫폼에 속하지 않는 새 컨트롤도 포함되어 있습니다. 자세한 내용은 [WinUI 라이브러리 로드맵](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)을 참조하세요.

이 섹션에서는 WinUI 2.x 라이브러리의 WinRT XAML 컨트롤을 사용자 컨트롤에 추가하는 방법을 보여줍니다.

> [!NOTE]
> 현재 XAML Islands는 WinUI 2.x 라이브러리의 호스팅 컨트롤만 지원합니다. WinUI 3 라이브러리의 호스팅 컨트롤에 대한 지원은 이후 릴리스에서 제공됩니다.

1. UWP 앱 프로젝트에서 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 패키지의 최신 릴리스 또는 시험판을 설치합니다.

    > [!NOTE]
    > 데스크톱 앱이 [MSIX 패키지](/windows/msix)에 패키지된 경우 시험판 또는 릴리스 버전의 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NugGet 패키지를 사용할 수 있습니다. 데스크톱 앱이 MSIX를 사용하여 패키지되지 않은 경우 시험판 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 패키지를 설치해야 합니다.

2. 이 프로젝트의 App.xaml 파일에서 다음 자식 요소를 `<xaml:XamlApplication>` 요소에 추가합니다.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    이 요소를 추가한 후에는 이 파일의 내용이 다음과 같이 표시됩니다.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. UWP 클래스 라이브러리 프로젝트에서 최신 버전의 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 패키지(UWP 앱 프로젝트에 설치한 것과 동일한 버전)를 설치합니다.

4. 동일한 프로젝트에서 사용자 정의 컨트롤에 대한 XAML 파일을 열고 `<UserControl>` 요소에 다음 네임스페이스 선언을 추가합니다.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. 동일한 파일에서 `<winui:RatingControl />` 요소를 `<StackPanel>`의 자식으로 추가합니다. 이 요소는 WinUI 라이브러리에서 [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) 클래스의 인스턴스를 추가합니다. 이 요소를 추가한 후 `<StackPanel>`은 다음과 같이 표시됩니다.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom WinRT XAML control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. 앱을 빌드 및 실행하고 새 등급 컨트롤이 예상대로 표시되는지 확인합니다.

## <a name="package-the-app"></a>앱 패키지

필요에 따라 배포를 위해 [MSIX 패키지](/windows/msix)에 WPF 앱을 패키지할 수 있습니다. MSIX는 Windows용 최신 앱 패키징 기술로, MSI, .appx, App-V 및 ClickOnce 설치 기술의 조합을 기준으로 합니다.

다음 지침에서는 Visual Studio 2019의 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 사용하여 솔루션에 있는 모든 구성 요소를 MSIX 패키지에 패키지하는 방법을 보여 줍니다. 이러한 단계는 MSIX 패키지에서 WPF 앱을 패키지하는 경우에만 필요합니다. 

> [!NOTE]
> 배포를 위해 [MSIX 패키지](/windows/msix)에 애플리케이션을 패키지하지 않도록 선택하는 경우 앱을 실행하는 컴퓨터에 [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)이 설치되어 있어야 합니다.

1. 새 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 솔루션에 추가합니다. 프로젝트를 만들 때 UWP 프로젝트에 대해 선택한 것과 동일한 **대상 버전** 및 **최소 버전**을 선택합니다.

2. 패키징 프로젝트에서 **애플리케이션** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다. 프로젝트 목록에서 솔루션의 WPF 프로젝트를 선택하고 **확인**을 클릭합니다.

3. 패키징 프로젝트를 빌드하고 실행합니다. WPF가 실행되고 UWP 사용자 지정 컨트롤이 예상대로 표시되는지 확인합니다.

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)
* [XAML Islands 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)