---
author: msatranjr
title: "지도에 경로 및 길 찾기 표시"
description: "경로 및 길 찾기를 요청하고 앱에 표시합니다."
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
translationtype: Human Translation
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: eb3596236e7de29473635b26f48f0c7e4fa1d49f

---

# 지도에 경로 및 길 찾기 표시


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


경로 및 길 찾기를 요청하고 앱에 표시합니다.

**팁** 앱에서 지도를 사용하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

**팁** 지도가 앱의 핵심 기능이 아닌 경우 대신 Windows 지도 앱을 실행하는 것이 좋습니다. `bingmaps:`, `ms-drive-to:` 및 `ms-walk-to:` URI 스키마로 Windows 지도 앱을 실행하여 특정 지도 및 턴바이턴 길 찾기를 표시할 수 있습니다. 자세한 내용은 [Windows 지도 앱 실행](https://msdn.microsoft.com/library/windows/apps/mt228341)을 참조하세요.

 

## MapRouteFinder 결과 소개


다음은 경로 및 길 찾기 클래스의 관계입니다.

-   [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 클래스에는 경로와 길 찾기를 가져오는 메서드가 있습니다.
-   이러한 메서드는 [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939)를 반환합니다.
-   [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939)에는 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 개체가 포함되어 있습니다. **MapRouteFinderResult**의 [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) 속성을 통해 이 개체에 액세스합니다.
-   [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937)에는 [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) 개체 컬렉션이 포함됩니다. **MapRoute**의 [**Legs**](https://msdn.microsoft.com/library/windows/apps/dn636973) 속성을 통해 이 컬렉션에 액세스합니다.
-   각 [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955)에는 [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961) 개체 컬렉션이 포함됩니다. **MapRouteLeg**의 [**Maneuvers**](https://msdn.microsoft.com/library/windows/apps/dn636959) 속성을 통해 이 컬렉션에 액세스합니다.

## 길 찾기 표시


[**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 클래스의 메서드(예제: [**GetDrivingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636943) 또는 [**GetWalkingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636953))를 호출하여 운전 또는 보행 경로와 길 찾기를 가져옵니다. [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) 개체에는 [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) 속성을 통해 액세스할 수 있는 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 개체가 포함됩니다.

경로를 요청할 경우 다음을 지정할 수 있습니다.

-   시작점과 끝점만 제공하거나 일련의 중간 지점을 제공하여 경로를 계산할 수 있습니다.
-   최적화(예제: 거리 최적화)를 지정할 수 있습니다.
-   제한 사항(예제: 고속도로 회피)을 지정할 수 있습니다.

계산된 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937)에는 경로를 따라 이동하는 데 소요되는 시간, 경로 길이, 그리고 경로 구간을 포함하는 [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) 개체를 제공하는 속성이 있습니다. 각 **MapRouteLeg** 개체에는 [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961) 개체 컬렉션이 포함됩니다. **MapRouteManeuver** 개체에는 [**InstructionText**](https://msdn.microsoft.com/library/windows/apps/dn636964) 속성을 통해 액세스할 수 있는 길 찾기가 포함됩니다.

**중요** 지도 서비스를 사용하려면 먼저 지도 인증 키를 지정해야 합니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

 

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

이 예제는 `tbOutputText` 입력란에 다음 결과를 표시합니다.

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

## 경로 표시


[**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)에 [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937)를 표시하려면 **MapRoute**를 사용하여 [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122)를 생성합니다. 그런 다음 **MapRouteView**를 **MapControl**의 [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) 컬렉션에 추가합니다.

**중요** 지도 서비스 또는 지도 컨트롤을 사용하려면 먼저 지도 인증 키를 지정해야 합니다. 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

 

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

다음 예제에서는 **MapWithRoute**라는 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 컨트롤에 다음을 표시합니다.

![경로를 표시하는 지도 컨트롤](images/routeonmap.png)

## 관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)




<!--HONumber=Aug16_HO3-->


