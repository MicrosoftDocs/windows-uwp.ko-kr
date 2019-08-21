---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가 하 고, MSIX 개의 패키지를 만들고, 기타 최신 구성 요소를 WPF 앱에 통합 하는 방법을 보여 줍니다.
title: XAML Islands를 사용하여 UWP CalendarView 컨트롤 추가
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643369"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>3부: XAML Islands를 사용하여 UWP CalendarView 컨트롤 추가

Contoso 지출 이라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법을 보여 주는 자습서의 세 번째 부분입니다. 샘플 앱을 다운로드 하기 위한 자습서, 필수 구성 요소 및 지침에 대 한 개요를 [보려면 자습서: WPF 앱](modernize-wpf-tutorial.md)을 현대화 합니다. 이 문서에서는 [2 부](modernize-wpf-tutorial-2.md)를 이미 완료 했다고 가정 합니다.

이 자습서의 가상 시나리오에서 Contoso development 팀은 터치 사용 장치에서 경비 보고서의 날짜를 더 쉽게 선택할 수 있도록 하려고 합니다. 자습서의이 부분에서는 앱에 UWP [Calendarview](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) 컨트롤을 추가 합니다. 이는 작업 표시줄의 Windows 10 날짜 및 시간 기능에 사용 되는 컨트롤과 동일 합니다.

![CalendarViewControl 이미지](images/wpf-modernize-tutorial/CalendarViewControl.png)

[2 부](modernize-wpf-tutorial-2.md)에서 추가한 **InkCanvas** 컨트롤과 달리 WINDOWS Community Toolkit은 WPF 앱에서 사용할 수 있는 UWP **calendarview** 의 래핑된 버전을 제공 하지 않습니다. 대신 일반 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤에서 **InkCanvas** 를 호스팅합니다. 이 컨트롤을 사용 하 여 Windows SDK 또는 WinUI 라이브러리나 타사에서 만든 사용자 지정 UWP 컨트롤에서 제공 하는 자사 UWP 컨트롤을 호스트할 수 있습니다. **Windowsxamlhost** 컨트롤은 `Microsoft.Toolkit.Wpf.UI.XamlHost` 패키지 NuGet 패키지에서 제공 됩니다. 이 패키지는 [2 부에서](modernize-wpf-tutorial-2.md)설치한 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 패키지에 포함 되어 있습니다.

> [!NOTE]
> 이 자습서에서는 [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 를 사용 하 여 Windows SDK에서 제공 하는 자사 **calendarview** 컨트롤을 호스트 하는 방법을 보여 줍니다. 사용자 지정 컨트롤을 호스트 하는 방법을 보여 주는 연습은 [XAML 아일랜드를 사용 하 여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스팅](host-custom-control-with-xaml-islands.md)을 참조 하세요.

**Windowsxamlhost** 컨트롤을 사용 하려면 WPF 앱의 코드에서 WinRT api를 직접 호출 해야 합니다. `Microsoft.Windows.SDK.Contracts` NuGet 패키지에는 앱에서 WinRT api를 호출할 수 있도록 하는 데 필요한 참조가 포함 되어 있습니다. 이 패키지는 `Microsoft.Toolkit.Wpf.UI.Controls` [2 부에서](modernize-wpf-tutorial-2.md)설치한 NuGet 패키지에도 포함 되어 있습니다.

## <a name="add-the-windowsxamlhost-control"></a>WindowsXamlHost 컨트롤 추가

1. **솔루션 탐색기**에서 **ContosoExpenses** 프로젝트의 **Views** 폴더를 확장 하 고 **AddNewExpense** 파일을 두 번 클릭 합니다. 새 비용을 목록에 추가 하는 데 사용 되는 양식입니다. 앱의 현재 버전에 표시 되는 방법은 다음과 같습니다.

    ![새 비용 추가](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF에 포함 된 날짜 선택 컨트롤은 마우스 및 키보드를 사용 하는 기존 컴퓨터를 위한 것입니다. 터치 스크린을 사용 하 여 날짜를 선택 하는 것은 작은 크기의 컨트롤과 달력의 각 날짜 사이에 제한 된 공간으로 인해 그다지 유용 하지 않습니다.

2. **AddNewExpense** 파일의 맨 위에서 **Window** 요소에 다음 특성을 추가 합니다.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    이 특성을 추가한 후에는 이제 **Window** 요소가 다음과 같이 표시 됩니다.

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

3. **Window** 요소의 **Height** 특성을 450에서 800로 변경 합니다. 이는 UWP **Calendarview** 컨트롤이 WPF 날짜 선택기 보다 더 많은 공간을 차지 하기 때문에 필요 합니다.

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

4. 파일의 아래쪽 근처에서 요소를찾아이요소를다음XAML로바꿉니다.`DatePicker`

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    이 XAML은 **Windowsxamlhost** 컨트롤을 추가 합니다. **Initialtypename** 속성은 호스팅할 UWP 컨트롤의 전체 이름 (이 경우에는 **Windows.** x m l. x m l. x m l. x m l. x m l. x m l)을 나타냅니다.

5. F5 키를 눌러 디버거에서 앱을 빌드하고 실행 합니다. 목록에서 직원을 선택 하 고 **새 비용 추가** 단추를 누릅니다. 다음 페이지가 새 UWP **calendarview** 컨트롤을 호스트 하는지 확인 합니다.

    ![CalendarView 래퍼](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 앱을 종료합니다.

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost 컨트롤과 상호 작용

다음으로 앱을 업데이트 하 여 선택한 날짜를 처리 하 고, 화면에 표시 하 고, **비용** 개체를 채워 데이터베이스에 저장 합니다.

UWP [Calendarview](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 에는이 시나리오와 관련 된 두 개의 멤버가 포함 되어 있습니다.

- **Selecteddates** 속성은 사용자가 선택한 날짜를 포함 합니다.
- 사용자가 날짜를 선택 하면 **SelectedDatesChanged** 이벤트가 발생 합니다.

그러나 **Windowsxamlhost** 컨트롤은 *모든* 종류의 UWP 컨트롤에 대 한 일반 호스트 컨트롤입니다. 따라서이 속성은 **Calendarview** 컨트롤의 특정 이므로 **Selecteddates** 또는 **SelectedDatesChanged**이라는 이벤트 라는 속성을 노출 하지 않습니다. 이러한 멤버에 액세스 하려면 **Windowsxamlhost** 를 **calendarview** 형식으로 캐스팅 하는 코드를 작성 해야 합니다. 이 작업을 수행 하는 가장 좋은 장소는 호스트 된 컨트롤이 렌더링 될 때 발생 하는 **Windowsxamlhost** 컨트롤의 **childchanged** 이벤트에 대 한 응답입니다.

1. **AddNewExpense** 파일에서 이전에 추가한 **windowsxamlhost** 컨트롤의 **childchanged** 이벤트에 대 한 이벤트 처리기를 추가 합니다. 완료 되 면 **Windowsxamlhost** 요소가 다음과 같이 표시 됩니다.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 동일한 파일에서 기본 **표의** **표. rowdefinitions** 요소를 찾습니다. 자식 요소 목록의 끝에 **Auto** 와 같은 **높이가** 있는 **rowdefinition** 요소를 하나 더 추가 합니다. 완료 되 면 **표. Rowdefinitions** 요소가 다음과 같이 표시 됩니다 (이제는 9 개의 **rowdefinitions** 요소가 있어야 함).

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

4. **Windowsxamlhost** 요소 뒤에 다음 XAML을 추가 하 고 파일의 끝 부분에 있는 **Button** 요소 앞에 다음 XAML을 추가 합니다.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 파일 끝 근처에 있는 **Button** 요소를 찾아서 **표. 행** 속성을 **7** 에서 **8**로 변경 합니다. 그러면 새 행을 추가 했으므로이 단추를 모눈의 한 행 아래로 이동 합니다.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. **AddNewExpense.xaml.cs** 코드 파일을 엽니다.

7. 파일의 맨 위에 다음 문을 추가 합니다.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. `AddNewExpense` 클래스에 다음 이벤트 처리기를 추가합니다. 이 코드는 이전에 XAML 파일에서 선언한 **Windowsxamlhost** 컨트롤의 **childchanged** 이벤트를 구현 합니다.

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

    이 코드는 **Windowsxamlhost** 컨트롤의 **자식** 속성을 사용 하 여 UWP **calendarview** 컨트롤에 액세스 합니다. 그런 다음 코드는 **SelectedDatesChanged** 이벤트를 구독 합니다 .이 이벤트는 사용자가 달력에서 날짜를 선택할 때 트리거됩니다. 이 이벤트 처리기는 선택한 날짜를 새 **비용** 개체가 생성 되어 데이터베이스에 저장 된 ViewModel에 전달 합니다. 이렇게 하기 위해이 코드에서는 MVVM Light NuGet 패키지에서 제공 하는 메시징 인프라를 사용 합니다. 이 코드는 **SelectedDateMessage** 라는 메시지를 ViewModel에 보냅니다. 그러면이 메시지를 수신 하 고 선택한 값을 사용 하 여 **Date** 속성을 설정 합니다. 이 시나리오에서 **Calendarview** 컨트롤은 단일 선택 모드에 대해 구성 되므로 컬렉션에는 요소가 하나만 포함 됩니다.

    이 시점에서 프로젝트는 **SelectedDateMessage** 클래스에 대 한 구현이 누락 되었으므로 여전히 컴파일되지 않습니다. 다음 단계에서는이 클래스를 구현 합니다.

9. **솔루션 탐색기**에서 **Messages** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가-> 클래스**를 선택 합니다. 새 클래스의 이름을 **SelectedDateMessage** 하 고 **추가**를 클릭 합니다.

10. **SelectedDateMessage.cs** 코드 파일의 내용을 다음 코드로 바꿉니다. 이 코드는 MVVM Light NuGet 패키지에서 `GalaSoft.MvvmLight.Messaging` 네임 스페이스에 대 한 using 문을 추가 하 고, **messagebase** 클래스에서 상속 하며, public 생성자를 통해 초기화 된 **DateTime** 속성을 추가 합니다. 완료 되 면 파일은 다음과 같이 표시 됩니다.

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

11. 다음에는이 메시지를 받고 ViewModel의 **Date** 속성을 채우기 위해 viewmodel을 업데이트 합니다. **솔루션 탐색기**에서 **viewmodels** 폴더를 확장 하 고 **AddNewExpenseViewModel.cs** 파일을 엽니다.

12. `AddNewExpenseViewModel` 클래스에 대 한 public 생성자를 찾아 생성자의 끝에 다음 코드를 추가 합니다. 이 코드는 **SelectedDateMessage**를 수신 하 고, **SelectedDate** 속성을 통해 선택 된 날짜를 추출 하 고, ViewModel에서 노출 하는 **날짜** 속성을 설정 하는 데 사용 합니다. 이 속성은 이전에 추가한 **TextBlock** 컨트롤과 바인딩되므로 사용자가 선택한 날짜를 볼 수 있습니다.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    완료 되 면 생성자는 `AddNewExpenseViewModel` 다음과 같이 표시 됩니다. 

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
    > 동일한 `SaveExpenseCommand` 코드 파일의 메서드는 데이터베이스에 비용을 저장 하는 작업을 수행 합니다. 이 메서드는 **Date** 속성을 사용 하 여 새 **Expense** 개체를 만듭니다. 이 속성은 이제 방금 만든 메시지를 `SelectedDateMessage` 통해 **calendarview** 컨트롤로 채워집니다.

13. F5 키를 눌러 디버거에서 앱을 빌드하고 실행 합니다. 사용 가능한 직원 중 하나를 선택 하 고 **새 비용 추가** 단추를 클릭 합니다. 양식의 모든 필드를 완성 하 고 새 **Calendarview** 컨트롤에서 날짜를 선택 합니다. 선택한 날짜가 컨트롤 아래에 문자열로 표시 되는지 확인 합니다.

14. **저장** 단추를 누릅니다. 양식이 닫히고 새 비용이 경비 목록의 끝에 추가 됩니다. 비용 날짜가 인 첫 번째 열은 **Calendarview** 컨트롤에서 선택한 날짜 여야 합니다.

## <a name="next-steps"></a>다음 단계

자습서의이 시점에서 WPF 날짜 시간 컨트롤은 마우스 및 키보드 입력 외에도 터치 및 디지털 펜을 지 원하는 UWP **Calendarview** 컨트롤로 대체 했습니다. Windows 커뮤니티 도구 키트는 WPF 앱에서 직접 사용할 수 있는 UWP **Calendarview** 컨트롤의 래핑된 버전을 제공 하지 않지만 일반 [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용 하 여 컨트롤을 호스트할 수 있습니다.

이제 4 부를 사용할 [준비가 되었습니다. Windows 10 사용자 활동 및 알림을](modernize-wpf-tutorial-4.md)추가 합니다.
