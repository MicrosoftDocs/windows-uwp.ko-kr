---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
author: KevinAsgari
description: " POST (/users/batch)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9187aa43d3d4ee3a76ec834ac0352b66fe59167f
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7561557"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
여러 사용자의 현재 상태를 가져옵니다.
이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`.

  * [설명](#ID4EV)
  * [권한 부여](#ID4EAB)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EDC)
  * [필요한 요청 헤더](#ID4EYF)
  * [선택적 요청 헤더](#ID4EGAAC)
  * [요청 본문](#ID4EGBAC)
  * [응답 본문](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

모든 클라이언트, 서비스 또는 사용자의 일괄 처리에 대 한 상태 정보를 보려면 하고자 하는 제목에서이 메서드를 사용 해야 합니다.

이 일괄 처리 요청에 대 한 응답에는 깊이 및 경로에서 필터 될 수 있습니다. 소비자와 사용자의 집합에 대 한 존재 여부를 표시 합니다.이 사용할 수 있습니다. 이 API의 필터 작동 Or로은 속성이 아니라 And에서 속성 합니다.

<a id="ID4EAB"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답 없는 경우|
| --- | --- | --- | --- |
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 사용할 수 없음|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

이 메서드는 항상 200 OK를 반환 하지만 응답 본문에 콘텐츠를 반환할 수 있습니다.

| 사용자를 요청합니다.| 대상 사용자의 개인 정보 설정| 동작|
| --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: "XBL3.0 x =&lt;userhash >; &lt;토큰 > "입니다.|
| xbl 계약 버전 x| string| 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext 합니다.|
| 수락| string| 허용 되는 콘텐츠-형식입니다. 유일 하 게 현재 상태에서 지 원하는 응용 프로그램/j 이지만 헤더에 지정 해야 합니다.|
| Accept Language| string| 응답에는 문자열에 대 한 허용 로캘입니다. 예제 값: EN-US입니다.|
| 호스트| 문자열| 도메인 이름 서버입니다. 예제 값: presencebeta.xboxlive.com 합니다.|
| Content-Length| string| 요청 본문의 길이입니다. 예제 값: 312 합니다.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 이 요청 전달 되어야 하는 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 인증 토큰의 클레임 헤더의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1입니다.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>필수 멤버

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 사용자| 사용자의 현재 상태 1100 XUIDs 한 번에 최대 자세한 XUIDs 목록입니다.|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>선택적 멤버

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| 목록에 알아야 할 사용자에서 사용 되는 장치 유형입니다. 모든 가능한 장치 유형에 기본값 배열 비어 있는 경우 (즉, none 필터링 됩니다).|
| 제목| 디바이스의 목록 형식을 사용자에 대해 알아야 합니다. 모든 가능한 타이틀 기본값으로 배열 비어 있는 경우 (즉, none 필터링 됩니다).|
| level| 가능한 값: <ul><li>사용자-get 사용자 노드</li><li>장치-get 사용자 및 장치 노드</li><li>제목-기본 제목 수준 정보 가져오기</li><li>다양 한 상태 정보, 미디어 정보 또는 둘 다 모두-가져오기</li></ul><br> 기본값은 "제목".|
| onlineOnly| 이 속성이 true 이면 일괄 작업을 오프 라인 사용자 (숨겨진된 항목 포함)에 대 한 레코드 필터링 합니다. 제공 되지 않은 경우 온라인 및 오프 라인 사용자가 반환 됩니다.|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>금지 된 멤버

다른 모든 구성원 요청에 사용할 수 없습니다.

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>샘플 요청


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>응답 본문

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>부모

[/users/batch](uri-usersbatch.md)
