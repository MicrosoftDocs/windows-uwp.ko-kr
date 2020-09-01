---
description: 지 오 펜싱를 사용 하 여 앱에서 지리적 컨텍스트 환경을 제공 하는 방법에 대 한 지침 및 모범 사례를 참조 하세요.
title: 지 오 펜싱 apps에 대 한 지침
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 지 오 펜싱
ms.localizationpriority: medium
ms.openlocfilehash: 76cbedaef76ff1403e1d6718c96303da6ad2ee67
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162537"
---
# <a name="guidelines-for-geofencing-apps"></a>지 오 펜싱 apps에 대 한 지침




**중요 API**

-   [**지 오 클래스 (XAML)**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)
-   [**Geolocator 클래스 (XAML)**](/uwp/api/Windows.Devices.Geolocation.Geolocator)

앱에서 [**지 오 펜싱**](/uwp/api/Windows.Devices.Geolocation.Geofencing) 에 대 한 다음 모범 사례를 따르세요.

## <a name="recommendations"></a>권장 사항


-   응용 프로그램에서 인터넷에 액세스 해야 하는 경우에는 지 [**오 이벤트가 발생 하는**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 경우에는 인터넷에 액세스할 수 있는지 확인 합니다.
    -   앱이 현재 인터넷에 연결 되어 있지 않은 경우에는 지 오를 설정 하기 전에 사용자에 게 인터넷 연결을 요청할 수 있습니다.
    -   인터넷 액세스를 사용할 수 없는 경우 지 오 펜싱 위치 확인에 필요한 기능을 사용 하지 마십시오.
-   지 오 이벤트에서 [**입력**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) 또는 **종료** 상태가 변경 된 것으로 표시 될 때 타임 스탬프 및 현재 위치를 확인 하 여 지 오 펜싱 알림의 관련성을 확인 합니다. 자세한 내용은 아래의 **타임 스탬프 및 현재 위치 확인** 을 참조 하세요.
자세한 내용은 아래 (#timestamp)를 참조 하세요.
-   장치에서 위치 정보에 액세스할 수 없는 경우를 관리 하 고 필요한 경우 사용자에 게 알릴 예외를 만듭니다. 사용 권한이 꺼져 있거나, 장치에 GPS 라디오가 없거나, GPS 신호가 차단 되었거나, Wi-fi 신호가 충분히 강력 하지 않아서 위치 정보를 사용할 수 없습니다.
-   일반적으로 포그라운드 및 백그라운드에서 동시에 발생 하는 지 오 이벤트를 수신할 필요가 없습니다. 그러나 앱이 포그라운드 및 백그라운드에서 지 오 트 이벤트를 수신 해야 하는 경우 다음을 수행 합니다.

    -   [**Readreports**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports) 메서드를 호출 하 여 이벤트가 발생 했는지 확인 합니다.
    -   앱이 사용자에 게 표시 되지 않는 경우 포그라운드 이벤트 수신기의 등록을 취소 하 고 다시 표시 되는 경우 다시 등록 합니다.

    코드 예제 및 자세한 내용은 [배경 및 전경 수신기](#background-and-foreground-listeners) 를 참조 하세요.

-   앱 당 1000 지역 구분 이상 사용 하지 마세요. 시스템은 실제로 앱 당 수천 개의 지역 구분을 지원 하므로, 1000 개 이하로 사용 하 여 앱의 메모리 사용량을 줄이는 데 도움이 되는 좋은 앱 성능을 유지할 수 있습니다.
-   50 미터 미만의 반지름이 있는 지 오를 만들지 마세요. 앱이 작은 반지름이 있는 지 오를 사용 해야 하는 경우 최상의 성능을 위해 GPS 라디오가 있는 장치에서 앱을 사용 하도록 사용자에 게 알립니다.

## <a name="additional-usage-guidance"></a>추가 사용법 지침

### <a name="checking-the-time-stamp-and-current-location"></a>타임 스탬프 및 현재 위치를 확인 하는 중

이벤트에서 [**입력**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) 또는 **종료** 됨 상태가 변경 되는 경우 이벤트의 타임 스탬프와 현재 위치를 모두 확인 합니다. 시스템에서 백그라운드 작업을 시작 하는 데 충분 한 리소스를 보유 하 고 있지 않거나, 사용자가 알림을 받지 않거나, 장치가 대기 모드 (Windows)에 있는 경우와 같은 다양 한 요소는 이벤트가 실제로 사용자에 의해 처리 될 때 영향을 줄 수 있습니다. 예를 들어 다음 시퀀스가 발생할 수 있습니다.

-   앱에서 오 오 트를 만들고 enter 및 exit 이벤트의 지 오를 모니터링 합니다.
-   사용자가 내 장치를 이동 하 여 이동 하면 enter 이벤트가 트리거됩니다.
-   앱이 사용자에 게 현재 지 오의 내부에 있다는 알림을 보냅니다.
-   사용자가 사용 중이 고 10 분 후에 알림 메시지를 표시 하지 않습니다.
-   10 분이 지연 되는 동안에는 사용자가 지 오 펜스 외부에서 다시 이동 했습니다.

타임 스탬프에서 작업이 과거에 발생 했음을 알 수 있습니다. 현재 위치에서 사용자가 현재 지 오의 외부에 있는 것을 볼 수 있습니다. 앱의 기능에 따라이 이벤트를 필터링 할 수 있습니다.

### <a name="background-and-foreground-listeners"></a>백그라운드 및 포그라운드 수신기

일반적으로 앱은 포그라운드 및 백그라운드 작업에서 동시에 지 [**오**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 트 이벤트를 수신할 필요가 없습니다. 두 가지 모두 필요할 수 있는 사례를 처리 하는 cleanest 메서드는 백그라운드 작업에서 알림을 처리할 수 있도록 하는 것입니다. 포그라운드 및 백그라운드 지 오 수신기를 모두 설정 하는 경우 먼저 트리거되는 보장이 없으므로 항상 [**Readreports**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports) 메서드를 호출 하 여 이벤트가 발생 했는지 확인 해야 합니다.

포그라운드 및 백그라운드 지 오 수신기를 모두 설정한 경우 앱이 사용자에 게 표시 되지 않을 때마다 포그라운드 이벤트 수신기의 등록을 취소 하 고 다시 표시 될 때 앱을 다시 등록 해야 합니다. 표시 이벤트를 등록 하는 예제 코드는 다음과 같습니다.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

표시 유형이 변경 되 면 여기에 표시 된 대로 포그라운드 이벤트 처리기를 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

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

### <a name="sizing-your-geofences"></a>지역 구분 크기 조정

GPS에서 가장 정확한 위치 정보를 제공할 수 있지만 지 오 펜싱는 Wi-fi 또는 다른 위치 센서를 사용 하 여 사용자의 현재 위치를 확인할 수도 있습니다. 그러나 이러한 다른 방법을 사용 하 여 만들 수 있는 지역 구분의 크기에 영향을 줄 수 있습니다. 정확도 수준이 낮으면 작은 지역 구분를 만드는 것은 유용 하지 않습니다. 일반적으로 50 미터 보다 작은 반지름이 있는 지 오를 만들지 않는 것이 좋습니다. 또한 지 오 펜스 백그라운드 작업은 Windows 에서만 정기적으로 실행 됩니다. 작은 지 오를 사용 하는 경우 [**Enter**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) 또는 **Exit** 이벤트를 완전히 누락 시킬 수 있습니다.

앱이 작은 반지름이 있는 지 오를 사용 해야 하는 경우 최상의 성능을 위해 GPS 라디오가 있는 장치에서 앱을 사용 하도록 사용자에 게 알립니다.

## <a name="related-topics"></a>관련 항목


* [지오펜스 설정](./set-up-a-geofence.md)
* [현재 위치 가져오기](./get-location.md)
* [UWP 위치 샘플 (지리적 위치)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 