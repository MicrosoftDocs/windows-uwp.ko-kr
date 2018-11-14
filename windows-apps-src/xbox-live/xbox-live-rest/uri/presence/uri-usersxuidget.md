---
title: GET (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid}))"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3d15bd82197a22ddcf6abf836e726774fd1c7943
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6150253"
---
# <a name="get-usersxuidxuid"></a>GET (/users/xuid({xuid}))
다른 사용자 또는 클라이언트의 존재 여부를 검색 합니다.
이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EDB)
  * [쿼리 문자열 매개 변수](#ID4EOB)
  * [권한 부여](#ID4E4C)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EAE)
  * [필요한 요청 헤더](#ID4EVH)
  * [선택적 요청 헤더](#ID4E1BAC)
  * [요청 본문](#ID4E1CAC)
  * [응답 본문](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

소비자 전체 개체에 관심이 없는 경우 [PresenceRecord](../../json/json-presencerecord.md) 의 일부를 제공 하도록 응답을 필터링 할 수 있습니다.

> [!NOTE] 
> 반환 된 데이터 개인 정보 및 콘텐츠 격리 규칙에 의해 제한 됩니다.



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| xuid| 64 비트 부호 없는 정수| Xbox 사용자 ID (XUID)는 대상 사용자의 합니다.|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- |
| level| string| 선택 사항입니다. <ul><li><b>사용자</b>: 사용자 노드만 반환 합니다.</li><li><b>장치</b>: 사용자 노드 및 장치 노드를 반환 합니다.</li><li><b>제목</b>: 기본값입니다. 활동을 제외 하 고 전체 트리를 반환합니다.</li><li><b>모든</b>: 활동 수준 상태를 포함 하 여 전체 트리를 반환 합니다.</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답 없는 경우|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 사용할 수 없음|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

이 메서드는 항상 200 OK를 반환 하지만 응답 본문에 콘텐츠를 반환할 수 있습니다.

| 사용자를 요청합니다.| 대상 사용자의 개인 정보 설정| 동작|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 내 정보 표시| -| 200 OK|
| 친구| 모든 사용자| 200 OK|
| 친구| 친구만| 200 OK|
| 친구| 차단| 200 OK|
| 비 친구 사용자| 모든 사용자| 200 OK|
| 비 친구 사용자| 친구만| 200 OK|
| 비 친구 사용자| 차단| 200 OK|
| 제 3 자 사이트| 모든 사용자| 200 OK|
| 제 3 자 사이트| 친구만| 200 OK|
| 제 3 자 사이트| 차단| 200 OK|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > "입니다.|
| xbl 계약 버전 x| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext 합니다.|
| 수락| string| 허용 되는 콘텐츠-형식입니다. 유일 하 게 현재 상태에서 지 원하는 응용 프로그램/j 이지만 헤더에 지정 해야 합니다.|
| Accept Language| string| 응답에는 문자열에 대 한 허용 로캘입니다. 예제 값: EN-US입니다.|
| 호스트| 문자열| 도메인 이름 서버입니다. 예제 값: presencebeta.xboxlive.com 합니다.|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>예제 응답

사용자에 대 한 기존 레코드가 없는 경우에 없는 장치를 사용 하 여 기록을 반환 됩니다.


```cpp
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
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>부모

[/users/xuid({xuid})](uri-usersxuid.md)
