---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2696e03389e21210216f038b7d5871d24729c6b7
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8779004"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

지정 된 일치 티켓을 만듭니다.

> [!IMPORTANT]
> 이 메서드를 계약 103 이상을 사용 하 여 사용 하기 위한 있으며 X Xbl-계약 버전의 헤더 요소: 103 또는 나중에 모든 요청.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [HTTP 상태 코드](#ID4E3C)
  * [요청 본문](#ID4EFD)
  * [응답 본문](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드에 서비스 구성 ID (서비스 안내) 수준에서 특정 이름의 hopper에 대 한 일치 티켓을 만듭니다. 이 메서드는 **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync** 메서드에서 줄 바꿈할 수 있습니다.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| 서비스 안내| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| hoppername | string | hopper의 이름입니다. |

<a id="ID4EJB"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 누락 된 경우 응답|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 남용 및 장치 유형| 예| 사용자의 deviceType 콘솔에 설정 된 경우 해당 클레임의 멀티 플레이 권한 있는 사용자만는 매치 메이 킹 서비스 호출을 할 수 있습니다. | 403|
| 장치 유형| 예| 이 사용자의 deviceType 또는 때 비-콘솔에 일치 하는 제목으로 설정 하 고 콘솔 전용 제목 수 없습니다. | 403|
| 제목 ID/증명 구매/장치 유형| 예| 제목에 일치 하 되는 지정 된 제목 클레임, 장치 유형 조합에 대 한 연결을 허용 해야 합니다. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4ELD"></a>


### <a name="required-members"></a>필수 멤버

| 멤버| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| 세션에 대 한 서비스 안내 합니다.|
| hopperName| string| hopper의 이름입니다.|
| giveUpDuration| 32 비트 부호 있는 정수| 최대 대기 시간 (초 정수 수)입니다.|
| preserveSession| 열거형| 일치 하는 세션 세션에 다시 사용 하는 경우를 나타내는 값입니다. 가능한 값은 "항상" 및 "없음"입니다. |
| ticketSessionRef| MultiplayerSessionReference| 플레이어 또는 그룹은 현재 재생 세션에 대 한 MultiplayerSessionReference 개체입니다. |
| ticketAttributes| 개체의 컬렉션| 특성 및 플레이어의 그룹에 대 한 사용자가 제공 된 값입니다.|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>금지 된 멤버

다른 모든 멤버는 요청에 사용할 수 없습니다.

<a id="ID4ECG"></a>


### <a name="sample-request"></a>샘플 요청

일치 티켓을 만들 수 있습니다 하 고 세션의 플레이어 관련 특성과 함께 일치 시킬 플레이어 있어야 합니다. **ticketSessionRef** 개체에서 참조 하 여 세션을 만들어야 합니다. 각 플레이어 만들거나 세션에 관련 된 일치 특성을 추가 하 고 MPSD에 대 한 세션에 참가 해야 합니다. 일치 특성은 각 플레이어에 matchAttrs 라는 사용자 지정 속성 필드에 저장 됩니다.

만들기 또는 연결 요청을 제출 **http://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** 와 같이 표시 될 수 있습니다.


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


세션을 만든 후 제목 티켓을 만드는 해당 세션에 대 한 연결 서비스를 호출할 수 있습니다.


> [!NOTE] 
> 타이틀이이 호출을 다시 시도 하는 사용자가 수행할 수 있지만 해야 다시 시도 하지이 자동으로 데이터에 실패 합니다.  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>응답 본문

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| 만들어지는 티켓에 대 한 ID입니다.|
| waitTime| 32 비트 부호 있는 정수| 평균 대기 hopper (정수 초 수)에 대 한 시간입니다.|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>부모  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
