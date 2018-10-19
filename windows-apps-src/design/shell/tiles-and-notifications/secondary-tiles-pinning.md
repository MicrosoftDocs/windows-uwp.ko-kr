---
author: andrewleader
Description: Learn how to pin a secondary tile from your UWP app.
title: 보조 타일 고정
label: Pin secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보조 타일, 고정, 고정하기, 빠른 시작, 코드 샘플, 예제, 보조타일
ms.localizationpriority: medium
ms.openlocfilehash: 437d149e22f035fdd0cb1f5251a114b6dd4765e4
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4966184"
---
# <a name="pin-secondary-tiles"></a>보조 타일 고정


이 항목에서는 UWP 앱의 보조 타일을 만들고 시작 메뉴에 고정하는 단계를 안내합니다.

![보조 타일의 스크린샷](images/secondarytiles.png)

보조 타일에 대한 자세한 내용은 [보조 타일 개요](secondary-tiles.md)를 참조하세요.


## <a name="add-namespace"></a>네임스페이스 추가

Windows.UI.StartScreen namespace includes the SecondaryTile 클래스.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>보조 타일 초기화

보조 타일은 몇 가지 핵심 구성 요소로 이루어져 있습니다.

* **TileId**: 여러 보조 타일 중에서 특정 타일을 식별할 수 있게 해 주는 고유 식별자입니다.
* **DisplayName**: 타일에 표시할 이름입니다.
* **Arguments**: 사용자가 타일을 클릭할 때 다시 앱으로 전달할 인수입니다.
* **Square150x150Logo**: 필수 로고이며, 중간 크기 타일에 표시됩니다(소형 로고가 제공되지 않을 경우 소형 타일에 맞게 크기가 조정됨).

위의 모든 속성에 대한 초기화 값을 **반드시** 제공해야 합니다. 그렇지 않으면 예외가 발생합니다.

사용 가능한 다양한 생성자가 있지만 tileId, displayName, arguments, square150x150Logo 및 desiredSize를 이용하는 생성자를 사용하면 필요한 모든 속성을 설정할 수 있습니다.

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>선택 사항: 큰 타일 크기에 대한 지원 추가

보조 타일에 풍부한 타일 알림을 표시할 생각이라면 사용자가 더 많은 콘텐츠를 볼 수 있도록 타일을 더 넓게 또는 더 크게 조정할 수 있도록 허용해야 합니다.

와이드 및 큰 타일 크기를 사용하려면 *Wide310x150Logo* 및 *Square310x310Logo*를 제공해야 합니다. 가능하다면 작은 타일 크기에 대한 *Square71x71Logo*도 제공해야 합니다. 그렇지 않으면 작은 타일에 필요한 Square150x150Logo의 크기가 축소됩니다.

또한 알림이 있으면 오른쪽 아래 모서리에 선택적으로 표시되는 고유의 *Square44x44Logo*를 제공할 수 있습니다. 이 로고를 제공하지 않으면 기본 타일의 *Square44x44Logo*가 대신 사용됩니다.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>선택 사항: 표시 이름을 표시하도록 설정

기본적으로 표시 이름이 표시되지 않습니다. 중간/와이드/큰 타일에 표시 이름을 표시하려면 다음 코드를 추가합니다.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>선택 사항: 3D 보조 타일
3D 자산을 추가하여 Windows Mixed Reality를 위한 보조 타일을 개선할 수 있습니다. 사용자는 혼합 현실 환경에서 앱을 사용할 때 시작 메뉴 대신 Windows Mixed Reality 홈에 직접 3D 타일을 배치할 수 있습니다. 예를 들어 360° 사진 뷰어 앱에 직접 연결되는 360° Photopheres를 만들거나 사용자가 선택한 경우 해당 객체의 가격 및 색상 옵션에 대한 세부 정보 페이지가 열리는 가구 카탈로그에서 의자의 3D 모델을 배치하도록 허용할 수 있습니다. 시작하려면, [혼합 현실 개발자 설명서](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home)를 참조하세요.



## <a name="pin-the-secondary-tile"></a>보조 타일 고정

마지막으로, 타일을 고정하라고 요청합니다. 이것은 UI 스레드에서 호출되어야 합니다. 타일을 고정할 것인지 사용자의 확인을 요청하는 대화 상자가 바탕 화면에 표시됩니다.

> [!IMPORTANT]
> 데스크톱 브리지를 사용하는 Windows 데스크톱 응용 프로그램인 경우 가장 먼저 [데스크톱 응용 프로그램에서 고정](secondary-tiles-desktop-pinning.md)에 설명된 대로 추가 단계를 수행해야 합니다.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>보조 타일이 있는지 확인

사용자가 이미 시작에 고정한 앱 페이지를 방문하는 경우 고정 대신 "고정 해제" 단추를 표시해야 합니다.

따라서 표시할 단추를 선택할 때에는 보조 타일이 현재 고정되어 있는지 여부를 먼저 확인해야 합니다.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>보조 타일 고정 해제

타일이 현재 고정되어 있고 사용자가 고정 해제 단추를 클릭하면 타일을 고정 해제(삭제)해야 합니다.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>보조 타일 업데이트

로고, 표시 이름 또는 보조 타일의 다른 항목을 업데이트해야 하는 경우 *RequestUpdateAsync*를 사용하면 됩니다.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>고정된 모든 보조 타일 열거

사용자가 고정한 모든 타일을 검색해야 하는 경우 *SecondaryTile.Exists*를 사용하는 대신 *SecondaryTile.FindAllAsync()* 를 사용할 수 있습니다.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>타일 알림 보내기

타일 알림을 통해 타일에 다양한 콘텐츠를 표시하는 방법을 알아보려면 [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)를 참조하세요.


## <a name="related"></a>관련 항목

* [보조 타일 개요](secondary-tiles.md)
* [보조 타일 지침](secondary-tiles-guidance.md)
* [타일 자산](app-assets.md)
* [타일 콘텐츠 설명서](create-adaptive-tiles.md)
* [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
