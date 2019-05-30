---
title: 지오코딩 및 리버스 지오코딩 수행
description: 이 가이드에서는 지리적 위치 (지 오 코딩)에 주소를 변환 하 고 Windows.Services.Maps 네임 스페이스의 MapLocationFinder 클래스의 메서드를 호출 하 여 지리적 위치 (역방향 지 오 코딩) 주소를 변환 하는 방법을 보여 줍니다.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 지오코딩, 지도, 위치
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371868"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>지오코딩 및 리버스 지오코딩 수행

이 가이드에서는 지리적 위치 (지 오 코딩)에 주소를 변환의 메서드를 호출 하 여 지리적 위치 (역방향 지 오 코딩) 주소를 변환 하는 방법을 보여 줍니다 합니다 [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스를 [ **Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 네임 스페이스입니다.

> [!TIP]
> 앱에서 지도 사용 하는 방법에 대 한 자세한 내용은 다운로드 합니다 [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 샘플을 합니다 [Windows 유니버설 샘플 리포지토리](h https://github.com/Microsoft/Windows-universal-samples) GitHub에서.

지 오 코딩 및 역방향 지 오 코딩에 관련 된 클래스는 다음과 같이 구성 됩니다.

-   합니다 [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스에는 지 오 코딩을 처리 하는 메서드가 포함 되어 있습니다. ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) 및 지 오 코딩 (역방향[ **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   이러한 두 메서드는 반환 된 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 인스턴스.
-   합니다 [ **위치** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) 속성을 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 컬렉션을 노출 [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체입니다. 
-   [**MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체 모두에 [ **주소** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) 노출 하는 속성을 [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) 주소를 나타내는 개체와 [ **지점** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) 노출 하는 속성을 [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 개체 지리적 위치를 나타내는입니다.

> [!IMPORTANT]
> 지도 서비스를 사용 하려면 먼저 맵 인증 키를 지정 해야 합니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

## <a name="get-a-location-geocode"></a>위치 가져오기(지오코드)

이 섹션에는 주소 또는 지역 이름 (지 오 코딩) 지리적 위치로 변환 하는 방법을 보여 줍니다.

1.  오버 로드 중 하나를 호출 합니다 [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) 메서드를 [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 장소 이름 또는 주소를 사용 하 여 클래스 주소입니다.
2.  합니다 [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) 메서드가 반환 되는 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 개체입니다.
3.  사용 합니다 [ **위치** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) 속성을 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 컬렉션을 노출 하 [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체입니다. 여러 개 있을 수 있습니다 [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체 시스템 여러 찾을 수 없기 때문에 지정된 된 입력에 해당 하는 위치입니다.

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

이 섹션에는 지리적 위치 (역방향 지 오 코딩) 주소를 변환 하는 방법을 보여 줍니다.

1.  [  **MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 클래스의 [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 메서드를 호출합니다.
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 메서드는 일치하는 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체 컬렉션을 포함하는 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 개체를 반환합니다.
3.  사용 합니다 [ **위치** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) 속성을 [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 컬렉션을 노출 하 [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체입니다. 여러 개 있을 수 있습니다 [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 개체 시스템 여러 찾을 수 없기 때문에 지정된 된 입력에 해당 하는 위치입니다.
4.  액세스 [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) 를 통해 개체를 [ **주소** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) 각각의 속성 [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

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

* [UWP 지도 샘플](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 교통 앱 샘플](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [지도에 대한 디자인 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 클래스](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** 메서드](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** 메서드](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
