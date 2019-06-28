---
description: 이 자습서에는 UWP XAML 사용자 인터페이스를 추가, MSIX 패키지 만들기 및 WPF 앱에 다른 최신 구성 요소를 통합 하는 방법을 보여 줍니다.
title: XAML 제도 사용 하 여 UWP CalendarView 컨트롤 추가
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420105"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>3부: XAML 제도 사용 하 여 UWP CalendarView 컨트롤 추가

Contoso Expenses 라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법에 설명 하는 자습서의 세 번째 부분입니다. 자습서, 필수 구성 요소 및 샘플 앱을 다운로드 하기 위한 지침의 개요를 참조 하세요. [자습서: WPF 앱을 현대화](modernize-wpf-tutorial.md)합니다. 이 문서에서는 이미 완료 했다고 가정 [2 부](modernize-wpf-tutorial-2.md)합니다.

이 자습서에서는 가상의 시나리오에서 Contoso 개발팀 쉽게 터치 사용 장치에서 경비 보고서에 대 한 날짜를 선택 하려고 합니다. UWP 자습서의이 부분에서는 추가 [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) 앱을 제어 합니다. 이 작업 표시줄에서 Windows 10 날짜 및 시간 기능에 사용 되는 동일한 컨트롤입니다.

![CalendarViewControl 이미지](images/wpf-modernize-tutorial/CalendarViewControl.png)

와 달리 합니다 **InkCanvas** 제어에 추가한 [2 부](modernize-wpf-tutorial-2.md), Windows 커뮤니티 도구 키트의 UWP 래핑된 버전을 제공 하지 않습니다 **CalendarView** WPF 앱에서 사용할 수 있는 . 호스트할 수 있습니다는 **InkCanvas** 제네릭에서 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 제어 합니다. 플랫폼 또는 타사 공급 업체에서 제공한 모든 UWP 컨트롤을 호스트 하려면이 컨트롤을 사용할 수 있습니다. **WindowsXamlHost** 에서 제공 하는 컨트롤을 `Microsoft.Toolkit.Wpf.UI.XamlHost` 패키지 NuGet 패키지. 이 패키지에 포함 되어는 `Microsoft.Toolkit.Wpf.UI.Controls` 에 설치 하는 NuGet 패키지 [2 부](modernize-wpf-tutorial-2.md)합니다.

사용 하려면 합니다 **WindowsXamlHost** WinRT Api WPF 앱의 코드에서 직접 호출 해야 제어 합니다. `Microsoft.Windows.SDK.Contracts` WinRT Api 앱에서 호출할 수 있습니다를 사용 하도록 설정 하는 데 필요한 참조를 포함 하는 NuGet 패키지. 이 패키지에도 포함 되어는 `Microsoft.Toolkit.Wpf.UI.Controls` 에 설치 하는 NuGet 패키지 [2 부](modernize-wpf-tutorial-2.md)합니다.

## <a name="add-the-windowsxamlhost-control"></a>WindowsXamlHost 컨트롤 추가

1. **솔루션 탐색기**를 확장 합니다 **뷰** 폴더에는 **ContosoExpenses.Core** 프로젝트를 두 번 클릭는 **AddNewExpense.xaml**파일입니다. 이러한 형식은 목록에 새 비용을 추가 하는 데 사용 합니다. 현재 버전의 앱에 표시는 다음과 같습니다.

    ![새 비용 추가](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF에 포함 된 날짜 선택 컨트롤은 마우스와 키보드를 사용 하 여 기존 컴퓨터에 대 한 것입니다. 터치 스크린을 사용 하 여 날짜를 선택 컨트롤 및 달력에서 매일 간에 제한 된 공간 크기를 작게 인해 실제로 적합 하지 않습니다.

2. 맨 위에 있는 합니다 **AddNewExpense.xaml** 파일에서 다음 특성을 추가 합니다 **창** 요소입니다.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    이 특성을 추가한 후 합니다 **창을** 요소에 이제이 처럼 보여야 합니다.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. 변경 합니다 **높이** 특성을 **창** 800 450에서 요소. 속성은 디자인 UWP **CalendarView** 컨트롤 WPF 날짜 선택 보다 더 많은 공간을 사용 합니다.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. 찾을 `DatePicker` 요소 파일의 아래쪽 및 다음 XAML을 사용 하 여이 요소를 바꿉니다.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    이 XAML을 추가 합니다 **WindowsXamlHost** 제어 합니다. 합니다 **InitialTypeName** 속성을 호스트 하려면 UWP 컨트롤의 전체 이름을 나타냅니다 (이 예에서 **Windows.UI.Xaml.Controls.CalendarView**).

5. F5 키를 눌러 빌드하고 디버거에서 앱을 실행 합니다. 목록에서 직원을 선택 하 고 다음 키를 누릅니다 합니다 **새 비용 추가** 단추입니다. 다음 페이지를 새 UWP를 호스팅하는 확인 **CalendarView** 제어 합니다.

    ![CalendarView 래퍼](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 앱을 종료합니다.

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost 컨트롤과 상호 작용

선택한 날짜를 처리 하 고, 화면에서 표시 한 다음 채우기 하도록 앱을 업데이트에서는 다음에 **비용** 데이터베이스에 저장 하는 개체입니다.

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 이 시나리오와 관련 된 두 멤버가 포함 되어 있습니다.

- 합니다 **SelectedDates** 속성 사용자가 선택한 날짜를 포함 합니다.
- 합니다 **SelectedDatesChanged** 날짜를 선택할 때 이벤트가 발생 합니다.

그러나 합니다 **WindowsXamlHost** 컨트롤은 일반 호스트 컨트롤에 대 한 *모든* UWP 컨트롤의 종류입니다. 따라서 라는 속성을 노출 하지 **SelectedDates** 또는 이벤트 호출 **SelectedDatesChanged**의 특정 이기 때문에는 **CalendarView** 컨트롤입니다. 이러한 멤버에 액세스 하려면 캐스팅 하는 코드를 작성 해야 합니다 **WindowsXamlHost** 에 **CalendarView** 형식입니다. 에 대 한 응답에는이 작업을 수행 하는 것이 좋습니다는 **ChildChanged** 이벤트를 **WindowsXamlHost** 호스팅된 컨트롤에서 렌더링 된 때 발생 하는 컨트롤입니다.

1. 에 **AddNewExpense.xaml** 파일에 이벤트 처리기를 추가 대 한를 **ChildChanged** 의 이벤트를 **WindowsXamlHost** 이전에 추가한 컨트롤입니다. 완료 되 면 합니다 **WindowsXamlHost** 요소는 다음과 같습니다.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 동일한 파일에서 찾습니다 합니다 **Grid.RowDefinitions** 기본 요소의 **그리드**합니다. 하나 더 추가 **RowDefinition** 요소를 **높이** 같음 **자동** 자식 요소 목록 끝에 있습니다. 완료 되 면 합니다 **Grid.RowDefinitions** 요소는 다음과 같습니다 (9 있어야 **RowDefinition** 요소).

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. 후 다음 XAML을 추가 합니다 **WindowsXamlHost** 요소 전과 합니다 **단추** 파일의 끝 부분에 요소입니다.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 찾을 **단추** 파일 및 변경의 끝 부분에 요소를 **Grid.Row** 속성을 **7** 에 **8**합니다. 이 새 행을 추가 했으므로 표의 한 행 아래로 단추를 이동 합니다.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 엽니다는 **AddNewExpense.xaml.cs** 코드 파일.

7. 파일의 맨 위에 다음 문을 추가 합니다.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 다음 이벤트 처리기를 추가 합니다 `AddNewExpense` 클래스입니다. 이 코드를 구현 하는 **ChildChanged** 의 이벤트를 **WindowsXamlHost** XAML 파일에 앞에서 선언한 제어 합니다.

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
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
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    이 코드를 사용 합니다 **자식** 의 속성을 **WindowsXamlHost** UWP 액세스 하기 위해 컨트롤 **CalendarView** 컨트롤입니다. 코드를 다음 구독을 **SelectedDatesChanged** 달력에서 날짜를 선택할 때 트리거되는 이벤트입니다. ViewModel에 선택한 날짜를 전달 하는이 이벤트 처리기는 새 **비용** 개체가 생성 되 고 데이터베이스에 저장 합니다. 이 작업을 수행 하는 코드는 MVVM Light NuGet 패키지에서 제공 하는 메시징 인프라를 사용 합니다. 코드 라고 하는 메시지를 보냅니다 **SelectedDateMessage** viewmodel에 수신 되며 설정 된 **날짜** 선택한 값을 사용 하 여 속성입니다. 이 시나리오에서는 합니다 **CalendarView** 컨트롤은 단일 선택 모드에 대 한 구성 되어 있으면 컬렉션에 요소가 하나만 포함 됩니다.

    이 시점에서 프로젝트 아직 컴파일되지 구현 없기 때문에 합니다 **SelectedDateMessage** 클래스입니다. 다음 단계는이 클래스를 구현 합니다.

9. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **메시지** 폴더 선택 **추가-> 클래스**합니다. 새 클래스 이름을 **SelectedDateMessage** 누릅니다 **추가**합니다.

10. 내용을 대체 합니다 **SelectedDateMessage.cs** 다음 코드를 사용 하 여 코드 파일. 이 코드를 사용 하 여 추가 문을 `GalaSoft.MvvmLight.Messaging` (MVVM Light NuGet 패키지)에서 네임 스페이스에서 상속 되는 **MessageBase** 클래스를 추가 **DateTime** 통해 초기화 된 속성은 public 생성자입니다. 완료 되 면 파일이 다음과 같아야 합니다.

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. 다음에이 메시지가를 채우는 ViewModel 업데이트 합니다 **날짜** ViewModel의 속성입니다. **솔루션 탐색기**를 확장 합니다 **Viewmodel** 폴더를 **AddNewExpenseViewModel.cs** 파일.

12. 에 대 한 public 생성자를 찾습니다는 `AddNewExpenseViewModel` 클래스 및 생성자의 끝에 다음 코드를 추가 합니다. 이 코드를 받으려면 등록를 **SelectedDateMessage**, 통해에서 선택한 날짜를 추출 합니다 **SelectedDate** 고 속성을 설정 하려면 사용 합니다 **날짜** 속성 ViewModel에 의해 노출. 이 속성을 사용 하 여 바인딩되므로 합니다 **TextBlock** 제어 하면 이전에 추가한 사용자가 선택한 날짜를 볼 수 있습니다.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    완료 되 면는 `AddNewExpenseViewModel` 생성자는 다음과 같습니다. 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > `SaveExpenseCommand` 메서드 같은 코드 파일에 비용을 데이터베이스에 저장 하는 작업을 수행 합니다. 이 메서드를 사용 합니다 **날짜** 만들 새 속성 **비용** 개체입니다. 이 속성에서 채워지는 중인 이제는 **CalendarView** 를 통해 제어할를 `SelectedDateMessage` 방금 메시지가 생성 합니다.

13. F5 키를 눌러 빌드하고 디버거에서 앱을 실행 합니다. 사용할 수 있는 직원 중 하나를 선택 하 고 클릭 합니다 **새 비용 추가** 단추입니다. 폼의 모든 필드를 완료 하 고 새 날짜를 선택할 **CalendarView** 제어 합니다. 선택한 날짜 컨트롤 아래 문자열로 표시 되는지 확인 합니다.

14. 키를 눌러 합니다 **저장할** 단추입니다. 폼 닫히고 새 지출 비용 목록의 끝에 추가 됩니다. 지출 날짜를 사용 하 여 첫 번째 열에서 선택한 날짜를 해야 합니다 **CalendarView** 컨트롤.

## <a name="next-steps"></a>다음 단계

이 시점에서 자습서에서는 성공적으로 대체 되었습니다 WPF 날짜 시간 컨트롤을 사용 하 여 UWP **CalendarView** 터치 및 마우스 및 키보드 입력 하는 것 외에도 디지털 펜 지 제어 합니다. Windows 커뮤니티 도구 키트에는 UWP의 래핑된 버전을 제공 하지 않지만 **CalendarView** 제어 하는 WPF 앱에서 직접 사용할 수 있는 제네릭을 사용 하 여 컨트롤을 호스팅할 수 있었던 [WindowsXamlHost ](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 제어 합니다.

에 대 한 준비가 [4 부: Windows 10 사용자 동작 및 알림을 추가](modernize-wpf-tutorial-4.md)합니다.
