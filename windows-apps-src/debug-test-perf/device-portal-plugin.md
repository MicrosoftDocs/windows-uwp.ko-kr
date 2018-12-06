---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Device Portal에 대한 사용자 지정 플러그 인 작성
description: Windows Device Portal을 사용하여 웹 페이지를 호스팅하고 진단 정보를 제공하는 UWP 앱을 작성하는 방법을 알아보세요.
ms.date: 03/24/2017
ms.topic: article
keywords: windows 10, uwp, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: d9e11445d77434320c8842608bf8183a078c0660
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8744661"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Device Portal에 대한 사용자 지정 플러그 인 작성

Windows Device Portal을 사용하여 웹 페이지를 호스팅하고 진단 정보를 제공하는 UWP 앱을 작성하는 방법을 알아보세요.

크리에이터 업데이트부터 Device Portal을 사용하여 앱의 진단 인터페이스를 호스팅할 수 있습니다. 이 문서에서는 appxmanifest 변경, Device Portal 서비스에 대한 앱의 연결을 설정, 수신 요청 처리 등 앱에 대한 DevicePortalProvider를 만들기 위한 세 가지 요소에 대해 설명합니다. 시작하기 위한 샘플 앱도 제공됩니다(서비스 예정). 

## <a name="create-a-new-uwp-app-project"></a>새 UWP앱 프로젝트 만들기
이 가이드에서는 편의상 모두를 한 솔루션으로 만듭니다.

Microsoft Visual Studio 2017에서 UWP 앱 프로젝트를 만듭니다. '파일 > 새 프로젝트 및 템플릿 선택 > Visual C# > Windows Universal > 비어 있는 앱(Windows Universal)'으로 이동합니다. "DevicePortalProvider"로 이름을 지정합니다. 이 앱이 앱 서비스를 포함하는 앱이 됩니다. 지원을 위해 크리에이터 업데이트 SDK를 선택했는지 확인합니다.  Visual Studio를 업데이트하거나 새 SDK를 설치해야 하는 경우 자세한 내용은 [여기](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/)를 참조하세요. 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>package.appxmanifest 파일에 devicePortalProvider 확장 추가
앱을 Device Portal 플러그 인으로 작동하게 하기 위해 *package.appxmanifest* 파일에 일부 코드를 추가해야 합니다. 먼저 파일의 위쪽에 다음 네임스페이스 정의를 추가합니다. `IgnorableNamespaces` 특성에도 추가합니다.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

앱을 Device Portal 공급자로 선언하기 위해 앱 서비스 및 이를 사용하는 새 Device Portal 공급자 확장을 생성해야 합니다. `Application` 아래의 `Extensions` 요소에 windows.appService 확장 및 windows.devicePortalProvider 확장을 추가합니다. 각 확장과 `AppServiceName` 특성이 일치하는지 확인합니다. 이는 처리기 네임스페이스의 요청을 처리하기 위해 이 앱 서비스가 시작할 수 있는 Device Portal 서비스에 나타납니다. 

```xml
...   
<Application 
    Id="App" 
    Executable="$targetnametoken$.exe"
    EntryPoint="DevicePortalProvider.App">
    ...
    <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MySampleProvider.SampleProvider">
            <uap:AppService Name="com.sampleProvider.wdp" />
        </uap:Extension>
        <uap4:Extension Category="windows.devicePortalProvider">
            <uap4:DevicePortalProvider 
                DisplayName="My Device Portal Provider Sample App" 
                AppServiceName="com.sampleProvider.wdp" 
                HandlerRoute="/MyNamespace/api/" />
        </uap4:Extension>
    </Extensions>
</Application>
...
```

`HandlerRoute` 특성은 앱에서 요청된 REST 네임스페이스를 참조합니다. Device Portal 서비스에서 받은 해당 네임스페이스(와일드카드가 암시적으로 다음에 옴)의 모든 HTTP 요청은 처리하기 위해 앱에 전송됩니다. 이 경우 `<ip_address>/MyNamespace/api/*`에 대해 성공적으로 인증된 모든 HTTP 요청이 앱으로 전송됩니다. 처리기 경로 간 충돌은 "가장 긴 성공" 검사 - 요청의 선택된 경로 일치가 어떤 것인지를 통해 해결됩니다. 즉, "/MyNamespace/api/foo"에 대한 요청은 "/MyNamespace"가 아닌 "/MyNamespace/api"의 공급자와 일치합니다.  

이 기능을 위해 두 가지 새로운 기능이 필요합니다. 이 또한 *package.appxmanifest* 파일에 추가해야 합니다.

```xml
...
<Capabilities>
    ...
    <Capability Name="privateNetworkClientServer" />
    <rescap:Capability Name="devicePortalProvider" />
</Capabilities>
...
```

> [!NOTE]
> "devicePortalProvider" 기능은 제한되어 있습니다("rescap"). 즉, 앱을 게시하기 전에 스토어에서 사전 승인을 받아야 합니다. 그러나 테스트용 로드를 통해 로컬에서 앱을 테스트할 수는 있습니다. 제한된 접근 권한 값에 대한 자세한 내용은 [앱 접근 권한 값 선언](https://msdn.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities)을 참조하세요.

## <a name="set-up-your-background-task-and-winrt-component"></a>백그라운드 작업 및 WinRT 구성 요소 설정
Device Portal 연결을 설정하려면 앱이 앱 내에서 실행되는 Device Portal 인스턴스로 Device Portal 서비스의 앱 서비스 연결을 연결해야 합니다. 이를 위해 [**IBackgroundTask**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtask)를 구현하는 클래스로 새로운 WinRT 구성 요소를 응용 프로그램에 추가합니다.

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

해당 이름이 앱 서비스 진입점("MySampleProvider.SampleProvider")에서 설정한 네임스페이스 및 클래스 이름과 일치하는지 확인합니다. Device Portal 공급자에 첫 번째로 요청하면 Device Portal은 요청을 숨기고, 앱의 백그라운드 작업을 시작하고, 이를 **실행** 메서드로 명명하고, [**IBackgroundTaskInstance**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance)에 전달합니다. 그러면 앱은 이를 사용하여 [**DevicePortalConnection**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection) 인스턴스를 설정합니다.

```csharp
// Implement background task handler with a DevicePortalConnection
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take a deferral to allow the background task to continue executing 
    this.taskDeferral = taskInstance.GetDeferral();
    taskInstance.Canceled += TaskInstance_Canceled;

    // Create a DevicePortal client from an AppServiceConnection 
    var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    var appServiceConnection = details.AppServiceConnection;
    this.devicePortalConnection = DevicePortalConnection.GetForAppServiceConnection(appServiceConnection);

    // Add Closed, RequestReceived handlers 
    devicePortalConnection.Closed += DevicePortalConnection_Closed;
    devicePortalConnection.RequestReceived += DevicePortalConnection_RequestReceived;
}
```

요청 처리 루프를 완료 하기 위해 앱에서 처리 해야 하는 두 가지 이벤트가: **Closed**, 때마다에 대 한 Device Portal 서비스가 종료 되 고 [**RequestReceived**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), 들어오는 HTTP을 표시 하는 요청 하 고 기본 제공 Device Portal 공급자의 기능입니다. 

## <a name="handle-the-requestreceived-event"></a>RequestReceived 이벤트 처리
플러그 인의 지정된 처리기 경로에 요청된 모든 HTTP 요청에 대해 **RequestReceived** 이벤트가 한 번 발생합니다. Device Portal 공급자가에 대한 요청 처리 루프는 NodeJS Express의 요청 처리 루프와 비슷합니다. 요청 및 응답 개체는 이벤트에 함께 제공되고 처리기는 응답 개체를 작성하여 응답합니다. Device Portal 공급자에서 **RequestReceived** 이벤트 및 처리기는 [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httprequestmessage) 및 [**HttpResponseMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpresponsemessage) 개체를 사용합니다.   

```csharp
// Sample RequestReceived echo handler: respond with an HTML page including the query and some additional process information. 
private void DevicePortalConnection_RequestReceived(DevicePortalConnection sender, DevicePortalConnectionRequestReceivedEventArgs args)
{
    var req = args.RequestMessage;
    var res = args.ResponseMessage;

    if (req.RequestUri.AbsolutePath.EndsWith("/echo"))
    {
        // construct an html response message
        string con = "<h1>" + req.RequestUri.AbsoluteUri + "</h1><br/>";
        var proc = Windows.System.Diagnostics.ProcessDiagnosticInfo.GetForCurrentProcess();
        con += String.Format("This process is consuming {0} bytes (Working Set)<br/>", proc.MemoryUsage.GetReport().WorkingSetSizeInBytes);
        con += String.Format("The process PID is {0}<br/>", proc.ProcessId);
        con += String.Format("The executable filename is {0}", proc.ExecutableFileName);
        res.Content = new HttpStringContent(con);
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
        res.StatusCode = HttpStatusCode.Ok;            
    }
    //...
}
```

이 샘플 요청 처리기에서는 먼저 *인수* 매개 변수에서 요청 및 응답 개체를 끌어온 다음 요청 URL과 몇 가지 추가 HTML 형식으로 문자열을 만듭니다. 이는 [**HttpStringContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpstringcontent) 인스턴스로 응답 개체에 추가됩니다. "문자열"과 "버퍼" 등에 대한 다른 [**IHttpContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.ihttpcontent) 클래스 또한 허용됩니다.

그러면 응답이 HTTP 응답으로 설정되며 200(확인) 상태 코드가 부여됩니다. 원래 호출한 브라우저에서 예상대로 렌더링되어야 합니다. **RequestReceived** 이벤트 처리기가 반환될 때 응답 메시지는 자동으로 사용자 에이전트에 반환됩니다. 추가 "send" 메서드가 필요 없습니다.

![장치 포털 응답 메시지](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>정적 콘텐츠 공급
공급자에 매우 쉽게 UI를 추가할 수 있도록 하여 정적 콘텐츠를 패키지 내의 폴더에서 직접 제공할 수 있습니다.  정적 콘텐츠를 제공하는 가장 쉬운 방법은 URL로 매핑할 수 있는 프로젝트에 콘텐츠 폴더를 만드는 것입니다.

![Device Portal 정적 콘텐츠 폴더](images/device-portal/plugin-static-content.png)
 
그 다음 정적 콘텐츠 경로를 감지하고 요청을 적절히 매핑하는 **RequestReceived** 이벤트 처리기에 경로 처리기를 추가합니다.  

```csharp
if (req.RequestUri.LocalPath.ToLower().Contains("/www/")) {
    var filePath = req.RequestUri.AbsolutePath.Replace('/', '\\').ToLower();
    filePath = filePath.Replace("\\backgroundprovider", "")
    try {
        var fileStream = Windows.ApplicationModel.Package.Current.InstalledLocation.OpenStreamForReadAsync(filePath).GetAwaiter().GetResult();
        res.StatusCode = HttpStatusCode.Ok;
        res.Content = new HttpStreamContent(fileStream.AsInputStream());
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    } catch(FileNotFoundException e) {
        string con = String.Format("<h1>{0} - not found</h1>\r\n", filePath);
        con += "Exception: " + e.ToString();
        res.Content = new HttpStringContent(con);
        res.StatusCode = HttpStatusCode.NotFound;
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    }
}
```
콘텐츠 폴더에서 모든 파일이 "콘텐츠"로 표시되고 Visual Studio의 속성 메뉴에서 "새 버전이면 복사" 또는 "항상 복사"로 설정되어 있는지 확인 합니다.  이렇게 하면 배포할 때 파일이 AppX 패키지 내에 위치하게 됩니다.  

![정적 콘텐츠 파일 복사 구성](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>기존 Device Portal 리소스및 API 사용
Device Portal 공급자에서 제공되는 정적 콘텐츠는 코어 Device Portal 서비스와 동일한 포트에서 제공됩니다.  즉, Device Portal에 포함된 기존 JS 및 CSS를 HTML의 `<link>` 및 `<script>` 태그에서 참조할 수 있습니다. 일반적으로 편리한 webbRest 개체에서 모든 핵심 Device Portal REST API를 래핑하는 *rest.js* 및 Device Portal UI의 나머지 부분에 맞게 콘텐츠의 스타일을 조정할 수 있는 *common.css* 파일을 사용하는 것이 좋습니다. 이러한 작업의 예는 이 샘플에 포함된 *index.html* 페이지에서 볼 수 있습니다. *rest.js*를 사용하여 Device Portal 포털에서 디바이스 이름 및 실행 프로세스를 검색합니다. 

![장치 포털 플러그 인 출력](images/device-portal/plugin-output.png)
 
중요한 것은 webbRest에서 HttpPost/DeleteExpect200 메서드를 사용하면 자동으로 웹 페이지에서 상태 변경 REST API를 호출할 수 있도록 하는 [CSRF 처리](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks)가 수행됩니다.  

> [!NOTE] 
> Device Portal에 포함된 정적 콘텐츠의 주요 변경 사항은 보증되지 않습니다. API는 자주 변경하지 않는 것이 좋습니다. 특히 공급자는 *common.js* 및 *controls.js* 파일을 사용하지 않아야 합니다. 

## <a name="debugging-the-device-portal-connection"></a>Device Portal 연결 디버깅
백그라운드 작업을 디버깅하기 위해 Visual Studio에서 코드를 실행하는 방법을 변경해야 합니다. 공급자가 HTTP 요청을 처리하는 방법을 검사하기 위해 앱 서비스 연결을 디버깅하려면 아래 단계를 따르세요.

1.  디버그 메뉴에서 DevicePortalProvider 속성을 선택합니다. 
2.  디버깅 탭 아래에서 시작 작업 섹션에서 "시작하지 않음(시작 시 코드 디버그)"을 선택합니다.  
![플러그 인 디버그 모드로 전환](images/device-portal/plugin-debug-mode.png)
3.  RequestReceived 처리기 함수에서 중단점을 설정합니다.
![requestreceived 처리기의 중단점](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> 빌드 아키텍처가 대상의 아키텍처와 정확하게 일치하는지 확인합니다. 64비트 PC를 사용하는 경우 AMD64 빌드를 사용하여 배포해야 합니다. 
4.  F5 키를 눌러 앱을 배포합니다.
5.  Device Portal을 끈 다음 다시 켜 앱을 찾습니다.(앱 매니페스트 변경 시에만 필요. 그 이외에는 다시 배포하고 이 단계를 건너뛸 수 있습니다.) 
6.  브라우저에서 공급자의 네임스페이스에 액세스하면 중단점이 발생할 것입니다.

## <a name="related-topics"></a>관련 항목
* [Windows Device Portal 개요](device-portal.md)
* [앱 서비스 만들기 및 사용](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


