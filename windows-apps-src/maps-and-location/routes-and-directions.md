---
title: 지도에 경로 및 방향 표시
description: MapRouteFinder 클래스를 사용 하 여 경로 및 방향을 검색 하 고 UWP (유니버설 Windows 플랫폼) 앱의 없습니다에 표시 하는 방법에 대해 알아봅니다.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, 경로, 지도, 위치, 방향
ms.localizationpriority: medium
ms.openlocfilehash: 4171598f47d28942adb56860452a8ec49cb5e2c2
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297607"
---
# <a name="display-routes-and-directions-on-a-map"></a>지도에 경로 및 방향 표시

> [!NOTE]
> [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 map services Requite는 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)라는 지도 인증 키를 만듭니다. Maps 인증 키를 가져오고 설정 하는 방법에 대 한 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조 하세요.

경로 및 길 찾기를 요청하고 앱에 표시합니다.

>[!Note]
>앱에서 지도를 사용 하는 방법에 대 한 자세한 내용을 보려면 [UWP (유니버설 Windows 플랫폼) 맵 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)을 다운로드 하세요.
>매핑이 앱의 핵심 기능이 아니면 대신 Windows Maps 앱을 시작 하는 것이 좋습니다. `bingmaps:`, `ms-drive-to:` 및 `ms-walk-to:` URI 체계를 사용 하 여 Windows maps 앱을 특정 맵으로 시작 하 고 방향을 다시 설정할 수 있습니다. 자세한 내용은 [Windows 지도 앱 실행](../launch-resume/launch-maps-app.md)을 참조하세요.

 
## <a name="an-intro-to-maproutefinder-results"></a>MapRouteFinder 결과를 소개 합니다.


경로 및 방향의 클래스는 다음과 같이 관련 됩니다.

* [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) 클래스에는 경로와 방향을 가져오는 메서드가 있습니다. 이러한 메서드는 [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult)을 반환 합니다.

* [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult) 에는 [**maproute**](/uwp/api/Windows.Services.Maps.MapRoute) 개체가 포함 되어 있습니다. **MapRouteFinderResult**의 [**Route**](/uwp/api/windows.services.maps.maproutefinderresult.route) 속성을 통해이 개체에 액세스 합니다.

* [**Maproute**](/uwp/api/Windows.Services.Maps.MapRoute) 에는 [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg) 개체의 컬렉션이 포함 되어 있습니다. **Maproute**의 [**변**](/uwp/api/windows.services.maps.maproute.legs) 속성을 통해이 컬렉션에 액세스 합니다.

* 각 [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg) 에는 [**MapRouteManeuver**](/uwp/api/Windows.Services.Maps.MapRouteManeuver) 개체의 컬렉션이 포함 되어 있습니다. **MapRouteLeg**의 [**연습**](/uwp/api/windows.services.maps.maprouteleg.maneuvers) 속성을 통해이 컬렉션에 액세스 합니다.

[**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) 클래스의 메서드를 호출 하 여 구동 경로 및 지침을 가져옵니다. 예를 들면 [**GetDrivingRouteAsync**](/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) 또는 [**GetWalkingRouteAsync**](/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync)입니다.

경로를 요청 하는 경우 다음을 지정할 수 있습니다.

* 시작점과 끝점을 제공 하거나 경로를 계산 하는 일련의 waypoints 제공할 수 있습니다.

    *Stop* waypoints는 각각 자체의 일정을 포함 하는 추가 경로를 추가 합니다. *Stop* waypoints을 지정 하려면 [**GetDrivingRouteFromWaypointsAsync**](/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync) 오버 로드 중 하나를 사용 합니다.

    이동 경로를 *통해* *stop* waypoints 사이의 중간 위치를 정의 합니다. 경로 다리를 추가 하지 않습니다.  단순히 경로를 통과 해야 하는 waypoints 있습니다. Waypoints를 *통해* 지정 하려면 [**GetDrivingRouteFromEnhancedWaypointsAsync**](/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync) 오버 로드 중 하나를 사용 합니다.

* 최적화를 지정할 수 있습니다 (예: 거리 최소화).

* 제한 (예: 톨게이트 방지)을 지정할 수 있습니다.

## <a name="display-directions"></a>표시 방향

[**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult) 개체에는 [**경로**](/uwp/api/windows.services.maps.maproutefinderresult.route) 속성을 통해 액세스할 수 있는 [**maproute**](/uwp/api/Windows.Services.Maps.MapRoute) 개체가 포함 되어 있습니다.

계산 된 [**Maproute**](/uwp/api/Windows.Services.Maps.MapRoute) 에는 경로를 트래버스하는 시간, 경로의 길이, 경로의 다리를 포함 하는 [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg) 개체의 컬렉션을 제공 하는 속성이 있습니다. 각 **MapRouteLeg** 개체에는 [**MapRouteManeuver**](/uwp/api/Windows.Services.Maps.MapRouteManeuver) 개체의 컬렉션이 포함 되어 있습니다. **MapRouteManeuver** 개체에는 [**InstructionText**](/uwp/api/windows.services.maps.maproutemaneuver.instructiontext) 속성을 통해 액세스할 수 있는 지침이 포함 되어 있습니다.

>[!IMPORTANT]
>Maps 인증 키를 지정 해야 map service를 사용할 수 있습니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조 하세요.

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

이 예에서는 텍스트 상자에 다음 결과를 표시 합니다 `tbOutputText` .

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>표시 경로


[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)에 [**maproute**](/uwp/api/Windows.Services.Maps.MapRoute) 를 표시 하려면 **Maproute**를 사용 하 여 [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) 를 생성 합니다. 그런 다음 **MapRouteView** 를 **없습니다**의 [**경로**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) 컬렉션에 추가 합니다.

>[!IMPORTANT]
>Maps 인증 키를 지정 해야 map service 또는 map 컨트롤을 사용할 수 있습니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조 하세요.

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

이 예제에서는 **Mapwithroute**라는 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 에 다음을 표시 합니다.

![경로를 표시 하는 지도 컨트롤입니다.](images/routeonmap.png)

다음은 두 *stop* waypoints 사이에서 이동 경로를 *통해* 를 사용 하는이 예제의 버전입니다.

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [지도에 대한 디자인 지침](./display-maps.md)
* [빌드 2015 비디오: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
