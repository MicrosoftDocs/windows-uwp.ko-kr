---
author: PatrickFarley
title: "지오코딩 및 리버스 지오코딩 수행"
description: "Windows.Services.Maps 네임스페이스에 있는 MapLocationFinder 클래스의 메서드를 호출하여 주소를 지리적 위치로 변환하고(지오코딩) 지리적 위치를 주소로 변환합니다(리버스 지오코딩)."
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: caf3ad6fecd6ed90c65f85477643fb42ab4787d3

---

# 지오코딩 및 리버스 지오코딩 수행


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[
            **Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스에 있는 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 메서드를 호출하여 주소를 지리적 위치로 변환하고(지오코딩) 지리적 위치를 주소로 변환합니다(리버스 지오코딩).

**팁** 앱에서 지도를 사용하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

지오코딩 및 리버스 지오코딩 클래스의 관계는 다음과 같습니다.

-   [
            **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스에는 지오코딩([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925))과 리버스 지오코딩([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928))을 수행하는 메서드가 있습니다.
-   이러한 메서드는 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)를 반환합니다.
-   [
            **MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)에는 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 컬렉션이 포함됩니다. **MapLocationFinderResult**의 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 속성을 통해 이 컬렉션에 액세스합니다.
-   각 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체에는 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 개체가 포함되어 있습니다. 각 **MapLocation**의 [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) 속성을 통해 이 개체에 액세스합니다.

**중요** 지도 서비스를 사용하려면 먼저 지도 인증 키를 지정해야 합니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

 

## 위치 가져오기(지오코드)


다음 단계를 수행하여 주소나 장소 이름을 지리적 위치(지오코딩)로 변환합니다.

1.  [
            **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 메서드 오버로드 중 하나를 호출합니다.
2.  [
            **FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 메서드는 일치하는 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 컬렉션을 포함하는 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 개체를 반환합니다.
3.  [
            **MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)의 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 속성을 통해 이 컬렉션에 액세스합니다.

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

## 주소 가져오기(리버스 지오코드)


다음 단계를 수행하여 지리적 위치를 주소로 변환합니다(리버스 지오코딩).

1.  [
            **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 클래스의 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 메서드를 호출합니다.
2.  [
            **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 메서드는 일치하는 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 개체 컬렉션을 포함하는 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 개체를 반환합니다.
3.  [
            **MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)의 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 속성을 통해 이 컬렉션에 액세스합니다.
4.  각 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)의 [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) 속성을 통해 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 개체에 액세스합니다.

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

## 관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)





<!--HONumber=Jun16_HO4-->


