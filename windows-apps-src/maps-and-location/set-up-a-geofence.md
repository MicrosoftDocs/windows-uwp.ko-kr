---
title: 지오펜스 설정
description: 앱에서 지 오를 설정 하 고, 포그라운드 및 백그라운드에서 알림을 처리 하는 방법을 알아봅니다.
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 지 오, 알림
ms.localizationpriority: medium
ms.openlocfilehash: 9b991930ba37cfaec333146bf7a95b4c9a9c8b98
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171707"
---
# <a name="set-up-a-geofence"></a>지오펜스 설정




앱에서 지 [**오**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 를 설정 하 고, 포그라운드 및 백그라운드에서 알림을 처리 하는 방법을 알아봅니다.

**팁** 앱의 위치에 액세스 하는 방법에 대 한 자세한 내용은 GitHub의 [Windows 유니버설 샘플 리포지토리](https://github.com/Microsoft/Windows-universal-samples) 에서 다음 샘플을 다운로드 하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>위치 기능 사용


1.  **솔루션 탐색기**에서 **appxmanifest.xml** 를 두 번 클릭 하 고 **기능** 탭을 선택 합니다.
2.  **기능** 목록에서 **위치**를 확인 합니다. 그러면 `Location` 패키지 매니페스트 파일에 장치 기능이 추가 됩니다.

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>지오펜스 설정


### <a name="step-1-request-access-to-the-users-location"></a>1 단계: 사용자의 위치에 대 한 액세스 요청

**중요** 사용자의 위치에 액세스를 시도 하기 전에 [**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 메서드를 사용 하 여 사용자의 위치에 대 한 액세스를 요청 해야 합니다. UI 스레드에서 **Requestaccessasync** 메서드를 호출 해야 하며 응용 프로그램은 전경에 있어야 합니다. 사용자가 앱에 권한을 부여할 때까지 앱에서 사용자의 위치 정보에 액세스할 수 없습니다.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

[**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 메서드는 사용자에 게 해당 위치에 액세스할 수 있는 권한을 요청 하 라는 메시지를 표시 합니다. 사용자에 게 한 번만 메시지가 표시 됩니다 (앱 당). 처음으로 사용 권한을 부여 하거나 거부 한 후에는이 메서드는 더 이상 사용자에 게 사용 권한을 요청 하지 않습니다. 메시지를 표시 한 후 사용자가 위치를 변경할 수 있도록 하려면이 항목의 뒷부분에 설명 된 대로 위치 설정에 대 한 링크를 제공 하는 것이 좋습니다.

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>2 단계: 지 오 상태 및 위치 권한에서 변경 내용 등록

이 예제에서 **switch** 문은 이전 예제의 **accessstatus** 와 함께 사용 되어 사용자의 위치에 대 한 액세스가 허용 되는 경우에만 작동 합니다. 사용자의 위치에 대 한 액세스가 허용 되는 경우 코드는 현재 지역 구분에 액세스 하 고, 지 오 상태 변경을 등록 하 고, 위치 사용 권한 변경 내용을 등록 합니다.

**팁** 지오펜스를 사용할 경우 Geolocator 클래스의 StatusChanged 이벤트 대신 GeofenceMonitor 클래스의 [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) 이벤트를 사용하여 위치 사용 권한의 변경을 모니터링합니다. **사용 안 함** [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus) 은 사용 하지 않도록 설정 된 [**positionstatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus) 와 동일 합니다. 둘 다 앱에 사용자의 위치에 액세스할 수 있는 권한이 없음을 의미 합니다.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;


    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

그런 다음 포그라운드 앱에서 이동할 때 이벤트 수신기를 등록 취소 합니다.

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>3 단계: 지 오를 만듭니다.

이제는 지 [**오**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 개체 개체를 정의 하 고 설정할 준비가 되었습니다. 요구 사항에 따라 선택할 수 있는 몇 가지 다른 생성자 오버 로드가 있습니다. 가장 기본적인 지 오 지 오 생성자에서 여기에 표시 된 것 처럼 [**Id**](/uwp/api/windows.devices.geolocation.geofencing.geofence.id) 및 [**geoshape**](/uwp/api/windows.devices.geolocation.geofencing.geofence.geoshape) 지정 합니다.

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);

```

다른 생성자 중 하나를 사용 하 여 지 오를 세부적으로 조정할 수 있습니다. 다음 예제에서 지 오 생성자는 다음과 같은 추가 매개 변수를 지정 합니다.

-   [**Monitoredstates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) -정의 된 지역에 들어오거나, 정의 된 지역에서 나 지 오를 제거 하는 알림을 수신 하려는 오 펜스 이벤트를 나타냅니다.
-   [**SingleUse**](/uwp/api/windows.devices.geolocation.geofencing.geofence.singleuse) -지 오를 모니터링 하는 모든 상태가 충족 되 면 지 오를 제거 합니다.
-   [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) -enter/exit 이벤트를 트리거하기 전에 사용자가 정의 된 영역에 있거나 그 어느 정도까지 있는지를 나타냅니다.
-   [**StartTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.starttime) -지 오의 모니터링을 시작 하는 시기를 나타냅니다.
-   [**Duration**](/uwp/api/windows.devices.geolocation.geofencing.geofence.duration) -지 오를 모니터링할 기간을 나타냅니다.

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

만든 후 모니터에 새 지 [**오**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 를 등록 해야 합니다.

```csharp
// Register the geofence
try {
   GeofenceMonitor.Current.Geofences.Add(geofence);
} catch {
   // Handle failure to add geofence
}
```

### <a name="step-4-handle-changes-in-location-permissions"></a>4 단계: 위치 권한 변경 내용 처리

[**GeofenceMonitor**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitor) 개체는 [**statuschanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) 이벤트를 트리거하여 사용자의 위치 설정이 변경 되었음을 표시 합니다. 해당 이벤트는 인수의 발신자를 통해 해당 상태를 전달 **합니다. Status** 속성 ( [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus)형식). 이 메서드는 UI 스레드에서 호출 되지 않으며 [**디스패처**](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체가 ui 변경을 호출 합니다.

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>포그라운드 알림 설정


지역 구분을 만든 후에는이 논리를 추가 하 여 지 오 이벤트 이벤트가 발생 했을 때 발생 하는 상황을 처리 해야 합니다. 설정한 [**Monitoredstates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) 에 따라 다음과 같은 경우에 이벤트를 받을 수 있습니다.

-   사용자가 관심 영역을 입력 합니다.
-   사용자는 관심 영역을 남겨 둡니다.
-   지 오 펜스가 만료 되었거나 제거 되었습니다. 백그라운드 앱은 제거 이벤트에 대해 활성화 되지 않습니다.

앱이 실행 중일 때 앱에서 직접 이벤트를 수신 대기 하거나 이벤트 발생 시 백그라운드 알림을 받도록 백그라운드 작업을 등록할 수 있습니다.

### <a name="step-1-register-for-geofence-state-change-events"></a>1 단계: 지 오 상태 변경 이벤트 등록

앱이 지 오 상태 변경의 포그라운드 알림을 받으려면 이벤트 처리기를 등록 해야 합니다. 이는 일반적으로 지 오를 만들 때 설정 됩니다.

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>2 단계: 지 오 이벤트 처리기 구현

다음 단계는 이벤트 처리기를 구현 하는 것입니다. 여기에서 수행 하는 작업은 앱이에서 지 오를 사용 하는 항목에 따라 다릅니다.

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>배경 알림 설정


지역 구분을 만든 후에는이 논리를 추가 하 여 지 오 이벤트 이벤트가 발생 했을 때 발생 하는 상황을 처리 해야 합니다. 설정한 [**Monitoredstates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) 에 따라 다음과 같은 경우에 이벤트를 받을 수 있습니다.

-   사용자가 관심 영역을 입력 합니다.
-   사용자는 관심 영역을 남겨 둡니다.
-   지 오 펜스가 만료 되었거나 제거 되었습니다. 백그라운드 앱은 제거 이벤트에 대해 활성화 되지 않습니다.

백그라운드에서 지 오 트 이벤트를 수신 대기 하려면

-   응용 프로그램의 매니페스트에서 백그라운드 작업을 선언 합니다.
-   앱에서 백그라운드 작업을 등록 합니다. 앱에서 인터넷에 액세스 해야 하는 경우 (클라우드 서비스에 액세스 하는 경우) 이벤트가 트리거될 때이에 대 한 플래그를 설정할 수 있습니다. 사용자가 알림 메시지를 받을 수 있도록 이벤트를 트리거할 때 사용자가 있는지 확인 하도록 플래그를 설정할 수도 있습니다.
-   응용 프로그램이 포그라운드에서 실행 되는 동안 사용자에 게 앱 위치 권한을 부여 하 라는 메시지를 표시 합니다.

### <a name="step-1-register-for-geofence-state-change-events"></a>1 단계: 지 오 상태 변경 이벤트 등록

응용 프로그램의 매니페스트에 있는 **선언** 탭에서 위치 백그라운드 작업에 대 한 선언을 추가 합니다. 가상 하드 디스크 파일에 대한 중요 정보를 제공하려면

-   **백그라운드 작업**형식의 선언을 추가 합니다.
-   **위치의**속성 작업 유형을 설정 합니다.
-   이벤트가 트리거될 때 호출할 진입점을 앱으로 설정 합니다.

### <a name="step-2-register-the-background-task"></a>2 단계: 백그라운드 작업 등록

이 단계의 코드는 지 오 펜싱 백그라운드 작업을 등록 합니다. 지 오를 만든 경우 위치 사용 권한을 확인 합니다.

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>3 단계: 백그라운드 알림 처리

사용자에 게 알리기 위해 수행 하는 작업은 앱에서 수행 하는 작업에 따라 달라 지지만 알림 메시지를 표시 하거나, 오디오 사운드를 재생 하거나, 라이브 타일을 업데이트할 수 있습니다. 이 단계의 코드는 알림을 처리 합니다.

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>개인 정보 설정 변경


위치 개인 정보 설정을 통해 앱에서 사용자의 위치에 액세스할 수 없는 경우 **설정** 앱에서 **위치 개인 정보 설정** 에 대 한 편리한 링크를 제공 하는 것이 좋습니다. 이 예제에서 Hyperlink 컨트롤은 URI로 이동 하는 데 사용 됩니다 `ms-settings:privacy-location` .

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

또는 앱에서 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 메서드를 호출 하 여 코드에서 **설정** 앱을 시작할 수 있습니다. 자세한 내용은 [Windows 설정 앱 시작](../launch-resume/launch-settings-app.md)을 참조 하세요.

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>응용 프로그램 테스트 및 디버그


지 오 펜싱 앱을 테스트 하 고 디버깅 하는 것은 장치의 위치에 따라 달라 지기 때문에 어려울 수 있습니다. 여기서는 포그라운드 및 백그라운드 지역 구분을 테스트 하기 위한 몇 가지 방법을 설명 합니다.

**지 오 펜싱 앱을 디버깅 하려면**

1.  장치를 새 위치로 실제로 이동 합니다.
2.  현재 실제 위치를 포함 하는 지 오 지역을 만들어 지 오를 테스트 하 여 현재 위치를 지 오 하는 사용자가 이미 존재 하 고 있고 "사용자가 입력 된 지 오" 이벤트가 즉시 트리거됩니다.
3.  Microsoft Visual Studio 에뮬레이터를 사용 하 여 장치에 대 한 위치를 시뮬레이션 합니다.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>포그라운드로 실행 되는 지 오 펜싱 앱 테스트 및 디버그

**포그라운드를 실행 하는 지 오 펜싱 앱을 테스트 하려면**

1.  Visual Studio에서 응용 프로그램을 빌드합니다.
2.  Visual Studio 에뮬레이터에서 앱을 시작 합니다.
3.  이러한 도구를 사용 하 여 지 오 지역 내부 및 외부에서 다양 한 위치를 시뮬레이션할 수 있습니다. [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) 속성으로 지정 된 시간이 지난 후 이벤트를 트리거할 수 있도록 해야 합니다. 앱에 대 한 위치를 사용 하도록 설정 하 라는 메시지가 표시 되어야 합니다. 위치를 시뮬레이션 하는 방법에 대 한 자세한 내용은 [장치의 시뮬레이션 된 지리적 위치 설정](/previous-versions/hh441475(v=vs.120)#BKMK_Set_the_simulated_geo_location_of_the_device)을 참조 하세요.
4.  에뮬레이터를 사용 하 여 서로 다른 속도로 검색 하는 데 필요한 만큼의 울타리와 유지 시간을 예측할 수도 있습니다.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>백그라운드에서 실행되는 지오펜싱 앱 테스트 및 디버깅

**배경을 실행 중인 지 오 펜싱 앱을 테스트 하려면**

1.  Visual Studio에서 응용 프로그램을 빌드합니다. 앱에서 **위치** 백그라운드 작업 유형을 설정 해야 합니다.
2.  먼저 앱을 로컬로 배포 합니다.
3.  로컬로 실행 되는 앱을 닫습니다.
4.  Visual Studio 에뮬레이터에서 앱을 시작 합니다. Background 지 오 펜싱 시뮬레이션은 에뮬레이터 내에서 한 번에 하나의 앱 에서만 지원 됩니다. 에뮬레이터 내에서 여러 지 오 펜싱 apps를 시작 하지 마세요.
5.  에뮬레이터에서 지 오 지역 내부 및 외부의 다양 한 위치를 시뮬레이션 합니다. [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) 가 이벤트를 트리거하는 데 충분 한 시간 동안 대기 해야 합니다. 앱에 대 한 위치를 사용 하도록 설정 하 라는 메시지가 표시 되어야 합니다.
6.  Visual Studio를 사용 하 여 위치 백그라운드 작업을 트리거합니다. Visual Studio에서 백그라운드 작업을 트리거하는 방법에 대 한 자세한 내용은 [백그라운드 작업을 트리거하는 방법](/previous-versions/hh974425(v=vs.120)#BKMK_Trigger_background_tasks)을 참조 하세요.

## <a name="troubleshoot-your-app"></a>앱 문제 해결


앱이 위치에 액세스할 수 있으려면 장치에서 **위치** 를 사용 하도록 설정 해야 합니다. **설정** 앱에서 다음 **위치의 개인 정보 설정이** 켜져 있는지 확인 합니다.

-   **이 장치의 위치** ... **켜 짐 (** Windows 10 Mobile에는 적용 되지 않음)
-   위치 서비스 설정 ( **위치**) **이 설정 되어** 있습니다.
-   **사용자의 위치를 사용할 수 있는 앱 선택**에서 앱이 **켜기** 로 설정 됩니다.

## <a name="related-topics"></a>관련 항목

* [UWP 지리적 위치 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [지 오 펜싱에 대 한 디자인 지침](./guidelines-for-geofencing.md)
* [위치 인식 앱에 대한 디자인 지침](./guidelines-and-checklist-for-detecting-location.md)