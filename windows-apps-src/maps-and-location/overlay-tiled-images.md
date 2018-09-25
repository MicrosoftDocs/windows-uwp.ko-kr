---
author: normesta
title: 지도에서 바둑판식 이미지 오버레이
description: '타일 소스를 사용하여 지도에 타사 또는 사용자 지정 바둑판식 이미지를 오버레이합니다. 타일 소스를 사용하여 특수 정보(예제: 날씨 데이터, 인구 데이터, 지진 데이터 등)를 오버레이하거나 기본 지도를 전체적으로 바꿉니다.'
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.author: normesta
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 지도, 위치, 이미지, 오버레이
ms.localizationpriority: medium
ms.openlocfilehash: ba1f7d52a1b16fbb421202229ce724dab384ffa0
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4179510"
---
# <a name="overlay-tiled-images-on-a-map"></a>지도에서 바둑판식 이미지 오버레이

타일 소스를 사용하여 지도에 타사 또는 사용자 지정 바둑판식 이미지를 오버레이합니다. 타일 소스를 사용하여 특수 정보(예제: 날씨 데이터, 인구 데이터, 지진 데이터 등)를 오버레이하거나 기본 지도를 전체적으로 바꿉니다.

**팁** 앱에서 지도 사용에 대한 자세한 내용을 보려면 Github에서 [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)을 다운로드하세요.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>바둑판식 이미지 개요

지도 서비스(예제: Nokia 지도 및 Bing 지도)에서는 빠른 검색과 표시를 위해 지도를 사각형 타일로 자릅니다. 이 타일은 256x256픽셀 크기이며 여러 수준의 정보에 미리 렌더링되어 있습니다. 또한 여러 타사 서비스에서 타일로 잘라낸 지도 기반 데이터를 제공합니다. 타일 소스를 사용하여 타사 타일을 검색하거나 사용자 지정 타일을 만든 다음 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)에 표시된 지도에서 타일을 오버레이합니다.

**중요**  
타일 소스를 사용하면 개별 타일을 요청하거나 배치하는 코드를 작성할 필요가 없습니다. [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)이 필요한 타일을 요청합니다. 각 요청은 개별 타일의 X 및 Y 좌표와 확대/축소 수준을 지정합니다. 사용할 URI 또는 파일 이름의 형식만 지정하여 **UriFormatString** 속성에서 타일을 검색합니다. 즉, 기본 URI 또는 파일 이름에 대체 가능한 매개 변수를 삽입하여 각 타일에 대해 X 및 Y 좌표와 확대/축소 수준을 전달할 위치를 나타냅니다.

다음 예에서는 X 및 Y 좌표와 확대/축소 수준에 대한 대체 가능한 매개 변수를 표시하는 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)의 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 속성을 보여 줍니다.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

X 및 Y 좌표는 지정된 정보 수준에서 세계 지도 내의 개별 타일 위치를 나타냅니다. 타일 번호 지정 시스템은 지도의 왼쪽 위 모서리를 기준으로 {0, 0}부터 시작합니다. 예를 들어 {1, 2}의 타일은 타일 그리드의 세 번째 행의 두 번째 열에 있습니다.

매핑 서비스에서 사용되는 타일 시스템에 대한 자세한 내용은 [Bing 지도 타일 시스템](http://go.microsoft.com/fwlink/p/?LinkId=626692)을 참조하세요.

### <a name="overlay-tiles-from-a-tile-source"></a>타일 소스의 타일 오버레이

[**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141)를 사용하여 타일 소스의 바둑판식 이미지를 지도에 오버레이합니다.

1.  [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141)에서 상속되는 데이터 원본 클래스 세 개 중 하나를 인스턴스화합니다.

    -   [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)
    -   [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)
    -   [**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983)

    기본 URI 또는 파일 이름에 대체 가능한 매개 변수를 삽입하여 타일을 요청하는 데 사용할 **UriFormatString**을 구성합니다.

    다음 예에서는 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)를 인스턴스화합니다. 이 예제에서는 **HttpMapTileDataSource**의 생성자에서 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 값을 지정합니다.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)를 인스턴스화하고 구성합니다. 이전 단계에서 구성한 [**MapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn637141)를 **MapTileSource**의 [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149)로 지정합니다.

    다음 예에서는 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)의 생성자에서 [**DataSource**](https://msdn.microsoft.com/library/windows/apps/dn637149)를 지정합니다.

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)의 속성을 사용하여 타일이 표시되는 조건을 제한할 수 있습니다.

    -   [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147) 속성 값을 제공하여 특정 지역 내에만 타일을 표시합니다.
    -   [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171) 속성 값을 제공하여 특정 세부 정보 수준으로만 타일을 표시합니다.

    선택적으로, 타일 표시 또는 로드에 영향을 주는 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)의 다른 속성(예제: [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157), [**AllowOverstretch**](https://msdn.microsoft.com/library/windows/apps/dn637145), [**IsRetryEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637153), [**IsTransparencyEnabled**](https://msdn.microsoft.com/library/windows/apps/dn637155))을 구성합니다.

3.  [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)를 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 [**TileSources**](https://msdn.microsoft.com/library/windows/apps/dn637053) 컬렉션에 추가합니다.

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>웹 서비스의 타일 오버레이


[**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)를 사용하여 웹 서비스에서 검색한 바둑판식 이미지를 오버레이합니다.

1.  [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)를 인스턴스화합니다.
2.  웹 서비스에 필요한 URI의 형식을 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 속성 값으로 지정합니다. 이 값을 만들려면 기본 URI에 대체 가능한 매개 변수를 삽입합니다. 예를 들어 다음 코드 샘플에서 **UriFormatString** 값은 다음과 같습니다.

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    웹 서비스에서 대체 가능한 매개 변수 {x}, {y} 및 {zoomlevel}을 포함하는 URI를 지원해야 합니다. 대부분의 웹 서비스(예제: Nokia, Bing 및 Google)에서는 이 형식의 URI를 지원합니다. 웹 서비스에 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 속성과 함께 사용할 수 없는 추가 인수가 필요한 경우 사용자 지정 URI를 만들어야 합니다. [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn636993) 이벤트를 처리하여 사용자 지정 URI를 만들고 반환합니다. 자세한 내용은 이 항목의 뒷부분에 있는 [사용자 지정 URI 제공](#customuri) 섹션을 참조하세요.

3.  그런 다음 이전에 [바둑판식 이미지 개요](#tileintro)에 설명된 나머지 단계를 수행합니다.

다음 예에서는 북미 지역 지도에 가상 웹 서비스의 타일을 오버레이합니다. [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992)의 값은 [**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)의 생성자에서 지정합니다. 이 예에서 타일은 선택적 [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn637147) 속성에 지정된 지리적 경계 내에만 표시됩니다.

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

## <a name="overlay-tiles-from-local-storage"></a>로컬 저장소의 타일 오버레이


[**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)를 사용하여 로컬 저장소에 파일로 저장된 바둑판식 이미지를 오버레이합니다. 일반적으로 앱에서 이러한 파일을 패키징하고 배포합니다.

1.  [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)를 인스턴스화합니다.
2.  파일 이름의 형식을 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 속성 값으로 지정합니다. 이 값을 만들려면 기본 파일 이름에 대체 가능한 매개 변수를 삽입합니다. 예를 들어 다음 코드 샘플에서 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 값은 다음과 같습니다.

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    파일 이름 형식에 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 속성과 함께 사용할 수 없는 추가 인수가 필요한 경우 사용자 지정 URI를 만들어야 합니다. [**UriRequested**](https://msdn.microsoft.com/library/windows/apps/dn637001) 이벤트를 처리하여 사용자 지정 URI를 만들고 반환합니다. 자세한 내용은 이 항목의 뒷부분에 있는 [사용자 지정 URI 제공](#customuri) 섹션을 참조하세요.

3.  그런 다음 이전에 [바둑판식 이미지 개요](#tileintro)에 설명된 나머지 단계를 수행합니다.

다음 프로토콜 및 위치를 사용하여 로컬 저장소의 타일을 로드할 수 있습니다.

| URI | 추가 정보 |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | 앱 설치 폴더의 루트를 가리킵니다. |
|  | [Package.InstalledLocation](https://msdn.microsoft.com/library/windows/apps/br224681) 속성에서 참조되는 위치입니다. |
| ms-appdata:///local | 앱 로컬 저장소의 루트를 가리킵니다. |
|  | [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621) 속성에서 참조되는 위치입니다. |
| ms-appdata:///temp | 앱의 temp 폴더를 가리킵니다. |
|  | [ApplicationData.TemporaryFolder](https://msdn.microsoft.com/library/windows/apps/br241629) 속성에서 참조되는 위치입니다. |

 

다음 예제에서는 `ms-appx:///` 프로토콜을 사용하여 앱의 설치 폴더에서 파일로 저장된 타일을 로드합니다. [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 값은 [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)의 생성자에서 지정합니다. 이 예에서는 지도의 확대/축소 수준이 선택적 [**ZoomLevelRange**](https://msdn.microsoft.com/library/windows/apps/dn637171) 속성에 지정된 범위 내에 있는 경우에만 타일이 표시됩니다.

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

[**HttpMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636986)의 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636992) 속성 또는 [**LocalMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636994)의 [**UriFormatString**](https://msdn.microsoft.com/library/windows/apps/dn636998) 속성과 함께 사용할 수 있는 대체 가능한 매개 변수가 타일을 검색하는 데 충분하지 않은 경우 사용자 지정 URI를 만들어야 합니다. **UriRequested** 이벤트에 대한 사용자 지정 처리기를 제공하여 사용자 지정 URI를 만들고 반환합니다. 각 개별 타일에 대해 **UriRequested** 이벤트가 발생합니다.

1.  **UriRequested** 이벤트에 대한 사용자 지정 처리기에서 필수 사용자 지정 인수를 [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177)의 [**X**](https://msdn.microsoft.com/library/windows/apps/dn610743), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn610744) 및 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn610745) 속성과 결합하여 사용자 지정 URI를 만듭니다.
2.  [**MapTileUriRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637177)의 [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637179) 속성에 포함된 [**MapTileUriRequest**](https://msdn.microsoft.com/library/windows/apps/dn637173)의 [**Uri**](https://msdn.microsoft.com/library/windows/apps/dn610748) 속성에 사용자 지정 URI를 반환합니다.

다음 예에서는 **UriRequested** 이벤트에 대한 사용자 지정 처리기를 만들어 사용자 지정 URI를 제공하는 방법을 보여 줍니다. 또한 사용자 지정 URI를 만들기 위해 비동기적으로 작업을 수행해야 하는 경우에 지연 패턴을 구현하는 방법을 보여 줍니다.

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

## <a name="overlay-tiles-from-a-custom-source"></a>사용자 지정 소스의 타일 오버레이

[**CustomMapTileDataSource**](https://msdn.microsoft.com/library/windows/apps/dn636983)를 사용하여 사용자 지정 타일을 오버레이합니다. 타일을 프로그래밍 방식으로 메모리에서 즉시 만들거나, 코드를 작성하여 다른 소스에서 기존 타일을 로드합니다.

사용자 지정 타일을 만들거나 로드하려면 [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984) 이벤트에 대한 사용자 지정 처리기를 제공합니다. 각 개별 타일에 대해 **BitmapRequested** 이벤트가 발생합니다.

1.  [**BitmapRequested**](https://msdn.microsoft.com/library/windows/apps/dn636984) 이벤트에 대한 사용자 지정 처리기에서 필수 사용자 지정 인수를 [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132)의 [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) 및 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) 속성과 결합하여 사용자 지정 타일을 만들거나 검색합니다.
2.  [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132)의 [**Request**](https://msdn.microsoft.com/library/windows/apps/dn637134) 속성에 포함된 [**MapTileBitmapRequest**](https://msdn.microsoft.com/library/windows/apps/dn637128)의 [**PixelData**](https://msdn.microsoft.com/library/windows/apps/dn637140) 속성에 사용자 지정 타일을 반환합니다. **PixelData** 속성은 [**IRandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701664) 형식입니다.

다음 예에서는 **BitmapRequested** 이벤트에 대한 사용자 지정 처리기를 만들어 사용자 지정 타일을 제공하는 방법을 보여 줍니다. 이 예에서는 부분적으로 불투명하고 동일한 빨간색 타일을 만듭니다. 이 예에서는 [**MapTileBitmapRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637132)의 [**X**](https://msdn.microsoft.com/library/windows/apps/dn637135), [**Y**](https://msdn.microsoft.com/library/windows/apps/dn637136) 및 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637137) 속성을 무시합니다. 실제 사례는 아니지만 이 예에서는 메모리 내 사용자 지정 타일을 즉시 만드는 방법을 보여 줍니다. 또한 사용자 지정 타일을 만들기 위해 비동기적으로 작업을 수행해야 하는 경우에 지연 패턴을 구현하는 방법을 보여 줍니다.

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

## <a name="replace-the-default-map"></a>기본 지도 바꾸기

기본 지도를 타사 또는 사용자 지정 타일로 완전히 바꾸려면

-   [**MapTileLayer**](https://msdn.microsoft.com/library/windows/apps/dn637143).**BackgroundReplacement**를 [**MapTileSource**](https://msdn.microsoft.com/library/windows/apps/dn637144)의 [**Layer**](https://msdn.microsoft.com/library/windows/apps/dn637157) 속성 값으로 지정합니다.
-   [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127).**None**를 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)의 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 속성 값으로 지정합니다.

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)
