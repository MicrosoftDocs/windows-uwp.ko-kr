---
description: 선언적 XAML 태그의 형태로 UI를 정의 유니버설 Windows 플랫폼 하는 방법은 Silverlight (Windows Phone Silverlight) 앱에서 매우 원활 하 게 변환 됩니다.
title: Windows Phone Silverlight XAML 및 UI를 UWP로 포팅
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a6f78d30b1366078f2094aa17ab15c65a050c43
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164347"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>Windows Phone Silverlight XAML 및 UI를 UWP로 포팅



이전 항목은 [문제 해결](wpsl-to-uwp-troubleshooting.md)이었습니다.

선언적 XAML 태그의 형태로 UI를 정의 유니버설 Windows 플랫폼 하는 방법은 Silverlight (Windows Phone Silverlight) 앱에서 매우 원활 하 게 변환 됩니다. 시스템 리소스 키 참조를 업데이트 하 고, 일부 요소 형식 이름을 변경 하 고, "clr-namespace"를 "using"로 변경한 후에는 태그의 많은 섹션이 호환 됩니다. 프레젠테이션 계층에서 대부분의 명령적 코드 (뷰 모델 및 UI 요소를 조작 하는 코드)는 포트에도 간단 합니다.

## <a name="a-first-look-at-the-xaml-markup"></a>XAML 태그를 먼저 확인 합니다.

이전 항목에서는 XAML 및 코드 숨겨진 파일을 새 Windows 10 Visual Studio 프로젝트에 복사 하는 방법을 살펴보았습니다. Visual Studio XAML 디자이너에서 강조 표시 되는 첫 번째 문제 중 하나는 `PhoneApplicationPage` xaml 파일의 루트에 있는 요소가 UWP (유니버설 Windows 플랫폼) 프로젝트에 대해 유효 하지 않음을 나타내는 것입니다. 이전 항목에서는 Windows 10 프로젝트를 만들 때 Visual Studio에서 생성 한 XAML 파일의 복사본을 저장 했습니다. 해당 버전의 MainPage을 여는 경우 루트에는 형식 [**페이지**](/uwp/api/Windows.UI.Xaml.Controls.Page)( [**Windows. .xaml**](/uwp/api/Windows.UI.Xaml.Controls) 네임 스페이스에 있는)가 표시 됩니다. 따라서 모든 `<phone:PhoneApplicationPage>` 요소를로 변경 하 `<Page>` 고 (속성 요소 구문 기억 안 함) 선언을 삭제할 수 있습니다 `xmlns:phone` .

Windows Phone Silverlight 형식에 해당 하는 UWP 형식을 검색 하는 일반적인 방법은 [네임 스페이스 및 클래스 매핑을](wpsl-to-uwp-namespace-and-class-mappings.md)참조할 수 있습니다.

## <a name="xaml-namespace-prefix-declarations"></a>XAML 네임 스페이스 접두사 선언


보기 모델 인스턴스 또는 값 변환기 등 보기에서 사용자 지정 형식의 인스턴스를 사용 하는 경우 xaml 태그에 XAML 네임 스페이스 접두사 선언이 있습니다. 이러한 구문은 Windows Phone Silverlight와 UWP 사이에서 다릅니다. 다음은 몇 가지 예입니다.

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

"Clr-namespace"를 "using"으로 변경 하 고 어셈블리 토큰과 세미콜론을 삭제 합니다. 어셈블리가 유추 됩니다. 결과는 다음과 같습니다.

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

시스템이 다음과 같은 형식으로 정의 된 리소스가 있을 수 있습니다.

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

UWP에서 "System" 접두사 선언을 생략 하 고 대신 (이미 선언 된) "x" 접두사를 사용 합니다.

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>명령적 코드


뷰 모델은 UI 유형을 참조 하는 명령형 코드가 있는 한 곳입니다. 또 다른 장소는 UI 요소를 직접 조작 하는 코드 숨김이 있는 파일입니다. 예를 들어 다음과 같은 코드 줄이 아직 컴파일되지 않는 경우가 있습니다.


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** 는 Silverlight Windows Phone의 BitmapImage 네임 스페이스에 있고 동일한 파일에 using 지시문을 사용 하면 위의 코드 조각에서와 같이 네임 스페이스 한정자 없이 **BitmapImage** 을 사용할 수 **있습니다.** 이와 같은 경우 Visual Studio에서 형식 이름 (**BitmapImage**)을 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴의 [ **확인** ] 명령을 사용 하 여 새 네임 스페이스 지시어를 파일에 추가할 수 있습니다. 이 경우 형식이 UWP에 있는 위치에 해당 하는 [**Windows.**](/uwp/api/Windows.UI.Xaml.Media.Imaging) x m l. x m l. x m l 네임 스페이스를 추가 합니다. 지시문을 사용 하 여 **Windows** 를 제거할 수 있습니다 .이는 위의 코드 조각에서와 같은 코드를 이식 하는 데 사용 됩니다. 완료 되 면 모든 Windows Phone Silverlight 네임 스페이스를 제거 합니다.

이전 네임 스페이스의 형식을 새 네임 스페이스의 동일한 형식에 매핑하는 경우와 같은 간단한 경우에는 Visual Studio의 **찾기 및 바꾸기** 명령을 사용 하 여 소스 코드를 대량으로 변경할 수 있습니다. **Resolve** 명령은 형식의 새 네임 스페이스를 검색 하는 좋은 방법입니다. 또 다른 예로 모든 "system.xml"을 "Windows. .Xaml"로 바꿀 수 있습니다. 이는 기본적으로 모든 using 지시문 및 해당 네임 스페이스를 참조 하는 정규화 된 형식 이름을 모두 이식 합니다.

모든 이전 using 지시문을 제거 하 고 새 지시문을 추가한 후에는 Visual Studio의 **Using 구성** 명령을 사용 하 여 지시문을 정렬 하 고 사용 하지 않는 지시문을 제거할 수 있습니다.

경우에 따라 명령적 코드를 수정 하는 것은 매개 변수의 형식을 변경 하는 것 만큼 사소한 것입니다. 다른 경우에는 Windows 런타임 8gb 앱 용 .NET Api 대신 Windows 런타임 Api를 사용 해야 합니다. 지원 되는 Api를 식별 하려면이 포팅 가이드의 나머지 부분을 .Net과 함께 [Windows 런타임 8.x 앱 개요](/previous-versions/windows/apps/br230302(v=vs.140)) 및 [Windows 런타임 참조](/uwp/api/)에 대해 사용 합니다.

프로젝트를 빌드하는 단계에만 가져오려는 경우에는 필수적이 지 않은 코드를 주석 처리 하거나 스텁 할 수 있습니다. 그런 다음, 한 번에 하나의 문제를 반복 하 고이 섹션의 다음 항목 (및 이전 항목의 [문제 해결](wpsl-to-uwp-troubleshooting.md))을 참조 합니다.

## <a name="adaptiveresponsive-ui"></a>적응/반응 형 UI

Windows 10 앱은 각자의 화면 크기와 해상도를 포함 하 여 다양 한 범위의 장치에서 실행 될 수 있기 때문에 앱을 이식 하는 최소 단계를 벗어나 해당 장치에서 가장 잘 보이도록 UI를 조정 하는 것이 좋습니다. 적응 시각적 상태 관리자 기능을 사용 하 여 창 크기를 동적으로 검색 하 고 응답의 레이아웃을 변경할 수 있습니다 .이 작업을 수행 하는 방법의 예는 Bookstore2 사례 연구 항목의 [적응 UI](wpsl-to-uwp-case-study-bookstore2.md) 섹션에 나와 있습니다.

## <a name="alarms-and-reminders"></a>경보 및 미리 알림

**경보** 또는 **미리 알림** 클래스를 사용 하는 코드는 [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 클래스를 사용 하 여 백그라운드 작업을 만들고 등록 하 고 관련 시간에 알림 메시지를 표시 하도록 이식 해야 합니다. [백그라운드 처리](wpsl-to-uwp-business-and-data.md) 및 [알림을](#toasts)을 참조 하세요.

## <a name="animation"></a>애니메이션

키 프레임 애니메이션 및 from/to 애니메이션 대신 uwp 애니메이션 라이브러리를 UWP 앱에서 사용할 수 있습니다. 이러한 애니메이션은 원활 하 게 실행 되도록 설계 및 조정 되었으며, 기본 제공 앱에서와 같이 앱이 Windows와 통합 된 것으로 보이도록 설계 되었습니다. [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 적용](/previous-versions/windows/apps/hh452703(v=win.10))을 참조 하세요.

UWP 앱에서 키 프레임 애니메이션 또는 from/to 애니메이션을 사용 하는 경우 새 플랫폼에서 도입 된 독립 애니메이션과 종속 애니메이션 간의 차이점을 이해 하는 것이 좋습니다. [애니메이션 및 미디어 최적화를](../debug-test-perf/optimize-animations-and-media.md)참조 하세요. UI 스레드에서 실행 되는 애니메이션 (예: 레이아웃 속성에 애니메이션을 적용 하는 애니메이션)을 종속 애니메이션 이라고 하며, 새 플랫폼에서 실행 되는 경우 두 가지 작업 중 하나를 수행 하지 않으면 아무런 효과가 없습니다. 이를 다시 대상으로 하 여 [**Rendertransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform)과 같은 다른 속성에 애니메이션 효과를 주기 때문에 독립적으로 만들 수 있습니다. 또는 `EnableDependentAnimation="True"` 애니메이션 요소에 대해를 설정 하 여 원활 하 게 실행 되도록 보장할 수 없는 애니메이션의 실행 의도를 확인할 수 있습니다. Blend for Visual Studio를 사용 하 여 새 애니메이션을 작성 하는 경우 필요한 경우 해당 속성이 설정 됩니다.

## <a name="back-button-handling"></a>뒤로 단추 처리

Windows 10 앱에서는 한 가지 방법을 사용 하 여 뒤로 단추를 처리할 수 있습니다. 그러면 모든 장치에서 작동 합니다. 모바일 장치에서이 단추는 장치에서 용량 단추 또는 셸에서 단추로 제공 됩니다. 앱 내에서 백 탐색이 가능 하면 데스크톱 장치에서 앱의 chrome에 단추를 추가 하 고,이는 기간 업무 앱의 제목 표시줄 또는 태블릿 모드의 작업 표시줄에 표시 됩니다. 뒤로 단추 이벤트는 모든 장치 패밀리의 유니버설 개념으로, 하드웨어 또는 소프트웨어에서 구현 된 단추는 동일한 [**백 요청**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 된 이벤트를 발생 시킵니다.

아래 예제는 모든 장치 제품군에 대해 작동 하며, 동일한 처리가 모든 페이지에 적용 되 고 탐색을 있으므로 확인 하는 경우 (예: 저장 되지 않은 변경 내용에 대 한 경고)에 적합 합니다.

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

프로그래밍 방식으로 앱을 종료 하는 모든 장치 제품군에 대 한 단일 접근 방법도 있습니다.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>바인딩 및 {x:Bind}의 컴파일된 바인딩

바인딩 항목에는 다음이 포함 됩니다.

-   UI 요소를 "data", 즉 뷰 모델의 속성 및 명령에 바인딩
-   UI 요소를 다른 UI 요소에 바인딩
-   관찰 가능한 뷰 모델 작성 (즉, 속성 값이 변경 될 때 알림을 발생 시키고 명령의 가용성이 변경 되는 경우)

이러한 모든 측면은 대체로 계속 지원 되지만 네임 스페이스 차이가 있습니다. 예를 들어 **system.componentmodel에 매핑되는** **INotifyPropertyChanged** , INotifyPropertyChanged 및 INotifyPropertyChanged [**Windows.UI.Xaml.Data.Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding)에 매핑되는 [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)및 **System.Collections.Specialized.INotifyPropertyChanged** 에 매핑됩니다.. x m l. x m l. x m l. x m l. x m l. [**INotifyCollectionChanged**](/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged).

Windows Phone Silverlight 앱 바와 앱 바 단추는 UWP 앱에서 사용할 수 있는 것 처럼 바인딩할 수 없습니다. 앱 표시줄과 단추를 구성 하 고 속성 및 지역화 된 문자열에 바인딩한 다음 이벤트를 처리 하는 명령적 코드를 사용할 수 있습니다. 그렇다면 이제 속성 및 명령에 바인딩된 선언적 태그로 대체 하 고 정적 리소스 참조를 사용 하 여 명령적 코드를 이식 하는 옵션을 사용할 수 있습니다. 따라서 앱을 보다 안전 하 고 쉽게 관리할 수 있습니다. Visual Studio 또는 Blend for Visual Studio를 사용 하 여 다른 XAML 요소와 마찬가지로 UWP 앱 바 단추를 바인딩하고 스타일을 지정할 수 있습니다. UWP 앱에서 사용 하는 형식 이름은 [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 및 [**appbarbutton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)입니다.

UWP 앱의 바인딩 관련 기능에는 현재 다음과 같은 제한 사항이 있습니다.

-   데이터 입력 유효성 검사와 [**IDataErrorInfo**](/dotnet/api/system.componentmodel.idataerrorinfo) 및 [**INotifyDataErrorInfo**](/dotnet/api/system.componentmodel.inotifydataerrorinfo) 인터페이스를 기본적으로 지원 하지 않습니다.
-   [**바인딩**](/uwp/api/Windows.UI.Xaml.Data.Binding) 클래스는 Windows Phone Silverlight에서 사용할 수 있는 확장 서식 속성을 포함 하지 않습니다. 그러나 [**ivalueconverter.convert**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 을 구현 하 여 사용자 지정 형식을 제공할 수도 있습니다.
-   [**Ivalueconverter.convert**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 메서드는 [**CultureInfo**](/dotnet/api/system.globalization.cultureinfo) 개체 대신 언어 문자열을 매개 변수로 사용 합니다.
-   [**Collectionviewsource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 클래스는 정렬 및 필터링에 대 한 기본 제공 지원을 제공 하지 않으며 그룹화는 다르게 작동 합니다. 자세한 내용은 [심층 데이터 바인딩](../data-binding/data-binding-in-depth.md) 및 [데이터 바인딩 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)을 참조 하세요.

동일한 바인딩 기능이 여전히 대부분 지원 되기는 하지만, Windows 10은 {x:Bind} 태그 확장을 사용 하는 컴파일된 바인딩 이라는 새롭고 더 뛰어난 바인딩 메커니즘의 옵션을 제공 합니다. [데이터 바인딩: XAML 데이터 바인딩을 새롭게 향상 된 기능을 통해 앱의 성능](https://channel9.msdn.com/Events/Build/2015/3-635)향상 및 [x:bind 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)을 참조 하세요.

## <a name="binding-an-image-to-a-view-model"></a>이미지를 뷰 모델에 바인딩

[**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource)형식의 뷰 모델 속성에는 [**Image. Source**](/uwp/api/windows.ui.xaml.controls.image.source) 속성을 바인딩할 수 있습니다. 다음은 Windows Phone Silverlight 앱에서 이러한 속성의 일반적인 구현입니다.

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

UWP 앱에서는 ms appx [URI 체계](/previous-versions/windows/apps/jj655406(v=win.10))를 사용 합니다. 코드의 나머지 부분을 동일 하 게 유지할 수 있도록, **system.uri** 생성자의 다른 오버 로드를 사용 하 여 기본 uri에 MS appx uri 체계를 추가 하 고 나머지 경로를 해당 경로에 추가할 수 있습니다. 다음과 같습니다.

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

이렇게 하면 뷰 모델의 나머지 부분, 이미지 경로 속성의 경로 값 및 XAML 태그의 바인딩이 모두 동일 하 게 유지 될 수 있습니다.

## <a name="controls-and-control-stylestemplates"></a>컨트롤, 컨트롤 스타일/템플릿

Windows Phone Silverlight 앱은 **Microsoft 휴대폰. controls** **네임 스페이스 및 system.xml 네임 스페이스** 에 정의 된 컨트롤을 사용 합니다. XAML UWP 앱은 [**Windows. .xaml**](/uwp/api/Windows.UI.Xaml.Controls) 네임 스페이스에 정의 된 컨트롤을 사용 합니다. UWP의 XAML 컨트롤 아키텍처와 디자인은 Silverlight 컨트롤 Windows Phone 거의 동일 합니다. 그러나 사용 가능한 컨트롤 집합을 개선 하 고 Windows 앱과 통합 하기 위해 몇 가지 사항이 변경 되었습니다. 특정 예제는 다음과 같습니다.

| 컨트롤 이름 | 변경 |
|--------------|--------|
| ApplicationBar | [페이지. TopAppBar](/uwp/api/windows.ui.xaml.controls.page.topappbar) 속성입니다. |
| ApplicationBarIconButton | UWP 동급는 [Glyph](/uwp/api/windows.ui.xaml.controls.fonticon.glyph) 속성입니다. PrimaryCommands는 CommandBar의 content 속성입니다. XAML 파서는 요소의 내부 xml을 해당 콘텐츠 속성의 값으로 해석 합니다. |
| ApplicationBarMenuItem | UWP와 동등한 것은 메뉴 항목 텍스트에 설정 된 [레이블입니다.](/uwp/api/windows.ui.xaml.controls.appbarbutton.label) |
| ContextMenu (Windows Phone Toolkit) | 단일 선택 항목의 경우 [플라이](/uwp/api/Windows.UI.Xaml.Controls.Flyout)아웃을 사용 합니다. |
| ControlTiltEffect.TiltEffect 클래스 | UWP 애니메이션 라이브러리의 애니메이션은 공용 컨트롤의 기본 스타일로 빌드됩니다. [애니메이션 포인터 작업](/previous-versions/windows/apps/jj649432(v=win.10))을 참조 하세요. |
| 그룹화 된 데이터를 포함 하는 선택 Listselector | Windows Phone Silverlight를 사용 하는 두 가지 방법으로 작동 합니다. 첫째, 키로 그룹화 된 데이터를 표시할 수 있습니다 (예: 초기 문자별로 그룹화 된 이름 목록). 둘째, 두 의미 체계 보기 (예: 이름)를 "확대/축소" 하 고 그룹 키 자체 목록 (예: 초기 문자)만 사용할 수 있습니다. UWP를 사용 하면 [목록 및 그리드 뷰 컨트롤에 대 한 지침](../design/controls-and-patterns/lists.md)을 사용 하 여 그룹화 된 데이터를 표시할 수 있습니다. |
| 플랫 데이터를 포함 하는 선택 Listselector | 성능상의 이유로 매우 긴 목록의 경우에는 그룹화 되지 않은 플랫 데이터의 경우에도 Windows Phone Silverlight 목록 상자 대신 김 Listselector를 선택 하는 것이 좋습니다. UWP 앱에서 데이터를 그룹화 하는 것이 적합할 여부에 관계 없이 긴 항목 목록에 대해 [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) 를 선호 합니다. |
| 파노라마 | Windows Phone Silverlight 파노라마 컨트롤은 Windows 런타임 8gb 앱 및 허브 컨트롤에 대 한 지침에 [대 한 지침](../design/basics/navigation-basics.md) 에 매핑됩니다. <br/> 파노라마 컨트롤은 마지막 섹션에서 첫 번째 섹션으로 래핑하고 해당 배경 이미지는 섹션을 기준으로 시차에서 이동 합니다. [허브](/uwp/api/Windows.UI.Xaml.Controls.Hub) 섹션은 래핑되지 않으며 시차 사용 되지 않습니다. |
| 피벗 | Windows Phone Silverlight 피벗 컨트롤과 동일한 UWP는 [Windows](/uwp/api/Windows.UI.Xaml.Controls.Pivot). x m l. x m l. x m l. x m l. x m l입니다. 모든 장치 제품군에서 사용할 수 있습니다. |

**참고**    PointerOver 상태는 Windows 10 앱의 사용자 지정 스타일/템플릿과 관련이 있지만 Silverlight 앱 Windows Phone에서는 관련이 없습니다. 사용 중인 시스템 리소스 키, 사용 중인 시각적 상태 집합에 대 한 변경 및 Windows 10 기본 스타일/템플릿에 대 한 성능 향상을 비롯 하 여 기존 사용자 지정 스타일/템플릿이 Windows 10 앱에 적합 하지 않을 수 있는 다른 이유가 있습니다. Windows 10 용 컨트롤의 기본 템플릿 복사본을 새로 편집한 다음 스타일 및 템플릿 사용자 지정을 다시 적용 하는 것이 좋습니다.

UWP 컨트롤에 대 한 자세한 내용은 [함수 별 컨트롤](../design/controls-and-patterns/controls-by-function.md), 컨트롤 [목록](../design/controls-and-patterns/index.md)및 [컨트롤에 대 한 지침](../design/controls-and-patterns/index.md)을 참조 하세요.

##  <a name="design-language-in-windows10"></a>Windows 10의 디자인 언어

Windows Phone Silverlight 앱과 Windows 10 앱 간의 디자인 언어에는 약간의 차이가 있습니다. 모든 세부 정보는 [디자인](https://developer.microsoft.com/windows/apps/design)을 참조 하세요. 디자인 언어 변경에도 불구 하 고 설계 원칙은 일관 되 게 유지 됩니다. attentive, fiercely, 시각적 요소를 줄이고, 디지털 도메인에 대 한 인증에 집중 하는 것이 좋습니다. 특히 입력 체계와 함께 비주얼 계층 구조 사용 표에서 디자인 그리고 유체 애니메이션을 사용 하 여 환경을 만들 수 있습니다.

## <a name="localization-and-globalization"></a>지역화 및 세계화

지역화 된 문자열의 경우 UWP 앱 프로젝트의 Windows Phone Silverlight 프로젝트에서 .resx 파일을 다시 사용할 수 있습니다. 파일을 복사 하 여 프로젝트에 추가한 다음, 조회 메커니즘이 기본적으로 찾을 수 있도록 파일 이름을 Resources. resw로 바꿉니다. **빌드 작업** 을 작업 **리소스로** 설정 하 고 **출력 디렉터리로 복사** 를 **복사 하지**않습니다. 그런 다음 XAML 요소에 **x:Uid** 특성을 지정 하 여 태그에서 문자열을 사용할 수 있습니다. [빠른 시작: 문자열 리소스 사용](/previous-versions/windows/apps/hh965329(v=win.10))을 참조 하세요.

Windows Phone Silverlight 앱은 **CultureInfo** 클래스를 사용 하 여 앱의 세계화를 지원 합니다. UWP 앱은 런타임에 및 Visual Studio 디자인 화면에서 앱 리소스 (지역화, 크기 조정 및 테마)를 동적으로 로드할 수 있도록 하는 MRT.LOG (최신 리소스 기술)를 사용 합니다. 자세한 내용은 [파일, 데이터 및 세계화에 대 한 지침](../design/usability/index.md)을 참조 하세요.

[**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 항목에서는 장치 패밀리 리소스 선택 요소를 기준으로 장치 패밀리 특정 리소스를 로드 하는 방법을 설명 합니다.

## <a name="media-and-graphics"></a>미디어 및 그래픽

UWP 미디어와 그래픽에 대 한 정보를 읽으면 Windows 디자인 원칙에 따라 그래픽 복잡성 및 혼란을 비롯 하 여 불필요 한 모든 것을 fierce 수 있습니다. Windows 디자인은 일반적으로 시각 효과, 서체, 동작 등이 선명하고 명확합니다. 앱이 동일한 원칙을 따르는 경우 기본 제공 앱과 유사 하 게 보일 것입니다.

Windows Phone Silverlight에는 다른 [**브러시**](/uwp/api/Windows.UI.Xaml.Media.Brush) 형식이 있지만 UWP에 없는 **RadialGradientBrush** 형식이 있습니다. 비트맵을 사용 하 여 비슷한 효과를 얻을 수 있는 경우도 있습니다. [Microsoft DirectX](/windows/desktop/directx) 및 XAML c + + UWP에서 Direct2D를 사용 하 여 [방사형 그라데이션 브러시를 만들](/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush) 수 있습니다.

Windows Phone Silverlight에 **OpacityMask** 속성이 있지만이 속성은 UWP [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 형식의 멤버가 아닙니다 (). 비트맵을 사용 하 여 비슷한 효과를 얻을 수 있는 경우도 있습니다. [Microsoft DirectX](/windows/desktop/directx) 및 XAML c + + UWP 앱에서 Direct2D를 사용 하 여 [불투명 마스크를 만들](/windows/desktop/Direct2D/opacity-masks-overview) 수 있습니다. 그러나 **OpacityMask** 의 일반적인 사용 사례는 밝은 테마와 어두운 테마 모두에 맞게 조정 되는 단일 비트맵을 사용 하는 것입니다. 벡터 그래픽의 경우 테마 인식 시스템 브러시 (예: 아래에 설명 된 원형 차트)를 사용할 수 있습니다. 그러나 테마 인식 비트맵 (예: 아래에 설명 된 확인 표시)을 만들려면 다른 방법이 필요 합니다.

![테마 인식 비트맵](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

Windows Phone Silverlight 앱에서이 기술은 전경 브러시를 사용 하 여 채워진 **사각형** 의 **OpacityMask** 로 알파 마스크 (비트맵 형식)를 사용 하는 것입니다.

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

이를 UWP 앱으로 이식 하는 가장 간단한 방법은 다음과 같이 [**BitmapIcon**](/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)를 사용 하는 것입니다.

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

여기에서 winrt \_check.png는 wpslcheck.png는 것과 마찬가지로 비트맵 형식의 알파 마스크 이며 \_ 동일한 파일이 될 수 있습니다. 그러나 \_ 다양 한 크기 조정 요소에 사용 되는 여러 가지 winrtcheck.png 크기를 제공할 수 있습니다. 이에 대 한 자세한 내용과 **너비** 및 **높이** 값의 변경 내용에 대 한 자세한 내용은이 항목의 [보기 또는 효과적 픽셀, 거리 보기 및 배율 인수](#view-or-effective-pixels-viewing-distance-and-scale-factors) 를 참조 하세요.

비트맵의 밝은 테마와 어두운 테마 형식 간에 차이점이 있는 경우에는 보다 일반적인 방법으로, 어두운 전경 (밝은 테마의 경우)과 밝은 전경 (짙은 테마의 경우)을 포함 하는 두 개의 이미지 자산을 사용 합니다. 이 비트맵 자산 집합의 이름을 지정할 수 있는 방법에 대 한 자세한 내용은 [언어, 크기 조정 및 기타 한정자에 대 한 리소스 조정](../app-resources/tailor-resources-lang-scale-contrast.md)을 참조 하세요. 이미지 파일의 집합이 올바르게 지정 되 면 다음과 같이 루트 이름을 사용 하 여 추상에서 해당 파일을 참조할 수 있습니다.

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

Windows Phone Silverlight에서 **UIElement** 속성은 **기** 하 도형을 사용 하 여 표현할 수 있는 모든 셰이프 일 수 있으며, 일반적으로 **STREAMGEOMETRY** 미니 언어의 XAML 태그에서 serialize 됩니다. UWP에서 [**clip**](/uwp/api/windows.ui.xaml.uielement.clip) 속성의 형식은 [**RectangleGeometry**](/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry)이므로 사각형 영역을 클리핑할 수만 있습니다. 미니 언어를 사용 하 여 사각형을 정의 하는 것은 허용 되지 않습니다. 따라서 태그에서 클리핑 영역을 이식 하려면 **클립** 특성 구문을 바꾸고 다음과 유사한 속성 요소 구문으로 만듭니다.

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

[Microsoft DirectX](/windows/desktop/directx) 및 XAML c + + UWP 앱에서 Direct2D를 사용 하 여 [계층의 마스크로 임의의 기 하 도형을 사용할](/windows/desktop/Direct2D/direct2d-layers-overview) 수 있습니다.

## <a name="navigation"></a>탐색

Windows Phone Silverlight 앱의 페이지로 이동 하는 경우 URI (Uniform Resource Identifier) 주소 지정 체계를 사용 합니다.

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

UWP 앱에서 [**Frame. Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate) 메서드를 호출 하 고 페이지의 XAML 태그 정의의 **x:Class** 특성에 정의 된 대상 페이지의 형식을 지정 합니다.


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

WMAppManifest.xml에서 Windows Phone Silverlight 앱의 시작 페이지를 정의합니다.

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

UWP 앱에서 명령적 코드를 사용 하 여 시작 페이지를 정의 합니다. 다음은 방법을 보여 주는 App.xaml.cs의 일부 코드입니다.

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

Uri 매핑 및 조각 탐색은 URI 탐색 기술 이므로 uri를 기반으로 하지 않는 UWP 탐색에는 적용 되지 않습니다. Uri 매핑은 URI 문자열을 사용 하 여 대상 페이지를 식별 하는 약한 형식의 특성에 대 한 응답으로 존재 하며, 페이지를 다른 폴더로 이동 하 여 다른 상대 경로로 이동 해야 하는 취약 및 유지 관리 문제가 발생 합니다. UWP 앱은 강력한 형식 및 컴파일러 검사를 수행 하는 형식 기반 탐색을 사용 하며, URI 매핑에서 해결할 수 있는 문제가 없습니다. 조각 탐색에 대 한 사용 사례는 페이지에서 해당 콘텐츠의 특정 조각을 뷰로 스크롤할 수 있거나 다른 방식으로 표시 되도록 대상 페이지에 일부 컨텍스트를 전달 하는 것입니다. [**Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate) 메서드를 호출할 때 탐색 매개 변수를 전달 하 여 동일한 목표를 달성할 수 있습니다.

자세한 내용은 [탐색](../design/basics/navigation-basics.md)을 참조 하세요.

## <a name="resource-key-reference"></a>리소스 키 참조

디자인 언어가 Windows 10에서 진화 하 고 결과적으로 특정 시스템 스타일이 변경 되었으며 많은 시스템 리소스 키가 제거 되거나 이름이 바뀌었습니다. Visual Studio의 XAML 태그 편집기는 확인할 수 없는 리소스 키에 대 한 참조를 강조 표시 합니다. 예를 들어 XAML 태그 편집기는 빨간색 물결선으로 스타일 키에 대 한 참조에 밑줄을 표시 합니다 `PhoneTextNormalStyle` . 이 문제가 해결 되지 않으면 에뮬레이터 또는 장치에 앱을 배포 하려고 할 때 앱이 즉시 종료 됩니다. 따라서 XAML 태그 정확성에 참석 하는 것이 중요 합니다. Visual Studio는 이러한 문제를 식별하는 데 유용한 도구입니다.

또한 아래의 [텍스트](#text)를 참조 하세요.

## <a name="status-bar-system-tray"></a>상태 표시줄 (시스템 트레이)

시스템 트레이 (XAML 태그에서로 설정 `shell:SystemTray.IsVisible` 됨)는 상태 표시줄 이라고 하며 기본적으로 표시 됩니다. [**Windows.**](/uwp/api/windows.ui.viewmanagement.statusbar.showasync) [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) 메서드를 호출 하 여 명령적 코드에서 표시 유형을 제어할 수 있습니다.

## <a name="text"></a>텍스트

텍스트 (또는 입력 체계)는 UWP 앱의 중요 한 측면이 며 포팅 하는 동안 새 디자인 언어와 조화 되도록 보기의 시각적 디자인을 다시 확인할 수 있습니다. 다음 그림을 사용 하 여 사용할 수 있는 UWP **TextBlock** 시스템 스타일을 찾을 수 있습니다. 사용한 Windows Phone Silverlight 스타일에 해당 하는 항목을 찾습니다. 또는 고유한 범용 스타일을 만들고 Windows Phone Silverlight 시스템 스타일에서 해당 속성을 복사할 수 있습니다.

![windows 10 앱에 대 한 시스템 textblock 스타일](images/label-uwp10stylegallery.png)

Windows 10 앱에 대 한 시스템 TextBlock 스타일

Windows Phone Silverlight 앱에서 기본 글꼴 패밀리는 Segoe WP입니다. Windows 10 앱에서 기본 글꼴 패밀리는 맑은 고딕 됩니다. 따라서 앱의 글꼴 메트릭이 다를 수 있습니다. Windows Phone Silverlight 텍스트의 모양을 재현 하려는 경우 [**Lineheight**](/uwp/api/windows.ui.xaml.controls.textblock.lineheight) 및 [**LineStackingStrategy**](/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy)와 같은 속성을 사용 하 여 고유한 메트릭을 설정할 수 있습니다. 자세한 내용은 [글꼴에 대 한 지침](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) 및 [UWP 앱 디자인](https://developer.microsoft.com/windows/apps/design)을 참조 하세요.

## <a name="theme-changes"></a>테마 변경

Windows Phone Silverlight 앱의 경우 기본 테마는 기본적으로 짙은 색입니다. Windows 10 장치의 경우 기본 테마가 변경 되었지만 app.xaml에서 요청 된 테마를 선언 하 여 사용 되는 테마를 제어할 수 있습니다. 예를 들어 모든 장치에서 짙은 테마를 사용 하려면 `RequestedTheme="Dark"` 루트 응용 프로그램 요소에를 추가 합니다.

## <a name="tiles"></a>타일

UWP 앱에 대 한 타일은 몇 가지 차이점이 있지만 Windows Phone Silverlight 앱에 대 한 라이브 타일과 유사한 동작을 포함 합니다. 예를 들어, 보조 타일을 만들기 위한 **Microsoft. 휴대폰** 을 호출 하는 코드는 [**Secondarytile. requestcreateasync**](/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync)를 호출 하도록 이식 해야 합니다. 다음은 이전 및 이후 예제 이며 먼저 Windows Phone Silverlight 버전입니다.


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

및 UWP와 동일 합니다.

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

[**TileUpdateManager**](/uwp/api/Windows.UI.Notifications.TileUpdateManager), [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater), [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification)및/또는 [**ScheduledTileNotification**](/uwp/api/Windows.UI.Notifications.ScheduledTileNotification) 클래스를 사용 하려면 **ShellTileSchedule** 클래스를 사용 하 여 타일을 업데이트 하는 코드를 이식 **해야 합니다 (** 예를 들어,

타일, 알림을, 배지, 배너 및 알림에 대 한 자세한 내용은 타일 [만들기](/previous-versions/windows/apps/hh868260(v=win.10)) 및 [타일, 배지 및 알림 작업](/previous-versions/windows/apps/hh868259(v=win.10))에 대 한 작업을 참조 하세요. UWP 타일에 사용 되는 시각적 자산의 크기에 대 한 자세한 내용은 [타일 및 알림 시각적 자산](/previous-versions/windows/apps/hh781198(v=win.10))을 참조 하세요.

## <a name="toasts"></a>알림을

ScheduledToastNotification를 사용 하 여 알림 메시지를 표시 하는 코드는 [**To Notificationmanager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager), [**to **](/uwp/api/Windows.UI.Notifications.ToastNotifier) [**notification, to notification**](/uwp/api/Windows.UI.Notifications.ToastNotification)및/또는 [**ScheduledToastNotification**](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) 클래스를 사용 하도록 포팅 되어야 **합니다.** 모바일 장치에서 "알림"에 대 한 소비자 지향 용어는 "배너"입니다.

[타일, 배지 및 알림 작업](/previous-versions/windows/apps/hh868259(v=win.10))을 참조 하세요.

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>표시 또는 효과적 픽셀, 거리 보기 및 배율 인수

Windows Phone Silverlight 앱 및 Windows 10 앱은 UI 요소의 크기와 레이아웃을 실제 물리적 크기 및 해상도에서 벗어나 추상화 하는 방식에 차이가 있습니다. Windows Phone Silverlight 앱은 보기 픽셀을 사용 하 여이 작업을 수행 합니다. Windows 10에서 보기 픽셀의 개념은 효과적인 픽셀의 개념으로 구체화 되었습니다. 다음은 해당 용어에 대 한 설명, 의미 및 제공 되는 추가 가치입니다.

"해결 방법" 이라는 용어는 일반적으로 픽셀 수와 같이 픽셀 밀도를 측정 하는 것을 의미 합니다. "효과적인 해결 방법"은 이미지 또는 문자 모양을 구성 하는 실제 픽셀이 장치의 거리와 물리적 픽셀 크기 (픽셀 밀도가 실제 픽셀 크기의 상호)를 확인 하는 경우의 차이를 확인 하는 방법입니다. 효과적인 해결 방법은 사용자 중심 이므로 경험을 구축 하는 좋은 메트릭입니다. 모든 요인을 이해 하 고 UI 요소의 크기를 제어 하 여 사용자의 경험을 적절 하 게 만들 수 있습니다.

Windows Phone Silverlight 앱에는 화면에 표시 되는 실제 픽셀 수와 픽셀 밀도 또는 실제 크기에 관계 없이 모든 전화 화면이 정확 하 게 480 보기 픽셀 너비입니다. 즉,을 사용 하는 **Image** 요소는 `Width="48"` Windows Phone Silverlight 앱을 실행할 수 있는 모든 휴대폰 화면 너비의 1/10입니다.

Windows 10 앱의 경우 모든 장치가 고정 된 유효 픽셀 수의 일부는 *아닙니다* . UWP 앱을 실행할 수 있는 다양 한 장치를 제공 하는 것이 확실 합니다. 서로 다른 장치는 가장 작은 장치에 대해 320 window.epx.codesnippet부터, 적당 한 크기의 모니터의 경우 1024 window.epx.codesnippet까지, 그리고 훨씬 더 높은 너비까지 다양 한 유효 픽셀 수를 가집니다. 항상 자동 크기 요소 및 동적 레이아웃 패널을 사용 하는 것이 좋습니다. UI 요소의 속성을 XAML 태그의 고정 크기로 설정 하는 경우도 있습니다. 크기 조정 요소는 앱이 실행 되는 장치 및 사용자가 만든 표시 설정에 따라 앱에 자동으로 적용 됩니다. 그리고 이러한 크기 조정 요소를 사용 하면 다양 한 화면 크기를 통해 사용자에 게 일관 된 크기의 고정 된 (및 읽기) 대상을 제공 하는 고정 크기의 UI 요소를 유지할 수 있습니다. 그리고 동적 레이아웃을 사용 하는 경우 UI는 다른 장치에서 크기가 optically는 않지만, 사용 가능한 공간에 적절 한 양의 콘텐츠를 맞추는 데 필요한 작업을 대신 수행 합니다.

480는 일반적으로 휴대폰 크기의 화면에 대 한 보기 픽셀의 고정 너비 이며, 해당 값은 일반적으로 유효 픽셀에서 더 작으므로 Windows Phone Silverlight 앱 태그의 모든 차원을 0.8의 요소에 곱합니다.

앱이 모든 디스플레이에서 최상의 환경을 제공 하기 위해 각각 특정 배율 인수에 적합 한 크기의 범위로 각 비트맵 자산을 만드는 것이 좋습니다. 100% 규모, 200% 규모 및 400% 규모 (해당 우선 순위)로 자산을 제공 하면 대부분의 경우 중간 규모의 모든 요소에서 뛰어난 결과를 얻을 수 있습니다.

**참고**    어떤 이유로 든, 여러 크기의 자산을 만들 수 없는 경우 100% 규모의 자산을 만들 수 있습니다. Microsoft Visual Studio에서 UWP 앱에 대 한 기본 프로젝트 템플릿은 브랜딩 자산 (타일 이미지 및 로고)을 한 크기로 제공 하지만 100% 눈금은 제공 하지 않습니다. 사용자 고유의 앱에 대 한 자산을 제작 하는 경우이 섹션의 지침에 따라 100%, 200% 및 400% 크기를 제공 하 고 자산 팩을 사용 합니다.

복잡 한 아트 워크가 있는 경우 더 많은 크기의 자산을 제공 하는 것이 좋습니다. 벡터 아트를 사용 하 여 시작 하는 경우에는 규모에 관계 없이 고품질의 자산을 생성 하기가 비교적 쉽습니다.

모든 크기 조정 요인을 지원 하지 않는 것이 좋지만 Windows 10 앱에 대 한 크기 조정 요소의 전체 목록은 100%, 125%, 150%, 200%, 250%, 300% 및 400%입니다. 제공 하는 경우 저장소에서 각 장치에 대 한 올바른 크기의 자산을 선택 하 고 해당 자산만 다운로드 됩니다. 스토어는 장치의 DPI에 따라 다운로드할 자산을 선택 합니다.

자세한 내용은 [UWP 앱 용 반응 형 디자인 101](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)을 참조 하세요.

## <a name="window-size"></a>창 크기

UWP 앱에서 명령적 코드를 사용 하 여 최소 크기 (너비와 높이 모두)를 지정할 수 있습니다. 기본 최소 크기는 500x320epx 이며이는 허용 되는 최소 최소 크기 이기도 합니다. 허용 되는 가장 큰 최소 크기는 500x500epx입니다.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

다음 항목은 [i/o, 장치 및 앱 모델에 대해 포팅](wpsl-to-uwp-input-and-sensors.md)하는 항목입니다.

## <a name="related-topics"></a>관련 항목

* [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)