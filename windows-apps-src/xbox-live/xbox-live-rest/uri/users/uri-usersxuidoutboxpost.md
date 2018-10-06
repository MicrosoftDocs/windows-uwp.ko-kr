---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
author: KevinAsgari
description: " POST (/users/xuid({xuid})/outbox)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 260d55104a2083270b1f5c2d2892826cc7b3d6ed
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390396"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
받는 사람 목록에 지정 된 메시지를 보냅니다.
이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EAB)
  * [권한 부여](#ID4ENB)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4EYB)
  * [요청 본문](#ID4E3F)
  * [HTTP 상태 코드](#ID4ETCAC)
  * [응답 본문](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

이 API는 지원만 콘텐츠 형식은 "application/json", 각 호출의 HTTP 헤더에 필요한 합니다.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid | 64 비트의 부호 없는 정수 | Xbox 사용자 ID (XUID) 요청 하 고 있는 플레이어의 합니다. |

<a id="ID4ENB"></a>


## <a name="authorization"></a>권한 부여

사용자가 메시지를 보내는 고유한 사용자 클레임 및 유효한 골드 구독이 있어야 합니다.

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

성공적으로, 플레이어가 친구를 인지를 플레이어에 게 사용자 메시지를 보내는 결과 200 결과 코드. 그러나 차단한 사람에 게 메시지를 보내는 수신자는 메시지를 받지 하며 메시지 메시지 되지 않았지만 하는 표시 되지 않습니다.

에 얼마나 많은 메시지를 받을 하루 및 비 친구와 얼마나 많은 친구에 게 다음과 같이 제한도 있습니다.

   * 메시지 당 20 낯선 사람
   * 24 시간 마다 200 낯선 사람
   * 24 시간 마다 250 총 메시지
   * 24 시간 마다 2500 전체 받는 사람

| 사용자를 요청합니다.| 대상 사용자의 개인 정보 설정| 동작|
| --- | --- | --- | --- | --- | --- |
| 옵션인 추가 정보| -| 설명한 합니다.|
| 친구| 모든 사용자| 200 OK|
| 친구| 친구만| 200 OK|
| 친구| 차단| 200 OK|
| 비 친구 사용자| 모든 사용자| 200 OK|
| 비 친구 사용자| 친구만| 200 OK|
| 비 친구 사용자| 차단| 200 OK|
| 제 3 자 사이트| 모든 사용자| 200 OK|
| 제 3 자 사이트| 친구만| 200 OK|
| 제 3 자 사이트| 차단| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>요청 본문

| 속성| 형식| 최대 길이| 소비자| 설명|
| --- | --- | --- | --- | --- |
| 머리글| 헤더|  | 모두| 사용자가 메시지 헤더|
| messageText| string| 250| Windows 8을 제외 하 고 모든 플랫폼| 사용자가 메시지 텍스트 (u t F-8)|

#### <a name="header"></a>헤더

| 속성| 형식| 최대 길이| 소비자| 설명|
| --- | --- | --- | --- | --- |
| 받는 사람| 사용자]| 20| 모두| 메시지 받는 사람 목록|

#### <a name="user"></a>사용자

| 속성| 형식| 최대 길이| 소비자| 설명|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | 모두| 수신자의 XUID 합니다. 게이머는 발생 하는 경우 사용 되지 않습니다.|
| 게이머 태그| string| 15| 모두| 받는 사람의 게이머 합니다. XUID 전송 되는 경우 사용 되지 않습니다.|

#### <a name="sample-request-body"></a>샘플 요청 본문 

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

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| 코드| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 성공 합니다.|
| 400| 받는 사람 목록이 비어 있거나; 최대 길이 초과 또는 게이머 태그와 XUID 지정 했습니다. 또는 messageText 너무 깁니다.|
| 403| XUID 변환할 수 없습니다.|
| 404| 게이머 잘못 되었거나 사용자를 찾을 수 없습니다.|
| 409| 사용자가 시스템에 의해 적용 된 일일 제한에 도달 했습니다.|
| 500| 일반 서버 쪽 오류가 발생 했습니다.|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>응답 본문

개체가 응답 본문에 전송 됩니다.

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>부모  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>[표준 HTTP 상태 코드](../../additional/httpstatuscodes.md) 참조
