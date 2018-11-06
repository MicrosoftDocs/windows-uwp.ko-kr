---
title: GET (/global/scids/{scid}/data/{pathAndFileName},{type})
assetID: 5ca8e0dd-3c45-1b7b-022e-d5d61414fd7d
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapathandfilenametype-get.html
author: KevinAsgari
description: " GET (/global/scids/{scid}/data/{pathAndFileName},{type})"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 692a68644022dba21de49ed25c4aa69281389e10
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6050682"
---
# <a name="get-globalscidssciddatapathandfilenametype"></a>GET (/global/scids/{scid}/data/{pathAndFileName},{type})
파일을 다운로드 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4ECB)
  * [선택적 쿼리 문자열 매개 변수](#ID4EPB)
  * [필요한 요청 헤더](#ID4EZC)
  * [선택적 요청 헤더](#ID4ECE)
  * [요청 본문](#ID4EMF)
  * [HTTP 상태 코드](#ID4EZF)
  * [응답 헤더](#ID4EMDAC)
  * [응답 본문](#ID4EPEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 경로 부분 (최대 및 최종 슬래시 포함)에 대 한 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_)를 포함 하며 슬래시 (/) 합니다. 경로 부분 비어 있을 수 있습니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 후 모든) 파일 이름 부분 포함 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 없는 비워 둘 수, 마침표 또는 두 개의 연속 된 마침표 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은: 이진, 구성 또는 json 합니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자에 게가이 리소스에 액세스 하는 허용 되지 않으면, 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>선택적 쿼리 문자열 매개 변수 
 
Blob 형식에 따라 다릅니다. 이진 blob 쿼리 매개 변수를 지원 하지 않습니다.
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
|  목록에서 | string| 형식이 json 때에 사용할 수 있습니다. 응답이 매개이 변수를 따른 특정 속성/값은 JSON 포함만 해야 지정 합니다. "점" (.)를 사용 하 여 하위 속성 및 대괄호 지정 ('[' 및 ']') 배열 인덱스를 지정할 수 있습니다. 예를 들어 "array1 [4].prop2" "array1" 배열의 인덱스 4의 "prop2" 속성을 지정 합니다.| 
| customSelector| string| 형식 파일을 구성 합니다. 사용자 지정 가상 노드가 포함 하도록를 나타냅니다. 타이틀 저장소 구성 형식 파일에 대 한 자세한 내용은 참조 하세요.| 
  
<a id="ID4EZC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]. [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다. 권한 부여 헤더를 만들고 STS 토큰을 검색 하는 방법에 대 한 자세한 내용은 Authenticating 요청과 권한 부여 Xbox LIVE 서비스를 참조 하세요.| 
  
<a id="ID4ECE"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 작업을 완료 하려면 기존 항목 일치 해야 하는 ETag를 지정 합니다.| 
| If-None-Match| 작업을 완료 하려면 모든 기존 항목을 일치 해야 하는 ETag를 지정 합니다.| 
| 범위| 다운로드 바이트 범위를 지정 합니다. 표준 HTTP 범위 헤더 형식을 따릅니다.| 
  
<a id="ID4EMF"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EZF"></a>

 
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
  
<a id="ID4EMDAC"></a>

 
## <a name="response-headers"></a>응답 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 불투명 웹 서버는 URL에서 찾은 리소스의 특정 버전에 의해 할당 된 식별자입니다. 해당 URL에 리소스 콘텐츠가 계속 해 서 변경 다양 하며 ETag 할당 됩니다.| 
| 콘텐츠 범위| 부분 다운로드 되었으면이 헤더 다운로드 된 바이트의 범위를 지정 합니다.| 
  
<a id="ID4EPEAC"></a>

 
## <a name="response-body"></a>응답 본문
호출 되 면 서비스는 파일의 내용을 반환 합니다.  
<a id="ID4EYEAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4E1EAC"></a>

 
##### <a name="parent"></a>부모  

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

  
<a id="ID4EGFAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>참조 [TitleBlob (JSON)](../../json/json-titleblob.md)

   