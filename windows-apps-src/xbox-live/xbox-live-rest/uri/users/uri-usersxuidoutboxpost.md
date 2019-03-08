---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
description: " POST (/users/xuid({xuid})/outbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b225e8441fee3d499f172e2e5701096cdbc161a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594288"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
받는 사람 목록에 지정 된 메시지를 보냅니다.
이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`합니다.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EAB)
  * [권한 부여](#ID4ENB)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EYB)
  * [요청 본문](#ID4E3F)
  * [HTTP 상태 코드](#ID4ETCAC)
  * [응답 본문](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

이 API는 지원만 content-type은 "application/json"이 각 호출의 HTTP 헤더에 필요 합니다.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| xuid | 부호 없는 64 비트 정수 | Xbox 사용자 ID (XUID)은 요청을 수행 하는 플레이어입니다. |

<a id="ID4ENB"></a>


## <a name="authorization"></a>권한 부여

사용자 메시지를 보낼 사용자 고유의 사용자 클레임 및 gold 유효한 구독이 있어야 합니다.

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

결과 코드 200 결과 성공적으로 메시지를 보내는 사용자 플레이어를 해당 플레이어 친구를 인지 합니다. 그러나 사용자를 차단한 사람에 게 메시지를 보내는 경우 받는 사람이 메시지를 받지 및 메시지 되지 않았지만 되었다는 메시지를 받지 못합니다.

또한 대 한 제한이 있습니다 얼마나 많은 메시지가 보내고 받을 수 하루에 얼마나 많은 친구와 친구 들을 다음과 같이 합니다.

   * 메시지당 20 면허증
   * 24 시간 당 200 면허증
   * 24 시간 당 총 250 개의 메시지
   * 24 시간 마다 전체 받는 사람 2500

| 요청 하는 사용자| 대상 사용자의 개인 정보 설정| 동작|
| --- | --- | --- | --- | --- | --- |
| Me| -| 설명한 합니다.|
| friend| 모든 사용자| 200 OK|
| friend| 친구 들과| 200 OK|
| friend| 차단됨| 200 OK|
| friend가 아닌 사용자| 모든 사용자| 200 OK|
| friend가 아닌 사용자| 친구 들과| 200 OK|
| friend가 아닌 사용자| 차단됨| 200 OK|
| 타사 사이트| 모든 사용자| 200 OK|
| 타사 사이트| 친구 들과| 200 OK|
| 타사 사이트| 차단됨| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>요청 본문

| 속성| 형식| 최대 길이| 소비자| 설명|
| --- | --- | --- | --- | --- |
| 머리글| 헤더|  | 모두| 사용자 메시지 헤더|
| messageText| 문자열| 250| Windows 8을 제외한 모든 플랫폼| 사용자 메시지 텍스트 (u t F-8)|

#### <a name="header"></a>헤더

| 속성| 형식| 최대 길이| 소비자| 설명|
| --- | --- | --- | --- | --- |
| 받는 사람| 사용자]| 20| 모두| 메시지 받는 사람 목록|

#### <a name="user"></a>사용자

| 속성| 형식| 최대 길이| 소비자| 설명|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | 모두| 받는 사람 XUID 합니다. 게이머 태그 전송 되 면 사용 되지 않습니다.|
| 게이머 태그| 문자열| 15| 모두| 받는 사람의 게이머 태그입니다. XUID 전송 되 면 사용 되지 않습니다.|

#### <a name="sample-request-body"></a>예제 요청 본문 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 성공 했습니다.|
| 400| 받는 사람 목록이 비어 있거나 최대 길이; 초과 게이머 태그와 XUID 지정 된; 또는 또는 messageText 너무 깁니다.|
| 403| XUID는 변환할 수 없습니다.|
| 404| 게이머 태그 잘못 되었거나 사용자를 찾을 수 없습니다.|
| 409| 사용자는 시스템에서 지정한 일일 한도 도달 했습니다.|
| 500| 일반 서버 쪽 오류입니다.|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>응답 본문

개체가 응답 본문에 전송 됩니다.

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>참조 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)
