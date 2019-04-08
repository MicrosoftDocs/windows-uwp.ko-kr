---
title: 고객 데이터베이스 애플리케이션 만들기
description: 고객 데이터베이스 응용 프로그램을 만들고 기본 엔터프라이즈 앱 함수를 구현 하는 방법을 알아봅니다.
keywords: enterprise, 자습서, 고객 데이터를 읽고, 업데이트 delete REST 인증 만들기
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 9c09e0fb73e42fd8a3d0c70bbb5396be32624387
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623248"
---
# <a name="tutorial-create-a-customer-database-application"></a>자습서: 고객 데이터베이스 애플리케이션 만들기

이 자습서에는 고객 목록을 관리 하기 위한 간단한 앱을 만듭니다. 이 과정에서 다양 한 UWP에서 엔터프라이즈 앱에 대 한 기본 개념을 소개 합니다. 다음에 대한 방법을 알아봅니다.

* 만들기, 읽기, 업데이트를 구현 하 고 로컬 SQL 데이터베이스에 대 한 작업을 삭제.
* 데이터 표를 표시 하 고 편집 UI에서 고객 데이터를 추가 합니다.
* 기본 폼 레이아웃에서 UI 요소를 정렬 합니다.

이 자습서에 대 한 시작점은 최소한의 UI 및 기능, 간소화 된 버전을 기준으로 사용 하는 단일 페이지 앱을 [고객 주문 데이터베이스 샘플 앱](https://github.com/Microsoft/Windows-appsample-customers-orders-database)합니다. 작성 된 C# 를 XAML에 모두 해당 언어에 대 한 기본 지식이 있다면 기대 하 고 있습니다.

![작업 중인 앱의 기본 페이지](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>필수 구성 요소

* [Visual Studio 및 Windows 10 SDK의 최신 버전 확인](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [복제 또는 고객 데이터베이스 자습서 샘플 다운로드](https://aka.ms/customer-database-tutorial)

다운로드 한 후 복제/리포지토리를 열어 프로젝트를 편집할 수 있습니다 **CustomerDatabaseTutorial.sln** Visual Studio를 사용 하 여 합니다.

> [!NOTE]
> 체크 아웃 합니다 [전체 고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 이 자습서를 기반으로 앱을 봅니다.

## <a name="part-1-code-of-interest"></a>1부: 관심 코드

를 연 후에 즉시 앱을 실행 하는 경우 빈 화면 맨 위에 있는 단추를 몇 개 표시 됩니다. 볼 수 있지 않더라도 앱 몇 가지 테스트 고객과 프로 비전 된 로컬 SQLite 데이터베이스를 이미 포함 되어 있습니다. 여기에서 해당 고객에 게 표시 하는 UI 컨트롤을 구현 하 여 시작 하 고 데이터베이스에 대 한 작업에 추가 하 이동 합니다 수 있습니다. 시작 하기 전에 같습니다. 여기서 사용 하 게 됩니다.

### <a name="views"></a>보기

**CustomerListPage.xaml** 는이 자습서에는 단일 페이지에 대 한 UI를 정의 하는 앱의 보기입니다. 추가 하거나 ui에서 시각적 요소를 변경 하려면 언제 든 지 작업을이 파일에 있습니다. 이 자습서 이러한 요소를 추가 하는 과정을 안내 합니다.

* A **RadDataGrid** 표시 하 고 고객에 게 편집 합니다. 
* A **StackPanel** 새 고객에 대 한 초기 값을 설정 합니다.

### <a name="viewmodels"></a>Viewmodel

**ViewModels\CustomerListPageViewModel.cs** 앱의 기본 논리입니다. 보기에서 수행 되는 모든 사용자 동작 처리를 위해이 파일에 전달 됩니다. 이 자습서에서는 몇 가지 새 코드를 추가 하 고 다음 메서드를 구현 합니다.

* **CreateNewCustomerAsync**에 새 CustomerViewModel 개체를 초기화 하는 합니다.
* **DeleteNewCustomerAsync**, UI에 표시 되기 전에 새 고객을 제거 합니다.
* **DeleteAndUpdateAsync**를 delete 단추의 논리를 처리 하는 합니다.
* **GetCustomerListAsync**, 데이터베이스에서 고객 목록을 검색 하는 합니다.
* **SaveInitialChangesAsync**, 새 고객 정보 데이터베이스에 추가 합니다.
* **UpdateCustomersAsync**, 모든 고객은 추가 또는 삭제를 반영 하도록 UI를 새로 고치는 합니다.

**CustomerViewModel** 최근에 수정 되었는지 여부를 추적 하는 고객의 정보에 대 한 래퍼입니다. 이 클래스에 아무것도 추가 하지 않아도 됩니다. 하지만 일부 다른 곳에서 추가한 코드를 참조 합니다.

샘플 생성 방법에 대 한 자세한 내용은 체크 아웃 합니다 [앱 구조 개요](../enterprise/customer-database-app-structure.md)합니다.

## <a name="part-2-add-the-datagrid"></a>2부: DataGrid를 추가 합니다.

고객 데이터에서 작동 하도록 시작 하기 전에 해당 고객에 게 표시 하는 UI 컨트롤을 추가 해야 합니다. 이 위해 사용할 것을 미리 만든된 타사 **RadDataGrid** 제어 합니다. 합니다 **Telerik.UI.for.UniversalWindowsPlatform** NuGet 패키지는이 프로젝트에 이미 포함 되어 있습니다. 프로젝트에 표를 추가 하겠습니다.

1. 오픈 **Views\CustomerListPage.xaml** 솔루션 탐색기에서. 내에서 코드의 다음 줄을 추가 합니다 **페이지** 태그 데이터 그리드를 포함 하는 Telerik 네임 스페이스에 대 한 매핑을 선언 합니다.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. 아래는 **CommandBar** 주 내 **RelativePanel** 보기의 추가 **RadDataGrid** 컨트롤 몇 가지 기본 구성 옵션을 사용 하 여:

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

3. 데이터 표 추가 했으나 표시할 데이터가 필요 합니다. 다음 코드 줄을 추가 합니다.

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    표시할 데이터의 원본 정의한 했으므로 **RadDataGrid** 대부분의 UI 논리를 처리할 합니다. 그러나 프로젝트를 실행 하는 경우 여전히 볼 수 없습니다 데이터 표시. 이것은 ViewModel 아직 로드 되지 않습니다.

![고객이 없는 사용 하 여 비어 있는 앱](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>3부: 고객에 게 읽기

초기화 될 때 **ViewModels\CustomerListPageViewModel.cs** 호출을 **GetCustomerListAsync** 메서드. 메서드는 SQLite에서 테스트 고객 데이터를 검색 해야 하는 데이터베이스는 자습서에 포함 됩니다.

1. **ViewModels\CustomerListPageViewModel.cs**를 업데이트 하 **GetCustomerListAsync** 이 코드를 사용 하 여 메서드:

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
    합니다 **GetCustomerListAsync** 메서드는 ViewModel를 로드 하지만이 단계 전에 아무 것도 수행 되지 않은 것입니다. 에 대 한 호출을 추가 했습니다 여기에 **GetAsync** 에서 메서드 **리포지토리/SqlCustomerRepository**합니다. 이렇게 하면 고객 개체의 열거 가능한 컬렉션을 검색할 저장소에 연결할 수 있습니다. 다음 구문 분석 하 개별 개체로 내부에 추가 하기 전에 **ObservableCollection** 표시 및 편집할 수 있도록 합니다.

2. -앱을 실행 하면 고객의 목록을 표시 데이터 표를 이제 표시 됩니다.

![고객의 초기 목록](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>4부: 고객은 편집

데이터 그리드에서 항목을 두 번 클릭 하 여 편집할 수 있습니다 하지만 UI의 변경 내용이 코드 숨김에서 고객의 컬렉션에 내용이 있는지 확인 해야 합니다. 즉, 양방향 데이터 바인딩을 구현 해야 합니다. 이 대 한 자세한 정보를 하려는 경우 체크 아웃 우리의 [증상 소개](../get-started/display-customers-in-list-learning-track.md)합니다.

1. 먼저 선언 **ViewModels\CustomerListPageViewModel.cs** 구현 하는 **INotifyPropertyChanged** 인터페이스:

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 그런 다음 클래스의 본문 내에서 다음 이벤트와 메서드를 추가 합니다.

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    합니다 **OnPropertyChanged** 메서드를 사용 하면 쉽게 발생 하 여 setter는 **PropertyChanged** 양방향 데이터 바인딩을에 필요한 경우.

3. setter를 업데이트할 **SelectedCustomer** 이 함수 호출을 사용 하 여:

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

4. **Views\CustomerListPage.xaml**를 추가 합니다 **SelectedCustomer** 데이터 표의 속성입니다.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    그러면 사용자가 선택한 데이터 그리드에서 코드 숨김에 있는 해당 고객 개체를 사용 하 여 연결 됩니다. TwoWay 바인딩 모드의에서 변경을 내용을 UI 개체에 반영 될 수 있습니다.

5. 앱을 실행 합니다. 이제 표에 표시 된 고객 참조 하 고 UI를 통해 기본 데이터를 변경할 수 있습니다.

![데이터 그리드에서 고객을 편집합니다.](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>5부: 고객 업데이트

이제는 참조 하 고 고객에 게 편집할 수 있습니다, 다른 사용자가 적용 된 모든 업데이트를 가져오도록 및 데이터베이스에 변경 내용을 적용할 수 해야 합니다.

1. 돌아갑니다 **ViewModels\CustomerListPageViewModel.cs**로 이동 합니다 **UpdateCustomersAsync** 메서드. 이 코드를 사용 하 여 업데이트를 적용할 데이터베이스를 새 정보를 검색할 변경:

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
    이 코드를 활용 합니다 **IsModified** 속성을 **ViewModels\CustomerViewModel.cs**, 고객 변경 될 때마다 자동으로 업데이트 됩니다는 합니다. 이렇게 하면 불필요 한 호출을 방지 하 고만 데이터베이스에 업데이트 된 고객의 변경 내용 적용을 있습니다.

## <a name="part-6-create-a-new-customer"></a>6 부: 새 고객 만들기

새 고객이 추가 어렵습니다를 추가 하는 경우 해당 UI에 해당 속성에 대 한 값을 제공 하기 전에 고객 빈 행으로 표시 됩니다. 문제가 되지 않지만 여기에서는 더 쉽게 고객의 초기 값을 설정 합니다. 이 자습서에서는 간단한 축소 가능한 패널을 추가 하지만 추가 하려면 자세한 정보를 설치한 경우이 목적을 위해 별도 페이지를 만들 수 있습니다.

### <a name="update-the-code-behind"></a>관련 코드를 업데이트 합니다.

1. 새 개인 필드와 공용 속성을 추가 **ViewModels\CustomerListPageViewModel.cs**합니다. 이 패널에 표시 되는지 여부를 제어 하 게 됩니다.

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

2. Viewmodel에 역변환이 고, 값에 대 한 새 공용 속성을 추가할 **AddingNewCustomer**합니다. 이 패널에 표시 되는 경우 일반 명령 모음 단추를 사용 하지 않도록 설정 됩니다.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    축소 가능한 패널을 표시 하 고 그 편집 하려면 고객을 만드는 방법을 지금 해야 합니다. 

3. Viewmodel에 새로 만든된 고객을 보유 하는 새 개인 핀 드 및 공용 속성을 추가 합니다.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if {_newCustomer != value}
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  업데이트 프로그램 **CreateNewCustomerAsync** 메서드를 새 고객을 만들고 리포지토리에 추가 하 고 선택한 고객으로 설정 합니다.

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. 업데이트를 **SaveInitialChangesAsync** 리포지토리에 새로 만든 고객을 추가 하는 방법 UI를 업데이트 하 고 창을 닫습니다.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Setter의 마지막 줄으로 다음 코드 줄을 추가 **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    이렇게 하면 **EnableCommandBar** 자동으로 업데이트 될 때마다 **AddingNewCustomer** 변경 됩니다.

### <a name="update-the-ui"></a>UI 업데이트

1. 돌아갑니다 **Views\CustomerListPage.xaml**, 추가 **StackPanel** 간에 다음과 같은 속성을 사용 하 여 프로그램 **CommandBar** 및 데이터 표의:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    합니다 **x: 로드** 특성 새 고객을 추가 하는 경우이 패널만 표시 되는지 확인 합니다.

2. 다음과 같이 변경 하면 새 패널이 표시 되 면 이동 되도록 데이터 눈금의 위치를 확인 합니다.

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 과 함께 4 개의 프로그램 스택 패널을 업데이트 **텍스트 상자** 컨트롤입니다. 새 고객의 개별 속성에 바인딩할 하며 데이터 표를 추가 하기 전에 해당 값을 편집할 수 있도록 하겠습니다.

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

4. 새로 만든 고객을 저장 하 여 새 스택 패널에 간단한 단추를 추가 합니다.

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. 업데이트를 **CommandBar**이므로 스택 패널 표시 되는 경우 일반 만들기, 삭제 및 업데이트 단추가 비활성화 됩니다.

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. 앱을 실행 합니다. 이제 고객을 만드는 하 고 스택 패널에서 해당 데이터를 입력할 수 있습니다.

![새 고객 만들기](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>7 부: 고객 삭제

고객을 삭제 하는 구현 해야 하는 최종 기본 작업입니다. 즉시 호출 하려는 데이터 그리드 내에서 선택한 고객을 삭제 하면 **UpdateCustomersAsync** UI를 업데이트 하기 위해. 그러나 방금 만든 고객을 삭제 하는 경우 해당 메서드를 호출할 필요가 없습니다.

1. 이동할 **ViewModels\CustomerListPageViewModel.cs**, 및 업데이트 된 **DeleteAndUpdateAsync** 메서드:

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

2. **Views\CustomerListPage.xaml**, 두 번째 단추를 포함 하도록 새 고객을 추가 하는 것에 대 한 스택 패널을 업데이트 합니다.

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

3. **ViewModels\CustomerListPageViewModel.cs**를 업데이트 합니다 **DeleteNewCustomerAsync** 새 고객을 삭제 하는 방법.

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

![새 고객을 삭제 합니다.](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>결론

축하합니다. 앱은 모든이 수행 하면 이제 로컬 데이터베이스 작업의 전체 범위를 있습니다. 만들기, 읽기, 업데이트 및 하면 UI 내에서 고객을 삭제할 수 있습니다 하 고 이러한 변경 내용을 데이터베이스에 저장 되 고 앱의 다른 실행 간에 유지 됩니다.

이 과정을 완료 했으므로 다음을 고려 합니다.
* 이미 않았다면 체크 아웃 합니다 [앱 구조 개요](../enterprise/customer-database-app-structure.md) 어떻게 앱을 빌드할 하는 이유에 대 한 자세한 내용은 합니다.
* 탐색 합니다 [전체 고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 이 자습서를 기반으로 앱을 봅니다.

또는 계속 수 인 경우 챌린지를 등록 하는 중...

## <a name="going-further-connect-to-a-remote-database"></a>계속 진행 합니다. 원격 데이터베이스에 연결

로컬 SQLite 데이터베이스에 대해 이러한 호출을 구현 하는 방법의 단계별 연습을 제공 했습니다. 그러나 원격 데이터베이스를 대신 사용 하려는 경우에 어떻게?

시도해 하려는 경우에 고유한 Azure Active Directory (AAD) 계정 및 사용자 지정 데이터 소스를 호스트 하는 기능 해야 합니다.

인증을 REST 호출을 처리 한 다음 원격 데이터베이스와 상호 작용 하는 함수를 추가 해야 합니다. 전체에 코드가 있으면 [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 필요한 각 작업에 대 한 참조 수입니다.

### <a name="settings-and-configuration"></a>설정 및 구성

사용자 고유의 원격 데이터베이스에 연결 하는 데 필요한 단계에 명시 되는 [샘플의 추가 정보](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)합니다. 다음을 수행 해야 합니다.

* Azure 계정 클라이언트 Id를 제공 하려면 [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)합니다.
* 원격 데이터베이스의 url을 제공 [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)합니다.
* 데이터베이스에 대 한 연결 문자열을 제공 [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)합니다.
* Microsoft Store 사용 하 여 앱을 연결 합니다.
* 복사 합니다 [서비스 프로젝트](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) 를 앱에 Azure에 배포 합니다.

### <a name="authentication"></a>Authentication

인증 시퀀스 및 팝업 또는 사용자의 정보를 수집 하는 별도 페이지를 시작 하려면 단추를 만들려면 해야 합니다. 하는 작업을 만든 후에 사용자의 정보를 요청 하 고 액세스 토큰을 획득 하는 데 사용 하는 코드를 제공 해야 합니다. 고객 주문 데이터베이스 샘플 사용 하 여 Microsoft Graph 호출을 래핑하는 **WebAccountManager** 토큰을 획득 하 고 AAD 계정에 대 한 인증을 처리 하는 라이브러리입니다.

* 인증 논리를 구현 [ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)합니다.
* 인증 프로세스 사용자 지정에 표시 됩니다 [ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) 제어 합니다.

### <a name="rest-calls"></a>REST 호출

수정 하지 않아도 코드의 추가한이 자습서에서는 REST 호출을 구현 하기 위해. 대신, 다음을 수행 해야 합니다.

* 새 구현을 만들 합니다 **ICustomerRepository** 및 **ITutorialRepository** 인터페이스, SQLite 대신 REST 통해 함수의 동일한 집합을 구현 합니다. 직렬화 및 JSON으로 역직렬화 해야 하 고 별도의 REST 호출을 래핑할 수 있습니다 **HttpHelper** 해야 할 경우 클래스입니다. 가리킵니다 [전체 샘플을](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) 세부 사항에 대 한 합니다.
* **App.xaml.cs**대신 호출을 REST 리포지토리를 초기화 하는 새 함수를 만들거나 **SqliteDatabase** 앱 초기화 될 때입니다. 마찬가지로 가리킵니다 [전체 샘플을](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)입니다.

다음이 단계 중 세 가지를 모두 완료 되 면 앱을 통해 AAD 계정에 인증할 수 해야 합니다. 원격 데이터베이스에 대 한 REST 호출에는 로컬 SQLite 호출을 대체할 하지만 사용자 환경을 동일 해야 합니다. 관심이 있으면 훨씬 더 원대한, 둘 간의 동적으로 전환할 수 있도록 설정 페이지를 추가할 수 있습니다.