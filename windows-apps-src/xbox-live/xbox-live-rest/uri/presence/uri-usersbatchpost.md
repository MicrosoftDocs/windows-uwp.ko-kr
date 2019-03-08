---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655678"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
사용자의 일괄 처리에 대 한 현재 상태를 가져옵니다.
이러한 Uri에 대 한 도메인은 `userpresence.xboxlive.com`합니다.

  * [설명](#ID4EV)
  * [권한 부여](#ID4EAB)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EDC)
  * [필요한 요청 헤더](#ID4EYF)
  * [선택적 요청 헤더](#ID4EGAAC)
  * [요청 본문](#ID4EGBAC)
  * [응답 본문](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

모든 클라이언트, 서비스 또는 사용자의 일괄 처리에 대 한 현재 상태 정보를 학습 하려는 제목으로이 메서드를 사용 해야 합니다.

이 일괄 처리 요청에 대 한 응답에는 깊이를 경로 필터 수 있습니다. 소비자 알아보고 사용자 집합에 대 한 현재 상태를 표시 하는 데 사용할 수 있습니다. 이 API 작업에 Or로의 필터는 속성이 아니라 ANDs 속성에서.

<a id="ID4EAB"></a>


## <a name="authorization"></a>권한 부여

| 형식| 필수| 설명| 응답 없는 경우|
| --- | --- | --- | --- |
| XUID| 예| 호출자의 Xbox 사용자 ID (XUID)| 403 Forbidden|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

이 메서드는 항상 200 OK를 반환 하지만 응답 본문에 콘텐츠를 반환할 수 있습니다.

| 요청 하는 사용자| 대상 사용자의 개인 정보 설정| 동작|
| --- | --- | --- | --- | --- | --- | --- |
| Me| -| 200 OK|
| friend| 모든 사용자| 200 OK|
| friend| 친구 들과| 200 OK|
| friend| 차단됨| 200 OK|
| friend가 아닌 사용자| 모든 사용자| 200 OK|
| friend가 아닌 사용자| 친구 들과| 200 OK|
| friend가 아닌 사용자| 차단됨| 200 OK|
| 타사 사이트| 모든 사용자| 200 OK|
| 타사 사이트| 친구 들과| 200 OK|
| 타사 사이트| 차단됨| 200 OK|

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예를 들어 값: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| x-xbl-contract-version| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 예제 값: 3, vnext입니다.|
| 수락| 문자열| 허용 되는 콘텐츠-유형입니다. 현재 상태에서 지 원하는 유일한 application/json이 되었지만 헤더에 지정 해야 합니다.|
| Accept-Language| 문자열| 응답에는 문자열에 대 한 허용 가능한 로캘입니다. 예제 값: EN-US입니다.|
| 호스트| 문자열| 서버의 도메인 이름입니다. 예제 값: presencebeta.xboxlive.com 합니다.|
| Content-Length| 문자열| 요청 본문의 길이입니다. 예를 들어 값: 312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청만으로 라우팅되는 헤더, 인증 토큰의 클레임의 유효성을 확인 한 후 서비스는 합니다. 기본값: 1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>요청 본문

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>필요한 멤버

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 사용자| 목록 XUIDs 사용자 최대 한 번에 1100 XUIDs 학습 하려면 현재 상태입니다.|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>선택적 멤버

| 멤버| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| 목록에 알아야 할 사용자를 사용 하는 장치 종류입니다. 배열은 비워 두면 기본적으로 모든 가능한 장치 유형 (즉, none은 필터링 되어 제외).|
| 제목| 장치 목록에 대 한 확인 하려는 사용자 형식입니다. 배열은 비워 두면 기본적으로 모든 가능한 제목 (즉, none은 필터링 되어 제외).|
| level| 가능한 값: <ul><li>사용자-get 사용자 노드</li><li>장치-사용자 및 장치 노드</li><li>제목-기본 제목 수준 정보 가져오기</li><li>다양 한 상태 정보, 미디어 정보 또는 둘 다 모든-가져오기</li></ul><br> 기본값은 "title".|
| onlineOnly| 이 속성이 true 인 경우 일괄 처리 작업 오프 라인 사용자 (감추어진된 항목 포함)에 대 한 레코드를 필터링 합니다. 제공 되지 않으면 온라인 및 오프 라인 사용자가 반환 됩니다.|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>허용 되지 않는 멤버

다른 모든 멤버를 요청에 사용할 수 없습니다.

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


### <a name="sample-response"></a>샘플 응답

이 메서드는 반환 된 [PresenceRecord](../../json/json-presencerecord.md)합니다.


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


##### <a name="parent"></a>Parent

[/users/batch](uri-usersbatch.md)
