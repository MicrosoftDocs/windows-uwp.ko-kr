---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
author: KevinAsgari
description: " PUT (/{uri})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 61eecfbc6d5ebeda4825b8a3d29e90347b9988af
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4507911"
---
# <a name="put-uri"></a>PUT (/{uri})
게임 클립 데이터를 업로드 합니다.
이러한 Uri에 대 한 도메인은 `gameclipsmetadata.xboxlive.com` 및 `gameclipstransfer.xboxlive.com`해당 URI의 기능에 따라 합니다.

  * [설명](#ID4EX)
  * [URI 매개 변수](#ID4EQB)
  * [쿼리 문자열 매개 변수](#ID4ERC)
  * [필요한 요청 헤더](#ID4EBE)
  * [선택적 요청 헤더](#ID4ENG)
  * [요청 본문](#ID4EWH)
  * [HTTP 상태 코드](#ID4ECAAC)
  * [필수 응답 헤더](#ID4EYEAC)
  * [선택적 응답 헤더](#ID4ELHAC)
  * [응답 본문](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>설명

**InitialUploadResponse** 를 반환한 후 업로드 해당 개체에 반환 된 **uploadUri** 를 통해 수행 됩니다. 클라이언트 해야 분할 파일 **expectedBlocks** 순차적 2 MB 보다 더 큰 블록입니다. 동시에 업로드할 수 있습니다.

블록에 파일을 업로드 하는 서버 각 요청에 대 한 승인 됨 (202)의 HTTP 상태 코드를 반환 하는, 수신한 모든 예상 된 블록의 경우는 될 때까지 모든 블록 생성 (201) 반환 하는 하나의 파일로 커밋합니다. 이러한 경우 응답에는 개체 및 서버 추가 처리를 예약할 수 있습니다. 오류 발생 시 **ServiceErrorResponse** 개체는 적절 한 HTTP 상태 코드와 함께 반환 될 수 있습니다.

복구할 수 있는 오류 코드에서 클라이언트는 해당 표준 백오프 재시도 메커니즘을 사용 하 여 다시 시도해 야 합니다.

> [!NOTE] 
> 성공적으로 업로드를 완료 하는 경우에 추가 처리 발생는 수 업로드 또는 메타 데이터에 대 한 상관 이유로 클립 거부 보완 프로세스 합니다.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- |
| <b>uri</b>| string| <b>InitialUploadResponse</b> 개체 내에서 <b>uploadUri</b> 필드입니다.|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>쿼리 문자열 매개 변수

| 매개 변수| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| 32 비트 부호 없는 정수| <b>ExpectedBlocks</b> 설정 된 경우 필요 합니다. 0 인덱스 블록 번호 파일에서 블록의 순서를 결정 합니다. 예를 들어 <b>expectedBlocks</b> 7 인 경우 다음 <b>blockNum</b> 수 0도에서 6. |
| <b>uploadId</b>| string| 필수. 불투명 <b>GameClipsServiceUploadResponse</b> 개체 ID입니다.|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>필요한 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 권한 부여| 문자열| HTTP 인증에 대 한 자격 증명을 인증 합니다. 예제 값: <b>Xauth =&lt;authtoken ></b>|
| X RequestedServiceVersion| string| 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.|
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.|
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>선택적 요청 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| string| Encodings 허용 가능한 압축 합니다. 예제 값: gzip만 줄이기, identity 합니다.|
| ETag| string| 캐시 최적화에 사용 됩니다. 예제 값: "686897696a7c876b7e"입니다.|

<a id="ID4EWH"></a>


## <a name="request-body"></a>요청 본문

개체가이 요청 본문에 전송 됩니다.

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>HTTP 상태 코드

서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 되는 표준 HTTP 상태 코드의 전체 목록을 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.

| Code| 이유 구문| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 확인| 세션을 검색 했습니다.|
| 301| 영구적으로 이동| 서비스는 다른 URI로 이동 합니다.|
| 307| 임시 리디렉션| 서비스는 다른 URI로 이동 합니다.|
| 400| 잘못 된 요청| 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.|
| 401| 권한 없음| 필요한 사용자 인증을 요청 합니다.|
| 403| 금지| 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.|
| 404| 찾을 수 없음| 지정된 된 리소스를 찾을 수 없습니다.|
| 406| 허용할 수 없음| 리소스 버전은 지원 되지 않습니다.|
| 408| 요청 시간 제한| 요청을 완료 하는 데 너무 오래 걸렸습니다.|
| 410| 최신 상태가 아닌| 요청 된 리소스는 더 이상 사용할 수 없습니다.|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>필수 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion| string| 이 요청은 전송 Xbox LIVE 서비스의 이름/번호를 빌드하십시오. 요청 헤더, 인증 토큰 등의 클레임의 유효성을 확인 한 후 해당 서비스에만 라우트된 됩니다. 예: 1, vnext 합니다.|
| Content-Type| 문자열| 응답 본문의 MIME 형식입니다. 예: <b>응용 프로그램/j</b>합니다.|
| 캐시 제어| string| 정중 요청 캐싱 동작을 지정 합니다.|
| 수락| string| 콘텐츠 형식의 허용 되는 값입니다. 예: <b>응용 프로그램/j</b>합니다.|
| 다시 시도 후| string| 사용할 수 없는 서버의 경우 나중에 다시 시도 하는 클라이언트에 지시 합니다.|
| 다| string| 응답을 캐시 하는 방법을 다운스트림 프록시에 지시 합니다.|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>선택적 응답 헤더

| 헤더| 유형| 설명|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| string| 캐시 최적화에 사용 됩니다. 예: "686897696a7c876b7e"입니다.|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>응답 본문

개체가 응답 본문에 전송 됩니다.

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>참고 항목

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>부모

[/{uri}](uri-uri.md)
