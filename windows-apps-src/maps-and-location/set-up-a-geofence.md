---
author: PatrickFarley
title: "지오펜스 설정"
description: "앱에서 지오펜스를 설정하고 포그라운드 및 백그라운드에서 알림을 처리하는 방법을 알아봅니다."
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 지도, 위치, 지오펜스, 알림"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8a143359948e536d30efb425055969ae8ac0987f
ms.lasthandoff: 02/07/2017

---

# <a name="set-up-a-geofence"></a>지오펜스 설정


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


앱에서 [**지오펜스**](https://msdn.microsoft.com/library/windows/apps/dn263587)를 설정하고 포그라운드 및 백그라운드에서 알림을 처리하는 방법을 알아봅니다.

**팁** 앱에서 위치에 액세스하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="enable-the-location-capability"></a>위치 접근 권한 값 사용


1.  **솔루션 탐색기**에서 **package.appxmanifest**를 두 번 클릭하고 **기능** 탭을 선택합니다.
2.  **기능** 목록에서 **위치**를 선택합니다. 그러면 `Location` 디바이스 접근 권한 값이 패키지 매니페스트 파일에 추가됩니다.

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>지오펜스 설정


### <a name="step-1-request-access-to-the-users-location"></a>1단계: 사용자의 위치에 대한 액세스 요청

**중요** 사용자의 위치에 액세스하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 메서드를 사용하여 사용자의 위치에 대한 액세스를 요청해야 합니다. UI 스레드에서 **RequestAccessAsync** 메서드를 호출해야 하며 앱이 포그라운드에 있어야 합니다. 사용자가 앱에 권한을 부여할 때까지는 앱에서 사용자의 위치 정보에 액세스할 수 없습니다.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 메서드는 사용자에게 위치 엑세스 권한을 허용하라고 메시지를 표시합니다. 이 메시지는 앱당 한 번만 표시됩니다. 사용자가 최초로 권한을 부여하거나 거부한 후에는 사용자에게 더 이상 권한을 묻지 않습니다. 메시지가 표시된 후 나중에 사용자가 위치 권한을 변경할 수 있도록 이 항목의 뒷부분에 설명된 것처럼 위치 설정에 대한 링크를 제공하는 것이 좋습니다.

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>2단계: 지오펜스 상태 및 위치 사용 권한 변경에 대해 등록

이 예제에서는 사용자 위치에 대한 액세스가 허용될 때만 **switch** 문이 이전 예제의 **accessStatus**와 함께 사용됩니다. 사용자의 위치에 대한 액세스가 허용되면 코드에서 현재 지오펜스에 액세스하고 지오펜스 상태 변경을 등록하고 위치 사용 권한의 변경을 등록합니다.

**팁** 지오펜스를 사용할 경우 Geolocator 클래스의 StatusChanged 이벤트 대신 GeofenceMonitor 클래스의 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn263646) 이벤트를 사용하여 위치 사용 권한의 변경을 모니터링합니다. **Disabled**의 [**GeofenceMonitorStatus**](https://msdn.microsoft.com/library/windows/apps/dn263599)는 사용하지 않도록 설정된 [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599)와 같습니다. 둘 다 앱이 사용자의 위치에 액세스할 수 있는 권한이 없음을 나타냅니다.

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

그러면 포그라운드 앱을 벗어날 때 이벤트 수신기의 등록이 취소됩니다.

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>3단계: 지오펜스 만들기

이제 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 개체를 정의하고 설정할 준비가 되었습니다. 필요에 따라 선택할 수 있는 여러 생성자 오버로드가 있습니다. 다음과 같이 가장 기본적인 지오펜스 생성자에서 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn263724) 및 [**Geoshape**](https://msdn.microsoft.com/library/windows/apps/dn263718)만 지정합니다.

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

다른 생성자 중 하나를 사용하여 추가로 지오펜스를 미세 조정할 수 있습니다. 다음 예제에서 지오펜스 생성자는 다음과 같은 추가 매개 변수를 지정합니다.

-   [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728) - 정의된 지역으로 들어가거나, 정의된 지역을 떠나거나, 지오펜스를 제거는 경우 알림을 받을 지오펜스 이벤트를 나타냅니다.
-   [**SingleUse**](https://msdn.microsoft.com/library/windows/apps/dn263732) 플래그 - 지오펜스를 모니터링하는 모든 상태가 충족되었을 때 지오펜스를 제거합니다.
-   [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) - enter/exit 이벤트가 트리거되기까지 사용자가 정의된 영역에 있거나 이 영역에서 벗어나 있어야 하는 시간을 나타냅니다.
-   [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn263735) - 지오펜스 모니터링을 시작해야 할 시기를 나타냅니다.
-   [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn263697) - 지오펜스를 모니터링할 기간을 나타냅니다.

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

### <a name="step-4-handle-changes-in-location-permissions"></a>4단계: 위치 사용 권한의 변경 내용 처리

[**GeofenceMonitor**](https://msdn.microsoft.com/library/windows/apps/dn263595) 개체는 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn263646) 이벤트를 트리거하여 사용자의 위치 설정이 변경되었음을 나타냅니다. 해당 이벤트는 인수의 **sender.Status** 속성([**GeofenceMonitorStatus**](https://msdn.microsoft.com/library/windows/apps/dn263599) 유형)을 통해 해당 상태를 전달합니다. 이 메서드는 UI 스레드로부터 호출되지 않고 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 개체가 UI 변경 사항을 호출합니다.

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


지오펜스를 만든 후에는 지오펜스 이벤트가 발생할 때 일어날 일을 처리하기 위한 논리를 추가해야 합니다. 설정한 [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728)에 따라, 다음의 경우 이벤트를 수신할 수 있습니다.

-   사용자가 관심 영역에 들어올 때
-   사용자가 관심 영역을 떠날 때
-   지오펜스가 만료되거나 제거되었을 때 제거 이벤트에 대해서는 백그라운드 앱이 활성화되지 않습니다.

실행 중인 앱에서 직접 이벤트를 수신 대기할 수도 있고, 이벤트 발생 시 백그라운드 알림을 수신하도록 백그라운드 작업을 등록할 수도 있습니다.

### <a name="step-1-register-for-geofence-state-change-events"></a>1단계: 지오펜스 상태 변경 이벤트 등록

앱이 지오펜스 상태 변경의 포그라운드 알림을 수신하려면 이벤트 처리기를 등록해야 합니다. 이는 대개 지오펜스를 만들 때 설정됩니다.

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>2단계: 지오펜스 이벤트 처리기 구현

다음 단계에서는 이벤트 처리기를 구현합니다. 여기에서 취하는 조치는 앱에서 지오펜스를 사용하는 목적에 따라 달라집니다.

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

## <a name="set-up-background-notifications"></a>백그라운드 알림 설정


지오펜스를 만든 후에는 지오펜스 이벤트가 발생할 때 일어날 일을 처리하기 위한 논리를 추가해야 합니다. 설정한 [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728)에 따라, 다음의 경우 이벤트를 수신할 수 있습니다.

-   사용자가 관심 영역에 들어올 때
-   사용자가 관심 영역을 떠날 때
-   지오펜스가 만료되거나 제거되었을 때 제거 이벤트에 대해서는 백그라운드 앱이 활성화되지 않습니다.

백그라운드에서 지오펜스 이벤트를 수신 대기하려면

-   앱의 매니페스트에서 백그라운드 작업을 선언합니다.
-   앱에서 백그라운드 작업을 등록합니다. 이벤트가 트리거될 때 앱이 인터넷에 액세스해야 하는 경우(예: 클라우드 서비스에 액세스) 이에 대한 플래그를 설정할 수 있습니다. 이벤트가 트리거될 때 사용자에게 표시되어 사용자가 알 수 있도록 플래그를 설정할 수도 있습니다.
-   앱이 포그라운드에서 실행 중일 때 앱 위치 사용 권한을 허용하도록 사용자에게 메시지를 표시합니다.

### <a name="step-1-register-for-geofence-state-change-events"></a>1단계: 지오펜스 상태 변경 이벤트 등록

앱 매니페스트의 **선언** 탭에서 위치 백그라운드 작업에 선언을 추가합니다. 이렇게 하려면 다음을 수행합니다.

-   **백그라운드 작업** 유형의 선언을 추가합니다.
-   **위치**의 속성 작업 유형을 설정합니다.
-   이벤트가 트리거될 때 호출하도록 앱에 진입점을 설정합니다.

### <a name="step-2-register-the-background-task"></a>2단계: 백그라운드 작업 등록

이 단계의 코드는 지오펜싱 백그라운드 작업을 등록합니다. 지오펜스가 생성되면 위치 사용 권한을 확인한다는 점을 기억하세요.

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

### <a name="step-3-handling-the-background-notification"></a>3단계: 배경 알림 처리

사용자에게 알리기 위해 취하는 조치는 앱의 작업 내용에 달려 있지만, 알림 메시지를 표시하거나 오디오 사운드를 재생하거나 라이브 타일을 업데이트할 수 있습니다. 이 단계의 코드는 알림을 처리합니다.

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


위치 개인 정보 설정에 따라 앱이 사용자의 위치에 액세스할 수 없는 경우 **설정** 앱에서 **위치 개인 정보 설정**에 대한 편리한 링크를 제공하는 것이 좋습니다. 이 예제에서는 하이퍼링크 컨트롤이 `ms-settings:privacy-location` URI로 이동하는 데 사용됩니다.

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

또는 앱이 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 호출하여 코드에서 **설정** 앱을 실행할 수 있습니다. 자세한 내용은 [Windows 설정 앱 실행](https://msdn.microsoft.com/library/windows/apps/mt228342)을 참조하세요.

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>앱 테스트 및 디버그


지오펜싱 앱 테스트 및 디버깅은 장치 위치에 따라 달라지기 때문에 어려울 수 있습니다. 여기서는 포그라운드 및 백그라운드 지오펜스를 둘 다 테스트하는 여러 메서드에 대해 간략하게 설명합니다.

**지오펜싱 앱을 디버그하려면**

1.  실제로 장치를 새 위치로 이동합니다.
2.  현재 실제 위치를 포함하여 이미 지오펜스 내부에 있고 "지오펜스 입력" 이벤트가 즉시 트리거되도록 지오펜스 영역을 만들어 지오펜스 입력을 테스트합니다.
3.  Microsoft Visual Studio 에뮬레이터를 사용하여 장치 위치를 시뮬레이트합니다.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>포그라운드에서 실행되는 지오펜싱 앱 테스트 및 디버깅

**포그라운드에서 실행되는 지오펜싱 앱을 테스트하려면**

1.  Visual Studio에서 앱을 빌드합니다.
2.  Visual Studio 에뮬레이터에서 앱을 시작합니다.
3.  이러한 도구를 사용하여 지오펜스 영역 내부와 외부의 다양한 위치를 시뮬레이트합니다. 이벤트를 트리거하기 전에 [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) 속성에 지정된 시간 동안 기다려야 합니다. 앱에서 위치를 사용할 수 있도록 할 것인지 묻는 메시지가 표시될 때 수락해야 합니다. 위치 시뮬레이트에 대한 자세한 내용은 [시뮬레이트된 디바이스의 지리적 위치 설정](http://go.microsoft.com/fwlink/p/?LinkID=325245)을 참조하세요.
4.  에뮬레이터를 사용하여 펜스 크기와 다양한 속도에서 감지되어야 하는 대략적인 유지 시간을 예측할 수도 있습니다.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>백그라운드에서 실행되는 지오펜싱 앱 테스트 및 디버깅

**백그라운드에서 실행되는 지오펜싱 앱을 테스트하려면**

1.  Visual Studio에서 앱을 빌드합니다. 앱에서는 **위치** 백그라운드 작업 형식을 설정해야 합니다.
2.  먼저 로컬 앱을 배포합니다.
3.  로컬에서 실행 중인 앱을 닫습니다.
4.  Visual Studio 에뮬레이터에서 앱을 시작합니다. 백그라운드 지오펜싱 시뮬레이션은 에뮬레이터 내에서 한 번에 하나의 앱에서만 지원됩니다. 에뮬레이터 내에서 지오펜싱 앱을 여러 개 시작하지 마세요.
5.  에뮬레이터에서 지오펜스 영역 내부와 외부의 다양한 위치를 시뮬레이트합니다. 이벤트를 트리거하기 전에 [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) 동안 기다려야 합니다. 앱에서 위치를 사용할 수 있도록 할 것인지 묻는 메시지가 표시될 때 수락해야 합니다.
6.  Visual Studio를 사용하여 위치 백그라운드 작업을 트리거합니다. Visual Studio에서 백그라운드 작업을 트리거하는 방법에 대한 자세한 내용은 [백그라운드 작업을 트리거하는 방법](http://go.microsoft.com/fwlink/p/?LinkID=325378)을 참조하세요.

## <a name="troubleshoot-your-app"></a>앱 문제 해결


앱이 위치에 액세스하려면 먼저 디바이스에서 **위치**를 사용하도록 설정해야 합니다. **설정** 앱에서 다음 **위치 개인정보 설정**이 켜져 있는지 확인합니다.

-   **이 장치의 위치...**가 **켜짐** 상태임(Windows 10 Mobile에는 해당되지 않음)
-   위치 서비스 설정 **위치**가 **켜짐** 상태임
-   **사용자의 위치를 사용할 수 있는 앱 선택**에서 앱이 **on** 상태임

## <a name="related-topics"></a>관련 항목

* [UWP 지리적 위치 샘플](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [지오펜스에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [위치 인식 앱에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/hh465148)

