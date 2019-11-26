---
title: 고객 데이터베이스 애플리케이션 만들기
description: 고객 데이터베이스 응용 프로그램을 만들고 기본적인 엔터프라이즈 앱 기능을 구현 하는 방법을 알아봅니다.
keywords: 엔터프라이즈, 자습서, 고객, 데이터, 읽기 업데이트 삭제, REST, 인증 만들기
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258614"
---
# <a name="tutorial-create-a-customer-database-application"></a>자습서: 고객 데이터베이스 응용 프로그램 만들기

이 자습서에서는 고객 목록을 관리 하기 위한 간단한 앱을 만듭니다. 이렇게 하면 UWP의 엔터프라이즈 앱에 대 한 기본 개념을 선택할 수 있습니다. 다음에 대한 방법을 알아봅니다.

* 로컬 SQL 데이터베이스에 대해 만들기, 읽기, 업데이트 및 삭제 작업을 구현 합니다.
* UI에서 고객 데이터를 표시 하 고 편집 하는 데이터 표를 추가 합니다.
* UI 요소를 기본 양식 레이아웃으로 함께 정렬 합니다.

이 자습서의 시작 지점은 간단한 버전의 [Customer Orders 데이터베이스 샘플 앱](https://github.com/Microsoft/Windows-appsample-customers-orders-database)을 기반으로 하는 최소한의 UI 및 기능을 갖춘 단일 페이지 앱입니다. 이는 C# 및 XAML로 작성 되었으며 이러한 언어에 대 한 기본적인 지식이 있어야 합니다.

![작업 중인 응용 프로그램의 기본 페이지](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>전제 조건

* [최신 버전의 Visual Studio 및 Windows 10 SDK가 있는지 확인 합니다.](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [고객 데이터베이스를 복제 하거나 다운로드 자습서 샘플](https://github.com/microsoft/windows-tutorials-customer-database)

리포지토리를 복제/다운로드 한 후에는 Visual Studio에서 **CustomerDatabaseTutorial** 를 열어 프로젝트를 편집할 수 있습니다.

> [!NOTE]
> 이 자습서에서 기반으로 하는 앱을 보려면 [전체 고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 을 확인 하세요.

## <a name="part-1-code-of-interest"></a>1 부: 관심 있는 코드

앱을 연 후 바로 앱을 실행 하면 빈 화면 위쪽에 몇 가지 단추가 표시 됩니다. 사용자에 게 표시 되지 않지만 응용 프로그램에는 소수의 테스트 고객과 함께 프로 비전 된 로컬 SQLite 데이터베이스가 이미 포함 되어 있습니다. 여기에서 먼저 해당 고객을 표시 하는 UI 컨트롤을 구현한 다음 데이터베이스에 대 한 작업에서 추가로 이동 합니다. 시작 하기 전에 다음 작업을 수행 합니다.

### <a name="views"></a>뷰

**Customerlistpage. xaml** 은이 자습서의 단일 페이지에 대 한 UI를 정의 하는 앱의 뷰입니다. UI의 시각적 요소를 추가 하거나 변경 해야 하는 경우 언제 든 지이 파일에서 작업을 수행할 수 있습니다. 이 자습서에서는 다음 요소를 추가 하는 과정을 안내 합니다.

* 고객을 표시 하 고 편집 하기 위한 **RadDataGrid** 입니다. 
* 새 고객의 초기 값을 설정 하는 **StackPanel** 입니다.

### <a name="viewmodels"></a>ViewModels

**Viewmodels\customerlistpageviewmodel.cs** 는 앱의 기본 논리가 있는 위치입니다. 뷰에서 취한 모든 사용자 작업은 처리를 위해이 파일에 전달 됩니다. 이 자습서에서는 몇 가지 새 코드를 추가 하 고 다음 메서드를 구현 합니다.

* **CreateNewCustomerAsync**는 새 customerviewmodel 개체를 초기화 합니다.
* **DeleteNewCustomerAsync**는 UI에 표시 되기 전에 새 고객을 제거 합니다.
* **DeleteAndUpdateAsync**는 삭제 단추의 논리를 처리 합니다.
* **Getcustomerlistasync**-데이터베이스에서 고객 목록을 검색 합니다.
* **Saveinitial인 async**-새 고객의 정보를 데이터베이스에 추가 합니다.
* **UpdateCustomersAsync**는 UI를 새로 고쳐 추가 되거나 삭제 된 고객을 반영 합니다.

**Customerviewmodel** 은 최근에 수정 되었는지 여부를 추적 하는 고객 정보에 대 한 래퍼입니다. 이 클래스에 아무 것도 추가 하지 않아도 되지만 다른 곳에서 추가 하는 코드 중 일부는이를 참조 합니다.

샘플을 구성 하는 방법에 대 한 자세한 내용은 [앱 구조 개요](../enterprise/customer-database-app-structure.md)를 확인 하세요.

## <a name="part-2-add-the-datagrid"></a>2 부: DataGrid 추가

고객 데이터에 대 한 작업을 시작 하기 전에 해당 고객을 표시 하는 UI 컨트롤을 추가 해야 합니다. 이렇게 하려면 미리 만들어진 타사 **RadDataGrid** 컨트롤을 사용 합니다. **Telerik Microsoft.netcore.universalwindowsplatform** NuGet 패키지는이 프로젝트에 이미 포함 되어 있습니다. 프로젝트에 그리드를 추가 해 보겠습니다.

1. 솔루션 탐색기에서 **Views\CustomerListPage.xaml** 을 엽니다. **페이지** 태그 내에 다음 코드 줄을 추가 하 여 데이터 그리드를 포함 하는 Telerik 네임 스페이스에 대 한 매핑을 선언 합니다.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. 보기의 주 **RelativePanel** 에 있는 **명령 모음** 아래에서 몇 가지 기본 구성 옵션을 사용 하 여 **RadDataGrid** 컨트롤을 추가 합니다.

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. 데이터 그리드를 추가 했지만 데이터가 표시 되어야 합니다. 다음 코드 줄을 추가 합니다.

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    표시할 데이터 원본을 정의 했으므로 **RadDataGrid** 에서 대부분의 UI 논리를 처리 합니다. 그러나 프로젝트를 실행 하는 경우에도 계속 표시 되는 데이터는 표시 되지 않습니다. ViewModel은 아직 로드 하지 않기 때문입니다.

![비어 있는 앱 (고객 없음)](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>3 부: 고객 읽기

초기화 될 때 **Viewmodelss\customerlistpageviewmodel.cs** 는 **Getcustomerlistasync** 메서드를 호출 합니다. 해당 메서드는 자습서에 포함 된 SQLite 데이터베이스에서 테스트 고객 데이터를 검색 해야 합니다.

1. **Viewmodels\customerlistpageviewmodel.cs**에서 다음 코드로 **Getcustomerlistasync** 메서드를 업데이트 합니다.

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    **Getcustomerlistasync** 메서드는 ViewModel이 로드 될 때 호출 되지만이 단계 전에는 아무것도 수행 하지 않습니다. 여기에서 **리포지토리/SqlCustomerRepository**의 **getasync** 메서드에 대 한 호출을 추가 했습니다. 이렇게 하면 it 부서에서 리포지토리에 연결 하 여 고객 개체의 열거 가능한 컬렉션을 검색할 수 있습니다. 그런 다음 해당 개체를 내부 **system.collections.objectmodel.observablecollection** 에 추가 하기 전에 개별 개체로 구문 분석 하 여 표시 하 고 편집할 수 있습니다.

2. 앱 실행-이제 고객 목록을 표시 하는 데이터 표가 표시 됩니다.

![고객의 초기 목록](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>4 부: 고객 편집

데이터 표에서 항목을 두 번 클릭 하 여 편집할 수 있지만 UI에서 변경한 내용이 코드 숨김으로 된 고객 컬렉션에도 적용 되도록 해야 합니다. 즉, 양방향 데이터 바인딩을 구현 해야 합니다. 이에 대 한 자세한 내용은 [데이터 바인딩 소개](../get-started/display-customers-in-list-learning-track.md)를 참조 하세요.

1. 먼저 **INotifyPropertyChanged** 인터페이스를 구현 하는 **Viewmodels\customerlistpageviewmodel.cs** 를 선언 합니다.

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 그런 다음 클래스의 주 본문 내에 다음 이벤트 및 메서드를 추가 합니다.

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    **OnPropertyChanged** 메서드를 사용 하면 setter가 양방향 데이터 바인딩에 필요한 **PropertyChanged** 이벤트를 쉽게 발생 시킬 수 있습니다.

3. 다음 함수 호출을 사용 하 여 **Selectedcustomer** 의 setter를 업데이트 합니다.

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. **Views\CustomerListPage.xaml**에서 **selectedcustomer** 속성을 데이터 표에 추가 합니다.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    이렇게 하면 데이터 표에 있는 사용자의 선택 내용이 코드 숨김으로 해당 하는 고객 개체와 연결 됩니다. TwoWay 바인딩 모드를 사용 하면 UI에서 변경한 내용이 해당 개체에 반영 될 수 있습니다.

5. 앱을 실행 합니다. 이제 표에 표시 된 고객을 확인 하 고 UI를 통해 기본 데이터를 변경할 수 있습니다.

![데이터 표에서 고객 편집](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>5 부: 고객 업데이트

이제 고객을 확인 하 고 편집할 수 있으므로 변경 내용을 데이터베이스에 푸시 하 고 다른 사용자가 변경한 모든 업데이트를 끌어올 수 있어야 합니다.

1. **Viewmodelss\customerlistpageviewmodel.cs**로 돌아가서 **UpdateCustomersAsync** 메서드로 이동 합니다. 이 코드를 사용 하 여 변경 내용을 데이터베이스에 푸시하고 새 정보를 검색 하려면이 코드를 업데이트 합니다.

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    이 코드는 사용자가 변경 될 때마다 자동으로 업데이트 되는 **Viewmodels\customerviewmodel.cs**의 **ismodified** 속성을 활용 합니다. 이렇게 하면 불필요 한 호출을 방지 하 고 업데이트 된 고객의 변경 내용만 데이터베이스로 푸시할 수 있습니다.

## <a name="part-6-create-a-new-customer"></a>6 부: 새 고객 만들기

새 고객을 추가 하면 해당 속성에 대 한 값을 제공 하기 전에 UI에 추가 하는 경우에는 빈 행으로 표시 되기 때문에 문제가 발생 합니다. 이것은 문제가 되지 않지만 여기에서 고객의 초기 값을 쉽게 설정할 수 있습니다. 이 자습서에서는 간단한 축소 가능 패널을 추가 하지만 추가 정보를 제공 하는 경우이 용도로 별도의 페이지를 만들 수 있습니다.

### <a name="update-the-code-behind"></a>코드 숨김이 업데이트 합니다.

1. 새 전용 필드 및 공용 속성을 **Viewmodelss\customerlistpageviewmodel.cs**에 추가 합니다. 이는 패널의 표시 여부를 제어 하는 데 사용 됩니다.

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. **AddingNewCustomer**값의 역함수 인 ViewModel에 새 public 속성을 추가 합니다. 패널이 표시 될 때 일반 명령 모음 단추를 사용 하지 않도록 설정 하는 데 사용 됩니다.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    이제 축소 가능 패널을 표시 하 고 고객을 만들어 해당 패널 내에서 편집할 수 있는 방법이 필요 합니다. 

3. 새 개인 fiend 및 public 속성을 ViewModel에 추가 하 여 새로 만든 고객을 보유 합니다.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  **CreateNewCustomerAsync** 메서드를 업데이트 하 여 새 고객을 만들고 리포지토리에 추가 하 고 선택한 고객으로 설정 합니다.

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. **Saveinitial된 비동기** 메서드를 업데이트 하 여 새로 만든 고객을 리포지토리에 추가 하 고, UI를 업데이트 하 고, 패널을 닫습니다.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. **AddingNewCustomer**에 대 한 setter의 마지막 줄에 다음 코드 줄을 추가 합니다.

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    이렇게 하면 **AddingNewCustomer** 가 변경 될 때마다 **EnableCommandBar** 이 자동으로 업데이트 됩니다.

### <a name="update-the-ui"></a>UI 업데이트

1. **Views\CustomerListPage.xaml**로 다시 이동 하 여 **명령 모음** 과 데이터 그리드 사이에 다음 속성을 포함 하는 **StackPanel** 을 추가 합니다.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    **X:load** 특성은 새 고객을 추가 하는 경우에만이 패널이 표시 되도록 합니다.

2. 새 패널이 나타날 때 아래로 이동 되도록 하려면 데이터 그리드 위치를 다음과 같이 변경 합니다.

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 4 개의 **TextBox** 컨트롤을 사용 하 여 스택 패널을 업데이트 합니다. 새 고객의 개별 속성에 바인딩하고 데이터 그리드에 추가 하기 전에 해당 값을 편집할 수 있습니다.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. 새 스택 패널에 간단한 단추를 추가 하 여 새로 만든 고객을 저장 합니다.

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. 스택 패널이 표시 될 때 일반 만들기, 삭제 및 업데이트 단추가 비활성화 되도록 **CommandBar**를 업데이트 합니다.

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. 앱을 실행 합니다. 이제 스택 패널에서 고객을 만들고 해당 데이터를 입력할 수 있습니다.

![새 고객 만들기](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>7 부: 고객 삭제

고객 삭제는 구현 해야 하는 최종 기본 작업입니다. 데이터 그리드 내에서 선택한 고객을 삭제 하면 UI를 업데이트 하기 위해 즉시 **UpdateCustomersAsync** 를 호출 하는 것이 좋습니다. 그러나 방금 만든 고객을 삭제 하는 경우에는 해당 메서드를 호출할 필요가 없습니다.

1. **Viewmodels\customerlistpageviewmodel.cs**로 이동 하 고 **DeleteAndUpdateAsync** 메서드를 업데이트 합니다.

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. **Views\CustomerListPage.xaml**에서 두 번째 단추를 포함 하도록 새 고객을 추가 하기 위해 스택 패널을 업데이트 합니다.

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. **Viewmodels\customerlistpageviewmodel.cs**에서 **DeleteNewCustomerAsync** 메서드를 업데이트 하 여 새 고객을 삭제 합니다.

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. 앱을 실행 합니다. 이제 데이터 그리드 내에서 또는 스택 패널에서 고객을 삭제할 수 있습니다.

![새 고객 삭제](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>결론

지금까지 이 작업이 완료 되 면 이제 앱은 전체 범위의 로컬 데이터베이스 작업을 수행 합니다. UI 내에서 고객을 만들고, 읽고, 업데이트 하 고, 삭제할 수 있으며 이러한 변경 내용은 데이터베이스에 저장 되며 응용 프로그램의 다른 시작에서 유지 됩니다.

이제 완료 되었으므로 다음 사항을 고려 하세요.
* 앱을 빌드하는 이유에 대 한 자세한 내용은 [앱 구조 개요](../enterprise/customer-database-app-structure.md) 를 참조 하세요.
* [전체 고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 을 탐색 하 여이 자습서의 기반이 되는 앱을 확인 합니다.

또는 문제를 해결 하는 경우 계속 진행할 수 있습니다.

## <a name="going-further-connect-to-a-remote-database"></a>자세히 살펴보기: 원격 데이터베이스에 연결

로컬 SQLite 데이터베이스에 대해 이러한 호출을 구현 하는 방법에 대 한 단계별 연습을 제공 합니다. 하지만 원격 데이터베이스를 사용 하려면 어떻게 해야 하나요?

이렇게 하려면 사용자 고유의 AAD (Azure Active Directory) 계정이 필요 하 고 고유한 데이터 원본을 호스트 하는 기능이 필요 합니다.

인증, REST 호출을 처리 하는 함수를 추가 하 고 상호 작용할 원격 데이터베이스를 만들어야 합니다. 전체 [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 에는 필요한 각 작업에 대해 참조할 수 있는 코드가 있습니다.

### <a name="settings-and-configuration"></a>설정 및 구성

사용자 고유의 원격 데이터베이스에 연결 하는 데 필요한 단계는 [샘플의 추가 정보](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)에 나와 있습니다. 다음 작업을 수행 해야 합니다.

* [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)에 Azure 계정 클라이언트 Id를 제공 합니다.
* 원격 데이터베이스의 url을 [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)에 제공 합니다.
* [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)에 데이터베이스에 대 한 연결 문자열을 제공 합니다.
* 앱을 Microsoft Store 연결 합니다.
* [서비스 프로젝트](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) 를 앱에 복사 하 고 Azure에 배포 합니다.

### <a name="authentication"></a>인증

사용자 정보를 수집 하려면 인증 시퀀스를 시작 하는 단추와 팝업 또는 별도의 페이지를 만들어야 합니다. 이를 만든 후에는 사용자의 정보를 요청 하 고이를 사용 하 여 액세스 토큰을 획득 하는 코드를 제공 해야 합니다. 고객 주문 데이터베이스 샘플은 **WebAccountManager** 라이브러리를 사용 하 여 호출 Microsoft Graph 래핑하여 토큰을 획득 하 고 AAD 계정에 대 한 인증을 처리 합니다.

* 인증 논리는 [**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)에서 구현 됩니다.
* 인증 프로세스는 사용자 지정 [**authenticationcontrol. xaml**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) 컨트롤에 표시 됩니다.

### <a name="rest-calls"></a>REST 호출

REST 호출을 구현 하기 위해이 자습서에서 추가한 코드를 수정할 필요가 없습니다. 대신, 다음을 수행 해야 합니다.

* **ICustomerRepository** 및 **ITutorialRepository** 인터페이스의 새 구현을 만들어 SQLite 대신 REST를 통해 동일한 함수 집합을 구현 합니다. JSON을 serialize 및 deserialize 해야 하며 필요한 경우 별도의 **Httphelper** 클래스에서 REST 호출을 래핑할 수 있습니다. 자세한 내용은 [전체 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) 을 참조 하세요.
* **App.xaml.cs**에서 REST 리포지토리를 초기화 하는 새 함수를 만들고 앱이 초기화 될 때 **SqliteDatabase** 대신이 함수를 호출 합니다. 다시, [전체 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)을 참조 하세요.

이러한 단계가 모두 완료 되 면 앱을 통해 AAD 계정에 인증할 수 있습니다. 원격 데이터베이스에 대 한 REST 호출은 로컬 SQLite 호출을 대체 하지만 사용자 환경은 동일 해야 합니다. 훨씬 더 많은 원대한 있는 경우 사용자가 둘 사이를 동적으로 전환할 수 있도록 설정 페이지를 추가할 수 있습니다.
