---
title: 지오코딩 및 리버스 지오코딩 수행
description: 이 가이드에서는 주소를 지리적 위치 (지 오 코딩)로 변환 하 고, 지 오 코딩 네임 스페이스에서 MapLocationFinder 클래스의 메서드를 호출 하 여 지리적 위치를 주소 (역방향)로 변환 하는 방법을 보여 줍니다.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 지오코딩, 지도, 위치
ms.localizationpriority: medium
ms.openlocfilehash: 5d7e1dda355cf87a2c8e26c11327cfff32e9d0b5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259365"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>지오코딩 및 리버스 지오코딩 수행

이 가이드에서는 주소를 지리적 위치 (지 오 코딩)로 변환 하 고, [**지 오 코딩 네임 스페이스**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 에서 [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스의 메서드를 호출 하 여 지리적 위치를 주소 (역방향)로 변환 하는 방법을 보여 줍니다.

> [!TIP]
> 앱에서 지도를 사용 하는 방법에 대해 자세히 알아보려면 GitHub의 [Windows 유니버설 샘플](h https://github.com/Microsoft/Windows-universal-samples) 리포지토리에서 [없습니다](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 샘플을 다운로드 하세요.

지 오 코딩 및 reverse 지 오 코딩와 관련 된 클래스는 다음과 같이 구성 됩니다.

-   [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스에는 지 오 코딩 ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) 및 역방향 지 오 코딩 ([**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync))를 처리 하는 메서드가 포함 되어 있습니다.
-   이러한 메서드는 둘 다 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 인스턴스를 반환 합니다.
-   [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 의 [**위치**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) 속성은 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체의 컬렉션을 노출 합니다. 
-   [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체에는 주소 속성, 주소를 나타내는 [**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) 개체를 노출 하는 [**주소**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) 속성 및 지리적 위치를 나타내는 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 개체가 노출 되는 [**Point**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) 속성이 모두 있습니다.

> [!IMPORTANT]
> map 인증 키를 지정 해야 map services를 사용할 수 있습니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

## <a name="get-a-location-geocode"></a>위치 가져오기(지오코드)

이 섹션에서는 주소 또는 장소 이름을 지리적 위치 (지 오 코딩)로 변환 하는 방법을 보여 줍니다.

1.  자리 이름 또는 주소를 사용 하 여 [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스의 [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) 메서드 오버 로드 중 하나를 호출 합니다.
2.  [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) 메서드는 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 개체를 반환 합니다.
3.  [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 의 [**위치**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) 속성을 사용 하 여 컬렉션 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체를 노출 합니다. 시스템이 지정 된 입력에 해당 하는 여러 위치를 찾을 수 있기 때문에 여러 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체가 있을 수 있습니다.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

이 코드는 `tbOutputText` 텍스트 상자에 다음 결과를 표시합니다.

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>주소 가져오기(리버스 지오코드)

이 섹션에서는 지리적 위치를 주소로 변환 하는 방법을 보여 줍니다 (역방향 지 오 코딩).

1.  [  **MapLocationFinder**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 클래스의 [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 메서드를 호출합니다.
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 메서드는 일치하는 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 개체 컬렉션을 포함하는 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체를 반환합니다.
3.  [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 의 [**위치**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) 속성을 사용 하 여 컬렉션 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체를 노출 합니다. 시스템이 지정 된 입력에 해당 하는 여러 위치를 찾을 수 있기 때문에 여러 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체가 있을 수 있습니다.
4.  각 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)의 [**address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) 속성을 통해 [**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) 개체에 액세스 합니다.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

이 코드는 `tbOutputText` 텍스트 상자에 다음 결과를 표시합니다.

``` syntax
town = Redmond
```

## <a name="related-topics"></a>관련 항목

* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [지도에 대한 디자인 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [비디오: Windows 앱의 휴대폰, 태블릿 및 PC에서 맵 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 클래스](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** 메서드](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** 메서드](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
