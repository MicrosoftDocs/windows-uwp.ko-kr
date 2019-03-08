---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659838"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

지정 된 일치 티켓을 만듭니다.

> [!IMPORTANT]
> 이 103 이상 계약과 함께 사용 하기 위한 메서드와 X Xbl-계약 버전 헤더 요소를 필요: 103 또는 나중에 모든 요청 합니다.

  * [설명](#ID4ET)
  * [URI 매개 변수](#ID4E5)
  * [권한 부여](#ID4EJB)
  * [HTTP 상태 코드](#ID4E3C)
  * [요청 본문](#ID4EFD)
  * [응답 본문](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>설명

이 HTTP/REST 메서드는 서비스 구성 서비스 ID (안내) 수준에서 특정 이름의 hopper에 대 한 일치 티켓을 만듭니다. 이 메서드를 래핑할 수는 **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync** 메서드.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| scid| GUID| 서비스 구성 (서비스 안내) 세션 식별자입니다.|
| hoppername | 문자열 | hopper의 이름입니다. |

<a id="ID4EJB"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답 없는 경우|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 및 장치 유형| 예| 사용자의 deviceType 콘솔에 설정 되 면 클레임에서 멀티 플레이 권한이 있는 사용자만 결혼 정보 회사 연결 서비스를 호출할 수 있습니다. | 403|
| 장치 유형| 예| 사용자의 deviceType 없는 또는 비-콘솔에에 일치 하는 제목 집합에는 콘솔 전용 제목을 사용 해야 합니다. 경우. | 403|
| 제목 ID/증빙 구매/장치 유형| 예| 제목에 일치 하는 지정 된 제목 클레임, 장치 형식 조합에 대 한 결혼 정보 회사 연결을 허용 해야 합니다. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드
서비스는 MPSD에 적용 되는 HTTP 상태 코드를 반환 합니다.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4ELD"></a>


### <a name="required-members"></a>필요한 멤버

| 멤버| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| 세션에 대 한 서비스를 안내 합니다.|
| hopperName| 문자열| hopper의 이름입니다.|
| giveUpDuration| 32 비트 부호 있는 정수| 최대 대기 시간 (정수 초 수)입니다.|
| preserveSession| 열거형| 세션에 일치 하는 세션으로 다시 사용 되는 경우를 나타내는 값입니다. 가능한 값은 "항상" 및 "never"입니다. |
| ticketSessionRef| MultiplayerSessionReference| 플레이어 또는 그룹을 현재 재생 중인 세션에 대 한 MultiplayerSessionReference 개체입니다. |
| ticketAttributes| 개체의 컬렉션| 특성 및 플레이어의 그룹에 대 한 사용자가 제공 하는 값입니다.|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>허용 되지 않는 멤버

다른 모든 멤버를 요청에 사용할 수 없습니다.

<a id="ID4ECG"></a>


### <a name="sample-request"></a>샘플 요청

세션에서 참조 하는 **ticketSessionRef** 일치 티켓을 만들 수 및 세션에는 해당 player 관련 특성 함께 일치 시킬 플레이어 있어야 합니다. 먼저 개체를 만들어야 합니다. 각 플레이어를 만들거나 세션에 관련 된 일치 특성을 추가 하 여 MPSD에 대 한 세션에 참가 해야 합니다. 일치 특성은 각 플레이어에 matchAttrs 라는 사용자 지정 속성 필드에 배치 됩니다.

만들기 또는 조인 요청을 제출 **https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** 하며이 같습니다.


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


세션이 만들어지면 제목 해당 세션에 대 한 티켓을 만들기 위해 결혼 정보 회사 연결 서비스를 호출할 수 있습니다.


> [!NOTE] 
> 제목을이 호출을 다시 시도 하려면 사용자가 수행할 수 있지만 해야이 자동으로 재시도 하지 데이터 실패 하는 경우.  



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
| ticketId| GUID| 생성 되는 티켓의 ID입니다.|
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


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
