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
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/06/2018
ms.locfileid: "8737102"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
서비스에서 지정된 된 수의 사용자 메시지 요약을 검색합니다.
이러한 Uri에 대 한 도메인은 `msg.xboxlive.com`.

  * [설명](#ID4EV)
  * [URI 매개 변수](#ID4EEB)
  * [쿼리 문자열 매개 변수](#ID4EIC)
  * [권한 부여](#ID4EGE)
  * [리소스의 개인 정보 설정의 효과](#ID4ETE)
  * [HTTP 상태 코드](#ID4E5E)
  * [JavaScript Object Notation (JSON) 응답](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>설명

사용자가 메시지 요약 메시지 제목을 포함 되어 있습니다. 사용자 생성 된 메시지,이 현재 메시지 텍스트의 처음 20 자입니다. 시스템 메시지 "라이브 시스템" 등의 대체 주체를 제공할 수 있습니다.

메시지; 수신 된 순서의 반대 방향으로 돌아갑니다. 즉, 새 메시지는 먼저 반환 됩니다.

이 API는 지원만 콘텐츠 형식은 "application/json", 각 호출의 HTTP 헤더에 필요한 합니다.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- |
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID) 요청 하 고 있는 플레이어의 합니다.|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| int| 100| 반환 되는 메시지의 최대 수입니다.|
| continuationToken| 문자열|  | 이전 열거형 호출;에 반환 하는 문자열 열거형을 계속 하는 데 사용 합니다.|
| skipItems| int| 100| 다양 한 메시지를 건너뜁니다. continuationToken 있는 경우 무시 됩니다.|

<a id="ID4EGE"></a>


## <a name="authorization"></a>권한 부여

사용자는 사용자가 메시지 요약을 검색 하 고 있어야 합니다.

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>리소스의 개인 정보 설정의 효과

만 고유한 사용자 메시지를 열거할 수 있습니다.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| 코드| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 요청이 성공 했습니다.|
| 400| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 403| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.|
| 404| 유효한 XUID URI에 없습니다.|
| 409| 기본 컬렉션에서 전달 된 연속 토큰에 따라 변경 합니다.|
| 416| 건너뛸 항목 수가 사용 가능한 항목 개수 보다 큽니다.|
| 500| 일반 서버 쪽 오류가 발생 했습니다.|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON) 응답

성공적으로 호출 하는 경우 서비스는 JSON 형식 결과 데이터를 반환 합니다.

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| 결과| 메시지]| 100| 사용자가 메시지의 배열|
| pagingInfo| PagingInfo|  | 현재 결과 집합에 대 한 정보를 페이징|

#### <a name="message"></a>Message

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| 머리글| 헤더|  | 사용자가 메시지 헤더|
| messageSummary| string| 20| U T F-8입니다. 일반적으로 처음 20 문자 메시지의|

#### <a name="header"></a>헤더

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| id| string| 50| 메시지 세부 정보를 검색 하거나 메시지 삭제에 사용 되는 메시지 식별자입니다.|
| isRead| 부울|  | 사용자가 메시지 세부 정보를 읽어 이미가 나타내는 플래그입니다.|
| 전송| DateTime|  | UTC 날짜 및 메시지를 보낸 시간입니다. (서비스에서 제공).|
| 만료| DateTime|  | UTC 날짜 및 시간 메시지에 만료 됩니다. (모든 메시지는 최대 수명, 나중에 따라 결정 됩니다.)|
| 메시지 종류| string| 50| 메시지 유형: 사용자, 시스템, FriendRequest, 비디오, QuickChat, VideoChat, PartyChat, 제목, GameInvite 합니다.|
| senderXuid| ulong|  | 발신자의 XUID 합니다.|
| 보낸 사람| string| 15| 발신자의 게이머 태그입니다.|
| hasAudio| 부울|  | 메시지에 있는지 여부 (음성) 오디오 첨부 합니다.|
| hasPhoto| 부울|  | 메시지에 사진 첨부 파일이 있는지 여부.|
| hasText| 부울|  | 메시지 텍스트를 포함 하는지 여부입니다.|

#### <a name="paging-info"></a>페이징 정보

| 속성| 형식| 최대 길이| 설명|
| --- | --- | --- | --- |
| continuationToken| 문자열| 100| 필요에 따라 서버에 의해 반환 됩니다. 열거형을 계속 하려면 이후 호출을 허용 합니다.|
| totalItems| int|  | 받은 편지함 메시지의 총 수입니다.|

#### <a name="sample-response"></a>예제 응답

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

오류가 발생 하는 경우 서비스는 서비스의 환경에서 값을 포함할 수 있는 errorResponse 개체를 반환할 수 있습니다.

| 속성| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| 지침에 오류가 발생 합니다.|
| 오류 코드| int| (Null 일 수) 오류와 관련 된 숫자 코드입니다.|
| errorMessage| string| 오류 세부 정보를 표시 하도록 구성 된 경우 자세히 설명 합니다.|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>부모  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>[표준 HTTP 상태 코드](../../additional/httpstatuscodes.md) 참조
