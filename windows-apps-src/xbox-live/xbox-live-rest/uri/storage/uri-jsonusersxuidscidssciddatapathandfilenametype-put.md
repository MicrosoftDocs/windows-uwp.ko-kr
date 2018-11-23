---
title: PUT (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
assetID: 02e43120-1f71-a3e7-c84e-96147b838b97
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype-put.html
author: KevinAsgari
description: " PUT (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cdadf2533ee12c92f2b486408b9305d2919acb66
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7570105"
---
# <a name="put-jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>PUT (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
파일을 업로드합니다. 다중 블록 업로드 json 종류의 데이터에 대 한 지원 되지 않습니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4EEB)
  * [선택적 쿼리 문자열 매개 변수](#ID4ERB)
  * [필요한 요청 헤더](#ID4EXC)
  * [선택적 요청 헤더](#ID4EAE)
  * [요청 본문](#ID4EDF)
  * [HTTP 상태 코드](#ID4EOF)
  * [응답 본문](#ID4EBDAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID) 플레이어의 요청 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 경로 부분 (최대 및 최종 슬래시 포함)에 대 한 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_)를 포함 하며 슬래시 (/) 합니다. 경로 부분 비어 있을 수 있습니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 후 모든) 파일 이름 부분 포함 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 없는 비워 둘 수, 마침표 또는 두 개의 연속 된 마침표 합니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자에 게가이 리소스에 액세스 하는 허용 되지 않으면, 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>선택적 쿼리 문자열 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| 마지막으로 날짜/시간 모든 클라이언트에 있는 파일의 파일을 업로드 합니다.| 
| displayName| string| 사용자에 게 표시 해야 하는 파일 이름입니다.| 
  
<a id="ID4EXC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]. [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다. 권한 부여 헤더를 만들고 STS 토큰을 검색 하는 방법에 대 한 자세한 내용은 Authenticating 요청과 권한 부여 Xbox LIVE 서비스를 참조 하세요.| 
  
<a id="ID4EAE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 작업을 완료 하려면 기존 항목 일치 해야 하는 ETag를 지정 합니다.| 
| If-None-Match| 작업을 완료 하려면 모든 기존 항목을 일치 해야 하는 ETag를 지정 합니다.| 
  
<a id="ID4EDF"></a>

 
## <a name="request-body"></a>요청 본문 
 
요청 본문에 업로드 되는 파일의 전체 내용을 포함 되어 있습니다. 
  
<a id="ID4EOF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 성공 했습니다.| 
| 201| 생성 | 엔터티를 만들었습니다.| 
| 400| 잘못 된 요청 | 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다. | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음 | 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 제한 | 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 500| 내부 서버 오류 | 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 했습니다.| 
| 503| 사용할 수 없는 서비스 | 요청을 제한, 클라이언트 재시도 값 (예: 5 초)을 초에서 후 다시 시도 합니다.| 
  
<a id="ID4EBDAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
업로드 되는 201 {} 응답으로 반환 됩니다.
  
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>부모  

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

   