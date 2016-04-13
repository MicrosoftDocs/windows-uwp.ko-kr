---
'2D, 3D 및 Streetside 뷰가 있는 지도 표시'
MapControl 클래스를 사용하여 앱에서 사용자 지정 가능한 지도를 표시하세요. 이 항목에서는 3D 위성뷰 및 Streetside 뷰도 소개합니다.
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
---

# 2D, 3D 및 Streetside 뷰가 있는 지도 표시


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 클래스를 사용하여 앱에서 사용자 지정 가능한 지도를 표시하세요. 이 항목에서는 3D 위성뷰 및 Streetside 뷰도 소개합니다.

**팁** 앱에서 지도를 사용하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## 지도 컨트롤을 앱에 추가합니다.


[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)을 추가하여 XAML 페이지에 지도를 표시합니다. **MapControl**을 사용하려면 XAML 페이지 또는 코드에서 [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 네임스페이스를 선언해야 합니다. 도구 상자에서 이 컨트롤을 끌면 이 네임스페이스 선언이 자동으로 추가됩니다. **MapControl**을 XAML 페이지에 수동으로 추가할 경우 페이지 맨 위에 네임스페이스 선언을 수동으로 추가해야 합니다.

다음 예제에서는 기본 지도 컨트롤을 표시하며, 터치식 입력을 허용하는 것 외에 확대/축소 및 이동(상하) 컨트롤을 표시하도록 지도를 구성합니다. 지도의 모양을 사용자 지정하는 방법에 대한 자세한 내용은 [지도 구성](#mapconfig)을 참조하세요.

```xaml
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

## 지도 인증 키 얻기 및 설정


[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 및 지도 서비스를 사용하려면 지도 인증 키를 [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) 속성 값으로 지정해야 합니다. 위 예제에서 `EnterYourAuthenticationKeyHere`을 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 가져온 키로 바꿉니다. 지도 인증 키를 지정할 때까지 텍스트 **경고: MapServiceToken이 지정되지 않음**이 컨트롤 아래에 계속 표시됩니다. 지도 인증 키를 얻고 설정하는 방법에 대한 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조하세요.

## 지도의 시작 위치 설정


코드에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 속성을 지정하거나 XAML 태그에서 속성을 바인딩하여 지도에 표시할 위치를 지정합니다. 다음 예에서는 시애틀시를 중심으로 하여 지도를 표시합니다.

**팁** 문자열을 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)로 변환할 수 없으므로 데이터 바인딩을 사용하지 않을 경우 XAML 태그에서 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 속성 값을 지정할 수 없습니다. 이 제한 사항은 [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264) 연결된 속성에도 적용됩니다.

 

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

## 지도의 현재 위치 설정


앱이 사용자 위치에 액세스할 수 있으려면 먼저 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 메서드를 호출해야 합니다. 이때 앱이 포그라운드에 있어야 하고 **RequestAccessAsync**이(가) UI 스레드에서 호출되어야 합니다. 사용자가 자신의 위치에 대한 권한을 앱에 부여하기 전에는 앱이 위치 데이터에 액세스할 수 없습니다.

[
            **Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 클래스의 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 메서드를 사용하여 장치의 현재 위치를 가져옵니다(위치를 사용할 수 없는 경우). 해당 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)를 가져오려면 지리적 위치의 지리적 좌표에 대한 [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665) 속성을 사용합니다. 자세한 내용은 [현재 위치 가져오기](get-location.md)를 참조하세요.

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

지도에 장치 위치를 표시할 때는 그래픽을 표시하고 위치 데이터의 정확도에 따라 확대/축소 수준을 설정하는 것이 좋습니다. 자세한 내용은 [위치 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465148)을 참조하세요.

## 지도의 위치 변경


2D 지도에 표시된 위치를 변경하려면 [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060) 메서드의 오버로드 중 하나를 호출합니다. 이 메서드를 사용하여 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005), [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068), [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 및 [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044)에 대한 새 값을 지정할 수 있습니다. 또한 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 열거형에서 상수를 제공하여 보기가 변경될 때 사용할 애니메이션(옵션)을 지정할 수도 있습니다.

3D 지도의 위치를 변경하려면 대신 [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) 메서드를 사용합니다. 자세한 내용은 [3D 뷰 표시](#display3d)를 참조하세요.

[
            **TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065) 메서드를 호출하여 지도에 [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949)의 콘텐츠를 표시합니다. 예를 들어 이 메서드를 사용하여 지도에 경로 또는 경로의 일부를 표시합니다. 자세한 내용은 [지도에 경로 및 길 찾기 표시](routes-and-directions.md)를 참조하세요.

## 디스크 구성


[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 다음 속성 값을 설정하여 지도와 모양을 구성합니다.

**지도 설정**

-   [
            **Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 속성을 설정하여 지도의 **center**를 지리적 지점으로 설정합니다.
-   [
            **ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 속성을 1과 20 사이의 값으로 설정하여 지도의 **zoom level**을 설정합니다.
-   [
            **Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 속성을 설정하여 지도의 **rotation**을 설정합니다. 여기서 0 또는 360도 = North, 90 = East, 180 = South 및 270 = West입니다.
-   [
            **DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) 속성을 0도에서 65도 사이의 값으로 설정하여 지도의 **tilt**를 설정합니다.

**지도 모양**

-   [
            **MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127) 상수 중 하나로 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 속성을 설정하여 지도의 **type**(예를 들면 도로 지도나 항공 지도)을 지정합니다.
-   [
            **ColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637010) 속성을 [**MapColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637003) 상수 중 하나로 설정하여 지도의 **color scheme**을 밝게 또는 어둡게 설정합니다.

[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 다음 속성에 대한 값을 설정하여 지도에 정보를 표시합니다.

-   [
            **LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023) 속성을 사용하거나 사용하지 않도록 설정하여 지도에 **buildings and landmarks**를 표시합니다.
-   [
            **PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042) 속성을 사용하거나 사용하지 않도록 설정하여 지도에 **pedestrian features**을 표시합니다.
-   [
            **TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055) 속성을 사용하거나 사용하지 않도록 설정하여 지도에 **traffic**을 표시합니다.
-   [
            **WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) 속성을 [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749) 상수 중 하나로 설정하여 **watermark**를 지도에 표시할지 여부를 지정합니다.
-   [
            **MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122)를 지도 컨트롤의 [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) 컬렉션에 추가하여 지도에 **driving or walking route**를 표시합니다. 자세한 내용과 예제는 [지도에 경로 및 길 찾기 표시](routes-and-directions.md)를 참조하세요.

[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)에서 고정핀, 셰이프 및 XAML 컨트롤을 표시하는 방법에 대한 자세한 내용은 [지도에 POI(안내 표시) 표시](display-poi.md)를 참조하세요.

## Streetside 뷰 표시


Streetside 뷰는 지도 컨트롤의 맨 위에 표시되는 위치의 거리 수준 관점입니다.

![지도 컨트롤의 Streetside 뷰 예](images/onlystreetside-730width.png)

Streetside 뷰의 "내부" 환경이 원래 지도 컨트롤에 표시된 지도와 구분되는 것으로 간주하세요. 예를 들어 Streetside 뷰의 위치를 변경한 경우 Streetside 뷰 "아래"에 있는 지도의 위치나 모양은 변경되지 않습니다. 컨트롤의 오른쪽 위에 있는 **X**를 클릭하여 Streetside 뷰를 닫은 후에도 원래 지도는 변경되지 않은 상태로 유지됩니다.

Streetside 뷰를 표시하려면

1.  [
            **IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271)를 클릭하여 Streetside 뷰가 장치에서 지원되는지 확인합니다.
2.  Streetside 뷰가 지원되는 경우 [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974361)를 호출하여 지정된 위치 근처에 [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360)를 만듭니다.
3.  [
            **StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360)가 null이 아닌지 확인하여 주변 파노라마가 있는지 확인합니다.
4.  주변 파노라마가 있는 경우 지도 컨트롤의 [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263) 속성에 대한 [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356)를 만듭니다.

이 예제에서는 이전 이미지와 유사한 Streetside 뷰를 표시하는 방법을 보여 줍니다.

**참고** 지도 컨트롤이 지나치게 작으면 개요 지도가 표시되지 않습니다.

 

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

## 3D 위성뷰 표시


[
            **MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) 클래스를 사용하여 지도의 3D 원근감을 지정합니다. 지도 장면은 지도에 표시되는 3D 뷰를 나타냅니다. [
            **MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) 클래스는 이러한 뷰를 표시하는 카메라의 위치를 나타냅니다.

![](images/mapcontrol-techdiagram.png)

지도 표면의 건물 및 기타 기능을 3D로 표시하려면 지도 컨트롤의 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 속성을 [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127)로 설정합니다. 다음은 **Aerial3DWithRoads** 스타일의 3D 뷰에 대한 예입니다.

![3D 지도 뷰의 예](images/only3d-730width.png)

3D 뷰를 표시하려면

1.  [
            **Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265)를 확인하여 3D 뷰가 장치에서 지원되는지 확인합니다.
2.  3D 뷰가 지원되는 경우 지도 컨트롤의 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 속성을 [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127)로 설정합니다.
3.  [
            **CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974336) 및 [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974334)와 같은 다양한 **CreateFrom** 메서드 중 하나를 사용하여 [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) 개체를 만듭니다.
4.  [
            **TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296)를 호출하여 3D 뷰를 표시합니다. 또한 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 열거형에서 상수를 제공하여 보기가 변경될 때 사용할 애니메이션(옵션)을 지정할 수도 있습니다.

다음 예제에서는 3D 뷰를 표시하는 방법을 보여 줍니다.

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 34.134, Longitude = -118.3216};
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

## 위치 및 요소에 대한 정보 가져오기


[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 다음 메서드를 호출하여 지도의 위치에 대한 정보를 가져옵니다.

-   [
            **GetLocationFromOffset**](https://msdn.microsoft.com/library/windows/apps/dn637016) 메서드 - 지도 컨트롤의 뷰포트에서 지정된 지점에 해당하는 지리적 위치를 가져옵니다.
-   [
            **GetOffsetFromLocation**](https://msdn.microsoft.com/library/windows/apps/dn637018) 메서드 - 지도 컨트롤의 뷰포트에서 지정된 지리적 위치에 해당하는 지점을 가져옵니다.
-   [
            **IsLocationInView**](https://msdn.microsoft.com/library/windows/apps/dn637022) 메서드 - 지정된 지리적 위치가 현재 지도 컨트롤의 뷰포트에 표시되는지 여부를 확인합니다.
-   [
            **FindMapElementsAtOffset**](https://msdn.microsoft.com/library/windows/apps/dn637014) 메서드 - 지도 컨트롤의 뷰포트에서 지정된 지점에 있는 지도 요소를 가져옵니다.

## 사용자 조작 및 변경 내용 처리


[
            **MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 다음 이벤트를 처리하여 지도에서 사용자 입력 제스처를 처리합니다. [
            **MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637090)의 [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637091) 및 [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637093) 속성 값을 확인하려면 지도의 지리적 위치와 제스처가 발생한 뷰포트의 실제 위치에 대한 정보를 가져옵니다.

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

컨트롤의 [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028) 이벤트를 처리하여 지도가 로드 중이거나 완전히 로드되었는지 여부를 확인합니다.

사용자나 앱에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 다음 이벤트를 처리하여 지도의 설정을 변경할 때 발생하는 변경 사항을 처리합니다. [지도에 대한 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## 관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [현재 위치 가져오기](get-location.md)
* [위치 인식 앱에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)




<!--HONumber=Mar16_HO1-->


