---
Description: Learn how to use the powerful Visits Tracking feature for more practical location tracking.
title: 방문 추적 사용에 대한 지침
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.date: 05/18/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, geovisit
ms.localizationpriority: medium
ms.openlocfilehash: db351660722cd13a4e8f14bebb651d60f33d1671
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927542"
---
# <a name="guidelines-for-using-visits-tracking"></a>방문 추적 사용에 대한 지침

방문 기능은 위치 추적 과정을 간소화하여 많은 앱의 유용한 목적을 더욱 효율적으로 사용할 수 있게 해 줍니다. 방문은 사용자가 들어오고 나가는 중요한 지리적 영역으로 정의됩니다. 방문은 사용자가 특정 관심 영역에 들어가거나 나갈 때만 앱에 알림을 허용하여, 배터리 사용 시간을 줄일 수 있는 지속적인 위치 추적의 필요성을 제거하는 [지오펜스](guidelines-for-geofencing.md)와 유사합니다. 하지만 지오펜스와 달리 방문 영역은 플랫폼 수준에서 동적으로 식별되므로 개별 앱에서 명시적으로 정의할 필요가 없습니다. 또한 앱이 어떤 방문을 추적하는지 선택하는 것은 개별 장소를 구독하는 방식 대신 세분화된 단일 설정하는 방식으로 처리됩니다.

## <a name="preliminary-setup"></a>사전 설정

진행하기 전에 앱이 장치의 위치에 액세스할 수 있는지 확인합니다. 매니페스트에 `Location` 기능을 선언하고 **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** 메서드를 호출하여 사용자가 앱 위치 권한을 부여했는지 확인해야 합니다. 이를 수행하는 방법에 대한 자세한 내용은 [사용자 위치 가져오기](get-location.md)를 참조하세요. 

`Geolocation` 네임스페이스를 클래스에 추가해야 합니다. 이 가이드의 모든 코드 조각이 작동하기 위해 필요합니다.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>마지막 방문 확인
방문 추적 기능을 사용하는 가장 간단한 방법은 마지막으로 알려진 방문 관련 상태 변경을 검색하는 것입니다. 상태 변경은 사용자가 중요 위치에 들어오고 나가거나, 마지막 보고 이후 중요한 움직임이 있거나, 사용자의 위치가 손실된 플랫폼 로그 이벤트입니다(**[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)** enum 참조). 상태 변경은 **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)** 인스턴스에 의해 나타납니다. 마지막으로 보고된 상태 변경에 대해 **Geovisit** 인스턴스를 검색하려면 **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)** 클래스에 지정된 메서드를 사용하면 됩니다.

> [!NOTE]
> 마지막으로 기록한 방문을 확인했다고 해서 현재 시스템에서 방문 수를 추적하고 있는 것은 아닙니다. 방문자가 발생했을 때 방문을 추적하려면 포그라운드에서 모니터링하거나 백그라운드 추적에 등록해야 합니다(아래 섹션 참조).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Geovisit 인스턴스 구문 분석(선택 사항)
다음 메서드는 **Geovisit** 인스턴스에 저장된 모든 정보를 쉽게 읽을 수 있는 문자열로 변환합니다. 이 가이드의 모든 시나리오에서 사용하여 보고되는 방문에 대한 피드백을 제공할 수 있습니다.

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>포그라운드에서 방문 모니터링

이전 섹션에서 사용된 **GeovisitMonitor** 클래스는 일정 기간 동안 상태 변경을 수신 대기하는 시나리오도 처리합니다. 이 클래스를 인스턴스화하고, 이벤트에 대한 처리기 메서드를 등록하고, `Start` 메서드를 호출하여 이를 수행할 수 있습니다.

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

이 예에서 `OnVisitStateChanged` 메서드는 들어오는 방문 보고를 처리합니다. 해당 **Geovisit** 인스턴스는 이벤트 매개 변수를 통해 전달됩니다.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
앱이 방문과 관련된 상태 변경에 대한 모니터링을 완료하면 모니터링을 중지하고 이벤트 처리기 등록을 취소해야 합니다. 앱이 일시 중단되거나 닫힐 때마다 이 작업을 수행해야 합니다.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>백그라운드에서 방문 모니터링

백그라운드 작업에서 방문 모니터링을 구현하여 앱이 열려 있지 않아도 방문 관련 활동을 장치에서 처리할 수 있습니다. 이는 더욱 유용하고 에너지 효율적이기 때문에 권장되는 방법입니다. 

이 가이드에서는 주 응용 프로그램 파일을 한 프로젝트에 저장하고 백그라운드 작업 파일을 동일한 솔루션의 별도 프로젝트에 저장하는 [Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) 과정의 모델을 사용합니다. 백그라운드 작업을 처음 사용하는 경우에는 먼저 이 지침을 따르고 아래에서 필요한 대체 작업을 수행하여 방문 처리 백그라운드 작업을 만드는 것이 좋습니다.

> [!NOTE]
> 다음 조각에서는 편의상 오류 처리 및 로컬 저장소 같은 일부 중요한 기능을 배제합니다. 백그라운드 방문 처리의 강력한 구현을 위해 [샘플 앱](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)을 참조하세요.


먼저 앱에 백그라운드 작업 권한이 있는지 확인합니다. *Package.appxmanifest* 파일의 `Application/Extensions` 요소에 다음 확장자를 추가합니다(아직 존재하지 않는 경우 `Extensions` 요소 추가).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

그런 다음, 백그라운드 작업 클래스 정의에 다음 코드를 붙여 넣습니다. 이 백그라운드 작업의 `Run` 메서드는 방문 정보를 포함하는 트리거 세부 정보를 별도의 메서드로 전달합니다.

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

이 동일한 클래스의 `GetVisitReports` 메서드를 정의합니다.

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

그런 다음 앱의 기본 프로젝트에서 이 백그라운드 작업의 등록을 수행해야 합니다. 일부 사용자 작업으로 호출할 수 있거나 클래스가 활성화될 때마다 호출되는 등록 메서드를 만듭니다.

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

이는 네임스페이스 `Tasks`에 있는 `VisitBackgroundTask` 백그라운드 작업 클래스가 `location` 트리거 유형으로 어떠한 작업을 수행할 것이라고 설정합니다. 

이제 앱에서 방문 처리 백그라운드 작업을 등록할 수 있어야 하며 장치가 방문 관련 상태 변경을 기록할 때마다 이 작업이 활성화되어야 합니다. 이 상태 변경 정보로 수행할 작업을 결정하려면 백그라운드 작업 클래스의 논리를 입력해야 합니다.

## <a name="related-topics"></a>관련 항목
* [Out-of-process 백그라운드 작업 만들기 및 등록](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [사용자 위치 가져오기](get-location.md)
* [Windows.Devices.Geolocation 네임스페이스](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)