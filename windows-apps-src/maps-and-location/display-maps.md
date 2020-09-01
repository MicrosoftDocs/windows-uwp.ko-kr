---
title: 2D, 3D 및 Streetside 뷰가 있는 지도 표시
description: 지도 *위치 카드* 또는 완전 한 기능을 갖춘 지도 컨트롤에서 지도를 light 무시할 수 있는 창에 표시할 수 있습니다.
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 지도 컨트롤, 지도 보기
ms.localizationpriority: medium
ms.openlocfilehash: 1979aa2b2e99a585a122e4835bb6920b9d342cd7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162647"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>2D, 3D 및 Streetside 뷰가 있는 지도 표시

지도를 무시할 수 있는 창에 지도를 *표시 하거나 완전* 한 기능을 갖춘 맵 컨트롤에 지도를 표시할 수 있습니다.

[Map 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 을 다운로드 하 여이 가이드에 설명 된 일부 기능을 사용해 보세요.

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>배치에 지도 표시
사용자를 표시 하는 데 사용 하는 사용자를 표시 하는 데 사용 하는 사용자를 UI 요소 또는 사용자가 사용 하는 응용 프로그램의 영역 아래 또는 UI 요소에 대 한 합니다. 지도에는 앱의 정보와 관련 된 도시 또는 주소가 표시 될 수 있습니다.  

이는 시애틀의 도시를 표시 합니다.

![시애틀 도시 표시](images/placecard-city.png)

시애틀이 단추 아래에 표시 되도록 하는 코드는 다음과 같습니다.

```csharp
private void Seattle_Click(object sender, RoutedEventArgs e)
{
    Geopoint seattlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6062, Longitude = -122.3321 });

    PlaceInfo spaceNeedlePlace = PlaceInfo.Create(seattlePoint);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

이 위치에는 시애틀의 공간 니 들의 위치가 표시 됩니다.

![우주 니 들의 위치 표시](images/placecard-needle.png)

우주 니 들이 단추 아래에 표시 되는 코드는 다음과 같습니다.

```csharp
private void SpaceNeedle_Click(object sender, RoutedEventArgs e)
{
    Geopoint spaceNeedlePoint = new Geopoint
        (new BasicGeoposition { Latitude = 47.6205, Longitude = -122.3493 });

    PlaceInfoCreateOptions options = new PlaceInfoCreateOptions();

    options.DisplayAddress = "400 Broad St, Seattle, WA 98109";
    options.DisplayName = "Seattle Space Needle";

    PlaceInfo spaceNeedlePlace =  PlaceInfo.Create(spaceNeedlePoint, options);

    FrameworkElement targetElement = (FrameworkElement)sender;

    GeneralTransform generalTransform =
        targetElement.TransformToVisual((FrameworkElement)targetElement.Parent);

    Rect rectangle = generalTransform.TransformBounds(new Rect(new Point
        (targetElement.Margin.Left, targetElement.Margin.Top), targetElement.RenderSize));

    spaceNeedlePlace.Show(rectangle, Windows.UI.Popups.Placement.Below);
}
```

<a id="map-control" />

## <a name="display-map-in-a-control"></a>컨트롤에 지도 표시

지도 컨트롤을 사용 하 여 응용 프로그램에 다양 하 고 사용자 지정 가능한 맵 데이터를 표시 합니다. 지도 컨트롤은도로 지도, 항공, 3D, 보기, 방향, 검색 결과 및 트래픽을 표시할 수 있습니다. 지도에서 사용자의 위치, 방향 및 관심 요소를 표시할 수 있습니다. 지도에는 3D 보기, Streetside 보기, 트래픽, 전송 및 로컬 항공 표시 될 수도 있습니다.

앱 내에서 사용자가 앱 특정 또는 일반 지리 정보를 볼 수 있도록 허용 하는 지도 컨트롤을 사용 합니다. 앱에 맵 컨트롤이 있으면 사용자가 앱 외부로 이동 하지 않고도 해당 정보를 얻을 수 있습니다.

> [!NOTE]
>사용자가 앱 외부에서 이동 하지 않는 경우 Windows Maps 앱을 사용 하 여 해당 정보를 제공 하는 것이 좋습니다. 앱은 Windows Maps 앱을 시작 하 여 특정 지도, 방향 및 검색 결과를 표시할 수 있습니다. 자세한 내용은 [Windows 지도 앱 실행](../launch-resume/launch-maps-app.md)을 참조하세요.

### <a name="add-a-map-control-to-your-app"></a>앱에 맵 컨트롤 추가

[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)를 추가 하 여 XAML 페이지에 지도를 표시 합니다. 없습니다를 사용 하려면 XAML 페이지 또는 코드에서 **MapControl** [**네임 스페이스를 선언**](/uwp/api/Windows.UI.Xaml.Controls.Maps) 해야 합니다. 도구 상자에서 컨트롤을 끌면이 네임 스페이스 선언이 자동으로 추가 됩니다. **없습니다** 를 XAML 페이지에 수동으로 추가 하는 경우 페이지 맨 위에 수동으로 네임 스페이스 선언을 추가 해야 합니다.

다음 예제에서는 기본 지도 컨트롤을 표시 하 고 터치 입력을 허용 하는 것 외에도 확대/축소 및 기울기 컨트롤을 표시 하도록 지도를 구성 합니다.

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>

 </Grid>
</Page>
```

코드에 지도 컨트롤을 추가 하는 경우 코드 파일의 맨 위에 네임 스페이스를 수동으로 선언 해야 합니다.

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

### <a name="get-and-set-a-maps-authentication-key"></a>맵 인증 키 가져오기 및 설정

[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 map services를 사용 하려면 먼저 맵 인증 키를 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) 속성의 값으로 지정 해야 합니다. 이전 예제에서를 `EnterYourAuthenticationKeyHere` [Bing Maps 개발자 센터](https://www.bingmapsportal.com/)에서 가져온 키로 바꿉니다. 텍스트 **경고: MapServiceToken 지정 되지 않음** 은 지도 인증 키를 지정할 때까지 컨트롤 아래에 계속 표시 됩니다. Maps 인증 키를 가져오고 설정 하는 방법에 대 한 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조 하세요.

## <a name="set-the-location-of-a-map"></a>지도의 위치 설정
원하는 위치에 대 한 맵을 가리키거나 사용자의 현재 위치를 사용 합니다.  

### <a name="set-a-starting-location-for-the-map"></a>지도의 시작 위치 설정

코드에서 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 의 [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 속성을 지정 하거나 XAML 태그에 속성을 바인딩하여 맵에 표시할 위치를 설정 합니다. 다음 예에서는 시애틀 도시를 중심으로 하는 지도를 표시 합니다.

> [!NOTE]
> 문자열은 [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)로 변환할 수 없으므로 데이터 바인딩을 사용 하지 않으면 XAML 태그에서 [**Center**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 속성의 값을 지정할 수 없습니다. 이 제한은 [**없습니다**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setlocation) 연결 된 속성에도 적용 됩니다.

 
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![지도 컨트롤의 예입니다.](images/displaymapsexample1.png)

### <a name="set-the-current-location-of-the-map"></a>지도의 현재 위치를 설정 합니다.

앱에서 사용자의 위치에 액세스할 수 있으려면 앱에서 [**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 메서드를 호출 해야 합니다. 이때 앱이 포그라운드에 있어야 하고 **RequestAccessAsync**가 UI 스레드에서 호출되어야 합니다. 사용자가 자신의 위치에 대한 권한을 앱에 부여하기 전에는 앱이 위치 데이터에 액세스할 수 없습니다.

[**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 클래스의 [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 메서드를 사용 하 여 장치 (위치가 사용 가능한 경우)의 현재 위치를 가져옵니다. 해당 [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)를 얻으려면 geoposition의 Geocoordinate [**Point**](/uwp/api/windows.devices.geolocation.geocoordinate.point) 속성을 사용 합니다. 자세한 내용은 [현재 위치 가져오기](get-location.md)를 참조 하세요.

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

지도에 장치의 위치를 표시 하는 경우 그래픽을 표시 하 고 위치 데이터의 정확도에 따라 확대/축소 수준을 설정 하는 것이 좋습니다. 자세한 내용은 [위치 인식 앱에 대 한 지침](./guidelines-and-checklist-for-detecting-location.md)을 참조 하세요.

### <a name="change-the-location-of-the-map"></a>지도의 위치 변경

2D 맵에 표시 되는 위치를 변경 하려면 [**Trysetviewasync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewasync) 메서드의 오버 로드 중 하나를 호출 합니다. 이 메서드를 사용 하 여 [**가운데**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center), 확대/확대 [**수준**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel), [**머리글**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading)및 [**피치**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitch)의 새 값을 지정 합니다. [**Mapanimation kind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 열거에서 상수를 제공 하 여 뷰가 변경 될 때 사용할 선택적 애니메이션을 지정할 수도 있습니다.

3D 지도의 위치를 변경 하려면 대신 [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) 메서드를 사용 합니다. 자세한 내용은 [Display 항공 3d views](#3Dviews)를 참조 하세요.

[**TrySetViewBoundsAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewboundsasync) 메서드를 호출 하 여 지도의 [**GeoboundingBox**](/uwp/api/Windows.Devices.Geolocation.GeoboundingBox) 콘텐츠를 표시 합니다. 예를 들어이 메서드를 사용 하 여 지도의 경로 또는 경로 일부를 표시 합니다. 자세한 내용은 [지도에 경로 및 방향 표시](routes-and-directions.md)를 참조 하세요.

## <a name="change-the-appearance-of-a-map"></a>지도의 모양 변경

지도의 모양과 느낌을 사용자 지정 하려면 맵 컨트롤의 [**StyleSheet**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet) 속성을 기존 [**mapstylesheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 개체 중 하나로 설정 합니다.

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![어두운 스타일 맵](images/style-dark.png)

또한 JSON을 사용 하 여 사용자 지정 스타일을 정의한 다음 해당 JSON을 사용 하 여 [**Mapstylesheet**](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 개체를 만들 수 있습니다.

스타일 시트 JSON은 [지도 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) 응용 프로그램을 사용 하 여 대화형으로 만들 수 있습니다.

```csharp
myMap.StyleSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""settings"": {
            ""landColor"": ""#FFFFFF"",
            ""spaceColor"": ""#000000""
        },
        ""elements"": {
            ""mapElement"": {
                ""labelColor"": ""#000000"",
                ""labelOutlineColor"": ""#FFFFFF""
            },
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            },
            ""area"": {
                ""fillColor"": ""#EEEEEE""
            },
            ""political"": {
                ""borderStrokeColor"": ""#CCCCCC"",
                ""borderOutlineColor"": ""#00000000""
            }
        }
    }
");
```

![사용자 지정 스타일 맵](images/style-custom.png)

전체 JSON 항목 참조는 [지도 스타일 시트 참조](elements-of-map-style-sheet.md)를 참조 하세요.

기존 시트로 시작한 다음 JSON을 사용 하 여 원하는 모든 요소를 재정의할 수 있습니다. 이 예에서는 기존 스타일로 시작 하 고 JSON을 사용 하 여 워터 마크 영역의 색만 변경 합니다.

```csharp
 MapStyleSheet \customSheet = MapStyleSheet.ParseFromJson(@"
    {
        ""version"": ""1.0"",
        ""elements"": {
            ""water"": {
                ""fillColor"": ""#DDDDDD""
            }
        }
    }
");

MapStyleSheet builtInSheet = MapStyleSheet.RoadDark();

myMap.StyleSheet = MapStyleSheet.Combine(new List<MapStyleSheet> { builtInSheet, customSheet });
```

![스타일 맵 결합](images/style-combined.png)

>[!NOTE]
>두 번째 스타일 시트에서 정의 하는 스타일은 첫 번째 스타일에서 스타일을 재정의 합니다.

## <a name="set-orientation-and-perspective"></a>방향 및 큐브 뷰 설정

지도의 카메라를 확대, 축소, 회전 및 기울기 하 여 원하는 효과에 대 한 오른쪽 각도를 가져옵니다. 이러한 속성을 사용해 보세요.

-   [**센터**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 속성을 설정 하 여 지도의 **중심** 을 지리적 점으로 설정 합니다.
-   확대/축소 수준 속성을 1에서 20 사이의 값으로 설정 하 여 지도의 **확대/축소 수준을** [**설정 합니다.**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel)
-   [**머리글**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) 속성을 설정 하 여 지도의 **회전** 을 설정 합니다. 여기에는 0 또는 360도 = 북부, 90 = 동부, 180 = 남부 및 270 = 서쪽이 있습니다.
-   [**DesiredPitch**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.desiredpitch) 속성을 0에서 65도 사이의 값으로 설정 하 여 지도의 **기울기** 를 설정 합니다.

## <a name="show-and-hide-map-features"></a>지도 기능 표시 및 숨기기

[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 속성 값을 설정 하 여도로 및 랜드마크 같은 지도 기능을 표시 하거나 숨깁니다.

* [**LandmarksVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.landmarksvisible) 속성을 사용 하거나 사용 하지 않도록 설정 하 여 지도에 **건물과 랜드마크** 을 표시 합니다.

  > [!NOTE]
  > 건물을 표시 하거나 숨길 수는 있지만 3 차원으로 표시 되는 것을 방지할 수는 없습니다.  

* [**PedestrianFeaturesVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pedestrianfeaturesvisible) 속성을 사용 하거나 사용 하지 않도록 설정 하 여 지도의 공개 계단과 같은 **보행자 기능** 을 표시 합니다.
* [**TrafficFlowVisible**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trafficflowvisible) 속성을 사용 하거나 사용 하지 않도록 설정 하 여 맵에 **트래픽을** 표시 합니다.
* [**WatermarkMode**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.watermarkmode) 속성을 [**MapWatermarkMode**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapWatermarkMode) 상수 중 하나로 설정 하 여 지도에 **워터 마크** 를 표시할지 여부를 지정 합니다.
* 지도 컨트롤의 [**경로**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) 컬렉션에 [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) 를 추가 하 여 지도에 **구동 경로** 를 표시 합니다. 자세한 내용과 예제는 [맵에 경로 및 방향 표시](routes-and-directions.md)를 참조 하세요.

[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)에 압정, SHAPES 및 XAML 컨트롤을 표시 하는 방법에 대 한 자세한 내용은 [map의 관심 지점 (Poi) 표시](display-poi.md)를 참조 하세요.

## <a name="display-streetside-views"></a>Streetside 보기 표시


Streetside 뷰는 지도 컨트롤의 맨 위에 표시 되는 위치의 거리 수준 관점입니다.

![지도 컨트롤의 streetside 뷰 예제입니다.](images/onlystreetside-730width.png)

Streetside 뷰를 맵 컨트롤에 원래 표시 된 맵과 분리 하는 환경 "내부"로 간주 합니다. 예를 들어 Streetside 보기에서 위치를 변경 해도 Streetside 보기에서 지도의 위치나 모양이 변경 되지 않습니다. 컨트롤의 오른쪽 위 모퉁이에 있는 **X** 를 클릭 하 여 Streetside 보기를 닫은 후 원래 맵은 변경 되지 않은 상태로 유지 됩니다.

Streetside 보기를 표시 하려면

1.  [**IsStreetsideSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.isstreetsidesupported)를 확인 하 여 장치에서 Streetside 보기가 지원 되는지 확인 합니다.
2.  Streetside view를 지 원하는 경우 [**FindNearbyAsync**](/uwp/api/windows.ui.xaml.controls.maps.streetsidepanorama.findnearbyasync)를 호출 하 여 지정 된 위치 근처에 [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) 를 만듭니다.
3.  [**StreetsidePanorama**](/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama) 가 null이 아닌지 확인 하 여 근처의 파노라마를 찾을 수 있는지 여부를 확인 합니다.
4.  근처의 파노라마를 찾았으면 맵 컨트롤의 [**Customexperience**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.customexperience) 속성에 대해 [**StreetsideExperience**](/uwp/api/windows.ui.xaml.controls.maps.streetsideexperience) 를 만듭니다.

이 예제에서는 이전 이미지와 비슷한 Streetside view를 표시 하는 방법을 보여 줍니다.

**참고**    지도 컨트롤 크기가 너무 작은 경우에는 개요 맵이 표시 되지 않습니다.

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

<a id="3Dviews" />
## <a name="display-aerial-3d-views"></a>항공 3D 뷰 표시


[**Mapscene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) 클래스를 사용 하 여 지도의 3d 큐브 뷰를 지정 합니다. 지도 장면은 지도에 표시 되는 3D 뷰를 나타냅니다. [**Mapcamera**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapCamera) 클래스는 이러한 뷰를 표시 하는 카메라의 위치를 나타냅니다.

![지도 장면 위치에 대 한 MapCamera 위치 다이어그램](images/mapcontrol-techdiagram.png)

지도 화면에서 건물과 기타 기능을 3D에 표시 하려면 지도 컨트롤의 [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 속성을 [**mapstyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)로 설정 합니다. **Aerial3DWithRoads** 스타일이 있는 3d 뷰의 예입니다.

![3d 맵 보기의 예입니다.](images/only3d-730width.png)

3D 뷰를 표시 하려면

1.  [**Is3DSupported**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.is3dsupported)를 확인 하 여 장치에서 3d 보기가 지원 되는지 확인 합니다.
2.  3D 뷰가 지원 되는 경우 지도 컨트롤의 [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 속성을 [**mapstyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)로 설정 합니다.
3.  [**Createfromlocationandradius**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromlocationandradius) , [**createfromcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromcamera)등의 많은 **createfrom** 메서드 중 하나를 사용 하 여 [**mapscene**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) 개체를 만듭니다.
4.  [**TrySetSceneAsync**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) 를 호출 하 여 3d 뷰를 표시 합니다. [**Mapanimation kind**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 열거에서 상수를 제공 하 여 뷰가 변경 될 때 사용할 선택적 애니메이션을 지정할 수도 있습니다.

이 예에서는 3D 뷰를 표시 하는 방법을 보여 줍니다.

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 43.773251, Longitude = 11.255474};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## <a name="get-info-about-locations"></a>위치에 대 한 정보 가져오기


[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 메서드를 호출 하 여 맵의 위치에 대 한 정보를 가져옵니다.

-   [**TryGetLocationFromOffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getlocationfromoffset) 메서드-지도 컨트롤의 뷰포트에 지정 된 지점에 해당 하는 지리적 위치를 가져옵니다.
-   [**GetOffsetFromLocation**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getoffsetfromlocation) method-지정 된 지리적 위치에 해당 하는 지도 컨트롤의 뷰포트에 있는 점을 가져옵니다.
-   [**Islocationinview**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.islocationinview) 메서드-지정 된 지리적 위치가 지도 컨트롤의 뷰포트에 현재 표시 되는지 여부를 확인 합니다.
-   [**FindMapElementsAtOffset**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.findmapelementsatoffset) 메서드-지도 컨트롤 뷰포트의 지정 된 지점에 있는 지도에서 요소를 가져옵니다.

## <a name="handle-interaction-and-changes"></a>상호 작용 및 변경 내용 처리


[**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 이벤트를 처리 하 여 맵에서 사용자 입력 제스처를 처리 합니다. [**Mapinputeventargs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapInputEventArgs)의 [**위치**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.location) 및 [**위치**](/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.position) 속성 값을 확인 하 여 제스처가 발생 한 뷰포트의 실제 위치와 지도의 지리적 위치에 대 한 정보를 가져옵니다.

-   [**MapTapped 때**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.maptapped)
-   [**MapDoubleTapped**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapdoubletapped)
-   [**MapHolding**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapholding)

컨트롤의 [**LoadingStatusChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.loadingstatuschanged) 이벤트를 처리 하 여 map이 로드 되는지 아니면 완전히 로드 되는지 확인 합니다.

사용자 또는 앱이 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 이벤트를 처리 하 여 맵의 설정을 변경 하는 경우 발생 하는 변경 내용을 처리 합니다. [Maps에 대 한 지침]()

-   [**가운데 변경 됨**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.centerchanged)
-   [**HeadingChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.headingchanged)
-   [**PitchChanged**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitchchanged)
-   [**확대/확대/변경**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevelchanged)

## <a name="best-practice-recommendations"></a>모범 사례 권장 사항

-   사용자가 이동 하거나 확대/축소 하 여 지리적 정보를 볼 필요가 없도록 지도를 표시 하려면 충분 한 화면 공간 (또는 전체 화면)을 사용 합니다.

-   지도를 사용 하 여 정적 정보 보기를 표시 하는 경우에는 더 작은 지도를 사용 하는 것이 더 적합할 수 있습니다. 더 작은 정적 지도를 사용 하는 경우 사용 편리성에 따라 크기를 조정 하는 데 충분 한 화면 공간을 충분히 절약 하는 데 충분 하지만 읽기 쉽게 유지할 수 있습니다.

-   지도 [**요소**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapelementsproperty)를 사용 하 여 지도 장면에 관심 위치를 포함 합니다. 모든 추가 정보는 지도 장면을 오버레이 하는 임시 UI로 표시 될 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [현재 위치 가져오기](get-location.md)
* [위치 인식 앱에 대한 디자인 지침](./guidelines-and-checklist-for-detecting-location.md)
* [지도에 대한 디자인 지침]()
* [빌드 2015 비디오: Windows 앱의 휴대폰, 태블릿 및 PC에서 맵 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)