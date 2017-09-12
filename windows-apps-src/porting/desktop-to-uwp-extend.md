---
author: normesta
Description: "Windows UI와 구성 요소로 데스크톱 응용 프로그램 확장"
Search.Product: eADQiWindows 10XVcnh
title: "Windows UI와 구성 요소로 데스크톱 응용 프로그램 확장"
ms.author: normesta
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: fce6076f07957b50e83cf80e8d12350630b99456
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2017
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장

일부 Windows 10 환경(예, 터치 구현 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 이러한 환경을 추가하려는 경우, UWP 구성 요소로 데스크톱 응용 프로그램을 확장합니다.

많은 경우 데스크톱 응용 프로그램에서 직접 UWP API를 호출할 수 있기 때문에 이 가이드를 검토하기 전에 [Windows 10 향상](desktop-to-uwp-enhance.md)을 참조하세요.

>[!NOTE]
>이 가이드는 데스크톱 브리지를 사용하여 데스크톱 응용 프로그램용 Windows 앱 패키지를 만들었다고 가정합니다. 아직 완료하지 않았다면 [데스크톱 브리지](desktop-to-uwp-root.md)를 참조하세요.

준비가 되었으면 시작하겠습니다.

## <a name="show-a-modern-xaml-ui"></a>최신 XAML UI 표시

응용 프로그램 흐름의 일부로 데스크톱 응용 프로그램에 최신 XAML 기반 사용자 인터페이스를 포함시킬 수 있습니다. 이러한 사용자 인터페이스는 자연스럽게 화면 크기와 해상도에 맞춰 조정이 되고, 터치와 잉크 같은 최신 대화형 모델을 지원합니다.

예를 들어, 조금의 XAML 태그로 사용자에게 강력한 지도 관련 시각화 기능을 제공할 수 있습니다.

이 이미지는 지도 컨트롤이 포함된 XAML 기반 최신 UI이 이미지를 XAML 기반 최신 UI가 열린 VB6 응용 프로그램입니다.

![적응형 디자인](images\desktop-to-uwp\extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>이 앱을 자세히 보세요.

:heavy_check_mark: [비디오 시청](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Add-a-XAML-UI-and-Toast-Notification-to-a-VB6-Application-OsJHC7WhD_8006218965)

:heavy_check_mark: [앱 다운로드](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [코드 찾아보기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>디자인 패턴

XAML 기반 UI를 표시하려면 다음 작업을 수행합니다.

1. [솔루션에 UWP 프로젝트 추가](#project)

2. [프로젝트에 프로토콜 확장 추가](#protocol)

3. [데스크톱 앱에서 UWP 앱 시작](#start)

4. [UWP 프로젝트에서 원하는 페이지 표시](#parse)

<span id="project" />
### <a name="add-a-uwp-project"></a>UWP 프로젝트 추가

솔루션에 **빈 앱(Universal Windows)** 프로젝트 추가

<span id="protocol" />
### <a name="add-a-protocol-extension"></a>프로토콜 확장 추가

**솔루션 탐색기**에서 프로젝트의 **package.appxmanifest** 파일을 열고, 확장을 추가합니다.

```xml
<Extensions>
      <uap:Extension
          Category="windows.protocol"
          Executable="MapUI.exe"
          EntryPoint=" MapUI.App">
        <uap:Protocol Name="desktopbridgemapsample" />
      </uap:Extension>
    </Extensions>     
```

프로토콜 이름을 지정하고, UWP 프로젝트가 생성한 실행 파일 이름과 진입점 클래스 이름을 제공합니다.

또 디자이너에서 **package.appxmanifest**를 열고 **선언** 탭을 선택한 다음 확장을 추가합니다.

![선언-탭](images\desktop-to-uwp\protocol-properties.png)



> [!NOTE]
> 지도 컨트롤은 인터넷에서 데이터를 다운로드 합니다. 지도 컨트롤을 사용하고 있다면, 인터넷 클라이언트 기능을 매니페스트에 추가해야 합니다.

<span id="start" />
### <a name="start-the-uwp-app"></a>UWP 앱 시작

먼저 UWP 앱으로 보내고 싶은 파라미터와 프로토콜 입력이 포함된 [Uri](https://msdn.microsoft.com/library/system.uri.aspx) 를 생성합니다. 그런 후 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 메서드를 호출합니다.

다음은C#의 기본 예제입니다.

```csharp

private async void showMap(double lat, double lon)
{
    string str = "desktopbridgemapsample://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

    if (success)
    {
        // URI launched
    }
    else
    {
        // URI launch failed
    }
}
```
샘플에서 조금 더 간접적인 작업을 합니다. VB6 호출 Interop 기능 이름 ``LaunchMap``에서 호출을 래핑합니다. C++를 사용하여 기능을 작성합니다.

VB 블록은 다음과 같습니다.

```VB
Private Declare Function LaunchMap Lib "UWPWrappers.dll" _
  (ByVal lat As Double, ByVal lon As Double) As Boolean
 
Private Sub EiffelTower_Click()
    LaunchMap 48.858222, 2.2945
End Sub
```

C++ 기능은 다음과 같습니다.

```C++

DllExport bool __stdcall LaunchMap(double lat, double lon)
{
  try
  {
    String ^str = ref new String(L"desktopbridgemapsample://");
    Uri ^uri = ref new Uri(
      str + L"location?lat=" + lat.ToString() + L"&?lon=" + lon.ToString());
 
    // now launch the UWP component
    Launcher::LaunchUriAsync(uri);
  }
  catch (Exception^ ex) { return false; }
  return true;
}

```

<span id="parse" />
### <a name="parse-parameters-and-show-a-page"></a>매개 변수 분석 및 페이지 표시

UWP 프로젝트의 **App** 클래스에서 **OnActivated** 이벤트 핸들러를 재정의합니다. 프로토콜이 앱을 활성화 한 경우, 매개변수를 구문 분석하고 원하는 페이지를 엽니다.

```C++
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ e)
{
  if (e->Kind == ActivationKind::Protocol)
  {
    ProtocolActivatedEventArgs^ protocolArgs = (ProtocolActivatedEventArgs^)e;
    Uri ^uri = protocolArgs->Uri;
    if (uri->SchemeName == "desktopbridgemapsample")
    {
      Frame ^rootFrame = ref new Frame();
      Window::Current->Content = rootFrame;
      rootFrame->Navigate(TypeName(MainPage::typeid), uri->Query);
      Window::Current->Activate();
    }
  }
}
```

### <a name="similar-samples"></a>비슷한 샘플

[Northwind 샘플: UWA UI 및 Win32 기존 코드에 대한 완전한 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Northwind 샘플: SQL Server에 연결된 UWP 앱](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>다른 앱에 서비스 제공

다른 앱이 사용할 수 있는 서비스를 추가합니다. 예를 들어, 다른 앱에 앱 이면의 데이터베이스에 제어된 액세스를 제공하는 서비스를 추가할 수 있습니다. 백그라운드 작업을 구현, 앱은 데스크톱 앱이 실행되지 않는 경우에도 서비스에 도달할 수 있습니다.

이에 대한 샘플입니다.

![적응형 디자인](images\desktop-to-uwp\winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>이 앱을 자세히 보세요.

:heavy_check_mark: [비디오 시청](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Expose-an-AppService-from-a-Windows-Forms-Data-Application-GiqNS7WhD_706218965)

:heavy_check_mark: [앱 다운로드](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [코드 찾아보기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>디자인 패턴

서비스 제공을 표시하려면 다음 작업을 수행합니다.

1. [Windows 런타임 구성 요소 추가](#component)

2. [앱 서비스 확장 추가](#extension)

3. [앱 서비스 구현](#appservice)

4. [앱 서비스 테스트](#test)

<span id="component" />

### <a name="add-a-windows-runtime-component"></a>Windows 런타임 구성 요소 추가

솔루션에 **Windows 런타임 구성 요소(유니버설 Windows)** 프로젝트를 추가합니다.

그런 다음 UWP 패키징 프로젝트에서 해당 런타임 구성 요소의 프로젝트를 참조합니다.

<span id="extension" />
### <a name="add-an-app-service-extension"></a>앱 서비스 확장 추가

**솔루션 탐색기**에서 패키징 프로젝트의 **package.appxmanifest** 파일을 열고, 앱 서비스 확장을 추가합니다.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="MyAppService.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```

앱 서비스에 이름을 지정하고, 진입점 클래스 이름을 지정합니다. 이는 앱 서비스 구현에 사용할 클래스입니다.

<span id="appservice" />
### <a name="implement-the-app-service"></a>앱 서비스 구현

여기에서 다른 앱의 요청을 검사 및 처리하게 될 것입니다.

```csharp
public sealed class AppServiceTask : IBackgroundTask
{
    private BackgroundTaskDeferral backgroundTaskDeferral;
 
    public void Run(IBackgroundTaskInstance taskInstance)
    {
        this.backgroundTaskDeferral = taskInstance.GetDeferral();
        taskInstance.Canceled += OnTaskCanceled;
        var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        details.AppServiceConnection.RequestReceived += OnRequestReceived;
    }
 
    private async void OnRequestReceived(AppServiceConnection sender,
                                         AppServiceRequestReceivedEventArgs args)
    {
        var messageDeferral = args.GetDeferral();
        ValueSet message = args.Request.Message;
        string id = message["ID"] as string;
        ValueSet returnData = DataBase.GetData(id);
        await args.Request.SendResponseAsync(returnData);
        messageDeferral.Complete();
    }
 
 
    private void OnTaskCanceled(IBackgroundTaskInstance sender,
                                BackgroundTaskCancellationReason reason)
    {
        if (this.backgroundTaskDeferral != null)
        {
            this.backgroundTaskDeferral.Complete();
        }
    }
}
```

<span id="test" />
### <a name="test-the-app-service"></a>앱 서비스 테스트

다른 앱에서 호출하여 서비스를 테스트 합니다.

```csharp
private async void button_Click(object sender, RoutedEventArgs e)
{
    AppServiceConnection dataService = new AppServiceConnection();
    dataService.AppServiceName = "com.microsoft.samples.winforms";
    dataService.PackageFamilyName = "Microsoft.SDKSamples.WinformWithAppService";
 
    var status = await dataService.OpenAsync();
    if (status == AppServiceConnectionStatus.Success)
    {
        string id = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("ID", id);
        AppServiceResponse response = await dataService.SendMessageAsync(message);
        string result = "";
 
        if (response.Status == AppServiceResponseStatus.Success)
        {
            if (response.Message["Status"] as string == "OK")
            {
                DisplayResult(response.Message["Result"]);
            }
        }
    }
}
```

앱 서비스에 대한 자세한 내용은 여기에서 참조하세요([앱 서비스 만들어 사용하기](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)).

### <a name="similar-samples"></a>비슷한 샘플

[앱 서비스 브리지 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[C++win32 앱과 앱 서비스 브리지](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[푸시 알림을 받는 MFC 응용 프로그램](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>데스크톱 응용 프로그램을 공유 대상으로 만들기

데스크톱 응용 프로그램을 공유 대상으로 만들면 사용자가 공유를 지원하는 다른 앱의 사진 등 데이터를 쉽게 공유할 수 있습니다.

예를 들어, 사용자가 여러분의 앱을 선택해 Microsoft Edge, 사진 앱의 사진을 공유할 수 있습니다. 다음은 이 기능을 가지고 있는 WPF 샘플 앱입니다.

![공유 대상](images\desktop-to-uwp\share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>이 앱을 자세히 보세요.

:heavy_check_mark: [비디오 시청](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Make-a-WPF-Application-a-Share-Target-xd6Fu6WhD_8406218965)

:heavy_check_mark: [앱 다운로드](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [코드 찾아보기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>디자인 패턴

응용 프로그램을 공유 대상으로 만들려면 이런 작업을 수행합니다.

1. [솔루션에 UWP 프로젝트 추가](#project2)

2. [공유 대상 확장 추가](#share-extension)

3. [OnNavigatedTo 이벤트 핸들러 재정의](#override)

<span id="project2" />
### <a name="add-a-uwp-project-to-your-solution"></a>솔루션에 UWP 프로젝트 추가

솔루션에 **빈 앱(Universal Windows)** 프로젝트 추가

<span id="share-extension" />
### <a name="add-a-share-target-extension"></a>공유 대상 확장 추가

**솔루션 탐색기**에서 프로젝트의 **package.appxmanifest** 파일을 열고, 확장을 추가합니다.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="ShareTarget.App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

UWP 프로젝트가 생성한 실행 파일 이름과 진입점 클래스 이름을 제공합니다. 또 앱이 공유할 수 있는 파일 형식을 지정해야 합니다.

<span id="override" />
### <a name="override-the-onnavigatedto-event-handler"></a>OnNavigatedTo 이벤트 핸들러 재정의

UWP 프로젝트의 **App** 클래스에서 **OnNavigatedTo** 이벤트 핸들러를 재정의합니다.

이 이벤트 핸들러는 사용자가 파일을 공유하기 위해 앱을 선택할 때 호출됩니다.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
  this.shareOperation = (ShareOperation)e.Parameter;
  if (this.shareOperation.Data.Contains(StandardDataFormats.StorageItems))
  {
      this.sharedStorageItems =
        await this.shareOperation.Data.GetStorageItemsAsync();
       
      foreach (StorageFile item in this.sharedStorageItems)
      {
          ProcessSharedFile(item);
      }
  }
}
```

## <a name="support-and-feedback"></a>지원 및 피드백

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)을 참조하세요.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
