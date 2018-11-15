---
author: PatrickFarley
title: 지오코딩 및 리버스 지오코딩 수행
description: 이 가이드에서는 Windows.Services.Maps 네임 스페이스에 있는 MapLocationFinder 클래스의 메서드를 호출 하 여 (리버스 지 오 코딩) 주소를 지리적 위치로 변환 하 고 (지 오 코딩) 지리적 위치를 주소를 변환 하는 방법을 보여 줍니다.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 지오코딩, 지도, 위치
ms.localizationpriority: medium
ms.openlocfilehash: bdd956dece4435ceb8e14121ec2b545095af3a11
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6833837"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>지오코딩 및 리버스 지오코딩 수행

이 가이드에서 Windows.Services.Maps [** [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 메서드를 호출 하 여 주소 (리버스 지 오 코딩) 지리적 위치를 변환 하 고 (지 오 코딩) 지리적 위치를 주소를 변환 하는 방법을 보여 줍니다. **](https://msdn.microsoft.com/library/windows/apps/dn636979)네임 스페이스입니다.

> [!TIP]
> 앱에서 지도 사용에 대 한 자세한 내용은 GitHub의 [Windows 유니버설 샘플 리포지토리](hhttps://github.com/Microsoft/Windows-universal-samples) 에서 [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 샘플을 다운로드 합니다.

지 오 코딩 및 리버스 지 오 코딩에 관련 된 클래스는 다음과 같이 구성 됩니다.

-   [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스는 지 오 코딩 ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925))를 처리 하 고 리버스 지 오 코딩 ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928))는 방법이 포함 되어 있습니다.
-   이러한 두 메서드는 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 인스턴스를 반환 합니다.
-   [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 의 [**위치**](https://msdn.microsoft.com/library/windows/apps/dn627552) 속성 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체의 컬렉션을 노출 합니다. 
-   [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 주소를 나타내는 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 개체를 노출 하는 [**주소**](https://msdn.microsoft.com/library/windows/apps/dn636929) 속성 및 지리적 위치를 나타내는 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 를 노출 하는 [**지점**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) 속성을 둘 다 있어야 합니다.

> [!IMPORTANT]
> 지도 서비스를 사용하려면 먼저 지도 인증 키를 지정해야 합니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

## <a name="get-a-location-geocode"></a>위치 가져오기(지오코드)

이 섹션에는 주소 또는 장소 이름 (지 오 코딩) 지리적 위치로 변환 하는 방법을 보여 줍니다.

1.  장소 이름 또는 주소를 사용 하 여 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 메서드의 오버 로드 중 하나를 호출 합니다.
2.  [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 메서드 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 개체를 반환합니다.
3.  [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 의 [**위치**](https://msdn.microsoft.com/library/windows/apps/dn627552) 속성을 사용 하 여 컬렉션 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체를 노출 합니다. 시스템 지정된 된 입력에 해당 하는 여러 위치를 찾을 수 없기 때문에 여러 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 수 있습니다.

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

이 섹션에서는 주소로 (리버스 지 오 코딩) 지리적 위치를 변환 하는 방법을 보여 줍니다.

1.  [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 메서드를 호출합니다.
2.  [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 메서드는 일치하는 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 컬렉션을 포함하는 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 개체를 반환합니다.
3.  [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 의 [**위치**](https://msdn.microsoft.com/library/windows/apps/dn627552) 속성을 사용 하 여 컬렉션 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체를 노출 합니다. 시스템 지정된 된 입력에 해당 하는 여러 위치를 찾을 수 없기 때문에 여러 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 수 있습니다.
4.  각 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)의 [**주소**](https://msdn.microsoft.com/library/windows/apps/dn636929) 속성을 통해 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 개체에 액세스 합니다.

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

* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [전화, 태블릿 및 Windows 앱에서 PC 간에 지도 및 위치를 활용 하는 비디오:](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 클래스](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync** 메서드](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync** 메서드](https://msdn.microsoft.com/library/windows/apps/dn636928)
