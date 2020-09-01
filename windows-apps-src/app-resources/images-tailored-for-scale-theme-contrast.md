---
Description: 앱은 디스플레이 배율 인수, 테마, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 이미지를 포함하는 이미지 리소스 파일을 로드할 수 있습니다.
title: 배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 0a1d6639a901385c3a33fb0ed670b9b7e4cf683e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157607"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드
앱은 [표시 배율 인수](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), 테마, 고대비 및 기타 런타임 컨텍스트에 맞게 조정 된 이미지 리소스 파일 (또는 기타 자산 파일)을 로드할 수 있습니다. 이러한 이미지는 명령 코드 또는 XAML 태그 (예: **이미지**의 **원본** 속성)에서 참조할 수 있습니다. 응용 프로그램 패키지 매니페스트 소스 파일 (파일)에 표시 될 수도 있습니다 `Package.appxmanifest` &mdash; . 예를 들어, Visual Studio 매니페스트 디자이너의 시각적 자산 탭에 있는 앱 아이콘에 대 한 값, &mdash; 타일 및 알림을 표시 될 수 있습니다. 이미지의 파일 이름에 한정자를 사용 하 고 필요에 따라 [**Resourcecontext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)를 사용 하 여 동적으로 로드 하면 표시 배율, 테마, 고대비, 언어 및 기타 컨텍스트의 사용자 런타임 설정과 가장 일치 하는 가장 적합 한 이미지 파일이 로드 될 수 있습니다.

이미지 리소스는 이미지 리소스 파일에 포함 되어 있습니다. 이미지를 자산으로 간주 하 고 자산 파일로 포함 하는 파일을 볼 수도 있습니다. 프로젝트의 \Assets 폴더에서 이러한 종류의 리소스 파일을 찾을 수 있습니다. 이미지 리소스 파일의 이름에 한정자를 사용 하는 방법에 대 한 배경 지식이 있으면 [언어, 크기 조정 및 기타 한정자에 대 한 리소스를 조정](tailor-resources-lang-scale-contrast.md)합니다 .를 참조 하세요.

이미지에 대 한 몇 가지 일반적인 한정자는 [scale](tailor-resources-lang-scale-contrast.md#scale), [theme](tailor-resources-lang-scale-contrast.md#theme), [contrast](tailor-resources-lang-scale-contrast.md#contrast)및 [targetsize](tailor-resources-lang-scale-contrast.md#targetsize)입니다.

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>크기 조정, 테마 및 대비를 위한 이미지 리소스를 한정 합니다.
한정자의 기본값은 `scale` `scale-100` 입니다. 따라서 이러한 두 변형은 동일 합니다. (둘 다 규모 100 또는 배율 인수 1에서 이미지를 제공 합니다.)

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
</pre>
</blockquote>


파일 이름 대신 폴더 이름에 한정자를 사용할 수 있습니다. 한정자 당 여러 자산 파일이 있는 경우 더 나은 전략입니다. 설명의 목적에 맞게 이러한 두 가지 변형은 위와 동일 합니다.

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
</pre>
</blockquote>

다음은 &mdash; `/Assets/Images/logo.png` &mdash; 표시 배율, 테마 및 고대비의 다양 한 설정에 대해 이름이 인 이미지 리소스의 변형을 제공 하는 방법의 예입니다. 이 예제에서는 폴더 이름을 사용 합니다.

<blockquote>
<pre>
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
</pre>
</blockquote>

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>XAML 태그 및 코드에서 이미지 또는 기타 자산 참조
&mdash;이미지 리소스의 이름 또는 식별자는 모든 &mdash; 한정자가 제거 된 경로 및 파일 이름입니다. 이전 섹션의 예제에서와 같이 폴더 및/또는 파일의 이름을 지정한 경우 단일 이미지 리소스와 해당 이름 (절대 경로)이 있습니다 `/Assets/Images/logo.png` . XAML 태그에서 해당 이름을 사용 하는 방법은 다음과 같습니다.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

`ms-appx`응용 프로그램의 패키지에서 제공 되는 파일을 참조 하 고 있으므로 URI 스키마를 사용 합니다. [URI 체계](uri-schemes.md)를 참조 하세요. 명령적 코드에서 동일한 이미지 리소스를 참조 하는 방법은 다음과 같습니다.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

`ms-appx`를 사용 하 여 앱 패키지에서 임의의 파일을 로드할 수 있습니다.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

`ms-appx-web`체계는 `ms-appx` 웹 구획에서와 동일한 파일에 액세스 합니다.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

이러한 예제에 표시 된 시나리오의 경우 [Urikind](/dotnet/api/system.urikind)를 유추 하는 [Uri 생성자](/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) 오버 로드를 사용 합니다. 스키마와 인증 기관을 포함 하 여 유효한 절대 URI를 지정 하거나, 위의 예제에서와 같이 권한을 기본 앱 패키지로 지정 합니다.

이러한 예제 Uri에서 스키마 (" `ms-appx` " 또는 " `ms-appx-web` ") 뒤에는 절대 경로가 오는 " `://` "가 나옵니다. 절대 경로에서 ""는 경로를 `/` 패키지의 루트에서 해석 합니다.

> [!NOTE]
> `ms-resource`( [문자열 리소스](localize-strings-ui-manifest.md)의 경우) 및 `ms-appx(-web)` (이미지 및 기타 자산의 경우) URI 체계는 자동 한정자 일치를 수행 하 여 현재 컨텍스트에 가장 적합 한 리소스를 찾습니다. `ms-appdata`응용 프로그램 데이터를 로드 하는 데 사용 되는 URI 체계는 이러한 자동 일치를 수행 하지 않지만, [QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 의 콘텐츠에 응답 하 고 URI에서 전체 물리적 파일 이름을 사용 하 여 앱 데이터에서 적절 한 자산을 명시적으로 로드할 수 있습니다. 앱 데이터에 대 한 정보는 [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)을 참조 하세요. 웹 URI 스키마 (예: `http` , `https` 및 `ftp` )는 자동 일치를 수행 하지 않습니다. 이 경우 수행할 작업에 대 한 정보는 [클라우드에서 이미지 호스팅 및 로드](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud)를 참조 하세요.

절대 경로는 이미지 파일이 프로젝트 구조에 있는 위치에 유지 되는 경우에 적합 합니다. 이미지 파일을 이동할 수는 있지만 참조 하는 XAML 태그 파일을 기준으로 동일한 위치에 유지 되는 경우에는 절대 경로 대신 포함 하는 태그 파일을 기준으로 하는 경로를 사용 하는 것이 좋습니다. 이렇게 하는 경우 URI 체계를 있으므로 사용 합니다. 이 경우에는 XAML 태그에서 상대 경로를 사용 하는 경우에만 자동 한정자 일치를 활용 하는 것이 좋습니다.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

또한 [언어, 배율 및 고대비에 대 한 타일 및 알림 지원](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)을 참조 하세요.

## <a name="qualify-an-image-resource-for-targetsize"></a>Targetsize에 대 한 이미지 리소스 한정
`scale` `targetsize` 동일한 이미지 리소스의 서로 다른 변형에 및 한정자를 사용할 수 있지만 리소스의 단일 변형에서 사용할 수 없습니다. 또한 한정자가 없는 변형을 하나 이상 정의 해야 `TargetSize` 합니다. 해당 variant는에 대 한 값을 정의 해야 `scale` 합니다. 그렇지 않으면로 기본값을 지정 해야 `scale-100` 합니다. 따라서 리소스의 이러한 두 가지 변형이 `/Assets/Square44x44Logo.png` 유효 합니다.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

그리고 이러한 두 가지 변형이 유효 합니다. 

<blockquote>
<pre>
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

그러나이 변형은 유효 하지 않습니다.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200_targetsize-24.png
</pre>
</blockquote>

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>앱 패키지 매니페스트에서 이미지 파일을 참조 하세요.
이전 섹션의 두 가지 유효한 예제 중 하나에서 폴더 및/또는 파일의 이름을 지정한 경우 단일 앱 아이콘 이미지 리소스가 있고 해당 이름 (상대 경로)은 `Assets\Square44x44Logo.png` 입니다. 앱 패키지 매니페스트에서 리소스를 이름으로 참조 하기만 하면 됩니다. URI 체계를 사용할 필요가 없습니다.

![리소스 추가, 영어](images/app-icon.png)

모든 작업을 수행 해야 하며, OS는 자동 한정자 일치를 수행 하 여 현재 컨텍스트에 가장 적합 한 리소스를 찾습니다. 지역화 하거나 이러한 방식으로 한정할 수 있는 앱 패키지 매니페스트의 모든 항목 목록은 [지역화 가능한 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)을 참조 하세요.

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Layoutdirection에 대해 이미지 리소스 한정
[미러링 이미지](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)를 참조 하세요.

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>특정 언어 또는 기타 컨텍스트에 대 한 이미지 로드
앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

기본 [**resourcecontext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) ( [**GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)에서 가져옴)에는 각 한정자 이름에 대 한 한정자 값이 포함 되어 있으며,이 값은 기본 런타임 컨텍스트 (즉, 현재 사용자 및 컴퓨터에 대 한 설정)를 나타냅니다. 이미지 파일은 해당 &mdash; 런타임 컨텍스트의 한정자 값에 대해 이름에 있는 한정자를 기준으로 일치 &mdash; 합니다.

그러나 앱에서 시스템 설정을 재정의 하 고 일치 하는 이미지를 찾을 때 사용할 언어, 배율 또는 다른 한정자 값을 명시적으로 지정할 수 있습니다. 예를 들어 로드 되는 고대비 이미지를 정확 하 게 제어 하는 것이 좋습니다.

이렇게 하려면 기본 항목을 사용 하는 대신 새 **Resourcecontext** 를 생성 하 고 해당 값을 재정의 한 다음 이미지 조회에서 해당 컨텍스트 개체를 사용 합니다.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

전역 수준에서 동일한 효과를 위해 기본 **Resourcecontext**에서 한정자 값을 재정의할 *수* 있습니다. 그러나 대신 [**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)를 호출 하는 것이 좋습니다. **SetGlobalQualifierValue** 에 대 한 호출을 사용 하 여 값을 한 번 설정 하면 해당 값이 조회에 사용할 때마다 기본 **resourcecontext** 에 적용 됩니다. 기본적으로 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 클래스는 기본 **resourcecontext**를 사용 합니다.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>한정자 값 변경 이벤트에 대 한 응답으로 이미지 업데이트
실행 중인 앱은 기본 리소스 컨텍스트의 한정자 값에 영향을 주는 시스템 설정 변경에 응답할 수 있습니다. 이러한 시스템 설정은 [**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)에서 [**mapchanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 이벤트를 호출 합니다.

이 이벤트에 대 한 응답으로 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 에서 기본적으로 사용 하는 기본 **resourcecontext**의 도움으로 이미지를 다시 로드할 수 있습니다.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>중요 API
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>관련 항목
* [언어, 크기 조정 및 기타 한정자에 맞게 리소스를 조정 합니다.](tailor-resources-lang-scale-contrast.md)
* [UI 및 앱 패키지 매니페스트의 문자열 지역화](localize-strings-ui-manifest.md)
* [설정과 기타 앱 데이터의 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)
* [언어, 배율 및 고대비에 대 한 타일 및 알림 지원](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
* [지역화할 수 있는 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [미러링 이미지](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [세계화 및 지역화](../design/globalizing/globalizing-portal.md)