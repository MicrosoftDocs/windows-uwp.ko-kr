---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, WPF 앱에 다른 최신 구성 요소를 통합하는 방법을 보여줍니다.
title: XAML Islands를 사용하여 UWP InkCanvas 컨트롤 추가
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6bb90fb9cbe7c9f54f60fd1920f0e73e174a3772
ms.sourcegitcommit: df0cd9c82d1c0c17ccde424e3c4a6ff680c31a35
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80482578"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>2부: XAML Islands를 사용하여 UWP InkCanvas 컨트롤 추가

Contoso 지출이라는 샘플 WPF 데스크톱 앱을 현대화하는 방법을 보여주는 자습서의 두 번째 파트입니다. 자습서의 개요, 필수 구성 요소 및 샘플 앱 다운로드 지침은 [자습서: WPF 앱 현대화](modernize-wpf-tutorial.md)를 참조하세요. 이 문서에서는 [1부](modernize-wpf-tutorial-1.md)를 이미 완료했다고 가정합니다.

이 자습서의 가상 시나리오에서 Contoso 개발 팀은 Contoso 지출 앱에 디지털 서명 지원을 추가하려고 합니다. UWP **InkCanvas** 컨트롤은 디지털 잉크 및 AI 기반 기능(예: 텍스트와 모양을 인식하는 기능)을 지원하기 때문에 이 시나리오에 유용한 옵션입니다. 이렇게 하려면 Windows 커뮤니티 도구 키트에 제공되는 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤을 사용합니다. 이 컨트롤은 WPF 앱에서 사용할 UWP **InkCanvas** 컨트롤의 인터페이스 및 기능을 래핑합니다. 래핑된 UWP 컨트롤에 대한 자세한 내용은 [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)를 참조하세요.

## <a name="configure-the-project-to-use-xaml-islands"></a>XAML Islands를 사용하도록 프로젝트 구성

Contoso 지출 앱에 **InkCanvas** 컨트롤을 추가하려면 먼저 UWP XAML Islands를 지원하도록 프로젝트를 구성해야 합니다.

1. Visual Studio 2019의 **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.

    ![Visual Studio의 NuGet 패키지 관리 메뉴](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭합니다. `Microsoft.Toolkit.Wpf.UI.Controls` 패키지를 검색하여 6.0.0 이상 버전을 설치합니다.

    > [!NOTE]
    > 이 패키지에는 **InkCanvas** 래핑된 UWP 컨트롤을 포함하여 WPF 앱에서 UWP XAML Islands를 호스팅하는 데 필요한 모든 인프라가 포함되어 있습니다. `Microsoft.Toolkit.Forms.UI.Controls`이라는 유사한 패키지를 Windows Forms 앱에 사용할 수 있습니다.

3. **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 -> 새 항목**을 선택합니다.

4. **애플리케이션 매니페스트 파일**을 선택하고, 이름을 **app.manifest**로 지정하고, **추가**를 클릭합니다. 애플리케이션 매니페스트에 대한 자세한 내용은 [이 문서](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)를 참조하세요.

5. 매니페스트 파일에서 Windows 10에 대한 다음 `<supportedOS>` 요소의 주석을 제거합니다.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. 매니페스트 파일에서 주석 처리된 `<application>` 요소를 찾습니다.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. 이 섹션을 삭제하고 다음 XML로 바꿉니다. 그러면 앱이 DPI를 인식하고 Windows 10에서 지원되는 다양한 배율 인수를 보다 잘 처리할 수 있습니다.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. `app.manifest` 파일을 저장한 후 닫습니다.

9. **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

10. **애플리케이션** 탭의 **리소스** 섹션에서 **매니페스트** 드롭다운이 **app.manifest**로 설정되어 있는지 확인합니다.

    ![.NET Core 앱 매니페스트](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. 프로젝트 속성의 변경 내용을 저장합니다.

## <a name="add-an-inkcanvas-control-to-the-app"></a>앱에 InkCanvas 컨트롤 추가

UWP XAML Islands를 사용하도록 프로젝트를 구성했으므로, 이제 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤을 앱에 추가할 준비가 되었습니다.

1. **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트의 **Views** 폴더를 펼치고, **ExpenseDetail.xaml** 파일을 두 번 클릭합니다.

2. XAML 파일의 위쪽에 있는 **Window** 요소에 다음 특성을 추가합니다. 이 특성은 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤에 대한 XAML 네임스페이스를 참조합니다.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    이 특성을 추가하면 **Window** 요소가 다음과 같습니다.

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. **ExpenseDetail.xaml** 파일에서 `<!-- Chart -->` 주석 바로 앞에 오는 닫는 `</Grid>` 태그를 찾습니다. 닫는 `</Grid>` 태그 바로 앞에 다음 XAML을 추가합니다. 이 XAML은 **InkCanvas** 컨트롤(앞에서 네임스페이스로 정의한 **toolkit** 키워드가 접두사로 추가됨) 및 컨트롤의 헤더 역할을 하는 간단한 **TextBlock**을 추가합니다.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. **ExpenseDetail.xaml** 파일을 저장합니다.

6. F5 키를 눌러 디버거에서 앱을 실행합니다.

7. 목록에서 직원을 선택한 다음, 사용 가능한 경비 중 하나를 선택합니다. 지출 정보 페이지에는 **InkCanvas** 컨트롤의 공간이 포함되어 있습니다.

    ![잉크 캔버스 펜만 해당](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Surface처럼 디지털 펜을 지원하는 디바이스를 갖고 있으며 물리적 머신에서 이 랩을 실행 중인 경우 디지털 펜을 사용해 보세요. 디지털 잉크가 화면에 표시되는 것을 볼 수 있습니다. 그러나 펜 지원 디바이스가 없는데 마우스로 서명하려고 시도하면 아무 작업도 수행되지 않습니다. **InkCanvas** 컨트롤은 기본적으로 디지털 펜에 대해서만 활성화되기 때문입니다. 그러나 이 동작을 변경할 수 있습니다.

8. **ContosoExpenses.Core** 프로젝트의 **Views** 폴더에서 **ExpenseDetail.xaml.cs** 파일을 두 번 클릭합니다.

9. 클래스의 맨 위에 다음 변수 선언을 추가합니다.

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. `ExpenseDetail()` 생성자를 찾습니다.

11. `InitializeComponent()` 메서드 뒤에 다음 코드 줄을 추가하고 코드 파일을 저장합니다.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter** 개체를 사용하여 기본 수동 입력 환경을 사용자 지정할 수 있습니다. 이 코드는 **InputDeviceTypes** 속성을 사용하여 마우스와 펜 입력을 사용하도록 설정합니다.

12. F5 키를 다시 눌러 디버거에서 앱을 다시 빌드하고 실행합니다. 목록에서 직원을 선택한 다음, 사용 가능한 경비 중 하나를 선택합니다.

13. 이제 마우스를 사용하여 서명 공간에 아무 것이나 그려 보세요. 이번에는 잉크가 화면에 표시되는 것을 볼 수 있습니다.

    ![서명](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>다음 단계

지금까지 자습서를 진행했다면 Contoso 지출 앱에 UWP **InkCanvas** 컨트롤이 추가되었을 것입니다. 이제 [3부: XAML Islands를 사용하여 UWP CalendarView 컨트롤 추가](modernize-wpf-tutorial-3.md)를 진행할 수 있습니다.
