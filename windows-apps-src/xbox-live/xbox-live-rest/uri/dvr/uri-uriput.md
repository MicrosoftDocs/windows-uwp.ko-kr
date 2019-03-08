---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633118"
---
# <a name="put-uri"></a>PUT (/{uri})
게임 클립 데이터를 업로드 합니다.
이러한 Uri에 대 한 도메인이 `gameclipsmetadata.xboxlive.com` 고 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.

  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EQB)
  * [쿼리 문자열 매개 변수](#ID4ERC)
  * [필요한 요청 헤더](#ID4EBE)
  * [선택적 요청 헤더](#ID4ENG)
  * [요청 본문](#ID4EWH)
  * [HTTP 상태 코드](#ID4ECAAC)
  * [필요한 응답 헤더](#ID4EYEAC)
  * [선택적 응답 헤더](#ID4ELHAC)
  * [응답 본문](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>설명

후 합니다 **InitialUploadResponse** 반환 되 면 업로드를 통해 수행 됩니다 합니다 **uploadUri** 해당 개체에서 반환 합니다. 클라이언트는 해당 파일을 분할 해야 **expectedBlocks** 각 2MB 이하의 순차 블록입니다. 동시에 업로드할 수 있습니다.

블록에서 파일을 업로드 하는 경우 서버는 각 요청에 대 한 수락 됨 (202) HTTP 상태 코드가 반환 됩니다, 그리고 받기 전에 필요한 모든 블록의 경우는 하나의 파일을 생성 (201)를 반환으로 모든 블록을 커밋합니다. 이러한 경우 응답 개체 없고 서버에서 추가 처리를 예약할 수 있습니다. 오류 시를 **ServiceErrorResponse** 개체는 적절 한 HTTP 상태 코드와 함께 반환 될 수 있습니다.

복구할 수 있는 오류 코드에 대 한 클라이언트는 표준 백오프 재시도 메커니즘을 사용 하 여 재시도 해야 합니다.

> [!NOTE] 
> 업로드를 성공적으로 완료 하는 경우에는 없습니다 이유로 클립 관련이 없는 메타 데이터를 업로드 하는 거부 보완 프로세스 발생 추가로 처리 합니다.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- |
| <b>uri</b>| 문자열| 합니다 <b>uploadUri</b> 내에서 필드를 <b>InitialUploadResponse</b> 개체입니다.|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| 32 비트 부호 없는 정수| 필요한 경우 <b>expectedBlocks</b> 설정 됩니다. 파일에는 블록의 순서를 결정 하는 인덱스가 0 인 블록 수입니다. 예를 들어 경우 <b>expectedBlocks</b> 7 이면 <b>blockNum</b> 0 6 수 있습니다. |
| <b>uploadId</b>| 문자열| 필수. 불투명 ID <b>GameClipsServiceUploadResponse</b> 개체입니다.|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증을 위해 자격 증명을 인증 합니다. 예제 값: <b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.|
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| 문자열| 허용 가능한 압축 인코딩입니다. 예제 값: gzip, deflate, identity입니다.|
| ETag| 문자열| 캐시 최적화를 위해 사용 합니다. 예를 들어 값: "686897696a7c876b7e".|

<a id="ID4EWH"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청의 본문에 전송 됩니다.

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.

| 코드| 이유 구| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 되었습니다.|
| 307| 임시 리디렉션| 서비스는 다른 URI로 이동 되었습니다.|
| 400| 잘못된 요청| 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 요청에 사용자 인증이 필요합니다.|
| 403| 사용할 수 없음| 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.|
| 404| 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용 되지 않음| 리소스 버전이 지원 되지 않습니다.|
| 408| 요청 시간 초과| 요청이 너무 길어서 완료 합니다.|
| 410| 완료| 요청된 된 리소스를 더 이상 사용할 수 없습니다.|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>필요한 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 문자열| 이 요청은 리디렉션되어야 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 등 인증 토큰의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우팅됩니다 됩니다. 예제: 1, vnext 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>application/json</b>합니다.|
| Cache-Control| 문자열| 캐싱 동작을 지정 하는 처리 완료 후 요청입니다.|
| 수락| 문자열| Content-type의 허용 되는 값입니다. 예: <b>application/json</b>합니다.|
| Retry-after| 문자열| 서버를 사용할 수 없는 경우 나중에 다시 시도 하도록 클라이언트를 지시 합니다.|
| 다| 문자열| 다운스트림 프록시 응답을 캐시 하는 방법을 지시 합니다.|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>선택적 응답 헤더

| 헤더| 형식| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 문자열| 캐시 최적화를 위해 사용 합니다. 예제: "686897696a7c876b7e".|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>응답 본문

개체가 응답 본문에 전송 됩니다.

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Parent

[/{uri}](uri-uri.md)
