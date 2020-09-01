---
title: 사용자 위치 가져오기
description: 사용자의 위치를 찾고 위치 변경에 응답 합니다. 사용자의 위치에 대 한 액세스는 설정 앱의 개인 정보 설정에 의해 관리 됩니다. 또한이 항목에서는 앱에 사용자의 위치에 액세스할 수 있는 권한이 있는지 확인 하는 방법을 보여 줍니다.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.date: 11/28/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 위치 기능
ms.localizationpriority: medium
ms.openlocfilehash: 79c34af48cf1b2d860d2a170fd642ef05945c15d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158717"
---
# <a name="get-the-users-location"></a>사용자 위치 가져오기




사용자의 위치를 찾고 위치 변경에 응답 합니다. 사용자의 위치에 대 한 액세스는 설정 앱의 개인 정보 설정에 의해 관리 됩니다. 또한이 항목에서는 앱에 사용자의 위치에 액세스할 수 있는 권한이 있는지 확인 하는 방법을 보여 줍니다.

**팁** 앱에서 사용자의 위치에 액세스 하는 방법에 대 한 자세한 내용은 GitHub의 [Windows 유니버설 샘플 리포지토리](https://github.com/Microsoft/Windows-universal-samples) 에서 다음 샘플을 다운로드 하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>위치 기능 사용


1.  **솔루션 탐색기**에서 **appxmanifest.xml** 를 두 번 클릭 하 고 **기능** 탭을 선택 합니다.
2.  **기능** 목록에서 **위치**확인란을 선택 합니다. 그러면 `location` 패키지 매니페스트 파일에 장치 기능이 추가 됩니다.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>현재 위치 가져오기


이 섹션에서는 [**Windows. Devices**](/uwp/api/Windows.Devices.Geolocation) 네임 스페이스의 api를 사용 하 여 사용자의 지리적 위치를 검색 하는 방법을 설명 합니다.

### <a name="step-1-request-access-to-the-users-location"></a>1 단계: 사용자의 위치에 대 한 액세스 요청

앱에 정교 하지 않은 위치 기능이 있는 경우 (참고 참조), 위치에 액세스 하기 전에 [**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 메서드를 사용 하 여 사용자의 위치에 대 한 액세스를 요청 해야 합니다. UI 스레드에서 **Requestaccessasync** 메서드를 호출 해야 하며 응용 프로그램은 전경에 있어야 합니다. 사용자가 앱에 권한을 부여할 때까지 앱에서 사용자의 위치 정보에 액세스할 수 없습니다.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



[**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 메서드는 사용자에 게 해당 위치에 액세스할 수 있는 권한을 요청 하 라는 메시지를 표시 합니다. 사용자에 게 한 번만 메시지가 표시 됩니다 (앱 당). 처음으로 사용 권한을 부여 하거나 거부 한 후에는이 메서드는 더 이상 사용자에 게 사용 권한을 요청 하지 않습니다. 메시지를 표시 한 후 사용자가 위치를 변경할 수 있도록 하려면이 항목의 뒷부분에 설명 된 대로 위치 설정에 대 한 링크를 제공 하는 것이 좋습니다.

>참고: 거칠게 배치 기능을 사용 하면 앱에서 사용자의 명시적 사용 권한을 가져오지 않고 의도적으로 난독 처리 된 (부정확 한) 위치를 가져올 수 있습니다. 그러나 시스템 전체 위치 스위치는 여전히 **켜져**있어야 합니다. 앱에서 거칠게 위치를 활용 하는 방법을 알아보려면 [**Geolocator**](/uwp/api/windows.devices.geolocation.geolocator) 클래스에서 [**AllowFallbackToConsentlessPositions**](/uwp/api/windows.devices.geolocation.geolocator.allowfallbacktoconsentlesspositions) 메서드를 참조 하세요.

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>2 단계: 사용자의 위치를 가져오고 위치 권한에서 변경 내용 등록

[**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 메서드는 현재 위치를 한 번만 읽습니다. 여기서는 **스위치** 문이 사용자의 위치에 대 한 액세스가 허용 되는 경우에만 작동 하는 **accessstatus** (이전 예제의 경우)와 함께 사용 됩니다. 사용자의 위치에 대 한 액세스를 허용 하는 경우 코드는 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 만들고, 위치 권한 변경을 등록 하 고, 사용자의 위치를 요청 합니다.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### <a name="step-3-handle-changes-in-location-permissions"></a>3 단계: 위치 권한 변경 내용 처리

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체는 [**statuschanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트를 트리거하여 사용자의 위치 설정이 변경 되었음을 표시 합니다. 이 이벤트는 인수의 **상태** 속성 ( [**positionstatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus)형식)을 통해 해당 상태를 전달 합니다. 이 메서드는 UI 스레드에서 호출 되지 않으며 [**디스패처**](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체가 ui 변경을 호출 합니다.

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix.
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## <a name="respond-to-location-updates"></a>위치 업데이트에 응답


이 섹션에서는 [**Positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트를 사용 하 여 일정 기간 동안 사용자 위치의 업데이트를 받는 방법에 대해 설명 합니다. 사용자는 언제 든 지 위치에 대 한 액세스 권한을 해지할 수 있으므로 이전 섹션에 표시 된 대로 [**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 를 호출 하 고 [**statuschanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트를 사용 하는 것이 중요 합니다.

이 섹션에서는 사용자가 이미 위치 기능을 사용 하도록 설정 하 고, 포그라운드 앱의 UI 스레드에서 [**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 를 호출 했다고 가정 합니다.

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>1 단계: 보고서 간격 정의 및 위치 업데이트 등록

이 예제에서 **switch** 문은 이전 예제의 **accessstatus** 와 함께 사용 되어 사용자의 위치에 대 한 액세스가 허용 되는 경우에만 작동 합니다. 사용자 위치에 대한 액세스가 허용된 경우 코드가 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 생성하고 추적 유형을 지정하며 위치 업데이트에 등록합니다.

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체는 위치 변경 (거리 기반 추적) 또는 시간 변경 (주기적 기반 추적)을 기반으로 [**positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트를 트리거할 수 있습니다.

-   거리 기반 추적의 경우 [**Movementthreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) 속성을 설정 합니다.
-   정기적 기반 추적의 경우 [**Reportinterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) 속성을 설정 합니다.

두 속성이 모두 설정 되지 않은 경우에는 1 초 마다 위치가 반환 됩니다 `ReportInterval = 1000` . 여기에서 2 초 ( `ReportInterval = 2000` ) 보고서 간격이 사용 됩니다.
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### <a name="step-2-handle-location-updates"></a>2 단계: 위치 업데이트 처리

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체는 사용자가 구성 된 방식에 따라 사용자 위치가 변경 되었거나 시간이 경과 했음을 나타내는 [**positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트를 트리거합니다. 이 이벤트는 인수의 **Position** 속성 ( [**Geoposition**](/uwp/api/Windows.Devices.Geolocation.Geoposition)형식)을 통해 해당 위치를 전달 합니다. 이 예제에서는 UI 스레드에서 메서드가 호출 되지 않으며 [**디스패처**](/uwp/api/Windows.UI.Core.CoreDispatcher) 개체가 ui 변경을 호출 합니다.

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## <a name="change-the-location-privacy-settings"></a>위치 개인 정보 설정 변경


위치 개인 정보 설정을 통해 앱에서 사용자의 위치에 액세스할 수 없는 경우 **설정** 앱에서 **위치 개인 정보 설정** 에 대 한 편리한 링크를 제공 하는 것이 좋습니다. 이 예제에서 Hyperlink 컨트롤은 URI로 이동 하는 데 사용 됩니다 `ms-settings:privacy-location` .

```xml
<!--Set Visibility to Visible when access to location is denied -->  
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

## <a name="troubleshoot-your-app"></a>앱 문제 해결


앱에서 사용자의 위치에 액세스 하기 전에 장치에서 **위치** 를 사용 하도록 설정 해야 합니다. **설정** 앱에서 다음 **위치의 개인 정보 설정이** 켜져 있는지 확인 합니다.

-   **이 장치의 위치** ... **켜 짐 (** Windows 10 Mobile에는 적용 되지 않음)
-   위치 서비스 설정 ( **위치**) **이 설정 되어** 있습니다.
-   **사용자의 위치를 사용할 수 있는 앱 선택**에서 앱이 **켜기** 로 설정 됩니다.

## <a name="related-topics"></a>관련 항목

* [UWP 지리적 위치 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [지 오 펜싱에 대 한 디자인 지침](./guidelines-for-geofencing.md)
* [위치 인식 앱에 대한 디자인 지침](./guidelines-and-checklist-for-detecting-location.md)