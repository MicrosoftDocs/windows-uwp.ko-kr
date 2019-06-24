---
title: 2D, 3D 및 Streetside 뷰가 있는 지도 표시
description: 지도 *장소 카드*라는 빠른 해제 가능 창 또는 전체 기능을 갖춘 지도 컨트롤에서 지도를 표시할 수 있습니다.
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 지도 컨트롤, 지도 보기
ms.localizationpriority: medium
ms.openlocfilehash: 366a6212f8974ef3d3fedffa8f2d657e08a1b549
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318668"
---
# <a name="display-maps-with-2d-3d-and-streetside-views"></a>2D, 3D 및 Streetside 뷰가 있는 지도 표시

지도 *장소 카드*라는 빠른 해제 가능 창 또는 전체 기능을 갖춘 지도 컨트롤에서 지도를 표시할 수 있습니다.

[지도 샘플](https://go.microsoft.com/fwlink/p/?LinkId=619977)을 다운로드하여 이 가이드에 설명된 몇 가지 기능을 시도해 보세요.

<a id="placecard" />

## <a name="display-map-in-a-placecard"></a>장소 카드에서 지도 표시
UI 요소 또는 사용자가 터치하는 앱의 영역 위, 아래, 또는 측면에 있는 경량 팝업 창 내에서 지도를 표시할 수 있습니다. 지도는 앱의 정보와 관련된 도시나 주소를 표시할 수 있습니다.  

이 장소 카드는 시애틀을 보여 줍니다.

![시애틀을 보여 주는 장소 카드](images/placecard-city.png)

장소 카드에서 시애틀이 단추 아래에 표시되도록 하는 코드는 다음과 같습니다.

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

이 장소 카드는 시애틀에서 Space Needle의 위치를 보여 줍니다.

![Space Needle의 위치를 보여 주는 장소 카드](images/placecard-needle.png)

장소 카드에서 Space Needle이 단추 아래에 표시되도록 하는 코드는 다음과 같습니다.

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

## <a name="display-map-in-a-control"></a>컨트롤에서 지도 표시

지도 컨트롤을 사용하여 앱에서 풍부하고 사용자 지정 가능한 지도 데이터를 보여 줍니다. 지도 컨트롤은 도로 지도, 위성뷰, 3D, 뷰, 길 찾기, 검색 결과, 교통 정보 등을 표시할 수 있습니다. 지도에서 사용자의 위치, 방향, 관심 지점을 표시할 수 있습니다. 또한 3D 위성뷰, Streetside 뷰, 교통 정보, 대중교통 및 지역 기업을 지도에 표시할 수도 있습니다.

사용자가 앱 관련 정보 또는 일반 지리 정보를 볼 수 있는 지도를 앱에 포함하려는 경우 지도 컨트롤을 사용합니다. 앱에 지도 컨트롤을 두면 사용자가 해당 정보를 얻기 위해 앱을 벗어나지 않아도 됩니다.

> [!NOTE]
>사용자가 앱을 벗어나도 상관없는 경우 Windows 지도 앱을 사용하여 해당 정보를 제공하는 것이 좋습니다. 앱에서 Windows 지도 앱을 실행하여 특정 지도, 길 찾기 및 검색 결과를 표시할 수 있습니다. 자세한 내용은 [Windows 지도 앱 실행](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app)을 참조하세요.

### <a name="add-a-map-control-to-your-app"></a>앱에 지도 컨트롤 추가

[  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)을 추가하여 XAML 페이지에 지도를 표시합니다. **MapControl**을 사용하려면 XAML 페이지 또는 코드에서 [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) 네임스페이스를 선언해야 합니다. 도구 상자에서 이 컨트롤을 끌면 이 네임스페이스 선언이 자동으로 추가됩니다. **MapControl**을 XAML 페이지에 수동으로 추가할 경우 페이지 맨 위에 네임스페이스 선언을 수동으로 추가해야 합니다.

다음 예제에서는 기본 지도 컨트롤을 표시하며, 터치식 입력을 허용하는 것 외에 확대/축소 및 이동(상하) 컨트롤을 표시하도록 지도를 구성합니다.

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

코드에 지도 컨트롤을 추가할 경우 코드 파일의 맨 위에 네임스페이스를 수동으로 선언해야 합니다.

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

### <a name="get-and-set-a-maps-authentication-key"></a>지도 인증 키 얻기 및 설정

[  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 지도 서비스를 사용하려면 지도 인증 키를 [**MapServiceToken**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) 속성 값으로 지정해야 합니다. 위 예제에서 `EnterYourAuthenticationKeyHere`을 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 가져온 키로 바꿉니다. 텍스트 **경고: 지정 되지 않은 MapServiceToken** 컨트롤 아래에 표시 되 지도 인증 키를 지정할 때까지 계속 됩니다. 지도 인증 키를 얻고 설정하는 방법에 대한 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

## <a name="set-the-location-of-a-map"></a>지도의 위치 설정
지도에서 원하는 위치를 가리키거나 사용자의 현재 위치를 사용합니다.  

### <a name="set-a-starting-location-for-the-map"></a>지도의 시작 위치 설정

코드에서 [**MapControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center)의 [**Center**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 속성을 지정하거나 XAML 태그에서 속성을 바인딩하여 지도에 표시할 위치를 지정합니다. 다음 예에서는 시애틀시를 중심으로 하여 지도를 표시합니다.

> [!NOTE]
> 문자열을 [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint)로 변환할 수 없으므로 데이터 바인딩을 사용하지 않을 경우 XAML 태그에서 [**Center**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 속성 값을 지정할 수 없습니다. 이 제한 사항은 [**MapControl.Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setlocation) 연결된 속성에도 적용됩니다.

 
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

![지도 컨트롤의 예](images/displaymapsexample1.png)

### <a name="set-the-current-location-of-the-map"></a>지도의 현재 위치 설정

앱이 사용자 위치에 액세스할 수 있으려면 먼저 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 메서드를 호출해야 합니다. 이때 앱이 포그라운드에 있어야 하고 **RequestAccessAsync**가 UI 스레드에서 호출되어야 합니다. 사용자가 자신의 위치에 대한 권한을 앱에 부여하기 전에는 앱이 위치 데이터에 액세스할 수 없습니다.

[  **Geolocator**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 클래스의 [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) 메서드를 사용하여 장치의 현재 위치를 가져옵니다(위치를 사용할 수 없는 경우). 해당 [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint)를 가져오려면 지리적 위치의 지리적 좌표에 대한 [**Point**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geocoordinate.point) 속성을 사용합니다. 자세한 내용은 [현재 위치 가져오기](get-location.md)를 참조하세요.

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

지도에 장치 위치를 표시할 때는 그래픽을 표시하고 위치 데이터의 정확도에 따라 확대/축소 수준을 설정하는 것이 좋습니다. 자세한 내용은 [위치 인식 앱에 대한 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/guidelines-and-checklist-for-detecting-location)을 참조하세요.

### <a name="change-the-location-of-the-map"></a>지도의 위치 변경

2D 지도에 표시된 위치를 변경하려면 [**TrySetViewAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewasync) 메서드의 오버로드 중 하나를 호출합니다. 이 메서드를 사용하여 [**Center**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center), [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel), [**Heading**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) 및 [**Pitch**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitch)에 대한 새 값을 지정할 수 있습니다. 또한 [**MapAnimationKind**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 열거형에서 상수를 제공하여 보기가 변경될 때 사용할 애니메이션(옵션)을 지정할 수도 있습니다.

3D 지도의 위치를 변경하려면 대신 [**TrySetSceneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync) 메서드를 사용합니다. 자세한 내용은 [3D 위성뷰 표시](#3Dviews)를 참조하세요.

[  **TrySetViewBoundsAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetviewboundsasync) 메서드를 호출하여 지도에 [**GeoboundingBox**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.GeoboundingBox)의 콘텐츠를 표시합니다. 예를 들어 이 메서드를 사용하여 지도에 경로 또는 경로의 일부를 표시합니다. 자세한 내용은 [지도에 경로 및 길 찾기 표시](routes-and-directions.md)를 참조하세요.

## <a name="change-the-appearance-of-a-map"></a>지도의 모양 변경

지도의 모양과 느낌을 사용자 지정하려면, 지도 컨트롤의 [**StyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.StyleSheet) 속성을 기존 [**MapStyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 개체 중 하나로 설정합니다.

```csharp
myMap.StyleSheet = MapStyleSheet.RoadDark();
```

![어두운 스타일 지도](images/style-dark.png)

JSON을 사용하여 사용자 지정 스타일을 정의한 다음, 이 JSON을 사용하여 [**MapStyleSheet**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet) 개체를 만들 수 있습니다.

스타일 시트를 사용 하 여 대화형으로 JSON을 만들 수 있습니다 합니다 [맵 스타일 시트 편집기](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) 응용 프로그램입니다.

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

![사용자 지정 스타일 지도](images/style-custom.png)

전체 JSON 항목에 대한 참조는 [지도 스타일 시트 참조](elements-of-map-style-sheet.md)를 보세요.

기존 시트로 시작한 후, JSON을 사용하여 원하는 요소를 재정의 할 수 있습니다. 이 예제는 기존 스타일로 시작한 후 JSON을 사용하여 물 영역의 색상만 변경한 것입니다.

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

![스타일 지도 결합](images/style-combined.png)

>[!NOTE]
>두 번째 스타일 시트에서 정의한 스타일이 첫 번째 스타일을 재정의합니다.

## <a name="set-orientation-and-perspective"></a>방향 및 원근 설정

원하는 효과에 맞는 각도를 얻기 위해 지도 카메라를 확대, 축소, 회전, 이동합니다. 다음 속성을 시도해 보세요.

-   [  **Center**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.center) 속성을 설정하여 지도의 **center**를 지리적 지점으로 설정합니다.
-   [  **ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) 속성을 1과 20 사이의 값으로 설정하여 지도의 **zoom level**을 설정합니다.
-   [  **Heading**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.heading) 속성을 설정하여 지도의 **rotation**을 설정합니다. 여기서 0 또는 360도 = North, 90 = East, 180 = South 및 270 = West입니다.
-   [  **DesiredPitch**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.desiredpitch) 속성을 0도에서 65도 사이의 값으로 설정하여 지도의 **tilt**를 설정합니다.

## <a name="show-and-hide-map-features"></a>지도 기능 표시 및 숨김

다음 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 속성 값을 설정하여 도로와 랜드마크 같은 지도의 기능을 표시하거나 숨깁니다.

* [  **LandmarksVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.landmarksvisible) 속성을 사용하거나 사용하지 않도록 설정하여 지도에 **buildings and landmarks**를 표시합니다.

  > [!NOTE]
  > 건물을 표시하거나 숨길 수는 있지만 3D로 표시되지 않도록 할 수는 없습니다.  

* [  **PedestrianFeaturesVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pedestrianfeaturesvisible) 속성을 사용하거나 사용하지 않도록 설정하여 지도에 **pedestrian features**을 표시합니다.
* [  **TrafficFlowVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trafficflowvisible) 속성을 사용하거나 사용하지 않도록 설정하여 지도에 **traffic**을 표시합니다.
* [  **WatermarkMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.watermarkmode) 속성을 [**MapWatermarkMode**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapWatermarkMode) 상수 중 하나로 설정하여 **watermark**를 지도에 표시할지 여부를 지정합니다.
* [  **MapRouteView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView)를 지도 컨트롤의 [**Routes**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) 컬렉션에 추가하여 지도에 **driving or walking route**를 표시합니다. 자세한 내용과 예제는 [지도에 경로 및 길 찾기 표시](routes-and-directions.md)를 참조하세요.

[  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)에서 고정핀, 셰이프 및 XAML 컨트롤을 표시하는 방법에 대한 자세한 내용은 [지도에 POI(안내 표시) 표시](display-poi.md)를 참조하세요.

## <a name="display-streetside-views"></a>Streetside 뷰 표시


Streetside 뷰는 지도 컨트롤의 맨 위에 표시되는 위치의 거리 수준 관점입니다.

![지도 컨트롤의 Streetside 뷰 예](images/onlystreetside-730width.png)

Streetside 뷰의 "내부" 환경이 원래 지도 컨트롤에 표시된 지도와 구분되는 것으로 간주하세요. 예를 들어 Streetside 뷰의 위치를 변경한 경우 Streetside 뷰 "아래"에 있는 지도의 위치나 모양은 변경되지 않습니다. 컨트롤의 오른쪽 위에 있는 **X**를 클릭하여 Streetside 뷰를 닫은 후에도 원래 지도는 변경되지 않은 상태로 유지됩니다.

Streetside 뷰를 표시하려면

1.  [  **IsStreetsideSupported**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.isstreetsidesupported)를 클릭하여 Streetside 뷰가 장치에서 지원되는지 확인합니다.
2.  Streetside 뷰가 지원되는 경우 [**FindNearbyAsync**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama)를 호출하여 지정된 위치 근처에 [**StreetsidePanorama**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.streetsidepanorama.findnearbyasync)를 만듭니다.
3.  [  **StreetsidePanorama**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.StreetsidePanorama)가 null이 아닌지 확인하여 주변 파노라마가 있는지 확인합니다.
4.  주변 파노라마가 있는 경우 지도 컨트롤의 [**CustomExperience**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.streetsideexperience) 속성에 대한 [**StreetsideExperience**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.customexperience)를 만듭니다.

이 예제에서는 이전 이미지와 유사한 Streetside 뷰를 표시하는 방법을 보여 줍니다.

**참고**  지도 컨트롤 크기가 너무 작아서 경우 개요 맵을 표시 되지 것입니다.

 

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
## <a name="display-aerial-3d-views"></a>3D 위성뷰 표시


[  **MapScene**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) 클래스를 사용하여 지도의 3D 원근감을 지정합니다. 지도 장면은 지도에 표시되는 3D 뷰를 나타냅니다. [  **MapCamera**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapCamera) 클래스는 이러한 뷰를 표시하는 카메라의 위치를 나타냅니다.

![지도 장면 위치에 대한 MapCamera 위치 다이어그램](images/mapcontrol-techdiagram.png)

지도 표면의 건물 및 기타 요소를 3D로 표시하려면 지도 컨트롤의 [**Style**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 속성을 [**MapStyle.Aerial3DWithRoads**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)로 설정합니다. 다음은 **Aerial3DWithRoads** 스타일의 3D 뷰에 대한 예입니다.

![3D 지도 뷰의 예](images/only3d-730width.png)

3D 뷰를 표시하려면

1.  [  **Is3DSupported**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.is3dsupported)를 확인하여 3D 뷰가 장치에서 지원되는지 확인합니다.
2.  3D 뷰가 지원되는 경우 지도 컨트롤의 [**Style**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 속성을 [**MapStyle.Aerial3DWithRoads**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)로 설정합니다.
3.  [  **CreateFromLocationAndRadius**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapScene) 및 [**CreateFromCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromlocationandradius)와 같은 다양한 **CreateFrom** 메서드 중 하나를 사용하여 [**MapScene**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapscene.createfromcamera) 개체를 만듭니다.
4.  [  **TrySetSceneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.trysetsceneasync)를 호출하여 3D 뷰를 표시합니다. 또한 [**MapAnimationKind**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapAnimationKind) 열거형에서 상수를 제공하여 보기가 변경될 때 사용할 애니메이션(옵션)을 지정할 수도 있습니다.

다음 예제에서는 3D 뷰를 표시하는 방법을 보여 줍니다.

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

## <a name="get-info-about-locations"></a>위치에 대한 정보 가져오기


[  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 메서드를 호출하여 지도의 위치에 대한 정보를 가져옵니다.

-   [**TryGetLocationFromOffset** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getlocationfromoffset) 메서드-Get 지도 컨트롤의 뷰포트에 지정된 된 지점에 해당 하는 지리적 위치입니다.
-   [**GetOffsetFromLocation** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.getoffsetfromlocation) 메서드-Get 뷰포트에 지정된 된 지리적 위치에 해당 하는 지도 컨트롤의 지점입니다.
-   [**IsLocationInView** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.islocationinview) 메서드-지정된 된 지리적 위치 지도 컨트롤의 뷰포트에 현재 표시 되는지 확인 합니다.
-   [**FindMapElementsAtOffset** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.findmapelementsatoffset) 메서드-지도 컨트롤의 뷰포트에 지정된 된 지점에 있는 지도 요소를 가져옵니다.

## <a name="handle-interaction-and-changes"></a>조작 및 변경 내용 처리


[  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 이벤트를 처리하여 지도에서 사용자 입력 제스처를 처리합니다. [  **MapInputEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.location)의 [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapinputeventargs.position) 및 [**Position**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapInputEventArgs) 속성 값을 확인하려면 지도의 지리적 위치와 제스처가 발생한 뷰포트의 실제 위치에 대한 정보를 가져옵니다.

-   [**MapTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.maptapped)
-   [**MapDoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapdoubletapped)
-   [**MapHolding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapholding)

컨트롤의 [**LoadingStatusChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.loadingstatuschanged) 이벤트를 처리하여 지도가 로드 중이거나 완전히 로드되었는지 여부를 확인합니다.

사용자나 앱에서 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 다음 이벤트를 처리하여 지도의 설정을 변경할 때 발생하는 변경 사항을 처리합니다. [지도 대 한 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)

-   [**CenterChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.centerchanged)
-   [**HeadingChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.headingchanged)
-   [**PitchChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.pitchchanged)
-   [**ZoomLevelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevelchanged)

## <a name="best-practice-recommendations"></a>모범 사례 권장 사항

-   사용자가 지리적 정보를 보기 위해 지나치게 이동하거나 확대/축소하지 않도록 충분한 화면 공간이나 전체 화면 공간을 사용하여 지도를 표시합니다.

-   지도를 사용하여 정적 정보 보기만 제공하는 경우에는 더 작은 지도를 사용하는 것이 더 나을 수 있습니다. 더 작은 정적 지도를 사용하려면 사용성에 따라 크기를 결정합니다. 화면 공간을 절약할 수 있을 정도로 작지만 충분히 읽을 수 있는 크기를 지정합니다.

-   [  **map elements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapelementsproperty)를 사용하여 지도 장면에 관심 지점을 포함합니다. 지도 장면에 오버레이되는 임시 UI로 모든 추가 정보를 표시할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [현재 위치 가져오기](get-location.md)
* [위치 인식 앱에 대한 디자인 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/guidelines-and-checklist-for-detecting-location)
* [지도에 대한 디자인 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [빌드 2015 비디오: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)
