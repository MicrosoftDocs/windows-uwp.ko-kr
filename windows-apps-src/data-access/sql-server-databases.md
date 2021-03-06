---
title: UWP 앱에서 SQL Server 데이터베이스 사용
description: System.Data.SqlClient를 사용하여 UWP 앱을 SQL Server 데이터베이스에 직접 연결한 다음, 데이터를 저장하고 검색하는 방법을 알아봅니다.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp, SQL Server, 데이터베이스
ms.localizationpriority: medium
ms.openlocfilehash: b775f1b6f7850b1e7c0b951c0126fcb97ceec8a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170187"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>UWP 앱에서 SQL Server 데이터베이스 사용
앱에서 [System.Data.SqlClient](/dotnet/api/system.data.sqlclient) 네임스페이스를 사용하여 SQL Server 데이터베이스에 직접 연결한 다음, 데이터를 저장하고 검색할 수 있습니다.

이 가이드에서 그 방법 중 하나를 살펴보겠습니다. SQL Server 인스턴스에 [Northwind](/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) 샘플 데이터베이스를 설치한 후 코드 조각을 사용하면, 기본 UI가 Northwind 샘플 데이터베이스의 제품을 보여 줍니다.

![Northwind 제품](images/products-northwind.png)

이 가이드에 표시되는 코드 조각은 더 [완전한 샘플](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo)을 기반으로 합니다.

## <a name="first-set-up-your-solution"></a>먼저 솔루션을 설정합니다.

앱을 SQL Server 데이터베이스에 직접 연결하려면 프로젝트의 최소 버전이 Fall Creators Update가 대상이어야 합니다.  UWP 프로젝트의 속성 페이지에서 이에 대한 정보를 찾을 수 있습니다.

![Windows SDK 최소 버전](images/min-version-fall-creators.png)

매니페스트 디자이너에서 UWP 프로젝트의 **Package.appxmanifest** 파일을 엽니다.

**기능** 탭에서 SQL Server 인증에 Windows 인증을 사용하는 경우 **엔터프라이즈 인증** 확인란을 선택합니다.

![엔터프라이즈 인증 기능](images/enterprise-authentication.png)

> [!IMPORTANT]
> 또한 Windows 인증 사용 여부에 관계없이 **인터넷(클라이언트 및 서버)** , **인터넷(클라이언트)** , **개인 네트워크(클라이언트 및 서버)** 를 선택해야 합니다.

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>SQL Server 데이터베이스에서 데이터 추가 및 검색

이 섹션에서는 다음을 수행합니다.

:one: 연결 문자열을 추가합니다.

:two: 제품 데이터를 저장할 클래스를 만듭니다.

:three: SQL Server 데이터베이스에서 제품을 검색합니다.

:four: 기본 사용자 인터페이스를 추가합니다.

:five: UI를 제품으로 채웁니다.

>[!NOTE]
> 이 섹션에서는 데이터 액세스 코드를 구성하는 방법 중 하나를 설명합니다. SQL Server 데이터베이스에서 데이터를 저장 및 검색하기 위해 [System.Data.SqlClient](/dotnet/api/system.data.sqlclient)를 사용하는 방법에 대한 예제만 제공합니다. 애플리케이션 디자인에 가장 적합한 방식으로 코드를 구성할 수 있습니다.

### <a name="add-a-connection-string"></a>연결 문자열 추가

**App.xaml.cs** 파일에서 ``App`` 클래스에 속성을 추가해 솔루션의 다른 클래스가 연결 문자열에 액세스 할 수 있도록 만듭니다.

연결 문자열은 SQL Server Express 인스턴스의 Northwind 데이터베이스를 가리킵니다.

```csharp
sealed partial class App : Application
{
    // Connection string for using Windows Authentication.
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    // This is an example connection string for using SQL Server Authentication.
    // private string connectionString =
    //     @"Data Source=YourServerName\YourInstanceName;Initial Catalog=DatabaseName; User Id=XXXXX; Password=XXXXX";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>제품 데이터를 저장할 클래스 만들기

[INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) 이벤트를 구현하는 클래스를 만들어 XAML UI 속성을 이 클래스의 속성에 바인딩합니다.

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>SQL Server 데이터베이스에서 제품 검색

Northwind 샘플 데이터베이스에서 제품을 가져오고, 이를 ``Product`` 인스턴스의 [ObservableCollection](/dotnet/api/system.collections.objectmodel.observablecollection-1) 컬렉션으로 반환하는 메서드를 만듭니다.

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>기본 사용자 인터페이스 추가

 다음 XAML을 UWP 프로젝트의 **MainPage.xaml** 파일에 추가합니다.

 이 XAML은 앞서 코드 조각에서 반환한 각 제품을 표시하고, [ListView](/uwp/api/windows.ui.xaml.controls.listview)에 있는 각 행의 특성을 ``Product`` 클래스에서 정의한 속성에 바인딩하는 [ListView](/uwp/api/windows.ui.xaml.controls.listview)를 만듭니다.

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>ListView에 제품 표시

**MainPage.xaml.cs** 파일을 열고 [ListView](/uwp/api/windows.ui.xaml.controls.listview)의 **ItemSource** 속성을 ``Product`` 인스턴스의 [ObservableCollection](/dotnet/api/system.collections.objectmodel.observablecollection-1)으로 설정하는 ``MainPage`` 클래스 생성자에 코드를 추가합니다.

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

프로젝트를 시작하고 Northwind 샘플 데이터베이스의 제품들이 UI에 표시되는지 확인합니다.

![Northwind 제품](images/products-northwind.png)

[System.Data.SqlClient](/dotnet/api/system.data.sqlclient) 네임스페이스를 확인해 SQL Server 데이터베이스에서 할 수 있는 다른 작업을 확인합니다.

## <a name="trouble-connecting-to-your-database"></a>데이터베이스에 연결하는 데 문제가 있나요?

대부분의 경우 SQL Server 구성의 일부 기능을 변경해야 합니다. Windows Forms 또는 WPF 애플리케이션 등의 다른 형식의 데스크톱 애플리케이션의 데이터베이스에 연결할 수 있는 경우 SQL Server에 대해 TCP/IP를 활성화했는지 확인하세요. **컴퓨터 관리** 콘솔에서 이를 수행할 수 있습니다.

![컴퓨터 관리](images/computer-management.png)

그런 다음, SQL Server Browser 서비스가 실행되고 있는지 확인합니다.

![SQL Server Browser 서비스](images/sql-browser-service.png)

## <a name="next-steps"></a>다음 단계

**간단한 데이터베이스를 사용해 사용자 디바이스에 데이터 저장**

[UWP 앱에서 SQLite 데이터베이스 사용](sqlite-databases.md)을 참조하세요.

**여러 플랫폼의 여러 앱 간에 코드 공유**

[데스크톱과 UWP 간에 코드 공유](../porting/desktop-to-uwp-migrate.md)를 참조하세요.

**Azure SQL 백 엔드로 마스터 세부 정보 페이지 추가**

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)을 참조하세요.