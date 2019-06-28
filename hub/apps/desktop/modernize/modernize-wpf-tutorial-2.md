---
description: 이 자습서에는 UWP XAML 사용자 인터페이스를 추가, MSIX 패키지 만들기 및 WPF 앱에 다른 최신 구성 요소를 통합 하는 방법을 보여 줍니다.
title: XAML 제도 사용 하 여 UWP InkCanvas 컨트롤 추가
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420090"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>2부: XAML 제도 사용 하 여 UWP InkCanvas 컨트롤 추가

Contoso Expenses 라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법에 설명 하는 자습서의 두 번째 부분입니다. 자습서, 필수 구성 요소 및 샘플 앱을 다운로드 하기 위한 지침의 개요를 참조 하세요. [자습서: WPF 앱을 현대화](modernize-wpf-tutorial.md)합니다. 이 문서에서는 이미 완료 했다고 가정 [1 부](modernize-wpf-tutorial-1.md)합니다.

이 자습서에서는 가상의 시나리오에서 Contoso 개발 팀은 Contoso expenses에 디지털 서명에 대 한 지원을 추가 하려고 합니다. UWP **InkCanvas** 컨트롤 디지털 잉크 및 텍스트 및 셰이프를 인식 하는 기능 등의 AI 기반 기능을 지원 하기 때문에이 시나리오에 대 한 유용한 옵션입니다. 이 작업을 수행 하려면 사용 합니다 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤 Windows 커뮤니티 도구 키트에서 사용할 수 있습니다. 이 컨트롤의 인터페이스 및 UWP 기능 래핑합니다 **InkCanvas** WPF 앱에서 사용할 컨트롤입니다. 래핑된 UWP 컨트롤에 대 한 자세한 내용은 참조 하세요. [데스크톱 앱 (XAML 제도)에서 호스트 UWP XAML 컨트롤](xaml-islands.md)합니다.

## <a name="configure-the-project-to-use-xaml-islands"></a>XAML 제도 사용 하도록 프로젝트 구성

추가 하기 전에 **InkCanvas** 컨트롤을 Contoso expenses 하면 첫 번째 필요가 UWP XAML 아일랜드를 지원 하도록 프로젝트를 구성 합니다.

1. Visual Studio 2019에서 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 프로젝트 **솔루션 탐색기** 선택한 **NuGet 패키지 관리**합니다.

    ![Visual Studio의 NuGet 패키지 메뉴 관리](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. 에 **NuGet 패키지 관리자** 창에서 클릭 **찾아보기**합니다. 선택 합니다 **시험판 포함** 옵션을 검색할는 `Microsoft.Toolkit.Wpf.UI.Controls` 패키지 하 고 결과에 표시 된 패키지의 최신 미리 보기 릴리스를 설치 합니다.

    > [!NOTE]
    > UWP XAML 제도 하는 WPF 앱에서 호스트 하기 위해 필요한 모든 인프라를 포함 하는이 패키지 포함 하는 **InkCanvas** UWP 컨트롤 래핑 합니다. 명명 된 비슷한 패키지 `Microsoft.Toolkit.Forms.UI.Controls` Windows Forms 앱에서 사용할 수 있습니다.

3. 마우스 오른쪽 단추로 클릭 **ContosoExpenses.Core** 프로젝트 **솔루션 탐색기** 선택한 **-> 새 항목 추가**합니다.

4. 선택 **응용 프로그램 매니페스트 파일**, 이름을 **app.manifest**를 클릭 하 고 **추가**합니다.

5. 열린된 매니페스트 파일에서 찾습니다 합니다 **호환성** 섹션 및 다음 주석으로 처리 된 항목을 식별 합니다.

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. 이 항목 아래에 다음 항목을 추가 합니다.

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. 주석 처리를 제거 합니다 **supportedOS** Windows 10에 대 한 항목입니다. 이제이 섹션에서는이 처럼 보여야 합니다.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > 이 항목은 앱을 Windows 10 버전 1903 (18362 빌드)에 필요 함을 지정 이상. XAML 제도 지 원하는 Windows 10의 첫 번째 버전입니다. 응용 프로그램 매니페스트에서이 항목이 없으면 앱에는 런타임 시 예외가 throw 됩니다.

8. 매니페스트 파일에서 주석 처리 된 다음 찾습니다 **응용 프로그램** 섹션입니다.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. 이 섹션을 삭제 하 고 다음 XML로 바꿉니다. 이 앱을 DPI 인식 하 고 더 나은 핸들 다른 배율 Windows 10에서 지원 되는 구성 합니다.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. 저장 하 고 닫습니다는 `app.manifest` 파일입니다.

12. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Core** 선택한 프로젝트 **속성**합니다.

13. 에 **리소스** 부분을 **응용 프로그램** 탭에서 **매니페스트** 드롭다운으로 설정 되어 **app.manifest**.

    ![.NET core 응용 프로그램 매니페스트](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. 프로젝트 속성에 변경 내용을 저장 합니다.

## <a name="add-an-inkcanvas-control-to-the-app"></a>InkCanvas 컨트롤을 앱에 추가

UWP XAML 제도 사용 하도록 프로젝트를 구성한 했으므로 준비가 이제 추가 하는 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 앱을 UWP 컨트롤입니다.

1. **솔루션 탐색기**를 확장 합니다 **뷰** 의 폴더를 **ContosoExpenses.Core** 프로젝트를 두 번 클릭는 **ExpenseDetail.xaml**파일입니다.

2. 에 **창을** 요소 XAML 파일의 위쪽에 다음 특성을 추가 합니다. 이 참조에 대 한 XAML 네임 스페이스는 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) 래핑된 UWP 컨트롤입니다.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    이 특성을 추가한 후 합니다 **창을** 요소에 이제이 처럼 보여야 합니다.

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

4. 에 **ExpenseDetail.xaml** 파일을 닫는 찾습니다 `</Grid>` 태그 바로 앞에 있는 `<!-- Chart -->` 주석입니다. 닫기 전에 다음 XAML을 추가 `</Grid>` 태그입니다. 이 XAML 추가 **InkCanvas** 컨트롤 (접두사로 **도구 키트** 키워드 네임 스페이스와 이전에 정의한) 및 간단한 **TextBlock** 컨트롤에 대 한 머리글으로 작동 하 합니다.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 저장 된 **ExpenseDetail.xaml** 파일입니다.

6. F5 키를 눌러 디버거에서 앱을 실행 합니다.

7. 목록에서 직원을 선택 하 고 사용할 수 있는 비용 중 하나를 선택 합니다. 비용 세부 정보 페이지에 대 한 공간에 포함 된 **InkCanvas** 컨트롤입니다.

    ![잉크 캔버스 펜만](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    디지털 펜을을 화면 처럼에 지 장치를이 랩이 물리적 컴퓨터에서 실행 중인 경우에 사용 하려고 합니다. 화면에 표시 되는 디지털 잉크를 볼 수 있습니다. 그러나 펜 지원 장치가 없는 경우 마우스를 사용 하 여 로그인 하려고 하면 아무 작업도 수행 합니다. 이 문제가 발생 하기 때문에 합니다 **InkCanvas** 컨트롤은 기본적으로 디지털 펜에 대해서만 사용 됩니다. 그러나이 동작은 변경할 수 있습니다.

8. 앱을 닫고 두 번 클릭 합니다 **ExpenseDetail.xaml.cs** 파일 합니다 **뷰** 의 폴더를 **ContosoExpenses.Core** 프로젝트.

9. 클래스의 맨 위에 있는 다음 네임 스페이스 선언을 추가 합니다.

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 찾을 `ExpenseDetail()` 생성자입니다.

11. 다음 코드 바로 다음 줄을 추가 합니다 `InitializeComponent()` 메서드 코드 파일을 저장 합니다.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    사용할 수는 **InkPresenter** 환경을 잉크 입력 기본 사용자 지정 하는 개체입니다. 이 코드를 사용 합니다 **InputDeviceTypes** 펜 입력 뿐만 아니라 마우스를 사용 하도록 설정 하려면 속성입니다.

12. F5 키를 눌러 다시 빌드하고 디버거에서 앱을 실행 합니다. 목록에서 직원을 선택 하 고 사용할 수 있는 비용 중 하나를 선택 합니다.

13. 이제 마우스를 사용 하 여 서명 공간에 무언가 그려야 해 보세요. 이 이번에는 화면에 표시 되는 잉크를 볼 수 있습니다.

    ![서명](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>다음 단계

이 시점에서 자습서를 성공적으로 추가한 UWP **InkCanvas** Contoso expenses 제어 합니다. 에 대 한 준비가 [3 부: XAML 제도 사용 하 여 UWP CalendarView 컨트롤을 추가](modernize-wpf-tutorial-3.md)합니다.
