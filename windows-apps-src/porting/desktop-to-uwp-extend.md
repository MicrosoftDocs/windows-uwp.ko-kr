---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: Windows UI와 구성 요소로 데스크톱 응용 프로그램 확장
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f3354dad1702d275fb7b2af53516689d2c5d5014
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3421161"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장

일부 Windows 10 환경(예, 터치 구현 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 이러한 환경을 추가하려는 경우, UWP 프로젝트 및 Windows 런타임 구성 요소로 데스크톱 응용 프로그램을 확장합니다.

많은 경우 데스크톱 응용 프로그램에서 직접 UWP API를 호출할 수 있기 때문에 이 가이드를 검토하기 전에 [Windows 10 향상](desktop-to-uwp-enhance.md)을 참조하세요.

>[!NOTE]
>이 가이드는 데스크톱 브리지를 사용하여 데스크톱 응용 프로그램용 Windows 앱 패키지를 만들었다고 가정합니다. 아직 완료하지 않았다면 [데스크톱 브리지](desktop-to-uwp-root.md)를 참조하세요.

준비가 되었으면 시작하겠습니다.

<a id="setup" />

## <a name="first-setup-your-solution"></a>먼저 솔루션 설정

하나 또는 여러 개의 UWP 프로젝트와 런타임 구성 요소를 솔루션에 추가합니다.

데스크톱 응용 프로그램에 대한 참조가 있는 **Windows 응용 프로그램 패키징 프로젝트**를 포함하는 솔루션으로 시작합니다.

이 이미지는 예시 솔루션을 보여 줍니다.

![새 프로젝트 시작](images/desktop-to-uwp/extend-start-project.png)

솔루션에 패키징 프로젝트가 없는 경우 [Visual Studio를 사용하여 앱 패키징](desktop-to-uwp-packaging-dot-net.md)을 참조하세요.

### <a name="add-a-uwp-project"></a>UWP 프로젝트 추가

솔루션에 **빈 앱(Universal Windows)** 을 추가합니다.

이는 최신 XAML UI를 빌드하거나 UWP 프로세스 내에서만 실행되는 AP를 사용할 위치입니다.

![UWP 프로젝트](images/desktop-to-uwp/add-uwp-project-to-solution.png)

패키징 프로젝트에서 **응용 프로그램** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.

![UWP 프로젝트 참조](images/desktop-to-uwp/add-uwp-project-reference.png)

그 다음 UWP 프로젝트에 대한 참조를 추가합니다.

![UWP 프로젝트 참조](images/desktop-to-uwp/choose-uwp-project.png)

솔루션은 다음과 같이 보입니다.

![UWP 프로젝트가 있는 솔루션](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(선택 사항)Windows 런타임 구성 요소 추가

일부 시나리오를 수행하기 위해 Windows 런타임 구성 요소에 코드를 추가해야 할 수 있습니다.

![런타임 구성 요소 앱 서비스](images/desktop-to-uwp/add-runtime-component.png)

그런 다음 UWP 프로젝트에서 런타임 구성 요소에 참조를 추가합니다. 솔루션은 다음과 같이 보입니다.

![런타임 구성 요소 참조](images/desktop-to-uwp/runtime-component-reference.png)

UWP 프로젝트와 런타임 구성 요소를 사용하여 할 수 있는 몇 가지 사항에 살펴보겠습니다.

## <a name="show-a-modern-xaml-ui"></a>최신 XAML UI 표시

응용 프로그램 흐름의 일부로 데스크톱 응용 프로그램에 최신 XAML 기반 사용자 인터페이스를 포함시킬 수 있습니다. 이러한 사용자 인터페이스는 자연스럽게 화면 크기와 해상도에 맞춰 조정이 되고, 터치와 잉크 같은 최신 대화형 모델을 지원합니다.

예를 들어, 조금의 XAML 태그로 사용자에게 강력한 지도 관련 시각화 기능을 제공할 수 있습니다.

이 이미지는 지도 컨트롤이 포함된 XAML 기반 최신 UI를 여는 Windows Forms 응용 프로그램을 보여 줍니다.

![적응형 디자인](images/desktop-to-uwp/extend-xaml-ui.png)

### <a name="the-design-pattern"></a>디자인 패턴

XAML 기반 UI를 표시하려면 다음 작업을 수행합니다.

:1: [솔루션 설정](#solution-setup)

:2: [XAML UI 만들기](#xaml-UI)

:3: [UWP 프로젝트에 프로토콜 확장 추가](#protocol)

:4: [데스크톱 앱에서 UWP 앱 시작](#start)

:5: [UWP 프로젝트에서 원하는 페이지 표시](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>솔루션 설정

솔루션을 설정하는 방법에 대한 일반적인 지침은 이 가이드의 첫 부분에 있는 [먼저 솔루션 설치](#setup) 섹션을 참조하십시오.

솔루션은 다음과 같이 보입니다.

![XAML UI 솔루션](images/desktop-to-uwp/xaml-ui-solution.png)

여기에서 Windows Forms 프로젝트의 이름은 **Landmarks**이며 XAML UI를 포함하는 UWP 프로젝트의 이름은 **MapUI**입니다.

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>XAML UI 만들기

UWP 프로젝트에 XAML UI를 추가합니다. 기본 지도의 XAML은 다음과 같습니다.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"    
                     HorizontalAlignment="Left"               
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>프로토콜 확장 추가

**솔루션 탐색기**솔루션에 패키징 프로젝트의 **package.appxmanifest** 파일 열고이 확장을 추가 합니다.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>    
```

프로토콜 이름을 지정하고, UWP 프로젝트가 생성한 실행 파일 이름과 진입점 클래스 이름을 제공합니다.

또 디자이너에서 **package.appxmanifest**를 열고 **선언** 탭을 선택한 다음 확장을 추가합니다.

![선언-탭](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> 지도 컨트롤은 인터넷에서 데이터를 다운로드 합니다. 지도 컨트롤을 사용하고 있다면, 인터넷 클라이언트 기능을 매니페스트에 추가해야 합니다.

<a id="start" />

### <a name="start-the-uwp-app"></a>UWP 앱 시작

먼저 데스크톱 응용 프로그램에서 UWP 앱으로 보내고 싶은 매개 변수와 프로토콜 입력이 포함된 [Uri](https://msdn.microsoft.com/library/system.uri.aspx)를 생성합니다. 그런 후 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 메서드를 호출합니다.

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>매개 변수 분석 및 페이지 표시

UWP 프로젝트의 **App** 클래스에서 **OnActivated** 이벤트 처리기를 재정의합니다. 프로토콜이 앱을 활성화 한 경우, 매개 변수를 구문 분석하고 원하는 페이지를 엽니다.

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

페이지에 전달된 매개 변수를 사용하기 위해 ``OnNavigatedTo`` 메서드를 무시합니다. 이 경우에 이 페이지에 전달된 위도 및 경도를 사용하여 지도에서 위치를 표시합니다.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

### <a name="similar-samples"></a>비슷한 샘플

[VB6 응용 프로그램에 UWP XAML 사용자 환경 추가](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

[Northwind 샘플: UWA UI 및 Win32 기존 코드에 대한 완전한 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Northwind 샘플: SQL Server에 연결된 UWP 앱](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>다른 앱에 서비스 제공

다른 앱이 사용할 수 있는 서비스를 추가합니다. 예를 들어, 다른 앱에 앱 이면의 데이터베이스에 제어된 액세스를 제공하는 서비스를 추가할 수 있습니다. 백그라운드 작업을 구현, 앱은 데스크톱 앱이 실행되지 않는 경우에도 서비스에 도달할 수 있습니다.

이에 대한 샘플입니다.

![적응형 디자인](images/desktop-to-uwp/winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>이 앱을 자세히 보세요.

:heavy_check_mark: [앱 다운로드](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [코드 찾아보기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>디자인 패턴

서비스 제공을 표시하려면 다음 작업을 수행합니다.

:1: [앱 서비스 구현](#appservice)

:2: [앱 서비스 확장 추가](#extension)

:3: [앱 서비스 테스트](#test)

<a id="appservice" />

### <a name="implement-the-app-service"></a>앱 서비스 구현

여기에서 다른 앱의 요청을 검사 및 처리하게 될 것입니다. 이 코드를 솔루션의 Windows 런타임 구성 요소에 추가합니다.

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

<a id="extension" />

### <a name="add-an-app-service-extension-to-the-packaging-project"></a>패키징 프로젝트에 앱 서비스 확장 추가

패키징 프로젝트의 **package.appxmanifest** 파일을 열고에 앱 서비스 확장 추가 ``<Application>`` 요소입니다.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="AppServiceComponent.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```
앱 서비스에 이름을 지정하고, 진입점 클래스 이름을 지정합니다. 이는 서비스를 구현한 클래스입니다.

<a id="test" />

### <a name="test-the-app-service"></a>앱 서비스 테스트

다른 앱에서 호출하여 서비스를 테스트합니다. 이 코드는 Windows 양식 또는 다른 UWP 앱 등의 데스크톱 응용 프로그램일 수 있습니다.

> [!NOTE]
> 이 코드는 ``AppServiceConnection`` 클래스의 ``PackageFamilyName`` 속성을 올바르게 설정하는 경우에만 작동합니다. UWP 프로젝트의 컨텍스트에서 ``Windows.ApplicationModel.Package.Current.Id.FamilyName``을 호출하여 이 이름을 얻을 수 있습니다. [앱 서비스 만들기 및 사용](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)을 참조하세요.

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

앱 서비스에 대한 자세한 내용은 [앱 서비스 만들기 및 사용](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)을 참조하세요.

### <a name="similar-samples"></a>비슷한 샘플

[앱 서비스 브리지 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[C++win32 앱과 앱 서비스 브리지](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[푸시 알림을 받는 MFC 응용 프로그램](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>데스크톱 응용 프로그램을 공유 대상으로 만들기

데스크톱 응용 프로그램을 공유 대상으로 만들면 사용자가 공유를 지원하는 다른 앱의 사진 등 데이터를 쉽게 공유할 수 있습니다.

예를 들어, 사용자가 여러분의 앱을 선택해 Microsoft Edge, 사진 앱의 사진을 공유할 수 있습니다. 다음은 이 기능을 가지고 있는 WPF 샘플 앱입니다.

![공유 대상](images/desktop-to-uwp/share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>이 앱을 자세히 보세요.

:heavy_check_mark: [앱 다운로드](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [코드 찾아보기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>디자인 패턴

응용 프로그램을 공유 대상으로 만들려면 이런 작업을 수행합니다.

:1: [공유 대상 확장 추가](#share-extension)

:2: [OnNavigatedTo 이벤트 처리기 재정의](#override)

<a id="share-extension" />

### <a name="add-a-share-target-extension"></a>공유 대상 확장 추가

**솔루션 탐색기**솔루션에 패키징 프로젝트의 **package.appxmanifest** 파일을 열고 확장을 추가 합니다.

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

<a id="override" />

### <a name="override-the-onnavigatedto-event-handler"></a>OnNavigatedTo 이벤트 처리기 재정의

UWP 프로젝트의 **App** 클래스에서 **OnNavigatedTo** 이벤트 처리기를 재정의합니다.

이 이벤트 처리기는 사용자가 파일을 공유하기 위해 앱을 선택할 때 호출됩니다.

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

## <a name="create-a-background-task"></a>백그라운드 작업 만들기

앱이 일시 중단된 경우에도 코드를 실행하는 백그라운드 작업을 추가합니다. 백그라운드 작업은 사용자의 상호 작용이 필요하지 않은 작은 작업에 적합합니다. 예를 들어, 메일을 다운로드하거나, 들어오는 채팅 메시지에 대한 알림을 표시하거나, 시스템 조건의 변경에 대응하는 작업입니다.

백그라운드 작업을 등록하는 WPF 샘플 앱은 다음과 같습니다.

![백그라운드 작업](images/desktop-to-uwp/sample-background-task.png)

작업은 http 요청을 만들고 요청이 응답을 반환하는 데 걸리는 시간을 측정합니다. 귀하의 작업 훨씬 더 흥미로울 수 있지만 이 샘플은 백그라운드 작업의 기본 메커니즘을 학습하는 데 적합합니다.

### <a name="have-a-closer-look-at-this-app"></a>이 앱을 자세히 보세요.

:heavy_check_mark: [코드 찾아보기](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)

### <a name="the-design-pattern"></a>디자인 패턴

백그라운드 서비스를 만들려면 다음 작업을 수행합니다.

:1: [백그라운드 작업 구현](#implement-task)

:2: [백그라운드 작업 구성](#configure-background-task)

:3: [백그라운드 작업 등록](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>백그라운드 작업 구현

Windows 런타임 구성 요소 프로젝트에 코드를 추가하여 백그라운드 작업을 구현합니다.

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>백그라운드 작업 구성

매니페스트 디자이너에서 솔루션에 패키징 프로젝트의 **package.appxmanifest** 파일을 엽니다.

**선언** 탭에서 **백그라운드 작업** 선언을 추가합니다.

![백그라운드 작업 옵션](images/desktop-to-uwp/background-task-option.png)

그런 다음 원하는 속성을 선택합니다. 이 샘플에서는 **타이머** 속성을 사용합니다.

![타이머 속성](images/desktop-to-uwp/timer-property.png)

백그라운드 작업을 구현하는 Windows 런타임 구성 요소의 정규화된 클래스 이름을 제공합니다.

![타이머 속성](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>백그라운드 작업 등록

백그라운드 작업을 등록하는 데스크톱 응용 프로그램 프로젝트에 코드를 추가합니다.

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```
## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
