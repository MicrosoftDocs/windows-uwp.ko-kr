---
title: POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
assetID: fb4cff17-2721-89c5-6646-5ab76952b411
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype-post.html
author: KevinAsgari
description: " POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 492b2ff62927812337f1b94f487a2649c0446f00
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563831"
---
# <a name="post-jsonusersbatchscidssciddatapathandfilenamejson"></a>POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
여러 사용자가 동일한 파일 이름으로에서 여러 파일을 다운로드합니다. 파일 다운로드 요청 URI에 의해 결정 됩니다. 요청 본문 파일을 다운로드 하려면 사용자의 XUIDs 목록이 포함 되어 있습니다. 응답 본문에는 고유한 헤더를 사용 하 여 특정 사용자에 대 한 파일을 나타내는 각 부분을 사용 하 여 다중 파트 MIME 메시지 됩니다. 성공 및 실패에 대 한 응답의 부분에 대 한 것이 가능 합니다. 이러한 Uri에 대 한 도메인은 `titlestorage.xboxlive.com`.
 
  * [URI 매개 변수](#ID4EX)
  * [권한 부여](#ID4ECB)
  * [요청 본문](#ID4EPB)
  * [HTTP 상태 코드](#ID4E3C)
  * [필수 응답 헤더](#ID4EPAAC)
  * [선택적 응답 헤더](#ID4ESBAC)
  * [응답 본문](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 매개 변수
 
| 매개 변수| 유형| 설명| 
| --- | --- | --- | 
| 서비스 안내| guid| 조회 서비스 구성의 ID입니다.| 
| pathAndFileName| string| 항목에 액세스할 수에 대 한 경로 파일 이름입니다. 경로 부분까지 포함 하 여 최종 슬래시를 사용할 수 있는 문자는 대문자 (A-z), 소문자 (a-z), 숫자 (0-9), 밑줄 (_) 및 슬래시 (/). 경로 부분 비어 있을 수 있습니다. 사용할 수 있는 문자 (A-z) 대문자, 소문자 (a-z), 숫자 (0-9) (최종 슬래시 다음의 모든) 파일 이름 부분을 포함 한 밑줄 (_), 마침표 (.) 및 하이픈 (-). 파일 이름 수 없습니다 비워 둘 수, 마침표 개 연속 합니다.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>권한 부여 
 
요청이 유효한 Xbox LIVE 권한 부여 헤더를 포함 해야 합니다. 호출자는이 리소스에 액세스할 수 없습니다 서비스는 403 사용할 수 없음 응답을 반환 합니다. 헤더 잘못 되거나 없는 경우 서비스는 401 무단된 응답을 반환 합니다. 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>요청 본문
 
| 속성| 형식| 설명| 
| --- | --- | --- | --- | --- | --- | 
| xuids| 64 비트 부호 없는 정수의 배열| 목록 파일을 다운로드 하는 XUIDs입니다.| 
 
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
 
서비스는이 리소스에서이 메서드를 사용 하 여 요청에 대 한 응답으로이 섹션의 상태 코드 중 하나를 반환 합니다. Xbox Live 서비스와 함께 사용 되는 표준 HTTP 상태 코드의 전체 목록을 [표준 HTTP 상태 코드](../../additional/httpstatuscodes.md)를 참조 하세요.
 
| Code| 이유 구문| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 확인 | 요청이 성공 했습니다.| 
| 201| 생성 | 엔터티를 만들었습니다.| 
| 400| 잘못 된 요청 | 서비스 잘못 된 요청을 이해 하지 못했습니다. 일반적으로 잘못 된 매개 변수입니다.| 
| 401| 권한 없음 | 필요한 사용자 인증을 요청 합니다.| 
| 403| 금지 | 사용자 또는 서비스에 대 한 요청을 허용 되지 않습니다.| 
| 404| 찾을 수 없음 | 지정된 된 리소스를 찾을 수 없습니다.| 
| 406| 허용할 수 없음 | 리소스 버전은 지원 되지 않습니다.| 
| 408| 요청 시간 제한 | 요청을 완료 하는 데 너무 오래 걸렸습니다.| 
| 500| 내부 서버 오류 | 서버에서 요청을 수행할 수 있는 예상치 못한 상황이 발생 했습니다.| 
| 503| 사용할 수 없는 서비스 | 요청을 제한, 클라이언트 재시도 값 초 (예: 5 초) 한 후 다시 시도 합니다.| 
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>필수 응답 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 내용 처리| 일부 콘텐츠를 설명합니다. 헤더의 "이름" 및 "filename" 부분이이 파일에 속하는 사용자의 XUID 됩니다.| 
| HttpStatusCode| HTTP 상태 코드와 관련 된이 특정 파일을 검색 합니다.| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>선택적 응답 헤더
 
| 헤더| 설명| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 특정 버전의 URL에 리소스를 웹 서버에서 할당 불투명 식별자입니다. 해당 URL에 리소스 콘텐츠가 변경, 새롭고 다른 ETag 할당 됩니다.| 
| Content-Type| 파일을 성공적으로 검색 하는 경우 파일의 콘텐츠 형식입니다.| 
| 콘텐츠 범위| 파일이 성공적으로 검색 하 고 부분 다운로드를 하는 경우 응답에 포함 된 파일의 바이트 범위입니다. | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>응답 본문
 
호출 되 면 서비스는 요청 된 파일의 내용을 다중 파트 응답에 반환 합니다.
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>예제 응답 
 

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

 
##### <a name="parent"></a>부모 

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

   