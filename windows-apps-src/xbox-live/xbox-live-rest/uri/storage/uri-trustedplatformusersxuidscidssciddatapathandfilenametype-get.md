---
title: GET (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: 203345eb-8092-7d38-6eab-cb7d53bda249
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype-get.html
author: KevinAsgari
description: " GET (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25666c76f5eaafe4f53aaf3ff76eba68d4be275d
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4357259"
---
# <a name="get-trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>GET (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
파일을 다운로드 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4ECB)
  * [선택적 쿼리 문자열 매개 변수](#ID4EPB)
  * [필요한 요청 헤더](#ID4EQC)
  * [선택적 요청 헤더](#ID4EZD)
  * [요청 본문](#ID4EDF)
  * [HTTP 상태 코드](#ID4EQF)
  * [응답 헤더](#ID4EDDAC)
  * [응답 본문](#ID4EGEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| xuid| 64 비트의 부호 없는 정수| Xbox 사용자 ID (XUID)의 플레이어를 요청 하 게 합니다.| 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 사용할 수 있는 문자를 포함 하 여 최종 슬래시 경로 부분에 대 한 (A Z) 대문자, 소문자 (a-z), 숫자 (0-9) 밑줄 (_)를 포함 하 고 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 유효한 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 후 모든) 파일 이름 부분 포함 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 비워 둘 수, 마침표 없거나 두 개의 연속 된 기간을 포함 합니다.| 
| 유형| 문자열| 데이터의 형식입니다. 가능한 값은 이진 또는 json 합니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자에 게가이 리소스에 액세스 하는 허용 되지 않으면, 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더 잘못 되었거나 누락 된 경우 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>선택적 쿼리 문자열 매개 변수 
 
Blob 형식에 따라 다릅니다. 이진 blob 쿼리 매개 변수를 지원 하지 않습니다.
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
|  목록에서 | string| 형식이 json 때에 사용할 수 있습니다. 응답이 매개이 변수 결정 된 대로 특정 속성/값은 JSON 포함만 해야 지정 합니다. "점" (.)를 사용 하 여 하위 속성 및 대괄호 지정 ('[' 및 ']') 배열 인덱스를 지정 합니다. 예를 들어 "array1 [4].prop2" "array1" 배열의 인덱스 4의 "prop2" 속성을 지정 합니다.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xbl 계약 버전 x| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]; [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다. STS 토큰을 검색 하 고 권한 부여 헤더를 만들기에 대 한 자세한 내용은 Authenticating 요청과 권한 부여 Xbox LIVE 서비스를 참조 하세요.| 
  
<a id="ID4EZD"></a>

 
## <a name="optional-request-headers"></a>선택적 요청 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 기존 항목 작업을 완료 하려면 일치 하는 ETag를 지정 합니다.| 
| If-None-Match| 작업을 완료 하려면 모든 기존 항목을와 일치 해야 하는 ETag를 지정 합니다.| 
| 범위| 다운로드 바이트 범위를 지정 합니다. 표준 HTTP 범위 헤더 형식을 따릅니다.| 
  
<a id="ID4EDF"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청 본문에 전송 됩니다.
  
<a id="ID4EQF"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 하는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 성공 했습니다.| 
| 201| 생성 | 엔터티를 만들었습니다.| 
| 400| 잘못 된 요청 | 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음 | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음 | 리소스 버전은 지원 되지 않습니다.| 
| 408| 요청 시간 제한 | 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 500| 내부 서버 오류 | 서버에서 요청을 수행할 수 있는 예기치 않은 조건이 발생 했습니다.| 
| 503| 사용할 수 없는 서비스 | 요청을 제한, 클라이언트 재시도 값 (예: 5 초)을 초에서 후 다시 시도 합니다.| 
  
<a id="ID4EDDAC"></a>

 
## <a name="response-headers"></a>응답 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 특정 버전의 URL에 리소스를 웹 서버에 의해 할당 된 불투명 식별자입니다. 해당 URL에 리소스 콘텐츠가 변경, 새로운 및 다른 ETag 할당 됩니다.| 
| 콘텐츠 범위| 이 헤더 부분 다운로드 되었으면 다운로드 된 바이트의 범위를 지정 합니다.| 
  
<a id="ID4EGEAC"></a>

 
## <a name="response-body"></a>응답 본문
호출에 성공 하면 서비스는 파일의 내용을 반환 합니다.  
<a id="ID4EPEAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EREAC"></a>

 
##### <a name="parent"></a>부모  

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

  
<a id="ID4E4EAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>참조 [TitleBlob (JSON)](../../json/json-titleblob.md)

   