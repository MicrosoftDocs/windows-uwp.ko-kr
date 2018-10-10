---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
author: KevinAsgari
description: " inventoryItem (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0a7521de7da4ebd31f0a1d8c59bb7c0134eddc08
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4500466"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
핵심 인벤토리 항목을 권리를 부여 수는 표준 항목을 나타냅니다.
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

InventoryItem 개체에는 다음 사양을 있습니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| url| string| 이 특정 인벤토리 항목에 대 한 고유 식별자입니다.|
| itemType| string| 항목의 유형입니다. 현재 값은 <ul><li><b>알 수 없음</b></li><li><b>Game</b></li><li><b>영화</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>포스터</b></li><li><b>팟캐스트</b></li><li><b>이미지</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>Theme</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>앨범</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>번들</b></li><li><b>XnaCommunityGame</b></li><li><b>프로 모션</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>앱</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>구독</b><br/><br/> **참고:** 게임 **GameV2**하 여 지정 된, 소모 성은 **GameConsumable**및 지속형 DLC **GameContent**됩니다. |
  | 컨테이너 | string | 이 항목을 포함 하는 "컨테이너"의 집합입니다. 특정 컨테이너에 속한 항목에 대 한 사용자의 인벤토리를 쿼리할 수 있습니다. 이러한 컨테이너는 항목 구매 하 여 인벤토리에 추가 될 때 결정 됩니다. |
  | 획득 | DateTime | 날짜 및 시간 사용자의 인벤토리에 항목이 추가 되었습니다. |
  | startDate | DateTime | 날짜 및 시간 항목 되는 사용 하기 위해 사용할 수 있습니다. |
  | endDate | DateTime | 날짜 및 시간 항목 되는 또는 사용할 수 없게 됩니다. |
  | 상태 | string | 항목의 상태입니다. 허용 되는 값은 **활성화**, **일시 중단**, **만료**, **취소**, **갱신**합니다.  |
  | 평가판 | 부울 값 | 필수. True 이면이 권리는 평가판; 그렇지 않으면 false입니다. 권리의 평가판 버전을 구입 하 고 다음 정식 버전을 구입 하는 경우 둘 다를 받게 됩니다. |
  | trialTimeRemaining | TimeSpan | Null을 허용 합니다. 분에서 평가판에 남아 있는 시간. |
  | 소모 성 | array | 소모 성 항목의 경우 소모 성 인벤토리 항목의 현재 수량에 대 한 고유 식별자 (링크)의 인라인 표현을 포함 되어 있습니다. |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>소모 성 인벤토리 항목

소모 성 엔터티 최소한의 소모 성 항목에 대 한 속성 집합을 제공합니다.

| 멤버| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| string| 특정 소모 성 인벤토리 항목에 대 한 고유 식별자입니다.|
| quantity| 32 비트 부호 있는 정수| 이 인벤토리 항목의 현재 수량입니다.|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>참조

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [인벤토리/소모품 / {itemID} /](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)
