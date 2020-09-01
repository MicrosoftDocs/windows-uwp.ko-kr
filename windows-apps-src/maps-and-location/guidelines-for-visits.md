---
Description: 더 실질적인 추적을 위해 강력한 방문 추적 기능을 사용하는 방법에 대해 알아봅니다.
title: 방문 추적 사용에 대한 지침
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.date: 05/18/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, geovisit, geovisit
ms.localizationpriority: medium
ms.openlocfilehash: 3b1766d0f883fa42b005908dcc63102e97ff0d4f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162517"
---
# <a name="guidelines-for-using-visits-tracking"></a>방문 추적 사용에 대한 지침

방문 기능을 사용 하면 위치 추적 프로세스를 간소화 하 여 많은 앱의 실용적인 용도에 더 효율적으로 적용할 수 있습니다. 방문은 사용자가 입력 하 고 종료 하는 중요 한 지역으로 정의 됩니다. 방문은 사용자가 관심 있는 특정 영역을 입력 하거나 종료 하는 경우에만 앱을 알릴 수 있도록 하는 [지역 구분](guidelines-for-geofencing.md) 와 유사 하며, 배터리 수명이 방전 될 수 있는 지속적인 위치 추적이 필요 하지 않습니다. 그러나 지역 구분와 달리 방문 영역은 플랫폼 수준에서 동적으로 식별 되며 개별 앱에서 명시적으로 정의할 필요가 없습니다. 또한 앱에서 추적할 선택 항목은 개별 위치를 구독 하는 대신 단일 세분성 설정에 의해 처리 됩니다.

## <a name="preliminary-setup"></a>예비 설정

더 진행 하기 전에 앱이 장치의 위치에 액세스할 수 있는지 확인 합니다. 매니페스트에서 기능을 선언 하 `Location` 고 **[Geolocator. RequestAccessAsync](/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** 메서드를 호출 하 여 사용자가 앱 위치 권한을 제공 하도록 해야 합니다. 이 작업을 수행 하는 방법에 대 한 자세한 내용은 [사용자 위치 가져오기](get-location.md) 를 참조 하세요. 

`Geolocation`네임 스페이스를 클래스에 추가 해야 합니다. 이 가이드의 모든 코드 조각이 작동 하려면이 작업을 수행 해야 합니다.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>최신 방문 확인
방문 추적 기능을 사용 하는 가장 간단한 방법은 알려진 마지막 방문 관련 상태 변경을 검색 하는 것입니다. 상태 변경은 사용자가 중요 한 위치를 입력/종료 하거나, 마지막 보고서 이후 상당한 이동이 발생 하거나, 사용자 위치가 손실 되는 플랫폼 로그 이벤트입니다 ( **[VisitStateChange](/uwp/api/windows.devices.geolocation.visitstatechange)** 열거형 참조). 상태 변경 내용은 **[Geovisit](/uwp/api/windows.devices.geolocation.geovisit)** 인스턴스로 표시 됩니다. 마지막으로 기록 된 상태 변경에 대 한 **Geovisit** 인스턴스를 검색 하려면 **[GeovisitMonitor](/uwp/api/windows.devices.geolocation.geovisitmonitor)** 클래스에서 지정 된 메서드를 사용 하면 됩니다.

> [!NOTE]
> 마지막으로 로그온 한 방문을 확인 해도 시스템에서 방문을 현재 추적 하 고 있음을 보장 하지는 않습니다. 발생 하는 방문을 추적 하려면 포그라운드에서 모니터링 하거나 백그라운드 추적을 등록 해야 합니다 (아래 섹션 참조).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Geovisit 인스턴스 구문 분석 (옵션)
다음 메서드는 **Geovisit** 인스턴스에 저장 된 모든 정보를 쉽게 읽을 수 있는 문자열로 변환 합니다. 이 가이드의 모든 시나리오에서 사용 하 여 보고 되는 방문에 대 한 피드백을 제공할 수 있습니다.

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

이전 섹션에서 사용 된 **GeovisitMonitor** 클래스는 일정 시간 동안의 상태 변경을 수신 하는 시나리오를 처리 합니다. 이를 위해이 클래스를 인스턴스화하고, 이벤트에 대 한 처리기 메서드를 등록 하 고, 메서드를 호출할 수 있습니다 `Start` .

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

이 예제에서 메서드는 `OnVisitStateChanged` 들어오는 방문 보고서를 처리 합니다. 해당 **Geovisit** 인스턴스는 이벤트 매개 변수를 통해 전달 됩니다.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
앱이 방문 관련 상태 변경에 대 한 모니터링을 마치면 모니터를 중지 하 고 이벤트 처리기를 등록 취소 해야 합니다. 앱이 일시 중단 되거나 닫힐 때마다이 작업을 수행 해야 합니다.

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

앱이 열려 있지 않은 경우에도 장치에서 방문 관련 작업을 처리할 수 있도록 백그라운드 작업에서 방문 모니터링을 구현할 수도 있습니다. 이 방법은 더 다양 하 고 에너지 효율적 이기 때문에 권장 되는 방법입니다. 

이 가이드에서는 [in-process 백그라운드 작업 만들기 및 등록](../launch-resume/create-and-register-a-background-task.md)에서 모델을 사용 합니다 .이 작업은 주 응용 프로그램 파일이 하나의 프로젝트에 있고 백그라운드 작업 파일은 동일한 솔루션의 별도 프로젝트에 상주 합니다. 백그라운드 작업을 처음 구현 하는 경우이 지침을 따르는 것이 좋습니다. 아래에서 필요한 내용을 참조 하 여 방문 처리 백그라운드 작업을 만듭니다.

> [!NOTE]
> 다음 코드 조각에서 오류 처리 및 로컬 저장소와 같은 몇 가지 중요 한 기능을 간단 하 게 사용할 수 없습니다. 백그라운드 방문 처리를 강력 하 게 구현 하려면 [샘플 앱](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)을 참조 하세요.


먼저 앱이 백그라운드 작업 권한을 선언 했는지 확인 합니다. `Application/Extensions` *Appxmanifest.xml* 파일의 요소에 다음 확장을 추가 합니다 ( `Extensions` 아직 없는 경우 요소 추가).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

다음으로 백그라운드 작업 클래스의 정의에서 다음 코드를 붙여 넣습니다. `Run`이 백그라운드 작업의 메서드는 개별 메서드에 트리거 세부 정보 (방문 정보 포함)를 전달 하기만 하면 됩니다.

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

`GetVisitReports`이 동일한 클래스의 어딘가에 메서드를 정의 합니다.

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

다음으로, 앱의 주 프로젝트에서이 백그라운드 작업의 등록을 수행 해야 합니다. 일부 사용자 작업으로 호출할 수 있거나 클래스가 활성화 될 때마다 호출 되는 등록 메서드를 만듭니다.

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

이렇게 하면 네임 스페이스에서 호출 된 백그라운드 작업 클래스가 `VisitBackgroundTask` `Tasks` 트리거 형식으로 작업을 수행 `location` 합니다. 

이제 앱에서 방문 처리 백그라운드 작업을 등록할 수 있어야 하며,이 작업은 장치에서 방문 관련 상태 변경을 로깅할 때마다 활성화 해야 합니다. 이 상태 변경 정보를 사용 하 여 수행할 작업을 결정 하려면 백그라운드 작업 클래스에서 논리를 입력 해야 합니다.

## <a name="related-topics"></a>관련 항목
* [Out-of-process 백그라운드 작업 만들기 및 등록](../launch-resume/create-and-register-a-background-task.md)
* [사용자 위치 가져오기](get-location.md)
* [Windows. 장치. 지리적 위치 네임 스페이스](/uwp/api/windows.devices.geolocation)