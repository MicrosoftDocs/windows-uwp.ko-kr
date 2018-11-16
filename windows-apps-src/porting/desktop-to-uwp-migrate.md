---
author: normesta
Description: Share code between a desktop application and a UWP app
Search.Product: eADQiWindows 10XVcnh
title: 데스크톱 응용 프로그램 및 UWP 앱 간의 코드 공유
ms.author: normesta
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a4eda15a18a441da2e06db17aaf7bea3d1842d1
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6857195"
---
# <a name="share-code-between-a-desktop-application-and-a-uwp-app"></a>데스크톱 응용 프로그램 및 UWP 앱 간의 코드 공유

코드를 .NET Standard 라이브러리로 옮기고 모든 Windows 10 장치에 연결할 수 있는 UWP(유니버설 Windows 플랫폼) 앱을 만들 수 있습니다. 데스크톱 응용 프로그램을 UWP 앱으로 변환할 수 있는 도구는 없지만, 수많은 기존 코드를 재사용할 수 있으며 이를 통해 빌드 비용을 낮출 수 있습니다. 이 가이드에서는 이 작업을 수행하는 방법을 보여 줍니다.

## <a name="share-code-in-a-net-standard-20-library"></a>.NET Standard 2.0 라이브러리에 있는 코드 공유

.NET Standard 2.0 클래스 라이브러리에 가능한 한 많은 코드를 배치합니다.  코드가 표준에 정의된 API를 사용하는 한 UWP 앱에서 해당 코드를 다시 사용할 수 있습니다. .NET Standard 2.0에 훨씬 많은 API가 포함되어 있기 때문에 이는 .NET Standard 라이브러리에 코드를 공유하는 것보다 쉽습니다.

이러한 방법에 대해 자세히 알려 주는 좋은 동영상이 있습니다.
&nbsp;
> [!VIDEO https://www.youtube.com/embed/YI4MurjfMn8]

### <a name="add-net-standard-libraries"></a>.NET Standard 라이브러리에 추가

먼저 하나 이상의 .NET Standard 클래스 라이브러리를 솔루션에 추가합니다.  

![dotnet 표준 프로젝트 추가](images/desktop-to-uwp/dot-net-standard-project-template.png)

솔루션에 추가하는 라이브러리의 수는 코드 구성 방법에 따라 다릅니다.

각 클래스 라이브러리가 **.NET Standard 2.0**을 대상으로 하는지 확인합니다.

![대상 .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

이 설정은 클래스 라이브러리 프로젝트의 속성 페이지에서 확인할 수 있습니다.

데스크톱 응용 프로그램 프로젝트에서 클래스 라이브러리 프로젝트에 참조를 추가합니다.

![클래스 라이브러리 참조](images/desktop-to-uwp/class-library-reference.png)

그런 다음 도구를 사용하여 표준에 맞는 코드의 양을 확인합니다. 이렇게 하면 코드를 라이브러리로 옮기기 전에 재사용할 수 있는 부분, 최소한의 수정이 필요한 부분 및 응용 프로그램에 따라 달라질 부분을 결정할 수 있습니다.

### <a name="check-library-and-code-compatibility"></a>라이브러리 및 코드 호환성 검사

타사에서 얻은 NuGet 패키지 및 기타 dll 파일부터 시작합니다.

응용 프로그램이 이 중 하나를 사용하는 경우 .NET Standard 2.0과 호환되는지 확인합니다. Visual Studio 확장 또는 명령줄 유틸리티를 사용하여 이를 수행할 수 있습니다.

동일한 도구를 사용하여 코드를 분석합니다. 해당 도구를 다운로드([dotnet apiport](https://github.com/Microsoft/dotnet-apiport/releases))한 다음 이 동영상을 보고 사용 방법을 알아봅니다.
&nbsp;
> [!VIDEO https://www.youtube.com/embed/rzs_FGPyAlY]

코드가 표준과 호환되지 않는 경우 해당 코드를 구현할 수 있는 다른 방법을 고려합니다. [.NET API 브라우저](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)를 여는 것으로 시작합니다. 해당 브라우저를 사용하여 .NET Standard 2.0에서 사용할 수 있는 API를 검토할 수 있습니다. .NET Standard 2.0으로 목록의 범위를 지정합니다.

![dot net 옵션](images/desktop-to-uwp/dot-net-option.png)

일부 코드는 플랫폼에 따라 다르며 데스크톱 응용 프로그램 프로젝트에 남아 있어야 합니다.

### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>예: 데이터 액세스 코드를 .NET Standard 2.0 라이브러리로 마이그레이션

Northwind 샘플 데이터베이스에서 고객을 표시 하는 매우 기본적인 Windows Forms 응용 프로그램 있다고 가정해 보겠습니다.

![Windows Forms 앱](images/desktop-to-uwp/win-forms-app.png)

프로젝트에는 **Northwind**라는 정적 클래스가 있는 .NET Standard 2.0 클래스 라이브러리가 포함되어 있습니다. 이 코드를 **Northwind** 클래스를 옮기는 경우 해당 코드가 ``SQLConnection``, ``SqlCommand`` 및 ``SqlDataReader`` 클래스 및 .NET Standard 2.0에서 사용할 수 없는 클래스를 사용하기 때문에 컴파일되지 않습니다.

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
하지만 대안을 찾기 위해 [.NET API 브라우저](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)를 사용할 수 있습니다. ``DbConnection``, ``DbCommand`` 및 ``DbDataReader`` 클래스는 모두 .NET Standard 2.0에서 사용할 수 있기 때문에 이를 대신 사용할 수 있습니다.  

이 수정된 버전은 이러한 클래스를 사용하여 고객 목록을 가져오지만, ``DbConnection`` 클래스를 생성하기 위해서는 클라이언트 응용 프로그램에서 생성한 팩터리 개체를 전달해야 합니다.

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

Windows Form의 코드 숨김 페이지에서 팩터리 인스턴스를 만들어 메서드에 전달할 수 있습니다.

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

## <a name="reach-all-windows-devices"></a>모든 Windows 장치에 연결

이제 솔루션에 UWP 앱을 추가할 준비가 되었습니다.

![데스크톱-UWP 브리지 이미지](images/desktop-to-uwp/adaptive-ui.png)

XAML에서 UI 페이지를 디자인하고 장치 또는 플랫폼 관련 코드를 작성해야 하지만, 완료하고 나면 다양한 Windows 10 장치에 연결할 수 있으며 앱 페이지는 다양한 화면 크기 및 해상도에 맞게 조정될 수 있는 현대적인 느낌을 갖습니다.

앱은 키보드와 마우스 이외의 입력 메커니즘에 반응할 것이며, 기능과 설정은 여러 장치에서 직관적일 수 있습니다. 즉, 사용자는 사용 방법을 한 번에 배울 수 있고 앱은 장치와 상관없이 매우 익숙한 방식으로 작동하게 됩니다.

이는 UWP와 함께 제공되는 몇 가지 장점일 뿐입니다. 자세히 알아보려면 [Windows를 사용한 멋진 환경 만들기](https://developer.microsoft.com/windows/why-build-for-uwp)를 참조하세요.

### <a name="add-a-uwp-project"></a>UWP 프로젝트 추가

먼저 솔루션에 UWP 프로젝트를 추가합니다.

![UWP 프로젝트](images/desktop-to-uwp/new-uwp-app.png)

그런 다음 UWP 프로젝트에서 .NET Standard 2.0 라이브러리 프로젝트에 대한 참조를 추가합니다.

![클래스 라이브러리 참조](images/desktop-to-uwp/class-library-reference2.png)

### <a name="build-your-pages"></a>페이지 빌드

.NET Standard 2.0 라이브러리에서 XAML 페이지를 추가하고 코드를 호출합니다.

![UWP 앱](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```


UWP를 사용하려면 [유니버설 Windows 플랫폼 개요](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)를 참조하세요.

## <a name="reach-ios-and-android-devices"></a>iOS 및 Android 장치에 연결

Xamarin 프로젝트를 추가하여 Android 및 iOS 장치에 연결할 수 있습니다.  

![Xamarin 앱](images/desktop-to-uwp/xamarin-apps.png)

이 프로젝트를 통해 C#을 사용하여 플랫폼 및 장치별 API에 대한 모든 액세스 권한을 가진 Android 및 iOS 앱을 빌드할 수 있습니다. 이러한 앱은 플랫폼별 하드웨어 가속을 활용하며 기본 성능을 위해 컴파일됩니다.

해당 앱은 iBeacon 및 Android Fragment와 같은 플랫폼별 기능을 포함하여 기본 플랫폼 및 장치에서 제공하는 모든 기능에 액세스할 수 있으며, 개발자는 표준 네이티브 사용자 인터페이스 컨트롤을 사용하여 사용자가 기대하는 모양과 느낌의 UI를 빌드할 수 있습니다.

UWP와 마찬가지로 .NET Standard 2.0 클래스 라이브러리에서 비즈니스 논리를 재사용할 수 있으므로 Android 또는 iOS 앱을 추가하는 데 드는 비용은 더 낮습니다. XAML에서 UI 페이지를 디자인하고 장치 또는 플랫폼별 코드를 작성해야 합니다.

### <a name="add-a-xamarin-project"></a>Xamarin 프로젝트 추가

먼저 **Android**, **iOS** 또는 **크로스 플랫폼** 프로젝트를 솔루션에 추가합니다.

**Visual C#** 그룹의 **새 프로젝트 추가** 대화 상자에서 이러한 템플릿을 찾을 수 있습니다.

![Xamarin 앱](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>크로스 플랫폼 프로젝트는 플랫폼별 기능이 거의 없는 앱에 적합합니다. 이를 사용하여 iOS, Android 및 Windows에서 실행되는 네이티브 XAML 기반 UI를 빌드할 수 있습니다. [여기](https://www.xamarin.com/forms)에서 자세한 내용을 알아보세요.

그런 다음 Android, iOS 또는 크로스 플랫폼 프로젝트에서 클래스 라이브러리 프로젝트에 대한 참조를 추가합니다.

![클래스 라이브러리 참조](images/desktop-to-uwp/class-library-reference3.png)

### <a name="build-your-pages"></a>페이지 빌드

이 예에서는 Android 앱의 고객 목록을 보여 줍니다.

![Android 앱](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Android, iOS 및 크로스 플랫폼 프로젝트를 시작하려면 [Xamarin 개발자 포털](https://developer.xamarin.com/)을 참조하세요.

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
