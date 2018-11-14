---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
author: KevinAsgari
description: " GET (/users/me)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 825b101ef5b450910f0bd9b2ab84991daa8074a7
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6197271"
---
# <a name="get-usersme"></a>GET (/users/me)
사용자의 XUID 알아야 할 필요 없이 현재 사용자의 [PresenceRecord](../../json/json-presencerecord.md) 를 가져옵니다.
이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.

  * [쿼리 문자열 매개 변수](#ID4EZ)
  * [권한 부여](#ID4EIC)
  * [필요한 요청 헤더](#ID4ELD)
  * [선택적 요청 헤더](#ID4EPF)
  * [요청 본문](#ID4EPG)
  * [응답 본문](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| level| string| 선택 사항입니다. <ul><li><b>사용자</b>: 사용자 노드만 반환 합니다.</li><li><b>장치</b>: 사용자 노드 및 장치 노드를 반환 합니다.</li><li><b>제목</b>: 기본값입니다. 활동을 제외 하 고 전체 트리를 반환합니다.</li><li><b>모든</b>: 활동 수준 상태를 포함 하 여 전체 트리를 반환 합니다.</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답 없는 경우|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 사용할 수 없음|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > "입니다.|
| xbl 계약 버전 x| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext 합니다.|
| 수락| string| 허용 되는 콘텐츠-형식입니다. 유일 하 게 현재 상태에서 지 원하는 응용 프로그램/j 이지만 헤더에 지정 해야 합니다.|
| Accept Language| string| 응답에는 문자열에 대 한 허용 로캘입니다. 예제 값: EN-US입니다.|
| 호스트| 문자열| 도메인 이름 서버입니다. 예제 값: presencebeta.xboxlive.com 합니다.|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4E1G"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4EAH"></a>


### <a name="sample-response"></a>예제 응답

이 메서드는 [PresenceRecord](../../json/json-presencerecord.md)를 반환합니다.


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


<a id="ID4EQH"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ESH"></a>


##### <a name="parent"></a>부모

[/users/me](uri-usersme.md)
