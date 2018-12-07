---
title: PresenceRecord(JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
description: " PresenceRecord(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d531352c4336e00c93a91e7c945602ab69695f2
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8806919"
---
# <a name="presencerecord-json"></a>PresenceRecord(JSON)
단일 사용자의 온라인 상태에 대 한 데이터를 제공 합니다.
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

PresenceRecord 개체에 다음과 같이 지정 합니다.

| 멤버| 유형| 설명|
| --- | --- | --- |
| xuid| string| Xbox 사용자 ID (XUID)는 대상 사용자의 합니다. 이 사용자에 대 한 현재 상태 데이터를 제공 됩니다.|
| 장치| [DeviceRecord](json-devicerecord.md) 의 배열| 사용자의 장치 레코드의 목록입니다.|
| 상태| string| Xbox LIVE에서 사용자의 작업 합니다. 가능한 값: <ul><li>온라인: 사용자가 하나 이상의 장치 레코드.</li><li>바로: 사용자가 Xbox LIVE에 서명 된 있지만 모든 제목에 활성화 되지 않습니다.</li><li>오프 라인: 사용자가 모든 장치에 존재 합니다.</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| 마지막 확인 한 정보는 사용자가 없는 유효한 DeviceRecords 때에 사용할 수 있습니다. 개체가 캐시에서 제거 된 데이터 하지 반환 될 수 있습니다, 없는 영구 저장소 때문입니다.|

<a id="ID4E2C"></a>


## <a name="sample-json-syntax"></a>JSON 구문 예제


```json
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:"W8",
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page"
      }
    }]
  }]
}

```


<a id="ID4EED"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EGD"></a>


##### <a name="parent"></a>부모

[JSON(JavaScript Object Notation) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>참조

[POST (/users/batch)](../uri/presence/uri-usersbatchpost.md)

 [GET (/users/me)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)
