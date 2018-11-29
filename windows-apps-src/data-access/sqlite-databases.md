---
title: UWP 앱에서 SQLite 데이터베이스 사용
description: UWP 앱에서 SQLite 데이터베이스 사용
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, SQLite, 데이터베이스
ms.localizationpriority: medium
ms.openlocfilehash: 1588dfbfb1c33b246caba0816c584135f2094f35
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7983349"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>UWP 앱에서 SQLite 데이터베이스 사용
SQLite를 사용해 사용자 장치에서 경량 데이터베이스의 데이터를 저장하고 검색할 수 있습니다. 이 가이드는 그 방법을 보여줍니다.

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>로컬 저장소에 SQLite를 사용했을 때의 이점

:heavy_check_mark: SQLite는 가볍고 독립적입니다. 다른 종속성이 없는 코드 라이브러리입니다. 구성이 필요 없습니다.

: heavy_check_mark: 데이터베이스 서버가 없습니다. 클라이언트 및 서버가 동일한 프로세스에서 실행됩니다.

: heavy_check_mark: SQLite는 공용 도메인에 기반을 두고 있어 자유롭게 사용하고 앱으로 배포할 수 있습니다.

:heavy_check_mark: SQLite는 여러 플랫폼과 아키텍처에서 작동합니다.

SQLite에 대한 자세한 내용은 [여기](https://sqlite.org/about.html)를 참조하세요.

## <a name="choose-an-abstraction-layer"></a>추상화 계층 선택

Microsoft가 빌드한 Entity Framework Core 또는 오픈 소스인 [SQLite 라이브러리](https://github.com/aspnet/Microsoft.Data.Sqlite/)를 사용하는 것이 좋습니다.

### <a name="entity-framework-core"></a>Entity Framework Core

EF(Entity Framework)는 도메인별 개체를 사용하여 관계형 데이터로 작업할 수 있는 개체 관계형 매퍼입니다. 다른 .NET 앱의 데이터에 이 프레임워크를 이미 사용하고 있다면, 코드를 UWP 앱으로 마이그레이션할 수 있습니다. 그러면 연결 스트링이 적절히 변경되어 작동을 합니다.

자세한 내용은 [UWP(유니버설 Windows 플랫폼)에서 EF Core를 새 데이터베이스로 시작하기](https://docs.microsoft.com/ef/core/get-started/uwp/getting-started)를 참조하세요.

### <a name="sqlite-library"></a>SQLite 라이브러리

[Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) 라이브러리는 [System.Data.Common](https://msdn.microsoft.com/library/system.data.common.aspx) 네임스페이스에 인터페이스를 구현합니다. Microsoft는 이러한 구현을 적극적으로 유지하고 있으며, 이는 저수준 기본 SQLite API에 대해 직관적인 래퍼를 제공합니다.

이 가이드의 나머지 부분은 이 라이브러리 활용에 도움을 줍니다.

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>Microsoft.Data.SQlite 라이브러리를 사용할 수 있도록 솔루션을 설정

기본 UWP 프로젝트로 시작을 하겠습니다. 라이브러리를 추가한 후 적절한 Nuget 패키지를 설치하겠습니다.

솔루션에 추가하는 클래스 라이브러리의 유형과 설치하는 패키지는 앱의 대상인 Windows SDK 최소 버전에 따라 달라집니다. UWP 프로젝트의 속성 페이지에서 이에 대한 정보를 찾을 수 있습니다.

![Windows SDK 최소 버전](images/min-version.png)

UWP 프로젝트의 대상인 Windows SDK 최소 버전에 따라 다음 섹션 중 하나를 사용하세요.

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>프로젝트의 최소 버전이 Fall Creators Update가 대상이 아닌 경우

Visual Studio 2015를 사용하고 있다면 **도움말**->**Microsoft Visual Studio에 대한 정보**를 참조하세요. 그런 다음 설치된 프로그램 목록에 NuGet 패키지 관리자 버전이 **3.5** 이상인지 확인합니다. 버전 번호가 이보다 낮을 경우, [여기](https://www.nuget.org/downloads)에서 최신 NuGet 버전을 설치하세요. 이 페이지의 **Visual Studio 2015**이라는 제목 아래에서 모든 NuGet 버전을 찾을 수 있습니다.

그런 다음 솔루션에 클래스 라이브러리를 추가합니다. 데이터 액세스 코드를 포함시키기 위해 클래스 라이브러리를 사용할 필요는 없습니다. 그러나 여기에서는 예로 사용합니다. 라이브러리에 **DataAccessLibrary**라는 이름을 지정할 것입니다. 또 라이브러리 클래스에는 **DataAccess**라는 이름을 줄 것입니다.

![클래스 라이브러리](images/class-library.png)

솔루션을 마우스 오른쪽 단추로 클릭한 후 **솔루션에서 NuGet 패키지 관리**를 클릭합니다.

![NuGet 패키지 관리](images/manage-nuget.png)

Visual Studio 2015를 사용하고 있다면 **설치된** 탭을 선택한 후 **Microsoft.NETCore.UniversalWindowsPlatform** 패키지 버전 번호가 **5.2.2** 이상인지 확인하세요.

![.NETCore 버전](images/package-version.png)

업데이트되어 있지 않다면, 패키지를 최신 버전으로 업데이트하세요.

**탐색** 탭을 선택한 다음 **Microsoft.Data.SQLite** 패키지를 검색합니다. 버전 **1.1.1**(이하)의 패키지를 설치합니다.

![SQLite 패키지](images/sqlite-package.png)

이 가이드의 [SQLite 데이터베이스의 데이터 추가 및 검색](#use-data)으로 이동합니다.

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>프로젝트 최소 버전의 대상이 Fall Creators Update인 경우

UWP 프로젝트의 최소 버전을 Fall Creator Update로 올리면 몇 가지 이점이 있습니다.

먼저 일반 클래스 라이브러리 대신 .NET Standard 2.0 라이브러리를 사용할 수 있습니다. 그러면 데이터 액세스 코드를 WPF, Windows FOrms, Android, iOS, ASP.NET 앱 등 다른 .NET 기반 앱과 공유할 수 있습니다.

둘째, 앱이 SQLite 라이브러리 패키지를 만들지 않습니다. 대신 Windows와 함께 설치된 SQLite 버전을 사용할 수 있습니다. 이는 몇 가지 방법으로 도움을 줍니다.

:heavy_check_mark: SQLite 바이너리를 다운로드받아 응용 프로그램의 일부로 패키징 할 필요가 없기 때문에 응용 프로그램의 크기를 줄입니다.

:heavy_check_mark: SQLite가 SQLite의 버그와 보안 취약점에 대해 중요한 수정 사항을 게시할 때 사용자에게 새 앱 버전을 푸시할 필요가 없습니다. SQLite Windows 버전은 Microsoft가 SQLite.org와 함께 유지관리하고 있습니다.

:heavy_check_mark: SQLite의 SDK 버전이 메모리에 로드되기 때문에 앱 로드 시간이 더 빨라질 수 있습니다.

.NET Standard 2.0 클래스 라이브러리를 솔루션에 추가해 시작합니다. 데이터 액세스 코드를 포함시키기 위해 클래스 라이브러리를 사용할 필요는 없습니다. 그러나 여기에서는 예로 사용합니다. 라이브러리에 **DataAccessLibrary**라는 이름을 지정할 것입니다. 또 라이브러리 클래스에는 **DataAccess**라는 이름을 줄 것입니다.

![클래스 라이브러리](images/dot-net-standard.png)

솔루션을 마우스 오른쪽 단추로 클릭한 후 **솔루션에서 NuGet 패키지 관리**를 클릭합니다.

![NuGet 패키지 관리](images/manage-nuget-2.png)

여기에서 선택을 할 수 있습니다. Windows에 포함된 SQLite 버전을 사용할 수 있습니다. 또는 특정 SQLite 버전을 사용해야 할 이유가 있다면 패키지에 SQLite 라이브러리를 포함시킬 수 있습니다.

Windows에 포함된 SQLite 버전 사용 방법부터 시작하겠습니다.

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>Windows에 설치된 SQLite 버전 사용

**탐색** 탭을 선택한 다음 **Microsoft.Data.SQLite core** 패키지를 검색해서 설치합니다.

![SQLite Core 패키지](images/sqlite-core-package.png)

**SQLitePCLRaw.bundle_winsqlite3** 패키지를 검색한 다음 솔루션의 UWP 프로젝트에만 설치합니다.

![SQLite PCL Raw 패키지](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>앱으로 SQLite 포함

이렇게 할 필요는 없습니다. 그러나 앱으로 특정 SQLite 버전을 포함시킬 이유가 있다면 **탐색** 탭을 선택한 다음 **Microsoft.Data.SQLite** 패키지를 검색하세요. 버전 **2.0**(이하)의 패키지를 설치합니다.

![SQLite 패키지](images/sqlite-package-v2.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>SQLite 데이터베이스에서 데이터를 추가 및 검색

다음과 같습니다.

:1: 데이터 액세스 클래스를 준비합니다.

:2: SQLite 데이터베이스를 초기화 합니다.

:3: SQLite 데이터베이스에 데이터를 삽입합니다.

:4: SQLite 데이터베이스에서 데이터를 검색합니다.

:5: 기본 사용자 인터페이스를 추가합니다.

### <a name="prepare-the-data-access-class"></a>데이터 액세스 클래스 준비

UWP 프로젝트에서 솔루션의 **DataAccessLibrary** 프로젝트에 참조를 추가합니다.

![데이터 액세스 클래스 라이브러리](images/ref-class-library.png)

UWP 프로젝트의 **App.xaml.cs** 및 **MainPage.xaml.cs** 파일에 다음 ``using`` 명령문을 추가합니다.

```csharp
using DataAccessLibrary;
```

**DataAccessLibrary** 솔루션에서 **DataAccess** 클래스를 열어 클래스를 정적 상태로 변경합니다.

>[!NOTE]
>여기 예에서는 정적 클래스에 데이터 액세스 코드를 배치했지만, 이는 디자인 측면의 선택 사항에 불과합니다.

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

명령문을 사용해 이 파일 맨 위에 다음을 추가합니다.

```csharp
using Microsoft.Data.Sqlite;
```

<a id="initialize" />

### <a name="initialize-the-sqlite-database"></a>SQLite 데이터베이스 초기화

SQLite 데이터베이스를 초기화하는 **DataAccess** 클래스에 메서드를 추가합니다.

```csharp
public static void InitializeDatabase()
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

이 코드는 SQLite 데이터베이스를 만들고, 응용 프로그램의 로컬 데이터 저장소에 저장합니다.

여기 예에서는 데이터베이스에 ``sqlliteSample.db``라는 이름을 지정했습니다. 그러나 초기화하는 [SqliteConnection](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0) 객체 모두에 사용한다면 어떤 이름이든 사용할 수 있습니다.

UWP 프로젝트의 **App.xaml.cs** 파일 생성자에서 **DataAccess** 클래스의 ``InitializeDatabase`` 메서드를 호출합니다.

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```

<a id="insert" />

### <a name="insert-data-into-the-sqlite-database"></a>SQLite 데이터베이스에 데이터 삽입

SQLite 데이터베이스에 데이터를 삽입하는 **DataAccess** 클래스에 메서드를 추가합니다. 이 코드는 SQL 삽입 공격을 방지하기 위해 쿼리에서 매개 변수를 사용합니다.

```csharp
public static void AddData(string inputText)
{
    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```

<a id="retrieve" />

### <a name="retrieve-data-from-the-sqlite-database"></a>SQLite 데이터베이스에서 데이터 검색

SQLite 데이터베이스에서 데이터 행을 가져오는 메서드를 추가합니다.

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

    using (SqliteConnection db =
        new SqliteConnection("Filename=sqliteSample.db"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

[Read](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_Read) 메서드는 반환된 데이터 행을 통과합니다. 남겨진 행이 있으면 **true**를, 없으면 **false**를 반환합니다.

[GetString](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_) 메서드는 특정 열의 값을 스트링으로 반환합니다. 원하는 데이터의 0에 기반을 둔 열 서수를 나타내는 정수 값을 수락합니다. [GetDataTime](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) 및 [GetBoolean](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_) 같이 유사한 메서드를 사용할 수 있습니다. 열에 포함된 데이터 유형에 따라 메서드를 선택합니다.

여기 예에서는 하나의 열에서 모든 항목들을 선택했기 때문에 서수 매개 변수가 중요하지 않습니다. 하지만 여러 열이 쿼리의 일부인 경우 데이터를 가져오기 원하는 열을 획득하기 위해 서수 값을 사용해야 합니다.

## <a name="add-a-basic-user-interface"></a>기본 사용자 인터페이스 추가

UWP 프로젝트의 **MainPage.xaml** 파일에 다음 XAML을 추가합니다.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

기본 사용자 인터페이스는 SQLite 데이터베이스에 추가할 스트링을 입력하는 데 사용할 수 있는 ``TextBox``를 제공합니다. 이 UI의 ``Button``을 SQLite 데이터베이스에서 데이터를 검색하고 ``ListView``에 데이터를 표시할 이벤트 처리기에 연결할 것입니다.

**MainPage.xaml.cs** 파일에 다음 처리기를 추가합니다. UI ``Button``의 ``Click`` 이벤트를 연결하는 메서드입니다.

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

됐습니다. [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0)를 탐색하면 SQLite 데이터베이스로 할 수 있는 다른 작업들을 확인할 수 있습니다. UWP 앱에서 데이터를 활용하는 다른 방법에 대한 자세한 내용은 아래 링크를 확인해 보세요.

## <a name="next-steps"></a>다음 단계

**앱을 SQL Server 데이터베이스에 직접 연결**

[UWP 앱에서 SQL Server 데이터베이스 사용](sql-server-databases.md)을 참조하세요.

**여러 앱이 여러 플랫폼에서 코드를 공유**

[데스크톱과 UWP에서 코드 공유](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)를 참조하세요.

**Azure SQL 백 엔드로 마스터 세부 정보 페이지 추가**

[고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database)을 참조하세요.
