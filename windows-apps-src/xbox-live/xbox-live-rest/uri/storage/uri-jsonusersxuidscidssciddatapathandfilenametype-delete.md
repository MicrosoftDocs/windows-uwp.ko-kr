---
title: DELETE (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
assetID: b8d862d1-1651-b6c2-cc25-c5398128e882
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype-delete.html
description: " DELETE (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 40089ce5b719d74aed5274e42ace01e79adad61c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662278"
---
# <a name="delete-jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>DELETE (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
파일을 삭제 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4EEB)
  * [필요한 요청 헤더](#ID4ERB)
  * [선택적 요청 헤더](#ID4E1C)
  * [요청 본문](#ID4EWD)
  * [HTTP 상태 코드](#ID4EDE)
  * [응답 본문](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 부호 없는 64 비트 정수| Xbox 사용자 ID (XUID) 플레이어의 요청을 수행 하는 합니다.| 
| scid| guid| 조회 서비스 구성 파일의 ID입니다.| 
| pathAndFileName| 문자열| 액세스 항목에 대 한 경로 및 파일 이름입니다. 유효한 문자 (최대 및 마지막 슬래시 포함) 경로 부분 포함 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_), 슬래시 (/)를 전달 합니다. 경로 부분이 비어 있을 수 있습니다. 유효한 문자 (마지막 슬래시 다음의 모든) 파일 이름 부분을 포함 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름을 하지 비워둘 수, 마침표로 끝날 수도 연속 마침표 두 개가 포함 합니다.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청을 올바른 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자는이 리소스에 액세스할 수 없습니다, 서비스 403 사용 권한 없음 응답을 반환 됩니다. 헤더가 잘못 되었거나 누락 된 경우 서비스는 401 권한 없음된 응답을 반환 합니다. 
  
<a id="ID4ERB"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x=[hash];[token]| STS 인증 토큰입니다. STSTokenString 인증 요청에 의해 반환 된 토큰으로 대체 됩니다. STS 토큰을 검색 하 고 인증 헤더 만들기에 대 한 추가 정보에 대 한 인증 및 권한 부여 Xbox LIVE 요청에 서비스를 참조 하세요.| 
  
<a id="ID4E1C"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 작업을 완료 하려면 기존 항목과 일치 해야 하는 ETag를 지정 합니다.| 
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 파일이 없거나 성공적으로 삭제 되었습니다.| 
| 201| 만든 날짜 | 엔터티를 만들었습니다.| 
| 400| 잘못된 요청 | 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음 | 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음 | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음 | 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과 | 요청이 너무 길어서 완료 합니다.| 
| 500| 내부 서버 오류 | 서버에는 요청을 처리 하지 못하게 하는 예기치 않은 상황이 발생 합니다.| 
| 503| 서비스 이용 불가 | 요청이 제한 되었습니다, 그리고 요청 클라이언트 다시 시도 값 (초) (예: 5 초) 후에 다시 시도 하십시오.| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>응답 본문 
 
개체가 응답 본문에 전송 됩니다.
  
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent  

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

   