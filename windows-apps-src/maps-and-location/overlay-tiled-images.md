---
title: 지도에 바둑판식 이미지 오버레이
description: '타일 소스를 사용하여 지도에 타사 또는 사용자 지정 바둑판식 이미지를 오버레이합니다. 타일 소스를 사용하여 특수 정보(예제: 날씨 데이터, 인구 데이터, 지진 데이터 등)를 오버레이하거나 기본 지도를 전체적으로 바꿉니다.'
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, 지도, 위치, 이미지, 오버레이
ms.localizationpriority: medium
ms.openlocfilehash: 8d4a8e3f8a8566fbaae64d44fe876808f094bdd5
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297631"
---
# <a name="overlay-tiled-images-on-a-map"></a>지도에 바둑판식 이미지 오버레이

> [!NOTE]
> [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 및 map services Requite는 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)라는 지도 인증 키를 만듭니다. Maps 인증 키를 가져오고 설정 하는 방법에 대 한 자세한 내용은 [지도 인증 키 요청](authentication-key.md)을 참조 하세요.

타일 소스를 사용하여 지도에 타사 또는 사용자 지정 바둑판식 이미지를 오버레이합니다. 타일 소스를 사용하여 특수 정보(예제: 날씨 데이터, 인구 데이터, 지진 데이터 등)를 오버레이하거나 기본 지도를 전체적으로 바꿉니다.

**팁** 앱에서 지도를 사용 하는 방법에 대 한 자세한 내용을 보려면 Github에서 [유니버설 Windows 플랫폼 (UWP) 맵 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 을 다운로드 하세요.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>바둑판식 이미지 개요

검색 및 표시를 위해 Nokia 맵, Bing Maps, 잘라내기 맵 등의 서비스를 정사각형 타일에 매핑합니다. 이러한 타일은 256 x 256 픽셀 크기 이며 여러 세부 수준으로 미리 렌더링 됩니다. 많은 타사 서비스에서는 타일로 잘린 맵 기반 데이터도 제공 합니다. 타일 소스를 사용 하 여 타사 타일을 검색 하거나 고유한 사용자 지정 타일을 만들고 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)표시 된 지도에 오버레이 합니다.

**중요**    타일 소스를 사용 하는 경우 개별 타일을 요청 하거나 배치 하는 코드를 작성할 필요가 없습니다. [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 는 필요에 따라 타일을 요청 합니다. 각 요청은 개별 타일에 대 한 X 및 Y 좌표와 확대/축소 수준을 지정 합니다. **Uriformatstring** 속성에서 타일을 검색 하는 데 사용할 Uri 또는 파일의 형식을 지정 하기만 하면 됩니다. 즉, 기본 Uri 또는 파일 이름에 대체 가능 매개 변수를 삽입 하 여 각 타일에 대 한 X 및 Y 좌표와 확대/축소 수준을 전달할 위치를 지정 합니다.

다음은 X 및 Y 좌표와 확대/축소 수준에 대 한 대체 가능 매개 변수를 보여 주는 [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 의 [**uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 속성 예입니다.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

X 및 Y 좌표는 지정 된 세부 수준에서 전 세계 지도 내에서 개별 타일의 위치를 나타냅니다. 타일 번호 매기기 시스템은 지도의 왼쪽 위 모퉁이에 있는 {0, 0}부터 시작 합니다. 예를 들어 {1, 2}의 타일은 타일 그리드의 세 번째 행의 두 번째 열에 있습니다.

매핑 서비스에서 사용 하는 타일 시스템에 대 한 자세한 내용은 [Bing Maps 타일 시스템](/bingmaps/articles/bing-maps-tile-system)을 참조 하세요.

### <a name="overlay-tiles-from-a-tile-source"></a>타일 원본의 오버레이 타일

[**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource)를 사용 하 여 맵의 타일 원본에서 바둑판식으로 배열 된 이미지를 오버레이 합니다.

1.  [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource)에서 상속 되는 세 개의 타일 데이터 소스 클래스 중 하나를 인스턴스화합니다.

    -   [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)
    -   [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)
    -   [**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)

    기본 Uri 또는 파일 이름에 대체 가능 매개 변수를 삽입 하 여 타일을 요청 하는 데 사용할 **Uriformatstring** 을 구성 합니다.

    다음 예제에서는 [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)를 인스턴스화합니다. 이 예에서는 **HttpMapTileDataSource**의 생성자에서 [**uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 의 값을 지정 합니다.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)를 인스턴스화하고 구성 합니다. 이전 단계에서 구성한 [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) 을 **MapTileSource**의 [**데이터 원본**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) 으로 지정 합니다.

    다음 예제에서는 [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)의 생성자에서 [**DataSource**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) 를 지정 합니다.

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)의 속성을 사용 하 여 타일이 표시 되는 조건을 제한할 수 있습니다.

    -   [**범위**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds) 속성에 대 한 값을 제공 하 여 특정 지리적 영역 내 에서만 타일을 표시 합니다.
    -   확대/확대 [**범위**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) 속성에 대 한 값을 제공 하 여 특정 세부 수준 에서만 타일을 표시 합니다.

    필요에 따라 [**계층**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer), [**allow과잉 늘이기**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.allowoverstretch), [**IsRetryEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.isretryenabled)및 [**IsTransparencyEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.istransparencyenabled)와 같은 타일의 로드 나 표시에 영향을 주는 [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 의 다른 속성을 구성 합니다.

3.  [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 [**TileSources**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.tilesources) 컬렉션에 [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 를 추가 합니다.

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>웹 서비스의 오버레이 타일


[**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)를 사용 하 여 웹 서비스에서 검색 된 바둑판식 바둑판식 이미지를 오버레이 합니다.

1.  [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)를 인스턴스화합니다.
2.  웹 서비스가 [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 속성 값으로 예상 하는 Uri의 형식을 지정 합니다. 이 값을 만들려면 기본 URI에 대체 가능한 매개 변수를 삽입합니다. 예를 들어 다음 코드 샘플에서 **Uriformatstring** 의 값은입니다.

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    웹 서비스는 대체 가능 매개 변수 {x}, {y} 및 {가 나 수준}를 포함 하는 Uri를 지원 해야 합니다. 대부분의 웹 서비스 (예: Nokia, Bing, Google)는이 형식의 Uri를 지원 합니다. 웹 서비스에 [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 속성과 함께 사용할 수 없는 추가 인수가 필요한 경우에는 사용자 지정 Uri를 만들어야 합니다. [**Urirequested**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.urirequested) 된 이벤트를 처리 하 여 사용자 지정 Uri를 만들고 반환 합니다. 자세한 내용은이 항목의 뒷부분에 나오는 [사용자 지정 URI 제공](#customuri) 단원을 참조 하십시오.

3.  그런 다음 [바둑판식 이미지 개요](#tileintro)에서 앞에서 설명한 나머지 단계를 따릅니다.

다음 예제에서는 북아메리카 맵의 가상 웹 서비스에서 타일을 오버레이 합니다. [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 의 값은 [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)의 생성자에 지정 됩니다. 이 예제에서 타일은 선택적 [**경계**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds) 속성으로 지정 된 지리적 경계 내에만 표시 됩니다.

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>로컬 저장소의 오버레이 타일


[**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)를 사용 하 여 로컬 저장소에 파일로 저장 된 바둑판식 바둑판식 이미지 오버레이 일반적으로 앱을 사용 하 여 이러한 파일을 패키지 하 고 배포 합니다.

1.  [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)를 인스턴스화합니다.
2.  파일 이름 형식을 [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 속성의 값으로 지정 합니다. 이 값을 만들려면 기본 파일 이름에 대체 가능한 매개 변수를 삽입 합니다. 예를 들어 다음 코드 샘플에서 [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 의 값은입니다.

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    파일 이름 형식에 [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 속성에 사용할 수 없는 추가 인수가 필요한 경우에는 사용자 지정 Uri를 만들어야 합니다. [**Urirequested**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.urirequested) 된 이벤트를 처리 하 여 사용자 지정 Uri를 만들고 반환 합니다. 자세한 내용은이 항목의 뒷부분에 나오는 [사용자 지정 URI 제공](#customuri) 단원을 참조 하십시오.

3.  그런 다음 [바둑판식 이미지 개요](#tileintro)에서 앞에서 설명한 나머지 단계를 따릅니다.

다음 프로토콜 및 위치를 사용 하 여 로컬 저장소에서 타일을 로드할 수 있습니다.

| URI | 추가 정보 |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | 응용 프로그램 설치 폴더의 루트를 가리킵니다. |
|  | [InstalledLocation](/uwp/api/windows.applicationmodel.package.installedlocation) 속성에서 참조 하는 위치입니다. |
| ms-appdata:///local | 는 앱의 로컬 저장소의 루트를 가리킵니다. |
|  | [Applicationdata. LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) 속성에서 참조 하는 위치입니다. |
| ms-appdata://\temp | 응용 프로그램의 임시 폴더를 가리킵니다. |
|  | [Applicationdata](/uwp/api/windows.storage.applicationdata.temporaryfolder) 속성에서 참조 하는 위치입니다. |

 

다음 예제에서는 프로토콜을 사용 하 여 앱의 설치 폴더에 파일로 저장 된 타일을 로드 합니다 `ms-appx:///` . [**Uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 의 값은 [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)의 생성자에 지정 됩니다. 이 예에서 타일은 지도의 확대/축소 [**수준이 선택적 확대**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) /축소 속성으로 지정 된 범위 내에 있는 경우에만 표시 됩니다.

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>사용자 지정 URI 제공

[**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 또는 [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) 의 [**uriformatstring**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 속성 [**에서 사용할**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 수 있는 대체 가능 매개 변수가 타일을 검색 하기에 충분 하지 않은 경우 사용자 지정 Uri를 만들어야 합니다. **Urirequested** 된 이벤트에 대 한 사용자 지정 처리기를 제공 하 여 사용자 지정 Uri를 만들고 반환 합니다. **Urirequested** 된 이벤트는 각 개별 타일에 대해 발생 합니다.

1.  **Urirequested** 된 이벤트에 대 한 사용자 지정 처리기에서 필요한 사용자 지정 인수를 [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs) 의 [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.y)및 확대/확대/확대/확대/확대/확대//확대/확대/동시 [**수준**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.zoomlevel) 속성과 결합 하 여 사용자
2.  [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs)의 [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.request) 속성에 포함 된 [**MapTileUriRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequest)의 [**uri**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequest.uri) 속성에서 사용자 지정 uri를 반환 합니다.

다음 예제에서는 **Urirequested** 된 이벤트에 대 한 사용자 지정 처리기를 만들어 사용자 지정 Uri를 제공 하는 방법을 보여 줍니다. 또한 사용자 지정 Uri를 만들기 위해 비동기 작업을 수행 해야 하는 경우 지연 패턴을 구현 하는 방법을 보여 줍니다.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>사용자 지정 원본의 오버레이 타일

[**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)를 사용 하 여 사용자 지정 타일을 오버레이 합니다. 즉시 메모리에 바둑판식으로 타일을 만들거나 다른 원본에서 기존 타일을 로드 하는 코드를 직접 작성 합니다.

사용자 지정 타일을 만들거나 로드 하려면 [**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) 이벤트에 대 한 사용자 지정 처리기를 제공 합니다. **BitmapRequested** 이벤트는 각 개별 타일에 대해 발생 합니다.

1.  [**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) 이벤트에 대 한 사용자 지정 처리기에서 필요한 사용자 지정 인수를 MapTileBitmapRequestedEventArgs의 [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y)및 확대/확대/확대/확대/확대/확대//확대/확대/ [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) [**속성과 결합**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) 하 여 사용자 지정 타일을 만들거나 검색
2.  [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs)의 [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.request) 속성에 포함 된 [**MapTileBitmapRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequest)의 [**PixelData**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequest.pixeldata) 속성에서 사용자 지정 타일을 반환 합니다. **PixelData** 속성은 [**IRandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.IRandomAccessStreamReference)형식입니다.

다음 예제에서는 **BitmapRequested** 이벤트에 대 한 사용자 지정 처리기를 만들어 사용자 지정 타일을 제공 하는 방법을 보여 줍니다. 이 예제에서는 부분적으로 불투명 한 동일한 빨간색 타일을 만듭니다. 이 예에서는 [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs)의 [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y)및 확대/확대/확대/확대/확대/확대/ [**의 속성을**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) 무시 실제 예제는 아니지만이 예제에서는 즉시 메모리 내 사용자 지정 타일을 만드는 방법을 보여 줍니다. 또한이 예제에서는 사용자 지정 타일을 만들기 위해 비동기적으로 작업을 수행 해야 하는 경우 지연 패턴을 구현 하는 방법을 보여 줍니다.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>기본 맵 바꾸기

기본 맵을 모두 타사 또는 사용자 지정 타일로 바꾸려면 다음을 수행 합니다.

-   [**MapTileLayer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileLayer)를 지정 합니다. **BackgroundReplacement** 는 [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)의 [**Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer) 속성 값으로 합니다.
-   [**Mapstyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle)을 지정 합니다. **None** 은 [**없습니다**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)의 [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 속성 값입니다.

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [지도에 대한 디자인 지침](./display-maps.md)
* [빌드 2015 비디오: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
