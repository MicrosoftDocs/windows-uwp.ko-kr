---
title: POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
assetID: fb4cff17-2721-89c5-6646-5ab76952b411
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cfe20136ad1320966fc851cf9dd6de765871c957
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623298"
---
# <a name="post-jsonusersbatchscidssciddatapathandfilenamejson"></a>POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
파일 이름이 같은 여러 사용자가에서 여러 파일을 다운로드 합니다. 파일을 다운로드할 수는 요청 URI에 의해 결정 됩니다. 요청 본문 XUIDs 사용자 목록을 다운로드 하려면 해당 파일을 포함 합니다. 응답의 본문에는 고유한 집합이 헤더를 사용 하 여 특정 사용자에 대 한 파일을 나타내는 각 부분을 사용 하 여 다중 파트 MIME 메시지를 됩니다. 성공 및 실패의 조합 수에 대 한 응답 부분에 대 한 것 같습니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`합니다.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4ECB)
  * [요청 본문](#ID4EPB)
  * [HTTP 상태 코드](#ID4E3C)
  * [필요한 응답 헤더](#ID4EPAAC)
  * [선택적 응답 헤더](#ID4ESBAC)
  * [응답 본문](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 형식| 설명| 
| --- | --- | --- | 
| scid| guid| 조회 서비스 구성 파일의 ID입니다.| 
| pathAndFileName| 문자열| 액세스 항목에 대 한 경로 및 파일 이름입니다. 유효한 문자 (최대 및 마지막 슬래시 포함) 경로 부분 포함 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_), 슬래시 (/)를 전달 합니다. 경로 부분이 비어 있을 수 있습니다. 유효한 문자 (마지막 슬래시 다음의 모든) 파일 이름 부분을 포함 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름을 하지 비워둘 수, 마침표로 끝날 수도 연속 마침표 두 개가 포함 합니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청을 올바른 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자는이 리소스에 액세스할 수 없습니다, 서비스 403 사용 권한 없음 응답을 반환 됩니다. 헤더가 잘못 되었거나 누락 된 경우 서비스는 401 권한 없음된 응답을 반환 합니다. 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>요청 본문
 
| 속성| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| xuids| 부호 없는 64 비트 정수 배열| 목록 파일을 다운로드 하려는 XUIDs입니다.| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>샘플 요청
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
## <a name="http-status-codes"></a>HTTP 상태 코드 
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답의이 섹션에는 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스를 사용 하는 표준 HTTP 상태 코드의 전체 목록은 참조 하세요 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)합니다.
 
| 코드| 이유 구| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>필요한 응답 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-disposition| 파트의 콘텐츠를 설명합니다. 헤더의 "name" 및 "filename" 부분에이 파일에 속하는 사용자의 XUID 됩니다.| 
| HttpStatusCode| HTTP 상태 코드와 관련 된이 특정 파일을 검색 합니다.| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag에는 URL에 있는 리소스의 특정 버전에는 웹 서버에서 할당 불투명 식별자입니다. 해당 URL에서 리소스 내용을 변경 하는 경우 다른 ETag은 할당 됩니다.| 
| Content-Type| 파일을 성공적으로 검색 하는 경우 파일의 콘텐츠 형식입니다.| 
| Content-range| 파일을 성공적으로 검색 및 부분 다운로드 하는 경우 응답에 포함 된 파일의 바이트 범위입니다. | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>응답 본문
 
호출에 성공한 경우 서비스는 요청된 된 파일의 내용을 다중 파트에 대 한 응답을 반환 합니다.
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>샘플 응답 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>참고 항목
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>Parent 

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

   