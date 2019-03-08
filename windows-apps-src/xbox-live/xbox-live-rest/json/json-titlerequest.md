---
title: TitleRequest(JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest(JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593798"
---
# <a name="titlerequest-json"></a>TitleRequest(JSON)
제목에 대 한 정보에 대 한 요청입니다. 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
TitleRequest 개체에 다음과 같이 지정 합니다.
 
| 멤버| 형식| 설명| 
| --- | --- | --- | 
| id| 32 비트 부호 없는 정수| 제목의 식별자입니다.| 
| activity| [ActivityRequest](json-activityrequest.md)| 제목에 사용 가능한 경우 다양 한 미디어 및 현재 상태 정보를 포함 한 정보입니다.| 
| 상태| 문자열| 사용자가 활성 인지 여부입니다. 이 필드는 사용자를 비활성으로 표시 해야 합니다. 기본값은 "활성".| 
| 배치| 문자열| 제목 배치 모드입니다. 가능한 값에 "전체", "fill", "끌어온" 또는 "백그라운드" 포함 됩니다. 기본값은 "전체".| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>JSON 구문 예제
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[JavaScript 개체 표기법 (JSON) 개체 참조](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>참고자료 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   