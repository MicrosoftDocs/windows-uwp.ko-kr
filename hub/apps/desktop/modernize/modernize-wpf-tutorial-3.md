---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, 다른 최신 구성 요소를 WPF 앱에 통합하는 방법을 보여 줍니다.
title: XAML Islands를 사용하여 UWP CalendarView 컨트롤 추가
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "69643369"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>3부: XAML Islands를 사용하여 UWP CalendarView 컨트롤 추가

Contoso Expenses라는 샘플 WPF 데스크톱 앱을 현대화하는 방법을 보여 주는 자습서의 세 번째 부분입니다. 자습서의 개요, 필수 조건 및 샘플 앱 다운로드 지침은 [자습서: WPF 앱 현대화](modernize-wpf-tutorial.md)를 참조하세요. 이 문서에서는 [2부](modernize-wpf-tutorial-2.md)를 이미 완료했다고 가정합니다.

이 자습서의 가상 시나리오에서는 Contoso 개발 팀이 터치 사용 디바이스에서 경비 보고서의 날짜를 더 쉽게 선택할 수 있게 하려고 합니다. 이 자습서 부분에서는 UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) 컨트롤을 앱에 추가합니다. 작업 표시줄의 Windows 10 날짜 및 시간 기능에 사용되는 것과 동일한 컨트롤입니다.

![CalendarViewControl 이미지](images/wpf-modernize-tutorial/CalendarViewControl.png)

[2부](modernize-wpf-tutorial-2.md)에서 추가한 **InkCanvas** 컨트롤과 달리, Windows 커뮤니티 도구 키트는 WPF 앱에서 사용할 수 있는 UWP **CalendarView**의 래핑된 버전을 제공하지 않습니다. 대신, 일반 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤에 **InkCanvas**를 호스트하겠습니다. 이 컨트롤을 사용하여 Windows SDK 또는 WinUI 라이브러리에서 제공하는 Microsoft UWP 컨트롤이나 타사에서 만든 사용자 지정 UWP 컨트롤을 호스트할 수 있습니다. **WindowsXamlHost** 컨트롤은 `Microsoft.Toolkit.Wpf.UI.XamlHost` NuGet 패키지에서 제공합니다. 이 패키지는 [2부](modernize-wpf-tutorial-2.md)에서 설치한 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 패키지에 포함되어 있습니다.

> [!NOTE]
> 이 자습서에서는 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)를 사용하여 Windows SDK에서 제공하는 Microsoft **CalendarView** 컨트롤을 호스트하는 방법을 보여 줍니다. 사용자 지정 컨트롤을 호스트하는 방법을 보여 주는 연습은 [XAML Islands를 사용하여 WPF 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands.md)를 참조하세요.

**WindowsXamlHost** 컨트롤을 사용하려면 WPF 앱의 코드에서 WinRT API를 직접 호출해야 합니다. `Microsoft.Windows.SDK.Contracts` NuGet 패키지에는 앱에서 WinRT API를 호출하는 데 필요한 참조가 포함되어 있습니다. 이 패키지도 [2부](modernize-wpf-tutorial-2.md)에서 설치한 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 패키지에 포함되어 있습니다.

## <a name="add-the-windowsxamlhost-control"></a>WindowsXamlHost 컨트롤 추가

1. **솔루션 탐색기**에서 **ContosoExpenses.Core** 프로젝트의 **Views** 폴더를 펼치고 **AddNewExpense.xaml** 파일을 두 번 클릭합니다. 새 비용을 목록에 추가하는 데 사용되는 양식입니다. 현재 앱 버전에서는 다음과 같이 표시됩니다.

    ![새 경비 추가](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF에 포함된 날짜 선택 컨트롤은 마우스와 키보드를 사용하는 기존 컴퓨터를 대상으로 합니다. 작은 컨트롤 크기와 달력의 각 날짜 간 공간 제한으로 인해 터치 스크린을 사용한 날짜 선택은 그다지 유용하지 않습니다.

2. **AddNewExpense.xaml** 파일 맨 위에 있는 **Window** 요소에 다음 특성을 추가합니다.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    이 특성을 추가한 후의 **Window** 요소는 다음과 같이 표시됩니다.

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

3. **Window** 요소의 **Height** 특성을 450에서 800으로 변경합니다. UWP **CalendarView** 컨트롤이 WPF 날짜 선택보다 많은 공간을 차지하기 때문에 필요한 작업입니다.

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

4. 파일 아래쪽에서 `DatePicker` 요소를 찾은 후에 이 요소를 다음 XAML로 바꿉니다.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    이 XAML은 **WindowsXamlHost** 컨트롤을 추가합니다. **InitialTypeName** 속성은 호스트하려는 UWP 컨트롤(이 경우에는 **Windows.UI.Xaml.Controls.CalendarView**)의 전체 이름을 나타냅니다.

5. F5 키를 눌러 디버거에서 앱을 빌드하고 실행합니다. 목록에서 직원을 선택하고 **새 경비 추가** 단추를 누릅니다. 다음 페이지가 새 UWP **CalendarView** 컨트롤을 호스트하는지 확인합니다.

    ![CalendarView 래퍼](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 앱을 종료합니다.

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost 컨트롤과 상호 작용

그다음에는 선택한 날짜를 처리하고, 화면에 표시하고, 데이터베이스에 저장할 **Expense** 개체를 채우도록 앱을 업데이트하겠습니다.

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)에는 이 시나리오와 관련된 다음 두 개의 멤버가 포함되어 있습니다.

- **SelectedDates** 속성은 사용자가 선택한 날짜를 포함합니다.
- 사용자가 날짜를 선택하면 **SelectedDatesChanged** 이벤트가 발생합니다.

그러나 **WindowsXamlHost** 컨트롤은 ‘모든’ 종류의 UWP 컨트롤에 대한 일반 호스트 컨트롤입니다.  따라서 **CalendarView** 컨트롤과 관련이 있는 **SelectedDates** 속성이나 **SelectedDatesChanged** 이벤트는 공개되지 않습니다. 해당 멤버에 액세스하려면 **WindowsXamlHost**를 **CalendarView** 형식으로 캐스팅하는 코드를 작성해야 합니다. 이 작업을 수행하기에 가장 적합한 위치는 호스트된 컨트롤을 렌더링할 때 발생하는 **WindowsXamlHost** 컨트롤의 **ChildChanged** 이벤트에 대한 응답입니다.

1. **AddNewExpense** 파일에서 이전에 추가한 **WindowsXamlHost** 컨트롤의 **ChildChanged** 이벤트에 대한 이벤트 처리기를 추가합니다. 완료된 후의 **WindowsXamlHost** 요소는 다음과 같이 표시됩니다.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 동일한 파일에서 기본 **Grid**의 **Grid.RowDefinitions** 요소를 찾습니다. 자식 요소 목록 끝에 **Height**가 **Auto**인 **RowDefinition** 요소를 하나 이상 추가합니다. 완료된 후의 **Grid.RowDefinitions** 요소는 다음과 같이 표시됩니다(이제 9개의 **RowDefinition** 요소가 있음).

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

4. 파일 끝 부분에서 **WindowsXamlHost** 요소 뒤, **Button** 요소 앞에 다음 XAML을 추가합니다.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 파일 끝 부분에서 **Button** 요소를 찾은 다음, **Grid.Row** 속성을 **7**에서 **8**로 변경합니다. 새 행을 추가했으므로 단추가 그리드에서 한 행 아래로 이동합니다.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. **AddNewExpense.xaml.cs** 코드 파일을 엽니다.

7. 다음 문을 파일 맨 위에 추가합니다.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. `AddNewExpense` 클래스에 다음 이벤트 처리기를 추가합니다. 이 코드는 XAML 파일의 앞부분에 선언한 **WindowsXamlHost** 컨트롤의 **ChildChanged** 이벤트를 구현합니다.

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

    이 코드는 **WindowsXamlHost** 컨트롤의 **Child** 속성을 사용하여 UWP **CalendarView** 컨트롤에 액세스합니다. 그런 다음, 사용자가 달력에서 날짜를 선택할 때 트리거되는 **SelectedDatesChanged** 이벤트를 구독합니다. 이 이벤트 처리기는 새 **Expense** 개체가 생성되어 데이터베이스에 저장되는 ViewModel에 선택한 날짜를 전달합니다. 이 작업을 위해 코드는 MVVM Light NuGet 패키지에서 제공하는 메시징 인프라를 사용합니다. 코드에서 **SelectedDateMessage**라는 메시지를 ViewModel로 전송하면, ViewModel은 메시지를 받아 **Date** 속성을 선택한 값으로 설정합니다. 이 시나리오에서는 **CalendarView** 컨트롤이 단일 선택 모드로 구성되었으므로 컬렉션에 요소가 하나만 포함됩니다.

    이 시점에는 **SelectedDateMessage** 클래스 구현이 없기 때문에 프로젝트가 여전히 컴파일되지 않습니다. 이 클래스는 다음 단계에서 구현합니다.

9. **솔루션 탐색기**에서 **Messages** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 -> 클래스**를 선택합니다. 새 클래스의 이름을 **SelectedDateMessage**로 지정하고 **추가**를 클릭합니다.

10. **SelectedDateMessage.cs** 코드 파일의 내용을 다음 코드로 바꿉니다. 이 코드는 MVVM Light NuGet 패키지의 `GalaSoft.MvvmLight.Messaging` 네임스페이스에 대한 using 문을 추가하고, **MessageBase** 클래스에서 상속하며, public 생성자를 통해 초기화되는 **DateTime** 속성을 추가합니다. 완료된 파일은 다음과 같이 표시됩니다.

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

11. 그다음에는 이 메시지를 받고 ViewModel의 **Date** 속성을 채우도록 ViewModel을 업데이트하겠습니다. **솔루션 탐색기**에서 **ViewModels** 폴더를 펼치고 **AddNewExpenseViewModel.cs** 파일을 엽니다.

12. `AddNewExpenseViewModel` 클래스의 public 생성자를 찾은 후에 다음 코드를 생성자 끝에 추가합니다. 이 코드는 **SelectedDateMessage**를 받고, **SelectedDate** 속성을 통해 이 메시지에서 선택한 날짜를 추출한 다음, ViewModel에서 공개하는 **Date** 속성을 설정하는 데 사용합니다. 이 속성은 이전에 추가한 **TextBlock** 컨트롤과 바인딩되어 있으므로 사용자가 선택한 날짜를 확인할 수 있습니다.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    완료된 `AddNewExpenseViewModel` 생성자는 다음과 같이 표시됩니다. 

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
    > 동일한 코드 파일의 `SaveExpenseCommand` 메서드는 데이터베이스에 경비를 저장하는 작업을 수행합니다. 이 메서드는 **Date** 속성을 사용하여 새 **Expense** 개체를 만듭니다. 이제 이 속성은 방금 만든 `SelectedDateMessage` 메시지를 통해 **CalendarView** 컨트롤에서 채워집니다.

13. F5 키를 눌러 디버거에서 앱을 빌드하고 실행합니다. 사용 가능한 직원 중 하나를 선택하고 **새 경비 추가** 단추를 클릭합니다. 양식의 모든 필드를 작성하고 새 **CalendarView** 컨트롤에서 날짜를 선택합니다. 선택한 날짜가 컨트롤 아래에 문자열로 표시되는지 확인합니다.

14. **저장** 단추를 누릅니다. 양식이 닫히고 새 경비가 경비 목록 끝에 추가됩니다. 경비 날짜가 포함된 첫 번째 열은 **CalendarView** 컨트롤에서 선택한 날짜여야 합니다.

## <a name="next-steps"></a>다음 단계

지금까지 자습서를 진행했다면 WPF 날짜 시간 컨트롤을 UWP **CalendarView** 컨트롤로 대체했습니다. 새 컨트롤은 마우스 및 키보드 입력 외에도 터치와 디지털 펜을 지원합니다. Windows Community Toolkit는 WPF 앱에서 직접 사용할 수 있는 UWP **CalendarView** 컨트롤의 래핑 버전을 제공하지 않지만, 일반 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤을 사용하여 컨트롤을 호스트할 수 있습니다.

이제 [4부: Windows 10 사용자 활동 및 알림 추가](modernize-wpf-tutorial-4.md)를 시작할 준비가 되었습니다.
