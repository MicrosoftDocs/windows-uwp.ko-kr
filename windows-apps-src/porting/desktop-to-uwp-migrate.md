---
Description: 데스크톱 애플리케이션 및 UWP 앱 간의 코드 공유
title: 데스크톱 애플리케이션 및 UWP 앱 간의 코드 공유
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c07a3bbff4b29d2b59ef7d6d8a5912ce3675a4e
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730349"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>데스크톱 응용 프로그램에서 UWP로 이동

.NET Framework (WPF 및 Windows Forms 포함) 또는 c + + Win32 Api를 사용 하 여 빌드된 기존 데스크톱 응용 프로그램이 있는 경우 유니버설 Windows 플랫폼 (UWP) 및 Windows 10으로 이동할 수 있는 몇 가지 옵션이 있습니다.

## <a name="package-your-desktop-application-in-an-msix-package"></a>MSIX 패키지에서 데스크톱 응용 프로그램 패키지

MSIX 패키지에서 데스크톱 응용 프로그램을 패키지 하 여 더 많은 Windows 10 기능에 액세스할 수 있습니다. MSIX는 UWP, WPF, Windows Forms 및 Win32 앱을 비롯한 모든 Windows 앱에 유니버설 패키징 환경을 제공하는 최신 Windows 앱 패키지 형식입니다. MSIX 패키지에 데스크톱 Windows 앱을 패키지하면 강력한 설치 및 업데이트 환경, 유연한 기능 시스템을 포함하는 관리형 보안 모듈, Microsoft Store, 엔터프라이즈 관리 및 여러 사용자 지정 모델에 대한 지원 기능에 액세스할 수 있습니다. 소스 코드가 있는지 또는 기존 설치 관리자 파일 (예: MSI 또는 App-v 설치 관리자)만 있는 경우 응용 프로그램을 패키지할 수 있습니다. 응용 프로그램을 패키지 한 후 패키지 확장 및 기타 UWP 구성 요소와 같은 UWP 기능을 통합할 수 있습니다.

자세한 내용은 패키지 [데스크톱 응용 프로그램 (데스크톱 브리지)](/windows/msix/desktop/desktop-to-uwp-root) 및 [패키지 Id가 필요한 기능](/windows/apps/desktop/modernize/modernize-packaged-apps)을 참조 하세요.

## <a name="use-windows-runtime-apis"></a>Windows 런타임 Api 사용

WPF, Windows Forms 또는 C++ Win32 데스크톱 앱에서 직접 여러 Windows 런타임 API를 호출하여 Windows 10 사용자에게 유용한 최신 환경을 통합할 수 있습니다. 예를 들어, Windows 런타임 API를 호출하여 데스크톱 앱에 알림 메시지를 추가할 수 있습니다.

자세한 내용은 [데스크톱 앱에서 Windows 런타임 API 사용](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)을 참조하세요.

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>.NET Framework 앱을 UWP 앱으로 마이그레이션

.NET Framework에서 응용 프로그램을 실행 하는 경우 .NET Standard 2.0를 활용 하 여 UWP 앱으로 마이그레이션할 수 있습니다. .NET Standard 2.0 클래스 라이브러리에 최대한 많은 코드를 이동한 다음 .NET Standard 2.0 라이브러리를 참조 하는 UWP 앱을 만듭니다. 

### <a name="share-code-in-a-net-standard-20-library"></a>.NET Standard 2.0 라이브러리에서 코드 공유

.NET Framework에서 응용 프로그램을 실행 하는 경우 .NET Standard 2.0 클래스 라이브러리에 가능한 한 많은 코드를 입력 합니다. 코드에서 표준에 정의 된 Api를 사용 하는 한 UWP 앱에서 다시 사용할 수 있습니다. .NET Standard 라이브러리에서 코드를 공유 하는 것 보다 더 쉽습니다. .NET Standard 2.0에 많은 Api가 포함 되어 있기 때문입니다.

이에 대해 자세히 알려주는 비디오는 다음과 같습니다.

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>.NET Standard 라이브러리 추가

먼저 솔루션에 하나 이상의 .NET Standard 클래스 라이브러리를 추가 합니다.  

![Dotnet 표준 프로젝트 추가](images/desktop-to-uwp/dot-net-standard-project-template.png)

솔루션에 추가 하는 라이브러리 수는 코드를 구성 하는 방법에 따라 달라 집니다.

각 클래스 라이브러리가 **.NET Standard 2.0**를 대상으로 하는지 확인 합니다.

![대상 .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

이 설정은 클래스 라이브러리 프로젝트의 속성 페이지에서 찾을 수 있습니다.

데스크톱 응용 프로그램 프로젝트에서 클래스 라이브러리 프로젝트에 대 한 참조를 추가 합니다.

![클래스 라이브러리 참조](images/desktop-to-uwp/class-library-reference.png)

그런 다음 도구를 사용 하 여 표준에 맞는 코드의 양을 결정 합니다. 이렇게 하면 코드를 라이브러리로 이동 하기 전에 다시 사용할 수 있는 파트, 최소한의 수정이 필요한 파트 및 응용 프로그램에 관련 된 남은 부분을 결정할 수 있습니다.

#### <a name="check-library-and-code-compatibility"></a>라이브러리 및 코드 호환성 검사

먼저 Nuget 패키지와 타사에서 가져온 기타 dll 파일로 시작 합니다.

응용 프로그램에서이 중 하나를 사용 하는 경우 .NET Standard 2.0와 호환 되는지 확인 합니다. Visual Studio 확장 또는 명령줄 유틸리티를 사용 하 여이 작업을 수행할 수 있습니다.

이러한 동일한 도구를 사용 하 여 코드를 분석 합니다. 여기 ([dotnet apiport](https://github.com/Microsoft/dotnet-apiport/releases))에서 도구를 다운로드 한 다음이 비디오를 시청 하 여 사용 방법을 알아보세요.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

코드가 표준과 호환 되지 않는 경우 해당 코드를 구현할 수 있는 다른 방법을 고려 합니다. 먼저 [.NET API 브라우저](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)를 엽니다. 해당 브라우저를 사용 하 여 .NET Standard 2.0에서 사용할 수 있는 API를 검토할 수 있습니다. 목록의 범위를 .NET Standard 2.0으로 지정 해야 합니다.

![dot net 옵션](images/desktop-to-uwp/dot-net-option.png)

일부 코드는 플랫폼에 따라 달라 집니다. 데스크톱 응용 프로그램 프로젝트에 남아 있어야 합니다.

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>예: .NET Standard 2.0 라이브러리로 데이터 액세스 코드 마이그레이션

Northwind 샘플 데이터베이스에서 고객을 표시 하는 매우 기본적인 Windows Forms 응용 프로그램이 있다고 가정해 보겠습니다.

![Windows Forms 앱](images/desktop-to-uwp/win-forms-app.png)

프로젝트는 **Northwind**라는 정적 클래스가 있는 .NET Standard 2.0 클래스 라이브러리를 포함 합니다. 이 코드를 **Northwind** 클래스로 이동 하는 경우 ``SQLConnection`` , ``SqlCommand`` 및 ``SqlDataReader`` 클래스를 사용 하 고 .NET Standard 2.0에서 사용할 수 없는 클래스를 사용 하기 때문에 컴파일되지 않습니다.

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
그러나 [.NET API 브라우저](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0) 를 사용 하 여 다른 대안을 찾을 수 있습니다. ``DbConnection``, ``DbCommand`` 및 클래스는 ``DbDataReader`` 모두 .NET Standard 2.0에서 사용할 수 있으므로 대신 사용할 수 있습니다.  

이 수정 된 버전은 이러한 클래스를 사용 하 여 고객 목록을 가져오지만 클래스를 만들려면 ``DbConnection`` 클라이언트 응용 프로그램에서 만든 팩터리 개체를 전달 해야 합니다.

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

Windows Form의 코드 지향 페이지에서 팩터리 인스턴스를 만들고 메서드에 전달 하기만 하면 됩니다.

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

### <a name="create-a-uwp-app"></a>UWP 앱 만들기

이제 솔루션에 UWP 앱을 추가할 준비가 되었습니다.

![데스크톱-UWP 브리지 이미지](images/desktop-to-uwp/adaptive-ui.png)

UI 페이지를 XAML로 디자인 하 고 모든 장치 또는 플랫폼별 코드를 작성 해야 합니다. 완료 되 면 모든 범위의 Windows 10 장치에 연결할 수 있으며, 앱 페이지는 다양 한 화면 크기 및 해상도에 맞게 조정 되는 최신 느낌을 갖게 됩니다.

앱은 키보드 및 마우스 외에도 입력 메커니즘에 응답 하 고, 기능 및 설정은 장치에서 직관적으로 제공 됩니다. 즉, 사용자는 작업을 한 번 수행 하는 방법을 배우고 장치에 관계 없이 매우 친숙 한 방식으로 작동 합니다.

이러한 경쟁력는 UWP와 함께 제공 되는 몇 가지 방법입니다. 자세한 내용은 [Windows를 사용 하 여 뛰어난 환경 빌드](https://developer.microsoft.com/windows/why-build-for-uwp)를 참조 하세요.

#### <a name="add-a-uwp-project"></a>UWP 프로젝트 추가

먼저 솔루션에 UWP 프로젝트를 추가 합니다.

![UWP 프로젝트](images/desktop-to-uwp/new-uwp-app.png)

그런 다음 UWP 프로젝트에서 .NET Standard 2.0 라이브러리 프로젝트에 대 한 참조를 추가 합니다.

![클래스 라이브러리 참조](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>페이지 빌드

XAML 페이지를 추가 하 고 .NET Standard 2.0 라이브러리에서 코드를 호출 합니다.

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

UWP를 시작 하려면 [uwp 앱 이란?](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)을 참조 하세요.

### <a name="reach-ios-and-android-devices"></a>IOS 및 Android 장치에 도달

Xamarin 프로젝트를 추가 하 여 Android 및 iOS 장치에 연결할 수 있습니다.  

![Xamarin 앱](images/desktop-to-uwp/xamarin-apps.png)

이러한 프로젝트를 통해 c #을 사용 하 여 플랫폼별 및 장치별 Api에 대 한 모든 권한으로 Android 및 iOS 앱을 빌드할 수 있습니다. 이러한 앱은 플랫폼별 하드웨어 가속을 활용 하며 기본 성능에 맞게 컴파일됩니다.

이러한 기능은 iBeacons 장치 및 Android 조각과 같은 플랫폼별 기능을 포함 하 여 기본 플랫폼 및 장치에서 노출 하는 모든 기능에 액세스할 수 있으며, 표준 네이티브 사용자 인터페이스 컨트롤을 사용 하 여 사용자가 원하는 방식으로 표시 하 고 느낌을 주는 Ui를 빌드합니다.

UWPs와 마찬가지로 .NET Standard 2.0 클래스 라이브러리에서 비즈니스 논리를 다시 사용할 수 있으므로 Android 또는 iOS 앱을 추가 하는 비용이 더 낮습니다. UI 페이지를 XAML로 디자인 하 고 모든 장치 또는 플랫폼별 코드를 작성 해야 합니다.

#### <a name="add-a-xamarin-project"></a>Xamarin 프로젝트 추가

먼저, **Android**, **IOS**또는 **플랫폼 간** 프로젝트를 솔루션에 추가 합니다.

이러한 템플릿은 **새 프로젝트 추가** 대화 상자의 **Visual c #** 그룹에서 찾을 수 있습니다.

![Xamarin 앱](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>플랫폼 간 프로젝트는 플랫폼에 한정 되지 않는 기능을 갖춘 앱에 적합 합니다. IOS, Android 및 Windows에서 실행 되는 하나의 네이티브 XAML 기반 UI를 빌드하는 데 사용할 수 있습니다. 자세한 내용은 [여기](https://docs.microsoft.com/xamarin/xamarin-forms/)를 참조 하세요.

그런 다음 Android, iOS 또는 플랫폼 간 프로젝트에서 클래스 라이브러리 프로젝트에 대 한 참조를 추가 합니다.

![클래스 라이브러리 참조](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>페이지 빌드

이 예제에서는 Android 앱에 있는 고객의 목록을 보여 줍니다.

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

Android, iOS 및 플랫폼 간 프로젝트를 시작 하려면 [Xamarin 개발자 포털](https://docs.microsoft.com/xamarin)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문하세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
