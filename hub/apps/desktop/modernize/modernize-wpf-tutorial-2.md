---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가 하 고, MSIX 개의 패키지를 만들고, 기타 최신 구성 요소를 WPF 앱에 통합 하는 방법을 보여 줍니다.
title: XAML Islands를 사용하여 UWP InkCanvas 컨트롤 추가
ms.topic: article
ms.date: 08/15/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fb7bb6d4e5af8992571f9740c1321e271b2e1672
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643422"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>2부: XAML Islands를 사용하여 UWP InkCanvas 컨트롤 추가

이 자습서에서는 Contoso 지출 이라는 샘플 WPF 데스크톱 응용 프로그램을 현대화 하는 방법을 보여 주는 자습서의 두 번째 부분입니다. 샘플 앱을 다운로드 하기 위한 자습서, 필수 구성 요소 및 지침에 대 한 개요를 [보려면 자습서: WPF 앱](modernize-wpf-tutorial.md)을 현대화 합니다. 이 문서에서는 [1 부](modernize-wpf-tutorial-1.md)를 이미 완료 했다고 가정 합니다.

이 자습서의 가상 시나리오에서 Contoso 개발 팀은 Contoso 지출 앱에 디지털 서명 지원을 추가 하려고 합니다. UWP **InkCanvas** 컨트롤은 텍스트와 모양을 인식 하는 기능과 같은 디지털 잉크 및 AI 기반 기능을 지원 하기 때문에이 시나리오에 유용한 옵션입니다. 이렇게 하려면 Windows 커뮤니티 도구 키트에서 제공 되는 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤을 사용 합니다. 이 컨트롤은 WPF 앱에서 사용할 UWP **InkCanvas** 컨트롤의 인터페이스 및 기능을 래핑합니다. 래핑된 UWP 컨트롤에 대 한 자세한 내용은 [데스크톱 앱에서 UWP xaml 컨트롤 호스트 (XAML 아일랜드)](xaml-islands.md)를 참조 하세요.

## <a name="configure-the-project-to-use-xaml-islands"></a>XAML 아일랜드를 사용 하도록 프로젝트 구성

Contoso 지출 앱에 **InkCanvas** 컨트롤을 추가 하려면 먼저 UWP XAML 아일랜드를 지원 하도록 프로젝트를 구성 해야 합니다.

1. Visual Studio 2019의 **솔루션 탐색기** 에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다.

    ![Visual Studio의 NuGet 패키지 관리 메뉴](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. **NuGet 패키지 관리자** 창에서 **찾아보기**를 클릭 합니다. **시험판 포함** 옵션을 선택 하 고, `Microsoft.Toolkit.Wpf.UI.Controls` 패키지를 검색 하 고, 결과에 표시 된 패키지의 최신 미리 보기 릴리스를 설치 합니다. 6\.0.0-preview7 이상 버전을 설치 했는지 확인 합니다.

    > [!NOTE]
    > 이 패키지에는 **InkCanvas** 래핑된 uwp 컨트롤을 비롯 하 여 WPF 앱에서 UWP XAML 아일랜드를 호스트 하는 데 필요한 모든 인프라가 포함 되어 있습니다. Windows Forms 앱에는 `Microsoft.Toolkit.Forms.UI.Controls` 라는 유사한 패키지를 사용할 수 있습니다.

3. **솔루션 탐색기** 에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 항목**을 선택 합니다.

4. **응용 프로그램 매니페스트 파일**을 선택 하 고 이름을 **app .manifest**로 선택한 후 **추가**를 클릭 합니다. 응용 프로그램 매니페스트에 대 한 자세한 내용은 [이 문서](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)를 참조 하세요.

5. 매니페스트 파일에서 Windows 10에 대 한 `<supportedOS>` 다음 요소의 주석 처리를 제거 합니다.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. 매니페스트 파일에서 다음 주석 처리 `<application>` 된 요소를 찾습니다.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. 이 섹션을 삭제 하 고 다음 XML로 바꿉니다. 그러면 앱이 DPI를 인식 하 고 Windows 10에서 지 원하는 다양 한 크기 조정 요인을 더 잘 처리할 수 있도록 구성 됩니다.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. 파일을 `app.manifest` 저장 하 고 닫습니다.

9. **솔루션 탐색기**에서 **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.

10. **응용 프로그램** 탭의 **리소스** 섹션에서 **매니페스트** 드롭다운이 **app-v**로 설정 되었는지 확인 합니다.

    ![.NET Core 응용 프로그램 매니페스트](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. 프로젝트 속성에 대 한 변경 내용을 저장 합니다.

## <a name="add-an-inkcanvas-control-to-the-app"></a>앱에 InkCanvas 컨트롤 추가

UWP XAML 아일랜드를 사용 하도록 프로젝트를 구성 했으므로 이제 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤을 앱에 추가할 준비가 되었습니다.

1. **솔루션 탐색기**에서 **ContosoExpenses** 프로젝트의 **Views** 폴더를 확장 하 고 **ExpenseDetail** 파일을 두 번 클릭 합니다.

2. XAML 파일 위쪽의 **창** 요소에 다음 특성을 추가 합니다. [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤에 대 한 XAML 네임 스페이스를 참조 합니다.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    이 특성을 추가한 후에는 이제 **Window** 요소가 다음과 같이 표시 됩니다.

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

4. **ExpenseDetail** 파일에서 `</Grid>` `<!-- Chart -->` 주석 바로 앞에 닫는 태그를 찾습니다. 닫는 `</Grid>` 태그 바로 앞에 다음 XAML을 추가 합니다. 이 XAML은 **InkCanvas** 컨트롤 (이전에 네임 스페이스로 정의한 **toolkit** 키워드가 접두사로 추가 됨) 및 컨트롤의 헤더 역할을 하는 간단한 **TextBlock** 을 추가 합니다.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. **ExpenseDetail** 파일을 저장 합니다.

6. F5 키를 눌러 디버거에서 앱을 실행 합니다.

7. 목록에서 직원을 선택 하 고 사용 가능한 비용 중 하나를 선택 합니다. 경비 세부 정보 페이지에는 **InkCanvas** 컨트롤에 대 한 공간이 포함 되어 있습니다.

    ![잉크 캔버스 펜만](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    화면과 같이 디지털 펜을 지 원하는 장치를 사용 하 고이 랩을 물리적 컴퓨터에서 실행 하는 경우으로 이동 하 여 사용 합니다. 디지털 잉크가 화면에 표시 되는 것을 볼 수 있습니다. 그러나 펜 지원 장치가 없고 마우스로 서명 하려고 하면 아무 작업도 수행 되지 않습니다. **InkCanvas** 컨트롤은 기본적으로 디지털 펜에 대해서만 활성화 되기 때문에 발생 합니다. 그러나이 동작을 변경할 수 있습니다.

8. 앱을 닫고 **ContosoExpenses** 프로젝트의 **Views** 폴더 아래에 있는 **ExpenseDetail.xaml.cs** 파일을 두 번 클릭 합니다.

9. 클래스의 맨 위에 다음 네임 스페이스 선언을 추가 합니다.

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 생성자를 `ExpenseDetail()` 찾습니다.

11. `InitializeComponent()` 메서드 뒤에 다음 코드 줄을 추가 하 고 코드 파일을 저장 합니다.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    **InkPresenter** 개체를 사용 하 여 기본 잉크 사용 환경을 사용자 지정할 수 있습니다. 이 코드는 **Inputdevicetypes** 속성을 사용 하 여 마우스와 펜 입력을 사용 하도록 설정 합니다.

12. F5 키를 다시 눌러 디버거에서 앱을 다시 빌드하고 실행 합니다. 목록에서 직원을 선택 하 고 사용 가능한 비용 중 하나를 선택 합니다.

13. 이제 마우스를 사용 하 여 서명 공간에 항목을 그려 보세요. 이번에는 화면에 잉크가 표시 되는 것을 볼 수 있습니다.

    ![서명](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>다음 단계

자습서의이 시점에서는 Contoso 지출 앱에 UWP **InkCanvas** 컨트롤을 추가 했습니다. 이제 3 부에 사용할 [준비가 되었습니다. XAML 아일랜드](modernize-wpf-tutorial-3.md)를 사용 하 여 UWP calendarview 컨트롤을 추가 합니다.
