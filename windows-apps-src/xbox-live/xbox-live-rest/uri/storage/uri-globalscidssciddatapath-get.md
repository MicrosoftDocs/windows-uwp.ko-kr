---
title: GET (/global/scids/{scid}/data/{path})
assetID: 838abdf2-6fe3-cd7a-4356-6fb0b25a505b
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapath-get.html
author: KevinAsgari
description: " GET (/global/scids/{scid}/data/{path})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93414fcb069a79a1fd98d3f5313a7a4784bb97d4
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5171897"
---
# <a name="get-globalscidssciddatapath"></a>GET (/global/scids/{scid}/data/{path})
지정된 된 경로에 파일 정보를 나열합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [선택적 쿼리 문자열 매개 변수](#ID4ECB)
  * [권한 부여](#ID4EWC)
  * [필요한 요청 헤더](#ID4EDD)
  * [요청 본문](#ID4EME)
  * [HTTP 상태 코드](#ID4EZE)
  * [응답 본문](#ID4EMCAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| path| string| 반환할 데이터 항목의 경로입니다. 일치 하는 모든 디렉터리와 하위 가져오기 반환 됩니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/)를 포함 합니다. 비어 있을 수 있습니다. 256의 최대 길이입니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>선택적 쿼리 문자열 매개 변수 
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| int| 예를 들어 컬렉션에 N + 1에서 시작 하는 항목을 건너뛰고 N 항목을 반환 합니다.| 
| continuationToken| 문자열| 지정 된 연속 토큰에서 시작 하는 항목을 반환 합니다. ContinuationToken 매개 변수 모두 제공 되는 경우 skipItems 보다 우선 합니다. 즉, continuationToken 매개 변수가 있으면 skipItems 매개 변수는 무시 됩니다.| 
| maxItems| int| 최대 skipItems continuationToken 항목의 범위를 반환할 수와 결합할 수 있는 컬렉션에서 반환할 항목 수입니다. 서비스 수 기본값을 제공 maxItems 있는 이며 maxItems, 보다 적은 반환 될 수 결과의 마지막 페이지 아직 반환 되지 않은 경우에 합니다. | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 이 리소스에 액세스할 수는 호출자가 허용 되지 않으면, 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더에 잘못 되었거나 누락 된 경우, 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>필요한 요청 헤더
 
| 헤더| 값| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x xbl-계약 버전| 1| API 계약 버전입니다.| 
| 권한 부여| XBL3.0 x = [해시]; [토큰]| STS 인증 토큰입니다. STSTokenString 인증 요청으로 반환 하는 토큰으로 바뀝니다. 권한 부여 헤더를 만들고 STS 토큰을 검색 하는 방법에 대 한 자세한 내용은 Authenticating 요청과 권한 부여 Xbox LIVE 서비스를 참조 하세요.| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>요청 본문 
 
개체가이 요청의 본문에 전송 됩니다.
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스 사용 되는 표준 HTTP 상태 코드의 전체 목록을, [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EMCAC"></a>

 
## <a name="response-body"></a>응답 본문
 
호출 되 면 서비스는 [TitleBlob](../../json/json-titleblob.md) 개체의 배열을 반환 합니다. 
 
<a id="ID4E1CAC"></a>

 
### <a name="sample-response"></a>예제 응답
 

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

 
##### <a name="parent"></a>부모  

[/global/scids/{scid}/data/{path}](uri-globalscidssciddatapath.md)

  
<a id="ID4EUDAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>참조 [TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo(JSON)](../../json/json-paginginfo.md)

   