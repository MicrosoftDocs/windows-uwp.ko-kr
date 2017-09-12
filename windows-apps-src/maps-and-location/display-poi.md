---
author: normesta
title: "지도에 POI(관심 지점) 표시"
description: "고정핀, 이미지, 셰이프 및 XAML UI 요소를 사용하여 지도에 POI(관심 지점)를 추가합니다."
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.author: normesta
ms.date: 08/02/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 지도, 위치, 고정핀"
ms.openlocfilehash: 280a651df949018dc9cf490e14d72d167fee0858
ms.sourcegitcommit: 0ebc8dca2fd9149ea163b7db9daa14520fc41db4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/08/2017
---
# <a name="display-points-of-interest-poi-on-a-map"></a>지도에 POI(관심 지점) 표시


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


고정핀, 이미지, 셰이프 및 XAML UI 요소를 사용하여 지도에 POI(안내 표시)를 추가합니다. POI는 관심 있는 사항을 나타내는 지도의 특정 지점입니다. 비즈니스, 도시 또는 친구의 위치를 예로 들 수 있습니다.

**팁** 앱에서 POI를 표시하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

[**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077), [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 및 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 개체를 지도 컨트롤의 [**MapElements**](https://msdn.microsoft.com/library/windows/apps/dn637033) 컬렉션에 추가하여 고정핀, 이미지 및 셰이프를 표시합니다. 데이터 바인딩을 사용하거나 항목을 프로그래밍 방식으로 추가합니다. XAML 태그에서 **MapElements** 컬렉션에 선언적으로 바인딩할 수 없습니다.

[**MapControl**](https://msdn.microsoft.com/library/windows/apps/br209265)의 [**Children**](https://msdn.microsoft.com/library/windows/apps/br242739)으로서 추가하여 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209652), [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/dn637008) 또는 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/dn637004) 같은 XAML 사용자 인터페이스 요소를 지도에 표시합니다. 또한 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094)에 추가하거나 **MapItemsControl**을 항목 컬렉션으로 바인딩할 수도 있습니다.

요약하면 다음과 같습니다.

-   선택적 텍스트와 함께 고정핀 등의 이미지를 표시하려면 [MapIcon을 지도에 추가](#mapicon)합니다.
-   다중 지점 셰이프를 표시하려면 [MapPolygon을 지도에 추가](#mappolygon)합니다.
-   선을 지도에 표시하려면 [MapPolyline을 지도에 추가](#mappolyline)합니다.
-   사용자 지정 UI 요소를 표시하려면 [XAML을 지도에 추가](#mapxaml)합니다.

지도에 배치할 요소가 많은 경우 [바둑판식 이미지를 지도에 오버레이](overlay-tiled-images.md)하는 것이 좋습니다. 지도에 도로를 표시하려면 [경로 및 길 찾기를 표시](routes-and-directions.md)합니다.

## <a name="add-a-mapicon"></a>MapIcon 추가


[**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 클래스를 사용하여 텍스트가 있거나 없는 고정핀 등과 같은 이미지를 표시합니다. 기본 이미지를 적용하거나 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 속성을 사용하여 사용자 지정 이미지를 제공할 수 있습니다. 다음 이미지는 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) 속성에 대해 지정된 값이 없고, 짧은 제목, 긴 제목 및 매우 긴 제목을 가진 **MapIcon**에 대한 기본 이미지를 표시합니다.

![다양한 길이의 제목을 가진 샘플 mapicon](images/mapctrl-mapicons.png)

다음 예제에서는 시애틀시 지도를 표시하고 기본 이미지 및 제목(옵션)과 함께 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)을 추가하여 스페이스 니들(Space Needle)의 위치를 나타냅니다. 또한 아이콘 위에 지도의 중심을 지정하고 확대합니다. 지도 컨트롤을 사용하는 방법에 대한 일반적인 정보는 [2D, 3D 및 Streetside 뷰로 지도 표시](display-maps.md)를 참조하세요.

```csharp
      private void displayPOIButton_Click(object sender, RoutedEventArgs e)
      {
         // Specify a known location.
         BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
         Geopoint snPoint = new Geopoint(snPosition);

         // Create a MapIcon.
         var mapIcon1 = new MapIcon
         {
             Location = snPoint,
             NormalizedAnchorPoint = new Point(0.5, 1.0),
             Title = "Space Needle",
             ZIndex = 0,
         };

         // Add the MapIcon to the map.
         myMap.MapElements.Add(mapIcon1);

         // Center the map over the POI.
         myMap.Center = snPoint;
         myMap.ZoomLevel = 14;
      }
```

아래 예제에서는 지도에 다음 POI(가운데에 있는 기본 이미지)를 표시합니다.

![mapicon이 있는 지도](images/displaypoidefault.png)

다음 코드 줄은 프로젝트의 Assets 폴더에 저장된 사용자 지정 이미지를 사용하여 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)을 표시합니다. **MapIcon**의 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 속성에는 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 형식의 값이 필요합니다. 이 형식에는 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 네임스페이스용 **using** 문이 필요합니다.

**팁** 여러 지도 아이콘에 같은 이미지를 사용할 경우 최상의 성능을 위해 페이지 또는 앱 수준에서 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813)를 선언합니다.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

[**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 클래스로 작업할 때 고려해야 할 사항은 다음과 같습니다.

-   [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 속성은 최대 2048×2048 픽셀의 이미지 크기를 지원합니다.
-   기본적으로 지도 아이콘의 이미지는 표시되지 않을 수도 있습니다. 지도에 있는 다른 요소나 레이블을 가릴 경우 MapIcon이 숨겨질 수도 있습니다. 표시된 상태로 유지하려면 지도 아이콘의 [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) 속성을 [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314)로 설정합니다.
-   [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637088)의 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637077)(옵션)이(가) 표시되지 않을 수도 있습니다. 텍스트가 보이지 않으면 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637068)의 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637004) 속성에 대한 값을 낮춰서 축소합니다.
-   지도에 있는 특정 위치를 가리키는 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 이미지(예제: 고정핀이나 화살표)를 표시할 경우 [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) 속성의 값을 이미지에 표시되는 포인터의 대략적인 위치로 설정해 보세요. **NormalizedAnchorPoint** 값을 이미지의 오른쪽 위 모서리를 나타내는 기본값인 (0, 0)으로 유지하면 지도의 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068)을 변경할 경우 이미지가 다른 위치를 가리킬 수 있습니다.

## <a name="add-a-mapbillboard"></a>MapBillboard 추가

식당이나 랜드마크 사진 같은 지도 위치와 관련된 큰 이미지를 표시합니다. 사용자가 축소하면, 사용자가 지도의 더 많은 부분을 볼 수 있도록 이미지 크기가 비례해 축소됩니다. 이는 일반적으로 작고, 사용자가 지도를 축소 또는 확대했을 떄 동일한 크기를 유지하고, 특정 위치를 표시한 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)과 다릅니다.

![MapBillboard 이미지](images/map-billboard.png)

다음은 위 이미지에 제시한 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)를 표시하는 코드입니다.

```csharp
private void mapBillboardAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

        var mapBillboard = new MapBillboard(myMap.ActualCamera)
        {
            Location = myMap.Center,
            NormalizedAnchorPoint = new Point(0.5, 1.0),
            Image = mapBillboardStreamReference
        };

        myMap.MapElements.Add(mapBillboard);
}
```
코드에는 조금 더 자세히 검토할 3가지 부분이 있습니다. 이미지, 참조 카메라, [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard#Windows_UI_Xaml_Controls_Maps_MapBillboard_NormalizedAnchorPoint) 속성입니다.

### <a name="image"></a>이미지

프로젝트의 **Assets** 폴더에 저장된 사용자 지정 이미지를 표시하는 예제입니다. [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 [**이미지**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard#Windows_UI_Xaml_Controls_Maps_MapBillboard_Image) 속성은 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 유형의 값이 필요합니다. 이 형식에는 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 네임스페이스용 **using** 문이 필요합니다.

>[!NOTE]
>여러 지도 아이콘에 같은 이미지를 사용할 경우 최상의 성능을 위해 페이지 또는 앱 수준에서 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813)를 선언합니다.

### <a name="reference-camera"></a>참조 카메라

 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 이미지가 지도의 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol#Windows_UI_Xaml_Controls_Maps_MapControl_ZoomLevel) 변경에 따라 크기가 축소 또는 확대되기 때문에, [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol#Windows_UI_Xaml_Controls_Maps_MapControl_ZoomLevel)에서 이미지가 일반적인 1x 크기로 표시되도록 정의하는 것이 중요합니다. 이 위치는 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 참조 카메라에서 정의되어 설정됩니다. [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 개체를 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 생성자로 보내야 합니다.

 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)에서 원하는 위치를 지정한 후, 이 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)를 사용하여 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 개체를 만듭니다.  그러나 저희는 이 예제에서 지도 컨트롤의 [**ActualCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol#Windows_UI_Xaml_Controls_Maps_MapControl_ActualCamera)가 반환한 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 개체만 사용합니다. 지도의 내부 카메라입니다. 해당 카메라의 현재 위치는 참조 카메라 위치입니다. [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 이미지가 1 x 크기로 표시되는 위치입니다.

 앱이 사용자에게 지도를 확대할 수 있는 기능을 부여할 경우, 이미지 크기가 축소됩니다. 지도 내부 카메라의 고도가 상승하고, 1x 크기인 이미지가 참조 카메라 위치에 고정되기 때문입니다.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

[**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard#Windows_UI_Xaml_Controls_Maps_MapBillboard_NormalizedAnchorPoint)는 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 [**위치**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard#Windows_UI_Xaml_Controls_Maps_MapBillboard_Location) 속성에 고정된 이미지의 지점입니다. 지점 0,5,1이 이미지의 아래쪽 중앙입니다. [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)의 [**위치**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard#Windows_UI_Xaml_Controls_Maps_MapBillboard_Location) 속성을 지도 컨트롤의 중앙으로 설정했기 때문에, 이미지 아래쪽 중앙이 지도 컨트롤 중앙에 고정됩니다.

## <a name="add-a-mappolygon"></a>MapPolygon 추가

[**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 클래스를 사용하여 지도에 다중 지점 셰이프를 표시할 수 있습니다. 다음 예제에서는 [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)에서 파란색 테두리가 있는 빨간색 상자를 지도에 표시합니다.

```csharp
private void mapPolygonAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon()
    {
        Path = new Geopath(new List<BasicGeoposition>() {
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

    myMap.MapElements.Add(mapPolygon);
}
```

## <a name="add-a-mappolyline"></a>MapPolyline 추가


[**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 클래스를 사용하여 지도에 선을 표시합니다. 다음 예제에서는 [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)에서 파선을 지도에 표시합니다.

```csharp
private void mapPolylineAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition>() {
            new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
            new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
        }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

    myMap.MapElements.Add(mapPolyline);
}
```

## <a name="add-xaml"></a>XAML 추가

XAML을 사용하여 지도에 사용자 지정 UI 요소를 표시할 수 있습니다. XAML의 위치 및 정규화된 앵커 지점을 지정하여 XAML을 지도에 배치합니다.

-   [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369)을 호출하여 지도에서 XAML이 배치되는 위치를 설정합니다.
-   [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050)를 호출하여 XAML에서 지정된 위치에 해당하는 상대적 위치를 설정하려면 합니다.

다음 예제에서는 시애틀시 지도를 표시하고 XAML [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 컨트롤을 추가하여 Space Needle의 위치를 나타냅니다. 또한 영역 위에 지도의 중심을 지정하고 확대합니다. 지도 컨트롤을 사용하는 방법에 대한 일반적인 정보는 [2D, 3D 및 Streetside 뷰로 지도 표시](display-maps.md)를 참조하세요.

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
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

이 예제에서는 지도에 파란색 테두리를 표시합니다.

![지도상의 관심 지점에 표시된 xaml의 스크린샷](images/displaypoixaml.png)

다음 예제에서는 데이터 바인딩을 사용하여 페이지의 XAML 태그에 직접 XAML UI 요소를 추가하는 방법을 보여 줍니다. 콘텐츠를 표시하는 다른 XAML 요소와 마찬가지로, [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008)은 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 기본 콘텐츠 속성이므로 XAML 태그에서 명시적으로 지정할 필요가 없습니다.

다음 예제에서는 두 XAML 컨트롤을 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 암시적 자식으로 표시하는 방법을 보여 줍니다.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
</maps:MapControl>
```

다음 예제에서는 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 내에 포함된 두 XAML 컨트롤을 표시하는 방법을 보여 줍니다.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

다음 예제에서는 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094)에 바인딩된 XAML 요소의 컬렉션을 표시합니다.

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{Binding StateOverlays}">
    <maps:MapItemsControl.ItemTemplate>
      <DataTemplate>
        <StackPanel Background="Black" Tapped="Overlay_Tapped">
          <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Name}" maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
        </StackPanel>
      </DataTemplate>
    </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)
