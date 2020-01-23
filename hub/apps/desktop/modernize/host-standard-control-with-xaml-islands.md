---
description: 이 문서에서는 XAML 아일랜드를 사용 하 여 WPF 앱에서 표준 UWP 컨트롤을 호스트 하는 방법을 보여 줍니다.
title: XAML 아일랜드를 사용 하 여 WPF 앱에서 표준 UWP 컨트롤 호스팅
ms.date: 01/10/2010
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml 아일랜드, 래핑된 컨트롤, 표준 컨트롤, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 47998d9c79dbbb41060b4fbd52fe3ee805aecc52
ms.sourcegitcommit: 85fd390b1e602707bd9342cb4b84b97ae0d8b831
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76520388"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>XAML 아일랜드를 사용 하 여 WPF 앱에서 표준 UWP 컨트롤 호스팅

이 문서에서는 [XAML 아일랜드](xaml-islands.md)를 사용 하 여 WPF 앱에서 표준 uwp 컨트롤 (즉, Windows SDK 또는 WinUI 라이브러리에서 제공 하는 자사 uwp 컨트롤)을 호스트 하는 두 가지 방법을 보여 줍니다.

* Windows 커뮤니티 도구 키트에서 [래핑된 컨트롤](xaml-islands.md#wrapped-controls) 을 사용 하 여 UWP [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 및 [inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) 컨트롤을 호스트 하는 방법을 보여 줍니다. 이러한 컨트롤은 몇 가지 유용한 UWP 컨트롤 집합의 인터페이스 및 기능을 래핑합니다. WPF 또는 Windows Forms 프로젝트의 디자인 화면에 직접 추가 하 고 디자이너의 다른 WPF 또는 Windows Forms 컨트롤과 같은 방식으로 사용할 수 있습니다.

* 또한 Windows 커뮤니티 도구 키트의 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용 하 여 UWP [calendarview](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 컨트롤을 호스트 하는 방법을 보여 줍니다. UWP 컨트롤의 작은 집합만 래핑된 컨트롤로 사용할 수 있으므로 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 를 사용 하 여 다른 표준 UWP 컨트롤을 호스트할 수 있습니다.

WPF 앱에서 UWP 컨트롤을 호스팅하려면 다음 구성 요소가 필요 합니다. 이 문서에서는 이러한 각 구성 요소를 만드는 방법에 대 한 지침을 제공 합니다.

* WPF 앱에 대 한 프로젝트 및 소스 코드입니다.
* Windows Community Toolkit에서 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 클래스의 인스턴스를 정의 하는 UWP 앱 프로젝트입니다.
  > [!NOTE]
  > 앱이 모든 XAML 고립 시나리오에서 제대로 작동 하도록 하려면 WPF (또는 Windows Forms) 프로젝트에 `XamlApplication` 개체에 대 한 액세스 권한이 있어야 합니다. 이 개체는 응용 프로그램의 현재 디렉터리에 있는 어셈블리의 UWP XAML 형식에 대 한 메타 데이터를 로드 하기 위한 루트 메타 데이터 공급자로 작동 합니다. 이 작업을 수행 하는 권장 방법은 WPF (또는 Windows Forms) 프로젝트와 동일한 솔루션에 **빈 앱 (유니버설 Windows)** 프로젝트를 추가 하 고이 프로젝트의 기본 `App` 클래스를 수정 하 여 `XamlApplication`에서 파생 하는 것입니다.
  >
  > 이 단계는 자사 UWP 컨트롤을 호스트 하는 것과 같은 간단한 XAML 고립 시나리오에는 필요 하지 않지만 사용자 지정 UWP 컨트롤 호스팅을 비롯 하 여 XAML 고립 시나리오의 전체 범위를 지원 하기 위해 WPF 앱에이 `XamlApplication` 개체가 필요 합니다. 항상 UWP 프로젝트를 추가 하 고 XAML 아일랜드를 사용 하는 모든 솔루션에 `XamlApplication` 개체를 정의 하는 것이 좋습니다. 솔루션은 `XamlApplication` 개체를 정의 하는 프로젝트를 하나만 포함할 수 있습니다. 앱의 모든 사용자 지정 UWP 컨트롤은 동일한 `XamlApplication` 개체를 공유 합니다.

이 문서에서는 WPF 앱에서 UWP 컨트롤을 호스트 하는 방법을 보여 주지만 Windows Forms 앱에 대 한 프로세스는 유사 합니다.

## <a name="create-a-wpf-project"></a>WPF 프로젝트 만들기

시작 하기 전에 다음 지침에 따라 WPF 프로젝트를 만들고 XAML 아일랜드를 호스팅하도록 구성 합니다. 기존 WPF 프로젝트가 있는 경우 프로젝트에 대 한 이러한 단계 및 코드 예제를 적용할 수 있습니다.

1. Visual Studio 2019에서 새 **Wpf 앱 (.NET Framework)** 또는 **wpf 앱 (.net Core)** 프로젝트를 만듭니다. **WPF 앱 (.Net core)** 프로젝트를 만들려는 경우 최신 버전의 [.NET core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)를 먼저 설치 해야 합니다.

2. [패키지 참조](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) 를 사용할 수 있는지 확인 합니다.

    1. Visual Studio에서 **도구-> NuGet 패키지 관리자-> 패키지 관리자 설정**을 클릭 합니다.
    2. **기본 패키지 관리 형식**에 대해 **PackageReference** 이 선택 되어 있는지 확인 합니다.

3. **솔루션 탐색기** 에서 WPF 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.

4. **NuGet 패키지 관리자** 창에서 **시험판 포함** 이 선택 되어 있는지 확인 합니다.

5. **찾아보기** 탭을 선택 하 고, 6.0.0 [패키지 (](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 버전 v 이상)를 검색 하 고, 패키지를 설치 합니다. 이 패키지는 WPF에 래핑된 UWP 컨트롤을 사용 하는 데 필요한 모든 기능을 제공 합니다 ( [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [Inktoolbar 모음](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 및 [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤 포함).
    > [!NOTE]
    > Windows Forms 앱은 [6.0.0 패키지 (](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) 버전 v 이상)를 사용 해야 합니다.

6. X86 또는 x64와 같은 특정 플랫폼을 대상으로 하도록 솔루션을 구성 합니다. 대부분의 XAML 아일랜드 시나리오는 **모든 CPU**를 대상으로 하는 프로젝트에서 지원 되지 않습니다.

    1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **속성** -> **구성 속성** -> **Configuration Manager**를 선택 합니다. 
    2. **활성 솔루션 플랫폼**에서 **새로 만들기**를 선택 합니다. 
    3. **새 솔루션 플랫폼** 대화 상자에서 **x64** 또는 **x 86** 을 선택 하 고 **확인**을 누릅니다. 
    4. 열기 대화 상자를 닫습니다.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>UWP 앱 프로젝트에서 XamlApplication 개체 만들기

다음으로, WPF 프로젝트와 동일한 솔루션에 UWP 앱 프로젝트를 추가 합니다. 이 프로젝트의 기본 `App` 클래스를 수정 하 여 Windows 커뮤니티 도구 키트에서 제공 하는 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 클래스에서 파생 됩니다. 단일 자사 UWP 컨트롤을 호스트 하는 것과 같은 trivial XAML 고립 시나리오에는이 단계가 필요 하지 않지만 WPF 앱에는이 `XamlApplication` 개체가 있어야 전체 범위의 XAML 아일랜드 시나리오를 지원할 수 있습니다. XAML 아일랜드를 사용 하는 모든 솔루션에 항상이 프로젝트를 추가 하는 것이 좋습니다.

1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **추가** -> **새 프로젝트**를 선택 합니다.
2. 솔루션에 **빈 앱(Universal Windows)** 프로젝트 추가 대상 버전 및 최소 버전이 모두 **Windows 10 버전 1903** 이상으로 설정 되어 있는지 확인 합니다.
3. UWP 앱 프로젝트에서 6.0.0 [응용 프로그램](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 패키지 (버전 v 이상)를 설치 합니다.
4. **App.xaml 파일을** 열고이 파일의 내용을 다음 xaml로 바꿉니다. `MyUWPApp`를 UWP 앱 프로젝트의 네임 스페이스로 바꿉니다.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. **App.xaml.cs** 파일을 열고이 파일의 내용을 다음 코드로 바꿉니다. `MyUWPApp`를 UWP 앱 프로젝트의 네임 스페이스로 바꿉니다.

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

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>래핑된 컨트롤을 사용 하 여 InkCanvas 및 InkToolbar 호스트

UWP XAML 아일랜드를 사용 하도록 프로젝트를 구성 했으므로 이제 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 래핑된 UWP 컨트롤을 앱에 추가할 준비가 되었습니다.

1. **솔루션 탐색기**에서 **MainWindow.xaml** 파일을 엽니다.

2. XAML 파일 위쪽의 **창** 요소에 다음 특성을 추가 합니다. 이는 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 래핑된 UWP 컨트롤에 대 한 XAML 네임 스페이스를 참조 합니다.

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    이 특성을 추가한 후에는 이제 **창** 요소가 다음과 같이 표시 됩니다.

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

3. **Mainwindow.xaml** 파일에서 기존 `<Grid>` 요소를 다음 xaml로 바꿉니다. 이 XAML은 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 컨트롤 (이전에 네임 스페이스로 정의한 **Controls** 키워드가 접두사로 추가 됨)을 `<Grid>`에 추가 합니다.

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
    > **도구 상자** 의 **Windows Community Toolkit** 섹션에서 디자이너로 끌어와 이러한 컨트롤 및 기타 래핑된 컨트롤을 창에 추가할 수도 있습니다.

4. **Mainwindow.xaml** 파일을 저장 합니다.

    화면과 같이 디지털 펜을 지 원하는 장치를 사용 하 고이 랩을 물리적 컴퓨터에서 실행 하는 경우 이제 앱을 빌드 및 실행 하 고 펜을 사용 하 여 화면에 디지털 잉크를 그릴 수 있습니다. 그러나 펜 지원 장치가 없고 마우스로 서명 하려고 하면 아무 작업도 수행 되지 않습니다. **InkCanvas** 컨트롤은 기본적으로 디지털 펜에 대해서만 활성화 되기 때문에 발생 합니다. 그러나 이 동작을 변경할 수 있습니다.

5. **MainWindow.xaml.cs** 파일을 엽니다.

6. 파일의 맨 위에 다음 네임 스페이스 선언을 추가 합니다.

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. `MainWindow()` 생성자를 찾습니다. `InitializeComponent()` 메서드 뒤에 다음 코드 줄을 추가 하 고 코드 파일을 저장 합니다.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter** 개체를 사용 하 여 기본 잉크 사용 환경을 사용자 지정할 수 있습니다. 이 코드는 **Inputdevicetypes** 속성을 사용 하 여 마우스와 펜 입력을 사용 하도록 설정 합니다.

8. F5 키를 다시 눌러 디버거에서 앱을 다시 빌드하고 실행 합니다. 마우스를 사용 하 여 컴퓨터를 사용 하는 경우 잉크 캔버스 공간에서 마우스를 사용 하 여 항목을 그릴 수 있는지 확인 합니다.

## <a name="host-a-calendarview-by-using-the-host-control"></a>호스트 컨트롤을 사용 하 여 CalendarView 호스트

이제 앱에 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 및 [INKTOOLBAR](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) 래핑된 UWP 컨트롤을 추가 했으므로 이제 [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용 하 여 [calendarview](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 를 앱에 추가할 준비가 되었습니다.

> [!NOTE]
> [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤은 [Microsoft Toolkit. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) 패키지에서 제공 됩니다. 이 패키지는 이전에 설치한 [Microsoft Toolkit. 컨트롤](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) 패키지에 포함 되어 있습니다.

1. **솔루션 탐색기**에서 **MainWindow.xaml** 파일을 엽니다.

2. XAML 파일 위쪽의 **창** 요소에 다음 특성을 추가 합니다. 이는 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤에 대 한 XAML 네임 스페이스를 참조 합니다.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    이 특성을 추가한 후에는 이제 **창** 요소가 다음과 같이 표시 됩니다.

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

4. **Mainwindow.xaml** 파일에서 기존 `<Grid>` 요소를 다음 xaml로 바꿉니다. 이 XAML은 표에 행을 추가 하 고 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 개체를 마지막 행에 추가 합니다. UWP [Calendarview](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 컨트롤을 호스트 하기 위해이 XAML은 `InitialTypeName` 속성을 컨트롤의 정규화 된 이름으로 설정 합니다. 또한이 XAML은 호스팅된 컨트롤이 렌더링 될 때 발생 하는 `ChildChanged` 이벤트에 대 한 이벤트 처리기를 정의 합니다.

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

5. **Mainwindow.xaml** 파일을 저장 하 고 **MainWindow.xaml.cs** 파일을 엽니다.

7. 파일의 맨 위에 다음 네임 스페이스 선언을 추가 합니다.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. `MainWindow` 클래스에 다음 `ChildChanged` 이벤트 처리기 메서드를 추가 하 고 코드 파일을 저장 합니다. 호스트 컨트롤이 렌더링 되 면이 이벤트 처리기는를 실행 하 고 calendar 컨트롤의 `SelectedDatesChanged` 이벤트에 대 한 간단한 이벤트 처리기를 만듭니다.

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

11. F5 키를 다시 눌러 디버거에서 앱을 다시 빌드하고 실행 합니다. 이제 달력 컨트롤이 창의 아래쪽에 표시 되는지 확인 합니다.

## <a name="package-the-app"></a>앱 패키지

필요에 따라 배포를 위해 [Msix 패키지](https://docs.microsoft.com/windows/msix) 에 WPF 앱을 패키지할 수 있습니다. MSIX은 Windows 용 최신 앱 패키징 기술 이며 MSI, APPX, App-v 및 ClickOnce 설치 기술의 조합을 기반으로 합니다.

다음 지침에서는 Visual Studio 2019의 [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 사용 하 여 msix 패키지의 솔루션에 있는 모든 구성 요소를 패키지 하는 방법을 보여 줍니다. 이러한 단계는 MSIX 패키지에서 WPF 앱을 패키징하는 경우에만 필요 합니다.

1. 새 [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) 를 솔루션에 추가 합니다. 프로젝트를 만들 때 **Windows 10, 버전 1903 (10.0;)을 선택 합니다. 빌드 18362)** 는 **대상 버전** 및 **최소 버전**모두에 해당 합니다.

2. 패키징 프로젝트에서 **응용 프로그램** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다. 프로젝트 목록에서 솔루션의 WPF 프로젝트를 선택 하 고 **확인**을 클릭 합니다.

3. WPF 프로젝트에서 .NET Core 3을 대상으로 하는 경우 패키징 프로젝트 파일을 편집 해야 합니다. 이러한 변경은 현재 .NET Core 3을 대상으로 하 고 UWP 컨트롤을 호스트 하는 WPF 앱을 패키징하는 데 필요 합니다.

    1. 솔루션 탐색기에서 패키징 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 파일 편집**을 선택 합니다.
    2. 파일에서 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 요소를 찾습니다. 이 요소를 다음 XML로 바꿉니다.

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
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. 프로젝트 파일을 저장하고 닫습니다.

4. X86 또는 x64와 같은 특정 플랫폼을 대상으로 하도록 솔루션을 구성 합니다. 이는 Windows 응용 프로그램 패키징 프로젝트를 사용 하 여 MSIX 패키지에 WPF 앱을 빌드하는 데 필요 합니다.

    1. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 **속성** -> **구성 속성** -> **Configuration Manager**를 선택 합니다.
    2. **활성 솔루션 플랫폼**아래에서 **x64** 또는 **x 86**을 선택 합니다.
    3. WPF 프로젝트에 대 한 행의 **Platform** 열에서 **새로 만들기**를 선택 합니다.
    4. **새 솔루션 플랫폼** 대화 상자에서 **x64** 또는 **x86** ( **활성 솔루션 플랫폼**에 대해 선택한 것과 동일한 플랫폼)을 선택 하 고 **확인**을 클릭 합니다.
    5. 열기 대화 상자를 닫습니다.

5. 패키징 프로젝트를 빌드하고 실행 합니다. WPF가 실행 되 고 UWP 사용자 지정 컨트롤이 예상 대로 표시 되는지 확인 합니다.

## <a name="related-topics"></a>관련 항목

* [데스크톱 응용 프로그램의 UWP 컨트롤](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
