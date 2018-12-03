---
title: 지도 및 위치 개요
description: 이 섹션에서는 지도 표시, 지도 서비스 사용, 위치 찾기 및 앱에서 지오펜스 설정 등의 방법을 설명합니다. 이 섹션에서는 또한 특정 지도, 경로 또는 턴바이턴 길 찾기 집합으로 Windows 지도 앱을 시작하는 방법을 보여 줍니다.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 지도, 위치, 지도 서비스
ms.localizationpriority: medium
ms.openlocfilehash: aea553a46357a26028848db5ff0e9b5debbeae56
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458753"
---
# <a name="maps-and-location-overview"></a>지도 및 위치 개요




이 섹션에서는 지도 표시, 지도 서비스 사용, 위치 찾기 및 앱에서 지오펜스 설정 등의 방법을 설명합니다. 이 섹션에서는 또한 특정 지도, 경로 또는 턴바이턴 길 찾기 집합으로 Windows 지도 앱을 시작하는 방법을 보여 줍니다.

> [!TIP]
> 앱에서 지도 및 위치를 사용 하는 방법에 대 한 자세한 내용은 GitHub의 [Windows 유니버설 샘플 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979) 에서 다음 샘플을 다운로드 합니다.
-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [UWP 지리적 위치 샘플](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## <a name="display-maps"></a>지도 표시


[**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 네임스페이스의 API를 사용하여 앱에서 2D, 3D 또는 Streetside 뷰로 지도를 표시합니다. 고정핀, 이미지, 셰이프 또는 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 표시할 수 있습니다. 또한 바둑판식 이미지를 오버레이하거나 지도 이미지를 모두 함께 대체할 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [지도 인증 키 요청](authentication-key.md) | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 및 지도 서비스를 사용하려면 먼저 앱을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 문서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하여 앱에 추가하는 방법에 대해 설명합니다. |
| [2D, 3D, Streetside 뷰로 지도 표시](display-maps.md) | [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 클래스를 사용하여 앱에서 사용자 지정 가능한 지도를 표시하세요. 이 항목에서는 3D 위성뷰 및 Streetside 뷰도 소개합니다. |
| [지도에 POI(관심 지점) 표시](display-poi.md) | 고정핀, 이미지, 셰이프 및 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 추가하는 방법을 알아봅니다. |
| [지도에서 바둑판식 이미지 오버레이](overlay-tiled-images.md) | 타일 소스를 사용하여 지도에 타사 또는 사용자 지정 바둑판식 이미지를 오버레이합니다. 타일 소스를 사용하여 특수 정보(예제: 날씨 데이터, 인구 데이터, 지진 데이터 등)를 오버레이하거나 기본 지도를 전체적으로 바꿉니다. |



## <a name="access-map-services"></a>지도 서비스 액세스

[**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스의 API를 사용하여 경로, 길 찾기 및 지오코딩 접근 권한 값을 앱에 추가합니다.

| 항목 | 설명 |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [지도 인증 키 요청](authentication-key.md) | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 및 지도 서비스를 사용하려면 먼저 앱을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 문서에서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하고 이를 앱에 추가하는 방법을 설명합니다. |
| [지도에 POI(관심 지점) 표시](display-poi.md) | 고정핀, 이미지, 셰이프 및 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 추가하는 방법을 알아봅니다. |
| [경로 및 길 찾기 표시](routes-and-directions.md) | 경로 및 길 찾기를 요청하고 앱에 표시합니다. |
| [지오코딩 및 리버스 지오코딩 수행](geocoding.md) | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스에 있는 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 메서드를 호출하여 주소를 지리적 위치로 변환하고(지오코딩) 지리적 위치를 주소로 변환합니다(리버스 지오코딩). |
| [찾기 및 오프 라인 사용을 위해 맵 패키지를 다운로드 합니다.](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| 이전에 앱 설정 앱에 사용자를 오프 라인 지도 다운로드를 연결 해야 했습니다. 이제 ( [Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](https://docs.microsoft.com/en-us/uwp/api/windows.devices.geolocation.geoboundingbox)등 기반)는 지정 된 영역에 다운로드 한 패키지를 찾으려면 [Windows.Services.Maps.OfflineMaps](https://docs.microsoft.com/en-us/uwp/api/windows.services.maps.offlinemaps) 네임 스페이스의 클래스를 사용할 수 있습니다. <br> 있습니다도 확인 및 맵 패키지 다운로드 한 상태에 대 한 수신 뿐 아니라 수 없이 앱을 다운로드를 시작 합니다. <br> 참조 콘텐츠 및 [유니버설 Windows 플랫폼 (UWP) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)모두에서이 작업을 수행 하는 방법의 예제를 찾을 수 있습니다.

## <a name="get-the-users-location"></a>사용자 위치 가져오기

[**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 네임스페이스의 API를 사용하여 사용자의 현재 위치를 가져오고 앱에서 위치가 변경된 경우 알림을 받습니다. 이러한 API 멤버는 지도 API의 매개 변수에서 자주 사용됩니다. [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744) 네임스페이스의 API를 사용하면 사용자가 지오펜스(미리 정의된 지리적 영역)를 입력하거나 지오펜스가 있는 경우 앱에 알립니다.

| 항목 | 설명 |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [지도 인증 키 요청](authentication-key.md) | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 및 지도 서비스를 사용하려면 먼저 앱을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 문서에서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하고 이를 앱에 추가하는 방법을 설명합니다. |
| [위치 인식 앱에 대한 디자인 지침](guidelines-and-checklist-for-detecting-location.md) | 사용자 위치에 액세스해야 하는 앱에 대한 성능 지침입니다. |
| [사용자 위치 가져오기](get-location.md) | 사용자의 위치에 대한 액세스 권한을 받은 다음 검색합니다. | 
| [방문 추적 사용에 대한 지침](guidelines-for-visits.md) | 더욱 효과적인 추적을 위한 강력한 방문 추적 기능을 사용하는 방법에 대해 알아보세요. |
| [지오펜싱에 대한 디자인 지침](guidelines-for-geofencing.md) | 지 오 펜스 기능을 활용 하는 앱에 대 한 성능 지침입니다. |
| [지오펜스 설정](set-up-a-geofence.md) | 앱에서 지오펜스를 설정하고 포그라운드 및 백그라운드에서 알림을 처리하는 방법을 알아봅니다. |

## <a name="launch-the-windows-maps-app"></a>Windows 지도 앱 실행

앱에서 Windows 지도 앱을 시작하여 여기에 표시된 것처럼 특정 지도 및 턴바이턴 길 찾기를 표시할 수 있습니다. 사용자 고유의 앱에서 지도 기능을 직접 제공하는 대신 Windows 지도 앱을 사용하여 해당 기능을 제공하는 것이 좋습니다. 자세한 내용은 [Windows 지도 앱 실행](https://msdn.microsoft.com/library/windows/apps/mt228341)을 참조하세요.

![Windows 지도 앱의 예](images/mapnyc.png)

## <a name="related-topics"></a>관련 항목

* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 지리적 위치 샘플](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [현재 위치 가져오기](get-location.md)
* [위치 인식 앱에 대한 디자인 지침](guidelines-and-checklist-for-detecting-location.md)
* [지도에 대한 디자인 지침](controls-map.md)
* [개인 정보 인식 앱에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)
