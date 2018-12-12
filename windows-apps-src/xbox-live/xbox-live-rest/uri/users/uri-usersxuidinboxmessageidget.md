---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8945641"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
서비스에서 읽은 상태로 표시 하는 특정 사용자 메시지에 대 한 자세한 메시지 텍스트를 검색 합니다.
이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [권한 부여](#ID4ERB)
  * [요청 본문](#ID4E3B)
  * [리소스의 개인 정보 설정의 효과](#ID4EJC)
  * [HTTP 상태 코드](#ID4EUC)
  * [JavaScript Object Notation (JSON) 응답](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

가져오기 작업은 사용자, 시스템 및 FriendRequest 메시지 형식에만 수행할 수 있습니다.

이 URI는 새로 고침 Xbox.com에 필요합니다. 현재 Xbox 360 사용자가 로그 아웃 될 때까지 및에서 다시 읽기/읽지 않은 상태를 업데이트 되지 않습니다.

이 API는 지원만 콘텐츠 형식은 "application/json", 각 호출의 HTTP 헤더에 필요한 합니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid | 64 비트의 부호 없는 정수 | Xbox 사용자 ID (XUID) 요청 하 고 있는 플레이어의 합니다. |
| messageId | string [50] | 검색 되거나 삭제 되는 메시지의 ID입니다. |

<a id="ID4ERB"></a>


## <a name="authorization"></a>권한 부여

사용자가 메시지를 검색 하 고 고유한 사용자가 있어야 합니다.

<a id="ID4E3B"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스의 개인 정보 설정의 효과

만 고유한 사용자 메시지를 검색할 수 있습니다.

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| 코드| 설명|
| --- | --- | --- | --- | --- |
| 200| 성공 합니다.|
| 400| XUID 제대로 변환할 수 없습니다.|
| 403| XUID는 변환할 수 없으므로 또는 유효한 XUID 클레임을 찾을 수 없습니다.|
| 404| 유효한 XUID 없거나 또는 메시지 ID를 찾을 수 없는 또는 올바르게 구문 분석 합니다.|
| 500| 일반 서버 쪽 오류 또는 메시지 형식에 대 한 GET 올바르지 않습니다.|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON) 응답

성공적으로 호출 하는 경우 서비스는 JSON 형식 결과 데이터를 반환 합니다. 루트 개체는 UserMessageHeader 개체입니다.

#### <a name="usermessageheader"></a>UserMessageHeader

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| 머리글| 헤더|  | JSON 개체|
| messageText| string| 256| U T F-8|

#### <a name="header"></a>헤더

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| 전송| DateTime|  | 날짜 및 메시지를 보낸 시간입니다. (서비스에서 제공).|
| 만료| DateTime|  | 날짜 및 시간이 만료 되는 메시지입니다. (모든 메시지는 최대 수명, 나중에 따라 결정 됩니다.)|
| 메시지 종류| string| 13| 메시지 유형: 사용자, 시스템 FriendRequest 합니다.|
| senderXuid| ulong|  | 발신자의 XUID 합니다.|
| 보낸 사람| string| 15| 발신자의 게이머 태그입니다.|
| hasAudio| 부울|  | 메시지에 있는지 여부 (음성) 오디오 첨부 합니다.|
| hasPhoto| 부울|  | 메시지에 사진 첨부 파일이 있는지 여부.|
| hasText| 부울|  | 메시지 텍스트를 포함 하는지 여부입니다.|

#### <a name="sample-response"></a>예제 응답

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>오류 응답

오류가 발생 하는 경우 서비스는 서비스의 환경에서 값을 포함할 수 있는 errorResponse 개체를 반환할 수 있습니다.

| 속성| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| 지침에 오류가 발생 합니다.|
| 오류 코드| int| (Null 일 수) 오류와 관련 된 숫자 코드입니다.|
| errorMessage| string| 오류 세부 정보를 표시 하도록 구성 된 경우 자세히 설명 합니다.|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>부모  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>[표준 HTTP 상태 코드](../../additional/httpstatuscodes.md) 참조
