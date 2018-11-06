---
author: PatrickFarley
title: 사용자 위치 가져오기
description: 사용자의 위치를 찾고 위치 변경에 대응합니다. 사용자 위치에 대한 액세스는 설정 앱의 개인 정보 설정에서 관리합니다. 또한 이 항목에서는 앱에 사용자 위치 액세스 권한이 있는지 확인하는 방법을 보여 줍니다.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.author: pafarley
ms.date: 11/28/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 위치 기능
ms.localizationpriority: medium
ms.openlocfilehash: 2187bafa9fd2b4fdce049f3ef11d4e6766613de3
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6050990"
---
# <a name="get-the-users-location"></a>사용자 위치 가져오기




사용자의 위치를 찾고 위치 변경에 대응합니다. 사용자 위치에 대한 액세스는 설정 앱의 개인 정보 설정에서 관리합니다. 또한 이 항목에서는 앱에 사용자 위치 액세스 권한이 있는지 확인하는 방법을 보여 줍니다.

**팁** 앱에서 사용자의 위치에 액세스하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="enable-the-location-capability"></a>위치 접근 권한 값 사용


1.  **솔루션 탐색기**에서 **package.appxmanifest**를 두 번 클릭하고**접근 권한 값** 탭을 선택합니다.
2.  **기능** 목록에서 **위치** 상자를 선택합니다. 그러면 `location` 디바이스 접근 권한 값이 패키지 매니페스트 파일에 추가됩니다.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>현재 위치 가져오기


이 섹션에서는 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 네임스페이스에서 UPI를 사용하여 사용자의 지리적 위치를 검색하는 방법을 설명합니다.

### <a name="step-1-request-access-to-the-users-location"></a>1단계: 사용자의 위치에 대한 액세스 요청

앱에 거친 위치 접근 권한 값 (참고 참조) 위치에 액세스 하려면 시도 하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 메서드를 사용 하 여 사용자의 위치에 대 한 액세스를 요청 해야 합니다. UI 스레드에서 **RequestAccessAsync** 메서드를 호출해야 하며 앱이 포그라운드에 있어야 합니다. 사용자가 앱에 권한을 부여할 때까지는 앱에서 사용자의 위치 정보에 액세스할 수 없습니다.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 메서드는 사용자에게 위치 액세스 권한을 허용하라고 메시지를 표시합니다. 이 메시지는 앱당 한 번만 표시됩니다. 사용자가 최초로 권한을 부여하거나 거부한 후에는 사용자에게 더 이상 권한을 묻지 않습니다. 메시지가 표시된 후 나중에 사용자가 위치 권한을 변경할 수 있도록 이 항목의 뒷부분에 설명된 것처럼 위치 설정에 대한 링크를 제공하는 것이 좋습니다.

>그러나 참고: 거친 위치 기능을 사용 하면 앱 (시스템 수준 위치 스위치 여전히 있어야 **에서**)는 사용자의 명시적 권한을 하지 않고도 의도적으로 조작 된 (부정확 한) 위치를 얻을 수 있습니다. 앱의 거친 위치를 활용 하는 방법을 알아보려면 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx) 클래스에서 [**AllowFallbackToConsentlessPositions**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Geolocation.Geolocator.AllowFallbackToConsentlessPositions) 메서드를 참조 하세요.

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>2단계: 사용자의 위치를 가져오고 위치 사용 권한 변경에 대해 등록

[**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 메서드는 현재 위치에 대한 일회성 판독을 수행합니다. 사용자 위치에 액세스가 허용될 때만 **switch** 문이 (이전 예제의) **accessStatus**와 함께 사용됩니다. 사용자 위치에 대한 액세스가 허용되는 경우, 코드는 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 생성하고 위치 사용 권한 변경에 등록하며 사용자의 위치를 요청합니다.

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

### <a name="step-3-handle-changes-in-location-permissions"></a>3단계: 위치 사용 권한의 변경 내용 처리

[**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체는 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트를 트리거하여 사용자의 위치 설정이 변경되었음을 나타냅니다. 해당 이벤트는 인수의 **Status** 속성(유형 [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599))을 통해 해당 상태를 전달합니다. 이 메서드는 UI 스레드로부터 호출되지 않고 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 개체가 UI 변경 사항을 호출합니다.

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


이 섹션에서는 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트를 사용하여 일정 기간에 걸쳐 사용자의 위치 업데이트를 수신하는 방법을 설명합니다. 사용자는 언제든지 위치에 대한 액세스를 취소할 수 있으므로 이전 섹션에 표시된 대로 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)를 호출하고 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트를 사용해야 합니다.

이 섹션에서는 위치 접근 권한 값을 이미 사용하고 포그라운드 앱의 UI 스레드에서 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)를 호출한 것으로 가정합니다.

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>1단계: 보고서 간격을 정의하고 위치 업데이트에 등록

이 예제에서는 사용자 위치에 대한 액세스가 허용될 때만 **switch** 문이 이전 예제의 **accessStatus**와 함께 사용됩니다. 사용자 위치에 대한 액세스가 허용된 경우 코드가 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 생성하고 추적 유형을 지정하며 위치 업데이트에 등록합니다.

[**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체는 위치 변경(거리 기반 추적) 또는 시간 변경(정기 기반 추적)을 바탕으로 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트를 트리거할 수 있습니다.

-   거리 기반 추적의 경우 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 속성을 설정하세요.
-   정기 기반 추적의 경우 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541) 속성을 설정하세요.

이 속성이 둘 다 설정되지 않으면 위치가 1초 간격으로 반환됩니다(`ReportInterval = 1000`과 같음). 여기에서 2초(`ReportInterval = 2000`)의 보고서 간격이 사용됩니다.
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

### <a name="step-2-handle-location-updates"></a>2단계: 위치 업데이트 처리

[**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체는 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 구성된 방식에 따라 이벤트를 트리거하여 사용자의 위치가 변경되었거나 시간이 지났음을 나타냅니다. 해당 이벤트는 인수의 **Position** 속성(유형 [**Geoposition**](https://msdn.microsoft.com/library/windows/apps/br225543))을 통해 해당 위치로 전달됩니다. 이 예제에서는 메서드가 UI 스레드로부터 호출되지 않고 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 개체가 UI 변경 사항을 호출합니다.

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


위치 개인 정보 설정에 따라 앱이 사용자의 위치에 액세스할 수 없는 경우 **설정** 앱에서 **위치 개인 정보 설정**에 대한 편리한 링크를 제공하는 것이 좋습니다. 이 예제에서는 하이퍼링크 컨트롤이 `ms-settings:privacy-location` URI로 이동하는 데 사용됩니다.

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

또는 앱이 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 호출하여 코드에서 **설정** 앱을 실행할 수 있습니다. 자세한 내용은 [Windows 설정 앱 실행](https://msdn.microsoft.com/library/windows/apps/mt228342)을 참조하세요.

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="troubleshoot-your-app"></a>앱 문제 해결


앱이 사용자 위치에 액세스하려면 먼저 디바이스에서 **위치**를 사용하도록 설정해야 합니다. **설정** 앱에서 다음 **위치 개인정보 설정** 이 켜져 있는지 확인합니다.

-   **이 장치에 대 한 위치** 가 켜 **집니다** (Windows10 Mobile에는 해당 되지 않음)
-   위치 서비스 설정 **위치**가 **켜짐** 상태임
-   **사용자의 위치를 사용할 수 있는 앱 선택**에서 앱이 **on** 상태임

## <a name="related-topics"></a>관련 항목

* [UWP 지리적 위치 샘플](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [지오펜스에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [위치 인식 앱에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/hh465148)
