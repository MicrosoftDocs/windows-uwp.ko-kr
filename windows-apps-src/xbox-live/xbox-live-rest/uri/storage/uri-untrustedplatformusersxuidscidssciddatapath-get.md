---
title: GET (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path})
assetID: 7f1e511b-7c99-d5e9-7829-7777fd56fefc
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapath-get.html
description: " GET (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6905a91ae11f0d074296e3c1959691486fdcbe14
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599918"
---
# <a name="get-untrustedplatformusersxuidxuidscidssciddatapath"></a>GET (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path})
지정된 된 경로에서 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EX)
  * [선택적 쿼리 문자열 매개 변수](#ID4ECB)
  * [권한 부여](#ID4EWC)
  * [필요한 요청 헤더](#ID4EDD)
  * [요청 본문](#ID4EME)
  * [HTTP 상태 코드](#ID4EZE)
  * [응답 본문](#ID4EMCAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| xuid| 부호 없는 64 비트 정수| Xbox 사용자 ID (XUID) 플레이어의 요청을 수행 하는 합니다.| 
| scid| guid| 조회 서비스 구성 파일의 ID입니다.| 
| 경로| 문자열| 반환할 데이터 항목에 대 한 경로입니다. 일치 하는 모든 디렉터리와 하위 디렉터리를 반환 합니다. 유효한 문자는 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함 합니다. 비어 있을 수 있습니다. 최대 길이는 256 자입니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>선택적 쿼리 문자열 매개 변수 
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| int| 예를 들어, 컬렉션에서 N + 1부터 시작 하는 항목 건너뛰기 n 개의 항목을 반환 합니다.| 
| continuationToken| 문자열| 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. 연속 토큰 매개 변수가 둘 다 제공 하는 경우 skipItems 보다 우선 합니다. 즉, 연속 토큰 매개 변수가 있으면 skipItems 매개 변수가 무시 됩니다.| 
| maxItems| int| 항목의 범위를 반환 하는 continuationToken skipItems와 결합할 수 있는 컬렉션에서 반환할 항목의 최대 수입니다. 서비스를 제공할 수 있습니다 기본값 maxItems 없을 maxItems, 보다 더 적은 수를 반환할 수 있습니다 하는 경우 결과의 마지막 페이지 아직 반환 되지 않은 경우에. | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청을 올바른 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자는이 리소스에 액세스할 수 없습니다, 서비스 403 사용 권한 없음 응답을 반환 됩니다. 헤더가 잘못 되었거나 누락 된 경우 서비스는 401 권한 없음된 응답을 반환 합니다. 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x=[hash];[token]| STS 인증 토큰입니다. STSTokenString 인증 요청에 의해 반환 된 토큰으로 대체 됩니다. STS 토큰을 검색 하 고 인증 헤더 만들기에 대 한 추가 정보에 대 한 인증 및 권한 부여 Xbox LIVE 요청에 서비스를 참조 하세요.| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청에 성공 합니다.| 
| 201| 만든 날짜 | 엔터티를 만들었습니다.| 
| 400| 잘못된 요청 | 서비스 잘못 된 요청을 이해할 수 없었습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 요청에 사용자 인증이 필요합니다.| 
| 403| 사용할 수 없음 | 사용자 또는 서비스에 대 한 요청이 허용 되지 않습니다.| 
| 404| 없음 | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용 되지 않음 | 리소스 버전이 지원 되지 않습니다.| 
| 408| 요청 시간 초과 | 요청이 너무 길어서 완료 합니다.| 
| 500| 내부 서버 오류 | 서버에는 요청을 처리 하지 못하게 하는 예기치 않은 상황이 발생 합니다.| 
| 503| 서비스 이용 불가 | 요청이 제한 되었습니다, 그리고 요청 클라이언트 다시 시도 값 (초) (예: 5 초) 후에 다시 시도 하십시오.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="response-body"></a>응답 본문
 
서비스는 배열을 반환 호출이 성공 하면 [TitleBlob](../../json/json-titleblob.md) 개체입니다. 
 
<a id="ID4E1CAC"></a>

 
### <a name="sample-response"></a>샘플 응답
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EGDAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EIDAC"></a>

 
##### <a name="parent"></a>Parent  

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-untrustedplatformusersxuidscidssciddatapath.md)

  
<a id="ID4EUDAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>참조 [TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   