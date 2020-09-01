---
title: 지도에 POI(관심 지점) 표시
description: 압정, 이미지, 도형 및 XAML UI 요소를 사용 하 여 map에 관심 지점 (POI)을 추가 합니다.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.date: 08/11/2017
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 압정
ms.localizationpriority: medium
ms.openlocfilehash: c27132c0728c85238b80e710c62d2e733ee1dd5d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155807"
---
# <a name="display-points-of-interest-on-a-map"></a>지도에 관심 지점이 표시 됩니다.

압정, 이미지, 도형 및 XAML UI 요소를 사용 하 여 map에 관심 지점 (POI)을 추가 합니다. POI는 관심 있는 항목을 나타내는 맵의 특정 점입니다. 예를 들어 비즈니스, 도시 또는 친구의 위치입니다.

앱에서 POI를 표시 하는 방법에 대 한 자세한 내용은 GitHub의 [Windows 유니버설 샘플 리포지토리](https://github.com/Microsoft/Windows-universal-samples) : [유니버설 Windows 플랫폼 (UWP) 맵 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)에서 다음 샘플을 다운로드 하세요.

[**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon), [**mapicon**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard), [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)및 [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) 개체를 [**mapelement슬**](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) 개체의 **mapicon** 컬렉션에 추가 하 여 지도에 압정, 이미지 및 셰이프를 표시 합니다. 그런 다음 해당 레이어 개체를 지도 컨트롤의 **계층** 컬렉션에 추가 합니다.

>[!NOTE]
> 이전 릴리스에서이 가이드는 [**mapelements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) 컬렉션에 지도 요소를 추가 하는 방법을 살펴보았습니다. 이 방법을 계속 사용할 수 있지만 새 지도 계층 모델의 장점 중 일부를 놓치지 않습니다. 자세히 알아보려면이 가이드의 [계층 작업](#layers) 섹션을 참조 하세요.

[**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl) 에 추가 하거나 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 [**자식**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) 으로 추가 하 여 지도에 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button), [**하이퍼링크 단추**](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)또는 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 과 같은 XAML 사용자 인터페이스 요소를 표시할 수도 있습니다.

지도에 포함할 요소가 많은 경우 [지도에 바둑판식으로 이미지를 오버레이](overlay-tiled-images.md)하는 것이 좋습니다. 지도에도로를 표시 하려면 [경로 및 방향 표시](routes-and-directions.md) 를 참조 하세요.

## <a name="add-a-pushpin"></a>압정 추가

[**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 클래스를 사용 하 여 지도에 선택적 텍스트와 같은 압정 이미지를 표시 합니다. 기본 이미지를 그대로 사용 하거나 [**이미지**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) 속성을 사용 하 여 사용자 지정 이미지를 제공할 수 있습니다. 다음 이미지는 [**제목**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) 속성에 값이 지정 되지 않은 **mapicon** 의 기본 이미지를 표시 합니다 .이 이미지에는 짧은 제목, 긴 제목, 긴 제목 등이 있습니다.

![길이가 다른 제목의 샘플 mapicon입니다.](images/mapctrl-mapicons.png)

다음 예제에서는 시애틀 도시의 지도를 표시 하 고 기본 이미지와 함께 [**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 의 위치를 나타내는 선택적 제목을 추가 합니다. 또한 지도를 아이콘 위에 배치 하 고 확대 합니다. 지도 컨트롤 사용에 대 한 일반적인 정보는 [2d, 3d 및 Streetside 뷰가 포함 된 지도 표시](display-maps.md)를 참조 하세요.

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

이 예제에서는 맵에 다음 POI를 표시 합니다 (중앙의 기본 이미지).

![mapicon으로 맵](images/displaypoidefault.png)

다음 코드 줄은 프로젝트의 자산 폴더에 저장 된 사용자 지정 이미지가 있는 [**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 을 표시 합니다. **Mapicon** 의 [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) 속성에는 [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference)형식의 값이 필요 합니다. 이 형식을 사용 하려면 [**Windows. stream**](/uwp/api/Windows.Storage.Streams) 네임 스페이스에 대 한 **using** 문이 필요 합니다.

>[!NOTE]
>여러 지도 아이콘에 동일한 이미지를 사용 하는 경우 최상의 성능을 위해 페이지 또는 앱 수준에서 [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) 을 선언 합니다.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

[**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 클래스를 사용할 때 다음 사항을 고려 하십시오.

-   [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) 속성은 최대 2048 × 2048 픽셀의 이미지 크기를 지원 합니다.
-   기본적으로 지도 아이콘의 이미지가 표시 되지 않을 수 있습니다. 지도에서 다른 요소나 레이블이 가려질 때 숨겨져 있을 수 있습니다. 계속 표시 하려면 지도 아이콘의 [**CollisionBehaviorDesired**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.collisionbehaviordesired) 속성을 MapElementCollisionBehavior로 설정 합니다. [**RemainVisible**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapElementCollisionBehavior).
-   [**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 의 선택적 [**제목은**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) 표시 되지 않을 수 있습니다. 텍스트가 표시 되지 않으면 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 확대/축소 [**수준**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) 속성 값을 줄여서 축소 합니다.
-   지도에 있는 특정 위치를 가리키는 [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 이미지(예제: 고정핀이나 화살표)를 표시할 경우 [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.normalizedanchorpoint) 속성의 값을 이미지에 표시되는 포인터의 대략적인 위치로 설정해 보세요. 이미지의 왼쪽 위 모퉁이를 나타내는 값 (0, 0)의 기본값을 **NormalizedAnchorPoint** 값으로 그대로 두면 지도의 [**확대/확대**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) /확대/변경 내용이 다른 위치를 가리키는 이미지가 남을 수 있습니다.
-   고도 및 [AltitudeReferenceSystem](/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem) [을 명시적](/uwp/api/windows.devices.geolocation.basicgeoposition) 으로 설정 하지 않으면 [**mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 이 화면에 배치 됩니다.

## <a name="add-a-3d-pushpin"></a>3D 압정 추가

3D 개체를 지도에 추가할 수 있습니다. [MapModel3D](/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 클래스를 사용 하 여 [3Mf (3d 제조 형식)](https://3mf.io/specification/) 파일에서 3d 개체를 가져옵니다.

이 이미지는 3D 커피 cup를 사용 하 여 한 환경에서 커피 상점 위치를 표시 합니다.

![맵의 머그잔](images/mugs.png)

다음 코드는 3MF 파일 가져오기를 사용 하 여 커피를 맵에 추가 합니다. 이 코드는 항목을 간단 하 게 유지 하기 위해 지도의 중앙에 이미지를 추가 하지만 코드가 특정 위치에 이미지를 추가할 가능성이 있습니다.

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>이미지 추가

식당의 그림 또는 랜드마크와 같은 지도 위치와 관련 된 초대형 이미지를 표시 합니다. 사용자가 축소 하면 이미지의 크기가 비례적으로 축소 되므로 사용자가 더 많은 지도를 볼 수 있습니다. 이는 특정 위치를 표시 하는 [**Mapicon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) 과 약간 다르며, 일반적으로 작으며 사용자가 지도를 확대/축소 하는 것과 동일한 크기로 유지 됩니다.

![MapBillboard 이미지](images/map-billboard.png)

다음 코드에서는 위의 이미지에 표시 된 [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 를 보여 줍니다.

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

이 코드에서는 이미지, 참조 카메라 및 [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 속성의 세 부분을 자세히 검토할 가치가 있습니다.

### <a name="image"></a>이미지

이 예제에서는 프로젝트의 **자산** 폴더에 저장 된 사용자 지정 이미지를 보여 줍니다. [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 의 [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) 속성에는 [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference)형식의 값이 필요 합니다. 이 형식을 사용 하려면 [**Windows. stream**](/uwp/api/Windows.Storage.Streams) 네임 스페이스에 대 한 **using** 문이 필요 합니다.

>[!NOTE]
>여러 지도 아이콘에 동일한 이미지를 사용 하는 경우 최상의 성능을 위해 페이지 또는 앱 수준에서 [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) 을 선언 합니다.

### <a name="reference-camera"></a>참조 카메라

 [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 이미지는 맵의 확대/축소 [**수준이**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) 변경 됨에 따라 확대 및 축소 되기 때문에 해당 확대/축소 [**수준**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) 에서 이미지가 일반 1x 배율로 표시 되는 위치를 정의 하는 것이 중요 합니다. 이 위치는 [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 참조 카메라에서 정의 되 고 설정 하기 위해 [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 개체를 [**mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 생성자에 전달 해야 합니다.

 [**Geopoint**](/uwp/api/windows.devices.geolocation.geopoint)에서 원하는 위치를 정의한 다음 해당 [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint) 를 사용 하 여 [**mapcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 개체를 만들 수 있습니다.  그러나이 예제에서는 맵 컨트롤의 [**ActualCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) 속성에서 반환 된 [**mapcamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 개체를 사용 합니다. 지도의 내부 카메라입니다. 카메라의 현재 위치는 참조 카메라 위치가 됩니다. [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 이미지가 1x 배율로 표시 되는 위치입니다.

 응용 프로그램에서 사용자에 게 지도를 축소 하는 기능을 제공 하는 경우 지도 내부 카메라는도로 증가 하 고, 1x 눈금의 이미지는 참조 카메라의 위치에서 고정 된 상태로 유지 되기 때문에 이미지 크기가 줄어듭니다.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

[**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 는 [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) 속성에 고정 된 이미지의 지점입니다. 점 0.5, 1은 이미지의 하단 가운데입니다. [**Mapbillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 의 [**Location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) 속성을 지도 컨트롤의 중심으로 설정 했기 때문에, 이미지의 아래쪽 가운데는 맵 컨트롤의 가운데에 고정 됩니다. 이미지를 점 바로 가운데에 표시 하려면 [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 를 0.5, 0.5로 설정 합니다.  

## <a name="add-a-shape"></a>도형 추가

[**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon) 클래스를 사용 하 여 지도에 여러 point 셰이프를 표시 합니다. 다음 예제에서는 [UWP 맵 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)에서 지도에 파란색 테두리가 있는 빨간색 상자를 표시 합니다.

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>선 추가


[**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) 클래스를 사용 하 여 지도에 선을 표시 합니다. 다음 예제에서는 [UWP 맵 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)에서 지도의 파선을 표시 합니다.

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>XAML 추가

XAML을 사용 하 여 맵에 사용자 지정 UI 요소를 표시 합니다. XAML의 위치 및 정규화 된 앵커 지점을 지정 하 여 맵에 XAML을 배치 합니다.

-   [**Setlocation**](/windows/desktop/tablet/icontextnode-setlocation)을 호출 하 여 XAML이 배치 되는 맵의 위치를 설정 합니다.
-   [**SetNormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setnormalizedanchorpoint)을 호출 하 여 지정 된 위치에 해당 하는 XAML의 상대 위치를 설정 합니다.

다음 예제에서는 시애틀 도시의 지도를 표시 하 고 공간 니 들의 위치를 나타내는 XAML [**테두리**](/uwp/api/Windows.UI.Xaml.Controls.Border) 컨트롤을 추가 합니다. 또한 지도를 영역 위로 가운데에 배치 하 고 확대 합니다. 지도 컨트롤 사용에 대 한 일반적인 정보는 [2d, 3d 및 Streetside 뷰가 포함 된 지도 표시](display-maps.md)를 참조 하세요.

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

이 예제에서는 맵에 파란색 테두리를 표시 합니다.

![맵의 지점에 표시 되는 xaml의 스크린샷](images/displaypoixaml.png)

다음 예제에서는 데이터 바인딩을 사용 하 여 페이지의 XAML 태그에 직접 XAML UI 요소를 추가 하는 방법을 보여 줍니다. 콘텐츠를 표시 하는 다른 XAML 요소와 마찬가지로 [**자식은**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 의 기본 콘텐츠 속성 이며 XAML 태그에서 명시적으로 지정할 필요가 없습니다.

이 예제에서는 두 개의 XAML 컨트롤을 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 암시적 자식으로 표시 하는 방법을 보여 줍니다. 이러한 컨트롤은 지도에서 데이터 바인딩된 위치에 표시 됩니다.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

코드 숨김이 설정 된 파일의 속성을 사용 하 여 이러한 위치를 설정 합니다.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

이 예제에서는 [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl)내에 포함 된 두 개의 XAML 컨트롤을 표시 하는 방법을 보여 줍니다. 이러한 컨트롤은 지도에서 데이터 바인딩된 위치에 표시 됩니다.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

이 예제에서는 [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl)에 바인딩된 XAML 요소의 컬렉션을 표시 합니다.

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

``ItemsSource``위의 예제에서 속성은 코드 숨김이 파일에서 [IList](/dotnet/api/system.collections.ilist) 형식의 속성에 바인딩되어 있습니다.

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>계층 작업

이 가이드의 예제에서는 [Mapelementlayers](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) 컬렉션에 요소를 추가 합니다. 그런 다음이 컬렉션을 지도 컨트롤의 **계층** 속성에 추가 하는 방법을 보여 줍니다. 이전 릴리스에서이 가이드는 다음과 같이 [**mapelements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) 컬렉션에 지도 요소를 추가 하는 방법을 보여 주었습니다.

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

이 방법을 계속 사용할 수 있지만 새 지도 계층 모델의 장점 중 일부를 놓치지 않습니다. 여러 요소를 레이어로 그룹화 하 여 각 계층을 서로 독립적으로 조작할 수 있습니다. 예를 들어 각 계층에 자신의 이벤트 세트가 있으므로 특정 계층에서 이벤트에 응답하고 해당 이벤트 관련 작업을 수행할 수 있습니다.

또한 XAML을 [Maplayer](/uwp/api/windows.ui.xaml.controls.maps.maplayer)에 직접 바인딩할 수 있습니다. 이 항목은 [Mapelements](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)  컬렉션을 사용 하 여 수행할 수 없습니다.

이 작업을 수행 하는 한 가지 방법은 뷰 모델 클래스, 코드 숨김으로 XAML 페이지 및 XAML 페이지를 사용 하는 것입니다.

### <a name="view-model-class"></a>모델 클래스 보기

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>XAML 페이지 코드 숨김으로

뷰 모델 클래스를 코드 뒤 페이지에 연결 합니다.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>XAML 페이지

XAML 페이지에서 계층을 반환 하는 뷰 모델 클래스의 속성에 바인딩합니다.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [지도에 대한 디자인 지침](./display-maps.md)
* [빌드 2015 비디오: Windows 앱의 휴대폰, 태블릿 및 PC에서 맵 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)
* [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)
* [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline)