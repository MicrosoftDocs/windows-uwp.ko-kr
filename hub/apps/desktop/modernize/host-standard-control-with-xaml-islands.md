---
description: 이 문서에서는 XAML Islands를 사용하여 WPF 앱에서 표준 UWP 컨트롤을 호스트하는 방법을 보여 줍니다.
title: XAML Islands를 사용하여 WPF 앱에서 표준 UWP 컨트롤 호스트
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, 래핑된 컨트롤, 표준 컨트롤, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 1a2b5722ab836e715bce1b4c94fab97e6a28646e
ms.sourcegitcommit: cee2060bfc8489236e00e5951751bcc5bd632b0a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84614905"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>XAML Islands를 사용하여 WPF 앱에서 표준 UWP 컨트롤 호스트

이 문서에서는 [XAML Islands](xaml-islands.md)를 사용하여 WPF 앱에서 표준 UWP 컨트롤(즉, Windows SDK에서 제공하는 자사 UWP 컨트롤)을 호스트하는 두 가지 방법을 보여 줍니다.

* Windows 커뮤니티 도구 키트의 [래핑된 컨트롤](xaml-islands.md#wrapped-controls)을 사용하여 UWP [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 컨트롤을 호스트하는 방법을 보여 줍니다. 이러한 컨트롤은 몇 가지 유용한 UWP 컨트롤 세트의 인터페이스 및 기능을 래핑합니다. WPF 또는 Windows Forms 프로젝트의 디자인 화면에 이러한 컨트롤을 바로 추가하고 디자이너에서 다른 WPF 또는 Windows Forms 컨트롤처럼 사용할 수 있습니다.

* 또한 Windows 커뮤니티 도구 키트의 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 컨트롤을 호스트하는 방법을 보여 줍니다. 소수의 UWP 컨트롤 세트만 래핑된 컨트롤로 사용할 수 있기 때문에 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)를 사용하여 다른 표준 UWP 컨트롤을 호스트할 수 있습니다.

이 문서는 WPF 앱에서 UWP 컨트롤을 호스트하는 방법을 보여 주지만 Windows Forms 앱의 프로세스와 비슷합니다.

## <a name="required-components"></a>필수 구성 요소

WPF(또는 Windows Forms) 앱에서 UWP 컨트롤을 호스트하려면 솔루션에 다음 구성 요소가 필요합니다. 이 문서에서는 이러한 각 구성 요소를 만드는 방법에 대한 지침을 제공합니다.

* **앱에 대한 프로젝트 및 소스 코드**. [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 표준 자사 UWP 컨트롤을 호스트하는 것은 .NET Framework 또는 .NET Core 3를 대상으로 하는 앱에서 지원됩니다.

* **XamlApplication에서 파생되는 루트 Application 클래스를 정의하는 UWP 앱 프로젝트**. WPF 또는 Windows Forms 프로젝트는 사용자 지정 UWP XAML 컨트롤을 검색하고 로드할 수 있도록 Windows 커뮤니티 도구 키트에서 제공하는 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스의 인스턴스에 액세스할 수 있어야 합니다. 이 작업을 수행하는 권장 방법은 WPF 또는 Windows Forms 앱에 대한 솔루션의 일부인 별도의 UWP 앱 프로젝트에서 이 개체를 정의하는 것입니다. 

    > [!NOTE]
    > `XamlApplication` 개체가 자사 UWP 컨트롤을 호스트하는 데 필요하지는 않지만 사용자 지정 UWP 컨트롤 호스트를 포함하여 모든 범위의 XAML Island 시나리오를 지원하려면 앱에 이 개체가 필요합니다. 따라서 XAML Islands를 사용하는 모든 솔루션에서 항상 `XamlApplication` 개체를 정의하는 것이 좋습니다.

    > [!NOTE]
    > 솔루션은 `XamlApplication` 개체를 정의하는 프로젝트를 하나만 포함할 수 있습니다. 앱의 모든 사용자 지정 UWP 컨트롤은 동일한 `XamlApplication` 개체를 공유합니다. `XamlApplication` 개체를 정의하는 프로젝트에는 XAML Islands에 UWP 컨트롤을 호스트하는 데 사용되는 다른 모든 UWP 라이브러리 및 프로젝트에 대한 참조가 포함되어야 합니다.

## <a name="create-a-wpf-project"></a>WPF 프로젝트 만들기

시작하기 전에 다음 지침에 따라 WPF 프로젝트를 만들고 XAML Islands를 호스트하도록 구성합니다. 기존 WPF 프로젝트가 있는 경우 프로젝트에 대해 이러한 단계 및 코드 예제를 적용할 수 있습니다.

1. Visual Studio 2019에서 새 **WPF 앱(.NET Framework)** 또는 **WPF 앱(.NET Core)** 프로젝트를 만듭니다. **WPF 앱(.NET Core)** 프로젝트를 만들려면 먼저 최신 버전의 [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)를 설치해야 합니다.

2. [패키지 참조](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)를 사용하도록 설정했는지 확인합니다.

    1. Visual Studio에서 **도구 -> NuGet 패키지 관리자 -> 패키지 관리자 설정**을 클릭합니다.
    2. **기본 패키지 관리 형식**에 대해 **PackageReference**를 선택했는지 확인합니다.

3. **솔루션 탐색기**에서 WPF 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

4. **NuGet 패키지 관리자** 창에서 **시험판 포함**이 포함되어 있는지 확인합니다.

5. **찾아보기** 탭을 선택하고 [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 패키지(버전 v6.0.0 이상)를 검색한 다음, 패키지를 설치합니다. 이 패키지는 WPF에 래핑된 UWP 컨트롤을 사용하는 데 필요한 모든 항목을 제공합니다([InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)과 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤 포함).
    > [!NOTE]
    > Windows Forms 앱은 [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) 패키지(버전 v6.0.0 이상)를 사용해야 합니다.

6. x86 또는 x64와 같은 특정 플랫폼을 대상으로 하도록 솔루션을 구성합니다. 대부분의 XAML Islands 시나리오는 **모든 CPU**를 대상으로 하는 프로젝트에서 지원되지 않습니다.

    1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 **속성** -> **구성 속성** -> **Configuration Manager**를 선택합니다. 
    2. **활성 솔루션 플랫폼**에서 **새로 만들기**를 선택합니다. 
    3. **새 솔루션 플랫폼** 대화 상자에서 **x64** 또는 **x86**을 선택하고 **확인**을 누릅니다. 
    4. 열기 대화 상자를 닫습니다.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>UWP 앱 프로젝트에서 XamlApplication 클래스 정의

그런 다음, UWP 앱 프로젝트를 솔루션에 추가하고 이 프로젝트의 기본 `App` 클래스를 수정하여 Windows 커뮤니티 도구 키트에서 제공하는 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 클래스에서 파생하도록 합니다. 이 클래스는 [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider) 인터페이스를 지원합니다. 이 인터페이스를 통해 앱은 런타임 시 애플리케이션의 현재 디렉터리에 있는 어셈블리의 사용자 지정 UWP XAML 컨트롤에 대한 메타데이터를 검색하고 로드할 수 있습니다. 이 클래스는 또한 현재 스레드에 대한 UWP XAML 프레임워크를 초기화합니다.

> [!NOTE]
> 이 단계가 자사 UWP 컨트롤을 호스트하는 데 필요하지는 않지만 사용자 지정 UWP 컨트롤 호스트를 포함하여 모든 범위의 XAML Island 시나리오를 지원하려면 앱에 `XamlApplication` 개체가 필요합니다. 따라서 XAML Islands를 사용하는 모든 솔루션에서 항상 `XamlApplication` 개체를 정의하는 것이 좋습니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 프로젝트**를 선택합니다.
2. 솔루션에 **빈 앱(유니버설 Windows)** 프로젝트를 추가합니다. 대상 버전 및 최소 버전이 둘 다 **Windows 10, 버전 1903** 이상으로 설정되어 있는지 확인합니다.
3. UWP 앱 프로젝트에서 [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 패키지(버전 v6.0.0 이상)를 설치합니다.
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
7. UWP 앱 프로젝트를 빌드합니다.
8. WPF 프로젝트에서 UWP 앱 프로젝트에 대한 참조를 추가합니다. 

    * WPF 프로젝트의 대상이 .NET Core인 경우 **종속성** 노드를 마우스 오른쪽 단추로 클릭하고 UWP 앱 프로젝트에 대한 참조를 추가합니다. 
    * WPF 프로젝트의 대상이 .NET Framework인 경우 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고, **빌드 종속성** -> **프로젝트 종속성**을 선택한 다음, UWP 앱 프로젝트를 선택합니다.

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

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>래핑된 컨트롤을 사용하여 InkCanvas 및 InkToolbar 호스트

UWP XAML Islands를 사용하도록 프로젝트를 구성했으므로, 이제 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 래핑된 UWP 컨트롤을 앱에 추가할 준비가 되었습니다.

1. **솔루션 탐색기**에서 **MainWindow.xaml** 파일을 엽니다.

2. XAML 파일의 위쪽에 있는 **Window** 요소에 다음 특성을 추가합니다. 이 특성은 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 래핑된 UWP 컨트롤에 대한 XAML 네임스페이스를 참조합니다.

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    이 특성을 추가한 후의 **Window** 요소는 다음과 같이 표시됩니다.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. **Mainwindow.xaml** 파일에서 기존 `<Grid>` 요소를 다음 XAML로 바꿉니다. 이 XAML은 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 컨트롤(이전에 네임스페이스로 정의한 **Controls** 키워드를 접두사로 지정)을 `<Grid>`에 추가합니다.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > **도구 상자**의 **Windows 커뮤니티 도구 키트** 섹션에서 디자이너로 이러한 컨트롤 및 기타 래핑된 컨트롤을 끌어 창에 추가할 수도 있습니다.

4. **MainWindow.xaml** 파일을 저장합니다.

    Surface와 같이 디지털 펜을 지원하는 디바이스를 사용하고 물리적 컴퓨터에서 이 랩을 실행하는 경우 이제 앱을 빌드 및 실행하고 펜을 사용하여 화면에 디지털 잉크를 그릴 수 있습니다. 그러나 펜 지원 디바이스가 없는데 마우스로 서명하려고 시도하면 아무 작업도 수행되지 않습니다. **InkCanvas** 컨트롤은 기본적으로 디지털 펜에 대해서만 활성화되기 때문입니다. 그러나 이 동작을 변경할 수 있습니다.

5. **MainWindow.xaml.cs** 파일을 엽니다.

6. 파일의 맨 위에 다음 변수 선언을 추가합니다.

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. `MainWindow()` 생성자를 찾습니다. `InitializeComponent()` 메서드 뒤에 다음 코드 줄을 추가하고 코드 파일을 저장합니다.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter** 개체를 사용하여 기본 수동 입력 환경을 사용자 지정할 수 있습니다. 이 코드는 **InputDeviceTypes** 속성을 사용하여 마우스와 펜 입력을 사용하도록 설정합니다.

8. F5 키를 다시 눌러 디버거에서 앱을 다시 빌드하고 실행합니다. 컴퓨터에서 마우스를 사용하는 경우 잉크 캔버스 공간에서 마우스로 그림을 그릴 수 있는지 확인합니다.

## <a name="host-a-calendarview-by-using-the-host-control"></a>호스트 컨트롤을 사용하여 CalendarView 호스트

[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 래핑된 UWP 컨트롤을 앱에 추가했으므로 이제 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)를 앱에 추가할 준비가 되었습니다.

> [!NOTE]
> [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤은 [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) 패키지에서 제공됩니다. 이 패키지는 이전에 설치한 [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 패키지에 포함되어 있습니다.

1. **솔루션 탐색기**에서 **MainWindow.xaml** 파일을 엽니다.

2. XAML 파일의 위쪽에 있는 **Window** 요소에 다음 특성을 추가합니다. 이 특성은 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤에 대한 XAML 네임스페이스를 참조합니다.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    이 특성을 추가한 후의 **Window** 요소는 다음과 같이 표시됩니다.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. **Mainwindow.xaml** 파일에서 기존 `<Grid>` 요소를 다음 XAML로 바꿉니다. 이 XAML은 그리드에 행을 추가하고 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 개체를 마지막 행에 추가합니다. UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 컨트롤을 호스트하기 위해 이 XAML은 `InitialTypeName` 속성을 컨트롤의 정규화된 이름으로 설정합니다. 또한 이 XAML은 호스팅된 컨트롤이 렌더링될 때 발생하는 `ChildChanged` 이벤트에 대한 이벤트 처리기를 정의합니다.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. **MainWindow.xaml** 파일을 저장하고 **MainWindow.xaml.cs** 파일을 엽니다.

7. 파일의 맨 위에 다음 변수 선언을 추가합니다.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. `MainWindow` 클래스에 다음 `ChildChanged` 이벤트 처리기 메서드를 추가하고 코드 파일을 저장합니다. 호스트 컨트롤이 렌더링되면 이 이벤트 처리기가 실행되고 달력 컨트롤의 `SelectedDatesChanged` 이벤트에 대한 간단한 이벤트 처리기를 만듭니다.

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. F5 키를 다시 눌러 디버거에서 앱을 다시 빌드하고 실행합니다. 이제 달력 컨트롤이 창의 아래쪽에 표시되는지 확인합니다.

## <a name="package-the-app"></a>앱 패키지

필요에 따라 배포를 위해 [MSIX 패키지](https://docs.microsoft.com/windows/msix)에 WPF 앱을 패키지할 수 있습니다. MSIX는 Windows용 최신 앱 패키징 기술로, MSI, .appx, App-V 및 ClickOnce 설치 기술의 조합을 기준으로 합니다.

다음 지침에서는 Visual Studio 2019의 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 사용하여 솔루션에 있는 모든 구성 요소를 MSIX 패키지에 패키지하는 방법을 보여 줍니다. 이러한 단계는 MSIX 패키지에서 WPF 앱을 패키지하는 경우에만 필요합니다.

> [!NOTE]
> 배포를 위해 [MSIX 패키지](https://docs.microsoft.com/windows/msix)에 애플리케이션을 패키지하지 않도록 선택하는 경우 앱을 실행하는 컴퓨터에 [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)이 설치되어 있어야 합니다.

1. 새 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 솔루션에 추가합니다. 프로젝트를 만들 때 **대상 버전**과 **최소 버전**을 모두**Windows 10 버전 1903(10.0; 빌드 18362)** 으로 선택합니다.

2. 패키징 프로젝트에서 **애플리케이션** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다. 프로젝트 목록에서 솔루션의 WPF 프로젝트를 선택하고 **확인**을 클릭합니다.

3. x86 또는 x64와 같은 특정 플랫폼을 대상으로 하도록 솔루션을 구성합니다. Windows 애플리케이션 패키징 프로젝트를 사용하여 MSIX 패키지에 WPF 앱을 빌드하는 데 필요합니다.

    1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고 **속성** -> **구성 속성** -> **Configuration Manager**를 선택합니다.
    2. **활성 솔루션 플랫폼**에서 **x64** 또는 **x86**을 선택합니다.
    3. WPF 프로젝트의 행에서 **플랫폼** 열의 **새로 만들기**를 선택합니다.
    4. **새 솔루션 플랫폼** 대화 상자에서 **x64** 또는 **x86**(**활성 솔루션 플랫폼**에 대해 선택한 것과 동일한 플랫폼)을 선택하고 **확인**을 클릭합니다.
    5. 열기 대화 상자를 닫습니다.

5. 패키징 프로젝트를 빌드하고 실행합니다. WPF가 실행되고 UWP 사용자 지정 컨트롤이 예상대로 표시되는지 확인합니다.

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)
* [XAML Islands 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
