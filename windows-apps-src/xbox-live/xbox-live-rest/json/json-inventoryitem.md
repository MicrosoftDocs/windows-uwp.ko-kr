---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " inventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628498"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
Core 재고 항목을 자격 부여할 수 있는 표준 항목을 나타냅니다.
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

InventoryItem 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| url| 문자열| 이 특정 재고 항목에 대 한 고유 식별자입니다.|
| itemType| 문자열| 항목의 형식입니다. 현재 값은 <ul><li><b>알 수 없음</b></li><li><b>게임</b></li><li><b>동영상</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>Poster</b></li><li><b>Podcast</b></li><li><b>이미지</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>테마</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>앨범</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>Bundle</b></li><li><b>XnaCommunityGame</b></li><li><b>프로 모션</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>앱</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>구독</b><br/><br/> **참고:** 게임에서 지정 된 **GameV2**, 소모품 됩니다 **GameConsumable**, 영 속 DLC 이며 **GameContent**합니다. |
  | 컨테이너 | 문자열 | 이 항목을 포함 하는 "컨테이너"의 집합입니다. 특정 컨테이너에 속하는 항목에 대 한 사용자의 인벤토리를 쿼리할 수 있습니다. 이러한 컨테이너는 항목을 구매 하 여 인벤토리를 추가할 때 결정 됩니다. |
  | 가져온 | DateTime | 날짜 및 시간 항목이 사용자의 인벤토리에 추가 되었습니다. |
  | startDate | DateTime | 날짜 및 시간 항목이 않거나 용도로 사용할 수 있게 됩니다. |
  | endDate | DateTime | 날짜 및 시간 항목이 않거나 수 없게 됩니다. |
  | 상태 | 문자열 | 항목의 상태입니다. 허용 되는 값은 **사용**, **일시 중단**, **만료 됨**, **Canceled**를 **Renewed**합니다.  |
  | 평가판 | 부울 값 | 필수. 이 자격은 평가판; 이면 true 그렇지 않으면 false입니다. 평가판 자격을 구입 하 고 다음 전체 버전을 구입 하는 경우 받게 둘 다. |
  | trialTimeRemaining | TimeSpan | Null을 허용 합니다. 남은 시간 (분) 평가판을에 남아 있는 합니다. |
  | consumable | 배열 | 항목에 사용할 수 있는 경우 사용할 수 있도록 인벤토리 항목 뿐만 아니라 해당 현재 수량에 대 한 고유 식별자 (링크)에 대 한 인라인 표현이 포함 되어 있습니다. |

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


## <a name="consumable-inventory-item"></a>사용할 수 있는 재고 항목

사용할 수 있는 엔터티를 사용할 수 있는 항목에 대 한 속성의 최소 집합을 제공합니다.

| 멤버| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| 문자열| 사용할 수 있는 특정 재고 항목에 대 한 고유 식별자입니다.|
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


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>참고자료

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)
