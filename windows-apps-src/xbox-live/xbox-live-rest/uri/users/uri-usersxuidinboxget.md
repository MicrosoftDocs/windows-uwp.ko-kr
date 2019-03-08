---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632248"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
서비스에서 지정된 된 수의 사용자 메시지 요약을 검색합니다.
이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`합니다.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EIC)
  * [권한 부여](#ID4EGE)
  * [리소스에 대 한 개인 정보 설정의 효과](#ID4ETE)
  * [HTTP 상태 코드](#ID4E5E)
  * [JavaScript 개체 표기법 (JSON) 응답](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

메시지 대상만 포함 하는 사용자 메시지를 요약 합니다. 사용자에서 생성 된 메시지는 현재 메시지 텍스트의 처음 20 자입니다. 시스템 메시지에는 "라이브 시스템"와 같은 대체는 주체를 제공할 수 있습니다.

메시지를 보낸 순서와 반대로 반환 됩니다. 즉, 최신 메시지는 먼저 반환 됩니다.

이 API는 지원만 content-type은 "application/json"이 각 호출의 HTTP 헤더에 필요 합니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- |
| xuid| 부호 없는 64 비트 정수| Xbox 사용자 ID (XUID)은 요청을 수행 하는 플레이어입니다.|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| int| 100| 반환 되는 메시지의 최대 수입니다.|
| continuationToken| 문자열|  | 이전 열거형; 호출에서 반환 된 문자열 열거를 계속 하는 데 사용 합니다.|
| skipItems| int| 100| 건너뛸; 메시지 수 continuationToken 있으면 무시 됩니다.|

<a id="ID4EGE"></a>


## <a name="authorization"></a>권한 부여

사용자 클레임을 사용자 메시지 요약을 검색 해야 합니다.

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스에 대 한 개인 정보 설정의 효과

만 고유한 사용자 메시지를 열거할 수 있습니다.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 요청에 성공 합니다.|
| 400| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 403| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.|
| 404| 유효한 XUID URI에 없습니다.|
| 409| 기본 컬렉션을 전달 된 연속 토큰에 따라 변경 합니다.|
| 416| 건너뛸 항목 수가 사용 가능한 항목의 수보다 큽니다.|
| 500| 일반 서버 쪽 오류입니다.|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 개체 표기법 (JSON) 응답

성공적으로 호출 하는 경우 서비스는 결과 데이터를 JSON 형식으로 반환 합니다.

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| 결과| 메시지]| 100| 사용자 메시지의 배열|
| pagingInfo| pagingInfo|  | 현재 결과 집합에 대 한 페이징 정보|

#### <a name="message"></a>Message

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| 머리글| 헤더|  | 사용자 메시지 헤더|
| messageSummary| 문자열| 20| U T F-8입니다. 메시지의 일반적으로 처음 20 자|

#### <a name="header"></a>헤더

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| id| 문자열| 50| 메시지 세부 정보를 검색 또는 메시지를 삭제 하는 데 사용 되는 메시지 식별자입니다.|
| isRead| 부울|  | 사용자 이미 읽은 메시지 세부 정보를 나타내는 플래그입니다.|
| 전송| DateTime|  | UTC 날짜 및 시간 메시지를 보냈습니다. (서비스에서 제공).|
| 만료| DateTime|  | UTC 날짜 및 시간에는 메시지 만료 됩니다. (모든 메시지에는 나중에 결정 해야 하는 최대 수명을 가집니다.)|
| messageType| 문자열| 50| 메시지 유형: 사용자, 시스템, FriendRequest, 비디오, QuickChat, VideoChat, PartyChat, 제목, GameInvite입니다.|
| senderXuid| ulong|  | 보낸 사람의 XUID 합니다.|
| 발신자| 문자열| 15| 보낸 사람의 게이머 태그입니다.|
| hasAudio| 부울|  | 메시지 (의견) 오디오 첨부 파일에 있는지 여부.|
| hasPhoto| 부울|  | 메시지 첨부 파일을 사진에 있는지 여부.|
| hasText| 부울|  | 메시지 텍스트 포함 되는지 여부입니다.|

#### <a name="paging-info"></a>페이징 정보

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| continuationToken| 문자열| 100| 필요에 따라 서버에서 반환 합니다. 계속 열거 하려면 나중에 호출을 허용 합니다.|
| totalItems| int|  | 받은 편지함에 메시지의 총 수입니다.|

#### <a name="sample-response"></a>샘플 응답

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>오류 응답

오류가 하는 경우 서비스는 서비스의 환경에서 값을 포함할 수 있는 errorResponse 개체를 반환할 수 있습니다.

| 속성| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| 문자열| 오류가 발생 한 위치를 나타냅니다.|
| errorCode| int| (Null 일 수) 오류와 연결 된 숫자 코드입니다.|
| errorMessage| 문자열| 세부 정보를 표시 하도록 구성 하는 경우 오류 세부 정보입니다.|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>참조 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)
