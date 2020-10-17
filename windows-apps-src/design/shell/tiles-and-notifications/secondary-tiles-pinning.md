---
description: 보조 타일을 만들고 유니버설 Windows 플랫폼 (UWP) 앱에서 프로그래밍 방식으로 시작 메뉴에 고정 하는 방법을 알아봅니다.
title: 시작에 보조 타일 고정
label: Pin secondary tiles to Start
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 보조 타일, pin, 고정, 빠른 시작, 코드 샘플, 예제, secondarytile
ms.localizationpriority: medium
ms.openlocfilehash: 2e84885f6c75b37614c054fe1919cf71e09d5d07
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100351"
---
# <a name="pin-secondary-tiles-to-start"></a>시작에 보조 타일 고정


이 항목에서는 Windows 앱에 대 한 보조 타일을 만들고 시작 메뉴에 고정 하는 단계를 안내 합니다.

![보조 타일의 스크린샷](images/secondarytiles.png)

보조 타일에 대 한 자세한 내용은 [보조 타일 개요](secondary-tiles.md)를 참조 하세요.


## <a name="add-namespace"></a>네임 스페이스 추가

Windows. u i > 네임 스페이스는 SecondaryTile 클래스를 포함 합니다.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>보조 타일 초기화

보조 타일은 몇 가지 주요 구성 요소로 구성 됩니다.

* **TileId**: 다른 보조 타일 사이에서 타일을 식별할 수 있는 고유 식별자입니다.
* **DisplayName**: 타일에 표시 하려는 이름입니다.
* **인수**: 사용자가 타일을 클릭할 때 앱에 다시 전달할 인수입니다.
* **Square150x150Logo**: 미디어 크기 타일에 표시 되는 필수 로고 이며 작은 로고를 제공 하지 않는 경우 작은 크기의 타일로 크기가 조정 됩니다.

위의 모든 속성에 대해 초기화 된 값을 제공 **해야** 합니다. 그렇지 않으면 예외가 발생 합니다.

사용할 수 있는 다양 한 생성자가 있지만 tileId, displayName, arguments, square150x150Logo 및 System.windows.uielement.desiredsize를 사용 하는 생성자를 사용 하면 필요한 모든 속성을 설정할 수 있습니다.

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


## <a name="optional-add-support-for-larger-tile-sizes"></a>선택 사항: 더 큰 타일 크기에 대 한 지원 추가

보조 타일에 다양 한 타일 알림이 표시 되는 경우 사용자가 콘텐츠를 더 많이 볼 수 있도록 타일의 크기를 너비 또는 크게 조정 하는 것이 좋습니다.

광범위 하 고 긴 타일 크기를 사용 하도록 설정 하려면 *Wide310x150Logo* 및 *Square310x310Logo*를 제공 해야 합니다. 또한 가능한 경우 작은 타일 크기에 대 한 *Square71x71Logo* 을 제공 해야 합니다. 그렇지 않으면 작은 타일에 필요한 Square150x150Logo 크기를 조정 하 게 됩니다.

알림이 있는 경우 오른쪽 아래 모서리에 선택적으로 표시 되는 고유한 *Square44x44Logo*제공할 수도 있습니다. 이러한 항목을 제공 하지 않으면 기본 타일의 *Square44x44Logo* 대신 사용 됩니다.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>선택 사항: 표시 이름을 표시 하도록 설정 합니다.

기본적으로 표시 이름은 표시 되지 않습니다. 표시 이름을 중간/와이드/큼에 표시 하려면 다음 코드를 추가 합니다.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>선택 사항: 3D 보조 타일
3D 자산을 추가 하 여 Windows Mixed Reality의 보조 타일을 향상 시킬 수 있습니다. 사용자는 혼합 현실 환경에서 앱을 사용 하는 경우 시작 메뉴 대신 Windows Mixed Reality 홈에 직접 3D 타일을 추가할 수 있습니다. 예를 들어 360 ° photo viewer 앱에 직접 연결 되는 360 ° photo 구를 만들거나, 가구 카탈로그에서 해당 개체에 대 한 가격 책정 및 색 옵션에 대 한 세부 정보 페이지를 여는 경우 사용자가 해당 개체의 3D 모델을 선택할 수 있습니다. 시작 하려면 [Mixed Reality 개발자 설명서](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home)를 참조 하세요.



## <a name="pin-the-secondary-tile"></a>보조 타일 고정

마지막으로 타일 고정을 요청 합니다. 이는 UI 스레드에서 호출 해야 합니다. 바탕 화면에서 사용자에 게 타일을 고정할 지 여부를 확인 하는 대화 상자가 표시 됩니다.

> [!IMPORTANT]
> 데스크톱 브리지를 사용 하는 데스크톱 응용 프로그램의 경우 [데스크톱 앱에서 고정](secondary-tiles-desktop-pinning.md) 에 설명 된 대로 추가 단계를 먼저 수행 해야 합니다.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>보조 타일이 있는지 확인

사용자가 앱에서 시작 하기 위해 이미 고정 된 페이지를 방문 하는 경우 "고정 해제" 단추를 대신 표시 하는 것이 좋습니다.

따라서 표시할 단추를 선택할 때 먼저 보조 타일이 현재 고정 되어 있는지 확인 해야 합니다.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>보조 타일 고정 해제

타일이 현재 고정 되어 있고 사용자가 고정 해제 단추를 클릭 하면 타일을 고정 해제 (삭제) 할 수 있습니다.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>보조 타일 업데이트

보조 타일에서 로고, 표시 이름 또는 다른 항목을 업데이트 해야 하는 경우 *RequestUpdateAsync*를 사용할 수 있습니다.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>고정 된 모든 보조 타일 열거

*Secondarytile*를 사용 하는 대신 사용자가 고정 한 모든 타일을 검색 해야 하는 경우에는 *FindAllAsync ()* 를 사용할 수 있습니다.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>타일 알림 보내기

타일 알림을 통해 타일에 풍부한 콘텐츠를 표시 하는 방법을 알아보려면 [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)를 참조 하세요.


## <a name="related"></a>관련 항목

* [보조 타일 개요](secondary-tiles.md)
* [보조 타일 지침](secondary-tiles-guidance.md)
* [타일 자산](../../style/app-icons-and-logos.md)
* [타일 콘텐츠 설명서](create-adaptive-tiles.md)
* [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
