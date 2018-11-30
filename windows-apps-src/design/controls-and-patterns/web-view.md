---
Description: A web view control embeds a view into your app that renders web content using the Microsoft Edge rendering engine. Hyperlinks can also appear and function in a web view control.
title: 웹 보기
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2a29f58ff8dc842fd985a44f94ff44baea51dc2e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8205478"
---
# <a name="web-view"></a>웹 보기
 

웹 보기 컨트롤은 Microsoft Edge 렌더링 엔진을 사용하여 웹 콘텐츠를 렌더링하는 보기를 앱에 포함합니다. 웹 보기 컨트롤에 하이퍼링크를 표시하고 사용할 수도 있습니다.

> **중요 API**: [WebView 클래스](https://msdn.microsoft.com/library/windows/apps/br227702)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

웹 보기 컨트롤을 사용하여 원격 웹 서버, 동적으로 생성된 코드 또는 앱 패키지의 콘텐츠 파일에서 다양한 서식이 지정된 HTML 콘텐츠를 표시합니다. 서식 있는 콘텐츠는 스크립트 코드를 포함하고 스크립트와 앱 코드 간에 통신할 수도 있습니다.

## <a name="create-a-web-view"></a>웹 보기 만들기

**웹 보기의 모양 수정**

[WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx)는 [Control](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx) 하위 클래스가 아니므로 컨트롤 템플릿이 없습니다. 그러나 다양한 속성을 설정하여 웹 보기의 일부 시각적 측면을 제어할 수 있습니다.
- 표시 영역을 제한하려면 [Width](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) 및 [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 속성을 설정합니다. 
- 웹 보기를 변환, 크기 조정, 기울이기 및 회전하려면 [RenderTransform](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransform.aspx) 속성을 사용합니다.
- 웹 보기의 불투명도를 제어하려면 [Opacity](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) 속성을 설정합니다.
- HTML 콘텐츠에서 색을 지정하지 않는 경우 웹 페이지 배경으로 사용할 색을 지정하려면 [DefaultBackgroundColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultbackgroundcolor.aspx) 속성을 설정합니다. 

**웹 페이지 제목 가져오기**

[DocumentTitle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.documenttitle.aspx) 속성을 사용하여 현재 웹 보기에 표시된 HTML 문서의 제목을 가져올 수 있습니다. 

**입력 이벤트 및 탭 순서**

WebView는 Control 하위 클래스가 아니지만 키보드 입력 포커스를 받고 탭 순서에 참여합니다. [Focus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.focus.aspx) 메서드와 [GotFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.gotfocus.aspx) 및 [LostFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.lostfocus.aspx) 이벤트를 제공하지만 탭 관련 속성은 없습니다. 탭 순서상 해당 위치는 XAML 문서 순서상 위치와 같습니다. 탭 순서에는 입력 포커스를 받을 수 있는 웹 보기 콘텐츠의 모든 요소가 포함됩니다. 

[WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 클래스 페이지의 이벤트 테이블에 표시된 대로 웹 보기는 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)에서 상속된 사용자 입력 이벤트(예: [KeyDown](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keydown.aspx), [KeyUp](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keyup.aspx) 및 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx))를 대부분 지원하지 않습니다. 대신, JavaScript eval 함수와 함께 [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx)를 사용하여 HTML 이벤트 처리기를 사용하고, HTML 이벤트 처리기의 **window.external.notify**를 통해 [WebView.ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx)를 사용하는 응용 프로그램에 알릴 수 있습니다.

### <a name="navigating-to-content"></a>콘텐츠로 이동

웹 보기는 [GoBack](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goback.aspx), [GoForward](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goforward.aspx), [Stop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.stop.aspx), [Refresh](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.refresh.aspx), [CanGoBack](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoback.aspx), [CanGoForward](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoforward.aspx) 등 기본 탐색을 위한 여러 API를 제공합니다. 이러한 기능을 사용하여 일반적인 웹 검색 기능을 앱에 추가할 수 있습니다. 

웹 보기의 초기 콘텐츠를 설정하려면 XAML에서 [Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.source.aspx) 속성을 설정합니다. XAML 파서는 문자열을 [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.uri.aspx)로 자동으로 변환합니다. 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

코드에서 Source 속성을 설정할 수 있지만, 이렇게 하는 대신 일반적으로 **Navigate** 메서드 중 하나를 사용하여 코드에서 콘텐츠를 로드합니다. 

웹 콘텐츠를 로드하려면 http 또는 https 체계를 사용하는 **Uri**와 함께 [Navigate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigate.aspx) 메서드를 사용합니다. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

POST 요청과 HTTP 헤더가 포함된 URI로 이동하려면 [NavigateWithHttpRequestMessage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage.aspx) 메서드를 사용합니다. 이 메서드는 [HttpRequestMessage.Method](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.post.aspx) 속성 값에 대해 [HttpMethod.Post](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.get.aspx) 및 [HttpMethod.Get](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httprequestmessage.method.aspx)만 지원합니다. 

앱의 [LocalFolder]() 또는 [TemporaryFolder]() 데이터 저장소에서 압축 및 암호화되지 않은 콘텐츠를 로드하려면 [ms-appdata 체계]()를 사용하는 **Uri**와 함께 **Navigate** 메서드를 사용합니다. 웹 보기에서 이 체계를 지원하려면 로컬 또는 임시 폴더 아래의 하위 폴더에 콘텐츠를 배치해야 합니다. 이렇게 하면 ms-appdata:///local/*folder*/*file*.html 및 ms-appdata:///temp/*folder*/*file*.html과 같은 URI로 이동할 수 있습니다. 압축 또는 암호화된 파일을 로드하려면 [NavigateToLocalStreamUri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx)를 참조하세요. 

이러한 각 첫 번째 수준 하위 폴더는 다른 첫 번째 수준 하위 폴더의 콘텐츠와 분리됩니다. 예를 들어 ms-appdata:///temp/folder1/file.html로 이동할 수 있지만 이 파일에서 ms-appdata:///temp/folder2/file.html에 대한 링크를 사용할 수는 없습니다. 그러나 **ms-appx-web scheme**을 사용하여 앱 패키지의 HTML 콘텐츠에 연결하고, **http** 및 **https** URI 체계를 사용하여 웹 콘텐츠에 연결할 수 있습니다.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

앱 패키지에서 콘텐츠를 로드하려면 [ms-appx-web scheme](https://msdn.microsoft.com/library/windows/apps/xaml/jj655406.aspx#ms_appx_web)을 사용하는 **Uri**와 함께 **Navigate** 메서드를 사용합니다. 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

[NavigateToLocalStreamUri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx) 메서드를 사용하여 사용자 지정 확인자를 통해 로컬 콘텐츠를 로드할 수 있습니다. 이 기능을 통해 웹 기반 콘텐츠를 오프라인에서 사용하기 위해 다운로드 및 캐시, 압축 파일에서 콘텐츠 추출 등의 고급 시나리오를 사용할 수 있습니다.

### <a name="responding-to-navigation-events"></a>탐색 이벤트에 응답

웹 보기 컨트롤은 탐색 및 콘텐츠 로드 상태에 응답하는 데 사용할 수 있는 몇 가지 이벤트를 제공합니다. 이벤트는 루트 웹 보기 콘텐츠에 대해 다음 순서로 발생합니다. [NavigationStarting](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationstarting.aspx), [ContentLoading](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.contentloading.aspx), [DOMContentLoaded](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.domcontentloaded.aspx), [NavigationCompleted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationcompleted.aspx)


**NavigationStarting** - 웹 보기가 새 콘텐츠로 이동하기 전에 발생합니다. WebViewNavigationStartingEventArgs.Cancel 속성을 true로 설정하여 이 이벤트 처리기에서 탐색을 취소할 수 있습니다. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** - 웹 보기가 새 콘텐츠 로드를 시작했을 때 발생합니다. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** - 웹 보기가 현재 HTML 콘텐츠의 구문 분석을 완료했을 때 발생합니다. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** - 웹 보기가 현재 콘텐츠 로드를 완료했거나 탐색에 실패한 경우에 발생합니다. 탐색에 실패했는지 여부를 확인하려면 [WebViewNavigationCompletedEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess.aspx) 클래스의 [IsSuccess](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus.aspx) 및 [WebErrorStatus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.aspx) 속성을 확인합니다. 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

웹 보기 콘텐츠의 각 **iframe**에 대해 유사한 이벤트가 동일한 순서로 발생합니다. 
- [FrameNavigationStarting](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationstarting.aspx) - 웹 보기의 프레임이 새 콘텐츠로 이동하기 전에 발생합니다. 
- [FrameContentLoading](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framecontentloading.aspx) - 웹 보기의 프레임이 새 콘텐츠 로드를 시작했을 때 발생합니다. 
- [FrameDOMContentLoaded](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framedomcontentloaded.aspx) - 웹 보기의 프레임이 현재 HTML 콘텐츠의 구문 분석을 완료했을 때 발생합니다. 
- [FrameNavigationCompleted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationcompleted.aspx) - 웹 보기의 프레임이 해당 콘텐츠 로드를 완료했을 때 발생합니다. 

### <a name="responding-to-potential-problems"></a>잠재적 문제에 응답

장기 실행 스크립트, 웹 보기에서 로드할 수 없는 콘텐츠, 안전하지 않은 콘텐츠 경고와 같은 잠재적인 콘텐츠 문제에 응답할 수 있습니다. 

스크립트를 실행하는 동안 앱이 응답하지 않는 것처럼 보일 수도 있습니다. [LongRunningScriptDetected](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.longrunningscriptdetected.aspx) 이벤트는 웹 보기에서 JavaScript를 실행하는 동안 정기적으로 발생하며 스크립트를 중단할 수 있는 기회를 제공합니다. 스크립트가 실행된 기간을 확인하려면 [WebViewLongRunningScriptDetectedEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime.aspx)의 [ExecutionTime](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.aspx) 속성을 확인합니다. 스크립트를 중지하려면 이벤트 인수 [StopPageScriptExecution](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution.aspx) 속성을 **true**로 설정합니다. 후속 웹 보기 탐색 중에 다시 로드하지 않을 경우 중지된 스크립트는 다시 실행되지 않습니다. 

웹 보기 컨트롤은 임의 파일 형식을 호스트할 수 없습니다. 웹 보기에서 호스트할 수 없는 콘텐츠를 로드하려고 하면 [UnviewableContentIdentified](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unviewablecontentidentified.aspx) 이벤트가 발생합니다. 이 이벤트를 처리하고 사용자에게 알리거나, [Launcher](https://msdn.microsoft.com/library/windows/apps/xaml/windows.system.launcher.aspx) 클래스를 사용하여 파일을 외부 브라우저 또는 다른 앱으로 리디렉션할 수 있습니다.

마찬가지로, [UnsupportedUriSchemeIdentified](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsupportedurischemeidentified.aspx) 이벤트는 웹 콘텐츠에서 지원되지 않는 URI 체계를 호출하는 경우(예: fbconnect:// 또는 mailto://)에 발생합니다. 기본 시스템 시작 관리자가 URI를 시작하도록 허용하는 대신 이 이벤트를 처리하여 사용자 지정 동작을 제공할 수 있습니다.

[UnsafeContentWarningDisplayingevent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying.aspx)는 SmartScreen 필터에서 안전하지 않은 것으로 보고된 콘텐츠에 대한 경고 페이지가 웹 보기에 표시되는 경우에 발생합니다. 사용자가 계속 탐색하도록 선택할 경우 이후에 페이지로 이동할 때는 경고가 표시되지 않으며 이벤트도 발생하지 않습니다.

### <a name="handling-special-cases-for-web-view-content"></a>웹 보기 콘텐츠에 대한 특수한 경우 처리

[ContainsFullScreenElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelement.aspx) 속성 및 [ContainsFullScreenElementChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelementchanged.aspx) 이벤트를 사용하여 웹 콘텐츠에서 전체 화면 비디오 재생과 같은 전체 화면 환경을 검색, 응답 및 사용하도록 설정할 수 있습니다. 예를 들어 ContainsFullScreenElementChanged 이벤트를 사용하여 앱 보기 전체를 차지하도록 웹 보기의 크기를 조정하거나, 다음 예제와 같이 전체 화면 웹 환경이 필요한 경우 창 앱을 전체 화면 모드로 전환할 수 있습니다.

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

[NewWindowRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.newwindowrequested.aspx) 이벤트를 사용하여 호스트된 웹 콘텐츠가 팝업 창과 같은 새 창을 표시하도록 요청하는 경우를 처리할 수 있습니다. 다른 WebView 컨트롤을 사용하여 요청된 창의 내용을 표시할 수 있습니다.

특수 기능이 필요한 웹 기능을 사용하도록 설정하려면 [PermissionRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx) 이벤트를 사용합니다. 현재 이러한 기능에는 지리적 위치, IndexedDB 저장소, 사용자 오디오 및 동영상(예: 마이크 또는 webcam에서 제공)이 포함됩니다. 앱이 사용자 위치나 사용자 미디어에 액세스하는 경우에도 앱 매니페스트에서 이 기능을 선언해야 합니다. 예를 들어, 지리적 위치를 사용하는 앱은 Package.appxmanifest에 최소한 다음과 같은 기능 선언이 있어야 합니다.

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

앱에서 [PermissionRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx) 이벤트를 처리하는 것 외에도 사용자는 이러한 기능을 사용할 수 있도록 위치 또는 미디어 기능을 요청하는 앱에 대해 표준 시스템 대화 상자를 승인해야 합니다.

다음은 앱이 Bing 지도에서 지리적 위치를 사용하도록 설정하는 방법의 예입니다.

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

앱이 사용 권한 요청에 응답하기 위해 사용자 입력이나 다른 비동기 작업이 필요한 경우 [WebViewPermissionRequest](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx)의 [Defer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.aspx) 메서드를 사용하여 나중에 작업할 수 있는 [WebViewDeferredPermissionRequest](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewdeferredpermissionrequest.aspx)를 만듭니다. [WebViewPermissionRequest.Defer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx)를 참조하세요. 

사용자가 웹 보기에 호스트된 웹 사이트에서 안전하게 로그아웃해야 하는 경우 또는 보안이 중요한 다른 경우에는 정적 메서드 [ClearTemporaryWebDataAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cleartemporarywebdataasync.aspx)를 호출하여 웹 보기 세션에서 로컬에 캐시된 콘텐츠를 지웁니다. 이렇게 하면 악의적인 사용자가 중요한 데이터에 액세스할 수 없습니다. 

### <a name="interacting-with-web-view-content"></a>웹 보기 콘텐츠 조작

[InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) 메서드로 웹 보기의 콘텐츠를 조작하여 스크립트를 호출하거나 웹 보기 콘텐츠에 삽입하고, [ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) 이벤트를 사용하여 웹 보기 콘텐츠에서 다시 정보를 가져올 수 있습니다.

웹 보기 콘텐츠 내의 JavaScript를 호출하려면 [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) 메서드를 사용합니다. 호출된 스크립트는 문자열 값만 반환할 수 있습니다. 

예를 들어 이름이 `webView1`인 웹 보기의 콘텐츠에 이름이 `setDate`이고 매개 변수 3개를 사용하는 함수가 포함되어 있으면 다음과 같이 호출할 수 있습니다. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


JavaScript **eval** 함수와 함께 **InvokeScriptAsync**를 사용하여 웹 페이지에 콘텐츠를 삽입할 수 있습니다.

여기서 XAML 입력란의 텍스트(`nameTextBox.Text`)는 `webView1`에 호스트된 HTML 페이지의 div에 기록됩니다. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

웹 보기 콘텐츠의 스크립트는 문자열 매개 변수와 함께 **window.external.notify**를 사용하여 정보를 다시 앱으로 보낼 수 있습니다. 이러한 메시지를 받으려면 [ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) 이벤트를 처리합니다. 

외부 웹 페이지에서 window.external.notify를 호출할 때 **ScriptNotify** 이벤트가 발생하게 하려면 페이지의 URI를 앱 매니페스트의 **ApplicationContentUriRules** 섹션에 포함해야 합니다. 이 작업은 Microsoft Visual Studio의 Package.appxmanifest 디자이너, 콘텐츠 URI 탭에서 수행할 수 있습니다. 이 목록의 URI는 HTTPS를 사용해야 하며 하위 도메인 와일드카드(예: `https://*.microsoft.com`)를 포함할 수 있지만 도메인 와일드카드(예: `https://*.com` 및 `https://*.*`)는 포함할 수 없습니다. 매니페스트 요구 사항은 앱 패키지에서 시작되거나 ms-local-stream:// URI를 사용하거나 [NavigateToString](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetostring.aspx)을 통해 로드되는 콘텐츠에 적용되지 않습니다. 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>웹 보기에서 Windows 런타임 액세스

[AddWebAllowedObject](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx) 메서드를 사용하여 Windows 런타임 구성 요소의 네이티브 클래스 인스턴스를 웹 보기의 JavaScript 컨텍스트에 삽입할 수 있습니다. 이렇게 하면 해당 웹 보기의 JavaScript 콘텐츠에서 해당 개체의 네이티브 메서드, 속성 및 이벤트에 모두 액세스할 수 있습니다. 클래스는 [AllowForWeb](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.metadata.allowforwebattribute.aspx) 특성으로 데코레이트해야 합니다. 

예를 들어, 이 코드는 Windows 런타임 구성 요소에서 가져온 `MyClass` 인스턴스를 웹 보기에 삽입합니다.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

자세한 내용은 [WebView.AddWebAllowedObject](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx)를 참조하세요. 

또한 웹 보기에서 신뢰할 수 있는 JavaScript 콘텐츠는 Windows 런타임 API에 직접 액세스할 수 있습니다. 이 때문에 웹 보기에 호스트된 웹앱에 대한 강력한 기본 기능이 제공됩니다. 이 기능을 사용하려면 신뢰할 수 있는 콘텐츠의 URI가 WindowsRuntimeAccess를 구체적으로 "all"로 설정하여 Package.appxmanifest에 있는 앱의 ApplicationContentUriRules에서 허용 목록에 포함되어야 합니다. 

이 예제에서는 앱 매니페스트의 한 섹션을 보여 줍니다. 여기서는 로컬 URI에 Windows 런타임에 대한 액세스 권한이 부여됩니다. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>웹 콘텐츠 호스팅 옵션

[WebViewSettings](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.settings.aspx) 형식의 [WebView.Settings](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewsettings.aspx) 속성을 사용하여 JavaScript 및 IndexedDB의 사용 여부를 제어할 수 있습니다. 예를 들어, 웹 보기를 사용하여 엄격한 정적 콘텐츠를 표시하는 경우 최상의 성능을 위해 JavaScript를 사용하지 않도록 설정하는 것이 좋습니다.

### <a name="capturing-web-view-content"></a>웹 보기 콘텐츠 캡처

다른 앱과 웹 보기 콘텐츠를 공유할 수 있게 하려면 선택한 콘텐츠를 [DataPackage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync.aspx)로 반환하는 [CaptureSelectedContentToDataPackageAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datapackage.aspx) 메서드를 사용합니다. 이 메서드는 비동기식으로 작동하므로 지연을 사용하여 비동기 호출이 완료되기 전에 [DataRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx) 이벤트 처리기가 반환되지 않도록 해야 합니다. 

웹 보기의 현재 콘텐츠 미리 보기 이미지를 가져오려면 [CapturePreviewToStreamAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.capturepreviewtostreamasync.aspx) 메서드를 사용합니다. 이 메서드는 현재 콘텐츠의 이미지를 만들고 지정된 스트림에 씁니다. 

### <a name="threading-behavior"></a>스레딩 동작

기본적으로 웹 보기 콘텐츠는 데스크톱 디바이스 패밀리의 디바이스에 있는 UI 스레드와 다른 모든 디바이스의 UI 스레드 외부에 호스트됩니다. [WebView.DefaultExecutionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultexecutionmode.aspx) 정적 속성을 사용하여 현재 클라이언트의 기본 스레딩 동작을 쿼리할 수 있습니다. 필요한 경우 [WebView(WebViewExecutionMode)](https://msdn.microsoft.com/library/windows/apps/xaml/dn932036.aspx) 생성자를 사용하여 이 동작을 재정의할 수 있습니다. 

> **참고**&nbsp;&nbsp;모바일 디바이스의 UI 스레드에서 콘텐츠를 호스트할 때 성능 문제가 발생할 수도 있으므로 DefaultExecutionMode를 변경하는 경우 모든 대상 디바이스에서 테스트해야 합니다.

UI 스레드 외부에서 콘텐츠를 호스트하는 웹 보기는 웹 보기 컨트롤에서 부모로 제스처를 전파해야 하는 부모 컨트롤(예: [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx), [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) 및 기타 관련 컨트롤)과 호환되지 않습니다. 이러한 컨트롤은 스레드 외부 웹 보기에서 시작된 제스처를 받을 수 없습니다. 또한 스레드 외부 웹 콘텐츠 인쇄는 직접 지원되지 않습니다. 대신 [WebViewBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.aspx) 채우기를 사용하여 요소를 인쇄해야 합니다.

## <a name="recommendations"></a>권장 사항


-   로드된 웹 사이트의 형식이 장치에 맞게 지정되고 앱의 나머지 부분과 일관된 색, 입력 체계, 탐색이 사용되는지 확인합니다.
-   입력 필드는 크기가 적절해야 합니다. 사용자는 텍스트를 입력하기 위해 확대할 수 있다는 점을 알지 못할 수 있습니다.
-   웹 보기가 앱의 나머지 부분과 다르게 보일 경우 대체 컨트롤이나 방법을 사용하여 관련 작업을 수행하는 것이 좋습니다. 웹 보기가 앱의 나머지 부분과 일치하는 경우 사용자는 웹 보기를 하나의 완벽한 환경으로 봅니다.



## <a name="related-topics"></a>관련 항목

* [WebView 클래스](https://msdn.microsoft.com/library/windows/apps/br227702)
 

 




