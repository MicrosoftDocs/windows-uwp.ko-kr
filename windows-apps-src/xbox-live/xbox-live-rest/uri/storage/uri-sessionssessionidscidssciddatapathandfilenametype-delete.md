---
title: DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})
assetID: b4ddc3d2-890d-f677-0109-45d318c3128d
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapathandfilenametype-delete.html
author: KevinAsgari
description: " DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ff955b9076c1d9477605431afe61107600d7236d
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5436120"
---
# <a name="delete-sessionssessionidscidssciddatapathandfilenametype"></a>DELETE (/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type})
파일을 삭제 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4EEB)
  * [필요한 요청 헤더](#ID4ERB)
  * [선택적 요청 헤더](#ID4E1C)
  * [요청 본문](#ID4EWD)
  * [HTTP 상태 코드](#ID4EDE)
  * [응답 본문](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| sessionId| string| 조회 세션의 ID입니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 사용할 수 있는 문자를 포함 하 여 최종 슬래시 경로 부분에 대 한 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_)를 포함 하 고 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 다음의 모든) 파일 이름 부분에 포함 되어야 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 비워 둘 수, 마침표 없거나 두 개의 연속 되는 마침표를 포함 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 파일 또는 json 합니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자는이 리소스에 액세스할 수 없습니다 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4ERB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]. [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다. STS 토큰을 검색 하 고 권한 부여 헤더를 만들기에 대 한 자세한 내용은 Authenticating 요청과 권한 부여 Xbox LIVE 서비스를 참조 하세요.| 
  
<a id="ID4E1C"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 작업을 완료 하려면 기존 항목 일치 하는 ETag를 지정 합니다.| 
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스에 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 파일이 없거나 성공적으로 삭제 했습니다.| 
| 201| 생성 | 엔터티를 만들었습니다.| 
| 400| 잘못 된 요청 | 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 필요한 사용자 인증을 요청 합니다.| 
| 403| 사용할 수 없음 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없습니다. | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음 | 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 제한 | 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 500| 내부 서버 오류 | 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 합니다.| 
| 503| 사용할 수 없는 서비스 | 요청을 제한, 클라이언트 재시도 값 초 (예: 5 초) 한 후 다시 시도 합니다.| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
개체가 응답 본문에 전송 됩니다.
  
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>부모  

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

   