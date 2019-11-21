---
title: 지도 및 위치 개요
description: 이 섹션에서는 지도 표시, 지도 서비스 사용, 위치 찾기 및 앱에서 지오펜스 설정 등의 방법을 설명합니다. 이 섹션에서는 또한 특정 지도, 경로 또는 턴바이턴 길 찾기 집합으로 Windows 지도 앱을 시작하는 방법을 보여 줍니다.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 지도, 위치, 지도 서비스
ms.localizationpriority: medium
ms.openlocfilehash: 6a27eeeb9aa7349e532dcd76e5b7a7176ac20c08
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259342"
---
# <a name="maps-and-location-overview"></a>지도 및 위치 개요




이 섹션에서는 지도 표시, 지도 서비스 사용, 위치 찾기 및 앱에서 지오펜스 설정 등의 방법을 설명합니다. 이 섹션에서는 또한 특정 지도, 경로 또는 턴바이턴 길 찾기 집합으로 Windows 지도 앱을 시작하는 방법을 보여 줍니다.

> [!TIP]
> 앱에서 지도와 위치를 사용하는 방법에 대해 자세히 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](https://github.com/Microsoft/Windows-universal-samples)에서 다음 샘플을 다운로드하세요.
-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
-   [UWP 지리적 위치 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)

 

## <a name="display-maps"></a>지도 표시


[  **Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) 네임스페이스의 API를 사용하여 앱에서 2D, 3D 또는 Streetside 뷰로 지도를 표시합니다. 고정핀, 이미지, 셰이프 또는 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 표시할 수 있습니다. 또한 바둑판식 이미지를 오버레이하거나 지도 이미지를 모두 함께 대체할 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [지도 인증 키 요청](authentication-key.md) | [  **Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 네임스페이스에서 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 지도 서비스를 사용하려면 먼저 앱을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 문서에서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하고 이를 앱에 추가하는 방법을 설명합니다. |
| [2D, 3D 및 Streetside 뷰가 있는 지도 표시](display-maps.md) | [  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 클래스를 사용하여 앱에서 사용자 지정 가능한 지도를 표시하세요. 이 항목에서는 3D 위성뷰 및 Streetside 뷰도 소개합니다. |
| [지도에 POI(관심 지점) 표시](display-poi.md) | 고정핀, 이미지, 셰이프 및 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 추가하는 방법을 알아봅니다. |
| [지도에 바둑판식 이미지 오버레이](overlay-tiled-images.md) | 타일 소스를 사용하여 지도에 타사 또는 사용자 지정 바둑판식 이미지를 오버레이합니다. 타일 소스를 사용하여 특수 정보(예제: 날씨 데이터, 인구 데이터, 지진 데이터 등)를 오버레이하거나 기본 지도를 전체적으로 바꿉니다. |



## <a name="access-map-services"></a>지도 서비스 액세스

[  **Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 네임스페이스의 API를 사용하여 경로, 길 찾기 및 지오코딩 접근 권한 값을 앱에 추가합니다.

| 항목 | 설명 |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [지도 인증 키 요청](authentication-key.md) | [  **Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 네임스페이스에서 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 지도 서비스를 사용하려면 먼저 앱을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 문서에서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하고 이를 앱에 추가하는 방법을 설명합니다. |
| [지도에 POI(관심 지점) 표시](display-poi.md) | 고정핀, 이미지, 셰이프 및 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 추가하는 방법을 알아봅니다. |
| [경로 및 길 찾기 표시](routes-and-directions.md) | 경로 및 길 찾기를 요청하고 앱에 표시합니다. |
| [지오코딩 및 역방향 지오코딩 수행](geocoding.md) | [  **Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 네임스페이스에 있는 [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스의 메서드를 호출하여 주소를 지리적 위치로 변환하고(지오코딩) 지리적 위치를 주소로 변환합니다(리버스 지오코딩). |
| [오프라인 사용을 위해 맵 패키지 찾기 및 다운로드](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| 과거에는 사용자를 Settings 앱으로 안내하여 오프라인 Maps를 다운로드해야 했습니다. 이제는 [Windows.Services.Maps.OfflineMaps](https://docs.microsoft.com/en-us/uwp/api/windows.services.maps.offlinemaps) 네임스페이스의 클래스를 사용하여 지정된 영역([Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](https://docs.microsoft.com/en-us/uwp/api/windows.devices.geolocation.geoboundingbox) 등에 따라)에서 다운로드한 패키지를 찾을 수 있습니다. <br> 또한 맵 패키지의 다운로드 상태를 확인하고 수신 대기할 수 있을 뿐만 아니라 사용자가 앱을 종료하지 않고도 다운로드를 시작할 수도 있습니다. <br> 참조 콘텐츠와 [UWP(유니버설 Windows 플랫폼) 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 모두에서 이 작업을 수행하는 방법에 대한 예제를 찾을 수 있습니다.

## <a name="get-the-users-location"></a>사용자 위치 가져오기

[  **Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) 네임스페이스의 API를 사용하여 사용자의 현재 위치를 가져오고 앱에서 위치가 변경된 경우 알림을 받습니다. 이러한 API 멤버는 지도 API의 매개 변수에서 자주 사용됩니다. [  **Windows.Devices.Geolocation.Geofencing**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing) 네임스페이스의 API를 사용하면 사용자가 지오펜스(미리 정의된 지리적 영역)를 입력하거나 지오펜스가 있는 경우 앱에 알립니다.

| 항목 | 설명 |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [지도 인증 키 요청](authentication-key.md) | [  **Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 네임스페이스에서 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 지도 서비스를 사용하려면 먼저 앱을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 문서에서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하고 이를 앱에 추가하는 방법을 설명합니다. |
| [위치 인식 앱에 대한 디자인 지침](guidelines-and-checklist-for-detecting-location.md) | 사용자 위치에 액세스해야 하는 앱에 대한 성능 지침입니다. |
| [사용자 위치 가져오기](get-location.md) | 사용자의 위치에 대한 액세스 권한을 받은 다음 검색합니다. | 
| [방문 추적 사용에 대한 지침](guidelines-for-visits.md) | 더 실질적인 추적을 위해 강력한 방문 추적 기능을 사용하는 방법에 대해 알아봅니다. |
| [지오펜스에 대한 디자인 지침](guidelines-for-geofencing.md) | 지오펜스 기능을 활용하는 앱에 대한 성능 지침입니다. |
| [지오펜스 설정](set-up-a-geofence.md) | 앱에서 지오펜스를 설정하고 포그라운드 및 백그라운드에서 알림을 처리하는 방법을 알아봅니다. |

## <a name="launch-the-windows-maps-app"></a>Windows 지도 앱 실행

앱에서 Windows 지도 앱을 시작하여 여기에 표시된 것처럼 특정 지도 및 턴바이턴 길 찾기를 표시할 수 있습니다. 사용자 고유의 앱에서 지도 기능을 직접 제공하는 대신 Windows 지도 앱을 사용하여 해당 기능을 제공하는 것이 좋습니다. 자세한 내용은 [Windows 지도 앱 실행](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app)을 참조하세요.

![Windows 지도 앱의 예](images/mapnyc.png)

## <a name="related-topics"></a>관련 항목

* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [UWP 지리적 위치 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [현재 위치 가져오기](get-location.md)
* [위치 인식 앱에 대한 디자인 지침](guidelines-and-checklist-for-detecting-location.md)
* [지도에 대한 디자인 지침](controls-map.md)
* [개인 정보 인식 앱에 대한 디자인 지침](https://docs.microsoft.com/windows/uwp/security/index)
* [빌드 2015 비디오: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
