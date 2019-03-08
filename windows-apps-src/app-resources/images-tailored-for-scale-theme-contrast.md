---
Description: 앱은 디스플레이 배율 인수, 테마, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 이미지를 포함하는 이미지 리소스 파일을 로드할 수 있습니다.
title: 배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 6f4749b8560624ed58f43b33fe3373d909919347
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592028"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드
앱은 [디스플레이 배율 인수](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), 테마, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 이미지 리소스 파일(또는 기타 자산 파일)을 로드할 수 있습니다. 이러한 이미지는 명령적 코드 또는 XAML 태그에서 참조할 수 있으며 예를 들어 **이미지**의 **Source** 속성입니다. 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest`의 파일)에 표시될 수도 있습니다. 예를 들면 Visual Studio 매니페스트 디자이너의 시각적 자산 탭에 앱 아이콘으로 또는 타일 또는 알림에 표시됩니다. 이미지의 파일 이름에 한정자를 사용하고 선택적으로 이를 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)를 사용하여 동적으로 로드하여 디스플레이 배율, 테마, 고대비, 언어 및 다른 컨텍스트에 대한 사용자의 런타임 설정과 가장 적합한 가장 적절한 이미지 파일이 로드되도록 할 수 있습니다.

이미지 리소스는 이미지 리소스 파일에 포함되어 있습니다. 이미지를 자산으로, 이를 포함하는 파일을 자산 파일로 생각할 수 있습니다. 그리고 이러한 리소스 파일을 프로젝트의 \Assets 폴더에서 찾을 수 있습니다. 이미지 리소스 파일 이름에 한정자를 사용하는 방법에 대한 배경 지식은 [언어, 규모 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)을 참조하세요.

이미지에 대한 일반적인 한정자는 [scale](tailor-resources-lang-scale-contrast.md#scale), [theme](tailor-resources-lang-scale-contrast.md#theme), [contrast](tailor-resources-lang-scale-contrast.md#contrast), [targetsize](tailor-resources-lang-scale-contrast.md#targetsize) 등이 있습니다.

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>배율, 테마, 대비에 대한 이미지 리소스 인증
`scale` 한정자의 기본값은 `scale-100`입니다. 따라서 이러한 두 변형은 동일합니다(둘 다 이미지를 배율 100 또는 배율 인수 1로 제공).

```
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
```

파일 이름 대신 폴더 이름에서 한정자를 사용할 수 있습니다. 한정자별로 여러 자산 파일을 가지는 것이 좋은 전략일 것입니다. 설명을 위해 이 두 변형은 위의 두 변형과 동일합니다.

```
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
```

다음은 디스플레이 배율, 테마 및 고대비의 다른 설정에 대해 `/Assets/Images/logo.png`라는 이미지 리소스의 변형을 제공하는 방법의 예입니다. 이 예에서는 폴더 이름 지정을 사용합니다.

```
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
```

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>XAML 태그와 코드에서 이미지 또는 다른 자산 참조
리소스 이미지의 이름&mdash;또는&mdash;식별자는 모든 한정자가 제거된 경로 및 파일 이름입니다. 폴더 및/또는 파일의 이름을 이전 섹션의 예와 같이 지정하는 경우 단일 이미지 리소스를 가지며 이름(절대 경로로서)은 `/Assets/Images/logo.png`입니다. 다음은 XAML 태그에서 해당 이름을 사용하는 방법입니다.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

여기에서 앱 패키지에서 제공되는 파일을 참조하기 때문에 `ms-appx` URI 스키마를 사용합니다. [URI 스키마](uri-schemes.md)를 참조하세요. 명령적 코드에서 동일한 이미지를 참조하는 방법은 다음과 같습니다.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

`ms-appx`를 사용하여 앱 패키지에서 임의의 어떤 파일이든 로드할 수 있습니다.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

`ms-appx-web` 스키마는 웹 구획의 `ms-appx`와 동일한 파일에 액세스합니다.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

이러한 예에 제공된 모든 시나리오는 [UriKind](https://docs.microsoft.com/en-us/dotnet/api/system.urikind)를 유추하는 [Uri 생성자](https://docs.microsoft.com/en-us/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) 오버로드를 사용합니다. 스키마 또는 기관을 포함하는 유효한 절대 URI를 지정하거나 위의 예에서와 같이 앱 패키지 기관 기본값을 기다립니다.

이 예 URI에서 스키마("`ms-appx`" 또는 "`ms-appx-web`") 다음에 "`://`"로 시작하는 절대 경로가 나오는 것을 확인하세요. 절대 경로에서 처음 "`/`"는 패키지 루트에서 경로를 해석하도록 합니다.

**참고**`ms-resource`([문자열 리소스](localize-strings-ui-manifest.md)에 대해) 및 `ms-appx(-web)`(이미지 및 기타 자산에 대해) URI 스키마는 자동 한정자 일치를 수행하여 현재 컨택스트에 가장 적합한 리소스를 찾을 수 있습니다. `ms-appdata` URI 스키마(앱 데이터를 로드하는 데 사용됨)는 모든 이러한 자동 일치를 수행하지 않지만 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)의 콘텐츠에 응답할 수 있으며 URI에 전체 실제 파일 이름을 사용하는 앱 데이터에서 명시적으로 적절한 자산을 로드할 수 있습니다. 앱 데이터에 대한 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)을 참조하세요. 웹 URI 스키마(`http`, `https`, `ftp` 등)는 자동 일치를 수행하지 않습니다. 이 경우 수행할 작업에 대한 정보는 [클라우드에 이미지 호스팅 및 로드](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud)를 참조하세요.

이미지 파일이 프로젝트 구조의 위치에 있는 경우 절대 경로가 좋은 선택입니다. 이미지 파일을 이동하려고 하지만 참조 XAML 태그 파일과 관련된 동일한 위치에 있도록 하고자 하는 경우 절대 경로 대신 포함하는 태그 파일과 관련된 경로를 사용하고자 할 수 있습니다. 이렇게 하면 URI 스키마를 사용하지 않아도 됩니다. 이 경우 XAML 태그에서 상대 경로 사용하기 때문에 자동 한정자 일치를 여전히 사용할 수 있습니다.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

[언어, 배율, 고대비에 대한 타일 및 알림 메시지](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)도 참조하세요.

## <a name="qualify-an-image-resource-for-targetsize"></a>targetsize에 대한 이미지 리소스 인증
동일한 이미지 리소스의 여러 변형에 `scale`및 `targetsize` 한정자를 사용할 수 있지만 리소스의 단일 변형에 둘 다를 사용할 수는 없습니다. 또한 `TargetSize` 한정자 없이 하나 이상의 변형을 정의해야 합니다. 해당 변형은 `scale`에 대한 값을 정의하거나 기본값 `scale-100`을 가져야 합니다. 따라서 `/Assets/Square44x44Logo.png` 리소스의 이러한 두 변형은 유효합니다.

```
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
```

그리고 이 두 변형은 유효합니다. 

```
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
```

하지만 이 변형은 잘못되었습니다.

```
\Assets\Square44x44Logo.scale-200_targetsize-24.png
```

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>앱 패키지 매니페스트에서 이미지 파일 참조
폴더 및/또는 파일의 이름을 이전 섹션의 유효한 두 예 중 하나와 같이 지정하는 경우 단일 앱 아이콘 이미지 리소스를 가지며 이름(상대 경로로서)은 `Assets\Square44x44Logo.png`입니다. 앱 패키지 매니페스트에서 이름으로 리소스를 참조합니다. 모든 URI 스키마를 사용하지 않아도 됩니다.

![리소스 추가, 영어](images/app-icon.png)

이렇게만 하면 운영 체제는 자동 한정자 일치를 수행하여 현재 컨택스트에 가장 적합한 리소스를 찾을 수 있습니다. 지역화하거나 이러한 방법으로 자격을 얻을 수 있는 앱 패키지 메니페스트의 모든 항목 목록을 보려면 [지역화할 수 있는 매니페스트 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)을 참조하세요.

## <a name="qualify-an-image-resource-for-layoutdirection"></a>layoutdirection에 대한 이미지 리소스 인증
[이미지 미러링](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)을 참조하세요.

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>특정 언어 또는 다른 컨텍스트에 대한 이미지 로드
앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

기본 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)([**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)에서 가져옴)에는 기본 런타임 컨텍스트(즉, 현재 사용자와 시스템에 대한 설정)를 나타내는 각 한정자 이름에 대한 한정자 값이 포함됩니다. 이미지 파일은 이름의 한정자에 따라 해당 런타임 컨텍스트의 한정자 값에 대해 일치합니다.

하지만 앱에서 시스템 설정을 재정의하여 로드할 일치하는 이미지를 찾을 때 사용할 언어, 배율, 또는 기타 한정자 값을 명시하고자 할 수 있습니다. 예를 들어 로드할 고대비 이미지 및 그 시점을, 정확하게 제어하고자 할 수 있습니다.

이는 새 **ResourceContext**(기본값 대신)를 구성하고 해당 값을 재정의한 다음 이미지 조회에서 해당 컨텍스트 개체를 사용하여 수행할 수 있습니다.

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

전역 수준에서 같은 효과를 적용하려면 기본 **ResourceContext**의 한정자 값을 재정의*할 수* 있습니다. 그러나 이 대신 [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)를 호출하는 것이 좋습니다. **SetGlobalQualifierValue**를 호출하는 값을 한 번 설정하면 조회를 위해 해당 값을 사용할 때마다 기본 **ResourceContext**에 적용됩니다. 기본적으로 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 클래스는 **ResourceContext** 기본값을 사용합니다.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>한정자 값 변경 이벤트에 대한 응답으로 이미지 업데이트
실행 중인 앱은 기본 리소스 컨텍스트의 한정자 값에 영향을 미치는 시스템 설정의 변경에 응답할 수 있습니다. 이러한 모든 시스템 설정은 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)에서 [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 이벤트를 호출합니다.

이 이벤트에 대한 응답으로 기본적으로 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)가 사용하는 **ResourceContext** 기본값의 도움으로 이미지를 다시 로드할 수 있습니다.

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
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>관련 항목
* [언어, 배율 및 다른 한정자에 대 한 리소스를 조정 합니다.](tailor-resources-lang-scale-contrast.md)
* [UI 및 앱 패키지 매니페스트에 문자열 지역화](localize-strings-ui-manifest.md)
* [설정 및 기타 앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)
* [언어, 배율 및 고대비에 대 한 타일 및 알림 지원](tile-toast-language-scale-contrast.md)
* [매니페스트 지역화 가능한 항목](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [미러링 이미지](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [세계화 및 지역화](../design/globalizing/globalizing-portal.md)
