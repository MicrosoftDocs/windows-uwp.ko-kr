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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622588"
---
# <a name="presencerecord-json"></a>PresenceRecord(JSON)
단일 사용자의 온라인 상태에 대 한 데이터입니다.
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

PresenceRecord 개체에 다음과 같이 지정 합니다.

| 멤버| 형식| 설명|
| --- | --- | --- |
| xuid| 문자열| Xbox 사용자 ID (XUID) 대상 사용자입니다. 제공 된 현재 상태 데이터를이 사용자입니다.|
| 디바이스| 배열을 [DeviceRecord](json-devicerecord.md)| 사용자의 장치 레코드의 목록입니다.|
| 상태| 문자열| Xbox LIVE에서 사용자의 작업입니다. 가능한 값: <ul><li>온라인: 사용자가 하나 이상의 장치 레코드입니다.</li><li>제거: 사용자는 Xbox LIVE에 로그인 하지만 모든 제목에 활성 상태가 아닙니다.</li><li>오프 라인으로: 사용자는 장치에 나타나지 않습니다.</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| 사용자가 유효한 DeviceRecords 없는 경우에 마지막 표시 된 정보를 제공 됩니다. 개체 캐시에서 제거 된 경우 데이터 하지 반환 될 수 있습니다, 영구 저장소가 없는 있기 때문입니다.|

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


##### <a name="parent"></a>Parent

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>참고자료

[POST (/ 사용자/일괄 처리)](../uri/presence/uri-usersbatchpost.md)

 [가져오기 (/ 사용자/me)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)
