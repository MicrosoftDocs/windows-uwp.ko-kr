---
title: PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: b40a1a88-02c2-624f-de00-49664825bde3
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-put.html
author: KevinAsgari
description: " PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: afffb705758c70fd65e01f0ff211254a3f2638d2
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4750733"
---
# <a name="put-untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
파일을 업로드합니다. 데이터는 전체 업로드는 메타 데이터와 데이터 전송 되는 메타 데이터와 데이터 전송 되는 일련의 작은 블록에서 다중 블록 업로드 또는 단일 메시지를 업로드할 수 있습니다. 4 개의 메가바이트 보다 작은 파일만 단일 메시지로 보낼 수 있습니다. 다중 블록 업로드 형식 json의 데이터에 대 한 지원 되지 않습니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4EEB)
  * [선택적 쿼리 문자열 매개 변수](#ID4ERB)
  * [필요한 요청 헤더](#ID4EOE)
  * [선택적 요청 헤더](#ID4EXF)
  * [요청 본문](#ID4E1G)
  * [HTTP 상태 코드](#ID4EFH)
  * [응답 본문](#ID4EYEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID) 플레이어의 요청 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 경로 부분 (및까지 최종 슬래시)에 대 한 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_)를 포함 하며 슬래시 (/) 합니다. 경로 부분 비어 있을 수 있습니다. 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), (최종 슬래시 후 모든 것)는 파일 이름 부분에 포함 되어야 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 비워 둘 수, 마침표 없거나 두 개의 연속 된 마침표를 포함 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 파일 또는 json 합니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 이 리소스에 액세스할 수는 호출자가 허용 되지 않으면, 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더에 잘못 되었거나 누락 된 경우, 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>선택적 쿼리 문자열 매개 변수 
단일 메시지 업로드에 대 한 쿼리 문자열 매개 변수는 다음과 같습니다. 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| 마지막으로 날짜/시간 어떤 클라이언트에서 파일의 파일을 업로드 합니다.| 
| displayName| string| 사용자에 게 표시 해야 하는 파일의 이름입니다.| 
 
다중 블록 업로드에 대 한 쿼리 문자열 매개 변수는 다음과 같습니다.
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| 마지막으로 날짜/시간 어떤 클라이언트에서 파일의 파일을 업로드 합니다.| 
| displayName| string| 사용자에 게 표시 해야 하는 파일의 이름입니다.| 
| continuationToken| 문자열| 이전 업로드 요청 응답의 연속 토큰입니다. 첫 번째 블록 이면이 해야 지정할 수 없습니다. | 
| finalBlock| 부울| 파일의 마지막 블록에 대해 true로 설정 합니다. 다른 모든 블록에 대 한 false로 설정 합니다.| 
  
<a id="ID4EOE"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x xbl-계약 버전| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]; [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다. 권한 부여 헤더를 만들고 STS 토큰을 검색 하는 방법에 대 한 자세한 내용은 Authenticating 요청과 권한 부여 Xbox LIVE 서비스를 참조 하세요.| 
  
<a id="ID4EXF"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 작업을 완료 하는 기존 항목과 일치 해야 하는 ETag를 지정 합니다.| 
| If-None-Match| 작업을 완료 하려면 모든 기존 항목에 맞지 해야 ETag를 지정 합니다.| 
  
<a id="ID4E1G"></a>

 
## <a name="request-body"></a>요청 본문 
 
요청 본문에 업로드 되는 파일의 내용을 포함 되어 있습니다. 단일 메시지 업로드가 본문 파일의 전체 내용을입니다. 다중 블록 업로드가 본문 쿼리 문자열 매개 변수에서 지정 된 파일의 부분입니다. 
  
<a id="ID4EFH"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 성공 했습니다.| 
| 201| 생성 | 엔터티를 만들었습니다.| 
| 400| 잘못 된 요청 | 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 요청은 사용자 인증이 필요합니다.| 
| 403| 금지 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음 | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음 | 리소스 버전은 지원 되지 않습니다.| 
| 408| 요청 시간 제한 | 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 500| 내부 서버 오류 | 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 했습니다.| 
| 503| 사용할 수 없는 서비스 | 요청을 제한, 초 (예: 5 초) 클라이언트를 다시 시도 값 후 다시 시도 합니다.| 
  
<a id="ID4EYEAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
호출 다중 블록 요청을 성공적으로 수행 하는 경우 서비스는 다음 블록을 사용 하 여 전달할 continution 토큰을 반환 합니다.
 
<a id="ID4EEFAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

```cpp
{
    "continuationToken":"abcd1234-1111-2222-3333-abcd12345678-1"
}
         
```

   
<a id="ID4EQFAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4ESFAC"></a>

 
##### <a name="parent"></a>부모  

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

   