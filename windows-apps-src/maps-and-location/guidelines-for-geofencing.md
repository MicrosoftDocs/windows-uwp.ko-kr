---
author: PatrickFarley
Description: Follow these best practices for geofencing in your app.
title: 지오펜싱 앱에 대한 지침
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 지오펜싱
ms.localizationpriority: medium
ms.openlocfilehash: 86104f00ed0189290fd0cd718042573d9d592cc3
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5878692"
---
# <a name="guidelines-for-geofencing-apps"></a>지오펜싱 앱에 대한 지침




**중요 API**

-   [**지오펜스 클래스(XAML)**](https://msdn.microsoft.com/library/windows/apps/dn263587)
-   [**Geolocator 클래스(XAML)**](https://msdn.microsoft.com/library/windows/apps/br225534)

앱에서 [**지오펜스**](https://msdn.microsoft.com/library/windows/apps/dn263744)에 대한 다음 모범 사례를 따릅니다.

## <a name="recommendations"></a>권장 사항


-   [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 이벤트가 발생할 때 앱이 인터넷에 액세스해야 하는 경우 지오펜스를 만들기 전에 인터넷 액세스를 확인합니다.
    -   앱이 현재 인터넷에 액세스할 수 없으면 지오펜스를 설정하기 전에 인터넷에 연결하라는 메시지를 표시할 수 있습니다.
    -   인터넷에 액세스할 수 없는 경우 지오펜스 위치 확인에 필요한 전원 사용을 방지합니다.
-   지오펜스 이벤트에 [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) 또는 **Exited** 상태 변경이 표시되면 타임스탬프 및 현재 위치를 확인하여 지오펜스 알림의 관련성을 확인합니다. 자세한 내용은 아래에서 **타임스탬프 및 현재 위치 확인**을 참조하세요.
(#timestamp) 자세한 내용 아래를 참조하세요.
-   장치에서 위치 정보에 액세스할 수 없는 경우를 관리하는 예외를 만들고 필요한 경우 사용자에게 알립니다. 사용 권한이 해제되거나, 장치에 GPS 송수신 장치가 없거나, GPS 신호가 차단되거나, Wi-Fi 신호가 약하기 때문에 위치 정보를 사용할 수 없습니다.
-   일반적으로 포그라운드와 백그라운드에서 동시에 지오펜스 이벤트를 수신 대기할 필요가 없습니다. 그러나 앱에 포그라운드와 백그라운드에서 지오펜스 이벤트를 수신 대기해야 하는 경우 다음을 수행합니다.

    -   [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) 메서드를 호출하여 이벤트가 발생했는지 확인합니다.
    -   앱이 사용자에게 표시되지 않으면 포그라운드 이벤트 수신기를 등록 취소하고 다시 표시될 때 다시 등록합니다.

    코드 예제 및 자세한 내용은 [백그라운드 및 포그라운드 수신기](#background-and-foreground-listeners)를 참조하세요.

-   지오펜스를 앱당 1000개 넘게 사용하지 마세요. 시스템은 실제로 앱당 수천 개 지오펜스를 지원하지만, 1000개 이하로 사용하면 앱 메모리 사용이 감소하므로 좋은 앱 성능을 유지할 수 있습니다.
-   반지름이 50m보다 작은 지오펜스를 만들지 마세요. 앱에서 반지름이 작은 지오펜스를 사용해야 하는 경우 GPS 송수신 장치가 있는 장치에서 앱을 사용하여 최상의 성능을 보장하는 것이 좋습니다.

## <a name="additional-usage-guidance"></a>추가 사용법 지침

### <a name="checking-the-time-stamp-and-current-location"></a>타임스탬프 및 현재 위치 확인

이벤트가 [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) 또는 **Exited** 상태 변경을 나타내는 경우 이벤트의 타임스탬프와 현재 위치를 모두 확인하세요. 사용자가 실제로 이벤트를 처리하는 시기는 리소스가 부족하여 백그라운드 작업을 시작할 수 없는 시스템, 알림을 확인하지 못하는 사용자, 대기 상태인 Windows의 장치 등 여러 요소의 영향을 받을 수 있습니다. 예를 들면 다음 순서가 발생할 수 있습니다.

-   앱이 지오펜스를 만들고 enter 및 exit 이벤트에 대해 지오펜스를 모니터링합니다.
-   사용자가 장치를 지오펜스 안으로 이동하면 enter 이벤트가 트리거됩니다.
-   앱이 현재 지오펜스 안에 있다는 알림을 사용자에게 보냅니다.
-   사용자가 바빠서 10분이 지나도록 알림을 확인하지 못합니다.
-   10분이 지난 후 사용자가 지오펜스 외부에서 다시 돌아옵니다.

타임스탬프를 통해, 과거에 동작이 발생한 것을 알 수 있습니다. 현재의 위치에서, 사용자가 이제 지오펜스 외부에서 다시 돌아온 것을 알 수 있습니다. 앱의 기능에 따라 이 이벤트를 필터링할 수 있습니다.

### <a name="background-and-foreground-listeners"></a>백그라운드 및 포그라운드 수신기

일반적으로 앱은 포그라운드와 백그라운드 작업에서 동시에 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 이벤트를 수신 대기할 필요가 없습니다. 두 가지가 모두 필요할 수 있는 상황을 처리하기 위한 가장 분명한 방법은 백그라운드 작업이 알림을 처리하도록 하는 것입니다. 포그라운드와 백그라운드 지오펜스 수신기를 모두 설정하는 경우 어떤 것이 먼저 트리거될지 알 수 없으므로, 이벤트가 발생했는지 알아보려면 항상 [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) 메서드를 호출해야 합니다.

포그라운드와 백그라운드 지오펜스 수신기를 모두 설정한 경우 앱이 사용자에게 표시되지 않을 때마다 포그라운드 이벤트 수신기를 등록 취소하고 앱이 다시 표시될 때 앱을 다시 등록해야 합니다. 다음은 가시성 이벤트를 등록하는 몇 가지 예제 코드입니다.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

가시성이 변경되면, 여기에 보이는 것처럼 포그라운드 이벤트 처리기의 사용 여부를 설정할 수 있습니다.

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>지오펜스 크기 조정

GPS가 가장 정확한 위치 정보를 제공할 수 있지만, 지오펜싱은 또한 Wi-Fi 또는 기타 위치 센서를 사용해 사용자의 현재 위치를 확인할 수 있습니다. 그러나 이러한 방법을 사용하는 경우, 만들 수 있는 지오펜스의 크기에 영향이 미칠 수 있습니다. 정확도 수준이 낮은 경우에는 작은 지오펜스 만들면 도움이 되지 않습니다. 일반적으로 반지름이 50m보다 작은 지오펜스를 만들지 않는 것이 좋습니다. 또한 지오펜스 백그라운드 작업만 Windows에서 정기적으로 실행됩니다. 작은 지오펜스를 사용하는 경우 [**Enter**](https://msdn.microsoft.com/library/windows/apps/dn263660) 또는 **Exit** 이벤트가 완전히 누락될 수 있습니다.

앱에서 반지름이 작은 지오펜스를 사용해야 하는 경우 GPS 송수신 장치가 있는 장치에서 앱을 사용하여 최상의 성능을 보장하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목


* [지오펜스 설정](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [현재 위치 가져오기](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [UWP 위치 샘플(지리적 위치)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
