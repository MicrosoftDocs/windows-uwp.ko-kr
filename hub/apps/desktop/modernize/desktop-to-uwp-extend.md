---
Description: Windows UI와 구성 요소로 데스크톱 응용 프로그램 확장
title: Windows UI와 구성 요소로 데스크톱 응용 프로그램 확장
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 672485dd505227da0a59a220edaa9648e2521e63
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359633"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>최신 UWP 구성 요소를 사용 하 여 데스크톱 앱 확장

일부 Windows 10 환경(예, 터치 구현 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 이러한 환경을 추가하려는 경우, UWP 프로젝트 및 Windows 런타임 구성 요소로 데스크톱 응용 프로그램을 확장합니다.

대부분의 데스크톱 응용 프로그램에서 직접 Windows 런타임 Api를 호출, 따라서이 가이드를 검토 하기 전에 볼 수 있습니다 [향상에 대 한 Windows 10](desktop-to-uwp-enhance.md)합니다.

> [!NOTE]
> 이 문서에서 설명 하는 기능은 필요 데스크톱 응용 프로그램에 대 한 Windows 앱 패키지를 만들어야 합니다. 이렇게 아직 하지 않은 경우 [데스크톱 응용 프로그램 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)합니다.

준비가 되었으면 시작하겠습니다.

<a id="setup" />

## <a name="first-setup-your-solution"></a>먼저 솔루션 설정

하나 또는 여러 개의 UWP 프로젝트와 런타임 구성 요소를 솔루션에 추가합니다.

데스크톱 응용 프로그램에 대한 참조가 있는 **Windows 응용 프로그램 패키징 프로젝트**를 포함하는 솔루션으로 시작합니다.

이 이미지는 예시 솔루션을 보여 줍니다.

![새 프로젝트 시작](images/desktop-to-uwp/extend-start-project.png)

솔루션 패키징 프로젝트 없으면 참조 [Visual Studio를 사용 하 여 데스크톱 응용 프로그램 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)합니다.

### <a name="configure-the-desktop-application"></a>데스크톱 응용 프로그램 구성

데스크톱 응용 프로그램에 Windows 런타임 Api를 호출 해야 하는 파일에 대 한 참조가 있는지 확인 합니다.

이렇게 하려면 참조는 [프로젝트 설정](desktop-to-uwp-enhance.md#set-up-your-project) 섹션입니다.

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

### <a name="build-your-solution"></a>솔루션 빌드

오류가 표시 되는지 확인 하기 위해 솔루션을 빌드하십시오. 오류가 발생할 경우 엽니다 **Configuration Manager** 프로젝트는 동일한 플랫폼 대상 있는지 확인 합니다.

![구성 관리자](images/desktop-to-uwp/config-manager.png)

UWP 프로젝트와 런타임 구성 요소를 사용하여 할 수 있는 몇 가지 사항에 살펴보겠습니다.

## <a name="show-a-modern-xaml-ui"></a>최신 XAML UI 표시

응용 프로그램 흐름의 일부로 데스크톱 응용 프로그램에 최신 XAML 기반 사용자 인터페이스를 포함시킬 수 있습니다. 이러한 사용자 인터페이스는 자연스럽게 화면 크기와 해상도에 맞춰 조정이 되고, 터치와 잉크 같은 최신 대화형 모델을 지원합니다.

예를 들어, 조금의 XAML 태그로 사용자에게 강력한 지도 관련 시각화 기능을 제공할 수 있습니다.

이 이미지는 지도 컨트롤이 포함된 XAML 기반 최신 UI를 여는 Windows Forms 응용 프로그램을 보여 줍니다.

![적응형 디자인](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>이 예제에는 UWP 프로젝트를 솔루션에 추가 하 여 XAML UI를 보여 줍니다. 데스크톱 응용 프로그램에서 XAML Ui를 표시 하는 안정적인 지원 되는 방법입니다. 이 방법 대신 XAML 아일랜드를 사용 하 여 데스크톱 응용 프로그램에 직접 UWP XAML 컨트롤을 추가 하는 것입니다. XAML 제도 개발자 미리 보기로 현재 사용할 수 있습니다. 직접 사용해 고유의 프로토타입 코드에서 이제 하는 것이 좋습니다, 있지만 사용 것 프로덕션 코드에서는 현재 하지 않는 것이 좋습니다. 이러한 Api 및 컨트롤 성숙 하 고 안정화 향후 Windows 릴리스에서 계속 됩니다. XAML 아일랜드에 대 한 자세한 내용은를 참조 하세요. [데스크톱 응용 프로그램에서 UWP 컨트롤](xaml-islands.md)

### <a name="the-design-pattern"></a>디자인 패턴

XAML 기반 UI를 표시하려면 다음 작업을 수행합니다.

: 하나: [솔루션 설치](#solution-setup)

: 두: [XAML UI 만들기](#xaml-UI)

: 3: [UWP 프로젝트에 프로토콜 확장 추가](#add-a-protocol-extension)

: 4: [데스크톱 앱에서 UWP 앱 시작](#start)

: 5: [UWP 프로젝트에서 페이지를 표시 하려는](#parse)

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

**솔루션 탐색기**오픈를 **package.appxmanifest** 솔루션의 패키징 프로젝트의 파일이이 확장을 추가 합니다.

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

먼저 데스크톱 응용 프로그램에서 UWP 앱으로 보내고 싶은 매개 변수와 프로토콜 입력이 포함된 [Uri](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN)를 생성합니다. 그런 후 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 메서드를 호출합니다.

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

XAML 페이지에 숨김 코드에서 재정의 된 ``OnNavigatedTo`` 페이지에 전달 된 매개 변수를 사용 하는 방법입니다. 이 경우에 이 페이지에 전달된 위도 및 경도를 사용하여 지도에서 위치를 표시합니다.

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

## <a name="making-your-desktop-application-a-share-target"></a>데스크톱 응용 프로그램을 공유 대상으로 만들기

데스크톱 응용 프로그램을 공유 대상으로 만들면 사용자가 공유를 지원하는 다른 앱의 사진 등 데이터를 쉽게 공유할 수 있습니다.

예를 들어 사용자 Photos 앱은 Microsoft Edge에서 사진을 공유 하는 응용 프로그램을 선택할 수 있습니다. 해당 기능이 있는 WPF 샘플 응용 프로그램을 다음과 같습니다.

![공유 대상](images/desktop-to-uwp/share-target.png).

전체 샘플을 볼 [여기](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>디자인 패턴

응용 프로그램을 공유 대상으로 만들려면 이런 작업을 수행합니다.

: 하나: [공유 대상 확장 추가](#share-extension)

: 두: [OnShareTargetActivated 이벤트 처리기를 재정의 합니다.](#override)

: 3: [UWP 프로젝트에 데스크톱 확장 추가](#desktop-extensions)

: 4: [완전 신뢰 프로세스 확장 추가](#full-trust)

: 5: [공유 파일을 가져오려면 데스크톱 응용 프로그램 수정](#modify-desktop)

<a id="share-extension" />

다음 단계  

### <a name="add-a-share-target-extension"></a>공유 대상 확장 추가

**솔루션 탐색기**오픈를 **package.appxmanifest** 패키징의 솔루션의 프로젝트 파일과 공유 대상 확장명을 추가 합니다.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

UWP 프로젝트가 생성한 실행 파일 이름과 진입점 클래스 이름을 제공합니다. 이 태그는 UWP 앱에 대 한 실행 파일의 이름 이라고 가정 `ShareTarget.exe`합니다.

또 앱이 공유할 수 있는 파일 형식을 지정해야 합니다. 이 예제에서는 진행 중인 합니다 [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) 공유 대상 비트맵에 대 한 이미지를 지정 하도록 데스크톱 응용 프로그램 `Bitmap` 지원 되는 파일 형식에 대 한 합니다.

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>OnShareTargetActivated 이벤트 처리기를 재정의 합니다.

재정의 **OnShareTargetActivated** 이벤트 처리기는 **앱** UWP 프로젝트의 클래스입니다.

이 이벤트 처리기는 사용자가 파일을 공유하기 위해 앱을 선택할 때 호출됩니다.

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

이 코드에서는 사용자가 앱 로컬 저장소 폴더에 공유 되는 이미지를 저장 합니다. 나중에 동일한 폴더에서 이미지를 끌어올 데스크톱 응용 프로그램을 수정 합니다. UWP 앱으로 동일한 패키지에 포함 되어 있으므로 데스크톱 응용 프로그램은 가능 합니다.

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>UWP 프로젝트에 데스크톱 확장 추가

추가 된 **UWP 용 Windows 데스크톱 확장** UWP 앱 프로젝트를 확장 합니다.

![데스크톱 확장](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>완전 신뢰 프로세스 확장 추가

**솔루션 탐색기**오픈 합니다 **package.appxmanifest** 솔루션의 패키징 프로젝트의 파일 폴더를 추가한 다음이 추가 하는 공유 대상 확장명 옆에 있는 완전 신뢰 처리 확장 이전 파일입니다.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

이 확장 파일 공유를 원하는 데스크톱 응용 프로그램을 시작 하는 UWP 앱을 사용 하도록 설정 됩니다. 예제에서는 참조의 실행 파일을 [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) 데스크톱 응용 프로그램입니다.

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>공유 파일을 가져오려면 데스크톱 응용 프로그램 수정

찾기 및 공유 파일을 처리 하도록 데스크톱 응용 프로그램을 수정 합니다. UWP 앱이 예제에서는 로컬 앱 데이터 폴더에 공유 파일을 저장 합니다. 따라서 수정에서는 합니다 [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) 해당 폴더에서 끌어오기 사진에 데스크톱 응용 프로그램.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

이미 있는 데스크톱 응용 프로그램의 인스턴스를 사용자가 열려면에 대 한 것도 처리 합니다 [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher?view=netframework-4.7.2) 이벤트 및 파일 위치에 경로 전달 합니다. 이런 방식으로 데스크톱 응용 프로그램의 열려 있는 모든 인스턴스는 공유 사진을 표시 됩니다.

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>백그라운드 작업 만들기

앱이 일시 중단된 경우에도 코드를 실행하는 백그라운드 작업을 추가합니다. 백그라운드 작업은 사용자의 상호 작용이 필요하지 않은 작은 작업에 적합합니다. 예를 들어, 메일을 다운로드하거나, 들어오는 채팅 메시지에 대한 알림을 표시하거나, 시스템 조건의 변경에 대응하는 작업입니다.

백그라운드 작업을 등록 하는 WPF 샘플 응용 프로그램을 다음과 같습니다.

![백그라운드 작업](images/desktop-to-uwp/sample-background-task.png)

작업은 http 요청을 만들고 요청이 응답을 반환하는 데 걸리는 시간을 측정합니다. 귀하의 작업 훨씬 더 흥미로울 수 있지만 이 샘플은 백그라운드 작업의 기본 메커니즘을 학습하는 데 적합합니다.

전체 샘플을 참조 하세요 [여기](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)합니다.

### <a name="the-design-pattern"></a>디자인 패턴

백그라운드 서비스를 만들려면 다음 작업을 수행합니다.

: 하나: [백그라운드 작업 구현](#implement-task)

: 두: [백그라운드 태스크 구성](#configure-background-task)

: 3: [백그라운드 작업 등록](#register-background-task)

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

매니페스트 디자이너에서 엽니다는 **package.appxmanifest** 솔루션의 패키징 프로젝트의 파일입니다.

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

**질문에 답변**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.